---
title: Personalização de ícones de componentes no Adobe Experience Manager Sites
description: Os ícones de componentes permitem que os autores identifiquem rapidamente um componente com ícones ou abreviações significativas. Agora os autores podem encontrar os Componentes necessários para criar suas experiências da Web mais rápido do que nunca.
topics: components
audience: administrator, developer
doc-type: technical video
activity: develop
version: 6.4, 6.5
feature: Core Components
topic: Development
role: User
level: Intermediate
exl-id: 37dc26aa-0773-4749-8c8b-4544bd4d5e5f
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '374'
ht-degree: 1%

---

# Personalização de ícones de componentes {#developing-component-icons-in-aem-sites}

Os ícones de componentes permitem que os autores identifiquem rapidamente um componente com ícones ou abreviações significativas. Agora os autores podem encontrar os Componentes necessários para criar suas experiências da Web mais rápido do que nunca.

>[!VIDEO](https://video.tv.adobe.com/v/16778?quality=12&learn=on)

O navegador de componentes agora é exibido em um tema cinza consistente, exibindo:

* **[!UICONTROL Grupo de componente]**
* **[!UICONTROL Título do componente]**
* **[!UICONTROL Descrição do componente]**
* **[!UICONTROL Ícone do componente]**
   * As duas primeiras letras do Título do Componente *(padrão)*
   * Imagem PNG personalizada *(configurado por um desenvolvedor)*
   * Imagem SVG personalizada *(configurado por um desenvolvedor)*
   * Ícone CoralUI *(configurado por um desenvolvedor)*

## Opções de configuração do ícone de componente {#component-icon-configuration-options}

### Abreviações {#abbreviations}

Por padrão, os 2 primeiros caracteres do título do componente (**[cq:Component]@jcr:title**) são usadas como a abreviação . Por exemplo, se **[cq:Component]@jcr:title=Lista de artigos** a abreviação seria exibida como &quot;**Ar**&quot;.

A abreviação pode ser personalizada por meio do **[cq:Component]@abbreviation** propriedade. Embora esse valor possa aceitar mais de 2 caracteres, é recomendável limitar a abreviação a 2 caracteres para evitar qualquer perturbação visual.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - abbreviation = "AL"
```

### Ícones do CoralUI {#coralui-icons}

Os ícones CoralUI, fornecidos pelo AEM, podem ser usados para ícones de componentes. Para configurar um ícone CoralUI, defina um **[cq:Component]@cq:icon** propriedade para o valor do atributo de ícone HTML do ícone CoralUI desejado (enumerado no [Documentação do CoralUI](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html).

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  - cq:icon = "documentFragment"
```

### Imagens PNG {#png-images}

Imagens PNG podem ser usadas para ícones de componentes. Para configurar uma imagem PNG como um ícone de componente, adicione a imagem desejada como uma **nt:file** nomeado **cq:icon.png** nos termos do **[cq:Component]**.

O PNG deve ter um plano de fundo transparente ou uma cor de plano de fundo definida como **# 707070**.

As imagens PNG são dimensionadas para **20px por 20px**. No entanto, para acomodar monitores de retina **40px** por **40px** pode ser preferível.

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.png
     - jcr:primaryType = "nt:file"
```

### Imagens SVG {#svg-images}

Imagens SVG (baseadas em vetor) podem ser usadas para ícones de componentes. Para configurar uma SVG image como um ícone de componente, adicione o SVG desejado como um **nt:file** nomeado **cq:icon.svg** nos termos do **[cq:Component]**.

As imagens SVG devem ter uma cor de fundo definida como **# 707070** e um tamanho de **20px por 20px.**

```plain
/apps/.../components/content/my-component
  - jcr:primaryType = "cq:Component"
  + cq:icon.svg
     - jcr:primaryType = "nt:file"
```

## Recursos adicionais {#additional-resources}

* [Ícones CoralUI disponíveis](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/reference-materials/coral-ui/coralui3/Coral.Icon.html)
