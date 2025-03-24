---
title: Implantar os ativos de amostra
description: Implante os ativos de amostra no sistema pronto para nuvem local.
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Experience Manager as a Cloud Service
topic: Integrations
jira: KT-13717
exl-id: ae8104fa-7af2-49c2-9e6b-704152d49149
duration: 38
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '188'
ht-degree: 0%

---

# Implantar os ativos de amostra

Para que esse caso de uso funcione em seu sistema, implante os seguintes ativos em seu sistema pronto para nuvem local.

* Certifique-se de ter criado todas as configurações/contas necessárias listadas no[documento de introdução](./introduction.md)

* [Instalar o modelo de formulário adaptável personalizado e o componente de página associado](./assets/azure-portal-template-page-component.zip)

* [Instalar o Modelo de Dados de Formulário SendGrid](./assets/send-grid-form-data-model.zip). Será necessário fornecer sua chave de API e o endereço &quot;SendGrid verificado&quot; para que este modelo de dados de formulário funcione. Testar o modelo de dados de formulário no editor de modelo de dados de formulário

* [Instalar o Modelo de Dados de Formulário ](./assets/azure-storage-fdm.zip) com suporte do Azure. Será necessário fornecer suas credenciais da conta de Armazenamento do Azure para que o modelo de dados de formulário funcione. Teste o modelo de dados de formulário no editor de modelo de dados de formulário.

* [Importar a amostra de formulário adaptável](./assets/credit-applications-af.zip)
* [Importar a biblioteca do cliente](./assets/client-lib.zip)
* [Visualizar o formulário](http://localhost:4502/content/dam/formsanddocuments/azureportalstorage/creditapplications/jcr:content?wcmmode=disabled). Insira um email válido e clique no botão Salvar. Os dados do formulário devem ser armazenados no Armazenamento do Azure e um email com um link para o formulário salvo será enviado para o endereço de email especificado.
