### Usando lambda para identificar novos casos confirmados por dia

Novos casos por dia

```python
brasil.shape[0]

# 84

# Técnica de programação funcional
brasil['novoscasos'] = list(map(
    lambda x: 0 if (x==0) else brasil['confirmed'].iloc[x] - brasil['confirmed'].iloc[x-1],
    np.arange(brasil.shape[0])
))

# Visualizando
px.line(brasil, x='observationdate', y='novoscasos', title='Novos casos por dia')
```

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/86597507-689e-4123-934d-f9107c9363fa/Untitled.png)