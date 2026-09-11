# Contribuição: Plataformas Educacionais — ghdarulms

> Especialista: plataformas educacionais (Coursera, edX, Udemy, DataCamp, Moodle, Khan Academy, Duolingo, Brilliant; padrões LTI/xAPI/QTI; modelos de negócio).

## 1. Benchmark

| Plataforma | Adotar | Evitar |
|---|---|---|
| **Coursera** | Estrutura semanal clara (módulo → vídeo → leitura → quiz → projeto); certificados com verificação pública por URL; parceria institucional que dá credibilidade; "Specializations" como trilhas encadeadas. | Conclusão abaixo de 10%; vídeo como unidade primária; peer-review mal calibrado; certificado emitido por "assistiu e passou no quiz". |
| **edX** | Open edX como referência de arquitetura aberta (XBlocks, LTI, xAPI nativo); avaliações com tentativas e feedback imediato; "verified track" separando gratuito de certificado pago. | UX pesada de autoria (Studio); pouca personalização; cursos abandonados após a cohort de lançamento. |
| **Udemy** | Onboarding do instrutor em minutos; preview gratuito; Q&A por aula; marketplace com avaliações que expõem qualidade; B2B "Udemy Business" com catálogo por competência. | Excesso de vídeo passivo (cursos de 40h sem prática); sem objetivos de aprendizagem; sem medição de domínio; corrida ao fundo de preço. |
| **DataCamp** | **Exercício-primeiro**: vídeo de 3 min seguido de exercício executável com feedback automático e dicas progressivas (hint → solução com custo de XP); "skill tracks" com assessment de nível; prática diária de 5 min no mobile. | Exercícios "preencha a lacuna" que treinam padrão sem compreensão; pouca transferência para problemas abertos. |
| **Moodle** | Padrões: LTI, SCORM, xAPI, QTI, IMS Common Cartridge; gradebook completo; ecossistema de plugins; conceito de "completion criteria" por atividade; papéis e permissões granulares por contexto. | UX pesada, dezenas de cliques por tarefa; mobile como cidadão de segunda classe; visual datado; configuração exposta demais ao professor. |
| **Khan Academy** | **Mastery learning**: níveis por habilidade (Familiar → Proficiente → Domínio), decaimento e revisão para manter domínio; "unit test" e "course challenge"; dashboards de professor por habilidade; Khanmigo como tutor socrático que não dá a resposta. | Conteúdo estático e não autoral (o professor não insere seu material); pouca motivação social. |
| **Duolingo** | **Hábito**: streaks, sessões de 3–5 min, notificações no horário do usuário, lições curtas com feedback instantâneo, spaced repetition ("Practice"), ligas e metas diárias; mobile-first absoluto; A/B contínuo. | Gamificação que vira ansiedade; corações/vidas como paywall punitivo; profundidade limitada. |
| **Brilliant** | **Aprender fazendo**: cada tela é um problema interativo; explicação só após a tentativa; visualizações manipuláveis; progressão de dificuldade suave; sem vídeo. | Autoria cara (cada lição é artesanal, não escala para conteúdo do mentor); pouco espaço para texto longo/leitura. |

**Padrões extraídos para o ghdarulms:**
1. Exercício-primeiro: a micro-lição termina sempre em atividade; a explicação completa vem depois da tentativa (Brilliant/DataCamp).
2. Maestria, não consumo: progresso = nível Bloom demonstrado por conceito, com decaimento e revisão espaçada (Khan).
3. Hábito: sessões de 3–7 min, streak, meta diária, notificação inteligente (Duolingo).
4. Conteúdo do mentor, não marketplace de vídeo (anti-Udemy): a IA transforma texto/PDF/vídeo em prática.
5. Padrões abertos desde o início (Moodle/Open edX) para entrar em instituições sem substituir o LMS delas.
6. MOOCs ensinam que conclusão exige cohort, prazos suaves e tutor presente — o tutor de IA supre a presença a custo marginal.

## 2. Posicionamento e diferencial

**Frase-âncora:** *"O mentor traz o conteúdo, a IA constrói a trilha por Bloom, o aluno aprende fazendo no celular com um tutor que conhece o material."*

