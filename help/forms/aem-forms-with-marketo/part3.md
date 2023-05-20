---
title: AEM Forms com Marketo (Parte 3)
description: Tutorial para integrar o AEM Forms com o Marketo usando o Modelo de dados do formulário do AEM Forms.
feature: Adaptive Forms, Form Data Model
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 7096340b-8ccf-4f5e-b264-9157232e96ba
source-git-commit: 38e0332ef2ef45a73a81f318975afc25600392a8
workflow-type: tm+mt
source-wordcount: '386'
ht-degree: 1%

---

# Configurar fonte de dados

A Integração de dados do AEM Forms permite configurar e conectar-se a diferentes fontes de dados. Os seguintes tipos são prontos para uso. No entanto, com um pouco de personalização, também é possível integrar o a outras fontes de dados.

1. Bancos de dados relacionais - MySQL, Microsoft SQL Server, IBM DB2 e RDBMS de Oracle
1. Perfil de usuário AEM
1. Serviços Web RESTful
1. Serviços da Web com base em SOAP
1. Serviços OData

Para a integração do AEM Forms com o Marketo, estamos usando os serviços Web RESTful. A primeira etapa da integração é configurar um [fonte de dados.](https://helpx.adobe.com/experience-manager/6-4/forms/using/configure-data-sources.html#ConfigureRESTfulwebservices) Use o arquivo Swagger fornecido como parte deste tutorial. A captura de tela a seguir mostra as propriedades importantes que precisam ser especificadas durante a configuração da fonte de dados.
![fonte de dados](assets/datasource.jfif)

O &quot;marketo.json&quot; é o arquivo swagger e é fornecido como parte dos ativos deste tutorial.
O Host de propriedade é específico para sua instância do Marketo.
O Tipo de autenticação é personalizado e a Implementação de autenticação deve corresponder ao &quot;AemForms With Marketo&quot;. (A menos que você tenha alterado isso no código).

## Criar modelo de dados do formulário

Depois de configurar a fonte de dados, a próxima etapa é criar um Modelo de dados de formulário com base na fonte de dados configurada na etapa anterior. Para criar o modelo de dados de formulário, siga as seguintes etapas:

Aponte seu navegador para a [integração de dados.](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm) Isso lista todas as integrações de dados criadas na instância do AEM.

1. Clique em Criar | Modelo de dados do formulário
1. Forneça um título significativo, como FormsAndMarketo, e clique em Próximo
1. Selecione a fonte de dados configurada na etapa anterior e clique em criar e editar para abrir o Modelo de dados de formulário no modo de edição
1. Expanda o nó &quot;FormsAndMarketo&quot;. Expanda o nó Serviços
1. Selecione a primeira operação &quot;Get&quot;
1. Clique em Adicionar seleção
1. Clique em &quot;Selecionar tudo&quot; na caixa de diálogo &quot;Adicionar objetos de modelo associados&quot; e clique em Adicionar
1. Salve o modelo de dados do formulário clicando no botão Salvar
1. Guia para a guia Serviços
1. Selecione o único serviço listado e clique em Testar serviço
1. Forneça um leadId válido e clique em Testar. Se tudo correr bem, você deve obter os detalhes do lead, como mostrado na captura de tela abaixo
   ![resultados do teste](assets/testresults.jfif)

## Próximas etapas

[Tudo junto para testes](./part4.md)

