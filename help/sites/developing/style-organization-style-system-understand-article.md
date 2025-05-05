---
title: Noções básicas das práticas recomendadas do sistema de estilos com o AEM Sites
description: Um artigo detalhado explicando as práticas recomendadas para a implementação do Sistema de estilos com o Adobe Experience Manager Sites.
feature: Style System
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Intermediate, Experienced
doc-type: Article
exl-id: c51da742-5ce7-499a-83da-227a25fb78c9
duration: 328
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1522'
ht-degree: 0%

---

# Noções básicas sobre as práticas recomendadas do sistema de estilos{#understanding-style-organization-with-the-aem-style-system}

>[!NOTE]
>
>Revise o conteúdo em [Noções básicas sobre como codificar o Sistema de Estilos](style-system-technical-video-understand.md), para garantir uma compreensão das convenções semelhantes ao BEM usadas pelo Sistema de Estilos do AEM.

Há dois tipos ou estilos principais que são implementados para o Sistema de Estilos do AEM:

* **Estilos de layout**
* **Estilos de exibição**

Os **Estilos de layout** afetam muitos elementos de um componente para criar uma representação bem definida e identificável (design e layout) do componente, geralmente alinhada a um conceito de marca reutilizável específico. Por exemplo, um componente Teaser pode ser apresentado no layout tradicional baseado em cartão, em um estilo Promocional horizontal ou como um layout Hero sobrepondo o texto em uma imagem.

**Os estilos de exibição** são usados para afetar pequenas variações nos estilos de Layout. No entanto, eles não alteram a natureza fundamental ou a intenção do estilo de Layout. Por exemplo, um estilo de layout Herói pode ter estilos de exibição que mudam o esquema de cores do esquema de cores da marca principal para o esquema de cores da marca secundária.

## Práticas recomendadas de organização de estilo {#style-organization-best-practices}

Ao definir os nomes de estilo disponíveis para autores do AEM, é melhor:

* Nomear estilos usando um vocabulário compreendido pelos autores
* Minimizar o número de opções de estilo
* Expor somente as opções e combinações de estilo permitidas pelos padrões da marca
* Expor apenas combinações de estilo que tenham efeito
   * Se combinações ineficazes forem expostas, certifique-se de que pelo menos não tenham um efeito negativo

À medida que o número de combinações de estilos possíveis disponíveis para os autores do AEM aumenta, mais permutas existem que devem ser de controle de qualidade e validadas em relação aos padrões da marca. Muitas opções também podem confundir os autores, pois pode não ficar claro qual opção ou combinação é necessária para produzir o efeito desejado.

### Nomes de estilo vs classes CSS {#style-names-vs-css-classes}

Os nomes de estilo ou as opções apresentadas aos autores do AEM e os nomes de classe CSS de implementação são dissociados no AEM.

Isso permite que as opções de estilo sejam rotuladas em um vocabulário claro e compreendido pelos autores do AEM, mas permite que os desenvolvedores de CSS nomeiem as classes CSS de uma maneira semântica e inovadora. Por exemplo:

Um componente deve ter as opções para ser colorido com as cores **principal** e **secundária** da marca. No entanto, os autores do AEM conhecem as cores como **verde** e **amarelo**, em vez do idioma de design primário e secundário.

O Sistema de Estilos do AEM pode expor esses estilos de exibição de cores usando rótulos amigáveis para autor **Verde** e **Amarelo**, enquanto permite que os desenvolvedores de CSS usem a nomeação semântica de `.cmp-component--primary-color` e `.cmp-component--secondary-color` para definir a implementação de estilo real no CSS.

O nome do Estilo de **Verde** está mapeado para `.cmp-component--primary-color` e **Amarelo** para `.cmp-component--secondary-color`.

Se a cor da marca da empresa mudar no futuro, tudo o que precisa ser alterado são as implementações únicas de `.cmp-component--primary-color` e `.cmp-component--secondary-color`, e os nomes dos estilos.

## O componente Teaser como exemplo de caso de uso {#the-teaser-component-as-an-example-use-case}

Veja a seguir um exemplo de caso de uso de estilo de um componente de Teaser para ter vários estilos diferentes de Layout e Exibição.

Isso explorará como os nomes de estilo (expostos aos autores) e como as classes CSS de apoio são organizadas.

### Configuração de estilos de componente de teaser {#component-styles-configuration}

A imagem a seguir mostra a configuração de [!UICONTROL Estilos] do componente de Teaser para as variações discutidas no caso de uso.

Os nomes, o Layout e a Exibição do [!UICONTROL Grupo de Estilos] correspondem, por acaso, aos conceitos gerais de Estilos de exibição e Estilos de layout usados para categorizar conceitualmente os tipos de estilos neste artigo.

Os nomes do [!UICONTROL Grupo de Estilos] e o número de [!UICONTROL Grupos de Estilos] devem ser adaptados ao caso de uso do componente e às convenções de estilo do componente específico do projeto.

