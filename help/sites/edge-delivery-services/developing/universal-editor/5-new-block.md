---
title: Criar um bloco
description: Crie um bloco para um site do Edge Delivery Services que seja editável com o editor universal.
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 900
exl-id: 9698c17a-0ac8-426d-bccb-729b048cabd1
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '1767'
ht-degree: 100%

---

# Criar um novo bloco

Este capítulo aborda o processo de criação de um novo bloco de teaser editável para um site do Edge Delivery Services por meio do editor universal.

![Novo bloco de teaser](./assets//5-new-block/teaser-block.png)

O bloco, chamado de `teaser`, exibe os seguintes elementos:

- **Imagem**: uma imagem visualmente envolvente.
- **Conteúdo em texto**:
   - **Título**: um título que chame a atenção.
   - **Corpo do texto**: conteúdo descritivo que fornece o contexto ou detalhes, incluindo termos e condições opcionais.
   - **Botão de chamariz (CTA)**: um link criado para solicitar uma interação do usuário e aumentar seu engajamento.

O conteúdo do bloco `teaser` é editável no editor universal, garantindo facilidade de uso e reusabilidade em todo o site.

Observe que o bloco `teaser` é semelhante ao bloco `hero` do modelo padronizado; portanto, o bloco `teaser` serve apenas como um exemplo simples para ilustrar conceitos de desenvolvimento.

## Criar uma nova ramificação do Git

Para manter um fluxo de trabalho limpo e organizado, crie uma nova ramificação para cada tarefa de desenvolvimento específica. Isso ajuda a evitar problemas com a implantação de um código incompleto ou não testado na produção.

1. **Iniciar a partir da ramificação principal**: trabalhar a partir do código de produção mais atualizado garante uma base sólida.
2. **Buscar alterações remotas**: obter as atualizações mais recentes do GitHub garante que o código mais atual esteja disponível antes de iniciar o desenvolvimento.
   - Exemplo: obter as atualizações mais recentes após mesclar as alterações da ramificação `wknd-styles` no `main`.
3. **Criar uma nova ramificação**:

```bash
# ~/Code/aem-wknd-eds-ue

$ git fetch origin  
$ git checkout -b teaser origin/main  
```

Depois de criar a ramificação `teaser`, é possível começar a desenvolver o bloco de teaser.

## Pasta do bloco

Crie uma nova pasta chamada `teaser` no diretório `blocks` do projeto. Essa pasta contém os arquivos JSON, CSS e JavaScript do bloco, organizando os arquivos do bloco em um mesmo local:

```
# ~/Code/aem-wknd-eds-ue

/blocks/teaser
```

O nome da pasta de blocos atua como a ID do bloco e é usado para fazer referência ao bloco durante todo o desenvolvimento.

## Bloco JSON

O bloco JSON define três aspectos principais do bloco:

- **Definição**: registra o bloco como um componente editável no editor universal, vinculando-o a um modelo de bloco e, opcionalmente, a um filtro.
- **Modelo**: especifica os campos de criação do bloco e como esses campos são renderizados como HTML semântico do Edge Delivery Services.
- **Filtro**: configura as regras de filtragem para restringir a quais containers o bloco pode ser adicionado por meio do editor universal. A maioria dos blocos não é composto de containers, mas suas IDs são adicionadas aos filtros de outros blocos de container.

Crie um novo arquivo em `/blocks/teaser/_teaser.json` com a estrutura inicial a seguir, na mesma ordem. Se as chaves estiverem fora de ordem, elas podem não ser criadas corretamente.

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="Nome do arquivo da amostra de código abaixo."}

```json
{
    "definitions": [],
    "models": [],
    "filters": []
}
```

### Modelo de bloco

O modelo de bloco é uma parte crucial da configuração do bloco, pois ele define:

1. A experiência de criação, definindo os campos disponíveis para edição.

   ![Campos do editor universal](./assets/5-new-block/fields-in-universal-editor.png)

2. Como os valores dos campos são renderizados no HTML do Edge Delivery Services.

