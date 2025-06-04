---
title: Opções de bloco
description: Saiba como criar um bloco com várias opções de exibição.
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-17296
duration: 700
exl-id: f41dff22-bd47-4ea0-98cc-f5ca30b22c4b
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '1961'
ht-degree: 100%

---

# Desenvolver um bloco com opções

Este tutorial baseia-se no tutorial do Edge Delivery Services e do editor universal, guiando o usuário pelo processo de adicionar opções a um bloco. Ao definir opções de bloco, você pode personalizar a aparência e a funcionalidade de um bloco, permitindo que diferentes variações supram várias necessidades de conteúdo. Isso permite uma maior flexibilidade e reusabilidade no sistema de design do seu site.

![Opção de bloco lado a lado](./assets/block-options/main.png){align="center"}

Neste tutorial, você adicionará opções ao bloco de teaser, permitindo que os criadores escolham entre duas opções de exibição: **Padrão** e **Lado a lado**. A opção **Padrão** exibe a imagem acima e atrás do texto, enquanto a opção **Lado a lado** exibe a imagem e o texto lado a lado.

## Casos de uso comuns

Os casos de uso comuns das **Opções de bloco** no desenvolvimento do **Edge Delivery Services** e do **editor universal** incluem, mas não se limitam a:

1. **Variações de layout:** alterne facilmente entre os layouts. Por exemplo, horizontal versus vertical ou grade versus lista.
2. **Variações de estilo:** alterne facilmente entre temas ou tratamentos visuais. Por exemplo, modo claro versus escuro ou texto grande versus pequeno.
3. **Controle de exibição de conteúdo:** alterne a visibilidade dos elementos ou alterne entre estilos de conteúdo (compacto versus detalhado).

Essas opções oferecem flexibilidade e eficiência na criação de blocos dinâmicos e adaptáveis.

Este tutorial demonstra o caso de uso de variações de layout, em que o bloco de teaser pode ser exibido em dois layouts diferentes: **Padrão** e **Lado a lado**.

## Modelo de bloco

Para adicionar opções ao bloco de teaser, abra seu fragmento de JSON em `/block/teaser/_teaser.json` e adicione um novo campo à definição do modelo. Esse campo define sua propriedade `name` como `classes`, que é um campo protegido usado pelo AEM para armazenar opções de bloco, que são aplicadas ao HTML do Edge Delivery Services do bloco.

### Configurações de campos

As guias abaixo ilustram várias maneiras de configurar opções de bloco no modelo de bloco, incluindo a seleção única com uma classe de CSS, a seleção única com várias classes de CSS e a seleção múltipla com várias classes de CSS. Este tutorial [implementa a abordagem mais simples](#field-configuration-for-this-tutorial) usada em **Seleção com uma classe de CSS**.

>[!BEGINTABS]

>[!TAB Seleção com uma única classe de CSS]

Este tutorial demonstra como usar um tipo de entrada `select` (lista suspensa) para permitir que os criadores escolham uma opção de bloco, que é aplicada como uma classe de CSS correspondente.

![Seleção com uma única classe de CSS](./assets/block-options/tab-1.png){align="center"}

#### Modelo de bloco

A opção **Padrão** é representada por uma string vazia (`""`), enquanto a opção **Lado a lado** usa `"side-by-side"`. O **nome** e o **valor** da opção não precisam ser iguais, mas o **valor** determina as classes de CSS aplicadas ao HTML do bloco. Por exemplo, o valor da opção **Lado a lado** pode ser `layout-10` em vez de `side-by-side`. No entanto, é melhor usar nomes semanticamente significativos para classes de CSS, garantindo clareza e consistência nos valores da opção.

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="Nome do arquivo da amostra de código abaixo."}

```json{highlight="4,8,9-18"}
...
"fields": [
    {
        "component": "select",
        "name": "classes",
        "value": "",
        "label": "Teaser options",
        "valueType": "string",
        "options": [
            {
                "name": "Default",
                "value": ""
            },
            {
                "name": "Side-by-side",
                "value": "side-by-side"
            }
        ]
    }
]
...
```

#### HTML do bloco

