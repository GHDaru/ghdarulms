# Contribuição Pedagógica — Especificação ghdarulms

> Especialista: Educação / Pedagogia / Design Instrucional (Taxonomia de Bloom revisada, avaliação formativa, mastery learning, ciência da aprendizagem).

## 1. Modelo pedagógico central

A espinha dorsal é a Taxonomia de Bloom revisada (Anderson & Krathwohl, 2001), usada como **eixo de progressão**, não como rótulo decorativo. A hierarquia de conteúdo é:

**Disciplina/Trilha → Unidade → Conceito → Objetivo de aprendizagem (por nível Bloom) → Micro-lição/Atividade**

- **Disciplina**: o mentor declara o **alcance-alvo** (nível máximo de Bloom que o aluno deve atingir ao concluir). Ex.: "Contabilidade Introdutória — alcance: Aplicar".
- **Unidade**: agrupamento temático de conceitos (gerado pela IA a partir do conteúdo, revisado pelo mentor).
- **Conceito**: menor unidade avaliável de forma independente (ex.: "Débito e crédito", "Regime de competência"). Cada conceito tem um **alcance próprio** que pode ser ≤ ao da disciplina — o mentor ajusta: um conceito instrumental pode parar em Compreender; um conceito central chega a Criar.
- **Objetivo de aprendizagem**: uma frase "verbo de ação + objeto + condição/critério", uma por nível de Bloom até o alcance do conceito. A IA gera os objetivos com verbos obrigatórios da tabela abaixo; o mentor aprova.
- **Atividade**: unidade de 3–7 minutos ligada a exatamente um objetivo (portanto a um par conceito × nível).

| Nível | Verbos de ação (obrigatórios nos objetivos) | Pergunta-guia |
|---|---|---|
| 1 Memorizar | Listar, Relembrar, Reconhecer, Identificar, Localizar, Descrever, Citar, Nomear, Definir | "O que é?" |
| 2 Compreender | Esquematizar, Relacionar, Explicar, Demonstrar, Parafrasear, Associar, Converter, Exemplificar, Resumir | "Por que / como funciona?" |
| 3 Aplicar | Utilizar, Implementar, Modificar, Experimentar, Calcular, Demonstrar, Classificar, Executar, Resolver (rotineiro) | "Como uso isso aqui?" |
| 4 Analisar | Resolver (não rotineiro), Categorizar, Diferenciar, Comparar, Explicar, Integrar, Investigar, Decompor, Atribuir | "Quais são as partes e como se relacionam?" |
| 5 Avaliar | Defender, Delimitar, Estimar, Selecionar, Justificar, Comparar, Explicar, Criticar, Julgar, Priorizar | "Qual é melhor e por quê?" |
| 6 Criar | Elaborar, Desenhar, Produzir, Prototipar, Traçar, Idear, Inventar, Planejar, Compor | "O que posso construir com isso?" |

**Regras de geração** (prescritivas):
1. Todo conceito gera objetivos consecutivos de 1 até o alcance — não se pula nível (o aluno pode pular via diagnóstico, o currículo não).
2. Objetivos precisam ser observáveis: proibidos verbos como "entender", "conhecer", "saber", "aprender".
3. Cada objetivo tem no mínimo 2 atividades (uma de ensino, uma de evidência) e 1 item de revisão espaçada.
4. Níveis 4–6 devem estar ancorados em um **contexto/caso** (dado real, cenário, dilema) — sem contexto não há análise nem avaliação.

**Alcance da disciplina**: o mentor escolhe em um seletor único (Memorizar…Criar). A IA propõe o alcance por conceito com justificativa ("conceito instrumental: sugerido Aplicar"); o mentor confirma ou altera em uma tela de grade Conceito × Nível.

## 2. Modelo de domínio pedagógico

**Grafo de conceitos**: DAG dirigido onde aresta A→B significa "A é pré-requisito de B". A IA infere arestas e as classifica como *forte* (bloqueante) ou *fraca* (recomendada). Só arestas fortes bloqueiam avanço. O mentor pode adicionar/remover/reclassificar. Ciclos são rejeitados na validação.

