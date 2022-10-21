### Cálculo de taxa de crescimento médio de casos confirmados no Brasil

### Mortes

```python
fig = go.Figure()

fig.add_trace(
    go.Scatter(x=brasil.observationdate, y=brasil.deaths, name='Mortes',
              mode='lines+markers', line={'color': 'red'})
)

# Layout
fig.update_layout(title='Mortes por COVID-19 no Brasil')

fig.show()
```

### Taxa de crescimento

taxa_crescimento = (presente/passado)**(1/n) - 1

```python
def taxa_crescimento(data, variable, data_inicio=None, data_fim=None):
    # Se data início for None, define como a primeira data disponível
    if data_inicio == None:
        data_inicio = data.observationdate.loc[data[variable] > 0].min()
    else:
        data_inicio = pd.to_datetime(data_inicio)
        
    if data_fim == None:
        data_fim = data.observationdate.iloc[-1]
    else:
        data_fim = pd.to_datetime(data_fim)
        
    # Definir os valores do presente e do passado
    
    passado = data.loc[data.observationdate == data_inicio, variable].values[0]
    presente = data.loc[data.observationdate == data_fim, variable].values[0]
    
    # Define o número de pontos no tempo que vamos avaliar
    n = (data_fim - data_inicio).days
    
    # Calcular a taxa
    taxa = (presente/passado)**(1/n) - 1
    
    return taxa*100
```

```python
# Taxa de crescimento médio do COVID no Brasil em todo o período
taxa_crescimento(brasil, "confirmed")

# 16.27183353112116
```

### Estabelecendo a taxa de crescimento diário

```python
def taxa_crescimento_diario(data, variable, data_inicio=None):
    if data_inicio == None:
        data_inicio = data.observationdate.loc[data[variable] > 0].min()
    else:
        data_inicio = pd.to_datetime(data_inicio)
        
    data_fim = data.observationdate.max()
    
    # Define o número de pontos no tempo que vamos avaliar
    n = (data_fim - data_inicio).days
    
    # Taxa calculada de um dia para o outro
    taxas = list(map(
        lambda x: (data[variable].iloc[x] - data[variable].iloc[x-1]) / data[variable].iloc[x-1],
        range(1, n+1)
    ))
    return np.array(taxas) * 100
```

```python
tx_dia = taxa_crescimento_diario(brasil, "confirmed")

# Vai gerar um array com a taxa de cada dia
```

```python
primeiro_dia = brasil.observationdate.loc[brasil.confirmed > 0].min()

px.line(x=pd.date_range(primeiro_dia, brasil.observationdate.max())[1:],
       y=tx_dia, title='Taxa de crescimento de casos confirmados no Brasil')
```

![newplot.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/a73955de-b811-42d3-b771-84f4991810e1/newplot.png)