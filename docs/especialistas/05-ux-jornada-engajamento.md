# Contribuição UX de Jornada e Engajamento — ghdarulms

> Especialista: UX Research e Service Design (onboarding, engajamento, retenção, confiança em IA, ética de design).

## 1. Personas

| Persona | Objetivo | Dores | Contexto de uso |
|---|---|---|---|
| **Júlia, 21, universitária** (Engenharia, 3º período) | Passar na disciplina e entender de verdade, não decorar | Aulas densas, não sabe o que priorizar, vergonha de perguntar ao professor | Celular no ônibus, notebook à noite; sessões de 10-25 min; usa muito o chat |
| **Carlos, 38, profissional em requalificação** (analista migrando para dados) | Certificar-se em um tema novo mantendo o emprego atual | Só tem 15 min/dia, perde o fio quando pausa uma semana, culpa por "atrasar" | Celular, 100% mobile, manhã cedo ou fim da noite; interrompido com frequência |
| **Renata, 29, pós-graduanda** (mestrado, alta autonomia) | Ir direto ao que não domina, chegar em Avaliar/Criar | Conteúdo introdutório a irrita, quer controlar profundidade e pular etapas | Desktop, sessões longas (45-90 min), lê fontes originais, contesta respostas superficiais |
| **Prof. Marcos, 52, mentor** | Ver onde a turma trava e intervir com pouco esforço | Não tem tempo de ler logs, teme que a IA "ensine errado" em seu nome | Desktop, 20 min/semana no painel; reage a alertas |

## 2. Jornada do aluno em fases

Cada fase descreve: **sistema** (o que a plataforma faz), **IA** (o que o tutor faz), **mentor** (o que vê), **métrica**, **emoção** alvo.

### 2.1 Descoberta / convite
- **Sistema:** convite por link do mentor; página de entrada mostra em 3 linhas o que o curso cobre, o objetivo Bloom final, tempo estimado por conceito e a frase "seu tutor de IA é supervisionado pelo(a) [mentor]". Pré-visualização de uma micro-lição sem login.
- **IA:** nenhuma ação antes do consentimento.
- **Mentor:** taxa de aceite do convite.
- **Métrica:** % convite → conta criada (meta > 70%).
- **Emoção:** curiosidade, baixo compromisso.

### 2.2 Onboarding (≤ 5 min)
- **Sistema:** três telas: (a) diagnóstico inicial — 5 a 8 questões adaptativas cobrindo Memorizar→Analisar, com opção "não sei" sem penalidade; (b) definir meta — o aluno escolhe entre o objetivo do mentor ou um sub-objetivo próprio ("quero só Aplicar por enquanto"); (c) escolher ritmo e horário — dias/semana, minutos por sessão, janela preferida, formato inicial (texto/áudio/visual).
- **IA:** interpreta o diagnóstico e apresenta um "mapa de partida": "Você já domina X e Y no nível Compreender; começaremos por Z. Isso pode mudar conforme você avança."
- **Mentor:** distribuição do diagnóstico por conceito (heatmap agregado, sem expor aluno individual por padrão).
- **Métrica:** conclusão do onboarding < 5 min (P75); % que completa as 3 telas.
- **Emoção:** "me entenderam rápido"; sem sensação de prova.

### 2.3 Primeira semana (aha moment)
- **Sistema:** entrega a primeira micro-lição imediatamente após o onboarding (não amanhã). Aha moment definido: **o aluno resolve uma atividade de nível Aplicar sobre um conceito que marcou como "não sei" no diagnóstico**, dentro dos 3 primeiros dias.
- **IA:** tutor faz a primeira pergunta socrática curta; mostra "antes/depois" do diagnóstico ao fim da semana.
- **Mentor:** alerta "N alunos não iniciaram após 72h"; quem já atingiu o aha.
- **Métrica:** % alunos que atingem o aha em 3 dias (ativação); D7 retention.
- **Emoção:** competência precoce.

### 2.4 Rotina
- **Sistema:** tela inicial com uma única ação primária ("Continuar: [conceito] · 5 min"), progresso por conceito em níveis Bloom (não por %). Sessão sempre fechável com estado salvo.
- **IA:** ajusta ordem e profundidade; injeta revisão espaçada em 20% das sessões, explicando "estou trazendo isto de volta porque faz 6 dias".
- **Mentor:** painel semanal: conceitos com maior tempo médio, perguntas mais frequentes ao chat (anonimizadas).
- **Métrica:** sessões/semana vs. meta declarada pelo próprio aluno (não meta fixa).
- **Emoção:** previsibilidade, pouco atrito.

