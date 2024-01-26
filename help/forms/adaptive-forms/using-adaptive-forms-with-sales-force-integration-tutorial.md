---
title: Configuração do DataSource com o Salesforce no AEM Forms 6.3 e 6.4
description: Integração do AEM Forms com o Salesforce usando o modelo de dados de formulário
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 7a4fd109-514a-41a8-a3fe-53c1de32cb6d
last-substantial-update: 2020-02-14T00:00:00Z
duration: 228
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '816'
ht-degree: 0%

---

# Configuração do DataSource com o Salesforce no AEM Forms 6.3 e 6.4{#configuring-datasource-with-salesforce-in-aem-forms-and}

## Pré-requisitos {#prerequisites}

Neste artigo, abordamos o processo de criação da Fonte de dados com o Salesforce

Pré-requisitos para este tutorial:

* Role até o final desta página, baixe o arquivo Swagger e salve-o no disco rígido.
* AEM Forms com SSL ativado

   * [Documentação oficial para habilitar o SSL no AEM 6.3](https://helpx.adobe.com/experience-manager/6-3/sites/administering/using/ssl-by-default.html)
   * [Documentação oficial para habilitar o SSL no AEM 6.4](https://helpx.adobe.com/experience-manager/6-4/sites/administering/using/ssl-by-default.html)

* Você precisa ter uma conta do Salesforce
* É necessário criar um Aplicativo Conectado. A documentação oficial do Salesforce para criar o aplicativo está listada [aqui](https://help.salesforce.com/articleView?id=connected_app_create.htm&amp;type=0).
* Fornecer escopos OAuth apropriados para o aplicativo (selecionei todos os escopos OAuth disponíveis para fins de teste)
* Forneça a URL de retorno de chamada. No meu caso, o URL de retorno de chamada era

   * Se você estiver usando **AEM Forms 6.3**, a URL de retorno de chamada é https://gbedekar-w7-1:6443/etc/cloudservices/fdm/createlead.html. Nesse cabeçalho de criação de URL está o nome do meu modelo de dados de formulário.

   * Se estiver usando o** AEM Forms 6.4**, o URL de retorno de chamada será https://gbedekar-w7-:6443/libs/fd/fdm/gui/components/admin/fdmcloudservice/createcloudconfigwizard/cloudservices.html

Neste exemplo, gbedekar -w7-1:6443 é o nome do meu servidor e a porta na qual o AEM está sendo executado.

Depois de criar o Aplicativo conectado, observe que **Chave do consumidor e Chave secreta**. Você precisa delas ao criar a fonte de dados no AEM Forms.

Agora que você criou o aplicativo conectado, é necessário criar um arquivo swagger para as operações que você precisa executar no salesforce. Um exemplo de arquivo Swagger é incluído como parte dos ativos baixáveis. Esse arquivo Swagger permite criar um objeto &quot;Lead&quot; no envio do Formulário adaptável. Por favor, explore este arquivo Swagger.

A próxima etapa é criar uma Fonte de dados no AEM Forms. Siga as etapas a seguir de acordo com a versão do AEM Forms

## AEM Forms 6.3 {#aem-forms}

* Faça logon no AEM Forms usando o protocolo https
* Navegue até os serviços em nuvem digitando em https://&lt;servername>:&lt;serverport> /etc/cloudservices.html, Por exemplo, https://gbedekar-w7-1:6443/etc/cloudservices.html
* Role para baixo até &quot;Modelo de dados de formulário&quot;.
* Clique em &quot;Mostrar configurações&quot;.
* Clique em &quot;+&quot; para adicionar nova configuração
* Selecione &quot;Serviço completo Rest&quot;. Forneça Título e Nome significativos para a configuração. Por exemplo,

   * Nome: CreateLeadInSalesForce
   * Título: CreateLeadInSalesForce

* Clique em &quot;Criar&quot;

**Na próxima tela **

* Selecione &quot;Arquivo&quot; como a opção para o arquivo de origem do swagger. Navegue até o arquivo que você baixou anteriormente
* Selecione o tipo de autenticação como OAuth2.0
* Fornecer os valores de ID do cliente e Segredo do cliente
* O URL do OAuth é - **https://login.salesforce.com/services/oauth2/authorize**
* URL do token de atualização - **https://na5.salesforce.com/services/oauth2/token**
* **URL do token de acesso - https://na5.salesforce.com/services/oauth2/token**
* Escopo da autorização: ** api chatter_api id completa openid refresh_token visualforce web**
* Manipulador de autenticação: portador de autorização
* Clique em &quot;Conectar ao OAUTH&quot;. Se tudo correr bem, você não deve ver nenhum erro

Depois de criar seu Modelo de dados de formulário usando o Salesforce, você poderá criar a Integração de dados de formulário usando a Fonte de dados que acabou de criar. A documentação oficial para criar a Integração de dados do formulário é [aqui](https://helpx.adobe.com/aem-forms/6-3/data-integration.html).

Certifique-se de configurar o Modelo de dados de formulário para incluir o serviço POST para criar um objeto Lead no SFDC.

Você também precisará configurar o Serviço de Leitura e Gravação para o objeto de cliente potencial. Consulte as capturas de tela na parte inferior desta página.

Depois de criar o modelo de dados de formulário, você pode criar o Adaptive Forms com base nesse modelo e usar os métodos de envio do modelo de dados de formulário para criar um cliente potencial no SFDC.

## AEM Forms 6.4 {#aem-forms-1}

* Criar fonte de dados

   * [Navegar até as Fontes de dados](http://localhost:4502/libs/fd/fdm/gui/components/admin/fdmcloudservice/fdm.html/conf/global)

   * Clique no botão &quot;Criar&quot;
   * Forneça alguns valores significativos

      * Nome: CreateLeadInSalesForce
      * Título: CreateLeadInSalesForce
      * Tipo de serviço: serviço RESTful

   * Clique em Avançar
   * Origem do Swagger: Arquivo
   * Procure e selecione o arquivo Swagger que você baixou na etapa anterior
   * Tipo de autenticação: OAuth 2.0. Especifique os seguintes valores
   * Fornecer os valores de ID do cliente e Segredo do cliente
   * O URL do OAuth é - **https://login.salesforce.com/services/oauth2/authorize**
   * URL do token de atualização - **https://na5.salesforce.com/services/oauth2/token**
   * URL do token de acesso **l - https://na5.salesforce.com/services/oauth2/token**
   * Escopo da autorização: ** api chatter_api id completa openid refresh_token visualforce web**
   * Manipulador de autenticação: portador de autorização
   * Clique no botão &quot;Conectar ao OAuth&quot;. Caso veja algum erro, revise as etapas anteriores para garantir que todas as informações foram inseridas com precisão.

Depois de criar a Fonte de dados usando o SalesForce, você poderá criar a Integração de dados do formulário usando a Fonte de dados recém-criada. O link da documentação para isso é [aqui](https://helpx.adobe.com/experience-manager/6-4/forms/using/create-form-data-models.html)

Certifique-se de configurar o Modelo de dados de formulário para incluir o serviço POST para criar um objeto Lead no SFDC.

Você também precisará configurar o Serviço de Leitura e Gravação para o objeto de cliente potencial. Consulte as capturas de tela na parte inferior desta página.

Depois de criar o modelo de dados de formulário, você pode criar o Adaptive Forms com base nesse modelo e usar os métodos de envio do modelo de dados de formulário para criar um cliente potencial no SFDC.

>[!NOTE]
>
>Verifique se o URL no arquivo swagger corresponde à sua região. Por exemplo, o url no arquivo swagger de amostra é &quot;na46.salesforce.com&quot;, pois a conta foi criada na América do Norte. A maneira mais fácil é fazer logon em sua conta do Salesforce e verificar o URL.

![sfdc1](assets/sfdc1.gif)

![sfdc2](assets/sfdc2.png)

[ArquivoSwaggerDeAmostra](assets/swagger-sales-force-lead.json)
