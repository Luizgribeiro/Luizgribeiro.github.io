+++
title = "Google Cloud Functions & Node - Visão Geral"
date = "2022-06-07"
author = "Luiz Guilherme Ribeiro"
authorTwitter = "luiz_gribeiro" #do not include @
cover = ""
tags = ["cloud functions", "GCP", "serverless", "node"]
keywords = ["serverless", "cloud functions"]
description = "Uma análise introdutória da utilização de Node em cloud functions"
showFullContent = false
readingTime = false
+++

## Introdução:

### Um pouco de contexto:

Nas últimas semanas eu tive a oportunidade de conhecer melhor alguns produtos da GCP, o que tem sido uma jornada de bastante aprendizado,
muitos erros e alguns acertos.

Por conta disso, resolvi documentar minhas descobertas no formato de blog, e espero que este artigo seja tão esclarecedor para você quanto teria sido para mim há algum tempo atrás.
Nele, teremos uma visão geral das Google Cloud Functions com ênfase naquelas que são acionadas por requisições HTTP.

### Cloud Functions

Cloud Functions é um dos serviços de "serverless" da GCP. Nele, a pessoa desenvolvedora fornece apenas uma função tratadora (handler), que é acionada quando um envento ou requisição específico ocorre. Esses eventos fornecem os dados que são processados pela função e, a partir deles, todo tipo de operações pode ser realizado, como armazenamento em bancos de dados, processamento e até requisições para outros serviços.
Uma das principais vantagens deste serviço é que não é preciso se preocupar com infraestrutura. Aspectos como gerenciar máquinas virtuais, atualizar sistemas operacionais e configurar regras de firewall são, portanto, desnecessários, pois toda a parte de infra fica a cargo da GCP.

## Pontos principais (TLDR):

Os pontos abaixo são as questões que achei mais importantes entre as minhas descobertas.
Elas se encontram descritas com mais detalhes logo abaixo.

1. Uma requisição por vez por instância
2. Um handler por função
3. Express "builtin"
4. Functions framework

## Uma requisição por instância

Apesar de operações concorrentes poderem ser realizadas por uma cloud function através de Promises ou async/await, uma instância criada pela GCP para executar uma determinada cloud function trata apenas uma requisição por vez. Isso significa que, se uma requisição for realizada enquanto uma outra requisição está sendo processada, o serviço criará uma nova instância para que o processamento da nova requisição seja realizado. Em linhas gerais a criação de uma nova instância se traduz em uma maior latência (demora), pois o processo de iniciar uma nova instância demanda tempo.

## Um handler por função

Pela forma como o serviço foi planejado, um único ponto de entrada é passado como handler. Dessa forma, quando uma requisição é feita apenas a função definida como ponto de entrada é chamada. Uma ~~gambiarra~~ solução muito utilizada para que seja possível acionar diferenter funcionalidades em um mesmo handler é utilizar um parâmetro de rota junto com switch/case para definir qual função deve ser chamada dentro de um mesmo handler.

## Express "builtin"

Em HTTP functions que utilizam Node um servidor Express é utilizado como base. Na realidade, o handler abordado no ponto anterior é uma única rota do Express para qual a cloud function é mapeada.

## Functions Framework

Um framework muito utilizado no desenvolvimento neste tipo de aplicação é o serverless framework. No entanto, o suporte e documentação dele para cloud functions não chega perto de ser o mesmo fornecido para os lambdas da AWS.
Por conta disso, recomendo fortemente que, para auxiliar no desenvolvimento de cloud functions, o [Functions Framework](https://cloud.google.com/functions/docs/functions-framework) seja utilizado. Com ele é possível executar e realizar testes de forma local em um ambiente muito similar ao que uma Cloud Function é executada.
