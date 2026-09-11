# Contribuição UX Mobile / Microlearning — Experiência do Acadêmico (ghdarulms)

> Especialista: UX/Product Design mobile e microlearning (sessões curtas, interação com uma mão, PWA, acessibilidade).

## 1. Princípios de design mobile para aprendizagem

| # | Princípio | Regra prescritiva |
|---|---|---|
| P1 | **Uma tarefa por tela** | Cada card contém exatamente um estímulo (texto, imagem ou áudio) e uma ação. Nunca combinar leitura + pergunta + chat na mesma tela. Botão primário único, fixo no rodapé. |
| P2 | **Zona do polegar** | Ações primárias (Continuar, Verificar, Enviar) nos 25% inferiores da tela, largura total, altura mínima 56 px. Ações destrutivas ou raras (Sair, Reportar erro) no topo esquerdo, exigindo alcance deliberado. Alvos de toque ≥ 48×48 px com 8 px de espaçamento. |
| P3 | **Sessão interrompível** | Estado da sessão persistido a cada card concluído (local + servidor). Ao reabrir o app em até 24 h, tela "Retomar de onde parou?" com o card exato. Nenhuma sessão exige mais de 7 min; cada card é uma unidade atômica de progresso. |
| P4 | **Latência percebida da IA** | Respostas do tutor e geração de feedback sempre em *streaming* token a token. Se o tempo até o primeiro token > 400 ms, mostrar skeleton (3 linhas cinzas pulsando) com micro-texto "Lendo o material do mentor…". Tempo máximo tolerável sem feedback visual: 1 s. Pré-carregar o próximo card durante a resposta do atual. |
| P5 | **Leitura curta** | Máximo 60–80 palavras por card de conteúdo (≈ 25 s de leitura). Fonte base 17 px, entrelinha 1,5, largura de linha 35–45 caracteres. Se o conceito exigir mais, quebrar em cards sequenciais, nunca em scroll longo. |
| P6 | **Sem scroll infinito** | Toda lista (revisões, trilha, histórico) é finita e paginada com fim explícito ("Você viu tudo por hoje"). O app nunca "puxa" o aluno para consumo passivo. |
| P7 | **Feedback imediato** | Toda resposta do aluno recebe feedback em < 300 ms (correto/incorreto local) e explicação da IA em streaming logo depois. Vibração háptica curta em acerto/erro. |

## 2. Arquitetura de informação do app do aluno

**Tab bar inferior com 5 itens** (ordem por frequência de uso): **Hoje · Trilha · Tutor · Revisões · Perfil**. Progresso fica dentro de Trilha (por conceito) e Perfil (agregado) para não estourar 5 abas. Aba ativa com ícone preenchido + rótulo sempre visível (não depender só de ícone).

| Tela | Conteúdo principal | Estado vazio |
|---|---|---|
| **Hoje** (tela inicial) | Um único card grande "Sua sessão de hoje" com: nome do conceito, nível de Bloom da vez (ex.: "Aplicar"), duração estimada ("~5 min"), botão **Começar**. Abaixo: mini-linha "3 revisões pendentes (1 min)" e sequência de dias. | Antes do mentor publicar: ilustração + "Seu mentor ainda está preparando o conteúdo. Avisamos você." Sem CTA falso. |
| **Trilha** | Mapa de conceitos (ver §5). Topo: seletor de disciplina/curso. Cada nó abre uma folha de detalhes com 6 níveis de Bloom e botão "Praticar este nível". | "Nenhum curso ainda. Peça o código de convite ao seu mentor" + campo de código. |
| **Tutor** | Chat de tela cheia com histórico por curso; chips de sugestão no topo do teclado. | Três chips iniciais: "Explique o conceito de hoje", "Me faça uma pergunta", "O que devo revisar?" |
| **Revisões** | Lista finita de itens de repetição espaçada vencidos, agrupados por conceito, com badge de tempo total ("6 itens · 3 min"). Botão **Revisar tudo**. | "Nada para revisar. Volte amanhã" + data da próxima revisão. |
| **Perfil** | Progresso agregado (anel geral, dias de sequência, minutos na semana), preferências de notificação, acessibilidade, download offline, conta. | — |

