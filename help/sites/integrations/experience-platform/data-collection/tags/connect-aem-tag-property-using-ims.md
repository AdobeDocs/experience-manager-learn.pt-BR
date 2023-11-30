---
title: Conectar o AEM Sites com a propriedade de tag usando IMS
description: Saiba como conectar o AEM Sites com a propriedade de tag usando a configuração IMS no AEM. Essa configuração autentica o AEM com a API do Launch e permite que o AEM se comunique por meio das APIs do Launch para acessar as propriedades da tag.
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
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '313'
ht-degree: 2%

---

# Conectar o AEM Sites com a propriedade de tag usando IMS{#connect-aem-and-tag-property-using-ims}

>[!NOTE]
>
>O processo de renomear o Adobe Experience Platform Launch como um conjunto de tecnologias de coleção de dados está sendo implementado na interface, no conteúdo e na documentação do produto AEM, portanto, o termo Launch ainda está sendo usado aqui.

Saiba como conectar o AEM com a propriedade de tag usando a configuração IMS (Identity Management System) no AEM. Essa configuração autentica o AEM com a API do Launch e permite que o AEM se comunique por meio das APIs do Launch para acessar as propriedades da tag.

## Criar ou reutilizar configuração IMS

A configuração do IMS usando o projeto Adobe Developer Console é necessária para integrar o AEM à Propriedade de tag recém-criada. Essa configuração permite que o AEM se comunique com o aplicativo de tags usando APIs do Launch e o IMS lida com o aspecto de segurança dessa integração.

Sempre que um ambiente AEM as Cloud Service é provisionado, algumas configurações de IMS, como Asset compute, Adobe Analytics e Adobe Launch, são criadas automaticamente. A variável criada automaticamente **Adobe Launch** A configuração IMS pode ser usada ou uma nova configuração IMS deve ser criada se você estiver usando um ambiente AEM 6.X.

Revisar criado automaticamente **Adobe Launch** Configuração do IMS usando as etapas a seguir.

1. No AEM, abra o menu **Ferramentas**

1. Na seção Segurança, selecione Configurações do Adobe IMS.

1. Selecione o **Adobe Launch** e clique em **Propriedades**, revise os detalhes de **Certificado** e **Conta** guias. Clique em **Cancelar** para retornar sem modificar os detalhes criados automaticamente.

1. Selecione o **Adobe Launch** e desta vez clique em **Verificar integridade**, você deverá ver o **Sucesso** como abaixo.

   ![Configuração do IMS íntegro do Adobe Launch](assets/adobe-launch-healthy-ims-config.png)


## Próximas etapas

[Criar uma configuração do Cloud Service do Launch no AEM](create-aem-launch-cloud-service.md)