### 2.5 Travar num conceito
- **Sistema:** detecta trava por sinal composto: 2+ tentativas falhas na mesma atividade, tempo > 2× mediana, ou 3+ perguntas ao chat sobre o mesmo conceito. Nunca exibe "você errou 3 vezes"; exibe "este conceito está pedindo outro caminho".
- **IA:** troca de estratégia explícita: oferece três saídas nomeadas — "explicar de outro jeito", "voltar um nível (Compreender antes de Aplicar)", "ver material do mentor sobre isto". Não repete a mesma explicação.
- **Mentor:** alerta com contexto: conceito, quantos alunos, hipótese da IA ("o exemplo 2 parece ambíguo"). Um toque para gravar áudio/texto de esclarecimento que a IA passa a usar.
- **Métrica:** % de travas resolvidas em 48h sem mentor; % escaladas.
- **Emoção:** frustração reconhecida, não amplificada.

### 2.6 Recuperação (retorno após ausência)
- **Sistema:** ao voltar após ≥ 7 dias, tela "Bem-vindo de volta" com resumo de 3 linhas do que foi visto e um único botão "Retomar com uma revisão rápida de 3 min". Nada de contadores quebrados ou "você perdeu X dias".
- **IA:** recalibra com 2-3 questões e ajusta o mapa; oferece reduzir a meta semanal.
- **Mentor:** lista de retornos (para acolher, não cobrar).
- **Métrica:** taxa de reengajamento após ausência; % que completa a primeira sessão de retorno.
- **Emoção:** alívio, zero culpa.

### 2.7 Maestria / conclusão
- **Sistema:** conclusão por conceito quando o aluno demonstra o nível Bloom-alvo em atividade não assistida pelo chat. Certificado descreve competências ("Analisa X; Avalia Y"), não só "concluiu".
- **IA:** gera um retrato de evolução: diagnóstico inicial vs. final por conceito e nível; sugere próximo curso apenas se o mentor tiver cadastrado.
- **Mentor:** relatório de turma; pode validar ou ajustar o nível atingido antes de emitir certificado.
- **Métrica:** taxa de conclusão; % que atinge o nível Bloom definido pelo mentor.
- **Emoção:** orgulho fundamentado em evidência.

### 2.8 Retorno para revisão espaçada
- **Sistema:** após conclusão, convites opcionais em 7, 30 e 90 dias para uma revisão de 5 min; opt-in explícito na tela de conclusão.
- **IA:** seleciona os conceitos com maior risco de esquecimento (mais baixos na curva, mais críticos para o objetivo).
- **Mentor:** retenção de longo prazo agregada por conceito (insumo para melhorar o conteúdo).
- **Métrica:** acerto em revisão de 30 dias; opt-in de revisão.
- **Emoção:** "isso ficou comigo".

## 3. Confiança e transparência da IA

- **Página "O que a IA faz com seus dados"** em linguagem simples, acessível no onboarding e no perfil: quais sinais são usados (respostas, tempo, perguntas ao chat), para quê (adaptar ritmo e ordem), o que o mentor vê (progresso por conceito; conteúdo do chat só de forma anonimizada/agregada, salvo consentimento explícito), e como excluir dados.
- **Fonte em toda resposta do tutor:** rodapé com "Baseado em: [trecho do material do mentor]" ou "Material complementar: [link]" ou "Conhecimento geral da IA — não está no material do curso" (este último com selo visual diferente).
- **Marcação de origem:** três selos consistentes em todo o produto: **Mentor** (inserido pelo professor), **IA** (gerado), **IA · revisado pelo mentor**. Nunca misturar sem selo.
- **Reportar em 1 toque:** ícone em cada micro-lição, atividade e mensagem do chat; ao tocar, três motivos rápidos (Errado / Confuso / Inadequado) e campo opcional. Vai para a fila do mentor; o aluno recebe notificação de resolução.
- **"Por que estou vendo isto":** todo item recomendado (próxima lição, revisão, material) tem um link que abre uma frase objetiva: "Porque você errou 2 de 3 questões de Aplicar em Z" ou "Porque você escolheu ritmo intenso". A IA nunca justifica com "porque é melhor para você".
- **Limites declarados:** o chat informa quando não sabe ou quando o tema está fora do curso, e oferece "perguntar ao mentor".

## 4. Personalização com controle

