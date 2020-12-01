---
title: AEM Forms com Marketo (Parte 3)
seo-title: AEM Forms com Marketo (Parte 3)
description: Tutorial para integrar o AEM Forms ao Marketing usando o Modelo de dados de formulário AEM Forms.
seo-description: Tutorial para integrar o AEM Forms ao Marketing usando o Modelo de dados de formulário AEM Forms.
feature: adaptive-forms, form-data-model
topics: integrations
audience: developer
doc-type: tutorial
activity: setup
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: 3d54a8158d0564a3289a2100bbbc59e5ae38f175
workflow-type: tm+mt
source-wordcount: '397'
ht-degree: 1%

---


# Configurar fonte de dados

A Integração de dados da AEM Forms permite que você configure e conecte-se a fontes de dados diferentes. Os seguintes tipos são suportados prontamente. No entanto, com uma pequena personalização, você também pode se integrar com outras fontes de dados.

1. Bancos de dados relacionais - MySQL, Microsoft SQL Server, IBM DB2 e Oracle RDBMS
1. perfil do usuário AEM
1. Serviços Web RESTful
1. Serviços Web baseados em SOAP
1. Serviços OData

Para a integração do AEM Forms com o Marketing, usaremos os serviços Web RESTful. A primeira etapa da integração é configurar uma fonte de dados [.](https://helpx.adobe.com/experience-manager/6-4/forms/using/configure-data-sources.html#ConfigureRESTfulwebservices) Use o arquivo swagger fornecido como parte deste tutorial. A captura de tela a seguir mostra as propriedades importantes que precisam ser especificadas ao configurar a fonte de dados.
![fonte de dados](assets/datasource.jfif)

O &quot;marketo.json&quot; é o arquivo swagger e é fornecido a você como parte dos ativos deste tutorial.
O Host da propriedade é específico para a sua instância de Marketo.
O Tipo de autenticação é personalizado e a Implementação de autenticação deve corresponder a &quot;AemForms com marketing&quot;. (A menos que você tenha alterado isso em seu código).

## Criar modelo de dados do formulário

Depois disso, configurar a fonte de dados na próxima etapa é criar um Modelo de dados de formulário que tenha por base a fonte de dados configurada na etapa anterior. Para criar um Modelo de dados de formulário, siga as seguintes etapas:

Aponte seu navegador para a página [integrações de dados.](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm) Isso lista todas as integrações de dados criadas em sua instância AEM.

1. Clique em Criar | Modelo de dados de formulário
1. Forneça um título significativo, como FormsAndMarketo, e clique em Avançar
1. Selecione a fonte de dados configurada na etapa anterior e clique em criar e editar para abrir o Modelo de dados de formulário no modo de edição
1. Expanda o nó &quot;FormsAndMarketo&quot;. Expandir o nó Serviços
1. Selecione a primeira operação &quot;Obter&quot;
1. Clique em Adicionar selecionados
1. Clique em &quot;Selecionar tudo&quot; na caixa de diálogo &quot;Adicionar objetos de modelo associados&quot; e clique em Adicionar
1. Salve o modelo de dados do formulário clicando no botão Salvar
1. Guia na guia Serviços
1. Selecione o único serviço listado e clique em Test Service (Serviço de teste)
1. Forneça uma leadId válida e clique em Testar. Se tudo correr bem, você deve obter os detalhes principais, como mostrado na captura de tela abaixo
   ![resultados](assets/testresults.jfif)
