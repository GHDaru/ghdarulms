# UX do Mentor — Autoria e Acompanhamento (ghdarulms)

> Especialista: UX de ferramentas de autoria e dashboards para professores (human-in-the-loop, learning analytics, desktop-first responsivo).

## 1. Jobs-to-be-done e princípios

**JTBDs do mentor**

| # | Quando… | Quero… | Para… |
|---|---|---|---|
| J1 | tenho material bruto (slides, PDFs, vídeos) | transformá-lo em trilha estruturada sem reescrever tudo | publicar em menos de 1 hora |
| J2 | a IA gerou conteúdo | revisar rápido, corrigir o que está errado e confiar no resto | não perder autoridade pedagógica |
| J3 | a turma está em andamento | ver quem travou, onde e por quê | intervir antes da evasão |
| J4 | um aluno contesta ou o tutor não sabe responder | decidir em segundos, do celular | manter o fluxo do aluno |
| J5 | o conteúdo não funciona | descobrir qual item causa o problema | revisar só aquele item |

**Princípios de design**

1. **O mentor traz conteúdo e intenção; a IA faz o braçal; o mentor decide.** Nenhum item gerado chega ao aluno sem passar por um estado "Aprovado" (explícito ou por aprovação em lote).
2. **Revisão > criação.** Toda tela de IA é uma tela de revisão: aceitar é um clique, editar é inline, rejeitar exige motivo curto (alimenta a regeneração).
3. **Tempo de setup como métrica de produto.** Meta: material bruto → trilha publicável em < 60 min. O wizard mostra um cronômetro discreto e "faltam N itens para publicar".
4. **Padrões seguros.** Tudo tem default razoável (tom do tutor, política de respostas, limites). O mentor só altera se quiser.
5. **Rastreável e reversível.** Todo item gerado aponta para o trecho-fonte; toda edição tem versão anterior.

## 2. Wizard de criação de disciplina

Layout comum: barra de progresso à esquerda (7 passos, estado salvo automaticamente, retorno a qualquer passo), área central de trabalho, painel direito contextual ("Fonte", "Confiança", "Comentários"). Rodapé fixo: **Voltar · Salvar rascunho · Continuar**.

### (a) Criar disciplina e objetivo geral

- Campos: nome, área, público-alvo (chips: graduação, técnico, corporativo, livre), carga estimada, idioma.
- **Seletor de nível-alvo de Bloom**: seis colunas horizontais em escada (Memorizar → Criar). Clicar numa coluna marca-a como alvo e destaca tudo à esquerda como "níveis de base". Sob cada coluna, a grade de verbos:

| Memorizar | Compreender | Aplicar | Analisar | Avaliar | Criar |
|---|---|---|---|---|---|
| Listar, Relembrar, Reconhecer, Identificar, Localizar, Descrever, Citar | Esquematizar, Relacionar, Explicar, Demonstrar, Parafrasear, Associar, Converter | Utilizar, Implementar, Modificar, Experimentar, Calcular, Demonstrar, Classificar | Resolver, Categorizar, Diferenciar, Comparar, Explicar, Integrar, Investigar | Defender, Delimitar, Estimar, Selecionar, Justificar, Comparar, Explicar | Elaborar, Desenhar, Produzir, Prototipar, Traçar, Idear, Inventar |

- **Construtor de objetivo**: frase-modelo "Ao final, o aluno será capaz de **[verbo]** **[objeto]** **[condição/critério]**". Clicar num verbo da grade preenche o primeiro slot; o mentor digita o resto. A IA sugere 3 objetivos alternativos a partir do nome + área (chips clicáveis). Validação: alerta amarelo se o verbo pertencer a nível diferente do alvo escolhido.

### (b) Upload de material

- Zona de arrastar-e-soltar ocupando o centro; abas laterais: **Arquivo · URL · YouTube · Google Drive · Nota rápida**.
- Lista de materiais com colunas: nome, tipo, páginas/duração, status de processamento (fila → extraindo → indexado → erro), botão "usar apenas páginas X–Y" para PDFs e "usar trecho mm:ss–mm:ss" para vídeos.
- Marcação por material: **Principal / Complementar / Referência (não gerar lições)**.
- O mentor pode seguir para (c) com processamento em andamento; itens não indexados mostram aviso.

### (c) Grafo de conceitos + objetivos (revisão)