**Pré-requisito por nível**: a aresta carrega o nível mínimo exigido no pré-requisito. Ex.: "Juros compostos (Aplicar) requer Porcentagem (Aplicar)"; "Regime de competência (Compreender) requer Débito/crédito (Memorizar)". Padrão: para cursar o conceito B em nível *n*, o aluno precisa de A em nível *min(n, alcance(A))*.

**Estado de maestria** é uma tupla `(aluno, conceito, nível)` com valores:

| Estado | Definição operacional |
|---|---|
| `nao_iniciado` | sem exposição |
| `em_progresso` | fez ≥1 atividade, sem atingir critério |
| `dominado` | atingiu o critério de evidência do nível (seção abaixo) |
| `consolidado` | dominado + ≥3 revisões espaçadas bem-sucedidas com intervalo crescente (mín. 21 dias desde o `dominado`) |
| `decaido` | estava dominado/consolidado e falhou revisão espaçada → volta a `em_progresso` com prioridade de revisão |

Além do estado discreto, o sistema mantém uma **probabilidade de maestria** (modelo tipo Bayesian Knowledge Tracing, p_conhece ∈ [0,1]) atualizada a cada evidência, usada para ordenar o que apresentar. O estado discreto é o que o aluno vê; a probabilidade é interna.

**Critérios de evidência ("o que prova o nível")**:

| Nível | Evidência mínima aceita | Critério de "dominou" |
|---|---|---|
| Memorizar | Reconhecimento (múltipla escolha, associação) **e** evocação livre (resposta curta sem opções) | ≥ 85% em ≥ 6 itens, sendo ≥ 2 de evocação livre, em ≥ 2 sessões distintas |
| Compreender | Explicação com as próprias palavras, exemplo próprio, classificação de casos novos, esquema/mapa | Rubrica IA ≥ 3/4 em 2 artefatos diferentes; ≥ 80% em classificação de casos não vistos |
| Aplicar | Execução de procedimento em problema **novo** (parâmetros variados) com resultado verificável | ≥ 80% em ≥ 5 problemas gerados, com ≥ 1 problema em contexto diferente do exemplo |
| Analisar | Decomposição de caso, diagnóstico de erro, comparação estruturada, identificação de relação causa-efeito | Rubrica IA ≥ 3/4 em 2 casos; ≥ 1 caso com "distrator estrutural" (informação irrelevante) tratado corretamente |
| Avaliar | Julgamento com critérios explícitos + justificativa; escolha entre alternativas defensáveis; estimativa com margem | Rubrica IA ≥ 3/4 **e** amostragem de 1 artefato pelo mentor (ou aprovação automática se mentor configurou "confiar na IA" para o conceito) |
| Criar | Artefato original (plano, protótipo, texto, código, projeto) que integra ≥ 2 conceitos | Rubrica IA ≥ 3/4 como pré-triagem; **aprovação do mentor obrigatória** |

## 3. Tipos de atividade por nível (micro-lição mobile, 3–7 min)

Cada micro-lição segue a estrutura fixa **Gancho (30s) → Núcleo (2–4 min) → Prática (1–2 min) → Feedback (30s)**. Texto ≤ 250 palavras por tela, vídeo ≤ 3 min, uma ideia por tela.

