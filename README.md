# Projeto Prático Aprendizado Supervisionado no Neurônio Perceptron

**Redes Neurais Artificiais — 2026/2**
Escola Superior de Tecnologia (EST) — Universidade do Estado do Amazonas (UEA)
Profa. Elloá B. Guedes

## Equipe

| Integrante | GitHub |
|---|---|
| Julia Cohen | [@jubcohen](https://github.com/jubcohen) |
| Pedro Portela | [@portelapedro](https://github.com/portelapedro) |

Identificador da dupla: **0** (soma dos últimos dígitos das matrículas, resto da divisão por 4).

## Sobre o projeto

Implementação, em Python, do algoritmo de treinamento do neurônio Perceptron de Rosenblatt por aprendizado supervisionado, aplicado a problemas de classificação binária no plano. O trabalho é dividido em três partes:

1. **Parte I — Problema linearmente separável.** Treinamento até a convergência no conjunto `dataAll20262.txt`, com função degrau ($\vartheta$ = 0), η = 0,1 e pesos iniciais em U(−0,5; +0,5).
2. **Parte II — Experimentação.** Efeito da taxa de aprendizado e do intervalo dos pesos iniciais no custo do treinamento, com 10 repetições de cada uma das 6 configurações η × I = {0,4; 0,1; 0,01} × {(−100, +100); (−0,5, +0,5)} no conjunto `data020262.txt`. Os resultados são apresentados em tabela e boxplots.
3. **Parte III — Validação holdout em problema não linearmente separável.** Divisão 70/30 do conjunto `dataHoldout20262.txt`, treinamento por 100 épocas com os exemplos embaralhados a cada época e avaliação por matriz de confusão, acurácia, precisão, revocação e F-Score.

## Estrutura do repositório

```
.
├── pp-2.1-perceptron-2026-2.ipynb   # notebook com todo o código, resultados e discussões
├── dataAll20262.txt                 # dados da Parte I
├── data020262.txt                   # dados da Parte II (identificador 0)
├── dataHoldout20262.txt             # dados da Parte III
└── README.md
```

O notebook está organizado em duas camadas:

- **Seção 1 — Funções reutilizáveis:** leitura e normalização dos dados, operações do neurônio, laço de treinamento, visualização, experimentos e métricas.
- **Seções 2 a 4 — Partes da atividade:** cada parte apenas define seus parâmetros e chama as funções da Seção 1.

## Como executar

Requisitos: Python 3.6+ e as bibliotecas `numpy`, `pandas`, `matplotlib`, `prettytable` e `scikit-learn` (usado apenas para as métricas de desempenho).

Com conda:

```bash
conda create -n rna python=3.11
conda activate rna
pip install numpy pandas matplotlib prettytable scikit-learn jupyter
jupyter notebook pp-2.1-perceptron-2026-2.ipynb
```

Os arquivos de dados devem estar na mesma pasta do notebook.

## Decisões de implementação

- **Normalização dos atributos (Partes II e III).** Os atributos são levados para [−1, 1] por min-max, coluna a coluna. Sem isso, a diferença de escala entre os atributos (até ±1000 na Parte II, de 0 a 360 em x₁ na Parte III) e a entrada fixa de viés (−1) fazia o limiar se ajustar muito devagar, e o treinamento levava dezenas de milhares de épocas ou oscilava sem se estabilizar. Na Parte III, os limites da normalização são calculados apenas com o conjunto de treino e reaplicados no teste.
- **Aleatoriedade.** Na Parte II, uma semente nova é sorteada e impressa a cada execução, de modo que os resultados variam de uma execução para outra mas podem ser reproduzidos a partir da semente impressa. Na Parte III, a divisão treino/teste e o treinamento usam a semente da equipe (0).
- **Uso de bibliotecas.** Todas as operações matriciais usam `numpy`. O `scikit-learn` é usado somente para as métricas (`sklearn.metrics`); a divisão holdout foi implementada com `numpy`.

## Principais resultados

- **Parte I:** o Perceptron converge em poucas épocas e separa as duas classes sem erros.
- **Parte II:** a configuração η = 0,01 com pesos em (−100, +100) é consistentemente a mais custosa, e as configurações com intervalo equivalente pequeno são estatisticamente equivalentes. A discussão no notebook mostra que, com $\vartheta$ = 0, o que determina o custo do treino é a razão entre o intervalo inicial dos pesos e a taxa de aprendizado (I/η), e não cada um isoladamente.
- **Parte III:** no conjunto de teste, a solução obteve acurácia de 76,00%, precisão de 78,77%, revocação de 73,72% e F-Score de 76,16%, com desempenho praticamente igual ao do treino (76,57% de acurácia). O limite de desempenho vem da própria geometria do problema, já que nenhuma reta separa as classes sem erro.
