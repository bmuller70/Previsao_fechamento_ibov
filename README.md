# Previsão do Fechamento da Bolsa de Valores IBOVESPA: Uma Análise Comparativa entre Modelos para predição de séries temporais
![](https://media.moneytimes.com.br/uploads/2024/01/ibovespa-2024-01-12t073059.003.jpg)

### Colaboradores
@bmuller70/[Bruno Müller](https://www.linkedin.com/in/bruno-muller-335630196/) :computer:		

@marialuisamartins/[Maria Luisa Martins Brasil](https://www.linkedin.com/in/marialuisamartinsb/) :computer:	

@marialmrt97/[Maria Luiza Martins](https://www.linkedin.com/in/maria-luiza-martins-4115b213b/) :computer:	

@Pahmxx/[Phaola Oliveira Silva](https://www.linkedin.com/in/phaola-oliveira/) :computer:	


## Introdução
O Índice Bovespa (IBOVESPA) é o principal indicador do desempenho médio das cotações das ações negociadas na Bolsa de Valores de São Paulo (B3). Desde sua criação, em 1968, o índice tem sido uma referência crucial para investidores, analistas e economistas no acompanhamento do mercado acionário brasileiro. O IBOVESPA reflete a performance das empresas mais significativas e com maior volume de negociação na bolsa, proporcionando uma visão abrangente da saúde financeira do setor corporativo no Brasil. Esses índices não interessam apenas aos investidores, mas é também através deles que podemos observar saúde das empresas, expectativas de crescimento econômico e outras características econômicos do momento atual do país.

A previsão do fechamento do IBOVESPA é uma tarefa complexa e que possui relevância, tanto para investidores que buscam maximizar seus retornos quanto para gestores de risco que precisam mitigar potenciais perdas. Diversos métodos de predição de séries temporais têm sido aplicados na tentativa de antecipar os movimentos do índice, cada um com suas particularidades e níveis de precisão. 

Este trabalho visa realizar uma análise comparativa entre diferentes modelos de predição, destacando suas vantagens, limitações e a eficácia na previsão do fechamento da B3. Vamos abordar desde a captura dos dados e o desenvolvimento das etapas de tratamento até modelagem e avaliação de desempenho dos modelos escolhidos. 

## Base de dados
- **Captura de dados**\
   Para analise utilizamos os dados do site da [Investing](https://br.investing.com/indices/bovespa-historical-data), abrangendo o período de 2010 até 2024. A base de dados contém 3585 registros, cada um representando uma entrada diária com 7 colunas de informações sobre o IBOVESPA. O DataFrame resultante tem a seguinte estrutura:

**Exemplo de uma linha de dados:**
```
Data       Último   Abertura   Máxima   Mínima   Vol.    Var%
28062024   123907   124308     124500   123298   9,07B   -0,32%
```

## Construindo previsões

Conforme apontamos na introdução fazer uma predição dos valores de fechamento da IBOV é uma tarefa complexa, pois esses valores não são só uma função matemática, mas também a expressão do contexto econômico e politico do país. Devido a isso existem diversas variáveis exógenas que influenciam nesses valores, de eventos climáticos a comunicados de políticos e empresas. Dessa forma é importante entender que os modelos de Machine Learning buscam processar os dados de entrada, aqui o valor de fechamento, e expressar o comportamento dos dados em funções matemáticas não sendo capazes de captar a exogeneidade que o nosso objeto de estudo tem, e podemos observar essa característica na própria série temporal que chega próximo ao imprevisível, se assemelhando muito a previsão do tempo, que é possível prever mas até um determinado recorte temporal, normalmente de 15 dias. Essas características não inviabilizam a análise e sua predição, apenas servem como orientadores na nossa busca por resultados estatisticamente possíveis, sem cair na armadilha da “futurologia”.


Depois desse pequeno lembrete para a complexidade do objeto analisado vamos a aplicação dos modelos. 

## Testando os modelos

Para lidar com o nosso desafio escolhemos o modelo ARIMA e suas derivações que são o SARIMAX que faz a analise da serie utilizando 1 variável exógena, sendo a escolhida a VAR% além de levar em conta a sazonalidade. Utilizamos também o AutoArima que faz a seleção automática dos termos do ARIMA criando um modelo otimizado através de uma estratégia de tentativa-e-erro desses termos. Além do ARIMA também utilizamos o XGBOOST que atualmente é um dos modelos mais modernos e presentes no mercado.

### Observação tecnica sobre o conjunto treino e teste
Decidimos usar os primeiros 6 meses de 2024 como conjunto de teste e o período anterior, com todas as observações anuais completas, como conjunto de treino. Garantindo que o modelo seja treinado com dados completos e variados, refletindo diferentes condições de mercado, enquanto o conjunto de teste proporciona uma avaliação das previsões em um período recente e relevante. A diferenciação dos dados foi aplicada para estabilizar a série temporal e facilitar a modelagem.


 ## ARIMA

A partir da seleção dos valores padrão do ARIMA e utilizando uma sazonalidade de 30 dias obtivemos os seguintes resultados. 

* Caso você queria visualizar o diagnostico do modelo e uma explicação mais detalhada dos parametro consulte diretamente o [notebook do trabalho](TECH_CHALLENGE_FASE_2.ipynb) na seção modelo ARIMA.

O gráfico abaixo apresenta a predição realizada pelo modelo para 20 dias. Podemos observar que a linha pontilhada das previsões do modelo acompanha bem os sinais de subida e descida dos valores reais, indicando com o modelo foi minimamente eficiente.

 ![](/imagens/arima_20_dias.png)


### Avaliação do Modelo ARIMA

 - **Métricas de Avaliação:**
	- MSE (Mean Squared Error): 1358608.88
	- MAE (Mean Absolute Error): 950.72
	- MAPE (Mean Absolute Percentage Error): 10.31%
		- O MAPE relativamente baixo indica um bom desempenho geral do ARIMA. Embora o MSE e o MAE sejam altos, o MAPE fornece uma visão mais relativa e confiável da eficácia do modelo.
    
  - Os resultados indicam que o modelo ARIMA apresenta um erro médio absoluto (MAE) relativamente baixo e uma precisão razoável, conforme evidenciado pelo MAPE de 10,31%. O MSE também sugere uma variabilidade moderada nos erros de previsão.

### AutoArima
Com o objetivo de tentar melhorar um pouco os termos do modelo e consequentemente os resultados fizemos um teste utilizando o modelo gerado pelo AutoArima. Porém não obtivemos resultados significativos.
Mesmo com a série diferenciada podemos observar que as predições têm bastante dificuldade em seguir a linha dos valores reais. 

 ![](/imagens/autorima_preds.png)

- MSE: 5.747
- MAE: 0.006
- MAPE: 223.58%

Após o ajuste do AutoARIMA, o MSE de 5.747 e o MAE de aproximadamente 0,006 indicam uma melhora significativa na precisão das previsões, com erros muito baixos. No entanto, o MAPE de 223,58% sugere que, em média, as previsões têm um erro percentual muito alto em relação aos valores reais, o que pode ocorrer devido a valores reais próximos de zero. Isso indica que, embora o modelo esteja fazendo previsões precisas em termos absolutos, ele pode não estar capturando bem a variação relativa dos dados.


## Sarimax

O modelos SARIMAX utiliza os parâmetros de sazonalidade juntamente com uma variável exógena. Para execução dessa modelos optamos por usar a sazonalidade de 30 dias, por acreditar que mais do que isso o modelos poderia ficar enviesado. Referente a variável exógena optamos por utilizar a Var% por perceber que apresentava naturalmente alguns padrões interessantes para análise de séries temporais, como a estacionaridade. O gráfico abaixo apresenta a previsão para 30 dias utilizando o Sarimax.

 ![](/imagens/sarimax.png)

Observamos que as predições conseguem acompanhar as tendências, porém extrapolando muito os valores.  

- MAE: 1902.8919625508788
- MSE: 5350782.7445759075
- MAPE: 1.4937721679334748%

O MAE e o MSE indicam erros relativamente altos em termos absolutos, sugerindo que as previsões podem estar distantes dos valores reais. No entanto, o MAPE muito baixo indica que o modelo está capturando bem a variação percentual dos dados.

## XGBoost
O modelo XGBoost aceita que utilizemos características auxiliar para construir as previsões do modelo dessa forma utilizamos além da data(dia/mês/ano) também optamos por utilizar o valor de abertura que seriam as informações disponíveis  para previsão em um cenário real. 
O modelo também disponibiliza um recurso onde apresenta as características mais relevantes para as predições conforme mostra o gráfico abaixo. 

![](/imagens/imp_xgboost.png)

No gráfico a seguir seguem os valores reais acompanhados pelas previsões realizadas pelo modelo.

![](/imagens/pred_xgb.png)



## Resultados e Discussão
 - **Modelo ARIMA:** Embora tenha mostrado um MAPE razoável (7.49%), o ARIMA apresentou valores altos de MAE (708.31) e MSE (799,849.81). A análise dos resíduos sugere que o modelo está ajustado de forma adequada, sem sinais claros de overfitting. O MAPE é um bom indicador da precisão relativa do modelo.
   
 - **Modelo XGBoost:** Mostrou um excelente desempenho com MAE (3.95) e MSE (53.53) baixos, mas evidências de overfitting foram observadas, especialmente pela discrepância entre o desempenho em treinamento e teste. O MAPE extremamente baixo (0.04%) sugere que o modelo pode ter se ajustado demais aos dados de treinamento.

### Análise dos Resíduos:
 - **ARIMA:** Os resíduos mostraram uma distribuição razoavelmente normal, mas o histograma não era perfeitamente normal, indicando possíveis discrepâncias.

 - **XGBoost:** Os resíduos estavam próximos de zero, indicando uma boa correspondência entre previsões e valores reais, mas a precisão elevada pode mascarar problemas de overfitting.

### Impacto das Métricas:
 - **ARIMA:** Embora os valores altos de MSE e MAE sejam indicativos de erros grandes, o MAPE fornece uma visão relativa mais confiável, considerando a escala dos dados.

 - **XGBoost:** A baixa performance em dados de teste indica que o modelo pode estar se ajustando demais, o que é preocupante para as previsões futuras.

## Conclusão

 - **Resumo dos Resultados:** O ARIMA, apesar de ter apresentado métricas de erro mais altas, mostrou-se mais robusto e menos sujeito a overfitting em comparação ao XGBoost. O MAPE mais baixo do ARIMA é indicativo de uma previsibilidade mais consistente.

 - **Justificativa das Técnicas Utilizadas:** A escolha do ARIMA foi fundamentada na sua capacidade de generalização e estabilidade em previsões, mesmo com erros absolutos mais elevados. O XGBoost, apesar de seu bom desempenho absoluto, foi menos confiável devido ao overfitting.

 - **Recomendações para Trabalhos Futuros:** Melhorar o modelo XGBoost para evitar overfitting e explorar técnicas adicionais para validação cruzada e ajuste de parâmetros. Considerar a inclusão de variáveis exógenas ou modelos híbridos para aprimorar a precisão das previsões.
