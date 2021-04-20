---
title: Noções básicas sobre as práticas recomendadas do sistema de estilos com o AEM Sites
description: Um artigo detalhado explicando as práticas recomendadas para a implementação do Sistema de estilos com o Adobe Experience Manager Sites.
feature: Style System
topics: development, components, front-end-development
audience: developer
doc-type: article
activity: understand
version: 6.4, 6.5
topic: Development
role: Developer
level: Intermediate, Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1544'
ht-degree: 2%

---


# Noções básicas sobre as práticas recomendadas do sistema de estilos{#understanding-style-organization-with-the-aem-style-system}

>[!NOTE]
>
>Revise o conteúdo em [Entendendo como codificar para o Sistema de estilos](style-system-technical-video-understand.md), para garantir uma compreensão das convenções semelhantes ao BEM usadas pelo Sistema de estilos AEM.

Há dois sabores ou estilos principais que são implementados para o Sistema de estilos do AEM:

* **Estilos de layout**
* **Estilos de exibição**

**Os** estilos de layout afetam muitos elementos de um componente para criar uma representação bem definida e identificável (design e layout) do componente, geralmente alinhando com um conceito específico de Marca reutilizável. Por exemplo, um componente Teaser pode ser apresentado no layout tradicional baseado em cartão, um estilo promocional horizontal ou como um layout Herói sobrepondo o texto em uma imagem.

**Os** estilos de exibição são usados para afetar pequenas variações nos estilos de Layout. No entanto, eles não alteram a natureza ou a intenção fundamental do estilo de Layout. Por exemplo, um estilo de layout Herói pode ter estilos de Exibição que alteram o esquema de cores do esquema de cores da marca principal para o esquema de cores da marca secundária.

## Práticas recomendadas da organização de estilos {#style-organization-best-practices}

Ao definir os nomes de estilo disponíveis para autores do AEM, é melhor:

* Nomeie estilos usando um vocabulário compreendido pelos autores
* Minimizar o número de opções de estilo
* Exponha apenas as opções e combinações de estilo permitidas pelos padrões da marca
* Expor somente as combinações de estilo que têm um efeito
   * Caso sejam expostas combinações ineficazes, deve assegurar-se que pelo menos não produzem efeitos negativos

À medida que o número de combinações de estilo possíveis disponíveis para autores do AEM aumenta, mais permutas existem que devem ser QA e validadas em relação aos padrões da marca. Muitas opções também podem confundir os autores, pois pode tornar-se pouco claro qual opção ou combinação é necessária para produzir o efeito desejado.

### Nomes de estilo vs. classes CSS {#style-names-vs-css-classes}

Nomes de estilo ou as opções apresentadas para autores do AEM e os nomes de classe CSS implementadores são dissociados no AEM.

Isso permite que as opções de Estilo sejam rotuladas em um vocabulário claro e compreendido pelos autores do AEM, mas permite que os desenvolvedores de CSS nomeiem as classes de CSS de maneira semântica e à prova de futuro. Por exemplo:

Um componente deve ter as opções para ser colorido com as cores **primário** e **secundário** da marca. No entanto, os autores do AEM conhecem as cores como **verde** e **amarelo**, em vez da linguagem de design do primário e secundário.

O Sistema de estilos do AEM pode expor esses estilos de exibição de cores usando rótulos amigáveis ao autor **Verde** e **Amarelo**, enquanto permite que os desenvolvedores de CSS usem a nomenclatura semântica de `.cmp-component--primary-color` e `.cmp-component--secondary-color` para definir a implementação de estilo real no CSS.

O nome do Estilo de **Verde** é mapeado para `.cmp-component--primary-color` e **Amarelo** para `.cmp-component--secondary-color`.

Se a cor da marca da empresa mudar no futuro, tudo o que precisa ser alterado é as implementações únicas de `.cmp-component--primary-color` e `.cmp-component--secondary-color`, e os nomes de estilo.

## O componente Teaser como exemplo do caso de uso {#the-teaser-component-as-an-example-use-case}

Veja a seguir um exemplo de uso do estilo de um componente Teaser para ter vários estilos diferentes de Layout e Exibição.

Isso explorará como os nomes de estilo (expostos aos autores) e como as classes CSS de backup são organizadas.

### Configuração de estilos do componente Teaser {#component-styles-configuration}

A imagem a seguir mostra a configuração [!UICONTROL Estilos] do componente Teaser para as variações discutidas no caso de uso.

Os nomes [!UICONTROL Grupo de estilos], Layout e Exibir, por acaso, correspondem aos conceitos gerais de Estilos de exibição e Estilos de layout usados para categorizar conceitualmente os tipos de estilos neste artigo.

Os nomes [!UICONTROL Grupo de estilos] e o número de [!UICONTROL Grupos de estilos] devem ser adaptados ao caso de uso de componentes e às convenções de estilo de componentes específicas do projeto.

Por exemplo, o nome do grupo de estilos **Display** poderia ter sido chamado de **Colors**.