**Navegação secundária**: dentro da sessão, a tab bar é ocultada (modo foco); retorna ao concluir ou sair. Deep links: `/sessao/{id}/card/{n}`, `/conceito/{id}`, `/revisao/rapida` (usado por widget e notificação).

## 3. Anatomia da micro-lição no celular

Uma micro-lição = 6 a 12 cards, 3–7 min. Estrutura fixa:

```
[Barra de progresso segmentada — 1 segmento por card, topo, 4 px]
[X Sair]                                   [♡ Salvar]  [? Tutor]
─────────────────────────────────────────────
             ÁREA DO CARD (swipe desabilitado;
             avanço só por botão para evitar pulo acidental)
─────────────────────────────────────────────
[            Botão primário 56 px            ]
```

**Sequência típica de cards:**

| Ordem | Card | Tempo alvo | Interação |
|---|---|---|---|
| 1 | **Objetivo** — "Ao final você vai conseguir: [verbo Bloom + objeto]" | 10 s | Tap Continuar |
| 2–3 | **Conteúdo** — 60–80 palavras ou imagem/diagrama com legenda; áudio opcional (TTS) | 25–40 s | Tap; toque longo em termo destacado abre definição em popover |
| 4 | **Verificação rápida** | 20 s | Ver tabela abaixo |
| 5–6 | Conteúdo → prática (repete o par) | 60 s | — |
| 7+ | **Atividade de nível** (o nível Bloom-alvo da sessão) | 60–120 s | Ver tabela abaixo |
| Último | **Fechamento** — resumo em 3 bullets gerado pela IA, próxima revisão agendada, "Perguntar ao tutor sobre o que ficou confuso" | 15 s | Tap Concluir |

**Interações viáveis por nível de Bloom (toque em uma mão):**

| Nível | Interações mobile | Exemplo de card |
|---|---|---|
| Memorizar | Tap em múltipla escolha (4 opções, pilha vertical); flashcard virar (tap) e auto-avaliar (Fácil/Difícil); completar lacuna com chips | "Toque na definição correta" |
| Compreender | Ordenar frases arrastando (handles grandes à direita); parear conceito ↔ exemplo (tap-tap, não drag); verdadeiro/falso com justificativa em chips | "Arraste para ordenar as etapas" |
| Aplicar | Digitar resposta curta (≤ 140 caracteres, teclado numérico quando aplicável); escolher regra e aplicar a um caso; foto de exercício resolvido no papel (IA avalia) | "Qual o resultado? [campo]" |
| Analisar | Classificar itens em 2–3 caixas (tap no item, tap na caixa); marcar trecho de texto (seleção por toque longo) que sustenta uma afirmação; comparar dois casos em tabela de toggles | "Toque nos itens que são causas, não sintomas" |
| Avaliar | Escala de julgamento (slider 1–5 com rótulos) + justificativa por áudio (gravar até 60 s, IA transcreve e avalia critérios); escolher entre duas respostas de "colegas" e explicar | "Qual solução é melhor e por quê? 🎤" |
| Criar | Gravar explicação por áudio ("ensine para um amigo"); esboço em papel + foto; texto curto em 3 passos guiados (rascunho, revisão pela IA, versão final); construir mapa mental tocando nós | "Crie um exemplo próprio do conceito" |

Regras: cards de Analisar/Avaliar/Criar aceitam **"pular por agora"** (volta na revisão) para não travar a sessão; entrada por texto longo (> 140 caracteres) nunca é obrigatória no celular; desktop pode oferecer variantes expandidas da mesma atividade.

**Sair e retomar**: tap em X abre folha "Sair da sessão? Seu progresso até o card 5 está salvo" com botões *Continuar depois* / *Continuar agora*. Na tela Hoje aparece "Retomar (faltam 3 cards, ~2 min)".

## 4. O tutor de IA no mobile

**Dois modos, mesmo agente:**

