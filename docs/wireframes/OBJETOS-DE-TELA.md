# ghdarulms — Objetos de tela (wireframes)

Especificação dos objetos de cada tela dos dois aplicativos, com IDs estáveis usados no protótipo (`prototipo.html`, botão "Anotações" mostra os IDs sobre a interface). Design system: **Linear** (nexu-io/open-design), dark-first, Inter com `cv01, ss03`, pesos 400/510/590, acento índigo `#5e6ad2` / `#7170ff` reservado a ações e estados ativos, bordas brancas semitransparentes, elevação por luminância.

## 0. Vocabulário de componentes (mapeado ao DS Linear)

| Componente | Base Linear | Uso no ghdarulms |
|---|---|---|
| `Button/Primary` | Brand button `#5e6ad2`, radius 6, 8×16 | Ação primária única por tela (Começar, Verificar, Continuar, Publicar) |
| `Button/Ghost` | Ghost `rgba(255,255,255,.02)` + borda `.08` | Ações secundárias (Continuar depois, Pular por agora) |
| `Chip` | Pill `9999px`, borda `#23252a`, 12/510 | Nível Bloom, sugestões do tutor, filtros |
| `Badge/Origem` | Subtle badge 10/510, radius 2 | **Mentor** (verde `#27a644`), **IA · revisado** (acento), **Sugestão da IA** (cinza tracejado), **Complementar** (lavanda `#7a7fad`) |
| `Card/Surface` | `rgba(255,255,255,.02–.05)` + borda `.08`, radius 8/12 | Card de sessão, conceito, lição, item da fila |
| `BloomRing` | SVG próprio, 6 arcos de 60°, escala sequencial única | Progresso por conceito (vazio / em andamento / dominado) |
| `BloomLadder` | 6 colunas em escada com grade de verbos | Seletor de alcance (mentor) |
| `ProgressSegments` | 4 px, um segmento por card | Topo da micro-lição |
| `ConfidenceScale` | 3 pills: chuto / acho que sei / tenho certeza | Antes de cada item de evidência |
| `Sheet/Bottom` | Dialog Radix, 60→90% altura | Tutor contextual, detalhe do conceito |
| `TabBar` | 5 itens, ícone + rótulo, oculta em sessão | Hoje · Trilha · Tutor · Revisões · Perfil |
| `Heatmap` | Grade conceito × nível, célula = % dominou | Dashboard do mentor |
| `Table/Dense` | 13/510 cabeçalho, 15/400 linhas | Alunos em risco, materiais |
| `Sidebar/Steps` | Painel `#0f1011`, itens 13/510 | Wizard do mentor |
| `PhonePreview` | Moldura 375 px | Pré-visualização da lição no app do mentor |

Estados obrigatórios de todo objeto interativo: default, hover, focus visível (anel), disabled, loading (skeleton após 400 ms), erro (texto explicativo com próximo passo).

---

## A. App do Acadêmico (mobile-first, PWA)

### S1 · Hoje
| ID | Objeto | Conteúdo / dados | Estados & regras |
|---|---|---|---|
| S1.1 | Cabeçalho | Saudação com nome, disciplina ativa | — |
| S1.2 | Calendário de consistência | 4 semanas × 7 dias, dia com sessão preenchido | Sem contador que zera; nunca mensagem negativa |
| S1.3 | Card "Sua sessão de hoje" | Conceito, chip do nível Bloom da vez, "~5 min", objetivo em 1 linha, **Começar** | Se sessão interrompida: "Retomar (faltam N cards, ~M min)" |
| S1.4 | Linha de revisões | "N revisões pendentes · X min" → S5 | Oculta se zero |
| S1.5 | Sessão de 1 minuto | 2 flashcards vencidos + 1 confiança | Sempre visível |
| S1.6 | "Por que estou vendo isto" | Link que abre a justificativa da recomendação | Frase objetiva baseada em dados do aluno |
| S1.7 | TabBar | Hoje ativo | — |
| S1.v | Estado vazio | "Seu mentor ainda está preparando o conteúdo" | Sem CTA falso |

