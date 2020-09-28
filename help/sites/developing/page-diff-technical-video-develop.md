---
title: Desenvolvendo a diferença de página no AEM Sites
description: Este vídeo mostra como fornecer estilos personalizados para a funcionalidade Diferença de página do AEM Sites.
feature: page-diff
topics: development
audience: developer
doc-type: technical video
activity: develop
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '293'
ht-degree: 2%

---


# Desenvolvendo para diferença de página {#developing-for-page-difference}

Este vídeo mostra como fornecer estilos personalizados para a funcionalidade Diferença de página do AEM Sites.

## Personalização de estilos de diferença de página {#customizing-page-difference-styles}

>[!VIDEO](https://video.tv.adobe.com/v/18871/?quality=9&learn=on)

>[!NOTE]
>
>Este vídeo adiciona CSS personalizado à biblioteca do cliente we.Retail, onde essas alterações devem ser feitas no projeto do AEM Sites do personalizador; no código de exemplo abaixo: `my-project`.

AEM diferença de página obtém o CSS OOTB por meio de uma carga direta de `/libs/cq/gui/components/common/admin/diffservice/clientlibs/diffservice/css/htmldiff.css`.

Devido a essa carga direta de CSS em vez de usar uma categoria da biblioteca do cliente, precisamos encontrar outro ponto de injeção para os estilos personalizados, e esse ponto de injeção personalizado é a clientlib de criação do projeto.

Isso tem a vantagem de permitir que essas substituições de estilo personalizadas sejam específicas do locatário.

### Preparar a clientlib de criação {#prepare-the-authoring-clientlib}

Verifique a existência de uma `authoring` clientlib para o seu projeto em `/apps/my-project/clientlib/authoring.`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
        jcr:primaryType="cq:ClientLibraryFolder"
        categories="[my-project.authoring]"/>
```

### Fornecer o CSS personalizado {#provide-the-custom-css}

Adicione à clientlib do projeto um arquivo que aponte para o arquivo menor que fornecerá os estilos de substituição. `authoring` `css.txt` [Menos](https://lesscss.org/) é preferido devido aos seus muitos recursos convenientes, incluindo o encapsulamento de classe, que é aproveitado neste exemplo.

```shell
base=./css

htmldiff.less
```

Crie o `less` arquivo que contém as substituições de estilo em `/apps/my-project/clientlibs/authoring/css/htmldiff.less`e forneça os estilos de substituição conforme necessário.

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

### Incluir o CSS clientlib de criação por meio do componente de página {#include-the-authoring-clientlib-css-via-the-page-component}

Inclua a categoria clientlibs de criação na página base do projeto `/apps/my-project/components/structure/page/customheaderlibs.html` diretamente antes da `</head>` tag para garantir que os estilos sejam carregados.

Esses estilos devem ser limitados aos modos [!UICONTROL Editar] e [!UICONTROL pré-visualização] WCM.

```xml
<head>
  ...
  <sly data-sly-test="${wcmmode.preview || wcmmode.edit}" 
       data-sly-call="${clientLib.css @ categories='my-project.authoring'}"/>
</head>
```

O resultado final de uma página diff com os estilos acima aplicados seria semelhante a este (HTML adicionado e Componente alterado).

![Diferença de página](assets/page-diff.png)

## Recursos adicionais {#additional-resources}

* [Baixe o site de amostra we.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [Uso das bibliotecas do cliente AEM](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [Menos documentação CSS](https://lesscss.org/)
