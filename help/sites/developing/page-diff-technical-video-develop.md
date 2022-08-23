---
title: Desenvolvimento da diferença de página no AEM Sites
description: Este vídeo mostra como fornecer estilos personalizados para a funcionalidade de Diferença de página do AEM Sites.
feature: Authoring
topics: development
audience: developer
doc-type: technical video
activity: develop
version: 6.4, 6.5
topic: Development
role: Developer
level: Beginner
exl-id: 7d600b16-bbb3-4f21-ae33-4df59b1bb39d
source-git-commit: 307ed6cd25d5be1e54145406b206a78ec878d548
workflow-type: tm+mt
source-wordcount: '293'
ht-degree: 4%

---

# Desenvolvimento da diferença de página {#developing-for-page-difference}

Este vídeo mostra como fornecer estilos personalizados para a funcionalidade de Diferença de página do AEM Sites.

## Personalização de estilos de diferença de página {#customizing-page-difference-styles}

>[!VIDEO](https://video.tv.adobe.com/v/18871/?quality=9&learn=on)

>[!NOTE]
>
>Este vídeo adiciona CSS personalizado à biblioteca do cliente We.Retail, onde essas alterações devem ser feitas no projeto AEM Sites do personalizador; no código de exemplo abaixo: `my-project`.

AEM diferença de página obtém o CSS OOTB por meio de um carregamento direto de `/libs/cq/gui/components/common/admin/diffservice/clientlibs/diffservice/css/htmldiff.css`.

Devido a essa carga direta de CSS, em vez de usar uma categoria de biblioteca do cliente, devemos encontrar outro ponto de injeção para os estilos personalizados, e esse ponto de injeção personalizado é a clientlib de criação do projeto.

Isso tem a vantagem de permitir que essas substituições de estilo personalizadas sejam específicas do locatário.

### Preparar a clientlib de criação {#prepare-the-authoring-clientlib}

Garanta a existência de um `authoring` clientlib para seu projeto em `/apps/my-project/clientlib/authoring.`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
        jcr:primaryType="cq:ClientLibraryFolder"
        categories="[my-project.authoring]"/>
```

### Forneça o CSS personalizado {#provide-the-custom-css}

Adicionar ao `authoring` clientlib a `css.txt` que aponta para o arquivo less que fornecerá os estilos de substituição. [Menos](https://lesscss.org/) O é preferido devido a seus vários recursos convenientes, incluindo o encapsulamento de classe, que é aproveitado neste exemplo.

```shell
base=./css

htmldiff.less
```

Crie o `less` arquivo que contém as substituições de estilo em `/apps/my-project/clientlibs/authoring/css/htmldiff.less`e forneça os estilos de sobreposição, conforme necessário.

```css
/* Wrap with body to gives these rules more specificity than the OOTB */
body {

    /* .html-XXXX are the styles that wrap text that has been added or removed */

    .html-added {
        background-color: transparent;
     color: initial;
        text-decoration: none;
     border-bottom: solid 2px limegreen;
    }

    .html-removed {
        background-color: transparent;
     color: initial;
        text-decoration: none;
     border-bottom: solid 2px red;
    }

    /* .cq-component-XXXX require !important as the class these are overriding uses it. */

    .cq-component-changed {
        border: 2px dashed #B9DAFF !important;
        border-radius: 8px;
    }
    
    .cq-component-moved {
        border: 2px solid #B9DAFF !important;
        border-radius: 8px;
    }

    .cq-component-added {
        border: 2px solid #CCEBB8 !important;
        border-radius: 8px;
    }

    .cq-component-removed {
        border: 2px solid #FFCCCC !important;
        border-radius: 8px;
    }
}
```

### Inclua o CSS clientlib de criação por meio do componente página {#include-the-authoring-clientlib-css-via-the-page-component}

Inclua a categoria clientlibs de criação na página base do projeto `/apps/my-project/components/structure/page/customheaderlibs.html` diretamente antes da `</head>` para garantir que os estilos sejam carregados.

Esses estilos devem ser limitados a [!UICONTROL Editar] e [!UICONTROL visualização] Modos WCM.

```xml
<head>
  ...
  <sly data-sly-test="${wcmmode.preview || wcmmode.edit}" 
       data-sly-call="${clientLib.css @ categories='my-project.authoring'}"/>
</head>
```

O resultado final de uma página diff&#39;d com os estilos acima aplicados seria semelhante a este (HTML add e Component changed).

![Diferença da página](assets/page-diff.png)

## Recursos adicionais {#additional-resources}

* [Baixe o site de amostra We.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [Usar AEM bibliotecas de clientes](https://experienceleague.adobe.com/docs/experience-manager-65/developing/introduction/clientlibs.html?lang=pt-BR)
* [Menos documentação de CSS](https://lesscss.org/)
