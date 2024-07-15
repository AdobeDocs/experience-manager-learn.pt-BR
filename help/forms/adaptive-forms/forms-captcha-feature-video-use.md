---
title: Uso de CAPTCHAs com AEM Adaptive Forms
description: Adicionar e usar um CAPTCHA com AEM Adaptive Forms.
feature: Adaptive Forms,Workflow
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 7e5dcc6e-fe56-49af-97e3-7dfaa9c8738f
last-substantial-update: 2019-06-09T00:00:00Z
duration: 260
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '196'
ht-degree: 0%

---

# Uso de CAPTCHAs com AEM Adaptive Forms{#using-captchas-with-aem-adaptive-forms}

Adicionar e usar um CAPTCHA com AEM Adaptive Forms.

>[!VIDEO](https://video.tv.adobe.com/v/18336?quality=12&learn=on)

*Este vídeo aborda o processo de adicionar um CAPTCHA a um Formulário adaptável AEM usando o serviço AEM CAPTCHA integrado, bem como o serviço reCAPTCHA da Google.*

>[!NOTE]
>
>Esse recurso está disponível somente a partir do AEM 6.3.

>[!NOTE]
>
>**Para configurar o reCaptcha na instância de publicação, siga as etapas**
>
>Configurar reCaptach na instância do autor
>
>abra o Felix [web console](http://localhost:4502/system/console/bundles) na instância do autor
>
>pesquisar pacote com.adobe.granite.crypto.file
>
>Observe a id do pacote. No meu caso, são 20
>
>Navegue até a id do pacote no sistema de arquivos na instância do autor
>
>* &lt;author-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* Copie os arquivos HMAC e mestre
>
Abra o [felix web console](http://localhost:4502/system/console/bundles) na sua instância de publicação. Pesquise por pacote com.adobe.granite.crypto.file. Observe a id do pacote
>
Navegue até a id do pacote no sistema de arquivos da instância de publicação
>
* &lt;publish-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* exclua os arquivos HMAC e master existentes.
* cole os arquivos HMAC e mestres copiados da instância do autor
>
Reinicie o servidor de publicação AEM

## Materiais de suporte {#supporting-materials}

* [reCAPTCHA do Google](https://www.google.com/recaptcha)