- **Vista dupla alternável**: *Grafo* (nós = conceitos, setas = pré-requisitos, cor = nível de Bloom, borda tracejada = baixa confiança) e *Lista/Outline* (árvore indentada, arrastar para reordenar, indentação = dependência).
- Cada conceito abre painel direito com: título editável, resumo, objetivos por nível (um por nível até o alvo), **trecho-fonte** (citação com link "abrir na página 12"), confiança (0–100 + motivo).
- Ações por nó/objetivo: **Aceitar · Editar · Mesclar (arrastar um nó sobre outro) · Dividir · Excluir · Marcar como pré-requisito de…**
- Barra superior: "12 conceitos · 9 aceitos · 3 pendentes" e botão **Aceitar todos com confiança ≥ 80**.
- Detecção automática: ciclo de pré-requisitos, conceito órfão, conceito sem fonte → chips de alerta clicáveis.

### (d) Micro-lições e atividades (revisão em lote)

- Layout três colunas: **esquerda** = lista de conceitos com contador (lições/atividades geradas, aprovadas); **centro** = editor da lição (ver §3); **direita** = **pré-visualização em moldura de celular** (375 px), rolável, exatamente como o aluno verá, com tempo estimado de leitura (meta 3–7 min; alerta se > 7).
- Atividades listadas abaixo da lição com tipo (quiz, resposta curta, caso, projeto), nível de Bloom, gabarito/rubrica gerada e "requer avaliação humana" (automático para Avaliar/Criar, alterável).
- Navegação por teclado: `J/K` próximo/anterior, `A` aprovar, `R` rejeitar com motivo, `E` editar. Barra de progresso "34/58 itens revisados".
- Filtros: pendentes, baixa confiança, longos demais, sem atividade.

### (e) Material complementar sugerido

- Cards em grade: título, tipo (artigo, vídeo, dataset), fonte (domínio), **licença** (badge: CC-BY, domínio público, todos os direitos reservados, desconhecida), duração/tamanho, conceito(s) vinculados, resumo em 2 linhas gerado pela IA, "por que sugeri".
- Ações: **Aprovar · Rejeitar · Substituir por link meu**. Itens com licença desconhecida ficam com aprovação em amarelo e texto "verifique antes de publicar".

### (f) Configurar tutor

- Formulário em cards:
  - **Tom**: seletor (formal, coloquial, socrático) + campo de exemplo de resposta que atualiza um chat de demonstração ao lado.
  - **Política de respostas**: escala de 4 posições — *Nunca dá a resposta* / *Dá após 2 tentativas* / *Dá com explicação* / *Livre*; configurável por nível de Bloom (ex.: nunca em Avaliar/Criar).
  - **Limites**: temas fora de escopo (lista), palavras/ações proibidas, escalar ao mentor quando (confiança baixa, aluno frustrado, pedido de nota).
  - **Orçamento**: mensagens/aluno/dia e teto de custo mensal com alerta a 80 %.
- Botão **Testar tutor** abre chat sandbox usando o conteúdo já aprovado.

### (g) Publicar / convidar

- Checklist de publicação (bloqueante): objetivo definido, ≥ 1 conceito aprovado, todo conceito com ≥ 1 lição aprovada, tutor configurado, materiais com licença resolvida.
- Convite: link, código, CSV de e-mails, integração LTI (Could). Escolha: **publicar tudo** ou **liberar conceito a conceito** (padrão: pré-requisitos gateados).
- Resumo final com tempo total de setup e o botão **Publicar**.

## 3. Editor de conteúdo

- **Edição inline** em blocos (parágrafo, exemplo, imagem, vídeo embed, código, callout, atividade). Clicar edita no lugar; barra flutuante mínima.
- **Regenerar com instrução**: botão em cada bloco e no item inteiro. Menu com atalhos ("mais curto", "mais simples", "adicione exemplo do setor…", "troque o tom", "traduza") + campo livre. O resultado aparece como **proposta** ao lado, nunca substitui sem aceite.
- **Histórico de versões**: painel lateral com linha do tempo (autor: IA/mentor/co-mentor, hora, motivo). Restaurar cria nova versão, não apaga.
- **Diff**: modo lado a lado ou inline com adições/remoções marcadas; filtro "só o que a IA mudou desde minha última edição".
- **Blocos reutilizáveis**: qualquer bloco pode virar "componente" (ex.: aviso de segurança, glossário). Inserção por `/` no editor; alteração no componente propaga com aviso "usado em 6 lições — atualizar todas?".
- **Marcadores pedagógicos**: cada bloco carrega nível de Bloom e conceito; alerta se a lição não cobre o objetivo do conceito.

## 4. Dashboard de acompanhamento

Tela padrão ao abrir uma disciplina publicada. Cabeçalho com KPIs: alunos ativos (7d), progresso médio, taxa de conclusão, custo de IA no mês.

