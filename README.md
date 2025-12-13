# Previsão de Temperatura com Regressão Harmônica Dinâmica e Boosting

Este repositório contém os códigos, experimentos e análises desenvolvidos no artigo: 
**Avaliação da Regressão Harmônica Dinâmica com Boosting para Previsões de Temperatura em 365 Dias: Um Estudo de Caso em Chicago**  

---

## Visão Geral

A previsão de temperatura em horizontes longos é um problema desafiador devido à forte sazonalidade anual e à presença de heteroscedasticidade. Este projeto avalia uma abordagem **híbrida** de previsão, combinando:

- **Regressão Harmônica Dinâmica (RHD)** para modelar a estrutura temporal e sazonal;
- **LightGBM** aplicado aos resíduos da RHD, com o objetivo de capturar padrões remanescentes não explicados pelo modelo estatístico.

O estudo utiliza dados meteorológicos diários da cidade de **Chicago (EUA)** e considera um horizonte de previsão de até **365 dias à frente**.

---

## 🎯 Objetivos

- Avaliar o desempenho da Regressão Harmônica Dinâmica em previsões de longo prazo;
- Investigar se a modelagem dos resíduos com aprendizado de máquina gera ganhos de acurácia;
- Analisar as limitações de modelos híbridos em cenários com forte estrutura sazonal e covariáveis com baixo poder preditivo em grandes defasagens.

---

## 📊 Conjunto de Dados

- **Fonte:** API do pacote `Meteostat` (Python)
- **Período:** 21/11/2014 a 09/11/2025
- **Frequência:** Diária
- **Variável alvo:** Temperatura média diária (`tavg`)
- **Covariáveis utilizadas:**
  - Temperatura mínima e máxima
  - Precipitação
  - Neve
  - Velocidade do vento
  - Pressão atmosférica
  - Amplitude térmica diária (variável derivada)

As variáveis com baixa completude foram removidas e os dados passaram por um processo rigoroso de sanitização e preparação.

---

## 🧠 Metodologia

O trabalho foi estruturado em quatro etapas principais:

### 1. Sanitização dos Dados
- Verificação de consistência e continuidade temporal;
- Remoção de variáveis com alta taxa de ausência;
- Tratamento de valores extremos e inconsistências.

### 2. Análise Exploratória
- Identificação de sazonalidade anual dominante via análise wavelet;
- Investigação de tendência (considerada desprezível);
- Avaliação de heteroscedasticidade;
- Testes com transformação Box–Cox (posteriormente descartada).

### 3. Modelagem Temporal
- Ajuste de múltiplos modelos de **Regressão Harmônica Dinâmica** (ARIMA com termos de Fourier);
- Seleção baseada em AIC e validação por janela deslizante;
- Escolha da RHD como modelo baseline.

### 4. Modelagem dos Resíduos
- Engenharia extensiva de variáveis com defasagens longas (365–730 dias);
- Comparação entre XGBoost e LightGBM;
- Seleção de variáveis em múltiplas etapas (gain e forward selection);
- Otimização de hiperparâmetros via Optuna e Grid Search.

---

## 📈 Resultados

- A **Regressão Harmônica Dinâmica isolada** apresentou desempenho igual ou superior ao modelo híbrido na maior parte dos horizontes analisados;
- O **modelo híbrido (RHD + LightGBM)** obteve ganhos marginais, mais perceptíveis em horizontes mais longos;
- Os resultados indicam que, em contextos com forte sazonalidade e covariáveis pouco informativas, o potencial de melhoria via aprendizado de máquina é limitado.

---

## 🧪 Métricas de Avaliação

- **MAE (Mean Absolute Error)** – métrica principal
- **RMSE (Root Mean Squared Error)** – utilizada em análises exploratórias

---

## 🛠️ Tecnologias Utilizadas

- Python  
- `statsmodels`  
- `lightgbm`  
- `xgboost`  
- `optuna`  
- `pandas`, `numpy`, `scikit-learn`, `matplotlib`, `seaborn`

---

## 📁 Estrutura do Repositório

├── data/ # Dados brutos e processados
├── notebooks/ # Análises exploratórias e experimentos
├── src/ # Código-fonte dos modelos
├── results/ # Resultados, métricas e figuras
├── README.md # Documentação do projeto
└── artigo.pdf # Versão final do artigo


---

## 🚀 Reprodutibilidade

Os experimentos utilizam validação por janela deslizante e validação cruzada. As etapas de pré-processamento, modelagem e avaliação foram automatizadas para facilitar a reprodução dos resultados.

---

## 📚 Referência

Se utilizar este repositório, cite:

> Moura, P. G. *Avaliação da Regressão Harmônica Dinâmica com Boosting para Previsões de Temperatura em 365 Dias: Um Estudo de Caso em Chicago*.
