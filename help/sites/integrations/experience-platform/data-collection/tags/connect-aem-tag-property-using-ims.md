---
title: Conectar AEM com propriedade de tag usando IMS
description: Saiba como se conectar AEM com a propriedade de tag usando a configuração IMS no AEM. Essa configuração autentica AEM com a API do Launch e permite que o AEM se comunique por meio das APIs do Launch para acessar as propriedades da tag.
topics: integrations
audience: administrator
solution: Experience Manager, Data Collection, Experience Platform
doc-type: technical video
activity: setup
version: Cloud Service
kt: 5981
thumbnail: 38555.jpg
topic: Integrations
role: Developer
level: Intermediate
exl-id: 92dbd185-bad4-4a4d-b979-0d8f5d47c54b
source-git-commit: 18a72187290d26007cdc09c45a050df8f152833b
workflow-type: tm+mt
source-wordcount: '301'
ht-degree: 2%

---

# Conectar AEM com propriedade de tag usando IMS{#connect-aem-and-tag-property-using-ims}

>[!NOTE]
>
>O processo de renomear o Adobe Experience Platform Launch como um conjunto de tecnologias de coleta de dados está sendo implementado na interface do usuário, no conteúdo e na documentação do produto AEM, portanto, o termo Launch ainda está sendo usado aqui.

Saiba como se conectar AEM com a propriedade de tag usando a configuração IMS (Identity Management System) no AEM. Essa configuração autentica AEM com a API do Launch e permite que o AEM se comunique por meio das APIs do Launch para acessar as propriedades da tag.

## Criar ou reutilizar configuração IMS

A configuração do IMS usando o projeto do Console do Adobe Developer é necessária para integrar o AEM com a Propriedade de tag recém-criada. Essa configuração permite que o AEM se comunique com o aplicativo Tags usando APIs do Launch e o IMS lida com o aspecto de segurança dessa integração.

Sempre que um ambiente AEM as Cloud Service é provisionado, algumas configurações do IMS, como Asset compute, Adobe Analytics e Adobe Launch, são criadas automaticamente. A criação automática **Adobe Launch** A configuração IMS pode ser usada ou uma nova configuração IMS deve ser criada se você estiver usando AEM ambiente 6.X.

Revisar criado automaticamente **Adobe Launch** Configuração IMS usando as seguintes etapas.

1. No AEM, abra o menu **Ferramentas**

1. Na seção Segurança, selecione Configurações do Adobe IMS.

1. Selecione o **Adobe Launch** cartão e clique em **Propriedades**, revise os detalhes em **Certificado** e **Conta** guias. Em seguida, clique em **Cancelar** para retornar sem modificar os detalhes criados automaticamente.

1. Selecione o **Adobe Launch** cartão e, desta vez, clique em **Verificar integridade**, você deve ver a variável **Sucesso** como abaixo.

   ![Configuração IMS saudável do Adobe Launch](assets/adobe-launch-healthy-ims-config.png)


## Próximas etapas

[Criar uma configuração do Launch Cloud Service no AEM](create-aem-launch-cloud-service.md)