| Nível | Tipos de atividade | Exemplo concreto | Correção |
|---|---|---|---|
| Memorizar | Flashcard com evocação; múltipla escolha com distratores plausíveis; arrastar-para-associar; preencher lacuna | "Toque na definição correta de *passivo circulante*"; "Digite o nome do princípio que diz que receitas se registram quando ganhas" | Auto |
| Compreender | "Explique para um colega" (áudio/texto 60s); escolher o melhor exemplo/não-exemplo; ordenar etapas; completar mapa conceitual; parafrasear em 1 frase | "Em uma frase, explique por que uma venda a prazo é receita hoje e não quando o dinheiro entra" | Auto (ordenar, classificar) / **IA com rubrica** (explicação, paráfrase) |
| Aplicar | Problema parametrizado com resposta numérica/estruturada; simulador de passos (stepper); "conserte o cálculo"; mini-exercício de código com testes | "Lance no razonete a compra de R$ 4.200 de mercadorias, metade à vista" (valores gerados aleatoriamente) | Auto (com verificador) / IA para passos intermediários |
| Analisar | Estudo de caso de 1 tela: "o que está errado aqui?"; comparação em tabela (2 casos, 3 critérios); classificar causas; identificar suposição oculta | "Este balanço não fecha. Toque na linha com erro e diga qual princípio foi violado" | Auto (localizar erro) / **IA com rubrica** (justificativa) |
| Avaliar | Escolha entre 2–3 alternativas defensáveis + justificativa (120 palavras); estimativa de Fermi com margem; crítica de uma resposta de "outro aluno" (gerada); ranking com critérios | "Duas empresas com o mesmo lucro. Qual está mais saudável? Defenda com 2 indicadores" | **IA com rubrica** + amostragem do mentor |
| Criar | Mini-projeto fatiado em micro-entregas (cada uma 5–7 min); esboço de plano; protótipo em texto/diagrama/código; "e se" (idear variações) | "Proponha um plano de contas para uma padaria: 5 contas de ativo, 5 de passivo, justifique 2 escolhas" — entregue em 3 micro-lições | IA pré-triagem + **mentor obrigatório** |

Regras: níveis 1–3 devem ter ≥ 70% de atividades autocorrigíveis (garante feedback instantâneo em mobile); níveis 4–6 podem ser majoritariamente avaliadas por IA, mas com **rubrica exibida ao aluno antes** de responder. Entrada por voz é primeira classe para "explique com suas palavras".

## 4. Avaliação

**Rubricas por nível**: escala 0–4 em 3 critérios fixos por nível, gerada pela IA por objetivo e editável pelo mentor.
- Compreender: precisão conceitual · uso de palavras próprias (não cópia) · exemplo/relação correta.
- Analisar: identificação das partes · relações/causas corretas · tratamento de informação irrelevante.
- Avaliar: critérios explícitos · justificativa ancorada em evidência · reconhecimento de trade-offs.
- Criar: atende requisitos · integra conceitos · originalidade/viabilidade.

A IA devolve nota por critério, citação do trecho do aluno que embasa a nota, e uma sugestão de melhoria. Nota final = média; ≥ 3,0 conta como evidência.

**Avaliação formativa contínua**: toda atividade é avaliação. Não existe "prova" separada dos níveis 1–4; a prova é a soma das evidências. Feedback imediato obrigatório em atividades autocorrigíveis (< 1s) e ≤ 20s nas avaliadas por IA. Feedback segue o padrão **específico → por quê → próximo passo**, nunca só "correto/incorreto".

**Diagnóstico inicial (placement)**: ao entrar na disciplina, teste adaptativo de 8–15 min percorrendo o grafo dos conceitos-raiz para frente. Para cada conceito, 2–3 itens no nível Aplicar (ou no alcance, se menor); acerto → marca níveis inferiores como `dominado_por_placement` (provisório, validado nas primeiras revisões); erro → desce um nível. Aluno pode pular o placement (começa do zero). Placement nunca marca Avaliar/Criar.

**Recuperação espaçada (SRS) integrada**: cada par (conceito, nível) dominado gera itens de revisão. Algoritmo tipo FSRS/SM-2 com intervalos iniciais 1d → 3d → 7d → 16d → 35d; falha reinicia a 1d e derruba o estado para `decaido`. A revisão é **intercalada** (itens de conceitos diferentes na mesma sessão, nunca em bloco) e **variada** (Memorizar revisa por evocação, Aplicar revisa com problema novo, Compreender revisa pedindo explicação em contexto diferente). A sessão diária de revisão abre o app (≤ 5 min, "Revisão do dia") antes de novo conteúdo; limite de 20 itens/dia.

