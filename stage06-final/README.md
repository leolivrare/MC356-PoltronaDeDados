# Etapa Final

## Projeto `<Título do Projeto>`

## Equipe Poltrona de Dados
* Leonardo Livrare
* Pedro Pupo
* Pedro Strambeck

## Slides da Apresentação da Etapa Final

Os slides podem ser visualizados em [slides](slides/MC536_Final.pdf).

## Resumo do Projeto
Utilizamos diferentes bancos de dados (COVID-19 API, WorldBank) e relacionamos dados provenientes da pandemia de coronavírus em diversos países com seus respectivos dados socioeconômicos e geográficos, a fim de obter quais características de um país possuem correlação com sua resposta à pandemia.

## Motivação e Contexto

Devido a pandemia presente no ano de 2020, achamos relevante analisar dados referentes ao novo Coronavírus a fim de relacioná-los às características socioeconômicas de diversos países. Dessa forma, podemos utilizar dados referentes à saúde, educação e índices econômicos para determinar como tais fatores refletem na resposta de cada país perante a pandemia.

## Detalhamento do Projeto

Procuramos coletar o maior número de dados sobre a situação de cada país, visando determinar os efeitos que a situação política e socioeconômica dele influenciou na sua resposta e atuação contra a pandemia do coronavírus. Sendo assim, nossas duas principais fontes de dados foram a API da covid-19 e os datasets do World Bank. 

Como esses dados possuem formatos diferentes, World Bank possui uma organização tabular, enquanto a API da covid uma estrutura hierárquica, optamos por armazenar esses dados em bancos de dados diferentes. Os dados da API da covid-19 foram armazenados em uma instância do MongoDB na AWS, enquanto os dados do World Bank foram armazenados em várias tabelas em um banco postgreSQL, também na hospedado na AWS.

