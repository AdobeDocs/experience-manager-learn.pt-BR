---
title: Configuração da DataSource com Salesforce no AEM Forms 6.3 e 6.4
seo-title: Configuração da DataSource com Salesforce no AEM Forms 6.3 e 6.4
description: Integração da AEM Forms com o Salesforce usando o modelo de dados de formulário
seo-description: Integração da AEM Forms com o Salesforce usando o modelo de dados de formulário
uuid: 0124526d-f1a3-4f57-b090-a418a595632e
feature: adaptive-forms, form-data-model
topics: integrations
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 8e314fc3-62d0-4c42-b1ff-49ee34255e83
translation-type: tm+mt
source-git-commit: cce9f5d1dae05a36b942f6b07a46c65f82eac43c
workflow-type: tm+mt
source-wordcount: '928'
ht-degree: 0%

---


# Configuração da DataSource com Salesforce no AEM Forms 6.3 e 6.4{#configuring-datasource-with-salesforce-in-aem-forms-and}

## Pré-requisitos {#prerequisites}

Neste artigo, iremos acompanhar o processo de criação da Fonte de Dados com o Salesforce

Pré-requisitos para este tutorial:

* Role até a parte inferior desta página e baixe o arquivo swagger e salve-o no disco rígido.
* AEM Forms com SSL habilitado

   * [Documentação oficial para habilitar o SSL no AEM 6.3](https://helpx.adobe.com/experience-manager/6-3/sites/administering/using/ssl-by-default.html)
   * [Documentação oficial para habilitar o SSL no AEM 6.4](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/ssl-by-default.html)

* Você precisará ter a conta do Salesforce
* Será necessário criar um aplicativo conectado. A documentação oficial do Salesforce para criar o aplicativo está listada [aqui](https://help.salesforce.com/articleView?id=connected_app_create.htm&amp;type=0).
* Forneça os escopos OAuth apropriados para o aplicativo (selecionei todos os escopos OAuth disponíveis para fins de teste)
* Forneça o URL de retorno de chamada. O URL de retorno de chamada no meu caso era

   * Se você estiver usando **AEM Forms 6.3**, o URL de retorno de chamada será https://gbedekar-w7-1:6443/etc/cloudservices/fdm/createlead.html. Neste URL, createlead é o nome do meu modelo de dados de formulário.

   * Se você estiver usando o** AEM Forms 6.4**, o URL de retorno de chamada será [https://gbedekar-w7-:6443/libs/fd/fdm/gui/components/admin/fdmcloudservice/createcloudconfigwizard/cloudservices.html](https://gbedekar-w7-1:6443/libs/fd/fdm/gui/components/admin/fdmcloudservice/createcloudconfigwizard/cloudservices.html)

Neste exemplo, gbedekar -w7-1:6443 é o nome do meu servidor e a porta na qual o AEM está sendo executado.

Depois de criar o Aplicativo conectado, anote **Consumer key e Chave secreta**. Você precisará deles ao criar a fonte de dados no AEM Forms.

Agora que você criou seu aplicativo conectado, será necessário criar um arquivo swagger para as operações que você precisa executar no salesforce. Um arquivo de amostra swagger é incluído como parte dos ativos disponíveis para download. Esse arquivo swagger permite que você crie um objeto &quot;Lead&quot; no envio do formulário adaptável. Por favor, explore este arquivo swagger.

A próxima etapa é criar a Fonte de dados no AEM Forms. Siga as etapas abaixo de acordo com sua versão do AEM Forms

## AEM Forms 6.3 {#aem-forms}

* Faça logon no AEM Forms usando o protocolo https
* Navegue até serviços em nuvem digitando em https://&lt;nomedoservidor>:&lt;serverport> /etc/cloudservices.html, Por exemplo, https://gbedekar-w7-1:6443/etc/cloudservices.html
* Role para baixo até &quot;Form Data Model&quot; (Modelo de dados de formulário).
* Clique em &quot;Mostrar configurações&quot;.
* Clique em &quot;+&quot; para adicionar nova configuração
* Selecione &quot;Restaurar serviço completo&quot;. Forneça um Título e um Nome significativos para a configuração. Por exemplo,

   * Nome: CreateLeadInSalesForce
   * Título: CreateLeadInSalesForce

* Clique em &quot;Criar&quot;

**Na tela seguinte **

* Selecione &quot;Arquivo&quot; como a opção para o arquivo de origem do swagger. Navegue até o arquivo que você baixou anteriormente
* Selecione Tipo de autenticação como OAuth2.0
* Forneça os valores ClientID e Client Secret
* O URL OAuth é - **https://login.salesforce.com/services/oauth2/authorize**
* Atualizar URL do token - **https://na5.salesforce.com/services/oauth2/token**
* **Url Do Toque De Acesso - https://na5.salesforce.com/services/oauth2/token**
* Âmbito da autorização: ** api   id completo chatter_api   openid   flash_token web**
* Manipulador de autenticação: Portador de Autorização
* Clique em &quot;Conectar-se ao OAUTH&quot;.Se tudo correr bem, você não verá nenhum erro

Depois de criar seu Modelo de dados de formulário usando o Salesforce, você pode criar a Integração de dados de formulário usando a Fonte de dados que acabou de criar. A documentação oficial para criar a Integração de dados de formulário é [here](https://helpx.adobe.com/aem-forms/6-3/data-integration.html).

Certifique-se de configurar o Modelo de dados de formulário para incluir o serviço POST para criar um objeto de cliente potencial no SFDC.

Você também precisará configurar o Serviço de Leitura e Gravação para o objeto Lead. Consulte as capturas de tela na parte inferior desta página.

Depois de criar o Modelo de dados de formulário, é possível criar um Forms adaptável com base nesse modelo e usar os métodos de envio do Modelo de dados de formulário para criar o Lead no SFDC.

## AEM Forms 6.4 {#aem-forms-1}

* Criar fonte de dados

   * [Navegar até Fontes de Dados](http://localhost:4502/libs/fd/fdm/gui/components/admin/fdmcloudservice/fdm.html/conf/global)

   * Clique no botão &quot;Criar&quot;
   * Fornecer alguns valores significativos

      * Nome: CreateLeadInSalesForce
      * Título: CreateLeadInSalesForce
      * Tipo de serviço: Serviço RESTful
   * Clique em Avançar
   * Fonte do Swagger: Arquivo
   * Navegue e selecione o arquivo swagger que você baixou na etapa anterior
   * Tipo de autenticação: OAuth 2.0. Especifique os seguintes valores
   * Forneça os valores ClientID e Client Secret
   * O URL OAuth é - **https://login.salesforce.com/services/oauth2/authorize**
   * Atualizar URL do token - **https://na5.salesforce.com/services/oauth2/token**
   * token de acesso Ur **l - https://na5.salesforce.com/services/oauth2/token**
   * Âmbito da autorização: ** api chatter_api full id openid refresh_token visual web**
   * Manipulador de autenticação: Portador de Autorização
   * Clique no botão &quot;Conectar-se ao OAuth&quot;. Caso detecte erros, reveja as etapas anteriores para garantir que todas as informações foram digitadas com precisão.


Depois de criar sua Fonte de Dados usando a SalesForce, você poderá criar a Integração de Dados de Formulário usando a Fonte de Dados que acabou de criar. O link de documentação para isso é [here](https://helpx.adobe.com/experience-manager/6-4/forms/using/create-form-data-models.html)

Certifique-se de configurar o Modelo de dados de formulário para incluir o serviço POST para criar um objeto de cliente potencial no SFDC.

Você também precisará configurar o Serviço de Leitura e Gravação para o objeto Lead. Consulte as capturas de tela na parte inferior desta página.

Depois de criar o Modelo de dados de formulário, é possível criar um Forms adaptável com base nesse modelo e usar os métodos de envio do Modelo de dados de formulário para criar o Lead no SFDC.

>[!NOTE]
>
>Verifique se o url no arquivo swagger corresponde à sua região. Por exemplo, o url no arquivo swagger de amostra é &quot;na46.salesforce.com&quot;, pois a conta foi criada na América do Norte. A maneira mais fácil é fazer logon em sua conta do Salesforce e verificar o url.

![sfdc1](assets/sfdc1.gif)

![sfdc2](assets/sfdc2.png)

[SampleSwaggerFile](assets/swagger-sales-force-lead.json)
