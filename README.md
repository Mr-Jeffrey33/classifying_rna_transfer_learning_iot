# 🌸 Classificação de Flores com Redes Neurais Artificiais e Transfer Learning

> **Atividade Prática — Formação IoT Specialist (DIO)**  
> Construção de dataset, treinamento com Transfer Learning (MobileNetV2), análise de overfitting, otimização de hiperparâmetros (256 neurônios + Data Augmentation) e comparação experimental entre levas de treino.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com)
![Python](https://img.shields.io/badge/Python-3.10%2B-blue?logo=python)
![TensorFlow](https://img.shields.io/badge/TensorFlow-2.x-orange?logo=tensorflow)
![Keras](https://img.shields.io/badge/Keras-Deep%20Learning-red?logo=keras)

---

## 📌 Visão Geral do Projeto

Este projeto foi desenvolvido como parte prática da formação **IoT Specialist da DIO**. O objetivo foi construir um pipeline completo de Visão Computacional:
1. Criação e preparação de um **dataset balanceado de imagens** (>100 imagens por classe).
2. Treinamento de uma Rede Neural Convolucional aplicando **Transfer Learning** sobre a arquitetura **MobileNetV2** (pesos pré-treinados do *ImageNet*).
3. Coleta e diagnóstico das métricas da **1ª Leva de Treinamento** (identificação de *overfitting*).
4. **Otimização da Rede** na **2ª Leva**: expansão de capacidade (256 e 128 neurônios), regularização com Dropout e aplicação de *Data Augmentation*.
5. Análise comparativa entre os resultados antes e depois da otimização.

---

## 📁 Estrutura do Dataset

* **Classes:** 2 classes distintas de flores.
* **Volume:** Mais de 100 imagens por categoria com boa resolução (>400x400 px).
* **Divisão dos Dados:**
  * **80%** para Treinamento (com embaralhamento e cache).
  * **20%** para Validação.
* **Resolução de Entrada:** Imagens redimensionadas para `128x128` pixels em lotes de tamanho 16 (*batch size*).

---

## 🔬 Metodologia Experimental

### 1ª Leva: Transfer Learning Direto (Baseline)
* **Base convolucional:** MobileNetV2 (pesos congelados).
* **Camada densa:** 128 neurônios com Dropout (0.2).
* **Treinamento:** 10 épocas, otimizador Adam (`lr = 0.001`).

### 2ª Leva: Otimização com Aumento de Neurônios e Data Augmentation
* **Pipeline de Data Augmentation:**
  * Rotações aleatórias (`RandomRotation(0.2)`).
  * Espelhamento horizontal (`RandomFlip("horizontal")`).
  * Zoom aleatório (`RandomZoom(0.2)`).
* **Aumento da Capacidade da Rede:**
  * Camada 1: **256 neurônios** (`relu`) + Dropout (0.3).
  * Camada 2: **128 neurônios** (`relu`) + Dropout (0.3).
* **Treinamento:** 10 épocas, otimizador Adam com taxa refinada (`lr = 0.0005`).

---

## 📊 Resultados e Comparação Experimental

| Métrica | 1ª Leva (Baseline) | 2ª Leva (Otimizada) | Impacto da Otimização |
| :--- | :---: | :---: | :---: |
| **Acurácia Treino (Final)** | **99,56%** | **90,63%** | Eliminação da "decoreba" de dados |
| **Acurácia Validação (Final)** | **88,83%** | **84,88%** | Desempenho mais realista |
| **Melhor Validação (Pico)** | 90,05% (Época 3) | 85,97% (Época 6/7) | Atingida com dados transformados |
| **Perda Validação (Loss)** | 0.4341 (subindo desde época 3) | 0.3478 (no melhor ponto) | Maior estabilidade |
| **Gap (Treino - Validação)** | **~10,7%** ⚠️ | **~5,7%** ✅ | **Redução do Overfitting pela metade!** |

### 🧠 Análise Técnica dos Resultados

1. **Diagnóstico do Overfitting na 1ª Leva:**  
   Na primeira execução, a rede atingiu 99,56% de acurácia no conjunto de treino, enquanto a perda de validação subiu de `0.2915` para `0.4341`. Isso comprovou que a rede começou a memorizar detalhes específicos das fotos de treino em vez de generalizar para novas flores.

2. **Eficácia da Regularização na 2ª Leva:**  
   Com a introdução do *Data Augmentation* e o aumento para dois blocos com Dropout de 0.3, a distância entre a acurácia de treino e validação caiu de **10,7% para 5,7%**. A rede tornou-se significativamente mais robusta contra variações de ângulo, corte e iluminação do mundo real.

3. **Taxa de Convergência:**  
   Na 2ª leva, a curva de treino terminou em ascensão constante (subindo de 0.81 para 0.90 até a época 10), indicando que a rede com 256 neurônios ainda tinha margem de evolução para épocas adicionais (15 a 20 épocas) sem sofrer com sobreajuste precoce.

---

## 🛠️ Tecnologias Utilizadas

* **Linguagem:** Python 3
* **Framework:** TensorFlow / Keras
* **Arquitetura Base:** MobileNetV2 (ImageNet)
* **Visualização:** Matplotlib
* **Ambiente:** Google Colab com suporte a GPU

---

## 🚀 Como Reproduzir no Google Colab

1. Acesse o **Google Colab**.
2. Abra o notebook deste repositório.
3. Certifique-se de que a pasta `dataset/` contenha as duas categorias organizadas.
4. Execute as células em sequência:
   - Carregamento e divisão do dataset.
   - Treinamento da 1ª Leva.
   - Treinamento da 2ª Leva (Otimizada).
   - Plot dos gráficos comparativos de acurácia e perda.

---

## 🔮 Conclusões e Próximos Passos em IoT

* A combinação de **Transfer Learning + Data Augmentation** se provou indispensável para treinar modelos de visão com conjuntos de dados moderados.
* Como evolução futura para sistemas embarcados de IoT, o modelo final pode ser convertido para o formato **TensorFlow Lite (TFLite)** com quantização de pesos (INT8), permitindo que essa rede otimizada rode diretamente em dispositivos de borda como **Raspberry Pi** ou **ESP32-CAM** para identificação de plantas em campo.

---

## 👨‍💻 Autor

Mr Jeffrey

Projeto desenvolvido por **[Jefferson Santos]** como parte da Formação **IoT Specialist** da [DIO](https://dio.me).
