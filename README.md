

## `README.md`


# AM — Redes Neurais MLP

Projeto desenvolvido em sala de aula na disciplina **Aprendizagem de Máquina**, ministrada pelo professor **Ícaro Augusto Davies do Nascimento**, no curso **Desenvolvimento de Software Multiplataforma (DSM)** da **FATEC**.

O objetivo é estudar **Redes Neurais Multicamada (MLP — Multilayer Perceptron)** na prática, comparando o **Perceptron simples** (linear) com uma **MLP** (não-linear) e, em seguida, reimplementando a mesma solução em **TensorFlow/Keras**, analisando arquiteturas, tempo de treino e persistência de modelos.

## 📚 Conteúdo dos notebooks

### `01_mlp.ipynb` — MLP com scikit-learn

1. **Problema XOR com Perceptron**
   - Como o Perceptron (fronteira linear) **não consegue** separar o XOR → acurácia 50%
   - Visualização da fronteira de decisão em 2D

2. **Mesmo problema com MLPClassifier**
   - `hidden_layer_sizes=(4,)`, `activation="tanh"`, `solver="lbfgs"`
   - Acurácia **100%** no XOR — a MLP aprende uma fronteira **não-linear**
   - Gráfico comparativo das duas fronteiras de decisão

3. **Estudo de arquiteturas no dataset `make_moons`**
   - 1000 amostras, `noise=0.25`, split 70/30 estratificado
   - Comparação de três arquiteturas:

     | Arquitetura | Tempo (s) | Acurácia treino | Acurácia teste | Épocas |
     |---|---|---|---|---|
     | Subdimensionada `(4,)` | 0.2866 | 84.86% | 89.00% | 333 |
     | Equilibrada `(32, 16)` | 0.4132 | 94.29% | **97.33%** | 315 |
     | Hiperdimensionada `(256, 128, 64, 32)` | 0.6424 | 93.71% | 96.67% | 68 |

   - Conclusão prática: **mais neurônios ≠ melhor** — a arquitetura equilibrada generaliza melhor e treina mais rápido.

4. **Persistência do modelo com `joblib`**
   - Salva em `models/mlp_moons.joblib`
   - Recarrega em "nova versão" e simula inferência em produção com 2 amostras novas, exibindo classe prevista e probabilidade

### `02_tensorflow.ipynb` — MLP com TensorFlow/Keras

1. **Mesma arquitetura** `(32, 16)` em `tf.keras.Sequential`
   - `Dense(32, relu)` → `Dense(16, relu)` → `Dense(1, sigmoid)`
   - Total de **641 parâmetros**

2. **Compilação e treino**
   - Otimizador `Adam(learning_rate=0.01)`
   - Loss `binary_crossentropy`
   - 400 épocas, `batch_size=32`, validação explícita no conjunto de teste
   - **Acurácia final: 95.33%**

3. **Análise da curva de perda** (training vs validation loss)
   - Visualização da evolução ao longo das épocas

4. **Persistência e inferência**
   - Salva em `models/modelo_mlp_tensorflow.keras`
   - Recarrega com `tf.keras.models.load_model`
   - Simula requisições de API, retornando **classe prevista** e **probabilidade** (`>= 0.5`)

## 🛠️ Tecnologias

- **Python** (versão definida em `.python-version`)
- **uv** — gerenciador de dependências e ambientes
- **JupyterLab** — ambiente de notebooks
- **NumPy** e **pandas** — manipulação de dados
- **Matplotlib** — visualizações (fronteiras de decisão, curvas de perda)
- **scikit-learn** — `Perceptron`, `MLPClassifier`, `make_moons`, `train_test_split`, `accuracy_score`
- **TensorFlow / Keras** — implementação equivalente em framework
- **joblib** — serialização de modelos scikit-learn

## ⚙️ Como executar

### Pré-requisitos

- [uv](https://docs.astral.sh/uv/) instalado
- Python (a versão é gerenciada pelo `uv` via `.python-version`)

### Passos

```bash
# 1. Clone o repositório
git clone https://github.com/Leandro-dsm/am-redes-neurais-mpl.git
cd am-redes-neurais-mpl

# 2. Instale as dependências (cria o .venv automaticamente)
uv sync

# 3. Abra o JupyterLab
uv run jupyter lab
```

Depois execute os notebooks na ordem: `01_mlp.ipynb` → `02_tensorflow.ipynb`.

> ⚠️ A pasta `models/` não é versionada (ver `.gitignore`). Os arquivos `.joblib` e `.keras` são gerados ao executar os notebooks localmente.

## 📖 O que é uma MLP?

Uma **Multilayer Perceptron** é uma rede neural **feedforward** com pelo menos uma camada oculta. Ela resolve o que um Perceptron simples não resolve: problemas **não-linearmente separáveis** como o XOR e o `make_moons`.

- **Camadas ocultas** aplicam transformações lineares seguidas de funções de ativação não-lineares (ReLU, tanh...)
- **Backpropagation** calcula os gradientes e um otimizador (lbfgs, Adam...) ajusta os pesos
- A **fronteira de decisão** deixa de ser uma reta e passa a ser uma curva arbitrária

A principal lição prática do projeto: **arquitetura importa**. Redes pequenas demais não aprendem; redes grandes demais memorizam e gastam mais tempo de treino.

## 👨‍🏫 Créditos

- **Professor:** Ícaro Augusto Davies do Nascimento
- **Disciplina:** Aprendizagem de Máquina
- **Aluno:** Leandro Sabino Sueoka
- **Instituição:** FATEC — DSM

## 📄 Licença

Projeto de fins educacionais.