1. **Tutor contextual (bottom sheet)** — acionado pelo ícone "?" dentro do card ou automaticamente após 2 erros seguidos. Abre a 60% da altura, arrastável até 90%, com o card visível esmaecido atrás. A IA já recebe o card atual, a resposta do aluno e o objetivo Bloom. Fechar preserva a conversa como thread ligada ao card.
2. **Tutor em tela própria (aba)** — histórico completo por curso, busca, conversas sem contexto de card.

**Elementos da interface:**

- **Chips de sugestão** (máx. 3, roláveis horizontalmente acima do campo): gerados por contexto: "Explique de outro jeito", "Dê um exemplo", "Por que errei?", "Me teste", "Resuma em 1 frase". Chip tocado envia sem digitar.
- **Entrada por voz**: botão de microfone no campo (toque e segure para falar; soltar transcreve; texto editável antes de enviar). Resposta da IA com botão "Ouvir" (TTS) para uso com fone.
- **Citações tocáveis**: quando a IA usa material do mentor, a frase termina com chip `[Aula 2, p. 3]`; tap abre folha com o trecho original destacado e botão "Ver na trilha".
- **Indicador de proveniência** (obrigatório em toda resposta):
  - 📘 **Do material do mentor** — faixa azul fina no topo do balão
  - 💡 **Sugestão da IA (fora do material)** — faixa âmbar + texto "Não faz parte do conteúdo avaliado"
  - 🔗 **Material complementar** — faixa verde com fonte
- **Streaming** com cursor piscante; botão "Parar" durante geração; balões com no máx. 4 parágrafos curtos — se a resposta for longa, a IA oferece "Quer que eu quebre em passos?".
- **Ações no balão** (toque longo): Copiar, Salvar como nota, "Isso estava errado" (feedback para o mentor).

## 5. Visualização de progresso por Bloom

**Anel de 6 segmentos por conceito** (o "Anel Bloom"): círculo de 40 px dividido em 6 arcos iguais, no sentido horário de Memorizar (12 h) a Criar. Cada arco tem 3 estados: vazio (cinza), em andamento (contorno), dominado (preenchido). Cor única por curso, não por nível — a posição codifica o nível, evitando 6 cores. Legenda acessível via toque longo. Preenchimento semântico: dominado = ≥ 80% de acerto em 2 sessões espaçadas.

**Folha de detalhe do conceito**: lista vertical dos 6 níveis, cada linha com ícone, nome, barra fina de 0–100% e botão "Praticar". Só os níveis dentro do objetivo do mentor aparecem ativos; os demais mostram cadeado com texto "Não faz parte deste curso".

**Mapa de conceitos em tela pequena**: evitar grafo livre. Usar **trilha vertical em zigue-zague** (padrão Duolingo) dentro de uma coluna: cada nó é um Anel Bloom + nome; pré-requisitos ligados por linha; nós bloqueados em cinza com cadeado; nó atual pulsa. Agrupamento por módulos com cabeçalho fixo (sticky). Modo alternativo "Mapa de calor": grade de conceitos × 6 níveis, cada célula com tonalidade por domínio — cabe em 320 px de largura com nomes truncados e toque para expandir. Desktop pode oferecer o grafo completo com zoom; no mobile ele fica atrás de um botão "Ver mapa completo" com pan/pinch.

## 6. Notificações e hábito

- **Permissão**: pedir só após a primeira sessão concluída, com tela de pré-permissão ("Quer um lembrete diário de 5 min? Você escolhe o horário"). Se negar, não pedir de novo por 14 dias.
- **Lembrete inteligente**: o aluno define janela (ex.: 19h–21h); o app aprende o horário real de uso e envia no ponto mais provável dentro da janela. Nunca fora de 7h–22h local. Máximo 1 push/dia + 1 de revisão vencida a cada 3 dias. Copy concreta: "Revisão de 'Fotossíntese' leva 1 min. Tocar para começar."
- **Ações na notificação**: "Revisar 1 min" (abre `/revisao/rapida`) e "Hoje não" (adia sem culpa).
- **Widget / atalho "1 minuto de revisão"**: no nativo, widget de tela inicial com 1 flashcard; no PWA, atalho de app (`shortcuts` do manifest) e ícone na tela inicial que abre direto a revisão rápida.
- **Sequência (streak)** com "congelamento" gratuito 1×/semana — hábito sem punição.
- **Offline**: a sessão de hoje + revisões pendentes + últimos 3 conceitos são pré-baixados ao abrir com Wi-Fi (< 5 MB). Interações que não dependem da IA funcionam offline; as que dependem (feedback aberto, tutor) enfileiram com selo "Será avaliado quando conectar". Sincronização em segundo plano com resolução por *last-write-wins* por card e indicador discreto de estado ("Sincronizado 14:02").

