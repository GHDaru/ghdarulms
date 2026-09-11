# Active Learning no ghdarulms — Contribuição à Especificação

> Especialista: Active Learning e metodologias ativas (PBL, peer instruction, retrieval practice, autorregulação, Self-Determination Theory).

## 1. Princípios e o loop da micro-lição

**Fazer antes de ler.** Quando o aluno tenta resolver algo antes de receber a explicação, três efeitos se somam: (a) *productive failure* — a tentativa ativa esquemas prévios e cria "ganchos" para o conteúdo; (b) *pretesting effect* — errar em um teste prévio melhora a retenção do conteúdo que vem depois; (c) *generation effect* — o que é gerado pelo aluno é lembrado melhor do que o que é lido. Consequência de produto: **nenhuma micro-lição começa com texto ou vídeo explicativo**. Começa com uma pergunta, um caso ou um problema.

**Loop padrão da micro-lição (alvo 5 min, faixa 3–7):**

| Etapa | Tempo | O que acontece | Regra de produto |
|---|---|---|---|
| 1. Ativar | 30 s | Uma pergunta que conecta o conceito a algo que o aluno já domina ("Você já viu X; o que acha que acontece quando…?") | Gerada pela IA a partir dos pré-requisitos do grafo de conceitos |
| 2. Tentar | 60–90 s | O aluno responde/tenta o problema **antes** de qualquer explicação, com previsão de confiança | Botão "não sei" é permitido, mas exige um palpite escrito |
| 3. Conteúdo mínimo | 60–90 s | Explicação curta (≤ 150 palavras ou ≤ 60 s de mídia) focada no que a tentativa expôs | A IA escolhe a variante do conteúdo conforme o erro cometido |
| 4. Praticar | 60–120 s | 2–3 itens da mecânica correspondente ao nível de Bloom do objetivo | Feedback imediato, mas gradual (dica → pista → resposta) |
| 5. Refletir | 20–30 s | Diário de 1 linha + calibração: "Sua confiança era 4/5, acertou 1/3" | Obrigatório, mas nunca mais de uma linha |
| 6. Próximo passo | 10 s | A IA propõe 2 opções (avançar, reforçar, revisar algo antigo); o aluno escolhe | Sempre mostrar por quê a opção foi sugerida |

## 2. Catálogo de mecânicas ativas (mobile-first) mapeadas a Bloom

