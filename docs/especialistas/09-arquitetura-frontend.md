# Arquitetura Frontend — ghdarulms

> Especialista: arquitetura frontend (React/Next.js, PWA, offline-first, design systems, performance mobile, acessibilidade, streaming de LLM na UI).

## 1. Decisão de plataforma (app do acadêmico)

| Critério | PWA (React) | React Native / Expo | Flutter |
|---|---|---|---|
| Push iOS | Sim, desde iOS 16.4, **somente se instalado na tela inicial** (Web Push via Safari) | Nativo (APNs) | Nativo |
| Offline | Service Worker + IndexedDB, maduro | SQLite/MMKV, maduro | Hive/Isar, maduro |
| Câmera/microfone | `getUserMedia`, `<input capture>`, Web Speech API (limitada no iOS) | Completo | Completo |
| Reuso com apps mentor/admin | 100% (mesmo design system, mesmo bundle de domínio) | Parcial (RN Web é frágil para desktop rico) | Zero com React |
| Custo de time (2-4 devs) | 1 stack, 1 pipeline, sem loja | 2 pipelines (web + nativo), builds EAS, revisão de loja | 2 linguagens (Dart + TS) |
| Distribuição | Link/QR, sem revisão | Lojas | Lojas |

**Recomendação para MVP: PWA em Next.js (App Router) com React 19 + TypeScript.** Motivos: (a) equipe pequena mantém um único código para aluno, mentor e admin; (b) o produto é 90% texto, cards interativos e chat — não exige APIs nativas profundas; (c) push no iOS já funciona via Web Push quando o app é adicionado à tela inicial, e o fluxo "Adicionar à tela inicial" será guiado na primeira sessão; (d) distribuição por link/QR em turmas elimina fricção de loja.

**Caminho de evolução:** quando houver demanda de loja, push iOS sem instalação manual ou gravação de áudio robusta no Safari, empacotar a **mesma PWA com Capacitor** (`@capacitor/push-notifications`, `@capacitor/filesystem`). O código React não muda; apenas um adaptador `packages/platform` abstrai `push`, `storage` e `speech` com implementações `web` e `capacitor`. React Native fica como opção de longo prazo só se o produto passar a depender de vídeo em tempo real ou reconhecimento de fala on-device.

Por que Next.js e não Vite: SSR/streaming para o app do mentor (SEO irrelevante, mas TTFB e hidratação parcial ajudam em dashboards), Route Handlers como BFF fino para cookies httpOnly e proxy de SSE, e `next-pwa`/Serwist integram Workbox sem configuração manual. O app `student` roda com `output: 'export'` + Serwist quando quisermos hospedar em CDN estática; a decisão fica reversível.

## 2. Estrutura do monorepo

```
ghdarulms/
├── apps/
│   ├── student/        # Next.js, PWA mobile-first, manifest + SW
│   ├── mentor/         # Next.js, autoria + dashboard desktop-first
│   └── admin/          # Next.js, tenants, usuários, billing
├── packages/
│   ├── ui/             # design system: Tailwind preset, tokens, shadcn/Radix, Storybook
│   ├── domain/         # tipos puros: BloomLevel, Concept, ConceptGraph, MasteryState, Evidence
│   ├── api-client/     # gerado do OpenAPI (openapi-typescript + openapi-fetch), hooks TanStack Query
│   ├── lesson-engine/  # schema JSON, máquina de estados XState, renderizadores de card
│   ├── tutor-chat/     # componente de chat SSE, markdown incremental, citações
│   ├── offline/        # Workbox runtime, fila IndexedDB (Dexie), sync
│   ├── platform/       # adaptadores web/capacitor: push, speech, storage
│   └── config/         # eslint, tsconfig, tailwind, vitest presets
├── pnpm-workspace.yaml
└── turbo.json          # pipelines: build, lint, test, e2e, storybook
```