É atribuído aos modelos um `id` que corresponde à [definição do bloco](#block-definition) e inclui uma matriz `fields` para especificar os campos editáveis.

Cada campo na matriz `fields` tem um objeto JSON que inclui as seguintes propriedades necessárias:

| Propriedade JSON | Descrição |
|---------------|-----------------------------------------------------------------------------------------------------------------------|
| `component` | O [tipo de campo](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/implementing/developing/universal-editor/field-types#component-types), como `text`, `reference` ou `aem-content`. |
| `name` | O nome do campo que está mapeado para a propriedade JCR na qual o valor é armazenado no AEM. |
| `label` | O rótulo exibido para autores no editor universal. |

Para obter uma lista abrangente de propriedades, incluindo as opcionais, confira a [documentação de campos do editor universal](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/implementing/developing/universal-editor/field-types#fields).

#### Design do bloco

![Bloco de teaser](./assets/5-new-block/block-design.png)

O bloco de teaser inclui os seguintes elementos editáveis:

1. **Imagem**: representa o conteúdo visual do teaser.
2. **Conteúdo em texto**: inclui o título, o corpo do texto e o botão de chamariz, inseridos em um retângulo branco.
   - O **título** e o **corpo do texto** podem ser criados por meio do mesmo editor de rich text.
   - O **botão de chamariz** pode ser criado por meio de um campo `text` para o **rótulo** e através de um campo `aem-content` para o **link**.

O design do bloco de teaser é dividido entre esses dois componentes lógicos (imagem e conteúdo de texto), garantindo uma experiência de criação estruturada e intuitiva para os usuários.

### Campos do bloco

Defina os campos necessários para o bloco: imagem, texto alternativo da imagem, texto, rótulo e link do botão de chamariz.

>[!BEGINTABS]

>[!TAB A maneira correta]

**Esta guia ilustra a forma correta de modelar o bloco de teaser.**

O teaser consiste em duas áreas lógicas: imagem e texto. Para simplificar o código necessário para exibir o HTML do Edge Delivery Services como a experiência da web desejada, o modelo de bloco deve refletir essa estrutura.

- Agrupe a **imagem** e o **texto alternativo da imagem** por meio da opção de [recolher o campo](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#field-collapse).
- Agrupe os campos de conteúdo de texto por meio do [agrupamento de elementos](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#element-grouping) e do [recolhimento de campos para o botão de chamariz](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#field-collapse).

Se você não tiver familiaridade com o [recolhimento de campo](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#field-collapse), o [agrupamento de elementos](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#element-grouping) ou a [inferência de tipo](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#type-inference), consulte a documentação vinculada antes de continuar, pois esses elementos são essenciais para criar um modelo de bloco bem estruturado.

No exemplo abaixo:

- A [inferência de tipo](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#type-inference) é usada para criar automaticamente um elemento HTML `<img>` a partir do campo `image`. O recolhimento de campo é usado nos campos `image` e `imageAlt` para criar um elemento HTML `<img>`. O atributo `src` é definido como o valor do campo `image`, enquanto o atributo `alt` é definido como o valor do campo `imageAlt`.
- `textContent` é o nome de um grupo usado para categorizar campos. Ele deve ser semântico, mas pode ser qualquer item exclusivo desse bloco. Isso solicita ao editor universal que renderize todos os campos com esse prefixo dentro do mesmo elemento `<div>` na saída final em HTML.
- O recolhimento de campo também é aplicado no grupo `textContent` para o botão de chamariz (CTA). O CTA é criado como um `<a>` por meio da [inferência de tipo](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#type-inference). O campo `cta` é usado para definir o atributo `href` do elemento `<a>`, enquanto que o campo `ctaText` fornece o conteúdo de texto do link dentro das tags `<a ...>`.

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="Nome do arquivo da amostra de código abaixo."}

```json
{
    "definitions": [],
    "models": [
        {
            "id": "teaser", 
            "fields": [
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

Este modelo define as entradas de criação do bloco no editor universal.

O HTML do Edge Delivery Services resultante desse bloco coloca a imagem na primeira div e os campos do grupo de elementos `textContent` na segunda div.

```html
<div>
    <div>
        <!-- This div contains the field-collapsed image fields  -->
        <picture>
            ...
            <source .../>            
            <img src="..." alt="The authored alt text"/>
        </picture>
    </div>
    <div>
        <!-- This div, via element grouping contains the textContent fields -->
        <h2>The authored title</h2>
        <p>The authored body text</p>
        <a href="/authored/cta/link">The authored CTA label</a>
    </div>
</div>        
```

Como demonstrado [no próximo capítulo](./7a-block-css.md), essa estrutura de HTML simplifica a estilização do bloco como uma unidade coesa.

Para entender as consequências de não usar o recolhimento de campo e o agrupamento de elementos, consulte a guia **A maneira errada** acima.

>[!TAB A maneira errada]

**Esta guia ilustra uma maneira não ideal de modelar o bloco de teaser, sendo apenas uma justaposição ao jeito correto.**

Pode parecer interessante definir cada campo como independente no modelo de bloco sem usar [recolhimento de campo](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#field-collapse) e [agrupamento de elementos](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#element-grouping). No entanto, isso dificulta a estilização do bloco como uma unidade coesa.

Por exemplo, o modelo de teaser pode ser definido **sem** recolhimento de campos ou agrupamento de elementos da seguinte maneira:

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="Nome do arquivo da amostra de código abaixo."}

```json
{
    "definitions": [],
    "models": [
        {
            "id": "teaser", 
            "fields": [
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
                    "name": "alt",
                    "label": "Image alt text",
                    "required": true
                },
                {
                    "component": "richtext",
                    "name": "text",
                    "label": "Text",
                    "valueType": "string",
                    "required": true
                },
                {
                    "component": "aem-content",
                    "name": "link",
                    "label": "CTA",
                    "valueType": "string"
                },
                {
                    "component": "text",
                    "name": "label",
                    "label": "CTA label",
                    "valueType": "string"
                }
            ]
        }
    ],
    "filters": []
}
```

O HTML do Edge Delivery Services do bloco renderiza o valor de cada campo em um `div` separado, dificultando a compreensão do conteúdo, a aplicação de estilos e os ajustes da estrutura do HTML para atingir o design desejado.

```html
<div>
    <div>
        <!-- This div contains the field-collapsed image  -->
        <picture>
            ...
            <source .../>            
            <img src="/authored/image/reference"/>
        </picture>
    </div>
    <div>
        <p>The authored alt text</p>
    </div>
    <div>
        <h2>The authored title</h2>
        <p>The authored body text</p>
    </div>
    <div>
        <a href="/authored/cta/link">/authored/cta/link</a>
    </div>
    <div>
        The authored CTA label
    </div>
</div>        
```

Cada campo é isolado em seu próprio `div`, tornando difícil estilizar a imagem e o conteúdo do texto como unidades coesas. É possível atingir o design desejado com esforço e criatividade, mas usar o [agrupamento de elementos](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#element-grouping) para agrupar campos de conteúdo em texto e o [recolhimento de campos](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#field-collapse) para adicionar valores criados como atributos de elemento é mais simples, mais fácil e semanticamente correto.

Consulte a guia **A maneira correta** acima para saber como modelar melhor o bloco de teaser.

>[!ENDTABS]


### Definição do bloco

A definição do bloco registra-o no editor universal. Confira a seguir um detalhamento das propriedades JSON usadas na definição do bloco:

| Propriedade JSON | Descrição |
|---------------|-------------|
| `definition.title` | O título do bloco conforme exibido nos blocos **Adicionar** do editor universal. |
| `definition.id` | Um identificador exclusivo para o bloco, usado para controlar seu uso em `filters`. |
| `definition.plugins.xwalk.page.resourceType` | Define o tipo de recurso do Sling para renderizar o componente no editor universal. Sempre use um tipo de recurso `core/franklin/components/block/v#/block`. |
| `definition.plugins.xwalk.page.template.name` | O nome do bloco. Deve estar em minúsculas e ser hifenizado para corresponder ao nome da pasta do bloco. Esse valor também é usado para rotular a instância do bloco no editor universal. |
| `definition.plugins.xwalk.page.template.model` | Vincula esta definição à sua definição de `model`, que controla os campos de criação exibidos para o bloco no editor universal. O valor aqui deve corresponder a um valor de `model.id`. |
| `definition.plugins.xwalk.page.template.classes` | Propriedade opcional, cujo valor é adicionado ao atributo `class` do elemento do HTML do bloco. Isso permite variantes do mesmo bloco. O valor `classes` pode tornar-se editável ao [adicionar um campo de classes](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/create-block#block-options) ao [modelo](#block-model) do bloco. |


Este é um exemplo de JSON para a definição do bloco:

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="Nome do arquivo da amostra de código abaixo."}

```json
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

Neste exemplo:

- O bloco é denominado “Teaser” e usa o modelo `teaser`, que determina quais campos estão disponíveis para edição no editor universal.
- O bloco inclui conteúdo padrão para o campo `textContent_text`, que é uma área de rich text para o título e o corpo do texto, e `textContent_cta` e `textContent_ctaText` para o link e o rótulo do CTA (chamada para ação). Os nomes de campos do modelo com um conteúdo inicial correspondem aos nomes de campos definidos na [matriz de campos do modelo de conteúdo](#block-model);

Essa estrutura garante que o bloco seja configurado no editor universal com os campos, o modelo de conteúdo e o tipo de recurso adequados para renderização.

### Filtros do bloco

A matriz `filters` do bloco define, para [blocos de container](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#container), quais outros blocos podem ser adicionados ao container. Os filtros definem uma lista de IDs de blocos (`model.id`) que podem ser adicionados ao container.

[!BADGE /blocks/teaser/_teaser.json]{type=Neutral tooltip="Nome do arquivo da amostra de código abaixo."}

```json
{
  "definitions": [... populated from previous section ...],
  "models": [... populated from previous section ...],
  "filters": []
}
```

O componente de teaser não é um [bloco de container](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/content-modeling#container), o que significa que não é possível adicionar outros blocos a ele. Como resultado, sua matriz `filters` é deixada em branco. Em vez disso, adicione a ID do teaser à lista de filtros do bloco de seção, para que o teaser possa ser adicionado a uma seção.

![Filtros do bloco](./assets/5-new-block/filters.png)

Blocos fornecidos pela Adobe, como o bloco de seção, armazenam filtros na pasta `models` do projeto. Para ajustar, localize o arquivo JSON do bloco fornecido pela Adobe (por exemplo, `/models/_section.json`) e adicione a ID do teaser (`teaser`) à lista de filtros. A configuração sinaliza ao editor universal que o componente de teaser pode ser adicionado ao bloco de container da seção.

[!BADGE /models/_section.json]{type=Neutral tooltip="Nome do arquivo da amostra de código abaixo."}

```json
{
  "definitions": [],
  "models": [],
  "filters": [
    {
      "id": "section",
      "components": [
        "text",
        "image",
        "button",
        "title",
        "hero",
        "cards",
        "columns",
        "fragment",
        "teaser"
      ]
    }
  ]
}
```

A ID de definição do bloco de teaser de `teaser` foi adicionada à matriz `components`.

## Limpe os seus arquivos JSON

Certifique-se de [limpar com frequência](./3-local-development-environment.md#linting) as alterações no seu código para garantir que ele esteja limpo e consistente. A limpeza periódica ajuda a detectar problemas antecipadamente e reduz o tempo geral de desenvolvimento. O comando `npm run lint:js` também limpa arquivos JSON e capta todos os erros de sintaxe.

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint:js
```

## Compilar o JSON do projeto

Após configurar os arquivos JSON do bloco (por exemplo, `blocks/teaser/_teaser.json`, `models/_section.json`), eles são automaticamente compilados nos arquivos `component-models.json`, `component-definitions.json` e `component-filters.json` do projeto. Essa compilação é tratada automaticamente por um gancho de pré-confirmação [Husky](https://typicode.github.io/husky/) incluso no [modelo de projeto XWalk padronizado do AEM](https://github.com/adobe-rnd/aem-boilerplate-xwalk).

As compilações também podem ser acionadas de forma manual ou programática, usando-se os scripts NPM de [compilação de JSON](./3-local-development-environment.md#build-json-fragments) do projeto.

## Implantar o JSON do bloco

Para disponibilizar o bloco no editor universal, o projeto precisa ser confirmado e enviado a uma ramificação de um repositório do GitHub, neste caso, a ramificação `teaser`.

O nome exato da ramificação usado pelo editor universal pode ser ajustado, por usuário, por meio do URL do editor universal.

```bash
# ~/Code/aem-wknd-eds-ue

$ git add .
$ git commit -m "Add teaser block JSON files so it is available in Universal Editor"
# JSON files are compiled automatically and added to the commit via a husky precommit hook
$ git push origin teaser
```

Quando o editor universal é aberto com o parâmetro de consulta `?ref=teaser`, o novo bloco `teaser` aparece na paleta de blocos. Observe que o bloco não tem estilo; ele renderiza os campos do bloco como HTML semântico, estilizado somente por meio do [CSS global](./4-website-branding.md#global-css).