Para facilitar o deploy e configurações dos serviços na AWS utilizamos dois serviços, o [ElephantSQL](https://www.elephantsql.com/) e o [MongoAtlas](https://www.mongodb.com/cloud/atlas). Esses serviços nos possibilitaram configurar clusters que atendem nossas necessidades de forma gratuita.

Os dados fornecidos pela API do covid-19, são em sua maior parte dados sobre a quantidade de casos e mortes, para cada país. Infelizmente outros dados mais complexos não estavam disponíveis de forma gratuita nessa API, impossibilitando sua aquisição. 

Após feitas as requisições para a API do covid, os dados foram tratados, removendo campos desnecessários e inseridos no cluster online do MongoDB

[Notebook Covid](notebooks/covid_queries.ipynb)

Já os datasets do World Bank nos forneceram uma quantidade muito grande de dados e indicadores socioeconômicos, educacionais, de saúde e geográficos de diversos países. No entanto, vários desses dados estavam incompletos para determinados países ou anos. Sendo assim foi feito um trabalho para filtrar, selecionar, separar e agrupar os diversos dados, de modo que sua organização fosse clara e de fácil utilização posterior.

[Notebook Tratamento Dados World Bank](notebooks/tratamento_dados_worldBank.ipynb)

Após tratados, os dados do World Bank foram inseridos em suas respectivas tabelas no cluster online do postgreSQL.

[Notebook Insere Dados World Bank](notebooks/insere_dados_worldBank_no_postgres.ipynb)

Após todos os dados terem sido tratados e inseridos em seus respectivos bancos, precisávamos fazer queries desses dados e relacioná-los. Como os dados estão em formatos diferentes, nossa solução para relacioná-los foi trazer esses dados para formatos nativos do Python, possibilitando seu relacionamento. Sendo assim utilizamos a biblioteca SQLAlchemy para realizar as queries dos dados no Postgres e a biblioteca PyMongo para fazer as queries dos dados armazenados no MongoDB.

Após a obtenção de todos os dados em estruturas nativas do python, eles foram transformados em dataframes do Pandas, visando facilitar sua manipulação. Finalmente fizemos as análises comparando os dados de número de mortes e indicadores de cada país. Tais análises serão discutidas de maneira mais profunda na seção resultados e discussões.

[Notebook Queries e Análise](notebooks/queries_analise.ipynb)


## Evolução do Projeto

Passamos por várias mudanças ao longo do projeto, sendo a principal a opção por usar clusters online para os bancos, ao invés de bancos em memória.

A princípio fizemos os requests para a API da covid-19 e transformamos os dados de formato hierárquico para tabular, gerando csv's para os dados e os salvando em um banco relacional em memória.

No entanto, posteriormente quando fomos trabalhar com os dados do World Bank percebemos que esses também estavam em formato tabular. Sendo assim, visando utilizar uma maior variedade de bancos de dados, optamos por manter os dados da covid em formato hierárquico, ao invés de os transformar em formato tabular, e deixar os dados do World Bank em formato tabular.

O próximo problema que foi enfrentado foi como relacionar dados armazenados em formatos distintos. Não seria possível fazer um join entre uma tabela de um banco relacional e uma coleção de um banco não relacional. A solução para esse problema foi utilizar Python como um denominador comum. Ao fazer as queries a partir do Python, podíamos obter os dois tipos de dados em estruturas nativas do python, possibilitando seu relacionamento.

Decidido que seriam usados dois bancos distintos e que as queries seriam feitas a partir de um notebook em Python, passamos a olhar mais para a infraestrutura dos bancos. Optamos por utilizar clusters online para ambos os bancos. O cluster online tem várias vantagens em relação a um banco em memória, como a capacidade de armazenamento por exemplo. Porém a principal vantagem para nós era a separação entre código dos inserts e queries e o banco de fato. Dessa forma, é possível que qualquer um com as credenciais do banco se conecte a este a partir de qualquer plataforma, evitando fazer os inserts e criação de tabelas novamente, o que aconteceria em um banco em memória.

Utilizamos os serviços [ElephantSQL](https://www.elephantsql.com/) e [MongoAtlas](https://www.mongodb.com/cloud/atlas) para auxiliar no deploy das instâncias dos bancos na AWS. Além disso, durante o processo de desenvolvimento utilizamos ferramentas como o DBeaver para auxiliar a monitorar e configurar os bancos.

Uma última dificuldade enfrentada foi a pouca quantidade de indicadores fornecidos pela API da covid, infelizmente alguns indicadores mais interessantes eram bloqueados por pagamento. Sendo assim nos baseamos principalmente na taxa de mortalidade do país para fazer as queries.


## Resultados e Discussão
Através de todo o processo descrito anteriormente, foi possível alcançar alguns resultados interessantes relacionando o desempenho de cada país no combate à COVID-19 aos seus indicadores socioeconômicos, educacionais e políticos. Entretanto, vale explicitar algumas dificuldades em relação a confiabilidade dos dados obtidos da fonte do World Bank. Tal fonte trouxe muitos dados incompletos, o que limitou bastante o relacionamento completo dos dados entre todos os países, ou seja, não foi possível analisar e cruzar os dados de todos os países. Além disso, muitos dados trouxeram questionamento sobre a veracidade dos mesmos, pois cruzando os mesmos com outras fontes, foi possvel identificar discordancias.

Porém, mesmo com as dificuldades, foi possível completar as análises tranquilamente, de forma que a implementação não foi comprometida. O que ficou comprometido são os resultados obtidos, em razão da discordancia dos dados. Mas vale explicitar que foi possível obter grande aprendizado em relação a construção, desenvolvimento e uso de banco de dados como um todo. Dito isso, podemos apresentar todos os resultados obtidos.

### Infraestrutura
Primeiramente, vamos falar sobre os resultados no quesito de infraestrutura dos bancos utilizados. Com isso, utilizamos dois tipos principais de dados para estruturar nossa infraestrutura a fim de possibilitar as análises posteriormente. Utilizamos o tipo Tabular, vindos inteiramente da fonte de [World Bank](https://databank.worldbank.org/source/world-development-indicators) e o tipo hierárquico, o qual veio interamente da API da [COVID-19](https://covid19api.com/).

#### Estrutura Hierárquica (NoSQL):
Para o armazenamento dos dados desse formato, utilizamos uma instância do banco MongoDB na AWS, o qual consiste de um banco não relacional (NoSQL), muito bom para os dados da API do COVID-19. Com isso, foi possível extrair todos os dados interessantes da API da COVID-19 e armazenar diretamente nessa instância que criamos. Isso foi bastante interessante para o aprendizado sobre esse formato de banco de dados e também para a utilização do mesmo. Portanto, com o banco estruturado, seguimos no processo da inserção dos dados no mesmo para posterior execução das queries, buscando os dados necessários para nossas análises.

#### Estrutura Relacional:
Para o modelo Tabular utilizamos, como já descrito nos itens anteriores, uma instancia online do PostgreSQL, o qual é um Banco Relacional. O serviço utilizado foi o [ElephantSQL](https://www.elephantsql.com/) para subir a instancia. Com isso, foi possível estruturar diversas tabelas no modelo relacional, as quais armazenaram todos os dados do World Bank.

#### Estrutura das tabelas no Banco Relacional:

![Estrutura Postgres](tabelas_postgres.png)

Todas as tabelas que estão no banco do PostgreSQL contém dados sobre os países, sendo divididas por tabelas geográficas, economicas, educacionais, saúde e socioeconomicas. Como extraímos os dados para todos esses indicadores do World Bank para três anos distintos, 2017 até 2019, estruturamos uma tabela de cada indicador para cada ano, como pode ser visto na imagem acima.

#### Inserção dos Dados no Banco Relacional

Para fazer a inserção das tabelas no banco, utilizamos alguns scripts em Python para automatizar o processo, porém utilizamos SQL para a criação das tabelas e suas estruturas. Dessa forma, inserimos os dados de cada tabela a partir de um arquivo `.csv`, obtido da etapa de tratamento de dados. Portanto, segue o exemplo dos códigos utilizados para criar as tabelas no banco e inserir os dados nas mesmas:
##### Tabelas de Saúde:
```def create_health_table(conn, year):
  query = f"""
  begin;
  drop table if exists health_{year};
  create table if not exists health_{year} (
    id int primary key,
    country_code varchar(3),
    health_workers numeric,
    health_expenditure numeric,
    health_expenditure_per_capita numeric,
    hospital_beds numeric,
    nurses_and_midwives numeric,
    constraint fk_country_code
      FOREIGN KEY(country_code) 
	      REFERENCES country_codes(alpha_3)
  );
  commit;
  end;"""
  conn.execute(query)
  df = pd.read_csv(f"../data/processed/health_{year}.csv")
  df = df[df["country_code"].notnull()]
  df.to_sql(f"health_{year}", if_exists="append", con=conn, index=False)
```
O código para as demais tabelas pode ser visto no [Notebook Insere Dados World Bank](notebooks/insere_dados_worldBank_no_postgres.ipynb), onde todos seguiram a mesma lógica do apresentado acima.

### Consultas a partir do Banco Relacional

Com o banco estruturado e os dados inseridos, foi possível começar a implementação das queries nas tabelas do banco. Dessa forma, vamos apresentar algumas das consultas implementadas:

##### Query sobre dados de Saúde:
```
health_expenditure_pib_query = """
select
country_name,
country_code,
cc.alpha_2,
health_expenditure
from health_{} as h
inner join
country_codes as cc on cc.alpha_3 = h.country_code
where health_expenditure is not null
order by health_expenditure desc
"""
```
A qual teve como resultado:

![health_expenditure_pib_query](image.png)

##### Query sobre dados de Educação:
```
research_query = """
select
cc.country_name,
e.country_code,
cc.alpha_2,
research_and_development_expenditure
from education_{} e
inner join
country_codes as cc on cc.alpha_3 = e.country_code
where research_and_development_expenditure is not null
order by research_and_development_expenditure desc
"""
```
A qual teve como resultado:

![research_query](research_query.jpeg)

##### Query sobre dados Socio Econômicos:
```
unemployment_query = """
select
cc.country_name,
se.country_code,
cc.alpha_2,
unemployment
from "socioEconomics_{}" se
inner join
country_codes as cc on cc.alpha_3 = se.country_code
where unemployment is not null
order by unemployment desc
"""
```
A qual teve como resultado:

![unemployment_query](socio_economics_query.jpeg)

##### Query sobre dados Geograficos:
```
pop_density_query = """
select
cc.country_name,
e.country_code,
cc.alpha_2,
pop_density
from geographics_{} e 
inner join
country_codes as cc on cc.alpha_3 = e.country_code
where pop_density is not null
order by pop_density desc
"""
```
A qual teve como resultado:

![pop_density_query](pop_density_query.jpeg)

##### Query sobre Investimento em Saúde em Relação ao PIB:
```
health_pib_query = """
select
h2.country_code,
cc.country_name,
h2.health_expenditure_per_capita,
e.gdp,
gdp_per_capita
from health_2017 h2 
inner join
economics_2017 e on e.country_code = h2.country_code
inner join 
country_codes as cc on cc.alpha_3 = e.country_code
where h2.health_expenditure_per_capita is not null
order by h2.health_expenditure_per_capita desc
"""
```
A qual teve como resultado:

![df_health_pib](df_health_pib.jpeg)

Fizemos diversas outras queries cruzando as tabelas do Banco, as quais podem ser visualizadas no [Notebook Queries e Análise](notebooks/queries_analise.ipynb). Todas as queries implementadas estão nesse notebook, organizadas por categoria.

Com os dados apresentados acima, é possvel concluir que foi possível explorar bastante o Banco relacional, possibilitando o armazenamentos dos dados tratados e posteriormente algumas análises cruzando as tabelas do banco. Entretanto, o foco das análises desse trabalho é em cima do cruzamento dos dados apresentados anteriormente com os dados da COVID-19 para cada país. Portanto, vamos seguir com o tópico que apresentamos as queries no banco NoSQL utilizado para o tipo hierarquico.

### Queries no MongoDB

Para a execução das queries nesse banco, utilizamos scripts em Python, fazendo uso da biblioteca pymongo. Segue então as principais queries para extração dos dados:

#### Conexão com o Banco:
```db = client.covid_data```

Onde client é uma instância da classe MongoClient, com os dados de conexão para o nosso banco.

#### Lista as Coleções Disponíveis:
```db.list_collection_names()```

Resultando em: ['covid_summary', 'countries', 'first_cases']

#### Listagem de todas as informaçes de todos os países:
```
def get_country_info(country_code: str) -> dict:
    summary = covid_summary.find_one({"CountryCode": country_code})
    first = first_cases.find_one({"country_code": country_code})
    if first:
        return {**summary, **{"first_case": first["first_case"]}}
    return summary
   
list(map(get_country_info, map(lambda x: x["ISO2"], countries.find({}))))
```

Esse código extrai todos os dados necessários para as nossas análises do banco MongoDB. Foi possível notar que a escolha de armazenar os dados da API do COVID-19 nesse banco foi muito bem vinda, pois diminui bastante o tempo de obtenção dos dados diretamente da API. Além disso, o uso do MongoDB também facilita no processo de tratamento e extração dos dados. Com as queries em ambos os bancos feitas, é possível aplicar as análises relacionando COVID com os dados do World Bank, as quais foram feitas a partir de scripts em Python. É possível ver mais a fundo o código do MongoDB em [Notebook Covid](notebooks/covid_queries.ipynb).

### Análises

Agora, vamos apresentar as análises que foram implementadas cruzando os resultados obtidos pelas queries descritas acima, relacionando os dados do Banco Relacional com o MongoDB. Dessa forma, segue então as análises feitas:

#### Países em relação a taxa de mortalidade
![paises_taxa_mort](paises_taxa_mort.jpeg)

Essa tabela será utilizada em todas as análises apresentadas nesse trabalho, onde a mesma contém todos os dados do banco Mongo. Nela temos, para cada país, dados importantes de como a doença avançou e o indicador mais importante para as análises, a taxa de mortalidade.

#### Países com maior taxa de mortalidade em relação ao PIB

Nessa análise, ordenamos os países em relação ao PIB per-capita. Com isso, pegamos os 20 países com maiores PIB per-capita e os 20 países com menores PIB per-capita e extraímos a média da taxa de morte deles. Dessa forma, chegamos no seguinte resultado:
```
top20:  0.015978299079689017
last20:  0.02473783886622265
All:  0.02048843412682874
```

Portanto, podemos notar que os países com maior PIB per-capita (top20) tiveram uma média na taxa de mortalidade bastante menor que os países com piores PIB  per-capita (last20), indicando talvez uma relação entre a riquesa do país e seu desempenho na pandemia. Vale ressaltar também que os países mais ricos apresentaram uma média melhor que o mundo inteiro em geral, onde a média de tudo é apresentada pelo indicador 'All'. Entretanto, não podemos afirmar nada concreto dessa análise, pois os dados sobre PIB per-capita do World Bank estava bastante equivocado para alguns países, o que invalida parte dessa análise. Porém, mesmo com os dados equivocados, toda a lógica da implementação da análise segue a mesma para dados corretos. Só vale explicitar isso para não levar esse resultado como fonte de verdade.

#### Investimento em saúde em relação a taxa de mortalidade

Nessa análise, ordenamos os países em relação ao gasto com saúde, tanto público quanto privado. Com isso, pegamos os 10 países com maiores gastos em saúde e os 10 países com menores gastos em saúde e extraímos a média da taxa de morte deles, da mesma forma feita anteriormente para PIB. Dessa forma, chegamos no seguinte resultado:
```
Top 10:  0.017152070771987122
Last 10:  0.023677886835821914
All:  0.01907830705728344
```
Portanto, chegamos em outro resultado interessante, dessa vez não errôneo por causa dos dados do World Bank. Ou seja, nesse executamos uma verificação e os dados presentes na análise batem com dados de outras fontes. Com isso, chegamos a uma conclusão de que os países com mais investimento em saúde, tiveram, em média, um resultado mais positivo em relação a taxa de morte para COVID-19. Além disso, os dez países com mais investimento em saúde tiveram uma média inferior à média geral, reafirmando o melhor desempenho. Vale ressaltar que, para essa análise, construímos também uma tabela relacionando esses indicadores, as quais se originaram das tabelas do World Bank e dos dados extraídos do MongoDB.

#### Investimento em educação em relação a taxa de mortalidade

Nessa análise, ordenamos os países em relação ao gasto com pesquisa e desenvolvimento ciêntifico. Com isso, pegamos os 10 países com maiores gastos em pesquisa e os 10 países com menores gastos em pesquisa e extraímos a média da taxa de morte deles, da mesma forma feita anteriormente. Além disso, construímos também uma tabela relacionando tais indicadores. Dessa forma, chegamos no seguinte resultado:
```
Top 10:  0.015175292505589511
Last 10:  0.020476933583673147
All:  0.02030516412427076
```

Portanto, novamente, vemos um melhor desempenho por parte dos países com mais gastos em pesquisa e desenvolvimento. Tais países ficaram, em média, 0.5% abaixo dos demais pases (média geral) no indicador de taxa de mortalidade. Além disso, temos um resultado interessante, onde os dez piores países ficaram com uma média muito parecida com o média geral, o que indica uma maior uniformidade desses dados e um maior descolamento dos top 10 países em relação aos outros.

## Conclusões
> Apresente aqui as conclusões finais do trabalho e as lições aprendidas.

## Modelo Conceitual Final

![ModeloC](assets/Modelo-Conceitual.png)

## Modelos Lógicos Finais

![ModeloL](assets/Modelo-Logico.png)

## Programa de extração e conversão de dados atualizado

> Coloque um link para o arquivo do notebook que executa a extração e conversão de dados. Ele estará dentro da pasta `notebook`. Se por alguma razão o código não for executável no Jupyter, coloque na pasta `src`. Se a extração e conversão envolverem queries executadas através de uma interface de um SGBD não executável no Jupyter, como o Cypher, apresente na forma de markdown.

## Conjunto de queries para todos os modelos

> Acrescente um link para o(s) arquivo(s) do(s) notebook(s) que executa(m) as queries para cada um dos modelos lógicos. Eles estarão dentro da pasta `notebook`. Se por alguma razão o código não for executável no Jupyter, coloque na pasta `src`. Se as queries forem executadas através de uma interface de um SGBD não executável no Jupyter, como o Cypher, apresente na forma de markdown.
> Apresente todas as suas queries em versão final, mesmo que tenham aparecido em etapas anteriores.

## Bases de Dados
| Base   |  Link  |  Descrição |
|----------|:-------------:|------:|
| Covid-19 Dataset |  https://covid19api.com/ | Dados variados sobre COVID-19 de cada país, como, por exemplo, total de casos, total de mortes, etc. |
| World Development Dataset |    https://databank.worldbank.org/source/world-development-indicators   |  Dataset tabular contendo diversas informações e indicadores sobre a economia, saúde, aspectos da população em geral, entre outras áreas.  |
| World Education Dataset | https://databank.worldbank.org/source/education-statistics-%5e-all-indicators | Dataset contém informações variadas sobre a educação de diversos países. |

## Arquivos de Dados
nome do arquivo | link | breve descrição
----- | ----- | -----
`countries.csv` | [link](data/countries.csv) | `Lista de países e seus códigos`
`fist_case.csv` | [link](data/first_case.csv) | `Lista de países e seus primeiros casos de covid`
`summary.csv` | [link](data/summary.csv) | `Lista de países e dados gerais sonbre a covid`
`world_development_indicators.csv` | [link](data/world_development_indicators.csv) | `Lista de países e diversos indicatores agrupados nas tabelas do modelo lógico`
