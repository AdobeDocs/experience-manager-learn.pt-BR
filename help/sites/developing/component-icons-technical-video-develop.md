---
title: Personalização de ícones de componentes no Adobe Experience Manager Sites
description: Os ícones de componentes permitem que os autores identifiquem rapidamente um componente com ícones ou abreviações significativas. Os autores agora podem encontrar os Componentes necessários para criar suas experiências na Web mais rapidamente do que nunca.
topics: components
audience: administrator, developer
doc-type: technical video
activity: develop
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: c85a59a8bd180d5affe2a5bf5939dabfb2776d73
workflow-type: tm+mt
source-wordcount: '375'
ht-degree: 1%

---


# Personalizar ícones de componentes {#developing-component-icons-in-aem-sites}

Os ícones de componentes permitem que os autores identifiquem rapidamente um componente com ícones ou abreviações significativas. Os autores agora podem encontrar os Componentes necessários para criar suas experiências na Web mais rapidamente do que nunca.

>[!VIDEO](https://video.tv.adobe.com/v/16778/?quality=9&learn=on)

O navegador Componente agora é exibido em um tema cinza consistente, exibindo:

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

Por padrão, os primeiros 2 caracteres do título do componente (**[cq:Component]@jcr:title**) são usados como abreviação. Por exemplo, se **[cq:Component]@jcr:title=Article Lista** a abreviação exibiria como &quot;**Ar**&quot;.

A abreviação pode ser personalizada por meio da propriedade **[cq:Component]@abbreviation**. Embora esse valor possa aceitar mais de 2 caracteres, é recomendável limitar a abreviação a 2 caracteres para evitar qualquer perturbação visual.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - abbreviation = "AL"
```

### Ícones de CoralUI {#coralui-icons}

Os ícones CoralUI, fornecidos por AEM, podem ser usados para ícones de componentes. Para configurar um ícone CoralUI, defina uma propriedade **[cq:Component]@cq:icon** para o valor de atributo de ícone HTML do ícone CoralUI desejado (enumerado na documentação [CoralUI](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)).

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - cq:icon = "documentFragment"
```

### Imagens PNG {#png-images}

Imagens PNG podem ser usadas para ícones de componentes. Para configurar uma imagem PNG como um ícone de componente, adicione a imagem desejada como um **nt:file** chamado **cq:icon.png** sob o **[cq:Component]**.

O PNG deve ter um plano de fundo transparente ou uma cor de plano de fundo definida como **#707070**.

As imagens PNG serão dimensionadas para **20px por 20px**. Entretanto, para acomodar exibições de retina **40px** por **40px** podem ser preferíveis.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.png
     - jcr:primaryType = "nt:file"
```

### Imagens SVG {#svg-images}

Imagens SVG (baseadas em vetores) podem ser usadas para ícones de componentes. Para configurar uma imagem SVG como um ícone de componente, adicione o SVG desejado como **nt:file** chamado **cq:icon.svg** sob **[cq:Component]**.

As imagens SVG devem ter uma cor de fundo definida como **#707070** e um tamanho de **20px por 20px.**

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.svg
     - jcr:primaryType = "nt:file"
```

## Recursos adicionais {#additional-resources}

* [Ícones disponíveis do CoralUI](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)