| Nível | Mecânica | Duração | Input no celular | Papel da IA | Evidência de maestria registrada |
|---|---|---|---|---|---|
| **Memorizar** | Flashcards com recuperação livre (o aluno digita/fala antes de virar) | 30–60 s/card | Texto curto ou voz-para-texto; depois toque em "acertei / quase / errei" | Gera cards a partir dos conceitos; compara resposta livre com a esperada e sugere o autojulgamento; agenda por repetição espaçada (SM-2 ou FSRS) | Intervalo de retenção alcançado (ex.: ≥ 7 dias com acerto); taxa de acerto em recuperação livre |
| **Memorizar** | Completar lacunas (cloze) em definição ou fórmula | 30 s | Toque em chips ou digitação de 1 palavra | Escolhe lacunas que carregam significado (não conectivos) | Acerto sem dica em 2 sessões distintas |
| **Compreender** | "Explique com suas palavras" | 60–90 s | Texto de 2–4 linhas ou áudio de 30 s transcrito | Avalia por rubrica (ideia central, relação causal, exemplo próprio); devolve 1 ponto forte + 1 lacuna; **não reescreve** a resposta | Nota da rubrica ≥ 3/4 em duas explicações sobre o mesmo conceito, com pelo menos 1 dia entre elas |
| **Compreender** | Escolha da melhor analogia + justificativa | 45 s | Toque + frase curta | Gera 3 analogias (1 boa, 2 com falhas sutis); avalia a justificativa | Justificativa aponta a falha das analogias erradas |
| **Aplicar** | Mini-problema (cálculo, código curto, caso) | 90–150 s | Teclado numérico, editor de código com 1 função de ≤ 10 linhas, ou preencher template | Gera variantes com dados diferentes; avalia por testes automáticos (código) ou tolerância (cálculo); dá dica escalonada | 2 variantes distintas resolvidas sem dica |
| **Aplicar** | "Preveja a saída / o resultado" | 45 s | Chips ou número | Gera cenário e distratores baseados em erros comuns | Acerto + explicação correta do porquê |
| **Analisar** | Classificar / ordenar / comparar por arrastar | 60–90 s | Drag-and-drop com alvos grandes (≥ 44 px), ou toque sequencial | Gera itens e categorias; pede justificativa para 1 item posicionado (o mais ambíguo) | Classificação correta + justificativa aceita |
| **Analisar** | "Encontre o erro" em resposta/código/argumento | 60–90 s | Toque na linha/trecho + frase | Produz exemplos com erro plantado; valida localização e diagnóstico | Erro localizado e nomeado corretamente |
| **Avaliar** | "Julgue e justifique" (duas soluções ou afirmações) | 90–120 s | Escolha + 3 linhas de justificativa por critério | Fornece critérios explícitos; avalia se a justificativa usa os critérios e evidência, não só opinião | Justificativa cobre ≥ 2 critérios com evidência |
| **Avaliar** | Peer review de artefato (assíncrona, ver §5) | 3–4 min | Rubrica com toques + comentário | Modera tom, verifica se comentário é acionável | Comentário avaliado como útil pelo autor ou pela IA |
| **Criar** | "Produza um artefato pequeno" com rubrica (plano, esboço, função, parágrafo, mapa) | 5–7 min (pode dividir em 2 sessões) | Texto, foto de rascunho no papel, áudio, código | Entrega a rubrica **antes**; faz 2 perguntas socráticas durante; avalia por rubrica; sugere 1 iteração | Artefato com rubrica ≥ 75% + iteração feita; artefato vai para o portfólio |
| **Criar** | "Ensine a IA" (protégé effect, ver §5) | 3–4 min | Texto ou voz em diálogo | Age como aluno com dúvidas plausíveis | Aluno responde às dúvidas sem contradições |

Regra transversal: **cada conceito tem no mínimo uma mecânica em cada nível de Bloom até o nível-alvo definido pelo mentor**; a IA não gera mecânicas acima do nível-alvo, mas gera todas abaixo dele (o aluno precisa lembrar e compreender antes de aplicar).

## 3. Autorregulação e metacognição

- **Previsão de confiança antes de responder**: slider de 1–5 (ou 3 toques: "chuto / acho que sei / tenho certeza") obrigatório antes de cada item de prática. Custo: 1 s.
- **Calibração**: após o feedback, uma linha compara previsão × resultado. Semanalmente, um gráfico de calibração simples (confiança média vs. acerto médio por conceito). Conceitos com **alta confiança e baixo acerto** são os primeiros da fila de revisão — são os mais perigosos.
- **Diário de 1 linha**: ao final da micro-lição, campo com prompt rotativo ("O que ficou mais claro?", "O que ainda confunde?", "Onde você usaria isso?"). Voz aceita. A IA usa o diário para ajustar a próxima sessão e para a síntese semanal.
- **Meta diária**: escolhida pelo aluno entre 1, 2 ou 3 micro-lições (ou "1 minuto" em dias ruins). A IA sugere com base no histórico, nunca impõe. Meta cumprida ≠ pontos; cumprida = registro visível na trilha.
- **Revisão semanal gerada pela IA** (leitura de 2 min, domingo ou dia escolhido): o que avançou por nível de Bloom, 3 conceitos que pedem revisão (com o motivo: calibração ruim, esquecimento previsto, erro recorrente), 1 frase do próprio diário do aluno citada de volta, e uma pergunta de planejamento ("Quantas sessões esta semana?").

## 4. Motivação — Self-Determination Theory

