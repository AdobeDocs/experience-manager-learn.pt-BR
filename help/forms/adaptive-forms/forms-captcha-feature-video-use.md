---
title: Uso de CAPTCHAs com AEM Forms adaptável
seo-title: Uso de CAPTCHAs com AEM Forms adaptável
description: Adicionar e usar um CAPTCHA com AEM Forms adaptável.
seo-description: Adicionar e usar um CAPTCHA com AEM Forms adaptável.
feature: adaptive-forms
topics: integrations
audience: developer
doc-type: technical video
activity: setup
version: 6.4,6.5
uuid: bd63e207-4f4d-4f34-9ac4-7572ed26f646
discoiquuid: 5e184e44-e385-4df7-b7ed-085239f2a642
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '242'
ht-degree: 0%

---


# Uso de CAPTCHAs com AEM Forms{#using-captchas-with-aem-adaptive-forms} adaptável

Adicionar e usar um CAPTCHA com AEM Forms adaptável.

Visite a página [AEM Forms samples](https://forms.enablementadobe.com/content/samples/samples.html?query=0) para obter um link para uma demonstração ao vivo desse recurso.

>[!VIDEO](https://video.tv.adobe.com/v/18336/?quality=9&learn=on)

*Este vídeo aborda o processo de adicionar um CAPTCHA a um formulário adaptável AEM usando o serviço integrado AEM CAPTCHA, bem como o serviço reCAPTCHA do Google.*

>[!NOTE]
>
>Este recurso está disponível somente a partir da AEM 6.3.

>[!NOTE]
>
>**Para configurar o reCaptcha na instância de publicação, siga as etapas**
>
>Configurar reCaptach na instância do autor
>
>abra o felix [console da Web](http://localhost:4502/system/console/bundles) na instância do autor
>
>pesquisar por um pacote com.adobe.granite.crypto.file
>
>Observe a ID do pacote. No meu caso é 20
>
>Navegue até a ID do pacote no sistema de arquivos na instância do autor
>
>* &lt;author-aem-install-dir>/crx-quickstart/launch/felix/bundle20/data
* Copie os arquivos HMAC e principais

Abra o [console da Web felix](http://localhost:4502/system/console/bundles) em sua instância de publicação. Procure por um pacote com.adobe.granite.crypto.file. Observe a ID do pacote
Navegue até a ID do pacote no sistema de arquivos da sua instância de publicação
* &lt;publish-aem-install-dir>/crx-quickstart/launch/felix/bundle20/data
* exclua o HMAC existente e os arquivos principais.
* cole o HMAC e os arquivos principais copiados da instância do autor

Reinicie o servidor de publicação AEM

## Materiais de suporte {#supporting-materials}

* [Google reCAPTCHA](https://www.google.com/recaptcha)