Regras: `domain` não importa React nem `api-client`; `lesson-engine` importa `domain` mas não `api-client` (recebe callbacks); apps nunca importam umas às outras. `api-client` é regenerado em CI a partir do `openapi.json` do backend e a build falha se houver drift (`git diff --exit-code`). Versionamento com Changesets apenas para `ui` e `lesson-engine`, que podem ser publicados para uso por terceiros no futuro.

## 3. Stack

| Preocupação | Escolha | Justificativa |
|---|---|---|
| Framework | Next.js 15 App Router, React 19, TS strict | Ver §1 |
| Roteamento | App Router; `student` com rotas `/(learn)/course/[id]/concept/[cid]/lesson/[lid]`; `mentor` com grupos `(authoring)` e `(insights)` | Layouts aninhados, `loading.tsx` por rota, prefetch nativo |
| Estado servidor | TanStack Query v5 + `openapi-fetch` | Cache, invalidação por tag, `persistQueryClient` em IndexedDB para offline |
| Estado local | Zustand (slices: `session`, `lessonRuntime`, `tutor`, `network`) com `persist` seletivo | Pequeno, sem boilerplate, fácil de testar |
| Máquina de estados | XState v5 apenas no `lesson-engine` | Estados da sessão são explícitos e auditáveis |
| Formulários | React Hook Form + Zod; schemas Zod compartilhados em `domain` | Validação isomórfica com o que o backend expõe |
| i18n | `next-intl`, `pt-BR` padrão, mensagens em JSON por app, ICU plural | Rotas sem prefixo de locale no MVP |
| Estilo | Tailwind 4 + Radix Primitives via shadcn/ui, tokens CSS em `ui` | Acessibilidade de teclado/ARIA vem do Radix |
| Ícones | `lucide-react` com import nomeado (tree-shake) | |
| Animações | `motion` (Framer Motion, bundle `motion/react-m` reduzido) só para transições de card; CSS para o resto; `prefers-reduced-motion` respeitado | |
| Gráficos | **Recharts é pesado demais**; usar **SVG próprio** para o anel de Bloom e **`@unovis/react`** (tree-shakeable, ~30 kB para 2 gráficos) para mapa de calor e linhas no dashboard do mentor | |
| Grafo | `reagraph` no desktop (WebGL), lista hierárquica + mini-grafo SVG via `d3-force` no mobile | Ver §7 |
| Markdown | `react-markdown` + `remark-gfm` + `rehype-sanitize` + `rehype-katex` (lazy) | |
| Editor do mentor | Tiptap (ProseMirror) com extensões de callout e bloco de card | Saída em JSON, não HTML |

## 4. Motor de micro-lições (`lesson-engine`)

### 4.1 Modelo declarativo

Uma lição é um JSON versionado: metadados + sequência ordenada de cards. Cada card declara `type`, `bloom`, `conceptId` e um `payload` específico do tipo. O motor não conhece o backend: recebe a lição, emite `Evidence` por card e `SessionSnapshot` para persistência.

Tipos de card por nível de Bloom:

| Bloom | Tipos |
|---|---|
| Memorizar | `explain` (conteúdo), `flashcard`, `multiple_choice` |
| Compreender | `short_answer_ai`, `multiple_choice` (com `requireJustification`), `compare` |
| Aplicar | `order_items`, `categorize`, `short_code` (opcional, Monaco lazy ou textarea com highlight) |
| Analisar | `compare`, `categorize`, `short_answer_ai` com rubrica analítica |
| Avaliar | `judgment` (afirmação → concorda/discorda + justificativa avaliada por IA) |
| Criar | `mini_production` (texto/áudio/imagem curta avaliado por rubrica) |

### 4.2 JSON Schema (resumido)