Quando o criador seleciona uma opção, o valor correspondente é adicionado como uma classe de CSS ao HTML do bloco:

- Se **Padrão** for selecionado:

  ```html
  <div class="block teaser">
      <!-- Block content here -->
  </div>
  ```

- Se **Lado a lado** for selecionado:

  ```html
  <div class="block teaser side-by-side">
      <!-- Block content here -->
  </div>
  ```

Permite que um estilo diferente e um JavaScript condicional sejam aplicados, dependendo da abertura escolhida.


>[!TAB Seleção com várias classes de CSS]

**Esta abordagem não é usada neste tutorial, mas ilustra um método alternativo e opções de bloco avançadas.**

O tipo de entrada `select` permite que os criadores escolham uma única opção de bloco, que pode, opcionalmente, ser mapeado para várias classes de CSS. Para isso, liste as classes de CSS como valores delimitados por espaço.

![Seleção com várias classes de CSS](./assets/block-options/tab-2.png){align="center"}

#### Modelo de bloco

Por exemplo, a opção **Lado a lado** pode permitir variações nas quais a imagem aparece à esquerda (`side-by-side left`) ou à direita (`side-by-side right`).

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="Nome do arquivo da amostra de código abaixo."}

```json{highlight="4,8,9-21"}
...
"fields": [
    {
        "component": "select",
        "name": "classes",
        "value": "",
        "label": "Teaser options",
        "valueType": "string",
        "options": [
            {
                "name": "Default",
                "value": ""
            },
            {
                "name": "Side-by-side with Image on left",
                "value": "side-by-side left"
            },
            {
                "name": "Side-by-side with Image on right",
                "value": "side-by-side right"
            }
        ]
    }
]
...
```

#### HTML do bloco

Quando o autor seleciona uma opção, o valor correspondente é aplicado como um conjunto de classes de CSS separadas por espaços no HTML do bloco:

- Se **Padrão** for selecionado:

  ```html
  <div class="block teaser">
      <!-- Block content here -->
  </div>
  ```

- Se **Lado a lado com a imagem à esquerda** for selecionado:

  ```html
  <div class="block teaser side-by-side left">
      <!-- Block content here -->
  </div>
  ```

- Se **Lado a lado com a imagem à direita** for selecionado:

  ```html
  <div class="block teaser side-by-side right">
      <!-- Block content here -->
  </div>
  ```

Permite que estilos diferentes e um JavaScript condicional sejam aplicados, dependendo da opção escolhida.


>[!TAB Seleção múltipla com várias classes de CSS]

**Esta abordagem não é usada neste tutorial, mas ilustra um método alternativo e opções de bloco avançadas.**

O tipo de entrada `"component": "multiselect"` permite que o autor selecione várias opções simultaneamente. Isso permite permutas complexas da aparência do bloco ao combinar várias opções de design.

![Seleção múltipla com várias classes de CSS](./assets/block-options/tab-3.png){align="center"}

### Modelo de bloco

Por exemplo, a **Lado a lado**, **Imagem à esquerda** e **Imagem à direita** podem permitir variações nas quais a imagem é posicionada à esquerda (`side-by-side left`) ou à direita (`side-by-side right`).

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="Nome do arquivo da amostra de código abaixo."}

```json{highlight="4,6,8,10-21"}
...
"fields": [
    {
        "component": "multiselect",
        "name": "classes",
        "value": [],
        "label": "Teaser options",
        "valueType": "array",
        "options": [
            {
                "name": "Side-by-side",
                "value": "side-by-side"
            },
            {
                "name": "Image on left",
                "value": "left"
            },
            {
                "name": "Image on right",
                "value": "right"
            }
        ]
    }
]
...
```

#### HTML do bloco

Quando o criador seleciona várias opções, os valores correspondentes são aplicados como classes de CSS separadas por espaço no HTML do bloco:

- Se **Lado a lado** e **Imagem à esquerda** forem selecionados:

  ```html{highlight="1"}
  <div class="block teaser side-by-side left">
      <!-- Block content here -->
  </div>
  ```

- Se **Lado a lado** e **Imagem à direita** forem selecionados:

  ```html{highlight="1"}
  <div class="block teaser side-by-side right">
      <!-- Block content here -->
  </div>
  ```