- **Painel de preferências** (perfil e também atalho dentro da sessão): ritmo (leve/normal/intenso), formato preferido (texto/áudio/visual — aplicado quando houver variante disponível, com indicação clara quando não houver), profundidade (essencial/completo/aprofundado), tom do tutor (direto/encorajador).
- **A IA adapta, mas mostra o que mudou:** cartão de mudança ("Reduzi as atividades para 2 por sessão porque suas últimas 3 sessões ficaram abaixo de 6 min. Manter? / Desfazer"). Toda adaptação automática é reversível e registrada em "Histórico de ajustes".
- **Precedência:** preferência explícita do aluno > adaptação da IA > default do mentor. O mentor pode marcar conceitos como "não puláveis", e o sistema informa isso ao aluno.
- **Pular etapas:** Renata pode fazer um "teste de maestria" para pular um conceito; o resultado justifica a liberação.

## 5. Feedback e comunicação

- **Princípios de linguagem:** descrever o comportamento, não a pessoa; sempre indicar o próximo passo; sem exclamação em erro; sem infantilizar. Proibido: "Errado!", "Tente de novo", "Você falhou". Usado: "Ainda não — a resposta considera A, mas a questão pede B. Quer ver um exemplo ou tentar outro caminho?"
- **Erro:** mostra o raciocínio esperado após a segunda tentativa, nunca antes da primeira; erros contam como evidência de aprendizagem no painel, não como demérito.
- **Avanço de nível Bloom:** momento celebrado com uma tela curta e sóbria ("Você agora **Aplica** Z. Antes: Compreender."), com evidência (a atividade que comprovou) e opção de compartilhar. Sem confete excessivo, sem badges vazios.
- **Resumo semanal** (in-app e opcional por e-mail): 3 blocos — o que avançou (por conceito/nível), onde está pedindo atenção (máx. 1 item), sugestão para a próxima semana em uma frase. Comparação apenas com o próprio aluno, nunca com a turma.

## 6. Retenção ética

- **Lembretes:** só no horário escolhido pelo aluno, máximo 1/dia, com "adiar", "mudar horário" e "parar" a um toque no próprio lembrete. Após 3 lembretes ignorados, o sistema pergunta se quer pausar em vez de insistir.
- **Pausar sem culpa:** botão "Pausar por 1 semana / 1 mês / até eu voltar" visível no perfil; nenhum lembrete, nenhum e-mail durante a pausa; o mentor vê apenas "em pausa".
- **Sem streaks punitivos:** exibir consistência como "sessões nas últimas 4 semanas" (calendário), nunca contador que zera. Nenhum elemento se "quebra" por ausência.
- **Sem manipulação:** nenhuma escassez falsa, contagem regressiva artificial, "seus colegas estão na frente", ou notificação disfarçada de mensagem pessoal. Cancelar é tão fácil quanto assinar.
- **Meta é do aluno:** a meta semanal pode ser reduzida a qualquer momento sem confirmação em duas etapas.

## 7. Casos de borda

| Caso | Sinal | Resposta do sistema/IA | Escalada ao mentor |
|---|---|---|---|
| **Só usa o chat** | > 80% do tempo no chat, 0 atividades em 7 dias | Chat passa a encerrar respostas com uma micro-atividade opcional de 1 questão; explica que o progresso por nível exige evidência | Aparece como "aprendendo sem evidência"; sem punição |
| **Só faz quiz** | Pula lições, alta taxa de tentativas | Permite; quando erra, o quiz oferece o trecho exato da lição (30 s) em vez de forçar a lição inteira | Nenhuma, salvo conceitos "não puláveis" |
| **Ignora revisões** | 3+ revisões dispensadas | Reduz frequência e pergunta uma vez por que ("já sei", "sem tempo", "não vejo utilidade"); ajusta ou desliga por 30 dias | Nenhuma |
| **Frustrado** | Linguagem negativa no chat, abandono após erro | IA reconhece ("isto é difícil mesmo"), oferece pausa ou mudar caminho; nunca argumenta; propõe "falar com o mentor" | Sinal agregado de frustração por conceito |
| **Discorda da avaliação da IA** | Toca "Contestar" na atividade | Fluxo: (1) aluno escreve justificativa; (2) IA reavalia com a justificativa e responde em ≤ 1 min, podendo reverter; (3) se mantiver, "Enviar ao mentor" em 1 toque, com a resposta do aluno, o gabarito e o raciocínio da IA; (4) mentor decide, decisão registrada e visível ao aluno; nota provisória não bloqueia progresso | Sim, com SLA sugerido de 72h; contestações procedentes retroalimentam o conteúdo |

## 8. Pesquisa e validação

