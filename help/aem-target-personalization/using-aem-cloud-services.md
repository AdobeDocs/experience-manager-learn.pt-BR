---
title: Integração do Adobe Experience Manager com o Adobe Target usando o Cloud Services
seo-title: Integrating Adobe Experience Manager (AEM) with Adobe Target using Legacy Cloud Services
description: Passo a passo sobre como integrar o Adobe Experience Manager (AEM) com o Adobe Target usando AEM Cloud Service
seo-description: Step by step walkthrough on how to integrate Adobe Experience Manager (AEM) with Adobe Target using AEM Cloud Service
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '351'
ht-degree: 3%

---


# Uso AEM Cloud Services herdadas

Nesta seção, discutiremos como configurar o Adobe Experience Manager (AEM) com o Adobe Target usando o Cloud Services herdado.

>[!NOTE]
>
> O AEM Legacy Cloud Service com Adobe Target é **only** usado para estabelecer a conexão de back-end direta do Autor do AEM com o Adobe Target, facilitando a publicação de conteúdo do AEM para o Target. O Adobe Launch é usado para expor o Adobe Target na experiência de site aberto ao público oferecida pelo AEM.

Para usar AEM ofertas de Fragmento de experiência para potencializar suas atividades de personalização, vamos prosseguir para o próximo capítulo e integrar o AEM com o Adobe Target usando os serviços de nuvem herdados. Essa integração é necessária para enviar Fragmentos de experiência do AEM para o Target como ofertas HTML/JSON e para manter as ofertas do público alvo sincronizadas com o AEM. Essa integração é necessária para implementar [Cenário 1 discutido na seção de visão geral](./overview.md#personalization-using-aem-experience-fragment).

## Pré-requisitos

* **AEM**

   * AEM criar e publicar instâncias são necessárias para concluir este tutorial. Se você ainda não configurou a instância do AEM, pode seguir as etapas [aqui](./implementation.md#set-up-aem).

* **Experience Cloud**
   * Acesso à Adobe Experience Cloud de suas organizações - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud provisionado com as seguintes soluções
      * [Adobe Target](https://experiencecloud.adobe.com)

      >[!NOTE]
      >
      > O cliente precisa ser provisionado com Experience Platform Launch e Adobe I/O de [Adobe support](https://helpx.adobe.com/br/contact/enterprise-support.ec.html) ou entre em contato com o administrador do sistema


### Integração do AEM com o Adobe Target

>[!VIDEO](https://video.tv.adobe.com/v/28428?quality=12&learn=on)

1. Criar o Adobe Target Cloud Service usando a Autenticação Adobe IMS (*Usa a API do Adobe Target*) (00:34)
2. Obter o código de cliente Adobe Target (01:50)
3. Criar configuração Adobe IMS para Adobe Target (02:08)
4. Crie uma conta técnica para acessar a API do Target no console do Adobe I/O (02:08)
5. Adicionar o Adobe Target Cloud Service AEM aos Fragmentos de experiência (04:12)

Neste ponto, você integrou [AEM com o Adobe Target com êxito usando o Legacy Cloud Services](./using-aem-cloud-services.md#integrating-aem-target-options) conforme detalhado na Opção 2. Agora é possível criar um Fragmento de experiência no AEM, publicar o Fragmento de experiência como oferta HTML ou Oferta JSON para o Adobe Target e pode ser usado para criar uma atividade.