Embora a seleção múltipla ofereça flexibilidade, ela aumenta a complexidade do gerenciamento de permutas de design. Sem restrições, seleções conflitantes podem levar a experiências corrompidas ou não condizentes com a marca.

Por exemplo:

- **Imagem à esquerda** ou **Imagem à direita** sem selecionar **Lado a lado** aplica implicitamente o **Padrão**, que sempre define a imagem como um plano de fundo, portanto, o alinhamento à esquerda e à direita é irrelevante.
- A seleção de **Imagem à esquerda** e **Imagem à direita** é contraditória.
- Selecionar **Lado a lado** sem **Imagem à esquerda** ou **Imagem à direita** pode ser considerado ambíguo, pois a posição da imagem não é especificada.

Para evitar problemas e confusão do autor ao usar a seleção múltipla, verifique se as opções foram bem planejadas e se todas as permutas foram testadas. A seleção múltipla funciona melhor para aprimoramentos simples e não conflitantes, como “grande” ou “destaque”, em vez de opções de alteração de layout.


>[!TAB Opção padrão]

**Esta abordagem não é usada neste tutorial, mas ilustra um método alternativo e opções de bloco avançadas.**

As opções de bloco podem ser definidas como padrão ao adicionar uma nova instância de bloco a uma página no editor universal. Para isso, é necessário definir o valor padrão da propriedade `classes` na [definição do bloco](../5-new-block.md#block-definition).

#### Definição do bloco

No exemplo abaixo, a opção padrão é definida como **Lado a Lado**, atribuindo-se a propriedade `value` do campo `classes` a `side-by-side`. A entrada da opção de bloco correspondente no modelo de bloco é opcional.

Você também pode definir várias entradas para o mesmo bloco, cada uma com um nome e uma classe diferentes. Isso permite que o editor universal exiba entradas de blocos distintas, cada uma pré-configurada com uma opção de bloco específica. Embora apareçam como blocos separados no editor, a base de código contém um bloco que é renderizado dinamicamente com base na opção selecionada.

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="Nome do arquivo da amostra de código abaixo."}

```json{highlight="12"}
{
    "definitions": [{
      "title": "Teaser",
      "id": "teaser",
      "plugins": {
        "xwalk": {
          "page": {
            "resourceType": "core/franklin/components/block/v1/block",
            "template": {
              "name": "Teaser",
              "model": "teaser",
              "classes": "side-by-side",
              "textContent_text": "<h2>Enter a title</h2><p>...and body text here!</p>",
              "textContent_cta": "/",
              "textContent_ctaText": "Click me!"
            }
          }
        }
      }
    }],
    "models": [... from previous section ...],
    "filters": []
}
```

>[!ENDTABS]


### Configuração de campos para este tutorial


Neste tutorial, usaremos a abordagem “Seleção com uma única classe de CSS” descrita acima na primeira guia, que permite duas opções de bloco diferentes: **Padrão** e **Lado a lado**.

Na definição do modelo no fragmento do JSON do bloco, adicione um campo de seleção para as opções de bloco. Esse campo permite que os autores escolham entre o layout padrão e o lado a lado.

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="Nome do arquivo da amostra de código abaixo."}

```json{highlight="7-24"}
{
    "definitions": [...],
    "models": [
        {
            "id": "teaser", 
            "fields": [
                {
                    "component": "select",
                    "name": "classes",
                    "value": "",
                    "label": "Teaser options",
                    "description": "",
                    "valueType": "string",
                    "options": [
                        {
                            "name": "Default",
                            "value": ""
                        },
                        {
                            "name": "Side-by-side",
                            "value": "side-by-side"
                        }
                    ]
                },
                {
                    "component": "reference",
                    "valueType": "string",
                    "name": "image",
                    "label": "Image",
                    "multi": false
                },
                {
                    "component": "text",
                    "valueType": "string",
                    "name": "imageAlt",
                    "label": "Image alt text",
                    "required": true
                },
                {
                    "component": "richtext",
                    "name": "textContent_text",
                    "label": "Text",
                    "valueType": "string",
                    "required": true
                },
                {
                    "component": "aem-content",
                    "name": "textContent_cta",
                    "label": "CTA",
                    "valueType": "string"
                },
                {
                    "component": "text",
                    "name": "textContent_ctaText",
                    "label": "CTA label",
                    "valueType": "string"
                }
            ]
        }
    ],
    "filters": []
}
```

