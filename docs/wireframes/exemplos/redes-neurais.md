# Exemplo de conteúdo: Redes Neurais e Deep Learning

Mapeamento dos quatro laboratórios interativos da sessão "Pós Católica" (`docs/wireframes/referencias/pos-catolica/`) para o modelo do ghdarulms: disciplina → unidades → conceitos → objetivos por nível de Bloom → micro-lições de 3–7 min. É o conteúdo que o protótipo (`prototipo.html`, disciplina "Redes Neurais") usa.

## 1. O que muda de formato

| Laboratório original | Formato | No ghdarulms |
|---|---|---|
| Uma página longa (15 min), várias seções numeradas, visualizações manipuláveis, tema claro | Laboratório de exploração | Vira **material do mentor** (tipo "Laboratório") e é fatiado pela IA em **micro-lições de 6–12 cards**. Cada visualização manipulável vira um **card de simulador** com uma tarefa e um critério de verificação (evidência de Aplicar/Analisar). O laboratório completo fica disponível como "aprofundar" ao final da lição. |

O que se preserva dos laboratórios: o princípio "tente antes da explicação" (o backprop abre com "9 números contra você"), as visualizações vivas (plano de decisão, reta ajustada, rede 2-2-1, dígito desenhado) e a linguagem direta.

## 2. Disciplina

- **Nome:** Redes Neurais Artificiais e Deep Learning (pós-graduação)
- **Alcance:** Analisar (o mentor pode subir para Avaliar no trabalho final)
- **Materiais do mentor:** apostila (Unidades 1–3, docx/pdf), slides das aulas 1–5, enunciado do trabalho final, 4 laboratórios HTML

## 3. Grafo de conceitos

```
Unidade I · Fundamentos
  Neurônio de McCulloch–Pitts  (alcance Analisar)  ← lab neuronio_mp.html
    └─ forte → Perceptron e regra de aprendizagem (Aplicar)
    └─ forte → Regressão linear e gradiente descendente (Analisar)  ← lab regressao.html
Unidade II · Aprendizado
  Funções de ativação (Compreender)
  Backpropagation (Analisar)  ← lab backprop.html   requer: Gradiente (Aplicar), Perceptron (Aplicar)
Unidade III · Aplicações
  MLP e classificação de dígitos (Analisar)  ← lab mnist.html   requer: Backprop (Compreender)
  Redes convolucionais (Analisar)  requer: MLP (Aplicar)
  Transfer learning (Avaliar)  fraca: CNN
```

## 4. Objetivos do conceito "Neurônio de McCulloch–Pitts"

| Nível | Objetivo (verbo + objeto + condição) | Evidência |
|---|---|---|
| Memorizar | **Identificar** os componentes do neurônio MP (entradas binárias, pesos, viés, função degrau) | Flashcard + múltipla escolha |
| Compreender | **Explicar** por que a saída muda quando o viés muda, com um exemplo próprio | Resposta curta avaliada por rubrica |
| Aplicar | **Calcular** a saída do neurônio para entradas e pesos dados e **configurar** pesos e viés para implementar uma porta lógica | Simulador com tabela-verdade verificada |
| Analisar | **Diferenciar** funções linearmente separáveis (AND, OR) de não separáveis (XOR) usando o plano de decisão | Julgue e justifique com critérios |

## 5. Micro-lição do protótipo: "Neurônio MP · Aplicar" (8 cards, ~6 min)

| # | Card | Tipo | Bloom | Conteúdo |
|---|---|---|---|---|
| 1 | Objetivo | objective | Aplicar | "Calcular a saída do neurônio e configurar uma porta lógica" |
| 2 | Ativar | activate | — | Dois interruptores e uma lâmpada que só acende com os dois ligados: qual é a regra? |
| 3 | Tentar | mcq + confiança | Aplicar (evidência 1) | z = w₁x₁ + w₂x₂ + b com x=(1,0), w=(1,1), b=−1,5. Saída? Distrator "1" ↔ misconception "soma sem viés" |
| 4 | Conteúdo mínimo | content | — | Regra: dispara se z ≥ 0. O viés desloca a fronteira. Citação: Apostila, Unidade III |
| 5 | Praticar | simulador | Aplicar (evidência 2) | Ajuste w₁, w₂ e b para que a tabela-verdade seja a do AND. Verificação automática dos 4 casos |
| 6 | Analisar | judgment + confiança | Analisar | "Um único neurônio MP implementa o XOR." Concordo/discordo + justificativa (rubrica visível) |
| 7 | Refletir | reflect | — | Calibração + diário de 1 linha |
| 8 | Fechamento | done | — | Nível alcançado, próxima revisão, link "Abrir o laboratório completo (15 min)" |

## 6. Próximas micro-lições sugeridas pela IA (para o mentor aprovar)

- **Regressão e gradiente · Aplicar:** card de simulador com taxa de aprendizagem; tarefa "faça a perda cair abaixo de 0,1 em 20 passos".
- **Backprop · Compreender:** abre com o desafio "9 números contra você" (do lab), depois um card por etapa: sinal para frente, erro, culpa para trás.
- **MLP e dígitos · Analisar:** o aluno desenha um dígito, vê a inferência e responde "qual camada mudou mais ao trocar o 1 pelo 7?".