### S2 · Trilha
| ID | Objeto | Conteúdo / dados | Estados & regras |
|---|---|---|---|
| S2.1 | Seletor de disciplina | Nome + alcance Bloom da disciplina | — |
| S2.2 | Cabeçalho de unidade | Sticky, nome, "3/5 conceitos" | — |
| S2.3 | Nó de conceito | `BloomRing` + nome + estado | bloqueado (cadeado, cinza) · disponível · atual (pulsa) · dominado · consolidado |
| S2.4 | Linha de pré-requisito | Liga nós; forte = sólida, fraca = tracejada | — |
| S2.5 | Folha de detalhe do conceito | 6 linhas (nível, barra 0–100, **Praticar**); níveis acima do alcance com cadeado "não faz parte deste curso" | Bottom sheet |
| S2.6 | Alternar "Mapa de calor" | Grade conceitos × 6 níveis | Should |
| S2.7 | "Ver mapa completo" | Grafo com pan/zoom | Could |

### S3 · Sessão (micro-lição, 6–12 cards, 3–7 min)
| ID | Objeto | Conteúdo / dados | Estados & regras |
|---|---|---|---|
| S3.0 | `ProgressSegments` | 1 segmento por card | Preenche ao concluir |
| S3.1 | Barra de topo | X (sair), ♡ (salvar), ? (tutor contextual) | Sair abre folha "progresso salvo até o card N" |
| S3.2 | Card Objetivo | "Ao final você vai conseguir: [verbo Bloom + objeto]" + chip do nível | 10 s |
| S3.3 | Card Ativar | Pergunta ligada ao pré-requisito | Tap para tentar |
| S3.4 | `ConfidenceScale` | 3 pills antes de cada item de evidência | Obrigatório, ≤ 1 s |
| S3.5 | Card Tentar (múltipla escolha) | Enunciado ≤ 60 palavras, 4 opções em pilha vertical, **Verificar** | Feedback específico → por quê → próximo passo; distrator mapeado a misconception |
| S3.6 | Card Conteúdo mínimo | ≤ 80 palavras, termo destacado abre popover, badge de origem, citação tocável `[Aula 2, p. 3]` | Variante escolhida conforme o erro |
| S3.7 | Card Praticar (ordenar) | Itens com alça à direita; alternativa por botões ↑↓ | Crédito parcial |
| S3.8 | Card Avaliar (julgue e justifique) | Afirmação, concordo/discordo, justificativa ≥ 40 caracteres, rubrica visível antes, mic | `pending_ai` não bloqueia avanço |
| S3.9 | Card Refletir | Diário de 1 linha + calibração "confiança 3/3 · acertos 2/3" | Voz aceita |
| S3.10 | Card Fechamento | 3 bullets gerados, próxima revisão agendada, nível alcançado, **Concluir** | Celebração sóbria |
| S3.11 | Botão primário fixo | 56 px, largura total, zona do polegar | Único por card |
| S3.12 | Feedback inline | Faixa abaixo da resposta, sem exclamação | — |
| S3.13 | "Pular por agora" | Só em Analisar/Avaliar/Criar; volta na revisão | Ghost |

