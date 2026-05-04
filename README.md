# Tech Challenge — Fase 1 (FIAP) — Grupo 88

Projeto de **Machine Learning supervisionado** aplicado à **detecção de câncer do colo do útero** a partir de fatores de risco clínicos. Desenvolve um classificador binário capaz de apoiar a triagem oncológica, indicando pacientes com maior probabilidade de biópsia positiva.

> **Aviso clínico:** este é um trabalho acadêmico. O modelo é uma **ferramenta de apoio** e **não substitui** a avaliação médica. A palavra final no diagnóstico é sempre do(a) profissional de saúde.

---

## Sumário

- [Sobre o projeto](#sobre-o-projeto)
- [Estrutura do repositório](#estrutura-do-repositório)
- [Dataset](#dataset)
- [Como executar](#como-executar)
  - [Opção 1 — Google Colab (recomendado)](#opção-1--google-colab-recomendado)
  - [Opção 2 — Localmente com `venv`](#opção-2--localmente-com-venv)
- [Pipeline do notebook](#pipeline-do-notebook)
- [Resultados](#resultados)
- [Relatório técnico](#relatório-técnico)
- [Equipe](#equipe)

---

## Sobre o projeto

- **Problema:** classificação binária (`Biopsy` 0/1) usando fatores de risco demográficos, hábitos de vida e histórico clínico.
- **Dataset:** `kag_risk_factors_cervical_cancer.csv` (UCI ML Repository — 858 registros, 36 atributos).
- **Modelos avaliados:** Regressão Logística, Random Forest, **XGBoost** (selecionado), KNN e SVM.
- **Métrica primária:** *recall* da classe positiva (sensibilidade clínica), complementada por AUC, F1 e *precision*.

---

## Estrutura do repositório

```
.
├── TECH_CHALLENGE.ipynb              # Notebook principal (pipeline completo)
├── kag_risk_factors_cervical_cancer.csv  # Dataset
├── requirements.txt                  # Dependências Python
├── LINK.txt                          # Link original do dataset (UCI)
└── README.md                         # Este arquivo
```

---

## Dataset

**Cervical Cancer (Risk Factors) Data Set** — UCI Machine Learning Repository.

- **Origem:** Hospital Universitario de Caracas, Venezuela.
- **Registros:** 858 pacientes.
- **Atributos:** 36 (demografia, tabagismo, contracepção, DSTs, diagnósticos prévios).
- **Variáveis-alvo disponíveis:** `Hinselmann`, `Schiller`, `Citology`, `Biopsy`.
- **Variável-alvo escolhida:** **`Biopsy`** (padrão-ouro para diagnóstico definitivo).
- **Link original:** <https://archive.ics.uci.edu/dataset/383/cervical+cancer+risk+factors>

O CSV já vem incluído neste repositório.

---

## Como executar

### Opção 1 — Google Colab (recomendado)

1. Acesse <https://colab.research.google.com>.
2. Em **File → Open Notebook → GitHub**, cole a URL deste repositório:
   ```
   https://github.com/gilbertoag2007/fiap-tech-challenge-fase1
   ```
3. Selecione o arquivo **`TECH_CHALLENGE.ipynb`**.
4. No menu, clique em **Runtime → Run all** (ou `Ctrl+F9`).

> O notebook carrega o CSV diretamente de uma URL pública (raw GitHub), portanto **não é necessário fazer upload** do dataset manualmente. Todas as bibliotecas usadas (pandas, scikit-learn, xgboost, imbalanced-learn) já estão pré-instaladas no Colab.

**Tempo estimado de execução:** 3 a 4 minutos.

---

### Opção 2 — Localmente com `venv`

#### Pré-requisitos

- Python **3.10+**
- `pip` e `venv` instalados

#### Passo a passo

```bash
# 1. Clonar o repositório
git clone https://github.com/gilbertoag2007/fiap-tech-challenge-fase1.git
cd fiap-tech-challenge-fase1

# 2. Criar e ativar o ambiente virtual
python3 -m venv .venv

# Linux / macOS:
source .venv/bin/activate

# Windows (PowerShell):
.venv\Scripts\Activate.ps1

# 3. Instalar dependências
pip install --upgrade pip
pip install -r requirements.txt
pip install jupyter

# 4. Iniciar o Jupyter
jupyter notebook TECH_CHALLENGE.ipynb
```

Abra o notebook no navegador e execute as células sequencialmente (**Cell → Run All**).

#### Observação para usuários locais

A primeira célula importa `from google.colab import drive`. Esse import é específico do Google Colab e **falhará localmente**. Para executar local, basta **comentar essa linha** — todas as demais funcionalidades funcionam normalmente. Substituir por:

```python
# from google.colab import drive   # comentado para execução local
```

---

## Pipeline do notebook

O notebook está organizado nas seguintes etapas:

1. **Configuração** — importação de bibliotecas.
2. **Pré-processamento inicial** — carregamento, remoção de duplicatas, conversão de tipos, tratamento de `?` → `NaN`.
3. **Análise Exploratória (EDA)** — `describe()`, balanceamento, boxplots, histogramas, **heatmap de correlação**.
4. **Pré-processamento refinado** — detecção de *outliers* (IQR e Z-Score), imputação por regra de negócio + `SimpleImputer`.
5. **Divisão treino/teste** — 70 / 30 estratificado.
6. **Treinamento dos modelos** — Logística, Random Forest, XGBoost, KNN, SVM (com `GridSearchCV` e `class_weight`).
7. **Avaliação** — Curva ROC, comparativo final com AUC, *recall*, F1, *precision*.
8. **Conclusão** — escolha do modelo final e justificativa.

---

## Resultados

| Modelo                  | AUC Teste | Recall (1) | F1 (1) | Precision (1) |
|-------------------------|:---------:|:----------:|:------:|:-------------:|
| **XGBoost** ⭐          | **0,937** | **0,88**   | 0,72   | 0,61          |
| Regressão Logística     | 0,917     | 0,88       | 0,72   | 0,61          |
| Random Forest           | 0,914     | 0,88       | 0,70   | 0,58          |
| SVM                     | 0,864     | 0,62       | —      | —             |
| KNN                     | 0,796     | 0,12       | 0,20   | 0,50          |

**Modelo selecionado:** XGBoost — maior AUC, maior estabilidade em *cross-validation* (±0,02) e *recall* de 0,88 (detecta 14 dos 16 casos positivos no conjunto de teste).

---

## Equipe

**Grupo 88** — FIAP Pós-Tech em IA para Devs (Fase 1).