```json
{
  "$id": "https://ghdarulms.dev/schemas/lesson/v1",
  "type": "object",
  "required": ["id", "version", "conceptId", "bloomTarget", "estimatedMinutes", "cards"],
  "properties": {
    "id": { "type": "string", "format": "uuid" },
    "version": { "type": "integer" },
    "conceptId": { "type": "string" },
    "bloomTarget": { "enum": ["remember","understand","apply","analyze","evaluate","create"] },
    "estimatedMinutes": { "type": "number", "minimum": 3, "maximum": 7 },
    "cards": {
      "type": "array", "minItems": 1,
      "items": {
        "type": "object",
        "required": ["id", "type", "bloom", "payload"],
        "properties": {
          "id": { "type": "string" },
          "type": { "enum": ["explain","flashcard","multiple_choice","short_answer_ai","order_items","categorize","compare","judgment","mini_production","short_code"] },
          "bloom": { "$ref": "#/properties/bloomTarget" },
          "sources": { "type": "array", "items": { "$ref": "#/$defs/sourceRef" } },
          "payload": { "type": "object" },
          "scoring": { "$ref": "#/$defs/scoring" }
        },
        "discriminator": { "propertyName": "type" }
      }
    }
  },
  "$defs": {
    "sourceRef": { "type": "object", "required": ["materialId","anchor"], "properties": { "materialId": {"type":"string"}, "anchor": {"type":"string"}, "label": {"type":"string"} } },
    "scoring": { "type": "object", "properties": { "mode": { "enum": ["local","ai"] }, "rubricId": {"type":"string"}, "maxScore": {"type":"number"} } }
  }
}
```

Os `payload`s são validados por schemas Zod discriminados por `type` em `packages/lesson-engine/src/schema/`. O mentor nunca edita JSON; o editor Tiptap e formulários geram esse formato.

### 4.3 Contrato de evidência

Todo card emite exatamente um objeto `Evidence` ao ser concluído (ou vários, um por tentativa, se `allowRetry`):

```ts
interface Evidence {
  id: string;                 // uuid v7 gerado no cliente (idempotência)
  sessionId: string;
  lessonId: string; lessonVersion: number;
  cardId: string; cardType: CardType;
  conceptId: string; bloom: BloomLevel;
  attempt: number;
  response: unknown;          // tipado por cardType
  outcome: 'correct' | 'partial' | 'incorrect' | 'pending_ai' | 'skipped';
  score?: number;             // 0..1 quando conhecido localmente
  aiEvaluation?: { score: number; feedback: string; rubricHits: string[]; model: string };
  latencyMs: number;          // tempo até responder
  hintsUsed: number;
  clientTs: string;           // ISO, para ordenação offline
}
```

`outcome: 'pending_ai'` é resolvido quando o backend devolve a avaliação; a UI mostra feedback local imediato (por exemplo, "resposta enviada, avaliando...") e substitui pelo feedback da IA quando chega, sem bloquear o avanço.

### 4.4 Máquina de estados da sessão (XState)

```
idle ──start──▶ card.presenting ──answer──▶ card.evaluating ──▶ card.feedback
                     ▲                          │ (ai: pending)         │
                     │                          └──────────────────────▶│
                     │                                                   │ next
                     └──────── resume (from snapshot) ◀── suspended ◀────┤ (visibilitychange / offline)
                                                                         │
                                                              last card ─┴──▶ completed ──▶ summary
```

Estados: `idle`, `presenting`, `evaluating`, `feedback`, `suspended`, `completed`, `error`. Eventos: `START`, `ANSWER`, `HINT`, `SKIP`, `NEXT`, `SUSPEND`, `RESUME`, `AI_RESULT`, `FAIL`. Ações de saída de `feedback` enfileiram a `Evidence` no `offline` queue (nunca chamam rede diretamente).

### 4.5 Persistência para retomada

`SessionSnapshot { sessionId, lessonId, lessonVersion, cardIndex, machineState, answersDraft, startedAt, updatedAt }` gravado em IndexedDB (Dexie, tabela `sessions`) a cada transição e no `visibilitychange`. Ao abrir a lição, se existir snapshot com mesmo `lessonVersion` e menos de 7 dias, o motor oferece "Continuar de onde parou". Se a versão mudou, descarta e avisa.

### 4.6 Exemplo de micro-lição