## 7. Acessibilidade e inclusão

- **WCAG 2.2 AA**: contraste ≥ 4,5:1 (texto) e 3:1 (componentes); alvos ≥ 24×24 px (critério 2.5.8) — o app adota 48 px; nada depende apenas de cor (anéis usam preenchimento + textura); foco visível; sem limite de tempo por card (o "tempo alvo" é só orientação interna).
- **Leitor de tela** (TalkBack/VoiceOver): ordem lógica de foco; cada card anunciado como "Card 3 de 9, pergunta"; resultados de drag-and-drop têm alternativa por botões "Mover para cima/baixo"; balões do tutor em `aria-live="polite"` com anúncio ao final do streaming, não a cada token.
- **Fonte dinâmica**: respeitar preferência do sistema até 200% sem quebra de layout (cards usam altura flexível; botão fixo nunca cobre conteúdo).
- **Modo escuro** automático + manual; **Reduzir movimento** desliga animações e confetes.
- **Dados móveis baixos**: imagens em WebP com `srcset`, áudio em Opus, modo "Economizar dados" que substitui vídeos por transcrição e adia downloads.
- **Android de entrada**: alvo de desempenho em aparelho com 2 GB de RAM; bundle inicial < 300 KB gzip; primeiro card interativo em < 3 s em 3G; sem animações de blur/parallax.
- **Linguagem**: PT-BR simples, opção de dislexia (fonte com espaçamento maior), TTS em todo conteúdo.

## 8. PWA vs app nativo

**Recomendação para o MVP: PWA instalável**, com arquitetura preparada para empacotamento posterior (Capacitor/TWA).

| Necessidade | PWA (2026) | Nativo | Veredito MVP |
|---|---|---|---|
| Push | Android: completo. iOS 16.4+: funciona só se instalado na tela inicial | Completo | Aceitável; orientar instalação no iOS |
| Offline | Service Worker + IndexedDB cobre sessão e revisões | Completo | Suficiente |
| Instalação | Prompt de instalação + TWA na Play Store sem reescrever | Loja | Suficiente |
| Câmera/microfone | `getUserMedia` e `<input capture>` funcionam em ambos | Completo | Suficiente |
| Widget de tela inicial | Não disponível | Sim | Única lacuna relevante — substituir por atalho |
| Custo/velocidade | Uma base de código, deploy instantâneo, desktop grátis | 2–3 bases | Decisivo |

**Evolução**: quando métricas mostrarem retenção D30 > 20% e demanda por widget/push iOS sem instalação, empacotar a mesma base com Capacitor para ganhar widget, push nativo e presença nas lojas. Não construir nativo puro antes de validar o formato das micro-lições.

## 9. Métricas de UX a instrumentar

| Métrica | Definição | Alvo inicial |
|---|---|---|
| Tempo até primeira ação (TTFA) | Abertura do app → primeiro toque em Começar/Revisar | < 10 s |
| Taxa de conclusão de sessão | Sessões iniciadas que chegam ao card de fechamento | > 70% |
| Abandono por card | % de saídas em cada card, por tipo de interação e nível Bloom | Identificar cards > 15% |
| Taxa de retomada | Sessões interrompidas retomadas em 24 h | > 50% |
| Tempo real por card vs. alvo | Mediana por tipo de card | Dentro de ±30% |
| Uso do tutor | % de sessões com ≥ 1 mensagem; origem (chip, voz, digitado, automático) | 30–50% |
| Latência percebida | Tempo até primeiro token; % de respostas > 3 s | p95 < 1,5 s |
| Taxa de "Sugestão da IA" | % de respostas fora do material do mentor | Monitorar; alertar mentor se > 30% |
| Revisão rápida via notificação/atalho | Taps na ação "Revisar 1 min" → conclusão | > 40% |
| Retenção D1/D7/D30 e sequência média | Padrão | D7 > 35% |
| Acessibilidade | % de usuários com fonte ampliada, leitor de tela, modo escuro | Garantir zero erros de layout |

