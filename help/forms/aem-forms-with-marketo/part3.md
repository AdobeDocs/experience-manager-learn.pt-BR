---
title: AEM Forms com Marketo (Parte 3)
seo-title: AEM Forms com Marketo (Parte 3)
description: Tutorial para integrar o AEM Forms com o Marketo usando o AEM Forms Form Data Model.
seo-description: Tutorial para integrar o AEM Forms com o Marketo usando o AEM Forms Form Data Model.
feature: Adaptive Forms, Form Data Model
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
topic: Development
role: Developer
level: Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '405'
ht-degree: 1%

---


# Configurar fonte de dados

A integração de dados do AEM Forms permite configurar e se conectar a fontes de dados diferentes. Os seguintes tipos são suportados prontos para uso. No entanto, com uma pequena personalização, também é possível integrar com outras fontes de dados.

1. Bancos de dados relacionais - MySQL, Microsoft SQL Server, IBM DB2 e Oracle RDBMS
1. Perfil de usuário do AEM
1. Serviços Web RESTful
1. Serviços Web baseados em SOAP
1. Serviços OData

Para a integração do AEM Forms com o Marketo, usaremos os serviços Web RESTful. A primeira etapa na integração é configurar uma fonte de dados [.](https://helpx.adobe.com/experience-manager/6-4/forms/using/configure-data-sources.html#ConfigureRESTfulwebservices) Use o arquivo swagger fornecido como parte deste tutorial. A captura de tela a seguir mostra as propriedades importantes que precisam ser especificadas ao configurar a fonte de dados.
![datasource](assets/datasource.jfif)

O &quot;marketo.json&quot; é o arquivo de troca e é fornecido a você como parte dos ativos deste tutorial.
O Host de propriedade é específico para sua instância do Marketo.
O Tipo de autenticação é personalizado e a Implementação de autenticação deve corresponder ao &quot;AemForms com Marketo&quot;. (A menos que você tenha alterado isso em seu código).

## Criar modelo de dados do formulário

Depois de configurar a fonte de dados, a próxima etapa é criar um Modelo de dados de formulário baseado na fonte de dados configurada na etapa anterior. Para criar um Modelo de dados de formulário, siga as seguintes etapas:

Aponte seu navegador para a página de [integrações de dados.](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm) Isso lista todas as integrações de dados criadas na instância do AEM.

1. Clique em Criar | Modelo de dados de formulário
1. Forneça um título significativo, como FormsAndMarketo, e clique em Avançar
1. Selecione a fonte de dados que foi configurada na etapa anterior e clique em criar e editar para abrir o Modelo de dados de formulário no modo de edição
1. Expanda o nó &quot;FormsAndMarketo&quot;. Expanda o nó Serviços
1. Selecione a primeira operação &quot;Get&quot;
1. Clique em Adicionar selecionados
1. Clique em &quot;Selecionar tudo&quot; na caixa de diálogo &quot;Adicionar objetos de modelo associados&quot; e, em seguida, clique em Adicionar
1. Salve o modelo de dados do formulário clicando no botão Save
1. Guia para a guia Serviços
1. Selecione o único serviço listado e clique em Testar serviço
1. Forneça um leadId válido e clique em Testar. Se tudo correr bem, você deverá retornar os detalhes do cliente potencial, como mostrado na captura de tela abaixo
   ![resultados dos testes](assets/testresults.jfif)