```json
{
  "id": "6f1c1c1e-4d1a-7a2b-9f3e-0c2b7d1a9e10",
  "version": 3,
  "conceptId": "fotossintese.fase-clara",
  "bloomTarget": "analyze",
  "estimatedMinutes": 5,
  "cards": [
    {
      "id": "c1", "type": "explain", "bloom": "remember",
      "sources": [{ "materialId": "m-42", "anchor": "p3#s2", "label": "Apostila, p. 3" }],
      "payload": { "markdown": "Na **fase clara**, a luz excita a clorofila no fotossistema II...", "media": [{ "kind": "image", "src": "cdn://img/fase-clara.webp", "alt": "Esquema da membrana do tilacoide" }] }
    },
    {
      "id": "c2", "type": "flashcard", "bloom": "remember",
      "payload": { "front": "Onde ocorre a fase clara?", "back": "Na membrana dos tilacoides.", "selfRate": true },
      "scoring": { "mode": "local" }
    },
    {
      "id": "c3", "type": "multiple_choice", "bloom": "understand",
      "payload": {
        "stem": "Qual é o papel da água na fase clara?",
        "options": [
          { "id": "a", "text": "Doar elétrons ao fotossistema II", "correct": true },
          { "id": "b", "text": "Fixar CO₂", "correct": false, "feedback": "Isso ocorre no ciclo de Calvin." },
          { "id": "c", "text": "Produzir glicose", "correct": false }
        ],
        "requireJustification": true, "shuffle": true
      },
      "scoring": { "mode": "ai", "rubricId": "r-justificativa-curta", "maxScore": 1 }
    },
    {
      "id": "c4", "type": "order_items", "bloom": "apply",
      "payload": {
        "prompt": "Ordene os eventos da fase clara.",
        "items": [
          { "id": "i1", "text": "Fotólise da água" },
          { "id": "i2", "text": "Excitação da clorofila no PSII" },
          { "id": "i3", "text": "Transporte de elétrons pela cadeia" },
          { "id": "i4", "text": "Síntese de ATP pela ATP sintase" }
        ],
        "correctOrder": ["i2", "i1", "i3", "i4"], "partialCredit": true
      },
      "scoring": { "mode": "local", "maxScore": 1 }
    },
    {
      "id": "c5", "type": "judgment", "bloom": "evaluate",
      "payload": {
        "claim": "Sem luz, a planta ainda produz ATP pela fase clara.",
        "options": ["concordo", "discordo"], "expected": "discordo",
        "justificationMinChars": 40
      },
      "scoring": { "mode": "ai", "rubricId": "r-julgamento-causal", "maxScore": 2 }
    }
  ]
}
```

## 5. Tutor IA na UI (`tutor-chat`)

**Transporte.** `fetch('/api/tutor/stream', { method: 'POST', body, signal })` com leitura via `response.body.getReader()` e parser SSE próprio (não usar `EventSource`: não suporta POST nem headers). Eventos: `token`, `citation`, `suggestion`, `tool_status`, `done`, `error`. Reconexão com `Last-Event-ID` e backoff exponencial (1s, 2s, 4s, máx. 3 tentativas); se cair no meio da resposta, a UI mantém o texto parcial e mostra "Continuar".

**Contexto enviado.** `{ courseId, conceptId, lessonId, cardId, cardState: 'feedback', lastEvidenceId, masterySnapshot, locale }`. O cliente nunca envia o conteúdo da lição (o backend tem); envia só identificadores e a resposta atual do aluno, se relevante.

**Renderização incremental.** Tokens acumulam em um buffer; a cada `requestAnimationFrame` o markdown é reprocessado apenas para o último bloco aberto (parágrafo/lista) usando `react-markdown` com `rehype-sanitize`; blocos fechados são memoizados por hash. Isso evita re-parse de O(n²) e flicker.

**Citações.** Evento `citation` traz `{ n, materialId, anchor, excerpt }`; o markdown renderiza `[n]` como `<button>` com `aria-label="Fonte n"`. Toque abre um `Sheet` (Radix Dialog) com o trecho destacado e link "Abrir no material". Em desktop, popover.