Diferenciais defensáveis:
- **Autoria assistida por Bloom**: nenhuma plataforma pede ao mentor o nível cognitivo alvo e gera a trilha a partir dele. Moodle e Canvas têm "outcomes", mas o professor preenche tudo à mão.
- **Tutor com contexto fechado**: responde apenas com base no material do mentor (RAG sobre o conteúdo do curso), diferentemente de um chat genérico. Reduz alucinação e mantém a voz do mentor.
- **Mobile-first com prática curta**: DataCamp/Duolingo fazem isso, mas com conteúdo próprio; aqui o conteúdo é de terceiros.

Encaixe por segmento:

| Segmento | Dor | Uso do ghdarulms |
|---|---|---|
| **Mentoria individual / cursos livres** (entrada B2C) | Mentor tem PDF/slides e não tem tempo de criar exercícios | Sobe material, escolhe Bloom, publica em horas |
| **Educação superior** | LMS pesado, alunos passivos, professor sobrecarregado | Tool LTI dentro do Moodle/Canvas; nota volta ao gradebook |
| **Corporativo (L&D)** | Compliance e onboarding com SCORM antigo, baixa retenção | Import SCORM legado → trilha com prática; xAPI para o LRS da empresa |
| **Cursinhos/escolas técnicas** | Volume de alunos, necessidade de relatório por habilidade | Cohorts, dashboards de maestria por conceito |

Começar por mentoria individual e cursos livres (ciclo de venda curto, valida a IA), depois superior via LTI, depois corporativo.

## 3. Modelo de conteúdo e estrutura de curso

```
Organização (tenant)
└── Disciplina/Trilha
    └── Unidade (1–8 por trilha)
        └── Conceito (unidade de maestria; 3–10 por unidade)
            ├── Objetivos por Bloom (1 por nível alvo: "Aplicar: calcular X dado Y")
            ├── Micro-lições (3–7 min; texto, áudio, vídeo curto, visualização)
            ├── Atividades (tipos abaixo, vinculadas a um nível Bloom)
            ├── Material complementar (curado pela IA, aprovado pelo mentor)
            └── Pré-requisitos (grafo de conceitos, pode cruzar trilhas)
```

**Tipos de atividade por nível Bloom (mínimo):**
- Memorizar: flashcard com repetição espaçada, múltipla escolha, associação.
- Compreender: explique com suas palavras (avaliado por IA com rubrica), identifique o erro, ordene.
- Aplicar: problema numérico/código com verificação automática, caso curto.
- Analisar: compare dois cenários, classifique, complete o diagrama.
- Avaliar: julgue a solução dada, critique com rubrica, debata com o tutor.
- Criar: projeto/entregável aberto, avaliado por IA + mentor (ou pares), com rubrica gerada.

**Estado de maestria por conceito**: `não iniciado → em progresso → nível Bloom N atingido → dominado (nível alvo) → em revisão (decaimento após 14/30/60 dias)`. O progresso da trilha é a média ponderada dos conceitos dominados, não o percentual de itens abertos.

**Cohort vs self-paced**: toda trilha é self-paced por padrão; o mentor pode abrir uma *turma* (cohort) com data de início, ritmo sugerido (conceitos por semana), fórum/canal e prazos suaves (lembretes, não bloqueio). O mesmo conteúdo serve várias turmas; o material tem versionamento e a turma fica presa à versão publicada no início.

**Pré-requisitos**: grafo dirigido entre conceitos. Pré-requisito "forte" bloqueia, "suave" apenas recomenda. A IA sugere o grafo; o mentor edita. Um *diagnóstico inicial* opcional permite pular conceitos já dominados (como o "course challenge" do Khan).

**Certificação — critérios**:
- Certificado de trilha emitido quando ≥ X% dos conceitos (padrão 90%) atingem o nível Bloom alvo definido pelo mentor, com pelo menos uma atividade dos níveis Analisar+ (se o objetivo for ≥ Analisar) avaliada por rubrica.
- Sem critério de "assistiu N%". Tempo de tela não conta.
- Anti-fraude proporcional: variação de itens por aluno (banco de questões gerado pela IA), atividade de verificação oral via chat de voz opcional, log xAPI auditável.
- Certificado carrega o *perfil de maestria* (conceitos × nível Bloom), não apenas "aprovado".

## 4. Interoperabilidade e padrões

**LTI 1.3 + LTI Advantage (tool provider)**: o ghdarulms se registra como ferramenta em Moodle, Canvas, Blackboard, Brightspace. Usar Deep Linking (mentor escolhe a trilha/unidade de dentro do LMS), Names and Role Provisioning (sincroniza alunos e papéis, elimina matrícula manual), Assignment and Grade Services (devolve nota de maestria ao gradebook como 0–100 ou escala). Login via OIDC do LTI, sem senha extra. Este é o caminho de entrada em instituições; ser *platform* (consumidor de LTI) fica para v2.