**Autonomia.** O aluno escolhe: ordem entre conceitos desbloqueados (o grafo de pré-requisitos define o que está disponível, não a sequência exata); formato preferido de conteúdo mínimo (texto, áudio, esquema visual); mecânica alternativa quando houver duas para o mesmo nível; meta diária. Toda sugestão da IA vem com "por quê" e um botão "prefiro outro".

**Competência.** Progressão por nível de Bloom **visível por conceito**: cada conceito é um pequeno medidor de 6 segmentos (Memorizar → Criar), preenchido conforme evidência de maestria (§2). Feedback sempre orientado à ação ("você já aplica; falta analisar casos-limite"), nunca comparativo com outros. Dificuldade adaptativa: a IA mantém taxa de acerto na primeira tentativa entre ~60% e ~80% — abaixo disso, frustra; acima, entedia.

**Pertencimento (mesmo em curso individual).** O mentor é visível: foto, mensagem de boas-vindas, "desafio da semana" assinado, e comentários em artefatos selecionados. Marcos ("você concluiu o bloco 2 — 340 pessoas passaram por aqui") criam senso de trajetória compartilhada sem ranking. Cohort opcional: grupos de 5–8 alunos que iniciam na mesma semana, com um fio de discussão moderado pela IA (§5).

**Gamificação — usar:** streak suave (conta dias com qualquer atividade, inclusive "sessão de 1 minuto"; permite 1 "congelamento" por semana; perder o streak nunca gera mensagem negativa); marcos de maestria por nível de Bloom; portfólio de artefatos criados; "conceito consolidado" quando atinge retenção de 30 dias.
**Evitar:** ranking público, pontos sem lastro em maestria, moedas/lojas, notificações de culpa ("você está ficando para trás"), badges por volume (ex.: "100 flashcards vistos").

## 5. Interação social e peer learning viáveis com IA

- **Peer review assíncrona opcional** (nível Avaliar): artefatos do nível Criar podem ser marcados pelo autor como "aberto a revisão". Outro aluno da mesma trilha recebe o artefato, a rubrica e um roteiro de 3 perguntas. A IA verifica antes de publicar: tom respeitoso, comentário específico, ao menos 1 sugestão. Quem revisa ganha evidência em Avaliar; quem recebe pode responder com iteração. Anonimato por padrão.
- **"Ensine a IA"** (protégé effect): a IA assume persona de aluno iniciante e pede que o aluno explique o conceito. Faz 2–3 perguntas típicas de quem não entendeu ("Mas por que não pode ser o contrário?", "Me dá um exemplo do dia a dia?"), comete um erro deliberado para o aluno corrigir, e encerra resumindo "o que aprendi com você". O aluno recebe feedback sobre onde a explicação foi imprecisa. Esta mecânica gera a evidência mais forte de Compreender/Criar e deve ser oferecida ao final de cada bloco de conceitos.
- **Discussões moderadas por IA** (cohort ou trilha inteira): 1 pergunta aberta por semana, criada pelo mentor ou pela IA a partir do desafio da semana. Respostas curtas (≤ 300 caracteres). A IA agrupa respostas por posição, destaca contrastes ("A e B discordam sobre X — o que os dados dizem?") e sintetiza em 5 linhas para o mentor. Sem curtidas.
- **Pareamento "quem errou o que eu acertei"**: a IA pode oferecer, com consentimento, que um aluno que consolidou um conceito revise a explicação de outro que travou — troca de 2 minutos, assíncrona.

## 6. Hábito e microlearning

