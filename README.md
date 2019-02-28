## Customer Churn Analytics

Customer Churn Analytics (Taxa de rotatividade dos Clientes).

Atividade do Curso de Big Data Analytics com R e Azure

	A rotatividade de clientes é um problema para qualquer empresa, ela ocorre quando clientes ou assinantes 
param de fazer negócios com uma empresa ou serviço, gerando a perda de clientes ou taxa de cancelamento.
	Nesta atividade vamos prever a rotatividade de clientes usando um conjunto de dados de telecomunicações.
	Será usado dataset fornecido gratuitamente pela IBM, https://www.ibm.com/communities/
analytics/watson-analytics-blog/guide-to-sample-datasets/ , e pacotes do R para cálculo de regressão logística, árvore de decisão e
floresta aleatória como modelos de Machine Learning.
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
	
churn<-read.csv('Telco-Customer-Churn.csv')
View(churn)

# Há quem traduza a expressão como taxa de rotatividade de clientes
#ou evasão de clientes. O churn rate é uma métrica fundamental para
#alguns segmentos de negócio, em especial para empresas de Tecnologia e
#SaaS (Software as a Service) acompanharem o percentual de clientes
#que cancelaram o serviço

str(churn)

# Usamos sapply para verificar o número de valores ausentes (missing) em cada coluna. 
# Descobrimos que há 11 valores ausentes nas colunas "TotalCharges". 
# Então, vamos remover todas as linhas com valores ausentes.

sapply(churn, function(x) sum(is.na(x)))
churn <-churn[complete.cases(churn),]

churn[487,]

?complete.cases
complete.cases(churn)

# Olhe para as variáveis, podemos ver que temos algumas limpezas e ajustes para fazer.
# 1. Vamos mudar "No internet service" para "No" por seis colunas, que são: 
# "OnlineSecurity", "OnlineBackup", "DeviceProtection", "TechSupport", "streamingTV", 
# "streamingMovies".
str(churn)
cols_recode1 <- c(10:15)
for(i in 1:ncol(churn[,cols_recode1])) {
  churn[,cols_recode1][,i] <- as.factor(mapvalues
                                        (churn[,cols_recode1][,i], from =c("No internet service"),to=c("No")))
}


# 2. Vamos mudar "No phone service" para "No" para a coluna “MultipleLines”
churn$MultipleLines <- as.factor(mapvalues(churn$MultipleLines, 
                                           from=c("No phone service"),
                                           to=c("No")))

# 3. Como a permanência mínima é de 1 mês e a permanência máxima é de 72 meses, 
# podemos agrupá-los em cinco grupos de posse (tenure): 
# “0-12 Mês”, “12–24 Mês”, “24–48 Meses”, “48–60 Mês” Mês ”,“> 60 Mês”
min(churn$tenure); max(churn$tenure)

group_tenure <- function(tenure){
  if (tenure >= 0 & tenure <= 12){
    return('0-12 Month')
  }else if(tenure > 12 & tenure <= 24){
    return('12-24 Month')
  }else if (tenure > 24 & tenure <= 48){
    return('24-48 Month')
  }else if (tenure > 48 & tenure <=60){
    return('48-60 Month')
  }else if (tenure > 60){
    return('> 60 Month')
  }
}

churn$tenure_group <- sapply(churn$tenure,group_tenure)
churn$tenure_group <- as.factor(churn$tenure_group)

	


	