**xAPI (Tin Can) como formato canônico de eventos**, com envio opcional a LRS externo (Learning Locker, SCORM Cloud, Watershed) e perfil Caliper 1.2 traduzido para instituições que usam Canvas/Unizin. Vocabulário proposto:

| Evento do sistema | Verbo xAPI | Objeto | Extensão |
|---|---|---|---|
| Aluno abre micro-lição | `experienced` | lesson | `bloom_level`, `concept_id` |
| Conclui micro-lição | `completed` | lesson | duração |
| Tenta atividade | `attempted` | activity | tentativa nº |
| Responde | `answered` | activity | result.success, result.score, `bloom_level` |
| Pede dica ao tutor | `asked` | tutor-session | `hint_level` |
| Tutor responde | `responded` (context: agent=IA) | tutor-session | tokens, fontes citadas |
| Atinge nível Bloom em conceito | `mastered` | concept | `bloom_level: apply` |
| Decaimento / revisão | `reviewed` | concept | intervalo |
| Cohort iniciado | `joined` | cohort | — |
| Certificado | `earned` (ADL) | credential | URL verificável |
| Mentor publica versão | `authored` | course-version | — |

Mapeamento Bloom → verbos xAPI para o resultado de atividades: Memorizar→`recalled`, Compreender→`explained`, Aplicar→`applied`, Analisar→`analyzed`, Avaliar→`evaluated`, Criar→`created`. Registrar como extensão `https://ghdarulms.app/xapi/bloom` no statement `answered`, mantendo compatibilidade com LRS que só conhecem verbos ADL.

**QTI 2.2/3.0**: import de bancos de questões existentes (Moodle exporta QTI; Canvas idem) e export dos itens gerados pela IA, para o mentor não ficar preso. Suportar tipos choice, text-entry, match, extended-text no MVP.

**SCORM 1.2/2004**: somente import, com extração de texto/mídia para alimentar a IA; nunca como formato de entrega (corporativo exige aceitar o legado, mas o player SCORM é um poço de manutenção). Exportar xAPI, não SCORM.

**Open Badges 3.0 / Verifiable Credentials**: cada certificado é um badge assinado com URL pública de verificação e JSON-LD; opção de emissão via Credly/Badgr para instituições que já usam. Incluir o perfil de maestria no `evidence`.

**SSO**: OIDC (Google, Microsoft Entra) e SAML 2.0 para instituições; SCIM para provisionamento em corporativo (v1). Mentor independente: e-mail + magic link + Google.

**Google Classroom/Drive**: importar material do Drive (Docs, Slides, PDF) como fonte; publicar link da trilha no Classroom e sincronizar notas via Classroom API (Should, v1). Microsoft Teams for Education via LTI.

## 5. Papéis e permissões

| Papel | Escopo | Pode |
|---|---|---|
| **Aluno** | trilha/cohort | Estudar, conversar com tutor, ver próprio progresso e certificados, exportar seus dados (LGPD) |
| **Mentor** | trilha | Inserir conteúdo, definir Bloom, revisar/editar o que a IA gerou, publicar versões, abrir cohorts, avaliar entregáveis, ver relatórios de sua trilha, convidar co-mentores |
| **Co-mentor / Monitor** | cohort | Ver progresso, responder no fórum, avaliar entregáveis com rubrica, não edita conteúdo (co-mentor pode editar se autorizado) |
| **Admin de instituição** | tenant | Gerenciar usuários, SSO, LTI, cotas de IA, branding, relatórios agregados, catálogo interno; não vê conversas de tutor sem consentimento configurado |
| **Super-admin** | plataforma | Tenants, planos, feature flags, monitoramento de custo de IA, moderação |

**Multi-tenant**: todo dado pertence a um `org_id`. Um mentor independente é um tenant de uma pessoa (plano B2C); ao entrar numa instituição, a trilha pode ser *copiada* para o tenant institucional (mantendo autoria) ou *compartilhada* via licença. Isolamento por linha (row-level security) com chave de tenant em todo índice; instituições Enterprise podem exigir isolamento de banco. Permissões via RBAC por contexto (plataforma → tenant → trilha → cohort), como o Moodle, mas com apenas cinco papéis fixos no MVP — papéis customizados ficam para v2.

## 6. Modelo de negócio e planos

