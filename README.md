## Customer Churn Analytics

Customer Churn Analytics (Taxa de rotatividade dos Clientes).

Atividade do Curso de Big Data Analytics com R e Azure

	A rotatividade de clientes é um problema para qualquer empresa, ela ocorre quando clientes ou assinantes param de fazer negócios com uma empresa ou serviço, gerando a perda de clientes ou taxa de cancelamento.
	Nesta atividade vamos prever a rotatividade de clientes usando um conjunto de dados de telecomunicações.
	Será usado dataset fornecido gratuitamente pela IBM, https://www.ibm.com/communities/analytics/watson-analytics-blog/guide-to-sample-datasets/ , e pacotes do R para cálculo de regressão logística, árvore de decisão e floresta aleatória como modelos de Machine Learning.
	No dataset as linhas representam os clientes e as colunas cada atributo do cliente.

## Pacotes que deverão ser carregados

+ library(plyr)
+ library(corrplot)
+ library(ggplot2)
+ library(gridExtra)
+ library(ggthemes)
+ library(caret)
+ library(MASS)
+ library(randomForest)
+ library(party)

## Os dados

	Os dados estão no arquivo 'Telco-Customer-Churn.csv'
	O arquivo contém 7043(clientes)linhas e 21(recursos)colunas. A coluna Churn é do tipo lógica onde NO para o cliente que não saiu da empresa e SIM para o cliente que saiu da empresa.
	

	


	
