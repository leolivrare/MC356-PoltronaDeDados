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
> Apresente os resultados da forma mais rica possível, com gráficos e tabelas. Mesmo que o seu código rode online em um notebook, copie para esta parte a figura estática. A referência a código e links para execução online pode ser feita aqui ou na seção de detalhamento do projeto (o que for mais pertinente).
> A discussão dos resultados também pode ser feita aqui na medida em que os resultados são apresentados ou em seção independente. Aspectos importantes a serem discutidos: É possível tirar conclusões dos resultados? Quais? Há indicações de direções para estudo? São necessários trabalhos mais profundos?

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