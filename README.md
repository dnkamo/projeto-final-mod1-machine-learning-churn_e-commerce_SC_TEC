# Previsão de Churn em E-commerce

Este projeto foi desenvolvido como atividade final do Módulo 1 do curso SC TEC. O objetivo é criar e avaliar modelos preditivos de Machine Learning (KNN e Árvore de Decisão) para prever quais clientes têm chance de abandonar a plataforma de e-commerce (*Churn*), ajudando a empresa a tomar ações de marketing e retenção antes que o cliente saia.

## Sobre a Base de Dados
O conjunto de dados utilizado neste projeto simula o histórico operacional e comportamental de clientes de uma plataforma de e-commerce global. A base original contém dados cadastrais, métricas de engajamento (como tempo gasto no aplicativo e dispositivos registrados), histórico financeiro (compras e cashback recebido) e interações de suporte (como reclamações registradas). A variável alvo é a coluna **Churn**, que mapeia o abandono da plataforma.

## Objetivo
O objetivo principal deste projeto é construir, avaliar e comparar modelos preditivos de Machine Learning (KNN e Árvore de Decisão) capazes de antecipar quais clientes estão propensos a deixar a plataforma (*Churn*). Com essas previsões de alta confiabilidade, a equipe de marketing e retenção pode disparar ações preventivas direcionadas, minimizando a perda de receita operacional e otimizando a aquisição e retenção de Clientes.

---

## Dicionário de Dados (Atualizado)

O conjunto de dados original foi enriquecido e atualizado na fase de Engenharia de Features com a criação de uma nova métrica:

| Atributo | Tipo de Dado | Descrição |
| :--- | :--- | :--- |
| **Churn** | Binário (0/1) | Variável Alvo. Indica se o cliente saiu da plataforma (1) ou continuou ativo (0). |
| **Tenure** | Numérico | Tempo que o cliente está na plataforma (em meses). |
| **PreferredLoginDevice** | Categórico | Dispositivo de login mais usado (Ex: Celular, Computador). |
| **CityTier** | Categórico | Classificação do nível da cidade do cliente. |
| **WarehouseToHome** | Numérico | Distância entre o armazém do e-commerce e a casa do cliente. |
| **Gender** | Categórico | Gênero do cliente. |
| **HourSpendOnApp** | Numérico | Quantidade de horas que o cliente passa no aplicativo. |
| **NumberOfDeviceRegistered**| Numérico | Quantidade de dispositivos registrados pelo cliente. |
| **PreferedOrderCat** | Categórico | Categoria de produto mais pedida pelo cliente (Ex: Eletrônicos, Celulares). |
| **SatisfactionScore** | Numérico | Nota de satisfação do cliente com a plataforma (de 1 a 5). |
| **MaritalStatus** | Categórico | Estado civil do cliente (Casado, Solteiro). |
| **NumberOfAddress** | Numérico | Quantidade de endereços cadastrados pelo cliente. |
| **Complain** | Binário (0/1) | Indica se o cliente fez alguma reclamação recente (1) ou não (0). |
| **OrderAmountHikeFromlastYear**| Numérico | Porcentagem de aumento no valor gasto em compras comparado ao ano anterior. |
| **OrderCount** | Numérico | Quantidade total de pedidos feitos pelo cliente. |
| **DaySinceLastOrder** | Numérico | Quantidade de dias desde o último pedido do cliente. |
| **CashbackAmount** | Numérico | Valor total acumulado de cashback pelo cliente. |
| **cashback_por_pedido** | Numérico | **(Nova Feature)** Média de cashback por pedido feito (`CashbackAmount / OrderCount`). |

---

## Tecnologias Utilizadas
* **Linguagem:** Python 3
* **Manipulação de Dados:** Pandas, NumPy
* **Visualização:** Matplotlib, Seaborn
* **Machine Learning & Pré-processamento:** Scikit-Learn (Sklearn)
* **Balanceamento de Dados:** Imbalanced-Learn (SMOTE)

---


## Estrutura do Projeto
A organização dos arquivos no repositório segue a estrutura abaixo:
```text
├── dataset/
│   └── E Commerce Dataset.xlsx - E Comm.csv
├── notebook/
│   └── SC_TEC_Proj_Final_Mod1_E_commerce_Churn.ipynb
├── README.md
└── requirements.txt
```

---

## Etapas do Projeto Técnico

O pipeline do projeto foi estruturado de ponta a ponta seguindo as boas práticas da Ciência de Dados:

1. **Análise Exploratória (EDA):** Análise estatística dos dados, criação de gráficos de distribuições (Histogramas, Boxplots) e Heatmap de correlação linear (Pearson).
2. **Tratamento de Dados:** Preenchimento de valores ausentes por meio da Mediana e mitigação do impacto de outliers nas colunas `OrderCount` e `CashbackAmount` utilizando a técnica de Clipping via IQR.
3. **Engenharia de Features:** Criação da nova coluna `cashback_por_pedido`.
4. **Preparação dos Dados:** Separação estratificada de treino e teste (80/20) para evitar vazamento de dados (*Data Leakage*), transformação de texto em número através do *One-Hot Encoding* e balanceamento das classes de treino utilizando o algoritmo **SMOTE**.
5. **Modelagem e Escalonamento:** Aplicação do `StandardScaler` restrita ao algoritmo KNN (baseado em distâncias). O modelo de Árvore de Decisão foi treinado intencionalmente sem escalonamento, uma vez que suas partições matemáticas funcionam através de cortes monotônicos e não sofrem impacto da escala global dos dados.