**Chips de sugestão.** Até 3 chips gerados pelo backend (`suggestion` events) mais 2 fixos por estado ("Me dê uma dica", "Explique de outro jeito"). Chips são `<button>` em `role="group"`.

**Voz.** `packages/platform/speech`: usa `SpeechRecognition` quando disponível (Chrome Android); fallback grava com `MediaRecorder` e envia para `/api/transcribe`. Se nenhum estiver disponível, o botão de microfone some (não desabilita). Indicador visual de escuta e transcrição parcial no input.

**Estados.** `idle`, `streaming`, `paused_offline`, `error_retryable`, `error_fatal`, `cancelled`. Cancelamento por `AbortController` ao tocar "Parar" ou ao navegar. Mensagens com `role="log"` e `aria-live="polite"` só no fechamento de cada parágrafo, para não inundar leitores de tela.

## 6. Offline e sincronização (`offline`)

- **Service Worker** via Serwist (Workbox): precache do shell; `StaleWhileRevalidate` para `/api/courses/*` e `/api/concepts/*`; `CacheFirst` com expiração de 30 dias para mídia de lições; `NetworkOnly` para `/api/tutor/*` e auth.
- **Download de lições.** Botão "Baixar módulo" busca as lições do conceito e seus assets, grava JSON no Dexie (`lessons`) e mídia no Cache Storage. Badge "disponível offline" no mapa.
- **Fila de submissões.** Tabela `outbox { id (uuid v7), kind: 'evidence' | 'session_complete' | 'chat_message', payload, attempts, nextAttemptAt, status }`. Worker de sync roda em `online`, `visibilitychange` e via Background Sync API onde houver. Envio com header `Idempotency-Key: <evidence.id>`; o backend responde 200 para duplicatas. Ordem preservada por `clientTs`.
- **Indicador.** Pílula no topo: "Offline · 3 respostas pendentes" / "Sincronizando" / oculta quando sincronizado. Zustand `network` slice ouve `navigator.onLine` e testes reais de conectividade (`HEAD /api/health`), pois `onLine` mente em cativos.
- **Conflito.** Evidências nunca conflitam (append-only). Único conflito real: lição atualizada pelo mentor enquanto o aluno tinha versão baixada. Regra: evidências enviadas com `lessonVersion` antigo são aceitas e marcadas `stale` pelo backend; a UI avisa "Esta lição foi atualizada" e oferece rebaixar. Perfil/preferências usam last-write-wins com `updatedAt`.
- **Tutor offline.** Chat desabilitado com mensagem clara; as perguntas digitadas ficam em rascunho local.

## 7. Visualizações

**Mapa de conceitos.** Mobile: lista hierárquica (árvore com `role="tree"`) agrupada por módulo, cada nó com anel de Bloom miniatura e estado (bloqueado, disponível, em progresso, dominado); acima da lista, um mini-grafo SVG (≤ 40 nós visíveis, layout `d3-force` pré-calculado no backend e cacheado como `{x,y}` para não rodar simulação no celular) com pan/zoom via `d3-zoom`. Desktop (mentor e aluno): `reagraph` com layout `forceDirected2d`, clusters por módulo, seleção abre painel lateral. Grafos acima de 300 nós colapsam por módulo.

**Anel de progresso por Bloom.** Componente SVG `BloomRing` em `ui`: 6 arcos de 60° (Memorizar → Criar no sentido horário), cada um preenchido proporcionalmente ao `MasteryState[bloom].score` e com opacidade reduzida quando `notAttempted`. Cores: escala sequencial única (não 6 matizes), para que o anel leia como progressão. Tem `<title>` e uma tabela oculta equivalente para leitores de tela.

**Mapa de calor do mentor.** Unovis `XYContainer` + `Heatmap` (ou grid CSS quando ≤ 30×6 células, mais barato): linhas = conceitos, colunas = níveis de Bloom, célula = mediana de mastery da turma; toque/hover mostra n de alunos e distribuição. Filtro por turma e período. Exportar CSV.