## Atualizar bloco no editor universal

Para disponibilizar a entrada das opções de bloco atualizadas no editor universal, implante as alterações de código JSON no GitHub, crie uma nova página, adicione e crie o bloco de teaser com a opção **Lado a lado**, e publique a página para visualização. Depois de publicada, carregue a página no ambiente de desenvolvimento local para codificação.

### Enviar alterações ao GitHub

Para disponibilizar a entrada das opções de bloco atualizadas no editor universal a fim de definir as opções de bloco e desenvolver em relação ao HTML resultante, o projeto precisa ser limpo, e as alterações enviadas a uma ramificação do GitHub (neste caso, a ramificação `block-options`).

```bash
# ~/Code/aem-wknd-eds-ue

# Lint the changes to catch any syntax errors
$ npm run lint 

$ git add .
$ git commit -m "Add Teaser block option to JSON file so it is available in Universal Editor"
$ git push origin teaser
```

### Criar uma página de teste

No serviço do AEM Author, crie uma nova página para adicionar o bloco de teaser para desenvolvimento. Seguindo a convenção do capítulo [Criar um bloco](../6-author-block.md) do [Tutorial do Edge Delivery Services e do editor universal para desenvolvedores](../0-overview.md), crie uma página de teste em uma página `branches`, dando-lhe o nome da ramificação do Git em que você está trabalhando (neste caso, `block-options`).

### Criar o bloco

Edite a nova página **Opções de bloco** no editor universal e adicione o bloco de **teaser**. Adicione o parâmetro de consulta `?ref=block-options` ao URL para carregar a página com o código da ramificação do GitHub `block-options`,

A caixa de diálogo do bloco agora inclui uma lista suspensa de **Opções de teaser**, com as opções **Padrão** e **Lado a lado**. Escolha **Lado a lado** e conclua a criação de conteúdo restante.

![Teaser com caixa de diálogo de opções de bloco](./assets/block-options/block-dialog.png){align="center"}

Opcionalmente, adicione dois blocos de **teaser**: um definido como **Padrão** e o outro como **Lado a lado**. Isso permite que você visualize as duas opções lado a lado durante o desenvolvimento e garante que a implementação da opção **Lado a lado** não afete a opção **Padrão**.

### Publicar para visualização

Depois que o bloco de teaser for adicionado à página, [publique a página para visualização](../6-author-block.md), usando o botão **Publicar** e escolhendo publicar em **Visualizar** no editor universal.

## HTML do bloco

Para iniciar o desenvolvimento do bloco, revise primeiro a estrutura do DOM exposta pela visualização do Edge Delivery Services. O DOM é aprimorado com o JavaScript e estilizado com o CSS, servindo de base para a criação e personalização do bloco.

>[!BEGINTABS]

>[!TAB DOM a ser decorado]

O código a seguir é o DOM do bloco de teaser, com a opção de bloco **Lado a lado** selecionada, que é o destino a ser decorado com JavaScript e CSS.

