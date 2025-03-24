---
title: Desenvolvimento da diferença de página no AEM Sites
description: Este vídeo mostra como fornecer estilos personalizados para a funcionalidade Diferença de página do AEM Sites.
feature: Authoring
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
doc-type: Technical Video
exl-id: 7d600b16-bbb3-4f21-ae33-4df59b1bb39d
duration: 281
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 1%

---

# Desenvolvimento da diferença de página {#developing-for-page-difference}

Este vídeo mostra como fornecer estilos personalizados para a funcionalidade Diferença de página do AEM Sites.

## Personalização de estilos de diferenças de página {#customizing-page-difference-styles}

>[!VIDEO](https://video.tv.adobe.com/v/18871?quality=12&learn=on)

>[!NOTE]
>
>Este vídeo adiciona CSS personalizado à biblioteca do cliente we.Retail, em que, conforme essas alterações, devem ser feitas ao projeto AEM Sites do personalizador; no código de exemplo abaixo: `my-project`.

A diferença de página da AEM obtém o CSS OOTB por uma carga direta de `/libs/cq/gui/components/common/admin/diffservice/clientlibs/diffservice/css/htmldiff.css`.

Devido a essa carga direta de CSS em vez de usar uma categoria de biblioteca do cliente, precisamos encontrar outro ponto de injeção para os estilos personalizados, e esse ponto de injeção personalizado é a clientlib de criação do projeto.

Isso tem a vantagem de permitir que essas substituições de estilo personalizadas sejam específicas do locatário.

### Preparar a clientlib de criação {#prepare-the-authoring-clientlib}

Verifique a existência de uma clientlib `authoring` para seu projeto em `/apps/my-project/clientlib/authoring.`

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:jcr="http://www.jcp.org/jcr/1.0"
        jcr:primaryType="cq:ClientLibraryFolder"
        categories="[my-project.authoring]"/>
```

### Fornecer o CSS personalizado {#provide-the-custom-css}

Adicione ao clientlib do projeto `authoring` um `css.txt` que aponte para menos arquivo que fornecerá os estilos de substituição. [Less](https://lesscss.org/) é preferido devido a seus muitos recursos convenientes, incluindo quebra de classe que é utilizada neste exemplo.

```shell
base=./css

htmldiff.less
```

Crie o arquivo `less` que contém as substituições de estilo em `/apps/my-project/clientlibs/authoring/css/htmldiff.less` e forneça os estilos de substituição conforme necessário.

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

### Inclua o CSS de clientlib de criação por meio do componente de página {#include-the-authoring-clientlib-css-via-the-page-component}

Inclua a categoria clientlibs de criação no `/apps/my-project/components/structure/page/customheaderlibs.html` da página base do projeto diretamente antes da tag `</head>` para garantir que os estilos sejam carregados.

Esses estilos devem ser limitados aos modos WCM [!UICONTROL Editar] e [!UICONTROL visualizar].

```xml
<head>
  ...
  <sly data-sly-test="${wcmmode.preview || wcmmode.edit}" 
       data-sly-call="${clientLib.css @ categories='my-project.authoring'}"/>
</head>
```

O resultado final de uma página de comparação com os estilos acima aplicados seria semelhante a este (HTML adicionado e Componente alterado).

![Diferença de página](assets/page-diff.png)

## Recursos adicionais {#additional-resources}

* [Baixar o site de exemplo we.Retail](https://github.com/Adobe-Marketing-Cloud/aem-sample-we-retail/releases)
* [Usando Bibliotecas de Clientes AEM](https://helpx.adobe.com/br/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [Menos documentação de CSS](https://lesscss.org/)