## 8. Performance mobile

Budgets (obrigatórios em CI via Lighthouse CI e `size-limit`):

| Métrica | Alvo |
|---|---|
| LCP, rota de lição, Moto G4 / 3G rápido | < 2,5 s |
| INP | < 200 ms |
| JS inicial (rota de lição), gz | < 200 kB (meta interna 160 kB) |
| CSS inicial | < 30 kB |
| Chunk do `tutor-chat` | < 60 kB, carregado ao abrir o chat |
| Chunk `reagraph` | só no desktop, `dynamic(() => import(...), { ssr: false })` |

Táticas: code-splitting por rota e por tipo de card (`short_code` carrega Monaco só se a lição tiver esse card); imagens em AVIF/WebP via `next/image` com `sizes` correto e placeholder blur; fontes: uma família variável (Inter ou Source Sans 3) em `woff2`, `font-display: swap`, subset latin; `<link rel="prefetch">` da próxima lição prevista (JSON + primeira imagem) quando o aluno chega ao card final; TanStack Query `prefetchQuery` ao focar um nó do mapa; `React.memo` nos cards e `useDeferredValue` no input do chat; evitar `motion` na lista de conceitos. Dispositivo de referência de teste: Android com 2 GB RAM (Moto G-series ou Galaxy A0x) em BrowserStack, CPU throttling 4× no Lighthouse.

## 9. Acessibilidade e qualidade

- WCAG 2.2 AA: contraste 4,5:1, alvos de toque ≥ 24×24 CSS px (meta 44), foco visível, sem armadilhas de foco, `Drag` sempre com alternativa por botões ("mover para cima/baixo") para `order_items` e `categorize` (critério 2.5.7).
- Radix garante roles/teclado; testes com `axe-core` (`vitest-axe` em unitários, `@axe-core/playwright` em E2E) falham a build em violações `serious`/`critical`.
- Testes: Vitest + Testing Library para `lesson-engine` (cada tipo de card tem teste de emissão de `Evidence`), XState `@xstate/test` para a máquina; Playwright E2E em `Pixel 7` e `iPhone 13` emulados cobrindo: concluir lição, retomar sessão, offline → sync, chat com SSE mockado (`page.route`); testes de contrato do `api-client` contra Prism (mock do OpenAPI).
- Storybook 8 para `ui` e `lesson-engine` com addon a11y e Chromatic (ou `storybook test-runner` + snapshots) para regressão visual.
- ESLint flat config (`typescript-eslint`, `jsx-a11y`, `react-hooks`, `tailwindcss`), Prettier, `knip` para código morto, commitlint.
- CI (GitHub Actions + Turborepo remote cache): lint → typecheck → unit → build → size-limit → E2E (sharded) → Lighthouse CI em preview. Deploy de preview por PR.

## 10. Segurança no cliente

- **Tokens.** Sessão em cookie `httpOnly; Secure; SameSite=Lax` emitido pelo backend; o Next.js Route Handler atua como BFF e repassa cookies. Nada de JWT em `localStorage`. Refresh silencioso por endpoint `/auth/refresh`. Para a PWA instalada, o cookie persiste normalmente. Tenant vem do subdomínio ou claim, nunca de parâmetro editável no cliente.
- **CSP.** `default-src 'self'; script-src 'self' 'nonce-…'; style-src 'self' 'unsafe-inline'` (Tailwind gera classes, não inline styles, mas Radix injeta alguns); `img-src 'self' data: https://cdn.ghdarulms…`; `connect-src 'self' https://api…`; `frame-ancestors 'none'`; `object-src 'none'`. Report-only por 2 sprints, depois enforce.
- **Markdown de IA.** Sempre `rehype-sanitize` com schema restrito (sem `html` bruto, sem `javascript:`, links com `rel="noopener noreferrer"`, imagens só de domínios permitidos). KaTeX com `trust: false`. Citações são dados estruturados, não HTML.
- **Upload (mini-produção, material do mentor).** URL pré-assinada do backend; verificação de MIME por magic bytes no cliente (`file-type`) e limite de tamanho antes de enviar; imagens redimensionadas no cliente (Canvas) para ≤ 2048 px; nunca renderizar SVG enviado por usuário como `<img>` inline sem sanitizar (preferir bloquear SVG).
- **Outros.** `Permissions-Policy` restringindo câmera/microfone à própria origem; evitar `dangerouslySetInnerHTML`; dependências auditadas com `pnpm audit` e Renovate; Service Worker só em HTTPS e com escopo limitado.

