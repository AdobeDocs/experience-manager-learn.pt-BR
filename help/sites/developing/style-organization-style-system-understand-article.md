---
title: Noções básicas das práticas recomendadas do sistema de estilos com o AEM Sites
description: Um artigo detalhado explicando as práticas recomendadas para a implementação do Sistema de estilos com o Adobe Experience Manager Sites.
feature: Style System
version: 6.4, 6.5
topic: Development
role: Developer
level: Intermediate, Experienced
doc-type: Article
exl-id: c51da742-5ce7-499a-83da-227a25fb78c9
duration: 447
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '1522'
ht-degree: 0%

---

# Noções básicas sobre as práticas recomendadas do sistema de estilos{#understanding-style-organization-with-the-aem-style-system}

>[!NOTE]
>
>Revise o conteúdo em [Noções básicas sobre como codificar para o Sistema de estilos](style-system-technical-video-understand.md), para garantir a compreensão das convenções do tipo BEM utilizadas pelo Sistema de Estilos AEM.

Há dois tipos ou estilos principais que são implementados para o Sistema de Estilos do AEM:

* **Estilos de layout**
* **Estilos de exibição**

**Estilos de layout** afetar muitos elementos de um Componente para criar uma representação bem definida e identificável (design e layout) do componente, alinhando-se frequentemente a um conceito específico de Marca reutilizável. Por exemplo, um componente Teaser pode ser apresentado no layout tradicional baseado em cartão, em um estilo Promocional horizontal ou como um layout Hero sobrepondo o texto em uma imagem.

**Estilos de exibição** são usados para afetar pequenas variações nos estilos de Layout. No entanto, eles não alteram a natureza ou a intenção fundamental do estilo de Layout. Por exemplo, um estilo de layout Herói pode ter estilos de exibição que mudam o esquema de cores do esquema de cores da marca principal para o esquema de cores da marca secundária.

## Práticas recomendadas de organização de estilo {#style-organization-best-practices}

Ao definir os nomes de estilo disponíveis para autores de AEM, é melhor:

* Nomear estilos usando um vocabulário compreendido pelos autores
* Minimizar o número de opções de estilo
* Expor somente as opções e combinações de estilo permitidas pelos padrões da marca
* Expor apenas combinações de estilo que tenham efeito
   * Se combinações ineficazes forem expostas, certifique-se de que pelo menos não tenham um efeito negativo

À medida que o número de combinações de estilo possíveis disponíveis para autores de AEM aumenta, mais permutas existem que devem ser de controle de qualidade e validadas em relação aos padrões da marca. Muitas opções também podem confundir os autores, pois pode não ficar claro qual opção ou combinação é necessária para produzir o efeito desejado.

### Nomes de estilo vs classes CSS {#style-names-vs-css-classes}

Nomes de estilo, ou as opções apresentadas a autores de AEM, e os nomes de classe CSS de implementação são dissociados no AEM.

Isso permite que as opções de estilo sejam rotuladas em um vocabulário claro e compreendido pelos autores do AEM, mas permite que os desenvolvedores de CSS nomeiem as classes CSS de uma maneira semântica e inovadora. Por exemplo:

Um componente deve ter as opções para ser colorido com o da marca **principal** e **secundário** cores, no entanto, os autores AEM conhecem as cores como **verde** e **amarelo**, em vez do idioma de design primário e secundário.

O Sistema de Estilos AEM pode expor esses estilos de exibição de cores usando rótulos amigáveis para o autor **Verde** e **Amarelo**, permitindo que os desenvolvedores de CSS usem a nomeação semântica de `.cmp-component--primary-color` e `.cmp-component--secondary-color` para definir a implementação do estilo real em CSS.

O nome do estilo de **Verde** está mapeado para `.cmp-component--primary-color`, e **Amarelo** para `.cmp-component--secondary-color`.

Se a cor da marca da empresa mudar no futuro, basta mudar as implementações únicas de `.cmp-component--primary-color` e `.cmp-component--secondary-color`e os nomes do estilo.

## O componente Teaser como exemplo de caso de uso {#the-teaser-component-as-an-example-use-case}

Veja a seguir um exemplo de caso de uso de estilo de um componente de Teaser para ter vários estilos diferentes de Layout e Exibição.

Isso explorará como os nomes de estilo (expostos aos autores) e como as classes CSS de apoio são organizadas.

### Configuração de estilos de componente de teaser {#component-styles-configuration}

A imagem a seguir mostra o [!UICONTROL Estilos] Configuração do componente de Teaser para as variações discutidas no caso de uso.

A variável [!UICONTROL Grupo de estilos] Os nomes, o Layout e a Exibição correspondem, por acaso, aos conceitos gerais de Estilos de exibição e Estilos de layout usados para categorizar conceitualmente os tipos de estilos neste artigo.

A variável [!UICONTROL Grupo de estilos] nomes e o número de [!UICONTROL Grupos de Estilos] deve ser personalizado para o caso de uso do componente e para as convenções de estilo do componente específico do projeto.

