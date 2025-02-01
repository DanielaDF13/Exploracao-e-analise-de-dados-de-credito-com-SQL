# **Exploração e análise de dados de crédito com SQL**


## Os dados:

Os dados representam informações de clientes de um banco e contam com as seguintes colunas:
<center>

| Variável                | Descrição                                                | Tipo         |
| ----------------------- |:--------------------------------------------------------:| ------------:|
| idade                   |  idade do cliente                                        |     Inteiro  |
| sexo                    |  M = 'Masculino'; F = 'Feminino'                         |      M/F     |
| dependentes             |  número de dependentes do cliente                        |    Inteiro   |
| escolaridade            |  nível de escolaridade do clientes                       |    Texto     |
| salario_anual           |  faixa salarial do cliente                               |    Texto     |
| tipo_cartao             |  tipo de cartao do cliente                               |     Texto    |
| qtd_produtos            |  quantidade de produtos comprados nos últimos 12 meses   |     Inteiro  |
| iteracoes_12m           |  quantidade de iterações/transacoes nos ultimos 12 meses |     Inteiro  |
| meses_inativo_12m       |  quantidade de meses que o cliente ficou inativo         |    Inteiro   |
| limite_credito          |  limite de credito do cliente                            |    Float     |
| valor_transacoes_12m    |  valor das transações dos ultimos 12 meses               |    Float     |
| qtd_transacoes_12m      |  quantidade de transacoes dos ultimos 12 meses           |    Inteiro   |

</center>
<br>

A tabela foi criada no **AWS Athena** junto com o **S3 Bucket** com uma versão dos dados disponibilizados em: https://github.com/andre-marcos-perez/ebac-course-utils/tree/main/dataset

## **Exploração de dados:**

**Quantidade e informações temos na nossa base de dados**


Com o comando <br>
**Query:** SELECT count(*) FROM credito

> Reposta: 2564 linhas <br>

Temos a quantidade de linhas do dataset, importante se reforçar que esse dataset é um fragmento do dataset original disponível no link acima, que foi selecionado para estudo por limites computacionais e financeiros, porém quanto maior a quantidade de dados utilizada, mais confiável a análise.

**Tipos de dados**

**Query:** DESCRIBE credito

