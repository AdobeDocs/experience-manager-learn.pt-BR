---
title: Listas suspensas em cascata
description: Preencha as listas suspensas com base em uma seleção de lista suspensa anterior.
feature: Adaptive Forms
version: 6.4,6.5
kt: 9724
topic: Development
role: Developer
level: Intermediate
exl-id: f1f2cacc-9ec4-46d6-a6af-dac3f663de78
last-substantial-update: 2021-02-07T00:00:00Z
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '606'
ht-degree: 0%

---

# Listas suspensas em cascata

Uma lista suspensa em cascata é uma série de controles DropDownList dependentes dos quais um controle DropDownList depende dos controles pai ou anterior de DropDownList. Os itens no controle DropDownList são preenchidos com base em um item selecionado pelo usuário em outro controle DropDownList.

## Demonstração do caso de uso

>[!VIDEO](https://video.tv.adobe.com/v/340344?quality=12&learn=on)

Para o propósito deste tutorial, usei [API REST Geonames](http://api.geonames.org/) para demonstrar esse recurso.
Há várias organizações que fornecem esse tipo de serviço e, desde que tenham APIs REST bem documentadas, é possível integrar facilmente ao AEM Forms usando o recurso de integração de dados

As etapas a seguir foram seguidas para implementar listas suspensas em cascata no AEM Forms

## Criar conta de desenvolvedor

Crie uma conta de desenvolvedor com [Geonames](https://www.geonames.org/login). Anote o nome de usuário. Esse nome de usuário é necessário para chamar as APIs REST do geonames.org.

## Criar arquivo Swagger/OpenAPI

A Especificação de OpenAPI (antiga Especificação do Swagger) é um formato de descrição de API para APIs REST. Um arquivo OpenAPI permite descrever toda a API, incluindo:

* Pontos de extremidade disponíveis (/users) e operações em cada ponto de extremidade (GET /users, POST /users)
* Parâmetros de operação Entrada e saída para cada operação Métodos de autenticação
* Informações de contato, licença, termos de uso e outras informações.
* As especificações da API podem ser escritas em YAML ou JSON. O formato é fácil de aprender e legível tanto para seres humanos quanto para máquinas.

Para criar seu primeiro arquivo swagger/OpenAPI, siga o [Documentação da OpenAPI](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> O AEM Forms é compatível com a especificação OpenAPI versão 2.0 (FKA Swagger).

Use o [editor swagger](https://editor.swagger.io/) para criar seu arquivo swagger para descrever as operações que buscam todos os países e elementos secundários do país ou estado. O arquivo swagger pode ser criado no formato JSON ou YAML. O arquivo Swagger completo pode ser baixado de [aqui](assets/swagger-files.zip)
Os arquivos swagger descrevem a seguinte API REST
* [Obter todos os países](http://api.geonames.org/countryInfoJSON?username=yourusername)
* [Obter objeto filho de Geoname](http://api.geonames.org/childrenJSON?formatted=true&amp;geonameId=6252001&amp;username=yourusername)

## Criar fontes de dados

Para integrar o AEM/AEM Forms com aplicativos de terceiros, precisamos [criar fonte de dados](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) na configuração dos serviços em nuvem. Use o [arquivos swagger](assets/swagger-files.zip) para criar suas fontes de dados.
Você precisará criar duas fontes de dados (uma para buscar todos os países e outra para obter elementos secundários)


## Criar modelo de dados do formulário

A integração de dados do AEM Forms fornece uma interface intuitiva para criar e trabalhar com [modelos de dados de formulário](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html). Baseie o modelo de dados do formulário nas fontes de dados criadas na etapa anterior. Modelo de dados de formulário com 2 fontes de dados

![fdm](assets/geonames-fdm.png)


## Criar formulário adaptável

Integre as GET do modelo de dados de formulário ao seu formulário adaptável para preencher as listas suspensas.
Crie um formulário adaptável com duas listas suspensas. Um para listar os países e um para listar os estados/províncias dependendo do país selecionado.

### Lista suspensa Preencher países

A lista de países é preenchida quando o formulário é inicializado pela primeira vez. A captura de tela a seguir mostra o editor de regras configurado para preencher as opções da lista suspensa do país. Você terá que fornecer seu nome de usuário com a conta de nomes geográficos para que isso funcione.
![países get](assets/get-countries-rule-editor.png)

#### Preencher a lista suspensa Estado/Província

Precisamos preencher a lista suspensa Estado/Província com base no país selecionado. A captura de tela a seguir mostra a configuração do editor de regras
![estado-província-opções](assets/state-province-options.png)

### Exercício

Adicione 2 listas suspensas chamadas counties e cities no formulário para listar os condados e a cidade com base no país e estado/província selecionados.
![exercício](assets/cascading-drop-down-exercise.png)