Por exemplo, a variável **Exibir** o nome do grupo de estilos poderia ter sido nomeado **Cores**.

![Grupo de Estilos de Exibição](assets/style-config.png)

### Menu de seleção de estilo {#style-selection-menu}

A imagem abaixo mostra o [!UICONTROL Estilo] os autores de menu interagem com o para selecionar os estilos apropriados para o componente. Observe que [!UICONTROL Gráfico de Estilo] Os nomes do, bem como os nomes dos Estilos, são expostos ao autor.

![Menu suspenso Estilo](assets/style-menu.png)

### Estilo padrão {#default-style}

O estilo padrão é geralmente o estilo mais usado do componente e a exibição padrão sem estilo do teaser quando adicionado a uma página.

Dependendo da semelhança do estilo padrão, o CSS pode ser aplicado diretamente no `.cmp-teaser` (sem modificadores) ou em um `.cmp-teaser--default`.

Se as regras de estilo padrão se aplicarem com mais frequência do que não a todas as variações, é melhor usar `.cmp-teaser` como classes CSS do estilo padrão, já que todas as variações devem herdá-las implicitamente, supondo que as convenções semelhantes ao BEM sejam seguidas. Caso contrário, eles devem ser aplicados por meio do modificador padrão, como `.cmp-teaser--default`, que por sua vez precisa ser adicionado à [Classes CSS padrão da configuração de estilo do componente](#component-styles-configuration) , caso contrário, essas regras de estilo terão que ser substituídas em cada variação.

É possível até mesmo atribuir um estilo &quot;nomeado&quot; como o estilo padrão, por exemplo, o estilo Hero `(.cmp-teaser--hero)` definido abaixo, no entanto, é mais claro implementar o estilo padrão em relação ao `.cmp-teaser` ou `.cmp-teaser--default` Implementações da classe CSS.

>[!NOTE]
>
>Observe que o estilo de layout Padrão NÃO tem um nome de estilo de Exibição. No entanto, o autor pode selecionar uma opção de Exibição na ferramenta de seleção Sistema de Estilos AEM.
>
>Isso em violação das práticas recomendadas:
>
>**Expor apenas combinações de estilo que tenham efeito**
>
>Se um autor selecionar o Estilo de exibição de **Verde** nada acontecerá.
>
>Nesse caso de uso, admitiremos essa violação, pois todos os outros estilos de layout devem ser coloridos usando as cores da marca.
>
>No **Promoção (alinhado à direita)** abaixo, veremos como evitar combinações de estilos indesejadas.

![estilo padrão](assets/default.png)

* **Estilo de layout**
   * Padrão
* **Estilo de exibição**
   * Nenhum
* **Classes CSS efetivas**: `.cmp-teaser--promo` ou `.cmp-teaser--default`

### Estilo promocional {#promo-style}

A variável **Estilo de layout promocional** O é usado para promover conteúdo de alto valor no site e é apresentado horizontalmente para ocupar uma faixa de espaço na página da Web e deve poder ser estilizado por cores da marca, com o estilo de layout Promo padrão usando texto preto.

Para isso, uma **estilo de layout** de **Promoção** e a variável **estilos de exibição** de **Verde** e **Amarelo** são configurados no Sistema de Estilos AEM para o componente de Teaser.

#### Padrão promocional

![padrão da promoção](assets/promo-default.png)

* **Estilo de layout**
   * Nome do estilo: **Promoção**
   * Classe CSS: `cmp-teaser--promo`
* **Estilo de exibição**
   * Nenhum
* **Classes CSS efetivas**: `.cmp-teaser--promo`

#### Primário da promoção

![principal da promoção](assets/promo-primary.png)

* **Estilo de layout**
   * Nome do estilo: **Promoção**
   * Classe CSS: `cmp-teaser--promo`
* **Estilo de exibição**
   * Nome do estilo: **Verde**
   * Classe CSS: `cmp-teaser--primary-color`
* **Classes CSS efetivas**: `cmp-teaser--promo.cmp-teaser--primary-color`

#### Secundário promocional

![Secundário promocional](assets/promo-secondary.png)

* **Estilo de layout**
   * Nome do estilo: **Promoção**
   * Classe CSS: `cmp-teaser--promo`
* **Estilo de exibição**
   * Nome do estilo: **Amarelo**
   * Classe CSS: `cmp-teaser--secondary-color`
* **Classes CSS efetivas**: `cmp-teaser--promo.cmp-teaser--secondary-color`

### Estilo promocional alinhado à direita {#promo-r-align}

A variável **Promoção alinhada à direita** O estilo de layout é uma variação do estilo Promocional, que inverte a localização da imagem e do texto (imagem à direita, texto à esquerda).

O alinhamento direito, em seu núcleo, é um estilo de exibição, ele pode ser inserido no Sistema de estilo AEM como um Estilo de exibição selecionado em conjunto com o estilo de layout Promo. Isso viola as práticas recomendadas do:

**Expor apenas combinações de estilo que tenham efeito**

..que já foi violada no [Estilo padrão](#default-style).

Como o alinhamento direito afeta apenas o estilo de layout Promo, e não os outros 2 estilos de layout: padrão e herói, podemos criar um novo estilo de layout Promo (alinhado à direita) que inclui a classe CSS que alinha à direita o conteúdo dos estilos de layout Promo: `cmp -teaser--alternate`.

Essa combinação de vários estilos em uma única entrada de estilo também pode ajudar a reduzir o número de estilos e permutas de estilo disponíveis, o que é melhor minimizar.

Observe o nome da classe CSS, `cmp-teaser--alternate`, não precisa corresponder à nomenclatura compatível com o autor de &quot;alinhado à direita&quot;.

#### Padrão alinhado à direita da promoção

![promoção alinhada à direita](assets/promo-alternate-default.png)

* **Estilo de layout**
   * Nome do estilo: **Promoção (alinhado à direita)**
   * Classes CSS: `cmp-teaser--promo cmp-teaser--alternate`
* **Estilo de exibição**
   * Nenhum
* **Classes CSS efetivas**: `.cmp-teaser--promo.cmp-teaser--alternate`

#### Principal Promocional Alinhado à Direita

![Principal Promocional Alinhado à Direita](assets/promo-alternate-primary.png)

* **Estilo de layout**
   * Nome do estilo: **Promoção (alinhado à direita)**
   * Classes CSS: `cmp-teaser--promo cmp-teaser--alternate`
* **Estilo de exibição**
   * Nome do estilo: **Verde**
   * Classe CSS: `cmp-teaser--primary-color`
* **Classes CSS efetivas**: `.cmp-teaser--promo.cmp-teaser--alternate.cmp-teaser--primary-color`

#### Secundário Promo Alinhado à Direita

![Secundário Promo Alinhado à Direita](assets/promo-alternate-secondary.png)

* **Estilo de layout**
   * Nome do estilo: **Promoção (alinhado à direita)**
   * Classes CSS: `cmp-teaser--promo cmp-teaser--alternate`
* **Estilo de exibição**
   * Nome do estilo: **Amarelo**
   * Classe CSS: `cmp-teaser--secondary-color`
* **Classes CSS efetivas**: `.cmp-teaser--promo.cmp-teaser--alternate.cmp-teaser--secondary-color`

### Estilo herói {#hero-style}

O estilo de layout Herói exibe a imagem dos componentes como um plano de fundo com o título e o link sobrepostos. O estilo de layout Hero, como o estilo de layout Promo, deve ser colorido com as cores da marca.

Para colorir o estilo de layout Hero com as cores da marca, os mesmos estilos de exibição usados para o estilo de layout Promo podem ser aproveitados.

Por componente, o nome do estilo é mapeado para o único conjunto de classes CSS, o que significa que os nomes de Classe CSS que colorem o plano de fundo do estilo de layout Promo devem colorir o texto e o link do estilo de layout Hero.

Isso pode ser feito trivialmente definindo o escopo das regras de CSS, no entanto, isso requer que os desenvolvedores de CSS entendam como essas permutações são aplicadas no AEM.

CSS para colorir o plano de fundo do **Promover** estilo de layout com a cor primária (verde):

```css
.cmp-teaser--promo.cmp-teaser--primary--color {
   ...
   background-color: green;
   ...
}
```

CSS para colorir o texto do **Herói** estilo de layout com a cor primária (verde):

```css
.cmp-teaser--hero.cmp-teaser--primary--color {
   ...
   color: green;
   ...
}
```

#### Herói padrão

![Estilo herói](assets/hero.png)

* **Estilo de layout**
   * Nome do estilo: **Herói**
   * Classe CSS: `cmp-teaser--hero`
* **Estilo de exibição**
   * Nenhum
* **Classes CSS efetivas**: `.cmp-teaser--hero`

#### Principal herói

![Principal herói](assets/hero-primary.png)

* **Estilo de layout**
   * Nome do estilo: **Promoção**
   * Classe CSS: `cmp-teaser--hero`
* **Estilo de exibição**
   * Nome do estilo: **Verde**
   * Classe CSS: `cmp-teaser--primary-color`
* **Classes CSS efetivas**: `cmp-teaser--hero.cmp-teaser--primary-color`

#### Herói Secundário

![Herói Secundário](assets/hero-secondary.png)

* **Estilo de layout**
   * Nome do estilo: **Promoção**
   * Classe CSS: `cmp-teaser--hero`
* **Estilo de exibição**
   * Nome do estilo: **Amarelo**
   * Classe CSS: `cmp-teaser--secondary-color`
* **Classes CSS efetivas**: `cmp-teaser--hero.cmp-teaser--secondary-color`

## Recursos adicionais {#additional-resources}

* [Documentação do sistema de estilos](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/style-system.html)
* [Criação de bibliotecas de clientes AEM](https://helpx.adobe.com/br/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [Site da documentação do BEM (Block Element Modifier)](https://getbem.com/)
* [LESS Site de documentação](https://lesscss.org/)
* [Site do jQuery](https://jquery.com/)
