---
title: Uso de CAPTCHAs com AEM Adaptive Forms
seo-title: Uso de CAPTCHAs com AEM Adaptive Forms
description: Adicionar e usar um CAPTCHA com AEM Adaptive Forms.
seo-description: Adicionar e usar um CAPTCHA com AEM Adaptive Forms.
feature: '"Adaptive Forms,Workflow"'
topics: integrations
audience: developer
doc-type: technical video
activity: setup
version: 6.4,6.5
uuid: bd63e207-4f4d-4f34-9ac4-7572ed26f646
discoiquuid: 5e184e44-e385-4df7-b7ed-085239f2a642
topic: Desenvolvimento
role: Desenvolvedor
level: Intermediário
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '248'
ht-degree: 0%

---


# Uso de CAPTCHAs com AEM Adaptive Forms{#using-captchas-with-aem-adaptive-forms}

Adicionar e usar um CAPTCHA com AEM Adaptive Forms.

Visite a página [AEM Forms samples](https://forms.enablementadobe.com/content/samples/samples.html?query=0) para obter um link para uma demonstração ao vivo desse recurso.

>[!VIDEO](https://video.tv.adobe.com/v/18336/?quality=9&learn=on)

*Este vídeo aborda o processo de adicionar um CAPTCHA a um Formulário adaptável AEM usando o serviço AEM CAPTCHA integrado, bem como o serviço reCAPTCHA do Google.*

>[!NOTE]
>
>Esse recurso está disponível somente com o AEM 6.3 em diante.

>[!NOTE]
>
>**Para configurar o reCaptcha na instância de publicação, siga as etapas**
>
>Configurar o reCaptach na instância do autor
>
>abra o felix [web console](http://localhost:4502/system/console/bundles) na instância do autor
>
>pesquise o pacote com.adobe.granite.crypto.file
>
>Observe a id do pacote. No meu caso é 20
>
>Navegue até a id do pacote no sistema de arquivos na instância do autor
>
>* &lt;author-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* Copie o HMAC e os arquivos mestre

Abra o [console da Web felix](http://localhost:4502/system/console/bundles) em sua instância de publicação. Procure por um pacote com.adobe.granite.crypto.file. Observe a id do pacote
Navegue até a id do pacote no sistema de arquivos da sua instância de publicação
* &lt;publish-aem-install-dir>/crx-quickstart/launchpad/felix/bundle20/data
* exclua os arquivos HMAC e mestre existentes.
* cole o HMAC e os arquivos mestre copiados da instância do autor

Reinicie o servidor de publicação do AEM

## Materiais de suporte {#supporting-materials}

* [Google reCAPTCHA](https://www.google.com/recaptcha)

