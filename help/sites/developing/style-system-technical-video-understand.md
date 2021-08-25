---
title: Como entender o código do Sistema de estilos de AEM
description: Neste vídeo, vamos dar uma olhada na anatomia do CSS (ou LESS) e do JavaScript usados para criar um estilo no Componente de título principal do Adobe Experience Manager usando o Sistema de estilos, bem como em como esses estilos são aplicados ao HTML e DOM.
feature: Style System
version: 6.4, 6.5
topic: Development
role: Developer
level: Intermediate, Experienced
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '1092'
ht-degree: 0%

---


# Como entender o código do sistema de estilos{#understanding-how-to-code-for-the-aem-style-system}

Neste vídeo, vamos dar uma olhada na anatomia do CSS (ou [!DNL LESS]) e do JavaScript usados para criar um estilo no Componente de título principal do Experience Manager usando o Sistema de estilos, bem como em como esses estilos são aplicados ao HTML e DOM.


## Como entender o código do sistema de estilos {#understanding-how-to-code-for-the-style-system}

>[!VIDEO](https://video.tv.adobe.com/v/21538/?quality=12&learn=on)

O Pacote de AEM fornecido (**technical-review.sites.style-system-1.0.0.zip**) instala o estilo de título de exemplo, as políticas de amostra para os componentes Contêiner de layout e Título do We.Retail e uma página de exemplo.

[technical-review.sites.style-system-1.0.0.zip](assets/technical-review.sites.style-system-1.0.0.zip)

### O CSS {#the-css}

Esta é a definição [!DNL LESS] para o estilo de exemplo encontrado em:

* `/apps/demo/sites/style-system/clientlib-example/components/titles/styles/example.less`

Para aqueles que preferem CSS, abaixo desse trecho de código está o CSS no qual esse [!DNL LESS] é compilado.

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

O [!DNL LESS] acima é compilado nativamente por Experience Manager para o seguinte CSS.

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

O JavaScript a seguir coleta e injeta a data e a hora da última modificação da página atual, abaixo do texto do título, quando o estilo Exemplo é aplicado ao componente Título .

O uso do jQuery é opcional, assim como as convenções de nomenclatura usadas.

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

* O HTML (gerado via HTL) deve ser tão semântico estruturalmente quanto possível; evitar o agrupamento/aninhamento desnecessário de elementos.
* Os elementos HTML devem ser endereçáveis por meio de classes CSS de estilo BEM.

**Bom**  - Todos os elementos no componente são endereçáveis por meio da notação BEM:

```html
<!-- Good practice -->
<div class="cmp-list">
    <ul class="cmp-list__item-group">
        <li class="cmp-list__item">...</li>
    </ul>
</div>
```

**Ruim**  - Os elementos de lista e lista são endereçáveis somente pelo nome do elemento:

```html
<!-- Bad practice -->
<div class="cmp-list">
    <ul>
        <li>...</li>
    </ul>
</div>
```

* É melhor expor mais dados e ocultá-los do que expor muito poucos dados que exijam o desenvolvimento de back-end futuro para expô-los.

   * A implementação de alternâncias de conteúdo com autor pode ajudar a manter esse HTML limpo, em que os autores podem selecionar quais elementos de conteúdo são gravados no HTML. A pode ser especialmente importante ao gravar imagens no HTML que podem não ser usadas para todos os estilos.
   * A exceção a essa regra é quando recursos caros, por exemplo, imagens, são expostos por padrão, pois imagens de evento ocultas por CSS serão, nesse caso, buscadas desnecessariamente.

      * Os componentes de imagem modernos geralmente usam o JavaScript para selecionar e carregar a imagem mais apropriada para o caso de uso (visor).

### Práticas recomendadas de CSS {#css-best-practices}

>[!NOTE]
>
>O Sistema de estilos faz uma pequena divergência técnica de [BEM](https://en.bem.info/), na medida em que `BLOCK` e `BLOCK--MODIFIER` não são aplicadas ao mesmo elemento, conforme especificado por [BEM](https://en.bem.info/).
>
>Em vez disso, devido a restrições de produto, `BLOCK--MODIFIER` é aplicado ao pai do elemento `BLOCK`.
>
>Todos os outros locatários de [BEM](https://en.bem.info/) devem ser alinhados com.

* Use pré-processadores como [LESS](https://lesscss.org/) (suportado por AEM nativamente) ou [SCSS](https://sass-lang.com/) (requer sistema de compilação personalizado) para permitir uma definição clara de CSS e reutilizabilidade.

* Manter uniforme o peso/a especificidade do seletor; Isso ajuda a evitar e resolver conflitos em cascata de CSS difíceis de identificar.
* Organize cada estilo em um arquivo discreto.
   * Esses arquivos podem ser combinados usando LESS/SCSS `@imports` ou se CSS bruto for necessário, por meio da inclusão de arquivo da Biblioteca de clientes HTML ou sistemas personalizados de criação de ativos front-end.
* Evite misturar vários estilos complexos.
   * Quanto mais estilos puderem ser aplicados em um único momento a um componente, maior será a variedade de permutas. Isso pode se tornar difícil de manter/QA/garantir o alinhamento da marca.
* Sempre use classes CSS (após a notação BEM) para definir regras CSS.
   * Se a seleção de elementos sem classes CSS (ou seja, elementos nus) for absolutamente necessária, mova-os para cima na definição de CSS para deixar claro que eles têm uma especificidade menor do que qualquer colisão com elementos desse tipo que tenham classes CSS selecionáveis.
* Evite estilizar o `BLOCK--MODIFIER` diretamente, pois ele está anexado à Grade Responsiva. Alterar a exibição desse elemento pode afetar a renderização e a funcionalidade da Grade Responsiva, portanto, estime apenas nesse nível quando a intenção for alterar o comportamento da Grade Responsiva.
* Aplique o escopo do estilo usando `BLOCK--MODIFIER`. O `BLOCK__ELEMENT--MODIFIERS` pode ser usado no Componente, mas como o `BLOCK` representa o Componente, e o Componente é o que tem o estilo, o Estilo é &quot;definido&quot; e com escopo via `BLOCK--MODIFIER`.

Exemplo de estrutura do seletor de CSS deve ser o seguinte:

<table> 
 <tbody> 
  <tr> 
   <td valign="bottom"><p>Seletor de primeiro nível</p> <p>BLOCO—MODIFICADOR</p> </td> 
   <td valign="bottom"><p>Seletor de segundo nível</p> <p>BLOCO</p> </td> 
   <td valign="bottom"><p>Seletor de terceiro nível</p> <p>BLOCK_ELEMENT</p> </td> 
   <td> </td> 
   <td valign="middle">Seletor de CSS efetivo</td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-list — escuro</span></td> 
   <td valign="middle"><span class="code">.cmp-list</span></td> 
   <td valign="middle"><span class="code">.cmp-list_item</span></td> 
   <td valign="middle">→</td> 
   <td><p><span class="code">.cmp-list — escuro</span></p> <p><span class="code"> .cmp-list</span></p> <p><span class="code"> </span><strong><span class="code"> .cmp-list__item {  </span></strong></p> <p><strong> cor: Azul;</strong></p> <p><strong> }</strong></p> </td> 
  </tr> 
  <tr> 
   <td valign="middle"><span class="code">.cmp-image—hero</span></td> 
   <td valign="middle"><span class="code">.cmp-image</span></td> 
   <td valign="middle"><span class="code">.cmp-image__caption</span></td> 
   <td valign="middle">→</td> 
   <td valign="middle"><p><span class="code">.cmp-image—hero</span></p> <p><span class="code"> .cmp-image</span></p> <p><span class="code"> .cmp-image__caption {</span></p> <p><span class="code"> cor: vermelho;</span></p> <p><span class="code"> }</span></p> </td> 
  </tr> 
 </tbody> 
</table>

No caso de componentes aninhados, a profundidade do seletor de CSS para esses elementos de Componentes aninhados excederá o seletor de terceiro nível. Repita o mesmo padrão para o componente aninhado, mas com escopo pelo `BLOCK` do Componente pai. Ou, em outras palavras, inicie o `BLOCK` do componente aninhado no terceiro nível e o `ELEMENT` do Componente aninhado estará no quarto nível do seletor.

### Práticas recomendadas do JavaScript {#javascript-best-practices}

As práticas recomendadas definidas nesta seção pertencem ao &quot;style-JavaScript&quot; ou ao JavaScript especificamente destinado a manipular o componente para fins estilísticos, em vez de funcionais.

* O Style-JavaScript deve ser usado criteriosamente e é um caso de uso minoritário.
* O Style-JavaScript deve ser usado principalmente para manipular o DOM do componente para oferecer suporte a estilo por CSS.
* Reavalie o uso do Javascript se os componentes forem exibidos várias vezes em uma página e entenda o custo computacional/de redesenho.
* Reavalie o uso do Javascript se ele receber novos dados/conteúdo de forma assíncrona (via AJAX) quando o componente puder aparecer várias vezes em uma página.
* Lida com as experiências de publicação e criação.
* Reutilize style-Javascript quando possível.
   * Por exemplo, se vários estilos de um Componente exigirem que sua imagem seja movida para uma imagem de plano de fundo, o JavaScript de estilo poderá ser implementado uma vez e anexado a vários `BLOCK--MODIFIERs`.
* Separe o JavaScript de estilo do JavaScript funcional quando possível.
* Avalie o custo do JavaScript em vez de manifestar essas alterações DOM no HTML diretamente via HTL.
   * Quando um componente que usa o estilo-JavaScript requer modificação do lado do servidor, avalie se a manipulação do JavaScript pode ser trazida para o momento e quais efeitos/ramificações são para o desempenho e a capacidade de suporte do componente.

#### Considerações de desempenho {#performance-considerations}

* O Style-JavaScript deve ser mantido leve e limpo.
* Para evitar cintilação e redesenhações desnecessárias, oculte inicialmente o componente por `BLOCK--MODIFIER BLOCK` e mostre-o quando todas as manipulações DOM no JavaScript estiverem concluídas.
* O desempenho das manipulações style-JavaScript é semelhante aos plug-ins básicos do jQuery que são anexados e modificados em DOMReady.
* Verifique se as solicitações estão gzipadas e se o CSS e o JavaScript estão minificados.

## Recursos adicionais {#additional-resources}

* [Documentação do sistema de estilos](https://helpx.adobe.com/experience-manager/6-5/sites/authoring/using/style-system.html)
* [Criação AEM bibliotecas de clientes](https://helpx.adobe.com/experience-manager/6-5/sites/developing/using/clientlibs.html)
* [Site da documentação BEM (Block Element Modifier)](https://getbem.com/)
* [MENOS site de documentação](https://lesscss.org/)
* [Site do jQuery](https://jquery.com/)
