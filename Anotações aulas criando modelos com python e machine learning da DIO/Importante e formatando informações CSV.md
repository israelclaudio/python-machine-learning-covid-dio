### Importando informa��es do CSV

```python
import pandas as pd
import numpy as np
from datetime import datetime
import plotly.express as px
import plotly.graph_objects as go

# vamos importar os dados para o projeto
url = 'https://github.com/neylsoncrepalde/projeto_eda_covid/blob/master/covid_19_data.csv?raw=True'

df = pd.read_csv(url, parse_dates=['ObservationDate', 'Last Update'])
df
```

### Formatando as informa��es importadas

```python
# Conferir os tipos de cada coluna
df.dtypes

SNo                         int64
ObservationDate    datetime64[ns]
Province/State             object
Country/Region             object
Last Update        datetime64[ns]
Confirmed                 float64
Deaths                    float64
Recovered                 float64
dtype: object
```

Nomes de colunas n�o devem ter letras mai�sculas e nem caracteres especiais. Vamos implementar uma fun��o para fazer a limpeza dos nomes dessas colunas

```python
import re

def corrige_colunas(col_name):
    return re.sub(r"[/| ]", "", col_name).lower()

# Vamos corrigir todas as colunas do df
df.columns = [corrige_colunas(col) for col in df.columns]
```