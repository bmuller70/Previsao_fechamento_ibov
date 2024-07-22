# Previsão do Fechamento da Bolsa de Valores IBOVESPA: Uma Análise Comparativa entre Modelos para predição de péries temporais
![](https://media.moneytimes.com.br/uploads/2024/01/ibovespa-2024-01-12t073059.003.jpg)

## Introdução

## Tratamento da base de dados
- **Captura de dados**\
   Os dados foram extraídos do site da Investing, abrangendo o período de 2010 até 2024. A base de dados contém 3585 registros, cada um representando uma entrada diária com 7 colunas de informações sobre o IBOVESPA. O DataFrame resultante tem a seguinte estrutura:
 ```
<class 'pandas.core.frame.DataFrame'>
RangeIndex: 3585 entries, 0 to 3584
Data columns (total 7 columns):
#   Column    Non-Null Count  Dtype 
0   Data      3585 non-null   int64 
1   Último    3585 non-null   int64 
2   Abertura  3585 non-null   int64 
3   Máxima    3585 non-null   int64 
4   Mínima    3585 non-null   int64 
5   Vol.      3584 non-null   object
6   Var%      3585 non-null   object
dtypes: int64(5), object(2)
```

**Exemplo de uma linha de dados:**
```
Data       Último   Abertura   Máxima   Mínima   Vol.    Var%
28062024   123907   124308     124500   123298   9,07B   -0,32%
```

- **Verificação de dados:**
  - Identificação e tratamento de zeros e valores nulos foram realizados. A coluna "Vol." apresentou um valor nulo, que foi tratado para manter a integridade dos dados.
    
  - A coluna 'Data' foi ajustada para garantir que todas as datas estivessem no formato de 8 dígitos e convertida para o tipo de dado datetime. Isso facilita a análise temporal dos dados.
    
  - Foram criadas novas colunas para o dia, mês e ano com base na coluna 'Data', o que permite análises temporais mais detalhadas.
    
  - O índice do DataFrame foi ajustado para representar a data, facilitando a análise das séries temporais.
```
dados = pd.read_csv('/content/drive/My Drive/Fase 2/Dados Históricos - Ibovespa.csv', encoding='utf-8', sep=',', thousands='.', decimal=',')

### Arrumar a Data para que tenha 8 digitos em todas tipo 10032024 e 04032024
dados['Data'] = dados['Data'].astype(str).str.zfill(8)

# Coloca em Formato de Data
dados['Data'] = pd.to_datetime(dados['Data'], format='%d%m%Y', errors='coerce')

# Cria coluna de Mês e Ano
dados['Data'] = dados['ref'] = dados ['Data']
dados['mes'] = dados['Data'].dt.month
dados['ano'] = dados['Data'].dt.year
dados['dia'] = dados['Data'].dt.day
dados = dados.set_index('Data')
```
  