```html{highlight="7"}
...
<body>
    <header/>
    <main>
        <div>
            <!-- Start block HTML -->
            <div class="teaser block side-by-side" data-block-name="teaser" data-block-status="loaded">
                <div>
                    <div>
                    <picture>
                        <source type="image/webp" srcset="./media_15ba2b455e29aca38c1ca653d24c40acaec8a008f.jpeg?width=2000&amp;format=webply&amp;optimize=medium" media="(min-width: 600px)">
                        <source type="image/webp" srcset="./media_15ba2b455e29aca38c1ca653d24c40acaec8a008f.jpeg?width=750&amp;format=webply&amp;optimize=medium">
                        <source type="image/jpeg" srcset="./media_15ba2b455e29aca38c1ca653d24c40acaec8a008f.jpeg?width=2000&amp;format=jpeg&amp;optimize=medium" media="(min-width: 600px)">
                        <img loading="eager" alt="Woman looking out over a river." src="./media_15ba2b455e29aca38c1ca653d24c40acaec8a008f.jpeg?width=750&amp;format=jpeg&amp;optimize=medium" width="1600" height="900">
                    </picture>
                    </div>
                </div>
                <div>
                    <div>
                    <h2 id="wknd-adventures">WKND Adventures</h2>
                    <p>Join us on one of our next adventures. Browse our list of curated experiences and sign up for one when you're ready to explore with us.</p>
                    <p>Terms and conditions: By signing up, you agree to the rules for participation and booking.</p>
                    <p class="button-container"><a href="/" title="View trips" class="button">View trips</a></p>
                    </div>
                </div>
            </div>     
            <!-- End block HTML -->
        </div>
    </main>
    <footer/>
</body>
...
```

>[!TAB Como encontrar o DOM]

Para localizar o DOM a ser decorado, abra a página com o bloco no seu ambiente de desenvolvimento local, selecione o bloco com as ferramentas de desenvolvedor do navegador da web e inspecione o DOM. Isso permitirá identificar os elementos relevantes a serem decorados.

![Inspecionar DOM do bloco](./assets/block-options/dom.png){align="center"}

>[!ENDTABS]

## CSS do bloco

Edite `blocks/teaser/teaser.css` para adicionar estilos de CSS específicos para a opção **Lado a lado**. Esse arquivo contém o CSS padrão do bloco.

Para modificar os estilos da opção **Lado a lado**, adicione uma nova regra de CSS com escopo no arquivo `teaser.css` para direcionar os blocos de teaser configurados com a classe `side-by-side`.

```css
.block.teaser.side-by-side { ... }
```

Alternativamente, você pode usar o aninhamento de CSS para obter uma versão mais concisa:

```css
.block.teaser {
    ... Default teaser block styles ...

    &.side-by-side {
        ... Side-by-side teaser block styles ...
    }
}
```

Na regra `&.side-by-side`, adicione as propriedades de CSS necessárias para estilizar o bloco quando a classe `side-by-side` for aplicada.

Uma abordagem comum é redefinir os estilos padrão, aplicando-se `all: initial` aos seletores compartilhados e adicionando-se os estilos necessários para a variante `side-by-side`. Se a maioria dos estilos for compartilhada entre as opções, pode ser mais fácil substituir propriedades específicas. No entanto, se vários seletores precisarem de alterações, redefinir todos os estilos e reaplicar apenas os necessários poderá tornar o código mais claro e mais gerenciável.
[!BADGE /blocks/teaser/teaser.css]{type=Neutral tooltip="Nome do arquivo da amostra de código abaixo."}

