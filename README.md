<h1 align="center">QUESTÕES</h1>

<h2 align="center">Q1 - Importação dos dados e análise das colunas</h2>

```py
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

df = pd.read_csv('dados_produtividade_construcao.csv')
print(df.head())
df.info()
print("Colunas e tipos de dados:")
print(df.dtypes)
```

---

<h2 align="center">Q2 - PERGUNTAS DE PRODUTIVIDADE</h2>

- Quais são as obras mais e menos produtivas?
    - Isso ajuda a entender quais projetos estão performando bem e quais precisam de ajustes.

- A produtividade varia ao longo do tempo?
    - Podemos identificar padrões sazonais ou períodos críticos de baixa produtividade.

- Existe um tipo específico de equipe que tem melhor desempenho?

    - Comparar a produtividade entre equipes pode ajudar na alocação mais eficiente de recursos.

- A produtividade melhora conforme a obra avança?
    - Se sim, isso pode indicar um aprendizado ao longo do projeto. Se não, pode significar problemas operacionais.

- A produtividade de um bloco afeta a produtividade dos outros?
    - Talvez um bloco mais lento cause gargalos para os demais.

- Existe alguma relação entre produtividade e fatores externos (como clima ou atrasos no fornecimento de materiais)?
    - Cruzar dados meteorológicos ou de logística pode revelar correlações inesperadas.

- Qual é o impacto da experiência da equipe na produtividade?
    - Equipes mais experientes realmente entregam mais? Ou treinamento pode compensar experiência?

- A produtividade de um bloco depende da complexidade da tarefa?
    - Talvez blocos que exigem mais precisão tenham produtividade naturalmente menor.

- Há um padrão de produtividade dependendo do dia da semana?
    - Segunda-feira pode ser mais lenta devido ao início da semana, e sexta pode ter quedas por cansaço.

- Se tivéssemos o dobro de orçamento, a produtividade aumentaria proporcionalmente?
    - Isso pode ajudar a calcular até que ponto mais investimento resulta em ganhos reais.
 
---

<h2 align="center">Q3 - Diferença de produtividade entre obras</h2>

```py
print("Obras únicas:")
print(df['nome_obra'].unique())
print("Contagem de apropriações por obra:")
print(df.groupby('nome_obra')['produtividade'].count())
print("Estatísticas descritivas por obra:")
print(df.groupby('nome_obra')['produtividade'].describe())

plt.figure(figsize=(36,10))
sns.boxplot(x='nome_obra', y='produtividade', data=df)
plt.title("Boxplot da Produtividade por Obra")
plt.xlabel("nome_obra")
plt.ylabel("produtividade")
plt.xticks(rotation=57)
plt.show()

# 
#
# 

```
---

<h2 align="center">Q4 - Diferença de produtividade entre blocos (descrição)</h2>

```py
print("Blocos únicos:")
print(df['descricao'].unique())
print("Contagem de apropriações por bloco:")
print(df.groupby('descricao')['produtividade'].count())
print("Estatísticas descritivas por bloco:")
print(df.groupby('descricao')['produtividade'].describe())

plt.figure(figsize=(36,10))
sns.boxplot(x='descricao', y='produtividade', data=df)
plt.title("Boxplot da Produtividade por Bloco")
plt.xlabel("descricao")
plt.ylabel("produtividade")
plt.xticks(rotation=57)
plt.show()
```
---

<h2 align="center">Q5 - Relação entre média e mediana para detecção de outliers</h2>

```py
for categoria in ['nome_obra', 'descricao']:
    print(f"\nMédia e Mediana por {categoria}:")
    print(df.groupby(categoria)['produtividade'].agg(['mean', 'median']))
```
---

<h2 align="center">Q6 - Bloco com produtividade mais e menos previsível (análise de variabilidade)</h2>

```py
variabilidade = df.groupby('descricao')['produtividade'].std()
print("\nDesvio Padrão por Bloco:")
print(variabilidade)
```
---

<h3 align="center">Código completo</h3>

```py
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

df = pd.read_csv('dados_produtividade_construcao.csv')

import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

df = pd.read_csv('dados_produtividade_construcao.csv')

print(df.head())
df.info()
print(df['nome_obra'].unique())
print(df['descricao'].unique())
print(df.groupby('nome_obra')['produtividade'].count())
print(df.groupby('descricao')['produtividade'].count())
print(df.groupby('nome_obra')['produtividade'].describe())
print(df.groupby('descricao')['produtividade'].describe())

plt.figure(figsize=(36,10))
sns.boxplot(x='nome_obra', y='produtividade', data=df)
plt.title("Boxplot da Produtividade por Obra")
plt.xlabel("nome_obra")
plt.ylabel("produtividade")
plt.xticks(rotation=57)
plt.show()

plt.figure(figsize=(36,10))
sns.boxplot(x='descricao', y='produtividade', data=df)
plt.title("Boxplot da Produtividade por Bloco")
plt.xlabel("descricao")
plt.ylabel("produtividade")
plt.xticks(rotation=57)
plt.show()

for categoria in ['nome_obra', 'descricao']:
    print(f"\nMédia e Mediana por {categoria}:")
    print(df.groupby(categoria)['produtividade'].agg(['mean', 'median']))

variabilidade = df.groupby('descricao')['produtividade'].std()
print("\nDesvio Padrão por Bloco:")
print(variabilidade)
```
---

<h2 align="center">Q7 - O coeficiente (SIURB)</h2>

Pelo que entendemos do vídeo, esse coeficiente serve para ajustar os valores do orçamento comparando o custo planejado com o custo real. Parece que, se o custo real for maior que o previsto, o coeficiente diminui, e se for menor, ele aumenta – como um mecanismo para “corrigir” o orçamento. Talvez ele seja calculado dividindo o custo real pelo planejado. A ideia é que, ao aplicar esse coeficiente, o orçamento final se torne mais realista, evitando tanto a subestimação quanto a superestimação dos custos.

---

<h2 align="center">Q8 - DÚVIDAS (SIURB)</h2>

Confesso que o vídeo ficou bem técnico e, pra mim, meio confuso. Fiquei com dúvidas se esse coeficiente leva em conta só os custos dos materiais ou se também considera os custos da mão de obra. Além disso, não ficou claro se ele deve ser calculado para cada obra separadamente ou se é uma média geral para todas as obras. Acho que preciso ver mais exemplos práticos para entender melhor essa parte, porque a explicação misturou vários conceitos de uma forma que me deixou um pouco perdido.

---

<h1 align="center">Desenvolvedores</h1>

| Dev | Avatar | RM |
| ------------- | ------ | ----- |
| ![](https://img.shields.io/badge/DEV-Yuri-70b2b4?style=for-the-badge&logo=github) | <a href="https://github.com/yurisilpess"><img src="https://avatars.githubusercontent.com/u/99032447?v=4" height="50" style="border-radius:30px;"></a> | RM557475 |
| ![](https://img.shields.io/badge/DEV-Igor-7ca787?style=for-the-badge&logo=github) | <a href="https://github.com/igor-soos"><img src="https://avatars.githubusercontent.com/u/164360059?v=4" height="50" style="border-radius:30px;"></a> | RM556010 |
| ![](https://img.shields.io/badge/DEV-Gustavo-516b58?style=for-the-badge&logo=github) | <a href="https://github.com/gus7a2005"><img src="https://avatars.githubusercontent.com/u/161319479?v=4" height="50" style="border-radius:30px;"></a> | RM556289 |
