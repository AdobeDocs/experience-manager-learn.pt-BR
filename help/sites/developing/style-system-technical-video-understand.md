---
title: Noções básicas sobre como codificar para o Sistema de estilos do AEM
description: Neste vídeo, observaremos a anatomia do CSS (ou MENOS) e do JavaScript usados para estilizar o Componente principal de título do Adobe Experience Manager usando o Sistema de estilos, bem como a forma como esses estilos são aplicados ao HTML e ao DOM.
feature: Style System
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Intermediate, Experienced
doc-type: Technical Video
exl-id: 8fbc3819-3214-4c58-8629-a27eb6f0c545
duration: 1005
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1065'
ht-degree: 0%

---

# Noções básicas sobre como codificar para o Sistema de estilos{#understanding-how-to-code-for-the-aem-style-system}

Neste vídeo, observaremos a anatomia do CSS (ou [!DNL LESS]) e do JavaScript usados para estilizar o Componente Principal de Título do Experience Manager usando o Sistema de Estilos, bem como a forma como esses estilos são aplicados ao HTML e ao DOM.


## Noções básicas sobre como codificar para o Sistema de estilos {#understanding-how-to-code-for-the-style-system}

>[!VIDEO](https://video.tv.adobe.com/v/35621?quality=12&learn=on&captions=por_br)

O Pacote do AEM fornecido (**technical-review.sites.style-system-1.0.0.zip**) instala o estilo de título de exemplo, políticas de exemplo para os componentes de Contêiner de layout e Título do We.Retail e uma página de exemplo.

[technical-review.sites.style-system-1.0.0.zip](assets/technical-review.sites.style-system-1.0.0.zip)

### O CSS {#the-css}

Esta é a definição [!DNL LESS] para o estilo de exemplo encontrado em:

* `/apps/demo/sites/style-system/clientlib-example/components/titles/styles/example.less`

Para aqueles que preferem CSS, abaixo deste trecho de código está o CSS no qual este [!DNL LESS] é compilado.

```css
/* LESS */
.cmp-title--example {
 .cmp-title {
  text-align: center;

  .cmp-title__text {
   color: #EB212E;
   font-weight: 600;
   font-size: 5rem;
   border-bottom: solid 1px #ddd;
   padding-bottom: 0;
   margin-bottom: .25rem
  }

  // Last Modified At element injected via JS
  .cmp-title__last-modified-at {
   color: #999;
   font-size: 1.5rem;
   font-style: italic;
   font-weight: 200;
  }
 }
}
```

O [!DNL LESS] acima é compilado nativamente pelo Experience Manager para o seguinte CSS.

```css
/* CSS */
.cmp-title--example .cmp-title {
 text-align: center;
}

.cmp-title--example .cmp-title .cmp-title__text {
 color: #EB212E;
 font-weight: 600;
 font-size: 5rem;
 border-bottom: solid 1px #ddd;
 padding-bottom: 0;
 margin-bottom: 0.25rem;
}

.cmp-title--example .cmp-title .cmp-title__last-modified-at {
 color: #999;
 font-size: 1.5rem;
 font-style: italic;
 font-weight: 200;
}
```

### O JAVASCRIPT {#example-javascript}

O JavaScript a seguir coleta e insere a data e hora da última modificação da página atual abaixo do texto do título quando o estilo Exemplo é aplicado ao componente Título.

O uso de jQuery é opcional, assim como as convenções de nomenclatura usadas.

Esta é a definição [!DNL LESS] para o estilo de exemplo encontrado em:

* `/apps/demo/sites/style-system/clientlib-example/components/titles/styles/js/title.js`

```js
/**
 * JavaScript supporting Styles may be re-used across multi Component Styles.
 *
 * For example:
 * Many styles may require the Components Image (provided via an <img> element) to be set as the background-image.
 * A single JavaScript function may be used to adjust the DOM for all styles that required this effect.
 *
 * JavaScript must react to the DOMNodeInserted event to handle style-switching in the Page Editor Authoring experience.
 * JavaScript must also run on DOM ready to handle the initial page load rendering (AEM Publish).
 * JavaScript must mark and check for elements as processed to avoid cyclic processing (ie. if the JavaScript inserts a DOM node of its own).
 */
jQuery(function ($) {
    "use strict;"

    moment.locale("en");

    /**
     * Method that injects p element, containing the current pages last modified date/time, under the title text.
     *
     * Similar to the CSS Style application, component HTML elements should be targeted via the BEM class names (as they define the stable API)
     * and targeting "raw" elements (ex. "li", "a") should be avoided.
     */
    function applyComponentStyles() {

        $(".cmp-title--example").not("[data-styles-title-last-modified-processed]").each(function () {
            var title = $(this).attr("data-styles-title-last-modified-processed", true),
                url = Granite.HTTP.getPath() + ".model.json";

            $.getJSON(url, function(data) {
                var dateObject = moment(data['lastModifiedDate']),
                    titleText = title.find('.cmp-title__text');

                titleText.after($("<p>").addClass("cmp-title__last-modified-at").text("Last modified " + dateObject.fromNow()));
            });
        });
    }

    // Handle DOM Ready event
    applyComponentStyles();

    // Apply Styles when a component is inserted into the DOM (ie. during Authoring)
    $(".responsivegrid").bind("DOMNodeInserted", applyComponentStyles);
});
```

## Práticas recomendadas de desenvolvimento {#development-best-practices}

### Práticas recomendadas do HTML {#html-best-practices}

* O HTML (gerado por HTL) deve ser estruturalmente o mais semântico possível; evitando agrupamento/aninhamento desnecessário de elementos.
* Os elementos HTML devem ser endereçáveis por meio de classes CSS de estilo BEM.

**Bom** - Todos os elementos no componente são endereçáveis via notação BEM:

```html
<!-- Good practice -->
<div class="cmp-list">
    <ul class="cmp-list__item-group">
        <li class="cmp-list__item">...</li>
    </ul>
</div>
```

**Incorreto** - A lista e os elementos da lista são endereçáveis somente pelo nome do elemento:

```html
<!-- Bad practice -->
<div class="cmp-list">
    <ul>
        <li>...</li>
    </ul>
</div>
```

* É melhor expor mais dados e ocultá-los do que expor poucos dados que precisem de desenvolvimento de back-end futuro para expô-los.

   * A implementação de alternadores de conteúdo que podem ser criados pode ajudar a manter esse HTML enxuto, permitindo que os autores selecionem quais elementos de conteúdo são gravados na HTML. As podem ser especialmente importantes ao gravar imagens na HTML que podem não ser usadas para todos os estilos.
   * A exceção a essa regra é quando recursos caros, por exemplo, imagens, são expostos por padrão, já que as imagens de eventos ocultas pelo CSS são, nesse caso, buscadas desnecessariamente.

      * Os componentes de imagem modernos geralmente usam o JavaScript para selecionar e carregar a imagem mais apropriada para o caso de uso (visor).

### Práticas recomendadas de CSS {#css-best-practices}

>[!NOTE]
>
>O Sistema de Estilos faz uma pequena divergência técnica de [BEM](https://en.bem.info/), em que `BLOCK` e `BLOCK--MODIFIER` não são aplicados ao mesmo elemento, conforme especificado por [BEM](https://en.bem.info/).
>
>Em vez disso, devido a restrições de produto, o `BLOCK--MODIFIER` é aplicado ao pai do elemento `BLOCK`.
>
>Todos os outros locatários de [BEM](https://en.bem.info/) devem estar alinhados com.

* Use pré-processadores como [LESS](https://lesscss.org/) (com suporte nativo do AEM) ou [SCSS](https://sass-lang.com/) (requer um sistema de compilação personalizado) para permitir uma definição de CSS clara e a reutilização.

* Mantenha o peso/especificidade do seletor uniforme; isso ajuda a evitar e resolver conflitos em cascata de CSS difíceis de identificar.
* Organize cada estilo em um arquivo distinto.
   * Esses arquivos podem ser combinados usando LESS/SCSS `@imports` ou, se CSS bruto for necessário, por meio da inclusão de arquivos da Biblioteca de clientes da HTML ou de sistemas de build de ativos front-end personalizados.
* Evite misturar muitos estilos complexos.
   * Quanto mais estilos puderem ser aplicados em uma única vez a um componente, maior será a variedade de permutas. Isso pode se tornar difícil de manter/garantir o alinhamento da marca.
* Sempre use classes CSS (seguindo a notação BEM) para definir regras CSS.
   * Se a seleção de elementos sem classes CSS (ou seja, elementos simples) for absolutamente necessária, mova-os para cima na definição de CSS para deixar claro que eles têm uma especificidade menor do que qualquer colisão com elementos desse tipo que tenham classes CSS selecionáveis.
* Evite estilizar o `BLOCK--MODIFIER` diretamente, pois ele está anexado à Grade Responsiva. Alterar a exibição desse elemento pode afetar a renderização e a funcionalidade da Grade Responsiva. Portanto, somente o estilo nesse nível quando a intenção for alterar o comportamento da Grade Responsiva.
* Aplicar escopo de estilo usando `BLOCK--MODIFIER`. O `BLOCK__ELEMENT--MODIFIERS` pode ser usado no Componente, mas como `BLOCK` representa o Componente, e o Componente é o que é estilizado, o Estilo é &quot;definido&quot; e tem escopo definido via `BLOCK--MODIFIER`.

A estrutura do seletor de CSS de exemplo deve ser a seguinte:

<table> 
 <tbody> 
  <tr> 
   <td valign="bottom"><p>seletor de 1º nível</p> <p>BLOCO—MODIFICADOR</p> </td> 
   <td valign="bottom"><p>Seletor de segundo nível</p> <p>BLOQUEAR</p> </td> 
   <td valign="bottom"><p>seletor de 3º nível</p> <p>BLOCK_ELEMENT</p> </td> 
   <td> </td> 
   <td valign="middle">Seletor de CSS efetivo</td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-list — escuro</span></td> 
   <td valign="middle"><span class="code">.cmp-list</span></td> 
   <td valign="middle"><span class="code">.cmp-list_item</span></td> 
   <td valign="middle">→</td> 
   <td><p><span class="code">.cmp-list — escuro</span></p> <p><span class="code"> .cmp-list</span></p> <p><span class="code"> </span><strong><span class="code"> .cmp-list__item { </span></strong></p> <p><strong> cor: azul;</strong></p> <p><strong> }</strong></p> </td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-image—hero</span></td> 
   <td valign="middle"><span class="code">imagem .cmp</span></td> 
   <td valign="middle"><span class="code">.cmp-image_caption</span></td> 
   <td valign="middle">→</td> 
   <td valign="middle"><p><span class="code">.cmp-image—hero</span></p> <p><span class="code"> imagem .cmp</span></p> <p><span class="code"> .cmp-image__caption {</span></p> <p><span class="code"> cor: vermelho;</span></p> <p><span class="code"> }</span></p> </td> 
  </tr> 
 </tbody> 
</table>

No caso de componentes aninhados, a profundidade do seletor de CSS para esses elementos de componentes aninhados excederá o seletor de terceiro nível. Repita o mesmo padrão para o componente aninhado, mas com escopo definido pelo `BLOCK` do componente principal. Ou seja, inicie o `BLOCK` do componente aninhado no terceiro nível e o `ELEMENT` do componente aninhado está no quarto nível de seletor.

### Práticas recomendadas do JavaScript {#javascript-best-practices}

As práticas recomendadas definidas nesta seção referem-se ao &quot;style-JavaScript&quot;, ou JavaScript, especificamente destinado a manipular o componente para fins estilísticos, em vez de funcionais.

* O Style-JavaScript deve ser usado com critério e é um caso de uso minoritário.
* Style-JavaScript deve ser usado principalmente para manipular o DOM do componente para suportar estilos por CSS.
* Reavalie o uso do Javascript se os componentes aparecerem muitas vezes em uma página e entenda o custo computacional e/ou de redesenho.
* Reavalie o uso do Javascript se ele extrair novos dados/conteúdo de forma assíncrona (via AJAX) quando o componente puder aparecer muitas vezes em uma página.
* Lide com as experiências de publicação e criação.
* Quando possível, reutilize o style-Javascript.
   * Por exemplo, se vários estilos de um componente exigirem que sua imagem seja movida para uma imagem de plano de fundo, o estilo JavaScript poderá ser implementado uma vez e anexado a vários `BLOCK--MODIFIERs`.
* Separe o style-JavaScript do JavaScript funcional quando possível.
* Avalie o custo do JavaScript versus a manifestação dessas alterações de DOM no HTML diretamente por HTL.
   * Quando um componente que usa JavaScript de estilo exigir modificação no lado do servidor, avalie se a manipulação do JavaScript pode ser trazida no momento e quais são os efeitos/ramificações para o desempenho e a capacidade de suporte do componente.

#### Considerações sobre desempenho {#performance-considerations}

* O Style-JavaScript deve ser mantido leve e enxuto.
* Para evitar oscilações e redesenhos desnecessários, oculte inicialmente o componente por meio de `BLOCK--MODIFIER BLOCK` e exiba-o quando todas as manipulações de DOM no JavaScript estiverem concluídas.
* O desempenho das manipulações de JavaScript de estilo é semelhante aos plug-ins básicos de jQuery que anexam e modificam elementos no DOMReady.
* Verifique se as solicitações estão compactadas e se o CSS e o JavaScript estão minificados.

## Recursos adicionais {#additional-resources}

* [Documentação do Sistema de Estilos](https://helpx.adobe.com/br/experience-manager/6-5/sites/authoring/using/style-system.html)
* [Criando bibliotecas de Clientes AEM](https://helpx.adobe.com/br/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [Site de documentação do BEM (Block Element Modifier)](https://getbem.com/)
* [MENOS Site de documentação](https://lesscss.org/)
* [jQuery](https://jquery.com/)