```css
/* /blocks/teaser/teaser.css */

/* Scope each selector in the block with `.block.teaser` to avoid accidental conflicts outside the block */
.block.teaser {
    animation: teaser-fade-in 1s;
    position: relative;
    width: 1600px;
    max-width: 100vw;
    left: 50%; 
    transform: translateX(-50%);
    height: 500px;
    overflow: hidden; 


    /* The teaser image */
    .image-wrapper {
        position: absolute;
        z-index: -1;
        inset: 0;
        box-sizing: border-box;
        overflow: hidden; 

        .image {
            object-fit: cover;
            object-position: center;
            width: 100%;
            height: 100%;
            transform: scale(1); 
            transition: transform 0.6s ease-in-out;

            .zoom {
                transform: scale(1.1);
            }            
        }
    }

    /* The teaser text content */
    .content {
        position: absolute;
        bottom: 0;
        left: 50%;
        transform: translateX(-50%);
        background: var(--background-color);
        padding: 1.5rem 1.5rem 1rem;
        width: 80vw;
        max-width: 1200px;
  
        .title {
            font-size: var(--heading-font-size-xl);
            margin: 0;
        }

        .title::after {
            border-bottom: 0;
        }

        p {
            font-size: var(--body-font-size-s);
            margin-bottom: 1rem;
            animation: teaser-fade-in .6s;
        }

        p.terms-and-conditions {
            font-size: var(--body-font-size-xs);
            color: var(--secondary-color);
            padding: .5rem 1rem;
            font-style: italic;
            border: solid var(--light-color);
            border-width: 0 0 0 10px;
        }

        /* Add underlines to links in the text */
        a:hover {
            text-decoration: underline;
        }

        /* Add specific spacing to buttons. These button CSS classes are automatically added by Edge Delivery Services. */
        .button-container {
            margin: 0;
            padding: 0;        

            .button {   
                background-color: var(--primary-color);
                border-radius: 0;
                color: var(--dark-color);
                font-size: var(--body-font-size-xs);
                font-weight: bold;
                padding: 1em 2.5em;
                margin: 0;
                text-transform: uppercase;
            }
        }
    }

    /**
    *  Add styling for the side-by-side variant 
    **/

    /* This evaluates to .block.teaser.side-by-side */
    &.side-by-side {    
        /* Since this default teaser option doesn't have a style (such as `.default`), we use `all: initial` to reset styles rather than overriding individual styles. */
        all: initial;
        display: flex;
        margin: auto;
        max-width: 900px;

        .image-wrapper {
            all: initial;
            flex: 2;
            overflow: hidden;                 
            
            * {
                height: 100%;
            }        

            .image {
                object-fit: cover;
                object-position: center;
                width: 100%;
                height: 100%;
                transform: scale(1); 
                transition: transform 0.6s ease-in-out;                

                &.zoom {
                    /* This option has a different zoom level than the default */
                    transform: scale(1.5);
                }
            }
        }

        .content {
            all: initial;
            flex: 1;
            background-color: var(--light-color);
            padding: 3.5em 2em 2em;
            font-size: var(--body-font-size-s);
            font-family: var(--body-font-family);
            text-align: justify;
            text-justify: newspaper;
            hyphens: auto;

            p.terms-and-conditions {
                border: solid var(--text-color);
                border-width: 0;
                padding-left: 0;
                text-align: left;
            }
        }

        /* Media query for mobile devices */
        @media (width <= 900px) {
            flex-direction: column; /* Stack elements vertically on mobile */
        }
    }
}

/** Animations 
    Scope the @keyframes to the block (teaser) to avoid accidental conflicts outside the block

    Global @keyframes can defines in styles/styles.css and used in this file.
**/
@keyframes teaser-fade-in {
    from {
        opacity: 0;
    }

    to {
        opacity: 1;
    }
}
```


## JavaScript do bloco

Identificar a(s) opção(ões) de bloco ativa(s) é simples, bastando verificar as classes aplicadas ao elemento de bloco. Neste exemplo, precisamos ajustar onde os estilos `.image-wrapper` são aplicados de acordo com a opção ativa.

A função `getOptions` retorna uma matriz de classes aplicada ao bloco, excluindo `block` e `teaser` (já que todos os blocos têm a classe `block` e todos os blocos de teaser têm a classe `teaser`). As classes restantes na matriz indicam as opções ativas. Se a matriz estiver vazia, a opção padrão será aplicada.

```javascript
function getOptions(block) {
  // Get the block's classes, excluding 'block' and 'teaser'; anything remaining is a block option.
  return [...block.classList].filter((c) => !['block', 'teaser'].includes(c));
}
```

Essa lista de opções pode ser usada para executar condicionalmente a lógica personalizada no JavaScript do bloco:

```javascript
if (getOptions(block).includes('side-by-side')) {
  /* For side-by-side teaser, add the image-wrapper to a higher-level div to support CSS */
  block.querySelector(':scope > div:first-child').classList.add('image-wrapper');
} else if (!getOptions(block)) {
  /* For the default option, add the image-wrapper to the picture element to support CSS */
  block.querySelector('picture').classList.add('image-wrapper');
}
```

O arquivo de JavaScript completo atualizado para o bloco de teaser com as opções “Padrão” e “Lado a lado” é o seguinte:

[!BADGE /blocks/teaser/teaser.js]{type=Neutral tooltip="Nome do arquivo da amostra de código abaixo."}