1. **Mapa de calor turma × conceito × Bloom**: linhas = conceitos (na ordem da trilha), colunas = níveis de Bloom até o alvo; célula = % de alunos que dominaram (verde → vermelho), com número. Clique na célula → lista de alunos naquele estado. Toggle para trocar colunas por **alunos** (visão individual, com rolagem horizontal).
2. **Alunos em risco / travados**: tabela ordenável — nome, conceito atual, dias parado, tentativas falhas, última interação, sinal (sem acesso 5 dias, 3 falhas seguidas, tutor escalou). Ação rápida na linha: mensagem, ajustar trilha, ver conversa.
3. **Misconceptions recorrentes**: cards gerados pela IA ("31 % confundem X com Y no conceito Z"), com evidências (trechos anonimizados de respostas), e botões **Criar lição de correção** (pré-preenche o editor) · **Ignorar** · **Marcar resolvido**.
4. **Perguntas mais frequentes ao tutor**: lista agrupada semanticamente, contagem, conceito, qualidade da resposta do tutor (avaliada pelo aluno). Ação: **Adicionar ao conteúdo** (vira bloco FAQ).
5. **Conteúdo com pior desempenho**: ranking de lições/atividades por taxa de erro, abandono no meio, tempo real vs. estimado, avaliação dos alunos. Badge "revisar" → abre editor.
6. **Fila de avaliação humana**: itens de Avaliar/Criar pendentes — aluno, atividade, entregue há X, pré-avaliação da IA com rubrica preenchida (sugestão), campo de nota/feedback, **Aceitar sugestão** ou editar. Meta visível: tempo médio de resposta.
7. **Contestações**: aluno discorda de correção → card com resposta do aluno, gabarito, argumento, avaliação da IA sobre a contestação. Ações: **Manter · Aceitar (recalcula nota) · Corrigir gabarito** (propaga para todos).

## 5. Intervenção

| Ação | Onde | Comportamento |
|---|---|---|
| Mensagem para aluno/grupo | linha do aluno, seleção múltipla, célula do heatmap | modal com destinatários, modelo sugerido pela IA baseado no contexto ("vi que você travou em…"), envio por chat do tutor ou e-mail |
| Ajustar trilha do aluno | perfil do aluno | vista da trilha individual; arrastar conceitos, pular, adicionar lição de reforço, mudar nível-alvo para aquele aluno |
| Liberar/travar conceito | grafo ou lista | para turma ou grupo; agenda por data; aviso do impacto ("bloqueia 4 alunos em andamento") |
| Desafio da semana | botão no dashboard | IA propõe 3 desafios no nível-alvo a partir dos conceitos da semana; mentor escolhe/edita, define prazo e destinatários |
| Responder escalonamento do tutor | inbox + push | mostra a conversa, motivo da escalada, resposta sugerida; opções **Enviar como está · Editar · Devolver ao tutor com instrução**; opção "guardar como regra" |

## 6. Confiança na IA

- **Indicador de confiança** em todo item gerado: barra 0–100 + motivo textual curto ("fonte única", "conceito inferido, não citado no material", "atividade sem gabarito verificável"). Itens < 60 nunca entram em "Aceitar todos".
- **"O que mudou"**: ao reabrir uma disciplina, painel com alterações desde a última visita (regenerações, correções propostas pela IA a partir do desempenho, novos escalonamentos), cada uma com aceitar/ignorar.
- **Rastreabilidade**: qualquer objetivo, lição ou questão tem link "Fonte" → abre o material original no trecho destacado; itens sem fonte exibem "gerado sem material" em laranja.
- **Custos/limites**: página de consumo — tokens/custo por atividade (geração, tutor, avaliação), projeção mensal, tetos configuráveis, pausa automática com aviso. Antes de regenerações em lote, mostrar estimativa ("~R$ 4,20").
- **Sandbox**: "Testar como aluno" em qualquer ponto, com cenário (aluno iniciante/avançado).

## 7. Colaboração

- **Papéis**: *Dono* (tudo, inclusive publicar, custos, excluir); *Editor* (autoria, aprovação, intervenção); *Monitor* (dashboard, fila de avaliação, mensagens, sem edição de conteúdo).
- **Comentários** em qualquer item (conceito, bloco, atividade), com menções, resolver, e filtro "abertos para mim". Comentários aparecem no painel direito e como badge no outline.
- **Presença e bloqueio leve**: avatar de quem está editando; edição simultânea do mesmo bloco mostra aviso e faz merge por versão.
- **Atribuição** de itens da fila de avaliação a um co-mentor; divisão automática opcional.
- **Log de atividade** da disciplina (quem aprovou/publicou/alterou política do tutor).

## 8. Responsividade

