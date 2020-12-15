# Etapa 05 - Projeto final

## Slides da Proposta

Os slides podem ser visualizados em [slides](Slides/Slides_05.pdf).

## Motivação e Contexto

Devido a pandemia presente no ano de 2020, achamos relevante analisar dados referentes ao novo Coronavírus a fim de relacioná-los às características socioeconômicas de diversos países. Dessa forma, podemos utilizar dados referentes à saúde, educação e índices econômicos para determinar como tais fatores refletem na resposta de cada país perante a pandemia.

## Método

A metodologia do projeto seguirá o seguinte esquema:

![Metodologia](assets/Metodologia.png)

## Modelo Conceitual Inicial

![ModeloC](assets/Modelo-Conceitual.png)

## Modelos Lógicos Iniciais

![ModeloL](assets/Modelo-Logico.png)

## Extração e conversão de dados

O conjunto de requests está em [stage4-covid-api](notebooks/stage3-covid-api.ipynb).

## Queries

O conjunto de queries está em [stage4-queries](notebooks/stage-3-queries.ipynb).

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
