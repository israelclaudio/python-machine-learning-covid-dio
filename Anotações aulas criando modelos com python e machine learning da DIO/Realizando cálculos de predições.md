### Realizando cálculos e predições

### Predições

```python
from statsmodels.tsa.seasonal import seasonal_decompose
import matplotlib.pyplot as plt
```

```python
# Colocar as datas como index

confirmados = brasil.confirmed
confirmados.index = brasil.observationdate
```

```python
res = seasonal_decompose(confirmados)

fig, (ax1, ax2, ax3, ax4) = plt.subplots(4, 1, figsize=(10,8))

ax1.plot(res.observed)
ax2.plot(res.trend)  # tendencia
ax3.plot(res.seasonal)  # sazonalidade
ax4.plot(confirmados.index, res.resid)  # ruído
ax4.axhline(0, linestyle='dashed', c='black')
plt.show()
```