---
title: Conectar o AEM Sites com a propriedade de tag usando IMS
description: Saiba como conectar o AEM Sites com a propriedade de tag usando a configuração IMS no AEM.
solution: Experience Manager, Data Collection, Experience Platform
jira: KT-5981
thumbnail: 38555.jpg
topic: Integrations
feature: Integrations
role: Developer
level: Intermediate
badgeIntegration: label="Integração" type="positive"
badgeVersions: label="AEM Sites as a Cloud Service, AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 92dbd185-bad4-4a4d-b979-0d8f5d47c54b
duration: 50
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '263'
ht-degree: 1%

---

# Conectar o AEM Sites com a propriedade de tag usando IMS{#connect-aem-and-tag-property-using-ims}

Saiba como conectar o AEM com a Propriedade de tags usando a configuração IMS (Identity Management System) no AEM. Essa configuração autentica o AEM com a API de tags e permite que o AEM se comunique por meio das APIs de tags para acessar as propriedades da tag.

## Criar ou reutilizar configuração IMS

A configuração do IMS usando o projeto Adobe Developer Console é necessária para integrar o AEM à Propriedade de tag recém-criada. Essa configuração permite que o AEM se comunique com o aplicativo de tags usando APIs de tags, e o IMS lida com o aspecto de segurança dessa integração.

Sempre que um ambiente AEM as Cloud Service é provisionado, algumas configurações de IMS, como Asset Compute, Adobe Analytics e tags, são criadas automaticamente. As **tags criadas automaticamente na configuração do IMS do Adobe Experience Platform** podem ser usadas ou uma nova configuração do IMS deve ser criada se você estiver usando o ambiente AEM 6.X.

Revise as **tags criadas automaticamente na configuração IMS do Adobe Experience Platform** usando as seguintes etapas.

1. No AEM, abra o menu **Ferramentas**
1. Na seção Segurança, selecione Configurações do Adobe IMS.
1. Selecione o cartão **Adobe Launch** e clique em **Propriedades**, revise os detalhes nas guias **Certificado** e **Conta**. Em seguida, clique em **Cancelar** para retornar sem modificar os detalhes criados automaticamente.
1. Selecione o cartão **Adobe Launch** e desta vez clique em **Verificar integridade**. Você deverá ver a mensagem **Success** como abaixo.

   ![Configuração IMS Íntegra das Marcas](assets/adobe-launch-healthy-ims-config.png)

## Próximas etapas

[Criar uma configuração de Cloud Service de tags no AEM](create-aem-launch-cloud-service.md)
