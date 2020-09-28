---
title: Como codificar o sistema de estilo AEM
description: Neste vídeo, veremos a anatomia do CSS (ou LESS) e do JavaScript usado para criar o estilo do Componente de título principal do Adobe Experience Manager usando o Sistema de estilo, bem como como como esses estilos são aplicados ao HTML e DOM.
feature: style-system
topics: development, components, front-end-development
audience: developer, implementer
doc-type: technical video
activity: understand
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: 664d3964df796d508973067f8fa4fe5ef83c5fec
workflow-type: tm+mt
source-wordcount: '1145'
ht-degree: 1%

---


# Como codificar o Sistema de estilo{#understanding-how-to-code-for-the-aem-style-system}

Neste vídeo, vamos dar uma olhada na anatomia do CSS (ou [!DNL LESS]) e do JavaScript usado para criar o estilo do Componente do título principal do Experience Manager usando o Sistema de estilo, bem como em como esses estilos são aplicados ao HTML e DOM.

>[!NOTE]
>
>O Sistema de estilo AEM foi introduzido com [AEM 6.3 SP1](https://helpx.adobe.com/experience-manager/6-3/release-notes/sp1-release-notes.html) + [Feature Pack 20593](https://www.adobeaemcloud.com/content/marketplace/marketplaceProxy.html?packagePath=/content/companies/public/adobe/packages/cq630/featurepack/cq-6.3.0-featurepack-20593).
>
>O vídeo presume que o componente We.Retail Title foi atualizado para herdar dos Componentes [principais v2.0.0+](https://github.com/Adobe-Marketing-Cloud/aem-core-wcm-components/releases).

## Como codificar o Sistema de estilo {#understanding-how-to-code-for-the-style-system}

>[!VIDEO](https://video.tv.adobe.com/v/21538/?quality=9&learn=on)

O Pacote de AEM fornecido (**technical-review.sites.style-system-1.0.0.zip**) instala o estilo de título de exemplo, as políticas de amostra para os componentes de Título e Container de layout We.Retail e uma página de amostra.

[technical-review.sites.style-system-1.0.0.zip](assets/technical-review.sites.style-system-1.0.0.zip)

### O CSS {#the-css}

A seguir está a [!DNL LESS] definição do estilo de exemplo encontrado em:

* `/apps/demo/sites/style-system/clientlib-example/components/titles/styles/example.less`

Para aqueles que preferem o CSS, abaixo desse trecho de código está o CSS que esta [!DNL LESS] compilação realiza.

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

O acima [!DNL LESS] é compilado nativamente por Experience Manager para o seguinte CSS.

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

### O JavaScript {#example-javascript}

O JavaScript a seguir coleta e injeta a data e a hora da última modificação da página atual abaixo do texto do título quando o estilo Exemplo é aplicado ao componente Título.

O uso do jQuery é opcional, bem como as convenções de nomenclatura usadas.

A seguir está a [!DNL LESS] definição do estilo de exemplo encontrado em:

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

### Práticas recomendadas de HTML {#html-best-practices}

* O HTML (gerado via HTL) deve ser tão estruturalmente semântico quanto possível; evitar o agrupamento/aninhamento desnecessário de elementos.
* Os elementos HTML devem ser endereçáveis por meio de classes CSS estilo BEM.

**Bom** - Todos os elementos no componente são endereçáveis por meio da notação BEM:

```html
<!-- Good practice -->
<div class="cmp-list">
    <ul class="cmp-list__item-group">
        <li class="cmp-list__item">...</li>
    </ul>
</div>
```

**Ruim** - Os elementos de lista e lista só são endereçáveis pelo nome do elemento:

```html
<!-- Bad practice -->
<div class="cmp-list">
    <ul>
        <li>...</li>
    </ul>
</div>
```

* É melhor expor mais dados e ocultá-los do que expor muito poucos dados que exijam desenvolvimento back-end futuro para expô-los.

   * A implementação de alternâncias de conteúdo com autor pode ajudar a manter esse HTML limpo, em que os autores podem selecionar quais elementos de conteúdo são gravados no HTML. O pode ser especialmente importante ao gravar imagens no HTML que podem não ser usadas para todos os estilos.
   * A exceção a essa regra é quando recursos caros, por exemplo, imagens, são expostos por padrão, já que imagens de evento ocultas pelo CSS serão, nesse caso, buscadas desnecessariamente.

      * Os componentes de imagem modernos frequentemente usam o JavaScript para selecionar e carregar a imagem mais apropriada para o caso de uso (viewport).

### Práticas recomendadas de CSS {#css-best-practices}

>[!NOTE]
>
>O Sistema de estilo faz uma pequena divergência técnica do [BEM](https://en.bem.info/), na medida em que o `BLOCK` e não `BLOCK--MODIFIER` são aplicados ao mesmo elemento, conforme especificado pelo [BEM](https://en.bem.info/).
>
>Em vez disso, devido às restrições do produto, o `BLOCK--MODIFIER` é aplicado ao pai do `BLOCK` elemento.
>
>Todos os outros inquilinos da [BEM](https://en.bem.info/) devem ser alinhados.

* Use pré-processadores como [MENOS](https://lesscss.org/) (suportados nativamente pelo AEM) ou [SCSS](https://sass-lang.com/) (requer sistema de compilação personalizado) para permitir uma definição clara de CSS e reutilização.

* Manter o peso/especificidade do seletor uniforme; Isso ajuda a evitar e resolver conflitos em cascata de CSS difíceis de identificar.
* Organize cada estilo em um arquivo discreto.
   * Esses arquivos podem ser combinados usando LESS/SCSS `@imports` ou se for necessário CSS bruto, por meio da inclusão de arquivos da Biblioteca do cliente HTML ou sistemas de compilação de ativos front-end personalizados.
* Evite misturar muitos estilos complexos.
   * Quanto mais estilos puderem ser aplicados em uma única vez a um componente, maior será a variedade de permutações. Isso pode tornar-se difícil de manter/QA/garantir o alinhamento da marca.
* Sempre use classes CSS (após a notação BEM) para definir regras CSS.
   * Se a seleção de elementos sem classes CSS (ou seja, elementos nus) for absolutamente necessária, mova-os para cima na definição do CSS para deixar claro que eles têm uma especificidade menor do que qualquer colisão com elementos desse tipo que têm classes CSS selecionáveis.
* Evite estilizar o `BLOCK--MODIFIER` diretamente, pois ele está anexado à Grade responsiva. Alterar a exibição desse elemento pode afetar a renderização e a funcionalidade da Grade responsiva, portanto, somente o estilo neste nível quando a intenção é alterar o comportamento da Grade responsiva.
* Aplique o escopo do estilo usando `BLOCK--MODIFIER`. O `BLOCK__ELEMENT--MODIFIERS` pode ser usado no Componente, mas como o componente `BLOCK` representa o Componente, e o Componente é o estilo, o Estilo é &quot;definido&quot; e com escopo `BLOCK--MODIFIER`.

Exemplo de estrutura do seletor de CSS deve ser o seguinte:

<table> 
 <tbody> 
  <tr> 
   <td valign="bottom"><p>Seletor de primeiro nível</p> <p>BLOCO — MODIFICADOR</p> </td> 
   <td valign="bottom"><p>Seletor de segundo nível</p> <p>BLOCO</p> </td> 
   <td valign="bottom"><p>Seletor de terceiro nível</p> <p>BLOCK_ELEMENT</p> </td> 
   <td> </td> 
   <td valign="middle">Seletor CSS efetivo</td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-lista—escuro</span></td> 
   <td valign="middle"><span class="code">.cmp-lista</span></td> 
   <td valign="middle"><span class="code">.cmp-lista_item</span></td> 
   <td valign="middle">→</td> 
   <td><p><span class="code">.cmp-lista—escuro</span></p> <p><span class="code"> .cmp-lista</span></p> <p><span class="code"> </span><strong><span class="code"> .cmp-lista__item { </span></strong></p> <p><strong> cor: azul;</strong></p> <p><strong> }</strong></p> </td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-image — herói</span></td> 
   <td valign="middle"><span class="code">.cmp-image</span></td> 
   <td valign="middle"><span class="code">.cmp-image__caption</span></td> 
   <td valign="middle">→</td> 
   <td valign="middle"><p><span class="code">.cmp-image — herói</span></p> <p><span class="code"> .cmp-image</span></p> <p><span class="code"> .cmp-image__caption {</span></p> <p><span class="code"> cor: vermelho;</span></p> <p><span class="code"> }</span></p> </td> 
  </tr> 
 </tbody> 
</table>

No caso de componentes aninhados, a profundidade do seletor de CSS para esses elementos de Componente aninhados excederá o seletor de terceiro nível. Repita o mesmo padrão para o componente aninhado, mas delimitado pelo componente pai `BLOCK`. Ou em outras palavras, start os componentes aninhados `BLOCK` no terceiro nível, e os componentes aninhados `ELEMENT` estarão no quarto nível do seletor.

### Práticas recomendadas do JavaScript {#javascript-best-practices}

As práticas recomendadas definidas nesta seção pertencem ao &quot;style-JavaScript&quot; ou ao JavaScript especificamente destinado a manipular o Componente para fins estilísticos, em vez de funcionais.

* O Style-JavaScript deve ser usado de forma criteriosa e é um caso de uso minoritário.
* O Style-JavaScript deve ser usado principalmente para manipular o DOM do componente para oferecer suporte ao estilo por CSS.
* Reavalie o uso do Javascript se os componentes aparecerão muitas vezes em uma página e entenda o custo computacional/de redesenhar.
* Reavalie o uso do Javascript se ele extrair novos dados/conteúdo de forma assíncrona (via AJAX) quando o componente puder aparecer muitas vezes em uma página.
* Manipule as experiências de Publicação e Criação.
* Reutilize style-Javascript quando possível.
   * Por exemplo, se vários estilos de um Componente exigirem que sua imagem seja movida para uma imagem de plano de fundo, o style-JavaScript poderá ser implementado uma vez e anexado a vários `BLOCK--MODIFIERs`.
* Separe o estilo-JavaScript do JavaScript funcional quando possível.
* Avalie o custo do JavaScript em vez de manifestar essas alterações DOM no HTML diretamente via HTL.
   * Quando um componente que usa o estilo JavaScript exigir modificação do lado do servidor, avalie se a manipulação do JavaScript pode ser ativada no momento e quais efeitos/ramificações são para o desempenho e a capacidade de suporte do componente.

#### Considerações sobre desempenho {#performance-considerations}

* Style-JavaScript deve ser mantido leve e limpo.
* Para evitar oscilações e redesenhações desnecessárias, oculte inicialmente o componente via `BLOCK--MODIFIER BLOCK`, e mostre-o quando todas as manipulações DOM no JavaScript estiverem concluídas.
* O desempenho das manipulações style-JavaScript é semelhante aos plug-ins básicos do jQuery que se anexam e modificam elementos no DOMReady.
* Certifique-se de que as solicitações estejam com gzipado e de que CSS e JavaScript estejam minimizados.

## Recursos adicionais {#additional-resources}

* [Documentação do sistema de estilo](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/style-system.html)
* [Criar bibliotecas AEM cliente](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [Site de documentação BEM (Block Element Modifier)](https://getbem.com/)
* [Sítio Web da LESS Documentation](https://lesscss.org/)
* [Site do jQuery](https://jquery.com/)
