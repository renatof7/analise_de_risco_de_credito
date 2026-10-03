# Previsão de Risco de Crédito e Análise de Inadimplência

Projeto end-to-end de Machine Learning aplicado à concessão de crédito bancário, focado na minimização de perdas financeiras por inadimplência (Falsos Negativos) e na conformidade regulatória.

---

## Sumário Executivo

Este projeto aborda o desafio da concessão de crédito através de um pipeline estruturado em seis fases versionadas. O objetivo primário foi desenvolver um modelo capaz de identificar proativamente clientes com alto risco de default, priorizando a métrica de Recall (Sensibilidade) devido ao custo assimétrico do erro financeiro.

### Principais Insights da Análise Exploratória (EDA)
* **Desbalanceamento Severo:** A base histórica apresentou proporção desigual de clientes adimplentes em relação aos inadimplentes, exigindo estratégias de reamostragem exclusivas no treino para evitar um modelo enviesado.
* **Comprometimento de Renda como Fator Crítico:** A criação da variável sintética de comprometimento de renda (valor do empréstimo sobre a renda) revelou que clientes com alto grau de comprometimento apresentam taxa desproporcionalmente maior de inadimplência.
* **Outliers e Sensibilidade à Distância:** Variáveis como renda e idade apresentaram forte assimetria e valores extremos, evidenciando a necessidade de padronização para algoritmos baseados em distância (KNN) e destacando a imunidade natural de algoritmos baseados em árvore a essas distorções.

### Escolha do Modelo
* **Modelo Selecionado:** Árvore de Decisão com profundidade máxima limitada.
* **Justificativa Financeira:** A Árvore de Decisão atingiu o maior Recall na classe inadimplente e maior área sob a curva ROC (AUC), superando o KNN. Em termos econômicos, a Árvore reduziu significativamente o número de calotes não detectados (Falsos Negativos), gerando a menor perda financeira estimada na simulação de portfólio.
* **Explicabilidade e Compliance:** Diferente do KNN, a Árvore permite a extração da importância das variáveis e a auditoria direta das regras de decisão, critério fundamental para justificativas de recusa de crédito perante regulações bancárias e órgãos de proteção de dados.

---

## Problema de Negócio

No setor financeiro, a concessão de crédito envolve uma decisão assimétrica de risco:
* **Falso Positivo (Erro Tipo I):** Recusar crédito a um cliente bom pagador. O custo é apenas o custo de oportunidade (a margem de lucro de juros perdida).
* **Falso Negativo (Erro Tipo II):** Conceder crédito a um cliente que entra em default. O custo é a perda do valor principal emprestado somada aos custos de recuperação.

**Objetivo:** Maximizar a captura de clientes inadimplentes mantendo um nível aceitável de acurácia global e entregando um modelo interpretável.

---

## Estrutura e Arquitetura do Repositório (Git Versioning)

O repositório adota a arquitetura de versionamento Branch-per-Phase, onde cada etapa técnica foi desenvolvida e isolada em sua respectiva feature branch antes da consolidação na branch principal:

```text
main (produção / histórico consolidado)
 ├── feature/fase-1-eda ─────────────> Carga, estatística descritiva e gráficos visuais
 ├── feature/fase-2-limpeza ─────────> Imputação por mediana e remoção de outliers
 ├── feature/fase-3-feature-eng ─────> Criação da coluna 'comprometimento_renda'
 ├── feature/fase-4-prep-escalonamento> One-Hot Encoding, Split e Scaler seguro
 ├── feature/fase-5-modelagem ───────> Otimização do KNN e Árvore (Curvas de Overfitting)
 └── feature/fase-6-avaliacao-final ──> Matrizes de Confusão, ROC/AUC e Veredito Financeiro
