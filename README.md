
# Análise de Rendimento de Culturas Agrícolas

## Introdução e Visão Geral do Projeto

Este projeto tem como objetivo analisar e prever o rendimento de diferentes culturas agrícolas com base em variáveis climáticas e de umidade. Utilizamos técnicas de Análise Exploratória de Dados (EDA) para entender as características do dataset, clusterização para identificar grupos de culturas com padrões semelhantes e modelagem preditiva para construir um modelo capaz de prever o rendimento com alta acurácia.

## 1. Preparação dos Dados

A etapa inicial envolveu o carregamento do dataset `crop_yield.csv` utilizando a biblioteca `pandas`. O dataset é composto por **156 linhas** e **6 colunas**, sem a presença de valores nulos, indicando um conjunto de dados limpo para análise.

As colunas são:
*   **4 colunas `float64`:** `Precipitation (mm day-1)`, `Specific Humidity at 2 Meters (g/kg)`, `Relative Humidity at 2 Meters (%)`, `Temperature at 2 Meters (C)`. Estas representam medidas contínuas de fatores climáticos.
*   **1 coluna `int64`:** `Yield`. Corresponde à variável alvo (produção), um valor inteiro.
*   **1 coluna `object`:** `Crop`. Armazena os nomes das culturas como texto.

Foram identificados **4 tipos de culturas distintas**: 'Cocoa, beans', 'Oil palm fruit', 'Rice - paddy' e 'Rubber, natural'. A distribuição entre essas culturas é **uniforme**, com cada uma contendo a mesma quantidade de registros no dataset.

## 2. Análise Exploratória de Dados (EDA)

### Distribuição das Variáveis Numéricas

Através dos histogramas, observamos a distribuição das variáveis numéricas:
*   **`Precipitation (mm day-1)` e `Specific Humidity at 2 Meters (g/kg)`:** Apresentam distribuições aproximadamente normais.
*   **`Relative Humidity at 2 Meters (%)`:** Exibe uma assimetria à esquerda, com maior concentração de valores mais altos.
*   **`Temperature at 2 Meters (C)`:** Sugere uma distribuição bimodal ou com múltiplos picos, possivelmente indicando diferentes regimes climáticos.
*   **`Yield`:** Apresenta uma distribuição próxima de uniforme ou ligeiramente bimodal, refletindo níveis de produtividade específicos.

### Matriz de Correlação

A matriz de correlação revelou os seguintes insights:
*   **Correlações Positivas com `Yield`**: A produtividade ('Yield') exibe correlações positivas com `Precipitation`, `Specific Humidity` e `Temperature`, sugerindo que esses fatores climáticos tendem a impulsionar o rendimento.
*   **Fortes Correlações entre Variáveis Climáticas**: Há uma alta intercorrelação entre `Precipitation`, `Specific Humidity` e `Relative Humidity`, assim como entre `Temperature` e `Specific Humidity`, o que é esperado e reflete a complexidade do sistema climático.

### Rendimento por Cultura

O boxplot da distribuição do rendimento por cultura mostrou diferenças claras:
*   **'Oil palm fruit'**: Maior rendimento mediano e valores de produtividade mais elevados.
*   **'Cocoa, beans'** e **'Rubber, natural'**: Rendimentos medianos intermediários.
*   **'Rice - paddy'**: Menor rendimento mediano entre todas as culturas analisadas.

### Relação entre Temperatura e Rendimento

O gráfico de dispersão (`Temperature vs. Yield` por cultura) indicou que a temperatura é um fator crucial, com sua influência modulada pelo tipo de cultura:
*   **'Oil palm fruit'**: Maiores rendimentos em temperaturas mais elevadas.
*   **'Cocoa, beans'** e **'Rubber, natural'**: Rendimentos intermediários em faixas de temperatura semelhantes.
*   **'Rice - paddy'**: Menores rendimentos em todas as faixas de temperatura.

### Relação entre Precipitação e Rendimento

