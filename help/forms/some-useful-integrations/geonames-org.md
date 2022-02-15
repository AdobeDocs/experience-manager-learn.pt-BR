---
title: Listas suspensas em cascata
description: Preencha as listas suspensas com base em uma seleção de lista suspensa anterior.
feature: Adaptive Forms
version: 6.4,6.5
kt: 9724
topic: Development
role: Developer
level: Intermediate
source-git-commit: 15b57ec6792bc47d0041946014863b13867adf22
workflow-type: tm+mt
source-wordcount: '607'
ht-degree: 0%

---

# Listas suspensas em cascata

Uma lista suspensa em cascata é uma série de controles DropDownList dependentes em que um controle DropDownList depende dos controles pai ou DropDownList anterior. Os itens no controle DropDownList são preenchidos com base em um item selecionado pelo usuário de outro controle DropDownList.

## Demonstração do caso de uso

>[!VIDEO](https://video.tv.adobe.com/v/340344?quality=9&learn=on)

Para a finalidade deste tutorial, eu usei [API REST do Geonames](http://api.geonames.org/) para demonstrar essa capacidade.
Há várias organizações que fornecem esse tipo de serviço e, desde que tenham APIs REST bem documentadas, você pode se integrar facilmente ao AEM Forms usando o recurso de integração de dados

As etapas a seguir foram seguidas para implementar listas suspensas em cascata no AEM Forms

## Criar conta do desenvolvedor

Crie uma conta de desenvolvedor com [Geonames](https://www.geonames.org/login). Anote o nome de usuário. Esse nome de usuário será necessário para invocar APIs REST do geonames.org.

## Criar arquivo Swagger/OpenAPI

A Especificação OpenAPI (antiga Especificação Swagger) é um formato de descrição de API para APIs REST. Um arquivo OpenAPI permite descrever toda a sua API, incluindo:

* Pontos de extremidade (/users) disponíveis e operações em cada ponto de extremidade (GET/users, POST /users)
* Parâmetros de operação Entrada e saída para cada operação Métodos de autenticação
* Informações de contato, licença, termos de uso e outras informações.
* As especificações da API podem ser gravadas em YAML ou JSON. O formato é fácil de aprender e legível para seres humanos e máquinas.

Para criar seu primeiro arquivo swagger/OpenAPI, siga as [Documentação do OpenAPI](https://swagger.io/docs/specification/2-0/basic-structure/)

>[!NOTE]
> O AEM Forms suporta a especificação OpenAPI versão 2.0 (FKA Swagger).

Use o [editor de swagger](https://editor.swagger.io/) para criar seu arquivo swagger para descrever as operações que buscam todos os países e elementos filho do país ou estado. O arquivo swagger pode ser criado no formato JSON ou YAML. O arquivo swagger concluído pode ser baixado de [here](assets/swagger-files.zip)
Os arquivos do swagger descrevem a seguinte API REST
* [Obter todos os países](http://api.geonames.org/countryInfoJSON?username=yourusername)
* [Obter filhos do objeto Geoname](http://api.geonames.org/childrenJSON?formatted=true&amp;geonameId=6252001&amp;username=yourusername)

## Criar fontes de dados

Para integrar o AEM/AEM Forms com aplicativos de terceiros, precisamos [criar fonte de dados](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) na configuração dos serviços em nuvem. Use o [arquivos swagger](assets/swagger-files.zip) para criar suas fontes de dados.
Será necessário criar duas fontes de dados (uma para buscar todos os países e outra para obter elementos filhos)


## Criar modelo de dados do formulário

A integração de dados do AEM Forms oferece uma interface de usuário intuitiva para criar e trabalhar com [modelos de dados de formulário](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html). Baseie o modelo de dados de formulário nas fontes de dados criadas na etapa anterior. Modelo de dados de formulário com 2 fontes de dados

![fdm](assets/geonames-fdm.png)


## Criar formulário adaptável

Integre as invocações do GET do Modelo de Dados de Formulário com seu formulário adaptável para preencher as listas suspensas.
Crie um formulário adaptável com 2 listas suspensas. Um para listar os países e outro para listar os estados/províncias dependendo do país selecionado.

### Preencher a lista suspensa Países

A lista de países é preenchida quando o formulário é inicializado pela primeira vez. A captura de tela a seguir mostra o editor de regras configurado para preencher as opções da lista suspensa de países. Será necessário fornecer o nome de usuário com a conta de nomes de usuário para que isso funcione.
![países-alvo](assets/get-countries-rule-editor.png)

#### Preencha a lista suspensa Estado/província

Precisamos preencher a lista suspensa Estado/província com base no país selecionado. A captura de tela a seguir mostra a configuração do editor de regras
![opções de província do estado](assets/state-province-options.png)

### Exercício

Adicione 2 listas suspensas, chamadas de condados e cidades no formulário, para listar os condados e a cidade, com base no país e no estado/província selecionados.
![exercício](assets/cascading-drop-down-exercise.png)