**Mastery gates e política de avanço**:
1. Avançar dentro do conceito (nível n → n+1) exige `dominado` em n.
2. Iniciar conceito B exige pré-requisitos fortes no nível definido na aresta.
3. Concluir a Unidade exige todos os conceitos `dominado` no alcance.
4. Concluir a Disciplina exige todos os conceitos `consolidado` no alcance (ou `dominado` + revisões em dia, se o mentor relaxar).
5. **Gate flexível**: o mentor pode marcar um conceito como "soft gate" (permite avançar com aviso). Padrão é hard gate para arestas fortes.
6. Sem punição por tentativas: falha gera nova atividade equivalente (variação), não repetição do mesmo item, com scaffolding aumentado (dica → exemplo resolvido → passo a passo).

## 5. Papel pedagógico do tutor IA

**Postura padrão: socrático com escalada expositiva.** O tutor começa perguntando ("O que você já sabe sobre isso?", "O que aconteceria se…?") e só explica quando (a) o aluno pede explicitamente após ≥ 1 tentativa, (b) o aluno está em nível Memorizar/Compreender e não há base para descobrir, ou (c) sinais de frustração (3 erros seguidos, mensagens curtas/irritadas, tempo ocioso). Exposição é sempre curta (≤ 120 palavras) seguida de uma pergunta de checagem.

**Scaffolding e fading**: escala de 4 degraus por atividade — 1) pergunta reflexiva; 2) dica conceitual; 3) exemplo análogo resolvido; 4) passo a passo do próprio problema. O tutor sobe um degrau por tentativa fracassada. **Fading**: o degrau inicial permitido cai conforme p_conhece sobe (p > 0,7 → máximo degrau 2; p > 0,85 → só degrau 1). Em revisão espaçada, degraus 3–4 são bloqueados (o item é marcado como falho e reagendado).

**Quando dar a resposta**: nunca em revisão espaçada ou em item de evidência ativo; após a evidência ser registrada, a resposta e a resolução completa são liberadas. Em atividades de prática (não-evidência), dá após o degrau 4. Para Avaliar/Criar, nunca produz o artefato pelo aluno — critica, questiona e aponta lacunas.

**Detecção de misconceptions**: cada conceito carrega uma lista de misconceptions conhecidas (gerada pela IA, complementada pelo mentor), com padrões de resposta que as denunciam. Distratores de múltipla escolha são mapeados a misconceptions. Ao detectar, o tutor **não corrige direto**: apresenta um contraexemplo ou caso que gera conflito cognitivo e pede ao aluno reconciliar. A misconception é registrada no perfil do aluno e alimenta revisões futuras.

**Metacognição**: a cada micro-lição concluída, o aluno responde "Quão confiante você está? (1–4)" antes de ver o resultado; o sistema mede **calibração** (confiança × acerto). Após 3 atividades de um conceito, o tutor pede "explique em uma frase o que você aprendeu" e "o que ainda está confuso?". Superconfiança com erro dispara mais evocação; subconfiança com acerto dispara feedback de reforço.

**O que a IA nunca deve fazer**:
- Marcar `dominado` em Avaliar/Criar sem mentor (ou sem delegação explícita do mentor por conceito).
- Dar a resposta em item de evidência ou de revisão espaçada.
- Afirmar conteúdo que não está no material do mentor ou em fonte curada sem sinalizar "isto não está no material da disciplina" (grounding obrigatório com citação da fonte).
- Alterar objetivos, rubricas ou grafo sem aprovação do mentor.
- Emitir julgamento sobre a pessoa ("você é fraco em…"); só sobre o desempenho e a estratégia.
- Fazer a tarefa do aluno (escrever o texto, o código, o plano) em atividades de Criar.

## 6. Papel do mentor

