---
title: Integrar o AEM Forms as a Cloud Service e o Marketo (Parte 3)
description: Saiba como integrar o AEM Forms e o Marketo usando o Modelo de dados de formulário do AEM Forms.
feature: Form Data Model,Integration
version: Experience Manager as a Cloud Service
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="Integração" type="positive"
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
last-substantial-update: 2024-07-24T00:00:00Z
jira: KT-15876
exl-id: 43737765-b1ea-4594-853a-d78f41136b5e
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '237'
ht-degree: 2%

---

# Criar modelo de dados do formulário

Depois de configurar a fonte de dados, a próxima etapa é criar um Modelo de dados de formulário com base na fonte de dados configurada na etapa anterior. Para criar o modelo de dados de formulário, siga as seguintes etapas:

Aponte seu navegador para a página de [integrações de dados.](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments-fdm) Isso lista todas as integrações de dados criadas na sua instância do AEM.

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
   ![resultados do teste](assets/testresults.png)

Agora é possível criar um Formulário adaptável com base nesse Modelo de dados de formulário para inserir e buscar objetos do Marketo.