## 10. Requisitos de UX Mobile

| ID | Requisito | Prioridade |
|---|---|---|
| UXM-01 | Cada card apresenta um único estímulo e uma única ação, com botão primário fixo no rodapé (≥ 56 px, zona do polegar) | Must |
| UXM-02 | Micro-lição de 6–12 cards, 3–7 min, com barra de progresso segmentada no topo | Must |
| UXM-03 | Progresso persistido por card; retomada em até 24 h no card exato, com prompt "Retomar" na tela Hoje | Must |
| UXM-04 | Todas as respostas da IA em streaming; skeleton se > 400 ms; botão "Parar" | Must |
| UXM-05 | Cards de conteúdo limitados a 60–80 palavras; sem scroll infinito em nenhuma lista | Must |
| UXM-06 | Tab bar com Hoje, Trilha, Tutor, Revisões, Perfil; oculta durante a sessão | Must |
| UXM-07 | Interações por nível de Bloom conforme §3, incluindo múltipla escolha, ordenar, parear, resposta curta, áudio e foto | Must (tap/ordenar/texto curto) / Should (áudio/foto) |
| UXM-08 | Tutor contextual em bottom sheet com contexto do card atual, e aba de chat com histórico | Must |
| UXM-09 | Chips de sugestão contextuais (máx. 3) acima do campo de mensagem | Must |
| UXM-10 | Indicador de proveniência em toda resposta da IA (material do mentor / sugestão / complementar) e citações tocáveis | Must |
| UXM-11 | Entrada por voz (segurar para falar) e TTS nas respostas e conteúdos | Should |
| UXM-12 | Anel Bloom de 6 segmentos por conceito e folha de detalhe com barras por nível | Must |
| UXM-13 | Trilha vertical em zigue-zague com pré-requisitos e nó atual; mapa de calor como visão alternativa | Must / Should (mapa de calor) |
| UXM-14 | Grafo completo com pan/zoom atrás de botão "Ver mapa completo" | Could |
| UXM-15 | Pedido de permissão de push somente após a primeira sessão, com tela de pré-permissão | Must |
| UXM-16 | Lembrete diário dentro da janela definida pelo aluno, máx. 1/dia, nunca entre 22h–7h, com ação "Revisar 1 min" | Should |
| UXM-17 | Atalho/widget "1 minuto de revisão" abrindo `/revisao/rapida` | Should (atalho PWA) / Could (widget nativo) |
| UXM-18 | Pré-download da sessão do dia e revisões; interações não dependentes da IA funcionam offline; fila de sincronização com indicador | Must |
| UXM-19 | Conformidade WCAG 2.2 AA: contraste, alvos ≥ 48 px, foco visível, alternativas a arrastar, sem limite de tempo | Must |
| UXM-20 | Suporte a leitor de tela com anúncio de posição no card e `aria-live` no tutor | Must |
| UXM-21 | Fonte dinâmica até 200%, modo escuro, reduzir movimento, modo economizar dados | Must |
| UXM-22 | Desempenho: primeiro card interativo < 3 s em 3G e aparelho com 2 GB de RAM; bundle inicial < 300 KB gzip | Must |
| UXM-23 | Entrega como PWA instalável com manifest, service worker e TWA na Play Store; base preparada para Capacitor | Must |
| UXM-24 | Instrumentação das métricas da §9 com eventos por card (`card_view`, `card_answer`, `card_exit`, `tutor_open`, `session_complete`) | Must |
| UXM-25 | Sequência de dias com congelamento gratuito semanal | Could |
| UXM-26 | Ação "Isso estava errado" em respostas do tutor, encaminhada ao mentor | Should |
