---
title: Uso de CAPTCHAs com AEM Adaptive Forms
description: Adicionar e usar um CAPTCHA com AEM Adaptive Forms.
feature: Adaptive Forms,Workflow
version: 6.4,6.5
topic: Desenvolvimento
role: Developer
level: Intermediate
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '232'
ht-degree: 0%

---


# Uso de CAPTCHAs com AEM Adaptive Forms{#using-captchas-with-aem-adaptive-forms}

Adicionar e usar um CAPTCHA com AEM Adaptive Forms.

Visite a página [AEM Forms samples](https://forms.enablementadobe.com/content/samples/samples.html?query=0#collapse1) para obter um link para uma demonstração ao vivo desse recurso.

>[!VIDEO](https://video.tv.adobe.com/v/18336/?quality=9&learn=on)

*Este vídeo aborda o processo de adicionar um CAPTCHA a um Formulário adaptável AEM usando o serviço AEM CAPTCHA integrado, bem como o serviço reCAPTCHA do Google.*

>[!NOTE]
>
>Este recurso está disponível somente a partir do AEM 6.3.

>[!NOTE]
>
>**Para configurar o reCaptcha na instância de publicação, siga as etapas**
>
>Configurar o reCaptach na instância do autor
>
>abra o Felix [web console](http://localhost:4502/system/console/bundles) na instância do autor
>
>pesquise o pacote com.adobe.granite.crypto.file
>
>Observe a id do pacote. No meu caso é 20
>
>Navegue até a id do pacote no sistema de arquivos na instância do autor
>
>* &lt;author-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* Copie o HMAC e os arquivos principais

Abra o [console da Web felix](http://localhost:4502/system/console/bundles) em sua instância de publicação. Procure por um pacote com.adobe.granite.crypto.file. Observe a id do pacote
Navegue até a id do pacote no sistema de arquivos da sua instância de publicação
* &lt;publish-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* exclua os arquivos HMAC e principais existentes.
* cole o HMAC e os arquivos principais copiados da instância do autor

Reinicie o servidor de publicação de AEM

## Materiais de apoio {#supporting-materials}

* [Google reCAPTCHA](https://www.google.com/recaptcha)

