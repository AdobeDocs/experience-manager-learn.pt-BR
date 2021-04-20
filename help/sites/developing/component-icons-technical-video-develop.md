---
title: Personalização de ícones de componentes no Adobe Experience Manager Sites
description: Os ícones de componentes permitem que os autores identifiquem rapidamente um componente com ícones ou abreviações significativas. Agora os autores podem encontrar os Componentes necessários para criar suas experiências da Web mais rápido do que nunca.
topics: components
audience: administrator, developer
doc-type: technical video
activity: develop
version: 6.3, 6.4, 6.5
feature: Core Components
topic: Development
role: Business Practitioner
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '381'
ht-degree: 2%

---


# Personalizar ícones de componentes {#developing-component-icons-in-aem-sites}

Os ícones de componentes permitem que os autores identifiquem rapidamente um componente com ícones ou abreviações significativas. Agora os autores podem encontrar os Componentes necessários para criar suas experiências da Web mais rápido do que nunca.

>[!VIDEO](https://video.tv.adobe.com/v/16778/?quality=9&learn=on)

O navegador de componentes agora é exibido em um tema cinza consistente, exibindo:

* **[!UICONTROL Grupo de componentes]**
* **[!UICONTROL Título do componente]**
* **[!UICONTROL Descrição do componente]**
* **[!UICONTROL Ícone do componente]**
   * As duas primeiras letras do Título do componente *(padrão)*
   * Imagem PNG personalizada *(configurada por um desenvolvedor)*
   * Imagem SVG personalizada *(configurada por um desenvolvedor)*
   * Ícone CoralUI *(configurado por um desenvolvedor)*

## Opções de configuração do ícone de componente {#component-icon-configuration-options}

### Abreviações {#abbreviations}

Por padrão, os 2 primeiros caracteres do título do componente (**[cq:Component]@jcr:title**) são usados como a abreviação. Por exemplo, se **[cq:Component]@jcr:title=Article List** a abreviação seria exibida como &quot;**Ar**&quot;.

A abreviação pode ser personalizada por meio da propriedade **[cq:Component]@abbreviation**. Embora esse valor possa aceitar mais de 2 caracteres, é recomendável limitar a abreviação a 2 caracteres para evitar qualquer perturbação visual.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - abbreviation = "AL"
```

### Ícones de CoralUI {#coralui-icons}

Os ícones CoralUI, fornecidos pelo AEM, podem ser usados para ícones de componentes. Para configurar um ícone CoralUI, defina uma propriedade **[cq:Component]@cq:icon** para o valor do atributo de ícone HTML do ícone CoralUI desejado (enumerado na [documentação CoralUI](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html).

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - cq:icon = "documentFragment"
```

### Imagens PNG {#png-images}

Imagens PNG podem ser usadas para ícones de componentes. Para configurar uma imagem PNG como um ícone de componente, adicione a imagem desejada como um **nt:file** chamado **cq:icon.png** sob o **[cq:Component]**.

O PNG deve ter um plano de fundo transparente ou uma cor de plano de fundo definida como **#707070**.

As imagens PNG serão dimensionadas para **20px por 20px**. No entanto, para acomodar exibições de retina **40px** de **40px** podem ser preferíveis.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.png
     - jcr:primaryType = "nt:file"
```

### Imagens SVG {#svg-images}

Imagens SVG (com base em vetor) podem ser usadas para ícones de componentes. Para configurar uma imagem SVG como um ícone de componente, adicione o SVG desejado como um **nt:file** chamado **cq:icon.svg** sob o **[cq:Component]**.

As imagens SVG devem ter uma cor de fundo definida como **#707070** e um tamanho de **20px por 20px.**

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.svg
     - jcr:primaryType = "nt:file"
```

## Recursos adicionais {#additional-resources}

* [Ícones CoralUI disponíveis](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)