```javascript
/* /blocks/teaser/teaser.js */

/**
 * Block options are applied as classes to the block's DOM element
 * alongside the `block` and `<block-name>` classes.
 *
 * @param {HTMLElement} block represents the block's DOM element/tree
 */
function getOptions(block) {
  // Get the block's classes, excluding 'block' and 'teaser'.
  return [...block.classList].filter((c) => !['block', 'teaser'].includes(c));
}

/**
 * Adds a zoom effect to the image using event listeners.
 *
 * When the CTA button is hovered over, the image zooms in.
 *
 * @param {HTMLElement} block represents the block's DOM tree
 */
function addEventListeners(block) {
  block.querySelector('.button').addEventListener('mouseover', () => {
    block.querySelector('.image').classList.add('zoom');
  });

  block.querySelector('.button').addEventListener('mouseout', () => {
    block.querySelector('.image').classList.remove('zoom');
  });
}

/**
 * Entry point to the block's JavaScript.
 * Must be exported as default and accept a block's DOM element.
 * This function is called by the project's style.js, passing the block's element.
 *
 * @param {HTMLElement} block represents the block's DOM element/tree
 */
export default function decorate(block) {
  /* Common treatments for all options */
  block.querySelector(':scope > div:last-child').classList.add('content');
  block.querySelector('h1,h2,h3,h4,h5,h6').classList.add('title');
  block.querySelector('img').classList.add('image');

  // Process each paragraph and mark it as text or terms-and-conditions
  block.querySelectorAll('p').forEach((p) => {
    const innerHTML = p.innerHTML?.trim();
    if (innerHTML?.startsWith('Terms and conditions:')) {
      p.classList.add('terms-and-conditions');
    }
  });

  /* Conditional treatments for specific options */
  if (getOptions(block).includes('side-by-side')) {
    /* For side-by-side teaser, add the image-wrapper to a higher-level div to support CSS */
    block.querySelector(':scope > div:first-child').classList.add('image-wrapper');
  } else if (!getOptions(block)) {
    /* For the default option, add the image-wrapper to the picture element to support CSS */
    block.querySelector('picture').classList.add('image-wrapper');
  }

  addEventListeners(block);
}
```

## Visualização do desenvolvimento

À medida que o CSS e o JavaScript são adicionados, o ambiente de desenvolvimento local da CLI do AEM recarrega as alterações automaticamente, permitindo uma visualização rápida e fácil de como o código afeta o bloco. Passe o mouse sobre a CTA e verifique se o zoom da imagem do teaser aumenta ou diminui.

![Visualização do desenvolvimento local do teaser com CSS e JS](./assets/block-options//local-development-preview.png)

## Limpar o seu código

Certifique-se de [limpar com frequência](../3-local-development-environment.md#linting) as alterações no seu código para garantir que ele esteja limpo e seja consistente. A limpeza periódica ajuda a detectar problemas antecipadamente, reduzindo o tempo geral de desenvolvimento. Lembre-se de que você não pode mesclar o seu trabalho de desenvolvimento com a ramificação `main` até que todos os problemas de limpeza sejam resolvidos.

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint
```

## Visualizar no editor universal

Para exibir as alterações no editor universal do AEM, adicione, confirme e envie-as à ramificação do repositório do Git usada pelo editor universal. Isso garante que a implementação do bloco não interrompa a experiência de criação.

```bash
# ~/Code/aem-wknd-eds-ue

$ git add .
$ git commit -m "CSS and JavaScript implementation for Teaser block option Side-by-side"
# JSON files are compiled automatically and added to the commit via a Husky pre-commit hook
$ git push origin block-options
```

Agora, as alterações ficam visíveis no editor universal ao usar o parâmetro de consulta `?ref=block-options`.

![Teaser no editor universal](./assets/block-options/universal-editor-preview.png){align="center"}


## Parabéns!

Você explorou as opções de bloco no Edge Delivery Services e no editor universal, e agora dispõe das ferramentas para personalizar e simplificar a edição de conteúdo com mais flexibilidade. Comece a aplicar essas opções nos seus projetos para melhorar a eficiência e manter a consistência.

Para ver mais práticas recomendadas e técnicas avançadas, consulte a [documentação do editor universal](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/create-block#block-options).