### S4 · Tutor
| ID | Objeto | Conteúdo / dados | Estados & regras |
|---|---|---|---|
| S4.1 | Cabeçalho | "Tutor · [disciplina]", contexto do card quando aberto como sheet | — |
| S4.2 | Balão do tutor | Markdown curto (≤ 120 palavras), `Badge/Origem`, citações `[n]` tocáveis | Streaming com cursor; skeleton > 400 ms; botão **Parar** |
| S4.3 | Balão do aluno | Texto ou transcrição de voz | — |
| S4.4 | Chips de sugestão | Máx. 3 contextuais + fixos ("Me dê uma dica", "Explique de outro jeito") | Tocar envia |
| S4.5 | Campo de mensagem | Texto + mic (segurar para falar) + enviar | Offline: desabilitado com aviso |
| S4.6 | Folha de citação | Trecho original destacado + "Ver na trilha" | — |
| S4.7 | Ações no balão | Copiar, Salvar como nota, "Isso estava errado" (vai ao mentor) | Toque longo |
| S4.8 | Escada de dicas | Indicador "dica 2 de 4" quando em item de evidência | Nunca revela em evidência/revisão |

### S5 · Revisões
| ID | Objeto | Conteúdo / dados | Estados & regras |
|---|---|---|---|
| S5.1 | Resumo | "6 itens · 3 min" + **Revisar tudo** | — |
| S5.2 | Grupo por conceito | Nome, quantidade, nível de cada item | Intercalado na sessão |
| S5.3 | Item de revisão | Tipo (evocação, problema novo, explicação) e "há 7 dias" | Sem ajuda (degraus 3–4 bloqueados) |
| S5.4 | Estado vazio | "Nada para revisar. Volte amanhã" + próxima data | Lista finita |

### S6 · Perfil
| ID | Objeto | Conteúdo / dados | Estados & regras |
|---|---|---|---|
| S6.1 | Progresso agregado | Conceitos por nível alcançado, minutos na semana | Sem comparação com turma |
| S6.2 | Preferências | Ritmo, formato, profundidade, tom do tutor | Precedência sobre a adaptação da IA |
| S6.3 | Histórico de ajustes | Cartões "Manter / Desfazer" | — |
| S6.4 | Notificações | Janela de horário, 1/dia, adiar/parar | — |
| S6.5 | Pausar sem culpa | 1 semana / 1 mês / até eu voltar | Silêncio total |
| S6.6 | Dados e privacidade | "O que a IA faz com seus dados", exportar, excluir | Linguagem simples |
| S6.7 | Offline | Baixar próximas lições; "Sincronizado 14:02" | — |

---

## B. App do Mentor (desktop-first, responsivo)

### M1 · Wizard — (a) Disciplina e objetivo
| ID | Objeto | Conteúdo / dados | Estados & regras |
|---|---|---|---|
| M1.0 | `Sidebar/Steps` | 7 passos, estado salvo, "faltam N itens para publicar" | Retorno a qualquer passo |
| M1.1 | Campos | Nome, área, público (chips), carga, idioma | — |
| M1.2 | `BloomLadder` | 6 colunas; clicar marca alvo, à esquerda vira base; grade de verbos por coluna | Verbo ambíguo mostra alerta |
| M1.3 | Construtor de objetivo | "Ao final, o aluno será capaz de [verbo] [objeto] [condição]" | Clicar no verbo preenche o slot |
| M1.4 | Sugestões da IA | 3 objetivos alternativos como chips | — |
| M1.5 | Rodapé | Voltar · Salvar rascunho · **Continuar** | — |

### M2 · Wizard — (b) Material
| ID | Objeto | Conteúdo / dados |
|---|---|---|
| M2.1 | Zona de upload | Arrastar; abas Arquivo · URL · YouTube · Drive · Nota |
| M2.2 | `Table/Dense` de materiais | Nome, tipo, páginas/duração, status (fila → extraindo → indexado → erro), recorte, marcação Principal/Complementar/Referência |