Por exemplo, o nome do grupo de estilos **Exibição** poderia ter sido nomeado como **Cores**.

![Grupo de Estilos de Exibição](assets/style-config.png)

### Menu de seleção de estilo {#style-selection-menu}

A imagem abaixo exibe o menu [!UICONTROL Estilo] com o qual os autores interagem para selecionar os estilos apropriados para o componente. Observe que os nomes da [!UICONTROL Grade de Estilos], bem como os nomes dos Estilos, são todos expostos ao autor.

![Menu suspenso Estilo](assets/style-menu.png)

### Estilo padrão {#default-style}

O estilo padrão é geralmente o estilo mais usado do componente e a exibição padrão sem estilo do teaser quando adicionado a uma página.

Dependendo da semelhança de estilo padrão, o CSS pode ser aplicado diretamente no `.cmp-teaser` (sem modificadores) ou em um `.cmp-teaser--default`.

Se as regras de estilo padrão se aplicam com mais frequência do que não a todas as variações, é melhor usar `.cmp-teaser` como as classes CSS do estilo padrão, já que todas as variações devem herdá-las implicitamente, assumindo que as convenções do tipo BEM são seguidas. Caso contrário, eles devem ser aplicados por meio do modificador padrão, como `.cmp-teaser--default`, que por sua vez precisa ser adicionado ao campo Classes CSS Padrão[&#128279;](#component-styles-configuration) da configuração de estilo do componente , caso contrário, essas regras de estilo terão que ser substituídas em cada variação.

É possível até mesmo atribuir um estilo &quot;nomeado&quot; como o estilo padrão, por exemplo, o estilo Hero `(.cmp-teaser--hero)` definido abaixo, no entanto, é mais claro implementar o estilo padrão contra as implementações de classe CSS `.cmp-teaser` ou `.cmp-teaser--default`.

>[!NOTE]
>
>Observe que o Estilo de layout padrão NÃO tem um nome de Estilo de exibição, no entanto, o autor pode selecionar uma opção de Exibição na ferramenta de seleção Sistema de Estilos do AEM.
>
>Isso em violação das práticas recomendadas:
>
>**Expor apenas combinações de estilo que tenham efeito**
>
>Se um autor selecionar o Estilo de exibição de **Verde**, nada acontecerá.
>
>Nesse caso de uso, admitiremos essa violação, pois todos os outros estilos de layout devem ser coloridos usando as cores da marca.
>
>Na seção **Promo (alinhado à direita)** abaixo, veremos como evitar combinações de estilos indesejadas.

![estilo padrão](assets/default.png)

* **Estilo de layout**
   * Padrão
* **Estilo de exibição**
   * Nenhum
* **Classes CSS Efetivas**: `.cmp-teaser--promo` ou `.cmp-teaser--default`

### Estilo promocional {#promo-style}

O **Estilo de layout promocional** é usado para promover conteúdo de alto valor no site e é posicionado horizontalmente para ocupar uma faixa de espaço na página da Web e deve ser estilizável por cores de marca, com o estilo de layout Promocional padrão usando texto preto.

Para isso, um **estilo de layout** de **Promo** e os **estilos de exibição** de **Verde** e **Amarelo** estão configurados no Sistema de Estilos do AEM para o componente de Teaser.

#### Padrão promocional

![padrão promocional](assets/promo-default.png)

* **Estilo de layout**
   * Nome do estilo: **Promoção**
   * Classe CSS: `cmp-teaser--promo`
* **Estilo de exibição**
   * Nenhum
* **Classes CSS Efetivas**: `.cmp-teaser--promo`

#### Primário da promoção

![principal promocional](assets/promo-primary.png)

* **Estilo de layout**
   * Nome do estilo: **Promoção**
   * Classe CSS: `cmp-teaser--promo`
* **Estilo de exibição**
   * Nome do estilo: **Verde**
   * Classe CSS: `cmp-teaser--primary-color`
* **Classes CSS Efetivas**: `cmp-teaser--promo.cmp-teaser--primary-color`

#### Secundário promocional

![Secundário promocional](assets/promo-secondary.png)

* **Estilo de layout**
   * Nome do estilo: **Promoção**
   * Classe CSS: `cmp-teaser--promo`
* **Estilo de exibição**
   * Nome do estilo: **Amarelo**
   * Classe CSS: `cmp-teaser--secondary-color`
* **Classes CSS Efetivas**: `cmp-teaser--promo.cmp-teaser--secondary-color`

### Estilo promocional alinhado à direita {#promo-r-align}

O estilo de layout **Promo alinhado à direita** é uma variação do estilo Promo que inverte o local da imagem e do texto (imagem à direita, texto à esquerda).

O alinhamento direito, em sua essência, é um estilo de exibição. Ele pode ser inserido no Sistema de estilos do AEM como um Estilo de exibição selecionado em conjunto com o Estilo de layout Promo. Isso viola as práticas recomendadas do:

**Expor apenas combinações de estilo que tenham efeito**

..que já foi violado no [Estilo padrão](#default-style).

Como o alinhamento à direita afeta apenas o estilo de layout Promo, e não os outros 2 estilos de layout: padrão e herói, podemos criar um novo estilo de layout Promo (alinhado à direita) que inclui a classe CSS que alinha à direita o conteúdo dos estilos de layout Promo: `cmp -teaser--alternate`.

Essa combinação de vários estilos em uma única entrada de estilo também pode ajudar a reduzir o número de estilos e permutas de estilo disponíveis, o que é melhor minimizar.

Observe que o nome da classe CSS, `cmp-teaser--alternate`, não precisa corresponder à nomenclatura compatível com o autor de &quot;alinhado à direita&quot;.

#### Padrão alinhado à direita da promoção

![promoção alinhada à direita](assets/promo-alternate-default.png)

* **Estilo de layout**
   * Nome do estilo: **Promoção (alinhado à direita)**
   * Classes CSS: `cmp-teaser--promo cmp-teaser--alternate`
* **Estilo de exibição**
   * Nenhum
* **Classes CSS Efetivas**: `.cmp-teaser--promo.cmp-teaser--alternate`

#### Principal Promocional Alinhado à Direita

![Promo Primário alinhado à direita](assets/promo-alternate-primary.png)

* **Estilo de layout**
   * Nome do estilo: **Promoção (alinhado à direita)**
   * Classes CSS: `cmp-teaser--promo cmp-teaser--alternate`
* **Estilo de exibição**
   * Nome do estilo: **Verde**
   * Classe CSS: `cmp-teaser--primary-color`
* **Classes CSS Efetivas**: `.cmp-teaser--promo.cmp-teaser--alternate.cmp-teaser--primary-color`

#### Secundário Promo Alinhado à Direita

![Secundário alinhado à direita da promoção](assets/promo-alternate-secondary.png)

* **Estilo de layout**
   * Nome do estilo: **Promoção (alinhado à direita)**
   * Classes CSS: `cmp-teaser--promo cmp-teaser--alternate`
* **Estilo de exibição**
   * Nome do estilo: **Amarelo**
   * Classe CSS: `cmp-teaser--secondary-color`
* **Classes CSS Efetivas**: `.cmp-teaser--promo.cmp-teaser--alternate.cmp-teaser--secondary-color`

### Estilo herói {#hero-style}

O estilo de layout Herói exibe a imagem dos componentes como um plano de fundo com o título e o link sobrepostos. O estilo de layout Hero, como o estilo de layout Promo, deve ser colorido com as cores da marca.

Para colorir o estilo de layout Hero com as cores da marca, os mesmos estilos de exibição usados para o estilo de layout Promo podem ser aproveitados.

Por componente, o nome do estilo é mapeado para o único conjunto de classes CSS, o que significa que os nomes de Classe CSS que colorem o plano de fundo do estilo de layout Promo devem colorir o texto e o link do estilo de layout Hero.

Isso pode ser feito trivialmente definindo o escopo das regras de CSS, no entanto, isso requer que os desenvolvedores de CSS entendam como essas permutações são aplicadas no AEM.

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

#### Herói padrão

![Estilo Herói](assets/hero.png)

* **Estilo de layout**
   * Nome do estilo: **Herói**
   * Classe CSS: `cmp-teaser--hero`
* **Estilo de exibição**
   * Nenhum
* **Classes CSS Efetivas**: `.cmp-teaser--hero`

#### Principal herói

![Principal Herói](assets/hero-primary.png)

* **Estilo de layout**
   * Nome do estilo: **Promoção**
   * Classe CSS: `cmp-teaser--hero`
* **Estilo de exibição**
   * Nome do estilo: **Verde**
   * Classe CSS: `cmp-teaser--primary-color`
* **Classes CSS Efetivas**: `cmp-teaser--hero.cmp-teaser--primary-color`

#### Herói Secundário

![Herói Secundário](assets/hero-secondary.png)

* **Estilo de layout**
   * Nome do estilo: **Promoção**
   * Classe CSS: `cmp-teaser--hero`
* **Estilo de exibição**
   * Nome do estilo: **Amarelo**
   * Classe CSS: `cmp-teaser--secondary-color`
* **Classes CSS Efetivas**: `cmp-teaser--hero.cmp-teaser--secondary-color`

## Recursos adicionais {#additional-resources}

* [Documentação do Sistema de Estilos](https://helpx.adobe.com/br/experience-manager/6-5/sites/authoring/using/style-system.html)
* [Criando bibliotecas de Clientes AEM](https://helpx.adobe.com/br/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [Site de documentação do BEM (Block Element Modifier)](https://getbem.com/)
* [MENOS Site de documentação](https://lesscss.org/)
* [jQuery](https://jquery.com/)
