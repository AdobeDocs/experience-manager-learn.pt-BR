---
title: Listas suspensas em cascata
description: Preencha as listas suspensas com base em uma seleção de lista suspensa anterior.
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-9724
topic: Development
role: Developer
level: Intermediate
exl-id: f1f2cacc-9ec4-46d6-a6af-dac3f663de78
last-substantial-update: 2021-02-07T00:00:00Z
duration: 185
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '610'
ht-degree: 0%

---

# Listas suspensas em cascata

Uma lista suspensa em cascata é uma série de controles DropDownList dependentes dos quais um controle DropDownList depende dos controles pai ou anterior de DropDownList. Os itens no controle DropDownList são preenchidos com base em um item selecionado pelo usuário em outro controle DropDownList.

## Demonstração do caso de uso

>[!VIDEO](https://video.tv.adobe.com/v/340344?quality=12&learn=on)

Para fins deste tutorial, usei a [API REST Geonames](https://www.geonames.org/export/web-services.html) para demonstrar esse recurso.
Há várias organizações que fornecem esse tipo de serviço e, desde que tenham APIs REST bem documentadas, é possível integrar facilmente ao AEM Forms usando o recurso de integração de dados

As etapas a seguir foram seguidas para implementar listas suspensas em cascata no AEM Forms

## Criar conta de desenvolvedor

Crie uma conta de desenvolvedor com [Geonames](https://www.geonames.org/login). Anote o nome de usuário. Esse nome de usuário é necessário para chamar as APIs REST do geonames.org.

## Criar arquivo Swagger/OpenAPI

A Especificação de OpenAPI (antiga Especificação do Swagger) é um formato de descrição de API para APIs REST. Um arquivo OpenAPI permite descrever toda a API, incluindo:

* Pontos de extremidade disponíveis (/users) e operações em cada ponto de extremidade (GET /users, POST /users)
* Parâmetros de operação Entrada e saída para cada operação
Métodos de autenticação
* Informações de contato, licença, termos de uso e outras informações.
* As especificações da API podem ser escritas em YAML ou JSON. O formato é fácil de aprender e legível tanto para seres humanos quanto para máquinas.

Para criar seu primeiro arquivo swagger/OpenAPI, siga a [documentação sobre OpenAPI](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> O AEM Forms é compatível com a especificação OpenAPI versão 2.0 (FKA Swagger).

Use o [editor swagger](https://editor.swagger.io/) para criar seu arquivo swagger e descrever as operações que buscam todos os países e elementos secundários do país ou estado. O arquivo swagger pode ser criado no formato JSON ou YAML.

## Criar fontes de dados

Para integrar o AEM/AEM Forms com aplicativos de terceiros, precisamos [criar a fonte de dados](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) na configuração dos serviços em nuvem. Use os [arquivos swagger](assets/geonames-swagger-files.zip) para criar suas fontes de dados.
Você precisará criar duas fontes de dados (uma para buscar todos os países e outra para obter elementos secundários)


## Criar modelo de dados do formulário

A integração de dados do AEM Forms fornece uma interface de usuário intuitiva para criar e trabalhar com [modelos de dados de formulário](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html). Baseie o modelo de dados do formulário nas fontes de dados criadas na etapa anterior. Modelo de dados de formulário com 2 fontes de dados

![fdm](assets/geonames-fdm.png)


## Criar formulário adaptável

Integre as invocações do GET do modelo de dados de formulário ao seu formulário adaptável para preencher as listas suspensas.
Crie um formulário adaptável com duas listas suspensas. Um para listar os países e um para listar os estados/províncias dependendo do país selecionado.

### Lista suspensa Preencher países

A lista de países é preenchida quando o formulário é inicializado pela primeira vez. A captura de tela a seguir mostra o editor de regras configurado para preencher as opções da lista suspensa do país. Você terá que fornecer seu nome de usuário com a conta de nomes geográficos para que isso funcione.
![obter-países](assets/get-countries-rule-editor.png)

#### Preencher a lista suspensa Estado/Província

Precisamos preencher a lista suspensa Estado/Província com base no país selecionado. A captura de tela a seguir mostra a configuração do editor de regras
![opções-estado-província](assets/state-province-options.png)

### Exercício

Adicione 2 listas suspensas chamadas counties e cities no formulário para listar os condados e a cidade com base no país e estado/província selecionados.
![exercício](assets/cascading-drop-down-exercise.png)


### Assets de amostra

Você pode baixar os seguintes ativos para começar a criar a amostra de lista suspensa em cascata
Os arquivos concluídos do Swagger podem ser baixados de [aqui](assets/geonames-swagger-files.zip)
Os arquivos swagger descrevem a seguinte API REST
* [Obter Todos os Países](https://secure.geonames.org/countryInfoJSON?username=yourusername)
* [Obter Filhos do objeto Geoname](https://secure.geonames.org/children?formatted=true&amp;geonameId=6252001&amp;username=yourusername)

O [Modelo de Dados de Formulário concluído pode ser baixado daqui](assets/geonames-api-form-data-model.zip)