- **Sessões de 3–7 min**, com barra de tempo estimado visível antes de começar. Nada exige mais de 7 min contínuos; artefatos maiores são divididos em partes com salvamento automático.
- **Lembretes inteligentes**: um por dia no máximo, no horário em que o aluno historicamente mais abre o app (aprendido, não configurado); conteúdo do lembrete é uma pergunta de recuperação ("Você lembra o que diferencia X de Y? 1 minuto."), não um aviso. Silenciar por 1 semana com um toque, sem perder nada.
- **"Sessão de 1 minuto"** para dias ruins: 2 flashcards vencidos + 1 previsão de confiança. Conta para o streak e para a repetição espaçada. Sempre acessível na tela inicial.
- **Offline**: as próximas 3 micro-lições e os flashcards vencidos são baixados previamente; respostas ficam em fila e a avaliação por IA acontece na sincronização, com feedback local de gabarito quando houver resposta fechada. Indicador claro de "avaliação pendente".
- **Reinício sem culpa**: após ausência de 7+ dias, a primeira sessão é uma "sessão de retorno" de 3 min com os 5 conceitos mais fortes (para recuperar confiança) e um convite a redefinir a meta.

## 7. Papel do mentor nas metodologias ativas

O mentor não é substituído pela IA; ele governa a intenção pedagógica. Funções obrigatórias no produto:

1. **Definir o nível-alvo de Bloom por bloco** e revisar/editar objetivos e mecânicas geradas pela IA antes da publicação (aprovação em lote, com edição pontual).
2. **Desafio da semana**: um problema autêntico e aberto (nível Aplicar ou acima) escrito pelo mentor, com sua voz. A IA distribui, coleta e agrupa as respostas.
3. **Síntese**: o mentor recebe da IA um resumo semanal (erros mais comuns, conceitos com calibração ruim, trechos anônimos de diários) e grava uma resposta de ≤ 2 min (áudio, vídeo ou texto) que vira o "conteúdo mínimo" de uma micro-lição de revisão.
4. **Intervenção em quem travou**: painel com alunos sinalizados pela IA (3 falhas seguidas no mesmo conceito, 10 dias sem atividade, calibração invertida). O mentor envia mensagem em 1 toque, com rascunho sugerido pela IA, ou redireciona para um caminho alternativo.
5. **Curadoria de artefatos**: destacar 1–3 artefatos por semana (com permissão) como exemplos para a cohort.

## 8. Anti-padrões a evitar

- **Vídeo passivo longo**: mídia > 90 s sem interação intercalada. Se o mentor subir uma aula de 40 min, a IA a fragmenta e cada fragmento é precedido por uma pergunta.
- **Múltipla escolha trivial em excesso**: itens em que o distrator é obviamente errado; mais de 40% dos itens de uma trilha em formato de escolha fechada; nenhuma escolha fechada sem justificativa quando o objetivo está em Analisar ou acima.
- **Dar a resposta cedo demais**: o tutor de IA nunca entrega a resposta na primeira interação — segue a escada dica → pista → exemplo análogo → resposta com explicação. Exceção: o aluno pede explicitamente após 2 tentativas.
- **Tutor tagarela**: respostas do chat com mais de 120 palavras no celular. Preferir pergunta de volta a explicação.
- **Feedback apenas certo/errado**: todo feedback nomeia o tipo de erro e o próximo passo.
- **Progresso por consumo**: barra que avança porque o aluno "viu" o conteúdo. Progresso só avança com evidência de maestria.
- **Gamificação de volume**: recompensar quantidade de itens, tempo no app ou logins.
- **Sessões que não cabem no bolso**: qualquer tarefa que exija teclado físico, tela grande ou mais de 7 minutos contínuos sem ponto de pausa.

## 9. Requisitos de Active Learning

