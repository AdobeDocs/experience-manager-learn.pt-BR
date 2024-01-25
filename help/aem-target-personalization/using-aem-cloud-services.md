---
title: Integração do Adobe Experience Manager com o Adobe Target usando o Cloud Service
description: Apresentação passo a passo sobre como integrar o Adobe Experience Manager (AEM) ao Adobe Target usando o AEM Cloud Service
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="Integração" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 9b191211-2030-4b62-acad-c7eb45b807ca
duration: 346
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '358'
ht-degree: 1%

---

# Uso de Cloud Service herdados do AEM

Nesta seção, discutiremos como configurar o Adobe Experience Manager (AEM) com o Adobe Target usando Cloud Service herdados.

>[!NOTE]
>
> O Cloud Service herdado do AEM com Adobe Target é **somente** usado para estabelecer conexão de back-end direta entre AEM Author e Adobe Target, facilitando a publicação de conteúdo do AEM para o Target. O Adobe Launch é usado para expor o Adobe Target na experiência do site voltada para o público, veiculada pelo AEM.

Para usar as ofertas de Fragmento de experiência do AEM para potencializar suas atividades de personalização, vamos prosseguir para o próximo capítulo e integrar o AEM ao Adobe Target usando os serviços de nuvem herdados. Essa integração é necessária para enviar Fragmentos de experiência do AEM para o Target como ofertas HTML/JSON e manter as ofertas do Target em sincronia com o AEM. Essa integração é necessária para a implementação do [O cenário 1 foi discutido na seção de visão geral](./overview.md#personalization-using-aem-experience-fragment).

## Pré-requisitos

* **AEM**

   * Autor e instância de publicação do AEM são necessários para concluir este tutorial. Se ainda não tiver configurado a instância do AEM, siga as etapas [aqui](./implementation.md#set-up-aem).

* **Experience Cloud**
   * Acesso à Adobe Experience Cloud de suas organizações - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud provisionado com as seguintes soluções
      * [Adobe Target](https://experiencecloud.adobe.com)

     >[!NOTE]
     >
     > O cliente precisa ser provisionado com Experience Platform Launch e Adobe I/O do [suporte para Adobe](https://helpx.adobe.com/br/contact/enterprise-support.ec.html) ou entre em contato com o administrador do sistema

### Integração do AEM ao Adobe Target

>[!VIDEO](https://video.tv.adobe.com/v/28428?quality=12&learn=on)

1. Criar Cloud Service do Adobe Target usando a autenticação do Adobe IMS (*Usa a API do Adobe Target*) (00:34)
2. Obter código de cliente do Adobe Target (01:50)
3. Criar configuração do Adobe IMS para o Adobe Target (02:08)
4. Crie uma conta técnica para acessar a API do Target no Console do Adobe I/O (02:08)
5. Adicionar Cloud Service do Adobe Target aos Fragmentos de experiência do AEM (04:12)

Neste ponto, você integrou o com êxito [AEM com Adobe Target usando Cloud Service herdados](./using-aem-cloud-services.md#integrating-aem-target-options) conforme descrito na opção 2. Agora é possível criar um Fragmento de experiência no AEM e publicar o Fragmento de experiência como oferta HTML ou Oferta JSON no Adobe Target, e depois usá-lo para criar uma atividade.
