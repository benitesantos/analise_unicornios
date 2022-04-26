<img src = 'https://www.boqnews.com/wp-content/uploads/2022/01/unicor-e1641908034152.png' width = '210' height= '140' >

# Análise de empresas Unicórnios.
Analise das maiores startaps de capital fechado do mundo que chegaram a unicórnio, ou seja seu faturamento passou de 1 bilhão. 

## Requisitos:
- Ter o jupyter notebook instalado.
- Ter o arquivo Startups in 2021 end.csv na mesma pasta do arquivo
Projeto 01 unicórnios.ipynb

## Pacotes instalados:
- Pandas
``` bash
pip install pandas
```
- Matplotlib
``` bash
pip install matplotlib
```
- Seaborn
``` bash
pip install seaborn
```
- Numpy
``` bash
pip install numpy
```

## Processo das ferramentas que foram usadas:
Código que importa a tabela em csv e trás para o python.
```python
df = pd.read_csv('Startups in 2021 end.csv')
```
Código que vê o tamanho total da tabela , suas linhas e colunas.

``` python
df.shape
```
Mostra as primeiras 5 linhas da tabela.
``` python
df.head()
```
Visualiza em forma de lista o nome da cada coluna na tabela.
``` python
df.columns
```
Renomeia as colunas da tabela.
``` python
df.rename(columns={
    'Unnamed: 0': 'Id',
    'Company':'Empresa',
    'Valuation ($B)':'Valor ($)',
    'Date Joined':'Data de Adesão',
    'Country':'Pais',
    'City':'Cidade',
    'Industry':'Setor',
    'Select Investors':'Investidores'
    
},inplace=True)
```
Visualiza o tipo dos dados na tabela e valores nulos.

``` python
df.info()
```
Plota um gráfico de mapa de calor
``` python
plt.figure(figsize=(15,6))
plt.title('Analisando Campos Nulos')
sns.heatmap(df.isnull());
```
Visualiza campos únicos da tabela
``` python
df.nunique()
```
Visualiza campos únicos da tabela na coluna setor.
``` python
df['Setor'].unique()
```

Visualiza os campos unicos da tabela na coluna setor e conta a quantidade de vezes que aparece, com o normalize aparece em formato de porcentagem.
- Com essa pequena análise vemos que as fintecs são a maioria nas startaps.

``` python
df['Setor'].value_counts(normalize=True)
```

Plota um gráfico de barras.
``` python
plt.figure(figsize=(15,6))
plt.title('Analise dos Setores')
plt.bar(df['Setor'].value_counts().index,df['Setor'].value_counts());
plt.xticks(rotation=45,ha='right');
```

Criou-se uma variavel onde filtra a coluna país por porcentagem.

``` python
analise =round( df['Pais'].value_counts(normalize=True)*100,1)
```
Plota um gráfico de pizza.

``` python
plt.figure(figsize=(15,6))
plt.title('Análise dos Paises Geradores de unicórnios.')
plt.pie(
    analise,
    labels = analise.index,
    shadow=True,
    startangle=90,
    autopct='%1.1f%%');
```
Plota um gráfico de pizza, só que mostrando os 10 primeiros na lista.

```python
plt.figure(figsize=(15,6))
plt.title('Análise dos Paises Geradores de unicórnios.')
plt.pie(
    analise.head(10),
    labels = analise.index[0:10],
    shadow=True,
    startangle=90,
    autopct='%1.1f%%');
```
Mostra as primeiras 10 linhas do filtro da variável analise.
``` python
analise.head(10)
```
Converte a coluna para o tipo data.
```python
df['Data de Adesão']= pd.to_datetime(df['Data de Adesão'])

df['Data de Adesão'].head()
```
Cria-se mais 2 colunas para a extração do mês e o ano.
```python
df['Mes'] = pd.DatetimeIndex(df['Data de Adesão']).month
df['Ano'] = pd.DatetimeIndex(df['Data de Adesão']).year

df.head()
```
Cria-se uma tabela filtrada por país ,ano , mes e empresa.
```python
tb_grup = df.groupby(by=['Pais','Ano','Mes','Empresa']).count()['Id'].reset_index()
tb_grup
```
Código para achar valores mais especificos na tabela, como por exemplo uma páis a ser buscado.
- Nesta pequena filtragem percebemos que a nubank foi a primeira a atingir o unicórnio aqui no Brasil em 2018.

``` python
tb_grup.loc[
    tb_grup['Pais']== 'Brazil'
]
```
Código para transforma a coluna em um valor flutuante , float.
``` python
df['Valor ($)']= pd.to_numeric(df['Valor ($)'].apply(lambda linha: linha.replace('$','')))
df.head()
```
Cria-se uma nova tabela filtrada , só que desta vez por país e valor.
```python
analise_valor = analise_pais.sort_values('Valor ($)',ascending=False)
analise_valor.head()
```
Plota a nova tabela filtrada em um gráfico. para uma melhor visualização.
```python
plt.figure(figsize=(15,6))
plt.plot(analise_valor['Pais'],analise_valor['Valor ($)']);
plt.xticks(rotation=45,ha='right');
```
# Aprendizado:
- transformar toda uma coluna em tipo data.
- tranformar toda uma coluna em valor flutuante.

# Dificuldades do projeto:
- Filtrar determinados valores, unicos e especificos.