**Fluxo de trabalho**:
1. **Ingestão**: sobe materiais e define objetivo geral + alcance da disciplina.
2. **Revisão do mapa**: recebe o grafo de conceitos, alcances propostos, objetivos por nível e misconceptions. Aprova, edita, funde/divide conceitos, reclassifica pré-requisitos. Nada é publicado sem aprovação explícita (estado `rascunho` → `revisado` → `publicado`).
3. **Revisão de atividades**: amostra obrigatória de ≥ 20% das atividades por conceito (interface de "swipe": aprovar / editar / regenerar com instrução). O mentor pode marcar conceitos como "revisão completa obrigatória" (Avaliar/Criar por padrão).
4. **Curadoria complementar**: aprova ou rejeita cada material externo sugerido pela IA.
5. **Operação da turma**: painel com heatmap Aluno × Conceito × Nível, fila de artefatos aguardando avaliação (Avaliar/Criar), alertas automáticos.
6. **Intervenção**: mensagem direta ao aluno; ajuste de gate individual; sessão síncrona sugerida quando ≥ 30% da turma trava no mesmo conceito; "aula corretiva" gerada pela IA a partir da misconception dominante, aprovada pelo mentor.

**Human-in-the-loop**: o mentor é a autoridade sobre currículo e sobre evidências de alto nível; a IA é autoridade de execução com escalonamento. Toda decisão da IA que afeta o percurso do aluno (mudança de p_conhece por avaliação com rubrica, misconception registrada) é auditável e reversível pelo mentor. O mentor vê, para cada avaliação por IA, o trecho citado e pode discordar — discordâncias alimentam o ajuste das rubricas.

## 7. Riscos pedagógicos e mitigação

| Risco | Mitigação prescritiva |
|---|---|
| **Dependência da IA** (aluno pede resposta sempre) | Fading obrigatório; degraus de ajuda registrados; revisão espaçada sem ajuda; métrica "taxa de ajuda por evidência" visível ao aluno e ao mentor |
| **Ilusão de competência** (reler ≠ saber) | Evocação livre obrigatória em Memorizar; revisão espaçada com variação; calibração de confiança; conteúdo expositivo nunca marca maestria sozinho |
| **Alucinação do tutor** | Grounding no material do mentor com citação; resposta fora do material sinalizada; botão "reportar erro" que abre item na fila do mentor; conteúdo gerado passa por revisão antes de publicar |
| **Atalhos e cola** (colar resposta de outra IA) | Itens de evidência gerados com variação de parâmetros e contexto; "explique sua resposta" oral/escrito após artefato; detecção de mudança de estilo; avaliação de Criar exige processo (micro-entregas) e não só produto; entrevista de defesa opcional acionada pelo mentor |
| **Motivação e abandono** | Sessões de 3–7 min com fechamento claro; progresso visível por conceito (não por "pontos"); revisão do dia curta; meta semanal negociada com o aluno; alertas de inatividade ao mentor em 5 dias |
| **Gate frustrante** | Variação de atividade a cada tentativa; scaffolding progressivo; soft gate configurável; mentor pode liberar manualmente |
| **Currículo gerado de má qualidade** | Nada publicado sem aprovação; amostragem mínima; verbos de Bloom validados automaticamente; objetivos sem contexto em 4–6 rejeitados pelo validador |
| **Viés do avaliador IA** | Rubrica visível; citação obrigatória; amostragem do mentor; comparação de concordância mentor × IA por conceito, recalibração quando < 80% |

## 8. Métricas de aprendizagem (não vaidade)

Registradas por aluno, conceito e turma:
- **Retenção em revisão espaçada**: % de acertos em itens com intervalo ≥ 7 dias e ≥ 30 dias (métrica principal de aprendizagem real).
- **Progressão Bloom por conceito**: nível máximo dominado × alcance; distribuição na turma.
- **Tempo até maestria**: minutos ativos e dias-calendário entre `nao_iniciado` e `dominado` por nível.
- **Taxa de decaimento**: % de pares (conceito, nível) que voltaram a `decaido`.
- **Misconceptions recorrentes**: frequência por misconception na turma; taxa de resolução após intervenção.
- **Taxa de ajuda por evidência**: degrau médio de scaffolding usado; tendência ao longo do tempo (deve cair).
- **Calibração**: correlação confiança × acerto; % de superconfiança.
- **Transferência**: acerto em problemas de contexto diferente do ensinado vs. mesmo contexto.
- **Concordância mentor × IA** nas rubricas.
- **Gargalos do grafo**: conceitos com maior tempo até maestria e maior taxa de tentativas; pré-requisitos com maior correlação com falha posterior.
- **Regularidade**: dias com revisão completa / dias esperados (proxy de hábito, não de vaidade).