**Fases:**
1. **Entrevistas (pré-design):** 12 entrevistas — 4 por persona de aluno — sobre rotina de estudo, experiências com apps educacionais, atitude frente à IA; 4 mentores sobre carga de trabalho e confiança.
2. **Teste de usabilidade moderado (protótipo):** 8 participantes mobile, 4 desktop. Tarefas: completar onboarding (meta ≤ 5 min), concluir uma micro-lição, encontrar a fonte de uma resposta do chat, reportar um erro, contestar uma avaliação, pausar o curso. Sucesso ≥ 80% sem ajuda; SUS ≥ 75.
3. **Teste de compreensão da transparência:** após ler "por que estou vendo isto", o aluno explica com suas palavras (meta: ≥ 80% de explicações corretas).
4. **Piloto de 4 semanas com 2 turmas reais:** diário de bordo semanal (3 perguntas), análise de eventos.
5. **Testes A/B éticos** apenas em variantes de texto/estrutura, nunca em mecanismos de pressão.

**North Star sugerida:** **Conceitos que avançaram um nível Bloom por aluno ativo por semana** (mede aprendizagem real, não tempo de tela).

**Métricas de guarda (não podem piorar):**
- Taxa de pausa voluntária vs. abandono silencioso (queremos mais pausa explícita, menos sumiço).
- % de respostas do tutor com fonte exibida (≥ 95%).
- Tempo de resposta a reportes e contestações (≤ 72h).
- Reclamações de notificação / opt-out de lembretes (< 5%/mês).
- Diferença de progresso entre formatos preferidos (equidade de acesso).
- NPS do mentor sobre "confio no que a IA diz aos meus alunos".

## 9. Requisitos de UX de Jornada

| ID | Requisito | Prioridade |
|---|---|---|
| UXJ-01 | Onboarding completo (diagnóstico + meta + horário/formato) em ≤ 5 min, com "não sei" sem penalidade | Must |
| UXJ-02 | Primeira micro-lição disponível imediatamente após o onboarding | Must |
| UXJ-03 | Tela inicial com uma única ação primária "Continuar" com estimativa de tempo | Must |
| UXJ-04 | Progresso exibido por conceito e nível Bloom, nunca apenas por porcentagem | Must |
| UXJ-05 | Toda resposta do tutor exibe fonte (material do mentor, complementar ou "conhecimento geral") | Must |
| UXJ-06 | Selos de origem Mentor / IA / IA revisado em todo conteúdo | Must |
| UXJ-07 | Reportar erro em 1 toque em lições, atividades e mensagens do chat, com retorno ao aluno | Must |
| UXJ-08 | "Por que estou vendo isto" em toda recomendação, com justificativa baseada em dados do aluno | Must |
| UXJ-09 | Fluxo de contestação em 3 etapas (justificar → IA reavalia → mentor decide) sem bloquear progresso | Must |
| UXJ-10 | Detecção de trava e oferta de três caminhos alternativos nomeados | Must |
| UXJ-11 | Alerta ao mentor com contexto e resposta em 1 toque (áudio/texto) que a IA passa a usar | Must |
| UXJ-12 | Pausar sem culpa, com silêncio total de notificações durante a pausa | Must |
| UXJ-13 | Lembretes limitados a 1/dia no horário do aluno, com adiar/mudar/parar no próprio lembrete | Must |
| UXJ-14 | Nenhum streak que zera, nenhuma comparação com colegas, nenhuma escassez artificial | Must |
| UXJ-15 | Página de transparência de dados em linguagem simples, com exclusão de dados | Must |
| UXJ-16 | Linguagem de feedback orientada a crescimento conforme guia de redação (seção 5) | Must |
| UXJ-17 | Painel de preferências (ritmo, formato, profundidade, tom) com precedência sobre a adaptação da IA | Should |
| UXJ-18 | Cartão de adaptação com "Manter/Desfazer" e histórico de ajustes | Should |
| UXJ-19 | Tela de retorno após ausência com resumo e revisão de 3 min | Should |
| UXJ-20 | Resumo semanal em 3 blocos, comparando o aluno apenas consigo mesmo | Should |
| UXJ-21 | Celebração de avanço Bloom com evidência da atividade comprovadora | Should |
| UXJ-22 | Teste de maestria para pular conceitos (exceto "não puláveis" definidos pelo mentor) | Should |
| UXJ-23 | Chat encerra com micro-atividade opcional quando o aluno só conversa | Should |
| UXJ-24 | Revisão espaçada pós-conclusão em 7/30/90 dias, opt-in explícito | Should |
| UXJ-25 | Certificado descritivo por competências, validável pelo mentor | Should |
| UXJ-26 | Detecção de frustração no chat com oferta de pausa ou contato com mentor | Could |
| UXJ-27 | Variantes de formato (áudio/visual) geradas pela IA quando o mentor não fornecer | Could |
| UXJ-28 | Compartilhamento externo do avanço de nível Bloom | Could |
