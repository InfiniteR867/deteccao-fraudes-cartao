# Detecção de Fraudes em Transações de Cartão de Crédito

Projeto desenvolvido como parte de um desafio da DIO, com o objetivo de criar e avaliar modelos de Machine Learning capazes de identificar possíveis fraudes em transações de cartão de crédito.

## Sobre o Dataset

Foi utilizado o dataset **Credit Card Fraud Detection**, disponibilizado no Kaggle.

O conjunto possui **284.807 transações**, sendo:

- 284.315 transações normais
- 492 transações fraudulentas
- Aproximadamente 99,83% das transações são normais
- Aproximadamente 0,17% são fraudes

A variável `Class` representa a classificação da transação:

- `0` = transação normal
- `1` = fraude

As variáveis `V1` a `V28` foram previamente transformadas por PCA.

O dataset é baixado automaticamente durante a execução do notebook utilizando o KaggleHub, portanto o arquivo CSV não é armazenado neste repositório.

## Preparação dos Dados

Durante a preparação dos dados foram realizadas as seguintes etapas:

- Criação da variável `Amount_Log` utilizando transformação logarítmica.
- Separação dos dados em treino e teste.
- Utilização de `stratify` para preservar a proporção entre as classes.
- Padronização das variáveis `Time`, `Amount` e `Amount_Log` utilizando `StandardScaler`.
- Ajuste do scaler apenas nos dados de treinamento para evitar vazamento de dados.

## Modelos Utilizados

Foram treinados e avaliados três modelos:

- Regressão Logística
- Random Forest
- XGBoost

Devido ao forte desbalanceamento do dataset, foram utilizados mecanismos de ponderação das classes durante o treinamento.

## Resultados

Resultados obtidos para a classe fraude:

| Modelo | Precisão | Recall | F1-Score |
|---|---:|---:|---:|
| Regressão Logística | 6,14% | 91,84% | 11,51% |
| Random Forest | 96,10% | 75,51% | 84,57% |
| XGBoost | 34,55% | 86,73% | 49,42% |

Os resultados mostram comportamentos diferentes entre os modelos. A Regressão Logística apresentou maior recall com o limiar padrão, enquanto o Random Forest apresentou maior precisão e F1-Score.

## Ajuste do Limiar

Também foram testados diferentes limiares de classificação para o XGBoost.

Foi selecionado o limiar de **0,3**, aumentando o recall do XGBoost de aproximadamente **86,73% para 89,80%**.

Com esse limiar, o modelo identificou **88 das 98 fraudes** presentes no conjunto de teste, com precisão de aproximadamente **18,57%**.

A redução do limiar aumenta a capacidade de detectar fraudes, mas também aumenta a quantidade de falsos positivos.

## Curvas de Avaliação

Além das métricas tradicionais, foram utilizadas as curvas ROC e Precision-Recall.

Os valores de AUC ROC obtidos foram:

- Regressão Logística: 0,972
- Random Forest: 0,953
- XGBoost: 0,981

Na métrica Average Precision:

- Regressão Logística: 0,716
- Random Forest: 0,865
- XGBoost: 0,833

## Interpretabilidade com SHAP

Foi utilizada a biblioteca SHAP para analisar a influência das variáveis nas previsões realizadas pelo XGBoost.

Entre as variáveis que apresentaram maior influência estão:

- V14
- V4
- V10
- V12

Como as variáveis `V1` a `V28` foram transformadas por PCA, não é possível atribuir diretamente um significado financeiro original a cada uma delas.

## Tecnologias Utilizadas

- Python
- Pandas
- NumPy
- Matplotlib
- Scikit-learn
- XGBoost
- SHAP
- KaggleHub
- Google Colab

## Como Executar

1. Abra o arquivo `deteccao_fraudes_cartao.ipynb` no Google Colab.
2. Execute as células do notebook em ordem.
3. O dataset será baixado automaticamente pelo KaggleHub.
4. Os modelos serão treinados e avaliados durante a execução.

## Diferenças em Relação à Abordagem da Expert

O projeto seguiu o pipeline apresentado pela Expert como referência, mas algumas decisões foram adaptadas durante o desenvolvimento.

Foi criada a variável `Amount_Log` para representar o valor das transações em escala logarítmica e a padronização foi realizada somente após a separação entre treino e teste, ajustando o `StandardScaler` exclusivamente nos dados de treinamento para evitar vazamento de dados.

Também foram comparados diferentes limiares de decisão para o XGBoost. Neste projeto foi escolhido o limiar de 0,3, priorizando o aumento do recall da classe fraude.

Técnicas de undersampling e oversampling não foram implementadas nesta versão e ficam como possibilidades de evolução do projeto.

## Conclusão

O projeto demonstra a importância de utilizar métricas adequadas em problemas de classificação com dados fortemente desbalanceados.

A comparação entre os modelos mostrou diferentes relações entre precisão e recall. O ajuste do limiar do XGBoost também demonstrou como a escolha do ponto de classificação pode aumentar a detecção de fraudes ao custo de uma maior quantidade de falsos positivos.