| Celular (≥ 360 px) | Desktop (≥ 1024 px) |
|---|---|
| Inbox de alertas e escalonamentos com resposta rápida | Wizard completo, grafo, editor com preview lado a lado |
| Aprovar/rejeitar itens em cards deslizáveis (swipe) | Revisão em lote com atalhos de teclado |
| Fila de avaliação humana (leitura + nota + feedback curto, ditado por voz) | Diff de versões, blocos reutilizáveis |
| Heatmap simplificado (lista de conceitos com barra de domínio) | Heatmap completo, cruzamentos e drill-down |
| Mensagem para aluno, liberar/travar conceito | Configuração de tutor, custos, papéis |
| Push: aluno em risco, escalonamento, contestação, custo a 80 % | Tudo |

Regra: no celular, autoria pesada mostra "continue no desktop" com link que reabre o mesmo item; o estado é sincronizado.

## 9. Requisitos de UX do Mentor

| ID | Requisito | Prioridade |
|---|---|---|
| UXA-01 | Wizard de 7 passos com salvamento automático e retorno a qualquer etapa | Must |
| UXA-02 | Seletor de nível-alvo de Bloom com grade de verbos e construtor de objetivo | Must |
| UXA-03 | Upload multi-fonte (arquivo, URL, YouTube, Drive) com status de processamento e recorte de páginas/trechos | Must |
| UXA-04 | Revisão do grafo de conceitos com aceitar/editar/mesclar/dividir/excluir e reordenação de pré-requisitos | Must |
| UXA-05 | Detecção de ciclos, órfãos e conceitos sem fonte | Should |
| UXA-06 | Revisão em lote de lições/atividades com preview em moldura de celular e atalhos de teclado | Must |
| UXA-07 | Alerta de lição fora da faixa 3–7 min | Should |
| UXA-08 | Material complementar com fonte, licença e "por que sugeri"; aprovação individual | Must |
| UXA-09 | Configuração do tutor (tom, política de respostas por nível, limites, orçamento) com sandbox de teste | Must |
| UXA-10 | Checklist bloqueante de publicação e convite por link/código/CSV | Must |
| UXA-11 | Integração LTI para convite/notas | Could |
| UXA-12 | Edição inline por blocos com regeneração por instrução apresentada como proposta | Must |
| UXA-13 | Histórico de versões com restauração não destrutiva | Must |
| UXA-14 | Diff IA × mentor lado a lado/inline | Should |
| UXA-15 | Blocos reutilizáveis com propagação controlada | Could |
| UXA-16 | Heatmap turma × conceito × Bloom com drill-down para alunos | Must |
| UXA-17 | Lista de alunos em risco/travados com ações rápidas | Must |
| UXA-18 | Misconceptions recorrentes com evidências e "criar lição de correção" | Should |
| UXA-19 | Perguntas frequentes ao tutor com "adicionar ao conteúdo" | Should |
| UXA-20 | Ranking de conteúdo com pior desempenho ligado ao editor | Should |
| UXA-21 | Fila de avaliação humana com pré-avaliação por rubrica e meta de tempo de resposta | Must |
| UXA-22 | Fluxo de contestação com propagação de correção de gabarito | Must |
| UXA-23 | Mensagem para aluno/grupo com modelo contextual gerado pela IA | Must |
| UXA-24 | Ajuste de trilha individual (pular, reforçar, mudar nível-alvo) | Should |
| UXA-25 | Liberar/travar conceito com agendamento e aviso de impacto | Must |
| UXA-26 | Desafio da semana com propostas da IA | Could |
| UXA-27 | Resposta a escalonamento do tutor com "guardar como regra" | Must |
| UXA-28 | Indicador de confiança com motivo em todo item gerado; exclusão de itens < 60 do aceite em lote | Must |
| UXA-29 | Painel "o que mudou" desde a última visita | Should |
| UXA-30 | Rastreabilidade item → trecho-fonte destacado | Must |
| UXA-31 | Página de custos com tetos, projeção e estimativa antes de operações em lote | Must |
| UXA-32 | Papéis Dono/Editor/Monitor com permissões distintas | Must |
| UXA-33 | Comentários com menções e resolução em qualquer item | Should |
| UXA-34 | Presença e merge por versão em edição simultânea | Could |
| UXA-35 | Log de atividade da disciplina | Should |
| UXA-36 | App/web mobile com inbox, aprovação por swipe, fila de avaliação e heatmap simplificado | Must |
| UXA-37 | Push para risco, escalonamento, contestação e custo a 80 % | Should |
| UXA-38 | Continuidade celular → desktop no mesmo item | Should |
| UXA-39 | Cronômetro de setup e contador "faltam N itens para publicar" | Could |
| UXA-40 | "Testar como aluno" em qualquer etapa | Should |
