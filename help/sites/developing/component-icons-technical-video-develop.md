---
title: Personalização de ícones de componentes no Adobe Experience Manager Sites
description: Os Ícones de componente permitem que os autores identifiquem rapidamente um componente com ícones ou abreviações relevantes. Os autores agora podem localizar os Componentes necessários para criar suas experiências da Web mais rápido do que nunca.
version: 6.4, 6.5
feature: Core Components
topic: Development
role: User
level: Intermediate
doc-type: Technical Video
exl-id: 37dc26aa-0773-4749-8c8b-4544bd4d5e5f
duration: 379
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '361'
ht-degree: 0%

---

# Personalização de ícones de componentes {#developing-component-icons-in-aem-sites}

Os Ícones de componente permitem que os autores identifiquem rapidamente um componente com ícones ou abreviações relevantes. Os autores agora podem localizar os Componentes necessários para criar suas experiências da Web mais rápido do que nunca.

>[!VIDEO](https://video.tv.adobe.com/v/16778?quality=12&learn=on)

O navegador de componentes agora é exibido em um tema cinza consistente, exibindo o:

* **[!UICONTROL Grupo de componentes]**
* **[!UICONTROL Título do componente]**
* **[!UICONTROL Descrição do componente]**
* **[!UICONTROL Ícone do componente]**
   * As duas primeiras letras do Título de Componente *(padrão)*
   * Imagem PNG personalizada *(configurada por um desenvolvedor)*
   * Imagem de SVG personalizada *(configurada por um desenvolvedor)*
   * Ícone CoralUI *(configurado por um desenvolvedor)*

## Opções de configuração do ícone do componente {#component-icon-configuration-options}

### Abreviações {#abbreviations}

Por padrão, os dois primeiros caracteres do título do componente (**[cq:Component]@jcr:title**) são usados como abreviação. Por exemplo, se **[cq:Component]@jcr:title=Article List**, a abreviação seria exibida como &quot;**Ar**&quot;.

A abreviação pode ser personalizada por meio da propriedade **[cq:Component]@abbreviation**. Embora esse valor possa aceitar mais de 2 caracteres, é recomendável limitar a abreviação a 2 caracteres para evitar perturbações visuais.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - abbreviation = "AL"
```

### Ícones do CoralUI {#coralui-icons}

Os ícones CoralUI, fornecidos pelo AEM, podem ser usados para ícones de componentes. Para configurar um ícone CoralUI, defina uma propriedade **[cq:Component]@cq:icon** para o valor de atributo de ícone HTML do ícone CoralUI desejado (enumerado na [documentação do CoralUI](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html).

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - cq:icon = "documentFragment"
```

### Imagens PNG {#png-images}

Imagens PNG podem ser usadas para ícones de componentes. Para configurar uma imagem PNG como um ícone de componente, adicione a imagem desejada como um **nt:file** chamado **cq:icon.png** em **[cq:Component]**.

O PNG deve ter um plano de fundo transparente ou uma cor de plano de fundo definida como **#707070**.

As imagens PNG foram dimensionadas para **20px por 20px**. No entanto, para acomodar exibições de retina **40px** por **40px** pode ser preferível.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.png
     - jcr:primaryType = "nt:file"
```

### Imagens SVG {#svg-images}

imagens SVG (baseadas em vetor) podem ser usadas para ícones de componentes. Para configurar uma imagem de SVG como um ícone de componente, adicione o SVG desejado como um **nt:file** chamado **cq:icon.svg** em **[cq:Component]**.

As imagens SVG devem ter uma cor de plano de fundo definida como **#707070** e um tamanho de **20px por 20px.**

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.svg
     - jcr:primaryType = "nt:file"
```

## Recursos adicionais {#additional-resources}

* [Ícones CoralUI disponíveis](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)
