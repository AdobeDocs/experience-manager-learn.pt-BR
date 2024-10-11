---
title: Uso do sistema de estilos no AEM Forms
description: Definir as classes CSS para as variações
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16276
source-git-commit: 86d282b426402c9ad6be84e9db92598d0dc54f85
workflow-type: tm+mt
source-wordcount: '245'
ht-degree: 1%

---

# Criar variações para o componente de botão

Depois que o tema for clonado, abra o projeto usando o código do visual studio. Você verá uma exibição semelhante
no código do visual studio
![explorador do projeto](assets/easel-theme.png)

Abra o arquivo src->components->button->_button.scss. Definiremos nossas variações personalizadas neste arquivo.

## Variação corporativa

```css
.cmp-adaptiveform-button-corporate {
  @include container;
  .cmp-adaptiveform-button {
    &__widget {
      @include primary-button;
      background: $brand-red;
      text-transform: uppercase;
      border-radius: 0px;
      color: yellow;
    }
  }
}
```

## Explicação

* **cmp-adaptiveform-button—corporate**: este é o invólucro ou classe de contêiner principal do componente &quot;cmp-adaptiveform-button—corporate&quot;.
Quaisquer estilos ou mixins dentro deste bloco serão aplicados a elementos dentro desta classe.
* **@include container**: usa um mixin chamado container, que é definido no _mixins.scss. O contêiner de mixin normalmente aplica estilos relacionados ao layout, como configuração de margens, preenchimento ou outros estilos estruturais para garantir que o contêiner se comporte de forma consistente.
* **.cmp-adaptiveform-button**: dentro do bloco corporate-style-button, você está direcionando o elemento filho com a classe .cmp-adaptiveform-button.
* **&amp;__widget**: o símbolo &amp; se refere ao seletor pai, que neste caso é .cmp-adaptiveform-button.
Isso significa que a classe final direcionada será .cmp-adaptiveform-button_widget, uma classe de estilo BEM (Modificador de elemento de bloco) que representa um subcomponente (o elemento __widget) dentro do bloco .cmp-adaptiveform-button.
* **@include primary-button**: inclui um mixin de botão primário, que é definido em _mixin.scss e adiciona estilos relacionados ao botão (como preenchimento, cores, efeitos de focalização, etc.). As propriedades background, text-transform, border-radius, color definidas no botão primário de mixin são substituídas.

O arquivo _mixins.scss é definido em src->site, conforme mostrado na captura de tela abaixo

![mixin.scss](assets/mixins.png)

## Variação de marketing

```css
.cmp-adaptiveform-button--marketing {
  
  @include container;
  .cmp-adaptiveform-button {
  &__widget {
    @include primary-button;
    background-color: #3498db;
    color: white;
    font-weight: bold;
    border: none;
    border-radius: 50px;
    box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
    cursor: pointer;
    transition: all 0.3s ease;
    outline: none;
    text-transform: uppercase;
    letter-spacing: 0.05em;
    &:hover:not([disabled]) {
      position: relative;
      scale: 102%;
      transition: box-shadow 0.1s ease-out, transform 0.1s ease-out;
      background-color: #2980b9;
      box-shadow: 0 8px 15px rgba(0, 0, 0, 0.2);
      transform: translateY(-3px);
    }
  }
}
  
}
```

## Próximas etapas

[Testar as variações](./build.md)