![Tipos de dados](https://github.com/DanielaDF13/Exploracao-e-analise-de-dados-de-credito-com-SQL/blob/main/img/tipo-de-variaveis.png?raw=true)

**Dataset**


**Query:** SELECT * FROM credito LIMIT 10;

![Dez primeiras linhas do dataset](https://github.com/DanielaDF13/Exploracao-e-analise-de-dados-de-credito-com-SQL/blob/main/img/dados.png?raw=true)

É possível reparar que existem algumas informações nulas na tabela (valor na).

**Explorando variáveis categóricas do dataset**

Com as respectivas querys foi formado essas tabelas e agrupadas em apenas uma imagem.<br>



**Query:** SELECT DISTINCT escolaridade FROM credito <br>
**Query:** SELECT DISTINCT estado_civil  FROM credito <br>
**Query:** SELECT DISTINCT salario_anual  FROM credito <br>
**Query:** SELECT DISTINCT tipo_cartao  FROM credito <br>


<br>


![Tipos](https://github.com/DanielaDF13/Exploracao-e-analise-de-dados-de-credito-com-SQL/blob/main/img/Explora%C3%A7%C3%A3o%20de%20dados.png?raw=true)


Novamente pode-se notar informações nulas na tabela, isso deverá ser tratado nas próximos comandos.

## **Análise de dados**

Após exploramos os dados e entendemos as informções no banco de dados, podemos analisar as informações para buscar entender o que está acontecendo no banco de dados.

**Escolaridade dos clientes**

**Query:** SELECT count(*), escolaridade from credito group by escolaridade
<center>

![Escolaridade dos cliente](https://github.com/DanielaDF13/Exploracao-e-analise-de-dados-de-credito-com-SQL/blob/main/img/distribui%C3%A7%C3%A3o%20escolaridade.png?raw=true)

![Grafico escolaridade dos cliente](https://github.com/DanielaDF13/Exploracao-e-analise-de-dados-de-credito-com-SQL/blob/main/img/distribui%C3%A7%C3%A3o%20escolaridade%20grafico.png?raw=true)

</center>

> A maioria dos clientes dessa base de dados possuem mestrado como escolaridade, seguido por ensino médio e existem 346 clientes que não informaram ou não consta a escolaridade.

**Faixa salarial dos cliente**

**Query:** SELECT count(*), salario_anual from credito group by salario_anual
<center>

![Quantidade para cada faixa salarial](https://github.com/DanielaDF13/Exploracao-e-analise-de-dados-de-credito-com-SQL/blob/main/img/salario%20anual.png?raw=true)

![Grafico para cada faixa salarial](https://github.com/DanielaDF13/Exploracao-e-analise-de-dados-de-credito-com-SQL/blob/main/img/distibui%C3%A7%C3%A3o%20do%20salario%20anual.png?raw=true)

</center>

> A maioria dos clientes dessa base de dados possui um renda menor que 40K e existem 235 clientes que não informaram ou não consta a faixa salarial.<br>
> De certa forma, pode ser interessante para a empresa focar nesse público de mais baixa renda.

**Tipo de cartão por cliente**

**Query:** SELECT count(*), tipo_cartao from credito group by tipo_cartao
<center>

![Tipo de cartão por cliente](https://github.com/DanielaDF13/Exploracao-e-analise-de-dados-de-credito-com-SQL/blob/main/img/distribui%C3%A7%C3%A3o%20de%20tipo%20de%20cartao.png?raw=true)

![Grafico tipo de cartão por cliente](https://github.com/DanielaDF13/Exploracao-e-analise-de-dados-de-credito-com-SQL/blob/main/img/distribui%C3%A7%C3%A3o%20do%20tipo%20de%20cart%C3%A3o%20grafico.png?raw=true)

</center>

> Existe uma grande quantidade de cartões do tipo blue e muito pouco dos outros tipos.<br>
>Pode ser interessante analisar o crédito dos clientes com cartão blue para ver se um upgrade de cartão seria interessante para esses clientes.

**Informação de crédito por tipo de cartão**
<br>

**Query:** SELECT max(valor_transacoes_12m) as maior_valor_gasto, round(avg(valor_transacoes_12m),2) as  media_valor_gasto, min(valor_transacoes_12m) as min_valor_gasto, round(avg(qtd_produtos),2) as media_quantidade_produto, round(avg(iteracoes_12m),2) as media_interacoes, round(avg(meses_inativo_12m),2) as media_meses_inativos, tipo_cartao from credito group by tipo_cartao
<br>

<center>

![Informação de crédito por tipo de cartão](https://github.com/DanielaDF13/Exploracao-e-analise-de-dados-de-credito-com-SQL/blob/main/img/Informa%C3%A7%C3%A3o%20de%20cr%C3%A9dito%20por%20tipo%20de%20cart%C3%A3o.png?raw=true)

</center>

>Pode-se notar que a maior valor gasto, média de quantidade de produtos e quantidade de interações para o cartão do tipo blue são maiores e quanto média de meses inativo e mínimo valor gasto são melhores para o cartão do tipo platinum.


**Quantos clientes são homens e quantos são mulheres**

**Query:** SELECT count(*), sexo from credito group by sexo

<center>

![Distribuição de sexo](https://github.com/DanielaDF13/Exploracao-e-analise-de-dados-de-credito-com-SQL/blob/main/img/distribui%C3%A7%C3%A3o%20sexo.png?raw=true)

![Distribuição de sexo - gráfico](https://github.com/DanielaDF13/Exploracao-e-analise-de-dados-de-credito-com-SQL/blob/main/img/distruibui%C3%A7%C3%A3o%20de%20sexo%20grafico.png?raw=true)

</center>

> Existe maior proporção de homens nos dados.

**Gasto por sexo**

**Query:** select max(valor_transacoes_12m) as maior_valor_gasto, round(avg(valor_transacoes_12m),2) as  media_valor_gasto, min(valor_transacoes_12m) as min_valor_gasto, sexo from credito group by sexo


![Valor transacoes/sexo](https://github.com/DanielaDF13/Exploracao-e-analise-de-dados-de-credito-com-SQL/blob/main/img/valor%20por%20sexo.png?raw=true)

> Os gastos de homens e mulheres são similares.

**Idade dos clientes por sexo**

**Query:** select avg(idade) as media_idade, min(idade) as min_idade, max(idade) as max_idade, sexo from credito group by sexo
![Idade dos clientes por sexo](https://github.com/DanielaDF13/Exploracao-e-analise-de-dados-de-credito-com-SQL/blob/main/img/Idade%20dos%20clientes%20por%20sexo.png?raw=true)

> Por meio dessa análise não foi possível extrair nenhuma informação relevante.  A menor idade dos dois sexos é a mesma e a média é muito similar. A unica  diferença é a idade máxima, porém a diferença é muito pequena.

**Maior e menor transação dos clientes**

**Query:** select min(valor_transacoes_12m) as transacao_minima, max(valor_transacoes_12m) as transacao_minima from credito
![Valor transacoes](https://github.com/DanielaDF13/Exploracao-e-analise-de-dados-de-credito-com-SQL/blob/main/img/transacoes%20max%20e%20min.png?raw=true)

> Nesse banco de dados temos soma de transações em 12 meses variam de 510.16 a 4776.58.

**Características dos clientes que possuem os maiores creditos**

**Query:**
select max(limite_credito) as limite_credito, escolaridade, tipo_cartao, sexo
from credito<br>
where escolaridade != 'na'<br>
group by  escolaridade, tipo_cartao, sexo <br>
order by limite_credito desc<br>
limit 10

![Características dos clientes que possuem os maiores creditos](https://github.com/DanielaDF13/Exploracao-e-analise-de-dados-de-credito-com-SQL/blob/main/img/caracteristicas%20dos%20clientes%20que%20possuem%20maiores%20creditos.png?raw=true)

> Não parece haver um impacto da escolaridade no limite. O limite mais alto é oferecido para um homem sem educação formal. O cartão também parece não estar relacionado com a escolaridade nem com o limite. Dentre os maiores limites, encontramos clientes com cartão: gold, silver, platinum e blue

**Características dos clientes que possuem os menores creditos**

**Query:**
select min(limite_credito) as limite_credito, escolaridade, tipo_cartao, sexo
from credito<br>
where escolaridade != 'na' <br>
group by  escolaridade, tipo_cartao, sexo <br>
order by limite_credito asc<br>
limit 10

![Características dos clientes que possuem os menores creditos](https://github.com/DanielaDF13/Exploracao-e-analise-de-dados-de-credito-com-SQL/blob/main/img/caracteristicas%20dos%20clientes%20que%20possuem%20menores%20creditos.png?raw=true)

> Podemos perceber que todos os clientes da seleção possuem cartão do tipo blue e que assim como o limite máximo não há padrão de escolaridade.

**O salário por limite**

**Query:** select avg(qtd_produtos) as qts_produtos, avg(valor_transacoes_12m) as media_valor_transacoes, avg(limite_credito) as media_limite,  sexo,   salario_anual from credito <br>
where salario_anual != 'na'<br>
group by sexo, salario_anual<br>
order by avg(valor_transacoes_12m) desc

![Valor salario_anual Limite](https://github.com/DanielaDF13/Exploracao-e-analise-de-dados-de-credito-com-SQL/blob/main/img/salario%20por%20limite.png?raw=true)
'
>As pessoas que tem menor faixa salarial também apresentam menor limite de credito e maior valor médio de transações.

**Valor de transação por limite de crédito**

**Query:** select  valor_transacoes_12m , limite_credito, tipo_cartao, salario_anual from credito <br>
where salario_anual != 'na'  
order by valor_transacoes_12m desc <br>
limit 15 <br>

![Valor de transação por limite de crédito](https://github.com/DanielaDF13/Exploracao-e-analise-de-dados-de-credito-com-SQL/blob/main/img/limite%20por%20valor%20transacoes.png?raw=true)

>Pode-se notar que vários clientes utilizaram acima de seus limites e que os clientes com maior valor de transação possuem cartão blue.

# Conclusão

Essas foram algumas análises extraídas do dataset de crédito.  

Alguns insights interessantes:

- a maior parte dos clientes possui renda até 40K
- a maior parte dos clientes é masculino!
- a escolaridade não parece influenciar no limite nem no tipo do cartão
- dentre os menores limites só há presença de cartão blue
- a faixa salarial impacta diretamente no limite de crédito
- nao existem clientes com salário anual acima de 60K do sexo feminino
-Pela grande quantidade de cartões blue pode ser interessante o upgrade para outro nível de cartão vendo que a estatística de valores gastos e quantidade de produto no cartão blue é elevada.
-Os clientes com menor limites são os que possuem maior valor de transações seria interessante analisar se possuem bom crédito e aumentar seus limites

**Uma exploração maior dos dados pode explicar porque as mulheres tem menor crédito. Isso também pode ser um problema cultural que pode ser repensado.<br> Com uma exploração maior dos dados pode se analisar se dar upgrade para os clientes com melhores estatísticas com cartão blue seria interessante.**