---

## Resumo dos Resultados

* **KNN (K=5):** Conseguiu o maior **Recall (91.58%)** para identificar os clientes que de fato iam sair, gerando apenas 16 Falsos Negativos. É o modelo ideal para cenários onde a prioridade máxima é não perder o cliente.
* **Árvore de Decisão (Profundidade=9):** Teve a maior **Acurácia Global (90.76%)** e maior **Precisão (69.37%)**, reduzindo os Falsos Positivos para apenas 68 casos. É o modelo ideal para cenários com orçamentos restritos de retenção de marketing.

---

## Veredito do Negócio & Recomendação Estratégica

A escolha do modelo ideal deve ser guiada pelos objetivos operacionais e financeiros da empresa:

* **Estratégia Defensiva (Recomendado: KNN)**
  * **Foco:** Maximizar a retenção e evitar a perda de receita.
  * **Justificativa:** Com um **Recall de 91,58%**, o KNN falha em detectar apenas 16 clientes propensos ao *churn*. É a melhor opção quando o custo de perder um cliente (CAC para repor) é superior ao custo de enviar uma oferta de retenção.

* **Estratégia Eficiente (Recomendado: Árvore de Decisão)**
  * **Foco:** Otimização de orçamento e campanhas direcionadas.
  * **Justificativa:** Com **90,67% de Acurácia** e **69,59% de Precisão**, este modelo reduz drasticamente os Falsos Positivos (apenas 66 casos). É a escolha ideal caso a empresa tenha um orçamento limitado para cupons ou descontos e precise garantir que apenas clientes em risco real sejam impactados.

**Recomendação de Implementação:** Utilizar o **KNN** como modelo principal no pipeline de produção para alertas preventivos, aplicando a **Árvore de Decisão** em campanhas específicas onde o custo por ação de marketing for elevado.

---

## Como Executar o Projeto

Siga o passo a passo abaixo para configurar o seu ambiente e rodar o arquivo `.ipynb` localmente.

### 1. Clonar o Repositório
Abra o terminal e execute os comandos abaixo para baixar o projeto e acessar o diretório:

```bash
git clone https://github.com/dnkamo/projeto-final-mod1-machine-learning-churn_e-commerce_SC_TEC.git
```
### 2. Acessar a Pasta do Projeto
Navegue até o diretório criado pelo Git:

```bash
cd projeto-final-mod1-machine-learning-churn_e-commerce_SC_TEC
```

### 3. Instalar as Dependências
Instale todas as bibliotecas de Machine Learning e manipulação de dados contidas no arquivo `requirements.txt`:

```bash
pip install -r requirements.txt
```

### Opção A: Executar no VS Code (com Extensão Jupyter)

1. Abra o VS Code e certifique-se de ter a extensão oficial [Jupyter](https://marketplace.visualstudio.com/items?itemName=ms-toolsai.jupyter) instalada.

2. Abra a pasta do projeto clonado no VS Code (File > Open Folder), e selecione o arquivo `.ipynb`.

3. No canto superior direito da tela do notebook aberto, clique em **Select Kernel** (Selecionar Kernel) e escolha o interpretador Python onde você instalou as dependências no passo 2.

4. Clique em **Run All** (Executar Tudo) na barra de ferramentas superior ou execute célula por célula em ordem sequencial.

### Opção B: Executando no Jupyter Notebook Tradicional

1. No seu terminal de comando, certifique-se de estar na pasta do projeto e inicie o servidor local executando:
```bash
jupyter notebook
```

2. Uma aba será aberta automaticamente no seu navegador web exibindo a árvore de diretórios do projeto.

3. Selecione o arquivo com a extensão `.ipynb` para abrir o ambiente interativo.

4. No menu superior da página do Jupyter, clique em Cell > Run All para executar todos os blocos sequencialmente, ou execute célula por célula em ordem sequencial.


# Conclusão

Este projeto demonstra como o Machine Learning transforma dados brutos em estratégias eficientes de retenção de clientes. O sucesso dos modelos (Acurácia e Recall acima de 90%) destaca o impacto de três pilares fundamentais:

## 1. A Importância do Pipeline de Dados
O desempenho dos algoritmos foi garantido pela preparação correta dos dados, livre de vazamentos (*Data Leakage*):
* **Tratamento Limpo:** O uso da mediana para dados nulos e o ajuste de outliers protegeram os modelos contra distorções.
* **Ordem Correta:** Separar o treino e teste antes de balancear as classes (SMOTE) garantiu resultados realistas.
* **Uso Inteligente:** O escalonamento foi aplicado apenas ao KNN, respeitando a lógica matemática de cada algoritmo.

## 2. Modelos Alinhados ao Negócio
A escolha do melhor modelo depende da estratégia financeira da empresa:
* **Foco em Prevenção (KNN):** Alcançou **91.58% de Recall**, ideal para quando o e-commerce quer evitar a perda de receita a qualquer custo.
* **Foco em Precisão (Árvore de Decisão):** Alcançou **90.76% de Acurácia**, ideal para campanhas com orçamento restrito, evitando gastos com clientes que não iriam embora.

## 3. Valor Prático
Adicionar novas métricas (como o cashback médio por pedido) permitiu ao modelo encontrar padrões ocultos de comportamento. Em vez de reagir após a perda do cliente, a plataforma ganha a capacidade de agir de forma proativa, protegendo o faturamento e melhorando a relação com o consumidor.