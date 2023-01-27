# Análise de Dados com Python
Desafio:
Você trabalha em uma empresa de telecom e tem clientes de vários serviços diferentes, entre os principais: internet e telefone.

O problema é que, analisando o histórico dos clientes dos últimos anos, você percebeu que a empresa está com Churn de mais de 26% dos clientes.

Isso representa uma perda de milhões para a empresa.

O que a empresa precisa fazer para resolver isso?

Base de Dados: https://drive.google.com/drive/folders/1T7D0BlWkNuy_MDpUHuBG44kT80EmRYIs?usp=sharing
Link Original do Kaggle: https://www.kaggle.com/radmirzosimov/telecom-users-dataset

# Passo 1: Importar a base de dados
import pandas as pd
​
tabela = pd.read_csv("telecom_users.csv")
​
# Passo 2: Visualizar a base de dados
tabela = tabela.drop("Unnamed: 0", axis=1)
display(tabela)
# - Entender quais as informações tão disponíveis
# - Descobrir as cagadas da base de dados
IDCliente	Genero	Aposentado	Casado	Dependentes	MesesComoCliente	ServicoTelefone	MultiplasLinhas	ServicoInternet	ServicoSegurancaOnline	...	ServicoSuporteTecnico	ServicoStreamingTV	ServicoFilmes	TipoContrato	FaturaDigital	FormaPagamento	ValorMensal	TotalGasto	Churn	Codigo
0	7010-BRBUU	Masculino	0	Sim	Sim	72	Sim	Sim	Nao	SemInternet	...	SemInternet	SemInternet	SemInternet	2 anos	Nao	CartaoCredito	24.10	1734.65	Nao	NaN
1	9688-YGXVR	Feminino	0	Nao	Nao	44	Sim	Nao	Fibra	Nao	...	Nao	Sim	Nao	Mensal	Sim	CartaoCredito	88.15	3973.2	Nao	NaN
2	9286-DOJGF	Feminino	1	Sim	Nao	38	Sim	Sim	Fibra	Nao	...	Nao	Nao	Nao	Mensal	Sim	DebitoAutomatico	74.95	2869.85	Sim	NaN
3	6994-KERXL	Masculino	0	Nao	Nao	4	Sim	Nao	DSL	Nao	...	Nao	Nao	Sim	Mensal	Sim	BoletoEletronico	55.90	238.5	Nao	NaN
4	2181-UAESM	Masculino	0	Nao	Nao	2	Sim	Nao	DSL	Sim	...	Nao	Nao	Nao	Mensal	Nao	BoletoEletronico	53.45	119.5	Nao	NaN
...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...	...
5981	0684-AOSIH	Masculino	0	Sim	Nao	1	Sim	Nao	Fibra	Sim	...	Nao	Sim	Sim	Mensal	Sim	BoletoEletronico	95.00	95	Sim	NaN
5982	5982-PSMKW	Feminino	0	Sim	Sim	23	Sim	Sim	DSL	Sim	...	Sim	Sim	Sim	2 anos	Sim	CartaoCredito	91.10	2198.3	Nao	NaN
5983	8044-BGWPI	Masculino	0	Sim	Sim	12	Sim	Nao	Nao	SemInternet	...	SemInternet	SemInternet	SemInternet	Mensal	Sim	BoletoEletronico	21.15	306.05	Nao	NaN
5984	7450-NWRTR	Masculino	1	Nao	Nao	12	Sim	Sim	Fibra	Nao	...	Nao	Sim	Sim	Mensal	Sim	BoletoEletronico	99.45	1200.15	Sim	NaN
5985	4795-UXVCJ	Masculino	0	Nao	Nao	26	Sim	Nao	Nao	SemInternet	...	SemInternet	SemInternet	SemInternet	Anual	Nao	CartaoCredito	19.80	457.3	Nao	NaN
5986 rows × 22 columns

# Passo 3: Tratamento de dados
# - Valores que estão reconhecidos de forma errada
tabela["TotalGasto"] = pd.to_numeric(tabela["TotalGasto"], errors="coerce")
​
# - Valores vazios
# deletando as colunas vazias
# axis = 0 _> linha ou axis = 1 _> coluna
tabela = tabela.dropna(how="all", axis=1)
# deletando as linhas vazias
tabela = tabela.dropna(how="any", axis=0)
​
print(tabela.info())
# Passo 4: Análise Inicial
# Como estão os nossos cancelamentos?
print(tabela["Churn"].value_counts())
print(tabela["Churn"].value_counts(normalize=True).map("{:.1%}".format))
# Passo 5: Análise Mais completa
# comparar cada coluna da minha tabela com a coluna de cancelamento
import plotly.express as px
​
# etapa 1: criar o gráfico
for coluna in tabela.columns:
    # para edições nos gráficos: https://plotly.com/python/histograms/
    # para mudar a cor do gráfico , color_discrete_sequence=["blue", "green"]
    grafico = px.histogram(tabela, x=coluna, color="Churn", text_auto=True)
    # etapa 2: exibir o gráfico
    grafico.show()
!pip install plotly
Conclusões e Ações
Escreva aqui suas conclusões:

Clientes com contrato mensal tem MUITO mais chance de cancelar:

Podemos fazer promoções para o cliente ir para o contrato anual
Familias maiores tendem a cancelar menos do que famílias menores

Podemos fazer promoções pra pessoa pegar uma linha adicional de telefone
MesesComoCliente baixos tem MUITO cancelamento. Clientes com pouco tempo como cliente tendem a cancelar muito

A primeira experiência do cliente na operadora pode ser ruim
Talvez a captação de clientes tá trazendo clientes desqualificados
Ideia: a gente pode criar incentivo pro cara ficar mais tempo como cliente
QUanto mais serviços o cara tem, menos chance dele cancelar

podemos fazer promoções com mais serviços pro cliente
Tem alguma coisa no nosso serviço de Fibra que tá fazendo os clientes cancelarem

Agir sobre a fibra
Clientes no boleto tem MUITO mais chance de cancelar, então temos que fazer alguma ação para eles irem para as outras formas de pagamento