Similar à temperatura, a precipitação também é um fator crítico, com sua influência dependendo da cultura:
*   **'Oil palm fruit'**: Maiores rendimentos, especialmente em regiões com maior volume de chuva.
*   **'Cocoa, beans'** e **'Rubber, natural'**: Rendimentos intermediários em faixas de precipitação similares.
*   **'Rice - paddy'**: Menores rendimentos em todas as faixas de precipitação.

## 3. Clusterização

### Preparação dos Dados para Clusterização

Para a clusterização, a variável categórica 'Crop' foi removida, e as variáveis numéricas restantes (`Precipitation`, `Specific Humidity`, `Relative Humidity`, `Temperature`, `Yield`) foram normalizadas usando `StandardScaler` para garantir que todas as variáveis contribuíssem igualmente para o cálculo das distâncias.

### Método do Cotovelo

O **Método do Cotovelo** (`Elbow Method`) foi aplicado para determinar o número ideal de clusters. A análise da inércia em função do número de clusters sugeriu **4 como o número ideal de clusters** (`k=4`), dada a queda acentuada na inércia neste ponto.

### Aplicação do K-Means

O algoritmo K-Means foi aplicado com `n_clusters=4`. Os clusters resultantes, visualizados em um gráfico de dispersão (`Temperature vs. Yield`), mostraram **agrupamentos claros e bem definidos**. Esses clusters refletem padrões distintos de produtividade e condições climáticas, sugerindo que o algoritmo conseguiu agrupar observações que compartilham características semelhantes, correspondendo, em grande parte, aos diferentes tipos de `Crop` no dataset original.

## 4. Modelagem Preditiva - Regressão Supervisionada

### Preparação dos Dados para Modelagem

Para a modelagem preditiva do rendimento (`Yield`):
*   A variável categórica `Crop` foi codificada usando **One-Hot Encoding** (`pd.get_dummies`).
*   Os dados foram divididos em conjuntos de **treino (80%)** e **teste (20%)** usando `train_test_split` com `random_state=42`.

### Modelos de Regressão Utilizados

Os seguintes modelos de regressão foram avaliados:
*   Linear Regression
*   Decision Tree Regressor
*   Random Forest Regressor
*   Support Vector Regressor (SVR)
*   K-Nearest Neighbors Regressor (KNN)

### Métricas de Avaliação

Os modelos foram avaliados utilizando as métricas:
*   **MAE (Mean Absolute Error)**: Média do valor absoluto dos erros.
*   **RMSE (Root Mean Squared Error)**: Raiz quadrada da média dos quadrados dos erros.
*   **R2 (R-squared)**: Coeficiente de determinação, indicando a proporção da variância na variável dependente que é previsível a partir das variáveis independentes.

### Resultados dos Modelos

| Modelo            | MAE       | RMSE      | R2       |
| :---------------- | :-------- | :-------- | :------- |
| Linear Regression | 3816.90   | 5109.75   | 0.9933   |
| Decision Tree     | 3088.12   | 4973.91   | 0.9936   |
| **Random Forest** | **2661.48** | **4875.62** | **0.9939** |
| SVR               | 38965.31  | 71305.04  | -0.3108  |
| KNN               | 3250.52   | 5720.78   | 0.9916   |

### Seleção do Melhor Modelo

O modelo **Random Forest Regressor** apresentou o melhor desempenho geral, com o menor MAE (2661.48) e RMSE (4875.62), e o maior coeficiente R2 (0.9939). Isso indica que ele foi o mais preciso na previsão do rendimento, conseguindo explicar a maior parte da variância na variável alvo e minimizando os erros de previsão. Sua robustez e capacidade de capturar relações complexas nos dados o destacaram entre os demais.

## 5. Exportação do Modelo Treinado

O modelo de Random Forest treinado, devido ao seu desempenho superior, foi exportado e salvo no formato `joblib` como `modelo_random_forest_crop.pkl`. Isso permite que o modelo seja facilmente carregado e utilizado em uma aplicação real para fazer previsões de rendimento de culturas.