### M3 · Wizard — (c) Grafo e objetivos
| ID | Objeto | Conteúdo / dados | Estados & regras |
|---|---|---|---|
| M3.1 | Alternância Grafo / Outline | — | — |
| M3.2 | Outline | Árvore indentada = dependência; arrastar reordena | — |
| M3.3 | Nó | Nome, chip do alcance, confiança 0–100, borda tracejada se < 60 | — |
| M3.4 | Barra de revisão | "12 conceitos · 9 aceitos · 3 pendentes" + **Aceitar todos ≥ 80** | Itens < 60 nunca entram no lote |
| M3.5 | Painel direito | Título, resumo, objetivos por nível (verbo validado), trecho-fonte com "abrir na p. 12", confiança + motivo | — |
| M3.6 | Ações | Aceitar · Editar · Mesclar · Dividir · Excluir · Pré-requisito de… | — |
| M3.7 | Alertas | Ciclo, órfão, sem fonte (chips clicáveis) | — |

### M4 · Wizard — (d) Lições e atividades
| ID | Objeto | Conteúdo / dados | Estados & regras |
|---|---|---|---|
| M4.1 | Lista de conceitos | Contador lições/atividades geradas e aprovadas | — |
| M4.2 | Editor por blocos | Parágrafo, exemplo, callout, atividade; badge de nível e conceito | Regenerar com instrução → proposta lado a lado |
| M4.3 | `PhonePreview` | Exatamente como o aluno verá; tempo estimado; alerta > 7 min | — |
| M4.4 | Lista de atividades | Tipo, nível, gabarito/rubrica, "requer avaliação humana" | Automático em Avaliar/Criar |
| M4.5 | Atalhos | J/K, A aprovar, R rejeitar com motivo, E editar; "34/58 revisados" | — |
| M4.6 | Histórico / diff | IA × mentor, restaurar não destrutivo | — |

### M5 · Wizard — (e)(f)(g)
| ID | Objeto | Conteúdo / dados |
|---|---|---|
| M5.1 | Cards de material complementar | Fonte, licença (badge), "por que sugeri", Aprovar / Rejeitar / Substituir |
| M5.2 | Configuração do tutor | Tom, política de resposta por nível (nunca / após 2 tentativas / com explicação / livre), limites, orçamento, **Testar tutor** |
| M5.3 | Checklist de publicação | Bloqueante; convite por link/código/CSV; publicar tudo ou liberar por conceito |

### M6 · Dashboard
| ID | Objeto | Conteúdo / dados | Estados & regras |
|---|---|---|---|
| M6.1 | KPIs | Ativos 7d, progresso médio, conclusão, custo de IA no mês | Custo com alerta a 80% |
| M6.2 | `Heatmap` | Conceito × nível até o alvo; célula = % dominou; clique → alunos | Toggle colunas = alunos |
| M6.3 | Alunos em risco | Nome, conceito, dias parado, tentativas, sinal; ações Mensagem / Ajustar / Ver conversa | Rascunho da IA na mensagem |
| M6.4 | Misconceptions recorrentes | "31% confundem X com Y", evidências anonimizadas, **Criar lição de correção** | — |
| M6.5 | Perguntas frequentes ao tutor | Agrupadas, contagem, **Adicionar ao conteúdo** | — |
| M6.6 | Conteúdo com pior desempenho | Taxa de erro, abandono, tempo real × estimado | Badge "revisar" → editor |
| M6.7 | "O que mudou" | Alterações desde a última visita, aceitar/ignorar | — |

### M7 · Fila humana e contestações
| ID | Objeto | Conteúdo / dados | Estados & regras |
|---|---|---|---|
| M7.1 | Item da fila | Aluno, atividade (Avaliar/Criar), entregue há X, artefato | Ordenado por tempo de espera |
| M7.2 | Rubrica pré-preenchida | 3 critérios 0–4 com trecho citado pela IA; **Aceitar sugestão** ou editar | Meta de tempo visível |
| M7.3 | Contestação | Resposta do aluno, gabarito, argumento, reavaliação da IA; **Manter · Aceitar · Corrigir gabarito** | Corrigir gabarito propaga |
| M7.4 | Escalonamento do tutor | Conversa, motivo, resposta sugerida; Enviar · Editar · Devolver ao tutor; "guardar como regra" | Push no celular |