Métricas de vaidade explicitamente excluídas como indicadores de aprendizagem: minutos no app, vídeos assistidos, telas vistas, streaks sem revisão.

## 9. Requisitos pedagógicos

| ID | Requisito | Prioridade |
|---|---|---|
| PED-01 | Hierarquia Disciplina → Unidade → Conceito → Objetivo (por nível Bloom) → Atividade, com alcance definido pelo mentor na disciplina e por conceito | Must |
| PED-02 | Objetivos gerados com verbos de ação da tabela Bloom; validador rejeita verbos não observáveis | Must |
| PED-03 | Grafo de conceitos (DAG) com pré-requisitos fortes/fracos e nível mínimo por aresta; validação de ciclos | Must |
| PED-04 | Estado de maestria por (aluno, conceito, nível) com os 5 estados definidos e probabilidade interna (BKT ou similar) | Must |
| PED-05 | Critérios de evidência por nível conforme seção 2, configuráveis pelo mentor dentro de limites | Must |
| PED-06 | Micro-lições de 3–7 min com estrutura Gancho/Núcleo/Prática/Feedback, mobile-first | Must |
| PED-07 | ≥ 70% de atividades autocorrigíveis nos níveis 1–3; feedback imediato específico → porquê → próximo passo | Must |
| PED-08 | Avaliação por rubrica IA (0–4, 3 critérios) com citação do trecho do aluno e rubrica visível antes da resposta | Must |
| PED-09 | Aprovação obrigatória do mentor para maestria em Criar; amostragem em Avaliar (delegável por conceito) | Must |
| PED-10 | Revisão espaçada integrada (FSRS/SM-2), intercalada e variada por nível, sessão diária ≤ 5 min | Must |
| PED-11 | Mastery gates: nível→nível, pré-requisitos fortes, unidade e disciplina; soft gate configurável | Must |
| PED-12 | Tutor socrático por padrão com escala de scaffolding de 4 degraus e fading por p_conhece | Must |
| PED-13 | Tutor não fornece resposta em itens de evidência/revisão; não produz artefatos de Criar | Must |
| PED-14 | Grounding do tutor no material do mentor com citação; sinalização quando sai do material | Must |
| PED-15 | Nada gerado pela IA é publicado sem aprovação do mentor (rascunho → revisado → publicado) | Must |
| PED-16 | Diagnóstico inicial adaptativo (placement) com marcação provisória validada por revisões | Should |
| PED-17 | Registro de misconceptions por conceito, mapeadas a distratores, com resposta por conflito cognitivo | Should |
| PED-18 | Autoavaliação de confiança pré-resultado e métrica de calibração | Should |
| PED-19 | Painel do mentor: heatmap Aluno × Conceito × Nível, fila de artefatos, alertas de gargalo e inatividade | Should |
| PED-20 | Métricas da seção 8 registradas e exportáveis; métricas de vaidade não apresentadas como aprendizagem | Should |
| PED-21 | Variação automática de itens de evidência (parâmetros e contexto) a cada tentativa | Should |
| PED-22 | Concordância mentor × IA monitorada por conceito; recalibração de rubrica quando < 80% | Should |
| PED-23 | Entrada por voz para atividades "explique com suas palavras" | Should |
| PED-24 | Aula corretiva gerada a partir da misconception dominante da turma, sob aprovação do mentor | Could |
| PED-25 | Entrevista de defesa (oral) acionável pelo mentor para artefatos de Avaliar/Criar | Could |
| PED-26 | Meta semanal negociada com o aluno e ajuste do ritmo de novos conceitos vs. revisão | Could |
