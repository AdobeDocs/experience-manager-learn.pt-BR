---
title: Uso de CAPTCHAs com AEM Adaptive Forms
description: Adicionar e usar um CAPTCHA com AEM Adaptive Forms.
feature: Adaptive Forms,Workflow
version: 6.4,6.5
topic: Development
role: Developer
level: Intermediate
exl-id: 7e5dcc6e-fe56-49af-97e3-7dfaa9c8738f
source-git-commit: 9529b1f6d1a863fc570822c8ecd6c4be01b36729
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Uso de CAPTCHAs com AEM Adaptive Forms{#using-captchas-with-aem-adaptive-forms}

Adicionar e usar um CAPTCHA com AEM Adaptive Forms.

>[!VIDEO](https://video.tv.adobe.com/v/18336/?quality=9&learn=on)

*Este vídeo aborda o processo de adicionar um CAPTCHA a um Formulário adaptável AEM usando o serviço integrado AEM CAPTCHA e o serviço Google reCAPTCHA.*

>[!NOTE]
>
>Este recurso está disponível somente a partir do AEM 6.3.

>[!NOTE]
>
>**Para configurar o reCaptcha na instância de publicação, siga as etapas**
>
>Configurar o reCaptach na instância do autor
>
>abra o Felix [console da web](http://localhost:4502/system/console/bundles) na instância do autor
>
>pesquise o pacote com.adobe.granite.crypto.file
>
>Observe a id do pacote. No meu caso é 20
>
>Navegue até a id do pacote no sistema de arquivos na instância do autor
>
>* &lt;author-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* Copie o HMAC e os arquivos principais
Abra o [console da web felix](http://localhost:4502/system/console/bundles) na sua instância de publicação. Procure por um pacote com.adobe.granite.crypto.file. Observe a id do pacote
Navegue até a id do pacote no sistema de arquivos da sua instância de publicação
* &lt;publish-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* exclua os arquivos HMAC e principais existentes.
* cole o HMAC e os arquivos principais copiados da instância do autor

Reinicie o servidor de publicação de AEM

## Materiais de apoio {#supporting-materials}

* [Google reCAPTCHA](https://www.google.com/recaptcha)
