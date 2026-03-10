
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

---

# Estimativa de Custos AWS para Hospedagem de API com Machine Learning

## 1. Objetivo

O objetivo é realizar uma estimativa de custos utilizando a **AWS Pricing Calculator** para hospedar uma máquina virtual Linux responsável por executar uma API que receberá dados de sensores e executará rotinas de Machine Learning.

A simulação considera as seguintes especificações:

- 2 CPUs
- 1 Gigabit de memória
- Até 5 Gigabit de rede
- 50 GB de armazenamento (HD)
- Modelo de pagamento **On-Demand (100%)**

A estimativa foi realizada comparando duas regiões da AWS:

- **Norte da Virgínia (EUA)**
- **São Paulo (Brasil)**

---

# 2. Escolha do Serviço

Para a hospedagem da API foi escolhido o serviço **Amazon EC2 (Elastic Compute Cloud)**.

O EC2 permite criar máquinas virtuais na nuvem com diferentes configurações de CPU, memória, rede e armazenamento, sendo amplamente utilizado para hospedar aplicações web, APIs e serviços de processamento.

Neste projeto, o EC2 será responsável por:

- Executar a API que recebe dados dos sensores
- Processar informações utilizando Machine Learning
- Armazenar temporariamente os dados recebidos
- Disponibilizar o serviço de forma contínua

A escolha do EC2 foi feita por ser um serviço flexível e escalável, permitindo configurar exatamente os recursos necessários para o funcionamento da aplicação.

<img width="886" height="488" alt="image" src="https://github.com/user-attachments/assets/672c575f-e19a-4d93-b961-4df154aa80fb" />

---

# 3. Configuração da Instância EC2

Após selecionar o serviço Amazon EC2 na AWS Pricing Calculator, foi realizada a configuração da máquina virtual que será responsável por hospedar a API e executar os processos de Machine Learning.

A configuração foi definida de forma a atender aos requisitos propostos inicialmente, mantendo o menor custo possível.

## 3.1 Região

Foram avaliadas duas regiões da AWS para comparação de custos:

- **US East (N. Virginia)**
- **South America (São Paulo)**

A comparação entre regiões é importante pois o custo de infraestrutura da AWS pode variar dependendo da localização do datacenter.

---

## 3.2 Sistema Operacional

Foi selecionado o sistema operacional:

- **Linux**

O Linux foi escolhido por ser amplamente utilizado em servidores de aplicações e APIs, além de não possuir custos adicionais de licenciamento, diferentemente de sistemas operacionais comerciais.

<img width="886" height="382" alt="image" src="https://github.com/user-attachments/assets/8d64647d-edd1-4961-8c1a-21a7e4a28fa1" />

---

## 3.3 Tipo de Instância

A instância escolhida foi:

- t3.micro

Características principais da instância:

- 2 vCPU
- 1 Gigabit de memória
- Performance de rede de até 5 Gigabit
- Modelo de computação burstável

Esse tipo de instância pertence à família **General Purpose**, sendo indicado para aplicações leves como APIs, microsserviços e pequenos processamentos de dados.

<img width="886" height="391" alt="image" src="https://github.com/user-attachments/assets/14d50e9a-f3fe-4339-a254-a2ebc5c50a98" />

---

## 3.4 Modelo de Pagamento

O modelo de pagamento selecionado foi:

**On-Demand (100% de utilização mensal)**

Nesse modelo, o pagamento ocorre apenas pelo tempo de utilização da instância, sem necessidade de reserva antecipada ou contratos de longo prazo.

Esse modelo é ideal para ambientes de desenvolvimento ou aplicações que precisam permanecer disponíveis continuamente.

<img width="886" height="390" alt="image" src="https://github.com/user-attachments/assets/8a8a983a-7d9a-4c52-9f60-e34b60676e6a" />

---

## 3.5 Armazenamento

Para o armazenamento foi configurado:

- **50 GB de armazenamento EBS**

O Amazon EBS (Elastic Block Store) é utilizado para fornecer armazenamento persistente para instâncias EC2.

Esse espaço será utilizado para armazenar:

- arquivos da aplicação
- logs do sistema
- dados temporários processados pela API

<img width="886" height="261" alt="image" src="https://github.com/user-attachments/assets/657d6c28-1161-4cdc-901f-cebe6aab7a63" />

---

# 4. Estimativa de Custos

Após realizar a configuração da instância EC2 na AWS Pricing Calculator, foi gerada uma estimativa de custos para duas regiões distintas da AWS.

A simulação foi realizada utilizando exatamente a mesma configuração de máquina para ambas as regiões, permitindo uma comparação direta entre os custos.

## 4.1 Resultado da Simulação

| Região AWS | Custo Mensal | Custo Anual |
|-------------|-------------|-------------|
| US East (N. Virginia) | USD 8.34 | USD 100.08 |
| South America (São Paulo) | USD 13.70 | USD 164.40 |

Observa-se que a região **US East (N. Virginia)** apresenta um custo significativamente menor em comparação com a região **South America (São Paulo)**.

Essa diferença ocorre devido a fatores como:

- maior quantidade de datacenters nos Estados Unidos
- maior escala de infraestrutura
- custos operacionais menores


<img width="886" height="372" alt="image" src="https://github.com/user-attachments/assets/c6fe66fb-f621-468a-9a55-23b7ddb3c58a" />

---

# 5. Conclusão

A partir da simulação realizada na AWS Pricing Calculator foi possível estimar os custos de execução de uma máquina virtual Linux utilizando o serviço Amazon EC2 para hospedar uma API responsável por receber dados de sensores e executar rotinas de Machine Learning.

A configuração utilizada atendeu aos requisitos da atividade, considerando:

- 2 vCPU
- 1 Gigabit de memória
- até 5 Gigabit de rede
- 50 GB de armazenamento
- modelo de pagamento On-Demand

Durante a análise comparativa entre as regiões da AWS, observou-se que a região **US East (N. Virginia)** apresentou custo significativamente menor quando comparada à região **South America (São Paulo)**.

| Região AWS | Custo Mensal | Custo Anual |
|-------------|-------------|-------------|
| US East (N. Virginia) | USD 8.34 | USD 100.08 |
| South America (São Paulo) | USD 13.70 | USD 164.40 |

Sob uma perspectiva exclusivamente financeira, a escolha mais econômica seria a região **US East (N. Virginia)**.

Entretanto, o cenário proposto também considera dois fatores importantes:

- necessidade de **acesso rápido aos dados dos sensores**
- **restrições legais para armazenamento de dados no exterior**

Diante dessas condições, a escolha mais adequada para a hospedagem da aplicação seria a região **South America (São Paulo)**.

A utilização de uma região localizada no Brasil proporciona:

- **menor latência de rede**, melhorando o tempo de resposta na coleta de dados dos sensores
- **conformidade com requisitos legais e regulatórios**, garantindo que os dados permaneçam armazenados no país
- maior segurança quanto à **localização e governança dos dados**

Dessa forma, embora a região **US East (N. Virginia)** apresente menor custo, a região **South America (São Paulo)** foi escolhida por atender melhor aos requisitos operacionais e legais do projeto.

---

## Orçamento Completo

O relatório completo gerado pela AWS Pricing Calculator pode ser consultado no documento abaixo:

📄 [Orçamento AWS](./Orçamento%20FIAP%20Fase%205%20-%20Calculadora%20de%20Preços%20da%20AWS.pdf)