| Plano | Público | Preço de referência | Inclui | Limites de IA |
|---|---|---|---|---|
| **Free** | Mentor independente testando | R$ 0 | 1 trilha, até 20 alunos, marca ghdarulms | 3 gerações de trilha/mês; tutor 200 mensagens/mês por trilha |
| **Mentor Pro** (B2C) | Mentor com cursos livres | R$ 99–149/mês | Trilhas ilimitadas, 300 alunos, certificados, cohorts, domínio próprio | 30 gerações/mês; tutor 5.000 mensagens/mês; excedente por pacote |
| **Instituição** (B2B) | Escola, faculdade, empresa | R$ 15–25/aluno ativo/mês, mínimo anual | LTI, SSO/SAML, xAPI→LRS, relatórios, admin, SLA | Cota por aluno ativo (ex.: 150 msg/mês), pool no tenant, alerta a 80% |
| **Enterprise** | Universidade/grande empresa | Negociado | Isolamento dedicado, DPA, modelo de IA à escolha, SCIM | Cota negociada, BYO-key opcional |
| **B2B2C** | Instituição vende cursos de mentores aos seus alunos | Revenue share (70/20/10 mentor/instituição/plataforma) | Catálogo com trilhas licenciadas | Cobrado ao tenant vendedor |

**Cobrança por IA**: medir em "créditos" (1 crédito ≈ 1k tokens de saída) para não expor preço de modelo; roteamento por custo (modelo pequeno para tutor de Memorizar/Compreender, modelo grande para avaliação de Criar). Meta: custo de IA < 25% da receita por aluno.

**Marketplace (v2)**: catálogo público de trilhas de mentores Pro com avaliações, preview do primeiro conceito, política de qualidade (trilha precisa ter ≥ 60% das atividades em Aplicar+ para ser listada — antídoto ao Udemy). Plataforma retém 20–30%.

## 7. Table stakes vs pode esperar

**Não podem faltar no MVP**: cadastro/login (e-mail, Google); matrícula por link/código e convite por e-mail/CSV; estrutura trilha→conceito→micro-lição→atividade; progresso por conceito e por trilha; painel do mentor com progresso por aluno e por conceito; notas/relatório exportável em CSV; certificado em PDF com URL de verificação; notificações (e-mail + push PWA) de lembrete e meta diária; busca dentro da trilha; i18n da interface (pt-BR, en, es) e conteúdo em qualquer idioma; LGPD (consentimento, exportar e apagar conta); acessibilidade básica (WCAG 2.1 AA nos fluxos do aluno); PWA instalável e offline para micro-lições já abertas.

**Podem esperar**: fórum completo (usar comentário por conceito no MVP); avaliação por pares; LTI/SAML (v1); xAPI para LRS externo (v1; no MVP apenas armazenar internamente no formato); QTI export (v1); SCORM import (v1); marketplace, papéis customizados, apps nativos (v2); ligas/rankings; vídeo hospedado (embed YouTube/Vimeo no MVP); relatórios de BI customizados.

## 8. Roadmap

**MVP (3 meses) — "um mentor publica e 20 alunos aprendem no celular"**
- Autoria: upload de PDF/DOCX/URL/texto → IA gera conceitos, objetivos Bloom, micro-lições, atividades (6 tipos), material complementar; mentor revisa em editor inline; publicação com versão.
- Aluno: PWA mobile, trilha, micro-lição + atividade, tutor de IA com RAG restrito ao curso, progresso por conceito, streak e meta diária.
- Table stakes acima. Eventos internos já no formato xAPI.
- Critérios de sucesso: 30 mentores publicando; tempo de material → trilha publicada < 2 h; ≥ 40% dos alunos ativos fazem uma sessão em 5 dos 7 dias; ≥ 50% dos conceitos iniciados chegam ao nível alvo; edição pelo mentor de < 30% dos itens gerados (proxy de qualidade da IA); custo de IA por aluno ativo < R$ 4/mês.

**v1 (6 meses) — "entra na instituição"**
- LTI 1.3 Advantage (Deep Linking, NRPS, AGS) certificado com Moodle e Canvas; SSO OIDC/SAML; multi-tenant com admin; cohorts com ritmo e prazos suaves; export xAPI para LRS e Caliper; import QTI e SCORM; export QTI; Open Badges 3.0; Google Classroom; decaimento e revisão espaçada; avaliação de entregáveis (Criar) com rubrica IA + mentor; planos e cobrança.
- Sucesso: 3 instituições pagantes com LTI em produção; NPS mentor ≥ 50; conclusão de trilhas em cohort ≥ 35% (vs < 10% MOOC); nota devolvida ao gradebook sem intervenção manual.

