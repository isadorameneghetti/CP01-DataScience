## Análise de Dados de Homicídios por Estado
![Banner](Banner/banner.jpeg)

# ☠️ Homicídio por Estados Populosos ☠️

Este projeto tem como objetivo analisar dados de homicídios nos estados de São Paulo (SP), Rio de Janeiro (RJ), Minas Gerais (MG) e Bahia (BA) ao longo dos anos. A análise é realizada utilizando a linguagem Python, com as bibliotecas Pandas para manipulação de dados e Matplotlib para visualização gráfica. O projeto busca responder a diversas perguntas relacionadas à evolução dos homicídios nesses estados, como tendências ao longo dos anos, comparações entre estados e variações no número de homicídios.

## Dados

Os dados utilizados neste projeto foram obtidos de um arquivo CSV contendo informações sobre homicídios por estado ao longo dos anos. O arquivo contém colunas como `período` (ano), `nome` (estado), e `valor` (número de homicídios).

## Obtenção dos Dados

Os dados foram carregados a partir de um arquivo CSV chamado `homicidios_estado.csv`, utilizando a biblioteca Pandas. O arquivo foi lido com o separador `;` para garantir a correta interpretação dos dados.

```python
import pandas as pd
df = pd.read_csv('homicidios_estado.csv', sep=';')
```

## Tratamento dos Dados

Após a leitura dos dados, foi realizada uma limpeza e preparação dos dados para análise. A coluna `nome` foi renomeada para `estado` e os valores foram convertidos para strings, removendo espaços extras. Além disso, os dados foram filtrados para incluir apenas os estados de SP, RJ, MG e BA.

```python
df['estado'] = df['nome'].astype(str).str.strip()
estados_selecionados = ['SP', 'RJ', 'MG', 'BA']
df_filtrado = df[df['estado'].isin(estados_selecionados)]
```

## Análise

### Tendência de Homicídios ao Longo dos Anos

A primeira análise realizada foi a tendência de homicídios ao longo dos anos nos estados selecionados. Para isso, os dados foram agrupados por ano e estado, e o total de homicídios foi somado. O gráfico abaixo mostra a evolução dos homicídios em SP, RJ, MG e BA ao longo dos anos.

```python
import matplotlib.pyplot as plt

homicidios_ano_estado = df_filtrado.groupby(['período', 'estado'])['valor'].sum().unstack()

plt.figure(figsize=(12, 6))
for estado in estados_selecionados:
    plt.plot(homicidios_ano_estado.index, homicidios_ano_estado[estado], marker='o', label=estado)
plt.title('Evolução dos Homicídios ao Longo dos Anos (SP, RJ, MG, BA)')
plt.xlabel('Ano')
plt.ylabel('Total de Homicídios')
plt.legend()
plt.grid(True)
plt.show()
```

### Estados com Maior Número de Homicídios no Ano Mais Recente

A segunda análise identificou os estados com o maior número de homicídios no ano mais recente disponível no conjunto de dados. Para isso, os dados foram filtrados para o último ano registrado e agrupados por estado.

```python
ano_recente = df['período'].max()
df_recente = df[(df['período'] == ano_recente) & (df['estado'].isin(estados_selecionados))]
homicidios_recente = df_recente.groupby('estado')['valor'].sum()

print(f"\nHomicídios no ano {ano_recente}:")
print(homicidios_recente.sort_values(ascending=False))
```

### Média de Homicídios por Ano

A terceira análise calculou a média de homicídios por ano em cada estado. Isso permite comparar a média anual de homicídios entre os estados selecionados.

```python
media_homicidios_estado = df_filtrado.groupby('estado')['valor'].mean()

print("\nMédia de homicídios por ano:")
print(media_homicidios_estado)
```

### Variação no Número de Homicídios entre 2010 e 2020

A quarta análise comparou o número de homicídios entre os anos de 2010 e 2020 para identificar quais estados tiveram aumento ou redução no número de homicídios.

```python
homicidios_2010 = df[(df['período'] == 2010) & (df['estado'].isin(estados_selecionados))]
homicidios_2020 = df[(df['período'] == 2020) & (df['estado'].isin(estados_selecionados))]

homicidios_2010 = homicidios_2010.groupby('estado')['valor'].sum()
homicidios_2020 = homicidios_2020.groupby('estado')['valor'].sum()

comparacao = pd.DataFrame({'2010': homicidios_2010, '2020': homicidios_2020})
comparacao['diferenca'] = comparacao['2020'] - comparacao['2010']

print("\nDiferença no número de homicídios entre 2010 e 2020:")
print(comparacao[['diferenca']].sort_values(by='diferenca', ascending=False))
```

### Variação no Número de Homicídios ao Longo dos Anos

A quinta análise calculou a variação no número de homicídios ao longo dos anos para cada estado, identificando a diferença entre o máximo e o mínimo número de homicídios registrados.

```python
variacao_homicidios = df_filtrado.groupby('estado')['valor'].agg(['max', 'min'])
variacao_homicidios['variacao'] = variacao_homicidios['max'] - variacao_homicidios['min']

print("\nVariação no número de homicídios ao longo dos anos:")
print(variacao_homicidios[['variacao']].sort_values(by='variacao', ascending=False))
```

### Média Mensal de Homicídios por Estado

A sexta análise calculou a média mensal de homicídios por estado ao longo dos anos. Isso permite identificar quais estados têm a maior média de homicídios por mês.

```python
df['mes'] = df['período']  # Considerando o ano como base para as análises mensais
homicidios_mensais = df[df['estado'].isin(estados_selecionados)].groupby(['estado', 'mes'])['valor'].sum().unstack()
media_mensal_estado = homicidios_mensais.mean(axis=1)

print("\nMédia mensal de homicídios por estado:")
print(media_mensal_estado.sort_values(ascending=False))
```


# Execução

Para executar este projeto, siga os passos abaixo:

1. **Instalação das dependências**: Certifique-se de ter Python instalado. Em seguida, instale as bibliotecas necessárias executando:

   ```sh
   pip install pandas matplotlib
   ```

2. **Execução do script**: Execute o script Python que contém o código de análise:

   ```sh
   python analise_homicidios.py
   ```

3. **Visualização dos resultados**: Os gráficos serão exibidos automaticamente, e os resultados das análises serão impressos no console.


# Licença

The MIT License (MIT) 2023 - Seu Nome. Leia o arquivo [LICENSE.md](https://github.com/seu-usuario/seu-repositorio/blob/master/LICENSE.md) para mais detalhes.
