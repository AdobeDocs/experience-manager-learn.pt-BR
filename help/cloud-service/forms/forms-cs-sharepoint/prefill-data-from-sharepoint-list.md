---
title: Preencher formulário adaptável previamente com dados da lista do SharePoint
description: Saiba como preencher previamente o formulário adaptável usando o modelo de dados de formulário compatível com a lista de pontos de compartilhamento
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Cloud Service
topic: Integrations
jira: KT-14795
duration: 46
exl-id: 9abe9f9d-8fb3-4e01-a830-1dad1c27274d
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '234'
ht-degree: 0%

---

# Preencher previamente o formulário adaptável com os dados da lista de pontos de compartilhamento

Na versão anterior do Formulário AEM (6.5), o código personalizado precisava ser gravado para preencher previamente o formulário adaptável com suporte ao modelo de dados de formulário usando o atributo de solicitação. No AEM Forms as a cloud service, a necessidade de gravar código personalizado não é mais necessária.

Este artigo explica as etapas necessárias para preencher/preencher previamente o formulário adaptável com dados obtidos da lista do SharePoint usando o serviço de preenchimento do modelo de dados de formulário.

Este artigo supõe que você tenha [configurado com êxito um formulário adaptável para enviar dados à lista do SharePoint.](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/adaptive-forms-authoring/authoring-adaptive-forms-core-components/create-an-adaptive-form-on-forms-cs/configure-submit-actions-core-components.html?lang=en#connect-af-sharepoint-list)

A seguir estão os dados na lista do SharePoint
![lista do sharepoint](assets/list-data.png)

Para preencher previamente um formulário adaptável com os dados associados a um guid específico, as etapas a seguir precisam ser executadas

## Configurar o serviço get

* Criar um serviço get para o objeto de nível superior do modelo de dados de formulário usando o atributo guid
  ![obter-serviço](assets/mapping-request-attribute.png)

Nesta captura de tela, a coluna guid é associada por meio de um atributo de solicitação chamado `submissionid`.

O serviço de obtenção totalmente configurado tem esta aparência

![obter-serviço](assets/fdm-request-attribute.png)

## Configurar o formulário adaptável para usar o serviço de preenchimento do modelo de dados de formulário

* Abra um formulário adaptável com base no modelo de dados de formulário da lista de pontos de compartilhamento. Associar o serviço de Preenchimento de modelo de dados de formulário
  ![serviço de preenchimento prévio de formulário](assets/form-prefill-service.png)

## Testar o formulário

Pré-visualize o formulário incluindo o `submissionid` na URL, como mostrado abaixo

```html
http://localhost:4502/content/dam/formsanddocuments/contactusform/jcr:content?wcmmode=disabled&submissionid=57e12249-751a-4a38-a81f-0a4422b24412
```