## 11. Requisitos de Frontend

| ID | Requisito | Prioridade |
|---|---|---|
| FE-01 | App do acadêmico como PWA instalável (manifest, SW, ícones, prompt guiado de instalação no iOS) | Must |
| FE-02 | Monorepo pnpm + Turborepo com apps `student`, `mentor`, `admin` e packages `ui`, `domain`, `api-client`, `lesson-engine`, `tutor-chat`, `offline`, `platform` | Must |
| FE-03 | `api-client` gerado do OpenAPI em CI; build falha em drift | Must |
| FE-04 | `lesson-engine` renderiza os 10 tipos de card a partir do JSON versionado e emite `Evidence` conforme contrato | Must |
| FE-05 | Máquina de estados da sessão com retomada a partir de snapshot em IndexedDB | Must |
| FE-06 | Chat do tutor com streaming SSE via fetch, cancelamento, retentativa com texto parcial preservado | Must |
| FE-07 | Citações tocáveis que abrem o trecho-fonte | Must |
| FE-08 | Renderização de markdown de IA sanitizada (`rehype-sanitize`, schema restrito) | Must |
| FE-09 | Fila offline de evidências com envio idempotente (`Idempotency-Key`) e indicador de estado | Must |
| FE-10 | Download de lições para uso offline (JSON + mídia) | Should |
| FE-11 | Mapa de conceitos: lista hierárquica + mini-grafo no mobile; grafo interativo no desktop | Must |
| FE-12 | `BloomRing` de 6 segmentos com equivalente acessível | Must |
| FE-13 | Mapa de calor conceito × Bloom no dashboard do mentor | Should |
| FE-14 | Editor de autoria (Tiptap) que gera o JSON de lição sem edição manual de JSON | Must |
| FE-15 | Fluxo de revisão/aprovação do mentor com diff entre versões da lição | Should |
| FE-16 | Budgets: LCP < 2,5 s (3G rápido), JS inicial < 200 kB gz, aplicados em CI | Must |
| FE-17 | Code-splitting por rota e por tipo de card; prefetch da próxima lição | Must |
| FE-18 | WCAG 2.2 AA com axe em unitários e E2E; alternativa por botões para arrastar | Must |
| FE-19 | Playwright E2E em viewport mobile cobrindo lição, retomada, offline/sync e chat | Must |
| FE-20 | Storybook do design system com addon a11y e regressão visual | Should |
| FE-21 | Autenticação por cookie httpOnly via BFF; nada de token em storage | Must |
| FE-22 | CSP com nonce em enforce após período report-only | Must |
| FE-23 | Upload via URL pré-assinada com validação de tipo/tamanho e redimensionamento no cliente | Should |
| FE-24 | Entrada de voz no chat com Web Speech API e fallback por gravação | Could |
| FE-25 | Push notifications (Web Push) para revisão espaçada; adaptador `platform` pronto para Capacitor | Should |
| FE-26 | i18n com `next-intl`, pt-BR completo, estrutura pronta para en/es | Should |
| FE-27 | Card `short_code` com editor leve e execução sandboxed | Could |
| FE-28 | Empacotamento Capacitor (iOS/Android) reutilizando a PWA | Could |
| FE-29 | Revisão espaçada: fila diária de flashcards/cards curtos gerada do `MasteryState`, com notificação | Should |
| FE-30 | Tema claro/escuro e `prefers-reduced-motion` respeitados | Should |
