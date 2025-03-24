---
title: Adicionar identidade visual do site
description: Defina CSS global, variáveis CSS e fontes da Web para um site do Edge Delivery Services.
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 900
exl-id: a5cd9906-7e7a-43dd-a6b2-e80f67d37992
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1315'
ht-degree: 0%

---

# Adicionar identidade visual do site

Comece configurando a identidade visual geral, atualizando os estilos globais, definindo variáveis CSS e adicionando fontes da Web. Esses elementos fundamentais garantem que o site permaneça consistente e passível de manutenção, e devem ser aplicados de forma consistente em todo o site.

## Criar um problema do GitHub

Para manter tudo organizado, use o GitHub para rastrear o trabalho. Primeiro, crie um problema do GitHub para este corpo de trabalho:

1. Vá para o repositório do GitHub (consulte o capítulo [Criar um projeto de código](./1-new-code-project.md) para obter detalhes).
2. Clique na guia **Problemas** e depois em **Novo problema**.
3. Escreva um **título** e **descrição** para o trabalho a ser concluído.
4. Clique em **Enviar novo problema**.

O problema do GitHub é usado posteriormente ao [criar uma solicitação de pull](#merge-code-changes).

![Criar novo problema do GitHub](./assets/4-website-branding/github-issues.png)

## Criar uma ramificação de trabalho

Para manter a organização e garantir a qualidade do código, crie uma nova ramificação para cada corpo de trabalho. Essa prática impede que o novo código afete negativamente o desempenho e garante que as alterações não estejam ativas antes de serem concluídas.

Para este capítulo, que se concentra nos estilos básicos do site, crie uma ramificação chamada `wknd-styles`.

```bash
# ~/Code/aem-wknd-eds-ue

$ git checkout -b wknd-styles
```

## CSS global

O Edge Delivery Services usa um arquivo CSS global, localizado em `styles/styles.css`, para configurar os estilos comuns para todo o site. O `styles.css` controla aspectos como cores, fontes e espaçamento, certificando-se de que tudo esteja consistente em todo o site.

O CSS global deve ser agnóstico em relação a construções de nível inferior, como blocos, com foco na aparência geral do site e tratamentos visuais compartilhados.

Lembre-se de que os estilos CSS globais podem ser substituídos quando necessário.

### Variáveis CSS

[As variáveis CSS](https://developer.mozilla.org/en-US/docs/Web/CSS/Using_CSS_custom_properties) são uma ótima maneira de armazenar configurações de design, como cores, fontes e tamanhos. Usando variáveis, é possível alterar esses elementos em um local e atualizá-los em todo o site.

Para começar a personalizar as variáveis CSS, siga estas etapas:

1. Abra o arquivo `styles/styles.css` no editor de código.
2. Localize a declaração `:root`, onde as variáveis CSS globais são armazenadas.
3. Modifique as variáveis de cor e fonte para corresponderem à marca WKND.

Veja um exemplo:


```css
/* styles/styles.css */

:root {
  /* colors */
  --primary-color: rgb(255, 234, 3); /* WKND primary color */
  --secondary-color: rgb(32, 32, 32); /* Secondary brand color */
  --background-color: white; /* Background color */
  --light-color: rgb(235, 235, 235); /* Light background color */
  --dark-color: var(--secondary-color); /* Dark text color */
  --text-color: var(--secondary-color); /* Default text color */
  --link-color: var(--text-color); /* Link color */
  --link-hover-color: black; /* Link hover color */

  /* fonts */
  --heading-font: 'Roboto', sans-serif; /* Heading font */
  --body-font: 'Open Sans', sans-serif; /* Body font */
  --base-font-size: 16px; /* Base font size */
}
```

Explore as outras variáveis na seção `:root` e revise as configurações padrão.

Conforme você desenvolve um site e se encontra repetindo os mesmos valores de CSS, considere criar novas variáveis para facilitar o gerenciamento dos estilos. Exemplos de outras propriedades CSS que podem se beneficiar das variáveis CSS incluem: `border-radius`, `padding`, `margin` e `box-shadow`.

### Elementos simples

Os elementos simples são estilizados diretamente pelo nome do elemento, em vez de usar uma classe CSS. Por exemplo, em vez de estilizar uma classe CSS `.page-heading`, os estilos são aplicados ao elemento `h1` usando `h1 { ... }`.

No arquivo `styles/styles.css`, um conjunto de estilos base é aplicado a elementos HTML simples. Os sites da Edge Delivery Services priorizam o uso de elementos simples, pois estão alinhados com a HTML semântica nativa do Edge Delivery Service.

Para alinhar com a marca WKND, vamos estilizar alguns elementos simples em `styles.css`:

```css
/* styles/styles.css */

...
h2 {
  font-size: var(--heading-font-size-xl); /* Set font size for h2 */
}

/* Add a partial yellow underline under H2 */
h2::after {
  border-bottom: 2px solid var(--primary-color); /* Yellow underline */
  content: "";
  display: block;
  padding-top: 8px;
  width: 84px;
}
...
```

Esses estilos garantem que os elementos `h2`, a menos que sejam substituídos, sejam estilizados de forma consistente com a marca WKND, ajudando a criar uma hierarquia visual clara. O sublinhado amarelo parcial abaixo de cada `h2` adiciona um toque distinto aos cabeçalhos.

### Elementos inferidos

No Edge Delivery Services, o código `scripts.js` e `aem.js` do projeto aprimoram automaticamente elementos HTML básicos específicos com base em seu contexto no HTML.

Por exemplo, elementos de âncora (`<a>`) criados em sua própria linha - em vez de embutidos com o texto ao redor - são inferidos como botões com base nesse contexto. Essas âncoras são automaticamente encapsuladas com um contêiner `div` com a classe CSS `button-container` e o elemento âncora tem uma classe CSS `button` adicionada.

Por exemplo, quando um link é criado em sua própria linha, o Edge Delivery Services JavaScript atualiza o DOM para o seguinte:

```html
<p class="button-container">
  <a href="/authored/link" title="Click me" class="button">Click me</a>
</p>
```

Esses botões podem ser personalizados para corresponder à marca WKND - que dita que os botões aparecem como retângulos amarelos com texto preto.

Este é um exemplo de como estilizar os &quot;botões inferidos&quot; em `styles.css`:

```css
/* styles/styles.css */

/* Buttons */
a.button:any-link,
button {
  box-sizing: border-box;
  display: inline-block;
  max-width: 100%;
  margin: 12px 0;
  border: 2px solid transparent;
  padding: 0.5em 1.2em;
  font-family: var(--body-font-family);
  font-style: normal;
  font-weight: 500;
  line-height: 1.25;
  text-align: center;
  text-decoration: none;
  cursor: pointer;
  overflow: hidden;
  text-overflow: ellipsis;
  white-space: nowrap;

  /* WKND specific treatments */
  text-transform: uppercase;
  background-color: var(--primary-color);
  color: var(--dark-color);
  border-radius: 0;
}
```

Esse CSS define os estilos de botão de base e inclui tratamentos específicos de WKND, como texto em maiúsculas, plano de fundo amarelo e texto preto. As propriedades `background-color` e `color` usam variáveis CSS, permitindo que o estilo do botão permaneça alinhado com as cores da marca. Essa abordagem garante que os botões sejam estilizados de forma consistente em todo o site, permanecendo flexíveis.

## Fontes da Web

Os projetos do Edge Delivery Services otimizam o uso de fontes da Web para manter o alto desempenho e minimizar o impacto nas pontuações do Lighthouse. Esse método garante a renderização rápida sem comprometer a identidade visual do site. Veja como implementar fontes da Web com eficiência para obter um desempenho ideal.

### Faces de fontes

Adicione fontes da Web personalizadas usando declarações CSS `@font-face` no arquivo `styles/fonts.css`. Adicionar o `@font-faces` ao `fonts.css` garante que as fontes da Web sejam carregadas no momento ideal, ajudando a manter as pontuações do Lighthouse.

1. Abrir `styles/fonts.css`.
2. Adicione as seguintes declarações `@font-face` para incluir as fontes da marca WKND: `Asar` e `Source Sans Pro`.

```css
/* styles/fonts.css */

@font-face {
  font-family: Asar;
  font-style: normal;
  font-weight: 400;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/asar/v22/sZlLdRyI6TBIbkEaDZtQS6A.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: italic;
  font-weight: 300;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xKwdSBYKcSV-LCoeQqfX1RYOo3qPZZMkids18S0xR41.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: italic;
  font-weight: 400;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xK1dSBYKcSV-LCoeQqfX1RYOo3qPZ7nsDJB9cme.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: italic;
  font-weight: 600;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xKwdSBYKcSV-LCoeQqfX1RYOo3qPZY4lCds18S0xR41.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: normal;
  font-weight: 300;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xKydSBYKcSV-LCoeQqfX1RYOo3ik4zwlxdu3cOWxw.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: normal;
  font-weight: 400;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xK3dSBYKcSV-LCoeQqfX1RYOo3qOK7lujVj9w.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}

@font-face {
  font-family: 'Source Sans Pro';
  font-style: normal;
  font-weight: 600;
  font-display: swap;
  src: url("https://fonts.gstatic.com/s/sourcesanspro/v22/6xKydSBYKcSV-LCoeQqfX1RYOo3i54rwlxdu3cOWxw.woff2") format('woff2');
  unicode-range: U+0000-00FF, U+0131, U+0152-0153, U+02BB-02BC, U+02C6, U+02DA, U+02DC, U+0304, U+0308, U+0329, U+2000-206F, U+20AC, U+2122, U+2191, U+2193, U+2212, U+2215, U+FEFF, U+FFFD;
}
```

As fontes usadas neste tutorial são provenientes do Google Fonts, mas as fontes da Web podem ser obtidas de qualquer provedor de fontes, incluindo o [Adobe Fonts](https://fonts.adobe.com/).

+++Uso de arquivos de fontes da Web locais

Como alternativa, os arquivos de fontes da Web podem ser copiados para o projeto na pasta `/fonts` e referenciados nas declarações `@font-face`.

Este tutorial usa as fontes da Web remotas hospedadas para facilitar o acompanhamento.

```css
/* styles/fonts.css */

@font-face { 
    font-family: Asar;
    ...
    src: url("/fonts/asar.woff2") format('woff2'),
    ...
}
```

+++

Por fim, atualize as variáveis CSS `styles/styles.css` para usar as novas fontes:

```css
/* styles/styles.css */

:root {
    ...
    /* fonts */
    --body-font-family: 'Source Sans Pro', roboto-fallback, sans-serif;
    --heading-font-family: 'Asar', roboto-condensed-fallback, sans-serif;
    ...
}
```

As `roboto-fallback` e `roboto-condensed-fallback` são fontes substitutas atualizadas na seção [Fontes substitutas](#fallback-fonts) para se alinharem para suportar as fontes da Web `Asar` e `Source Sans Pro` personalizadas.

### Fontes de fallback

As fontes da Web geralmente afetam o desempenho devido ao seu tamanho, aumentando potencialmente as pontuações Cumulative Layout Shift (CLS) e reduzindo as pontuações gerais do Lighthouse. Para garantir a exibição instantânea do texto enquanto as fontes da Web estão sendo carregadas, os projetos Edge Delivery Services usam fontes substitutas nativas do navegador. Essa abordagem ajuda a manter uma experiência do usuário suave, enquanto a fonte desejada se aplica.

Para selecionar a melhor fonte de fallback, use a [Extensão Helix Font Fallback Chrome](https://www.aem.live/developer/font-fallback) da Adobe, que determina uma fonte exatamente correspondente para os navegadores usarem antes que a fonte personalizada seja carregada. As declarações de fonte de fallback resultantes devem ser adicionadas ao arquivo `styles/styles.css` para melhorar o desempenho e garantir uma experiência perfeita para os usuários.

![Extensão Helix Font Fallback Chrome](./assets/4-website-branding/font-fallback-chrome-plugin.png){align=center}

Para usar a [extensão do Helix Font Fallback Chrome](https://www.aem.live/developer/font-fallback), verifique se a página da Web tem fontes da Web aplicadas às mesmas variações usadas no site do Edge Delivery Services. Este tutorial demonstra a extensão no [wknd.site](http://wknd.site/us/en.html). Ao desenvolver um site, aplique a extensão ao site em que está sendo trabalhado, em vez de [wknd.site](http://wknd.site/us/en.html).

```css
/* styles/styles.css */
...

/* fallback fonts */

/* Fallback font for Asar (normal - 400) */
@font-face {
    font-family: "asar-normal-400-fallback";
    size-adjust: 95.7%;
    src: local("Arial");
}

/* Fallback font for Source Sans Pro (normal - 400) */
@font-face {
    font-family: "source-sans-pro-normal-400-fallback";
    size-adjust: 92.9%;
    src: local("Arial");
}

...
```

Adicione os nomes de família de fontes de fallback às variáveis CSS de fontes em `styles/styles.css` após os nomes de família de fontes &quot;reais&quot;.

```css
/* styles/styles.css */

:root {
    ...
    /* fonts */
    --body-font-family: 'Source Sans Pro', source-sans-pro-normal-400-fallback, sans-serif;
    --heading-font-family: 'Asar', asar-normal-400-fallback, sans-serif;
    ...
}
```

## Visualização do desenvolvimento

À medida que você adiciona o CSS, o ambiente de desenvolvimento local da CLI do AEM recarrega automaticamente as alterações, tornando mais rápido e fácil ver como o CSS está afetando o bloco.

![Visualização de desenvolvimento de CSS da marca WKND](./assets/4-website-branding/preview.png)


## Baixar arquivos CSS finais

Você pode baixar os arquivos CSS atualizados a partir dos links abaixo:

* [`styles.css`](https://raw.githubusercontent.com/davidjgonzalez/aem-wknd-eds-ue/refs/heads/main/styles/styles.css)
* [`fonts.css`](https://raw.githubusercontent.com/davidjgonzalez/aem-wknd-eds-ue/refs/heads/main/styles/fonts.css)

## Vincule os arquivos CSS

Certifique-se de [lint frequente](./3-local-development-environment.md#linting) suas alterações de código para garantir que elas estejam limpas e consistentes. A pintura regularmente ajuda a capturar problemas antecipadamente e reduz o tempo geral de desenvolvimento. Lembre-se, você não pode mesclar seu trabalho na ramificação principal até que todos os problemas de impressão sejam resolvidos!

```bash
$ npm run lint:css
```

## Mesclar alterações de código

Mescle as alterações na ramificação `main` no GitHub para criar trabalhos futuros sobre essas atualizações.

```bash
$ git add .
$ git commit -m "Add global CSS, CSS variables, and web fonts"
$ git push origin wknd-styles
```

Depois que as alterações forem enviadas para a ramificação `wknd-styles`, crie uma solicitação de pull no GitHub para mesclá-las à ramificação `main`.

1. Acesse o repositório GitHub do capítulo [Criar um novo projeto](./1-new-code-project.md).
2. Clique na guia **Pull requests** e selecione **New pull request**.
3. Defina `wknd-styles` como a ramificação de origem e `main` como a ramificação de destino.
4. Revise as alterações e clique em **Criar solicitação de pull**.
5. Nos detalhes da solicitação pull, **adicione o seguinte**:

   ```
   Add basic global CSS, CSS variables, and web fonts (including fallback fonts) to support the WKND brand.
   
   Fix #1
   
   Test URLs:
   - Before: https://main--wknd-aem-eds-ue--davidjgonzalez.aem.live/
   - After: https://wknd-styles--wknd-aem-eds-ue--davidjgonzalez.aem.live/
   ```

   * O `Fix #1` faz referência ao problema do GitHub criado anteriormente.
   * Os URLs de teste informam à Sincronização de código do AEM quais ramificações usar para validação e comparação. A URL &quot;Depois&quot; usa a ramificação de trabalho `wknd-styles` para verificar como as alterações de código afetam o desempenho do site.

6. Clique em **Criar solicitação de pull**.
7. Aguarde o [aplicativo GitHub da Sincronização de Código do AEM](./1-new-code-project.md) para **concluir as verificações de qualidade**. Se eles falharem, resolva os erros e execute as verificações novamente.
8. Depois que as verificações forem aprovadas, **mescle a solicitação de pull** em `main`.

Com as alterações mescladas em `main`, elas não são consideradas implantadas em produção, e o novo desenvolvimento pode continuar com base nessas atualizações.
