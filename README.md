# DividendYieldValue

### Análise de Dividendos e Rendimento de Dividendos

Nesta documentação, exploraremos como baixar dados de dividendos de ações, calcular o rendimento de dividendos e visualizar os resultados usando as bibliotecas `yfinance`, `vectorbt` e `pandas`.

#### 1. Instalando e Importando Bibliotecas

Antes de começar, certifique-se de que as bibliotecas `yfinance` e `vectorbt` estão instaladas. Você pode instalar essas bibliotecas usando os seguintes comandos:

```python
!pip install yfinance --quiet
!pip install vectorbt --quiet
```

Agora, vamos importar as bibliotecas que serão usadas no código:

```python
import vectorbt as vbt
import pandas as pd
import yfinance as yf
```

#### 2. Definindo Ações (Tickers)

Defina a lista de ações (tickers) para as quais você deseja baixar os dados de dividendos e realizar a análise:

```python
tickers = ['BBAS3.SA', 'BBDC4.SA', 'ITUB4.SA', 'SANB11.SA', 'ABCB4.SA', 'BPAC11.SA', 'BRSR6.SA', 'BPAN4.SA', 'VGIR11.SA']
```

#### 3. Extraindo Dados das Ações

Vamos baixar os dados de dividendos e preços de fechamento das ações usando as bibliotecas `yfinance` e `vectorbt`:

```python
div = vbt.YFData.download(tickers, start='2008-01-01').get('Dividends')
close = yf.download(tickers, start='2008-01-01')['Close']
```

#### 4. Padronizando os Dados

Padronizamos os índices e colunas dos DataFrames de dividendos e preços de fechamento:

```python
div.index = pd.to_datetime(div.index.date)
close.index = pd.to_datetime(close.index.date)

div = div.reindex(sorted(div.columns), axis=1)
close = close.reindex(sorted(close.columns), axis=1)
```

#### 5. Dividendos dos Últimos 12 Meses

Calculamos a soma dos dividendos dos últimos 12 meses para cada ação:

```python
div_12m = div.rolling('365D').sum()
```

#### 6. Dividend Yield

Calculamos o rendimento de dividendos (dividend yield) dos últimos 12 meses para cada ação:

```python
dy_12m = (div_12m / close) * 100
```

#### 7. Plot

Visualizamos o rendimento de dividendos dos últimos 12 meses para todas as ações usando a função `vbt.plot()`:

```python
dy_12m.vbt.plot(width=1000, height=600).show()
```

### Ideias para Exploração Adicional

1. **Comparação de Ações**: Além do rendimento de dividendos, você pode explorar outros indicadores de desempenho das ações, como o preço das ações, retorno total e volatilidade. Isso pode fornecer insights interessantes sobre como as ações se comportam em relação aos dividendos e aos preços.

2. **Análise Temporal**: Em vez de olhar apenas para os últimos 12 meses, você pode analisar os dados de dividendos e rendimento ao longo de diferentes períodos de tempo, como trimestres ou anos fiscais.

3. **Diversificação de Portfólio**: Explore como a diversificação de um portfólio de ações afeta o rendimento de dividendos agregado e como isso pode se relacionar com o risco e retorno geral do portfólio.

4. **Filtros de Seleção**: Crie filtros para selecionar ações com base em critérios específicos, como rendimento de dividendos mínimo ou histórico de pagamentos consistentes.

5. **Correlações**: Analise as correlações entre o rendimento de dividendos e outras métricas econômicas, como taxas de juros ou indicadores macroeconômicos.

Lembre-se de que essa é apenas uma introdução à análise de dados de dividendos e rendimento de dividendos. Há muitas outras análises possíveis, dependendo das suas necessidades e interesses específicos.
