---
title: Integração do Adobe Experience Manager com o Adobe Target usando os Serviços na Nuvem
seo-title: Integração do Adobe Experience Manager (AEM) com o Adobe Target usando os Serviços de nuvem herdados
description: Passo a passo sobre como integrar o Adobe Experience Manager (AEM) ao Adobe Target usando o AEM Cloud Service
seo-description: Passo a passo sobre como integrar o Adobe Experience Manager (AEM) ao Adobe Target usando o AEM Cloud Service
feature: Fragmentos de experiência
topic: Personalização
role: Desenvolvedor
level: Intermediário
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '388'
ht-degree: 4%

---


# Uso dos serviços em nuvem herdados do AEM

Nesta seção, discutiremos como configurar o Adobe Experience Manager (AEM) com o Adobe Target usando os Serviços da nuvem herdados.

>[!NOTE]
>
> O AEM Legacy Cloud Service com o Adobe Target é **somente** usado para estabelecer a conexão back-end direta do AEM Author to Adobe Target que facilita a publicação de conteúdo do AEM para o Target. O Adobe Launch é usado para expor o Adobe Target na experiência de site aberto ao público, servida pelo AEM.

Para usar as ofertas de Fragmento de experiência do AEM para potencializar suas atividades de personalização, vamos prosseguir para o próximo capítulo e integrar o AEM com o Adobe Target usando os serviços de nuvem herdados. Essa integração é necessária para enviar Fragmentos de experiência do AEM para o Target como ofertas HTML/JSON e para manter as ofertas de destino sincronizadas com o AEM. Essa integração é necessária para implementar [Cenário 1 discutido na seção de visão geral](./overview.md#personalization-using-aem-experience-fragment).

## Pré-requisitos

* **AEM**

   * O autor e a instância de publicação do AEM são necessários para concluir este tutorial. Se ainda não tiver configurado sua instância do AEM, você poderá seguir as etapas [aqui](./implementation.md#set-up-aem).

* **Experience Cloud**
   * Acesso às suas organizações na Adobe Experience Cloud - <https://>`<yourcompany>`.experiencecloud.adobe.com
   * Experience Cloud provisionada com as seguintes soluções
      * [Adobe Target](https://experiencecloud.adobe.com)

      >[!NOTE]
      >
      > O cliente precisa ser provisionado com o Experience Platform Launch e o Adobe I/O de [Suporte da Adobe](https://helpx.adobe.com/br/contact/enterprise-support.ec.html) ou entre em contato com o administrador do sistema



### Integração do AEM com o Adobe Target

>[!VIDEO](https://video.tv.adobe.com/v/28428?quality=12&learn=on)

1. Criar o Adobe Target Cloud Service usando a autenticação do Adobe IMS (*Usa a API do Adobe Target*) (00:34)
2. Obter o código de cliente do Adobe Target (01:50)
3. Criar configuração do Adobe IMS para o Adobe Target (02:08)
4. Crie uma conta técnica para acessar a API do Target no Console do Adobe I/O (02:08)
5. Adicionar o Adobe Target Cloud Service aos fragmentos de experiência do AEM (04:12)

Neste ponto, você integrou com êxito o [AEM ao Adobe Target usando os Serviços de nuvem herdados](./using-aem-cloud-services.md#integrating-aem-target-options), conforme detalhado na Opção 2. Agora é possível criar um Fragmento de experiência no AEM, publicar o Fragmento de experiência como oferta HTML ou Oferta JSON para o Adobe Target e pode ser usado para criar uma atividade.
