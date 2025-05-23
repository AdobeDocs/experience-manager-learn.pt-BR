---
title: AEM Forms com Marketo (Parte 3)
description: Tutorial para integrar o AEM Forms com o Marketo usando o Modelo de dados do formulário do AEM Forms.
feature: Adaptive Forms, Form Data Model
version: Experience Manager 6.4, Experience Manager 6.5
topic: Integrations, Development
role: Developer
level: Experienced
badgeIntegration: label="Integração" type="positive"
badgeVersions: label="AEM Forms 6.5" before-title="false"
exl-id: 7096340b-8ccf-4f5e-b264-9157232e96ba
duration: 78
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '217'
ht-degree: 3%

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

## Próximas etapas

[Tudo junto para testes](./part4.md)