| ID | Requisito | Prioridade |
|---|---|---|
| AL-01 | Toda micro-lição segue o loop Ativar → Tentar → Conteúdo mínimo → Praticar → Refletir → Próximo passo, com tentativa obrigatória antes do conteúdo | Must |
| AL-02 | Duração alvo de micro-lição entre 3 e 7 min, com estimativa visível antes de iniciar | Must |
| AL-03 | Cada conceito possui ao menos uma mecânica por nível de Bloom até o nível-alvo definido pelo mentor | Must |
| AL-04 | Flashcards com recuperação livre e agendamento por repetição espaçada (FSRS ou SM-2) | Must |
| AL-05 | "Explique com suas palavras" avaliado por rubrica de IA com feedback de 1 ponto forte + 1 lacuna | Must |
| AL-06 | Mini-problemas com variantes geradas e avaliação automática (testes de código, tolerância numérica, template) | Must |
| AL-07 | Mecânicas de arrastar (classificar/ordenar/comparar) com alvos ≥ 44 px e justificativa do item ambíguo | Should |
| AL-08 | "Julgue e justifique" com critérios explícitos e avaliação da justificativa por critério | Must |
| AL-09 | Artefato pequeno com rubrica entregue antes, perguntas socráticas durante e iteração sugerida; artefato salvo em portfólio | Must |
| AL-10 | Previsão de confiança obrigatória antes de cada item de prática (≤ 1 s de custo) | Must |
| AL-11 | Painel de calibração por conceito; conceitos com alta confiança e baixo acerto priorizados na revisão | Should |
| AL-12 | Diário de aprendizagem de 1 linha ao final de cada micro-lição, com voz aceita | Must |
| AL-13 | Meta diária escolhida pelo aluno (1/2/3 lições ou 1 minuto), com sugestão da IA | Must |
| AL-14 | Revisão semanal gerada por IA com progresso por nível de Bloom, 3 conceitos a revisar e citação do diário | Should |
| AL-15 | Progressão por nível de Bloom visível por conceito (medidor de 6 segmentos preenchido por evidência) | Must |
| AL-16 | Escolha do aluno de ordem entre conceitos desbloqueados e formato de conteúdo mínimo | Should |
| AL-17 | Dificuldade adaptativa mantendo acerto na primeira tentativa entre ~60% e ~80% | Should |
| AL-18 | Streak suave com congelamento semanal e sem mensagens negativas; marcos de maestria; sem ranking, pontos ou lojas | Must |
| AL-19 | "Ensine a IA" (protégé effect) disponível ao final de cada bloco de conceitos | Should |
| AL-20 | Peer review assíncrona opcional, anônima por padrão, moderada por IA quanto a tom e especificidade | Could |
| AL-21 | Cohort opcional com discussão semanal moderada e sintetizada por IA | Could |
| AL-22 | Lembrete diário único, no horário aprendido, formulado como pergunta de recuperação | Should |
| AL-23 | "Sessão de 1 minuto" sempre acessível na tela inicial e válida para streak e repetição espaçada | Must |
| AL-24 | Modo offline com pré-download das próximas 3 lições e flashcards vencidos; avaliação de IA em fila | Should |
| AL-25 | Sessão de retorno após 7+ dias de ausência, iniciando pelos conceitos mais fortes | Could |
| AL-26 | Mentor define nível-alvo de Bloom por bloco e aprova/edita objetivos e mecânicas geradas antes da publicação | Must |
| AL-27 | Desafio da semana autoral do mentor, distribuído e agrupado pela IA | Should |
| AL-28 | Painel de intervenção com alunos sinalizados (falhas repetidas, inatividade, calibração invertida) e mensagem em 1 toque | Must |
| AL-29 | Síntese semanal do mentor (≤ 2 min) inserida como micro-lição de revisão | Should |
| AL-30 | Tutor de IA segue escada de ajuda (dica → pista → exemplo → resposta) e não entrega a resposta antes de 2 tentativas | Must |
| AL-31 | Respostas do tutor limitadas a 120 palavras no celular, preferindo pergunta a explicação | Should |
| AL-32 | Mídia passiva limitada a 90 s sem interação; conteúdo longo do mentor é fragmentado automaticamente com pergunta prévia | Must |
| AL-33 | Máximo de 40% de itens de escolha fechada por trilha; itens em Analisar ou acima exigem justificativa | Should |
| AL-34 | Progresso avança apenas com evidência de maestria, nunca por consumo de conteúdo | Must |
