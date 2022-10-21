### Criando o primeiro gráfico com os dados selecionados

Vamos selecionar apenas os dados do Brasil para investigar

```python
df.countryregion.unique()

df.loc[df.countryregion == "Brazil"]

brasil = df.loc[
    (df.countryregion == "Brazil") &
    (df.confirmed > 0)
]
```

Casos confirmados

```python
# Gráfico da evolução de casos confirmados
px.line(brasil, "observationdate", "confirmed", title='Casos confirmados no Brasil')
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/3759bb4a-b6dc-48e1-bf30-402a52295ad9/Untitled.png)