**v2 (12 meses) — "escala e ecossistema"**
- Marketplace B2B2C; diagnóstico inicial adaptativo e pular conceitos; tutor por voz; papéis customizados; SCIM; apps nativos (ou PWA reforçada); API pública + webhooks; analytics de maestria comparativo por instituição; LTI como platform (consumir ferramentas externas, ex.: laboratórios de código).
- Sucesso: 10 mil alunos ativos/mês; GMV do marketplace cobrindo 20% da receita; retenção de mentor Pro ≥ 85% em 12 meses; custo de IA < 25% da receita.

## 9. Requisitos de Plataforma

| ID | Requisito | Prioridade |
|---|---|---|
| PLT-01 | Hierarquia trilha → unidade → conceito → micro-lição → atividade, com versionamento de publicação | Must |
| PLT-02 | Conceito como unidade de maestria com estado por nível Bloom e nível alvo definido pelo mentor | Must |
| PLT-03 | Progresso e conclusão baseados em maestria por conceito; tempo de tela não conta | Must |
| PLT-04 | Seis tipos mínimos de atividade, um por nível Bloom, com feedback imediato | Must |
| PLT-05 | Tutor de IA com contexto restrito ao material da trilha (RAG), citando a fonte | Must |
| PLT-06 | Matrícula por link/código, convite por e-mail e CSV | Must |
| PLT-07 | Painel do mentor: progresso por aluno × conceito, exportação CSV | Must |
| PLT-08 | Certificado PDF com URL pública de verificação e perfil de maestria | Must |
| PLT-09 | Notificações e-mail e push (PWA) com meta diária e lembrete de revisão | Must |
| PLT-10 | PWA mobile-first, instalável, offline para micro-lições já abertas | Must |
| PLT-11 | i18n de interface (pt-BR, en, es) e conteúdo em qualquer idioma | Must |
| PLT-12 | Multi-tenant com isolamento por `org_id` e RBAC de cinco papéis | Must |
| PLT-13 | LGPD: consentimento, exportação e exclusão de dados do aluno | Must |
| PLT-14 | Eventos armazenados internamente no formato xAPI com extensão Bloom | Must |
| PLT-15 | Acessibilidade WCAG 2.1 AA nos fluxos do aluno | Must |
| PLT-16 | Cotas de uso de IA por plano com medição em créditos e alerta a 80% | Must |
| PLT-17 | LTI 1.3 Advantage como tool provider (Deep Linking, NRPS, AGS) | Should (v1) |
| PLT-18 | SSO OIDC e SAML 2.0 para tenants institucionais | Should (v1) |
| PLT-19 | Export xAPI para LRS externo e tradução Caliper 1.2 | Should (v1) |
| PLT-20 | Import QTI 2.2/3.0 e export dos itens gerados | Should (v1) |
| PLT-21 | Import SCORM 1.2/2004 apenas para extração de conteúdo | Should (v1) |
| PLT-22 | Open Badges 3.0 com evidência de maestria; integração Credly/Badgr | Should (v1) |
| PLT-23 | Cohorts com ritmo sugerido, prazos suaves e fórum por conceito | Should (v1) |
| PLT-24 | Decaimento de maestria e revisão espaçada | Should (v1) |
| PLT-25 | Pré-requisitos entre conceitos (forte/suave) e diagnóstico inicial | Should (v1) |
| PLT-26 | Avaliação de entregáveis abertos com rubrica IA + mentor | Should (v1) |
| PLT-27 | Integração Google Classroom/Drive (import de fonte, sync de nota) | Should (v1) |
| PLT-28 | Cobrança por plano (Free, Pro, Instituição, Enterprise) | Should (v1) |
| PLT-29 | Marketplace B2B2C com revenue share e regra de qualidade (≥ 60% Aplicar+) | Could (v2) |
| PLT-30 | SCIM para provisionamento corporativo | Could (v2) |
| PLT-31 | Papéis customizados por tenant | Could (v2) |
| PLT-32 | API pública e webhooks | Could (v2) |
| PLT-33 | Tutor por voz | Could (v2) |
| PLT-34 | LTI como platform (consumir ferramentas externas) | Could (v2) |
| PLT-35 | Avaliação por pares calibrada | Could (v2) |
