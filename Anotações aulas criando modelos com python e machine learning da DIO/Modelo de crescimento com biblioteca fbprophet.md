### Modelo de crescimento com biblioteca fbprophet

**Modelo de crescimento**

```python
!conda install -c conda-forge fbprophet

from prophet import Prophet
```

```python
# Preprocessamentos

train = confirmados.reset_index()[:-5]
test = confirmados.reset_index()[-5:]

# Renomeando colunas
train.rename(columns={'observationdate':'ds', 'confirmed':'y'}, inplace=True)
test.rename(columns={'observationdate':'ds', 'confirmed':'y'}, inplace=True)

# Definir o modelo de crescimento
profeta = Prophet(growth='logistic', changepoints=['2020-03-21', '2020-03-30', '2020-04-25', 
                                                  '2020-05-03', '2020-05-10'])

pop = 211463256
# pop = 1000000
train['cap'] = pop

# treina o modelo
profeta.fit(train)

# Construir previsões para o futuro
future_dates = profeta.make_future_dataframe(periods=200)
future_dates['cap'] = pop
forecast = profeta.predict(future_dates)
```

```python
fig = go.Figure()

fig.add_trace(go.Scatter(x=forecast.ds, y=forecast.yhat, name='Predição'))
# fig.add_trace(go.Scatter(x=test.index, y=test, name='Observados - Teste'))
fig.add_trace(go.Scatter(x=train.ds, y=train.y, name='Observados - Treino'))
fig.update_layout(title='Predições de casos confirmados no Brasil')

fig.show()
```

![newplot.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/eb64b477-fb1b-400c-8ce2-b6a7bd82cc6e/newplot.png)
