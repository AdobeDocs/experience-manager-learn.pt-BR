---
title: Integração do Adobe Experience Manager com o Adobe Target usando Cloud Services
seo-title: Integração do Adobe Experience Manager (AEM) com o Adobe Target usando Cloud Services herdados
description: Passo a passo sobre como integrar o Adobe Experience Manager (AEM) com o Adobe Target usando AEM Cloud Service
seo-description: Passo a passo sobre como integrar o Adobe Experience Manager (AEM) com o Adobe Target usando AEM Cloud Service
translation-type: tm+mt
source-git-commit: 0443c8ff42e773021ff8b6e969f5c1c31eea3ae4
workflow-type: tm+mt
source-wordcount: '383'
ht-degree: 3%

---


# Uso AEM Cloud Services herdados

Nesta seção, discutiremos como configurar o Adobe Experience Manager (AEM) com a Adobe Target usando Cloud Services herdados.

>[!NOTE]
>
> O AEM Legacy Cloud Service com Adobe Target é **only** usado para estabelecer uma conexão back-end direta do autor de AEM com o Adobe Target, facilitando a publicação de conteúdo de AEM para Público alvo. O Adobe Launch é usado para expor a Adobe Target na experiência de site público, oferecida pela AEM.

Para usar AEM ofertas de fragmento de experiência para potencializar as atividades de personalização, vamos para o próximo capítulo e integremos AEM com a Adobe Target usando os serviços de nuvem herdados. Essa integração é necessária para enviar Fragmentos de experiência de AEM para Público alvo como ofertas HTML/JSON e para manter as ofertas do público alvo sincronizadas com AEM. Essa integração é necessária para implementar [Cenário 1 discutido na seção de visão geral](./overview.md#personalization-using-aem-experience-fragment).

## Pré-requisitos

* **AEM**

   * AEM instância de autor e publicação são necessárias para concluir este tutorial. Se você ainda não configurou sua instância AEM, siga as etapas [here](./implementation.md#set-up-aem).

* **Experience Cloud**
   * Acesso à Adobe Experience Cloud de suas organizações - <https://>`<yourcompany>`.experience.ecloud.adobe.com
   * Experience Cloud fornecido com as seguintes soluções
      * [Adobe Target](https://experiencecloud.adobe.com)

      >[!NOTE]
      >
      > O cliente precisa ser provisionado com o Experience Platform Launch e o Adobe I/O a partir de [suporte ao Adobe](https://helpx.adobe.com/br/contact/enterprise-support.ec.html) ou entre em contato com o administrador do sistema



### Integração de AEM com o Adobe Target

>[!VIDEO](https://video.tv.adobe.com/v/28428?quality=12&learn=on)

1. Crie o Adobe Target Cloud Service usando a Autenticação Adobe IMS (*Usa a API do Adobe Target*) (00:34)
2. Obter o código do cliente Adobe Target (01:50)
3. Criar configuração Adobe IMS para Adobe Target (02:08)
4. Criar uma conta técnica para acessar a API de Público alvo no Adobe I/O Console (02:08)
5. Adicionar o Adobe Target Cloud Service para AEM Fragmentos de experiência (04:12)

Neste ponto, você integrou [AEM com êxito ao Adobe Target usando Cloud Services herdados](./using-aem-cloud-services.md#integrating-aem-target-options) conforme detalhado na Opção 2. Agora você pode criar um Fragmento de experiência dentro do AEM e publicar o Fragmento de experiência como oferta HTML ou Oferta JSON no Adobe Target, e então pode ser usado para criar uma atividade.