![Exibir grupo de estilos](assets/style-config.png)

### Menu de seleção de estilo {#style-selection-menu}

A imagem abaixo exibe o menu [!UICONTROL Estilo] com o qual os autores interagem para selecionar os estilos apropriados para o componente. Observe que os nomes [!UICONTROL Gráfico de estilo], bem como os nomes de Estilo, são expostos ao autor.

![Menu suspenso Estilo](assets/style-menu.png)

### Estilo padrão {#default-style}

O estilo padrão é frequentemente o estilo mais usado do componente, e a exibição padrão e sem estilo do teaser quando adicionado a uma página.

Dependendo da compatibilidade do estilo padrão, o CSS pode ser aplicado diretamente no `.cmp-teaser` (sem modificadores) ou em um `.cmp-teaser--default`.

Se as regras de estilo padrão se aplicarem com mais frequência do que não a todas as variações, é melhor usar `.cmp-teaser` como as classes CSS do estilo padrão, já que todas as variações devem herdá-las implicitamente, supondo que as convenções semelhantes a BEM sejam seguidas. Caso contrário, elas deverão ser aplicadas por meio do modificador padrão, como `.cmp-teaser--default`, que por sua vez precisa ser adicionado ao campo [Classes CSS Padrão](#component-styles-configuration) da configuração de estilo do componente, caso contrário essas regras de estilo terão de ser substituídas em cada variação.

É possível atribuir um estilo &quot;nomeado&quot; como o estilo padrão, por exemplo, o Hero style `(.cmp-teaser--hero)` definido abaixo, no entanto, é mais claro implementar o estilo padrão em relação às implementações de classe CSS `.cmp-teaser` ou `.cmp-teaser--default`.

>[!NOTE]
>
>Observe que o estilo de layout padrão NÃO tem um nome de estilo de exibição; no entanto, o autor poderá selecionar uma opção de Exibição na ferramenta de seleção do Sistema de estilos do AEM.
>
>Isso em violação da prática recomendada:
>
>**Expor somente as combinações de estilo que têm um efeito**
>
>Se um autor selecionar o Estilo de exibição de **Verde** nada acontecerá.
>
>Nesse caso de uso, concederemos essa violação, pois todos os outros estilos de Layout devem ser coloridos usando as cores da marca.
>
>Na seção **Promo (alinhado à direita)** abaixo, veremos como evitar combinações de estilos indesejadas.

![estilo padrão](assets/default.png)

* **Estilo do layout**
   * Padrão
* **Estilo de exibição**
   * Nenhum
* **Classes** de CSS Efetivas:  `.cmp-teaser--promo` ou  `.cmp-teaser--default`

### Estilo promocional {#promo-style}

O **Estilo de layout promocional** é usado para promover conteúdo de alto valor no site e é apresentado horizontalmente para ocupar uma faixa de espaço na página da Web e deve ser passível de estilo por cores da marca, com o estilo de layout promocional padrão usando texto preto.

Para isso, um **estilo de layout** de **Promo** e **estilos de exibição** de **Verde** e **Amarelo** são configurados no Sistema de estilos do AEM para o componente Teaser.

#### Padrão de promoção

![padrão de promoção](assets/promo-default.png)

* **Estilo do layout**
   * Nome do estilo: **Promo**
   * Classe CSS: `cmp-teaser--promo`
* **Estilo de exibição**
   * Nenhum
* **Classes** de CSS Efetivas:  `.cmp-teaser--promo`

#### Principal de promoção

![principal promocional](assets/promo-primary.png)

* **Estilo do layout**
   * Nome do estilo: **Promo**
   * Classe CSS: `cmp-teaser--promo`
* **Estilo de exibição**
   * Nome do estilo: **Verde**
   * Classe CSS: `cmp-teaser--primary-color`
* **Classes** de CSS Efetivas:  `cmp-teaser--promo.cmp-teaser--primary-color`

#### Secundário de promoção

![Secundário de promoção](assets/promo-secondary.png)

* **Estilo do layout**
   * Nome do estilo: **Promo**
   * Classe CSS: `cmp-teaser--promo`
* **Estilo de exibição**
   * Nome do estilo: **Amarelo**
   * Classe CSS: `cmp-teaser--secondary-color`
* **Classes** de CSS Efetivas:  `cmp-teaser--promo.cmp-teaser--secondary-color`

### Estilo alinhado à direita promocional {#promo-r-align}

O estilo de layout **Promo alinhado à direita** é uma variação do estilo Promo que viaja o local da imagem e do texto (imagem à direita, texto à esquerda).

O alinhamento direito, em seu núcleo, é um estilo de exibição, pode ser inserido no Sistema de estilos do AEM como um estilo de exibição selecionado junto com o estilo de layout Promoção. Isso viola a prática recomendada de:

**Expor somente as combinações de estilo que têm um efeito**

...que já foi violado no [Estilo padrão](#default-style).

Como o alinhamento correto afeta apenas o estilo de layout Promo e não os outros 2 estilos de layout: padrão e herói, podemos criar um novo estilo de layout Promo (alinhado à direita) que inclui a classe CSS que alinha à direita o conteúdo dos estilos de layout Promo : `cmp -teaser--alternate`.

Essa combinação de vários estilos em uma única entrada de Estilo também pode ajudar a reduzir o número de estilos e permutas de estilo disponíveis, o que é melhor para minimizar.

Observe que o nome da classe CSS, `cmp-teaser--alternate`, não precisa corresponder à nomenclatura amigável para autor de &quot;alinhado à direita&quot;.

#### Padrão de alinhamento à direita promocional

![alinhamento à direita da promoção](assets/promo-alternate-default.png)

* **Estilo do layout**
   * Nome do estilo: **Promo (alinhada à direita)**
   * Classes em CSS: `cmp-teaser--promo cmp-teaser--alternate`
* **Estilo de exibição**
   * Nenhum
* **Classes** de CSS Efetivas:  `.cmp-teaser--promo.cmp-teaser--alternate`

#### Principal Alinhado à Direita da Promoção

![Principal Alinhado à Direita da Promoção](assets/promo-alternate-primary.png)

* **Estilo do layout**
   * Nome do estilo: **Promo (alinhada à direita)**
   * Classes em CSS: `cmp-teaser--promo cmp-teaser--alternate`
* **Estilo de exibição**
   * Nome do estilo: **Verde**
   * Classe CSS: `cmp-teaser--primary-color`
* **Classes** de CSS Efetivas:  `.cmp-teaser--promo.cmp-teaser--alternate.cmp-teaser--primary-color`

#### Promo secundário alinhado à direita

![Promo secundário alinhado à direita](assets/promo-alternate-secondary.png)

* **Estilo do layout**
   * Nome do estilo: **Promo (alinhada à direita)**
   * Classes em CSS: `cmp-teaser--promo cmp-teaser--alternate`
* **Estilo de exibição**
   * Nome do estilo: **Amarelo**
   * Classe CSS: `cmp-teaser--secondary-color`
* **Classes** de CSS Efetivas:  `.cmp-teaser--promo.cmp-teaser--alternate.cmp-teaser--secondary-color`

### Estilo de exemplo {#hero-style}

O estilo de layout Herdado exibe a imagem dos componentes como um plano de fundo com o título e o link sobreposto. O estilo de layout Herói, como o estilo de layout Promoção , deve ser colorido com as cores da marca.

Para colorir o estilo de layout Herdado com as cores da marca, os mesmos estilos de exibição usados para o estilo de layout Promoção podem ser aproveitados.

Por componente, o nome do estilo é mapeado para o conjunto único de classes CSS, o que significa que os nomes de Classe CSS que colorem o plano de fundo do estilo de layout Promoção devem colorir o texto e o link do estilo de layout Herói.

Isso pode ser feito trivialmente pelo escopo das regras de CSS. No entanto, isso requer que os desenvolvedores de CSS entendam como essas permutas serão implementadas no AEM.

CSS para colorir o plano de fundo do estilo de layout **Promover** com a cor primária (verde):

```css
.cmp-teaser--promo.cmp-teaser--primary--color {
   ...
   background-color: green;
   ...
}
```

CSS para colorir o texto do estilo de layout **Herói** com a cor primária (verde):

```css
.cmp-teaser--hero.cmp-teaser--primary--color {
   ...
   color: green;
   ...
}
```

#### Padrão de Hero

![Estilo da Herança](assets/hero.png)

* **Estilo do layout**
   * Nome do estilo: **Hero**
   * Classe CSS: `cmp-teaser--hero`
* **Estilo de exibição**
   * Nenhum
* **Classes** de CSS Efetivas:  `.cmp-teaser--hero`

#### Principal Herdado

![Principal Herdado](assets/hero-primary.png)

* **Estilo do layout**
   * Nome do estilo: **Promo**
   * Classe CSS: `cmp-teaser--hero`
* **Estilo de exibição**
   * Nome do estilo: **Verde**
   * Classe CSS: `cmp-teaser--primary-color`
* **Classes** de CSS Efetivas:  `cmp-teaser--hero.cmp-teaser--primary-color`

#### Hero Secundary

![Hero Secundary](assets/hero-secondary.png)

* **Estilo do layout**
   * Nome do estilo: **Promo**
   * Classe CSS: `cmp-teaser--hero`
* **Estilo de exibição**
   * Nome do estilo: **Amarelo**
   * Classe CSS: `cmp-teaser--secondary-color`
* **Classes** de CSS Efetivas:  `cmp-teaser--hero.cmp-teaser--secondary-color`

## Recursos adicionais {#additional-resources}

* [Documentação do sistema de estilos](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/style-system.html)
* [Criação de bibliotecas de clientes do AEM](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [Site da documentação BEM (Block Element Modifier)](https://getbem.com/)
* [MENOS site de documentação](https://lesscss.org/)
* [Site do jQuery](https://jquery.com/)
