# Wine Quality Classification — Tech Challenge Fase 2

**FIAP Pós Tech Data Analytics · Machine Learning Applied to Business · Grupo 75**

Modelos de machine learning que preveem se um vinho é de **alta qualidade** a partir das suas características físico-químicas medidas durante a produção.

🔗 **Repositório original do grupo:** https://github.com/grupo-postech-fiap/TechChallenge_FIAP-Fase2

---

## Problema

A avaliação de qualidade de vinhos costuma ser sensorial, subjetiva e demorada. O objetivo deste projeto é usar dados de produção para apoiar enólogos na tomada de decisão.

A nota de qualidade foi transformada em uma **classificação binária**:
- **1 – Alta Qualidade:** nota ≥ 7
- **0 – Baixa/Média Qualidade:** nota < 7

## Dataset

- **Fonte:** [Wine Quality Dataset – Kaggle](https://www.kaggle.com/datasets/yasserh/wine-quality-dataset)
- **Registros:** 1.143 amostras, sem dados faltantes
- **Features:** 11 variáveis físico-químicas (acidez fixa, acidez volátil, ácido cítrico, açúcar residual, cloretos, SO₂ livre e total, densidade, pH, sulfatos e teor alcoólico)
- **Desbalanceamento:** ~86% Baixa/Média vs. ~14% Alta, ou **6,19 : 1**

## Estrutura do repositório

```
├── data/                 # Dataset original e conjuntos de treino/teste
├── notebooks/
│   └── code_v2.ipynb     # Notebook principal: EDA, pré-processamento, modelos e avaliação
├── results/              # Gráficos gerados (GF01–GF13)
├── src/                  # Reservado para scripts auxiliares
├── requirements.txt
└── README.md
```

## Pipeline

1. **Compreensão do problema:** definição e binarização da variável alvo
2. **EDA:** distribuições, outliers, assimetria e correlações
3. **Pré-processamento:** padronização com `StandardScaler` e divisão treino/teste 80/20 estratificada
4. **Balanceamento:** SMOTE aplicado **somente no conjunto de treino**
5. **Modelagem:** Regressão Logística (baseline), Random Forest, Gradient Boosting e KNN
6. **Avaliação:** Precisão, Recall, F1-Score e AUC-ROC, métricas adequadas a classes desbalanceadas
7. **Interpretação:** importância das variáveis e implicações para a produção

## Correlações

![Matriz de correlação](results/GF06.png)

As variáveis mais correlacionadas com `quality_label` são **alcohol (+0,40)**, **volatile acidity (−0,30)**, **citric acid (+0,25)** e **sulphates (+0,21)**.

## Resultados

| Modelo | Precisão | Recall | F1-Score | AUC-ROC |
|---|---|---|---|---|
| Regressão Logística | 0,37 | 0,71 | 0,49 | 0,86 |
| Random Forest | 0,59 | 0,77 | 0,67 | **0,94** |
| **Gradient Boosting** | **0,65** | 0,74 | **0,69** | 0,93 |
| KNN | 0,42 | **0,80** | 0,55 | 0,87 |

![Matrizes de confusão](results/GF09.png)

- **Gradient Boosting** teve o melhor equilíbrio geral (maior F1 e maior precisão).
- **Random Forest** é a alternativa quando a prioridade é separar as classes (maior AUC-ROC).
- Os modelos baseados em árvores superaram os lineares e de distância, porque captam relações não lineares entre as variáveis.

## Variáveis de maior influência

![Importância das features – Random Forest](results/GF11.png)

**Teor alcoólico**, **acidez volátil** e **sulfatos** são as variáveis mais importantes para os modelos de árvore, seguidas por **ácido cítrico**.

### Implicações para a produção
- **Fermentação (álcool):** colher a uva no ponto ideal de maturação e controlar a temperatura de fermentação para atingir o teor alcoólico desejado.
- **Higiene e oxigênio (acidez volátil):** manter controle sanitário rigoroso e limitar a exposição ao oxigênio, usando SO₂ como antioxidante, para evitar bactérias acéticas.
- **Frescor e equilíbrio (ácido cítrico):** monitorar e, quando a regulamentação permitir, corrigir lotes com baixa vivacidade.

## Como executar

```bash
git clone https://github.com/ThiOliveira86/TC_FIAP-Fase2.git
cd TC_FIAP-Fase2
pip install -r requirements.txt
jupyter notebook notebooks/code_v2.ipynb
```

## Melhorias futuras

- Encapsular o pré-processamento em um `Pipeline` do scikit-learn, ajustando a padronização apenas no conjunto de treino.
- Aplicar validação cruzada estratificada com ajuste de hiperparâmetros (`GridSearchCV`) para métricas ainda mais robustas.

## Integrantes – Grupo 75

- Alexandre Bueno
- Alexandre Francisco
- Brenda Barbosa
- João Victor dos Santos
- Thiego de Oliveira
