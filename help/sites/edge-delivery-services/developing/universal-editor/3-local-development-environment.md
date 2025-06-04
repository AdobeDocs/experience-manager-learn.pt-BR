---
title: Configurar um ambiente de desenvolvimento local
description: Configure um ambiente de desenvolvimento local para sites fornecidos pelo Edge Delivery Services que são editáveis no editor universal.
version: Experience Manager as a Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 700
exl-id: 187c305a-eb86-4229-9896-a74f5d9d822e
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '994'
ht-degree: 100%

---

# Configuração de um ambiente de desenvolvimento local

Um ambiente de desenvolvimento local é essencial para um desenvolvimento rápido de sites entregues pelo Edge Delivery Services. O ambiente usa o código desenvolvido localmente enquanto fornece conteúdo do Edge Delivery Services, permitindo que desenvolvedores visualizem instantaneamente as alterações no código. Essa configuração permite um desenvolvimento e testes rápidos e iterativos.

As ferramentas e os processos de desenvolvimento de um projeto de site do Edge Delivery Services foram projetados para serem familiares aos desenvolvedores da web e para fornecer uma experiência de desenvolvimento rápida e eficiente.

## Topologia de desenvolvimento

Este vídeo fornece uma visão geral da topologia de desenvolvimento para um projeto de site do Edge Delivery Services que seja editável com o editor universal.

>[!VIDEO](https://video.tv.adobe.com/v/3443983/?learn=on&enablevpops&captions=por_br)

+++Veja mais detalhes da topologia de desenvolvimento

- **Repositório do GitHub**:
   - **Finalidade**: hospeda o código do site (CSS e JavaScript).
   - **Estrutura**: a **ramificação principal** contém o código pronto para produção, enquanto outras ramificações contêm o código de trabalho.
   - **Funcionalidade**: o código de qualquer ramificação pode ser executado nos ambientes de **produção** ou **visualização** sem afetar o site ativo.

- **Serviço de criação do AEM**:
   - **Finalidade**: serve como o repositório de conteúdo canônico no qual o conteúdo do site é editado e gerenciado.
   - **Funcionalidade**: o conteúdo é lido e gravado pelo **editor universal**. O conteúdo editado é publicado no **Edge Delivery Services** em ambientes de **produção** ou **visualização**.

- **Editor universal**:
   - **Finalidade**: fornece uma interface do tipo WYSIWYG para editar o conteúdo do site.
   - **Funcionalidade**: lê e grava no **serviço de criação do AEM**. Pode ser configurado para usar o código de qualquer ramificação do **repositório do GitHub** para testar e validar as alterações.

- **Edge Delivery Services**:
   - **Ambiente de produção**:
      - **Finalidade**: fornece o código e o conteúdo do site ativo aos usuários finais.
      - **Funcionalidade**: veicula o conteúdo publicado do **serviço de criação do AEM** usando o código da **ramificação principal** do **repositório do GitHub**.
   - **Ambiente de visualização**:
      - **Finalidade**: fornece um ambiente de preparo para testar e visualizar o conteúdo e o código antes da implantação.
      - **Funcionalidade**: veicula o conteúdo publicado pelo **serviço de criação do AEM** usando o código de qualquer ramificação do **repositório do GitHub**, permitindo realizar testes completos sem afetar o site ativo.

- **Ambiente de desenvolvedor local**:
   - **Finalidade**: permite que desenvolvedores gravem e testem o código (CSS e JavaScript) localmente.
   - **Estrutura**:
      - Um clone local do **repositório do GitHub** para desenvolvimento baseado em ramificações.
      - A **CLI do AEM**, que atua como um servidor de desenvolvimento, aplica as alterações no código local ao **ambiente de visualização** para fornecer uma experiência de recarregamento automático.
   - **Fluxo de trabalho**: os desenvolvedores gravam o código localmente, confirmam as alterações em uma ramificação de trabalho, enviam a ramificação ao GitHub, validam no **editor universal** (usando a ramificação especificada) e mesclam com a **ramificação principal** quando o código está pronto para implantação na produção.

+++

## Pré-requisitos

Antes de iniciar o desenvolvimento, instale o seguinte no seu computador:

1. [Git](https://git-scm.com/)
1. [Node.js e npm](https://nodejs.org)
1. [Microsoft Visual Studio Code](https://code.visualstudio.com/) (ou um editor de código semelhante)

## Clone o repositório do GitHub

Clone o [repositório do GitHub criado no novo capítulo de projeto de código](./1-new-code-project.md) que contém o projeto de código do Edge Delivery Services do AEM para seu ambiente de desenvolvimento local.

![Clone do repositório do GitHub](./assets/3-local-development-environment/github-clone.png)

```bash
$ cd ~/Code
$ git clone git@github.com:<YOUR_ORG>/aem-wknd-eds-ue.git
```

Uma nova pasta `aem-wknd-eds-ue` é criada no diretório `Code`, que serve como raiz do projeto. Embora o projeto possa ser clonado em qualquer local na máquina, este tutorial usa `~/Code` como o diretório raiz.

## Instalar dependências do projeto

Navegue até a pasta do projeto e instale as dependências necessárias com `npm install`. Embora os projetos do Edge Delivery Services não usem sistemas de compilação de Node.js tradicionais, como Webpack ou Vite, eles ainda exigem várias dependências para o desenvolvimento local.

```bash
# ~/Code/aem-wknd-eds-ue

$ npm install
```

## Instalar a CLI do AEM

A CLI do AEM é uma ferramenta de linha de comando de Node.js criada para simplificar o desenvolvimento de sites do AEM baseados no Edge Delivery Services, fornecendo um servidor de desenvolvimento local para agilizar o desenvolvimento e teste do seu site.

Para instalar a CLI do AEM, execute:

```bash
# ~/Code/aem-wknd-eds-ue

$ npm install @adobe/aem-cli
```

A CLI do AEM também pode ser instalada globalmente com `npm install --global @adobe/aem-cli`.

## Iniciar o servidor de desenvolvimento local do AEM

O comando `aem up` inicia o servidor de desenvolvimento local e abre automaticamente uma janela do navegador com o URL do servidor. Esse servidor atua como um proxy reverso para o ambiente do Edge Delivery Services, veiculando o conteúdo a partir desse local e usando sua base de código local para desenvolvimento.

```bash
$ cd ~/Code/aem-wknd-eds-ue 
$ aem up

    ___    ________  ___                          __      __ 
   /   |  / ____/  |/  /  _____(_)___ ___  __  __/ /___ _/ /_____  _____
  / /| | / __/ / /|_/ /  / ___/ / __ `__ \/ / / / / __ `/ __/ __ \/ ___/
 / ___ |/ /___/ /  / /  (__  ) / / / / / / /_/ / / /_/ / /_/ /_/ / /
/_/  |_/_____/_/  /_/  /____/_/_/ /_/ /_/\__,_/_/\__,_/\__/\____/_/

info: Starting AEM dev server version x.x.x
info: Local AEM dev server up and running: http://localhost:3000/
info: Enabled reverse proxy to https://main--aem-wknd-eds-ue--<YOUR_ORG>.aem.page
```

A CLI do AEM abre o site no navegador, em `http://localhost:3000/`. As alterações no projeto são recarregadas automaticamente no navegador da web, enquanto que as alterações no conteúdo [exigem a publicação no ambiente de visualização](./6-author-block.md) e a atualização do navegador da web.

Se o site for aberto com uma página 404, é provável que o [fstab.yaml ou paths.json](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-github-project) atualizado no [novo projeto de código](./1-new-code-project.md) esteja configurado incorretamente ou que as alterações não tenham sido confirmadas na ramificação `main`.

## Criar fragmentos de JSON

Os projetos do Edge Delivery Services criados com o [modelo XWalk padronizado do AEM](https://github.com/adobe-rnd/aem-boilerplate-xwalk) dependem das configurações de JSON que permitem a criação de blocos no editor universal.

- **Fragmentos de JSON**: são armazenados com seus blocos associados e definem os modelos, as definições e os filtros do bloco.
   - **Fragmentos de modelo**: armazenados em `/blocks/example/_example.json`.
   - **Fragmentos de definição**: armazenados em `/blocks/example/_example.json`.
   - **Fragmentos de filtro**: armazenados em `/blocks/example/_example.json`.


O [modelo de projeto XWalk padronizado do AEM](https://github.com/adobe-rnd/aem-boilerplate-xwalk) inclui um gancho [Husky](https://typicode.github.io/husky/) de pré-confirmação que detecta alterações nos fragmentos de JSON e as compila nos arquivos `component-*.json` apropriados em `git commit`.

Embora os seguintes scripts de NPM possam ser executados manualmente por meio do `npm run` para criar os arquivos JSON, isso geralmente é desnecessário, pois o gancho Husky de pré-confirmação os manipula automaticamente.

```bash
# ~/Code/aem-wknd-eds-ue

npm run build:json
```

| Script de NPM | Descrição |
|--------------------|-----------------------------------------------------------------------------|
| `build:json` | Cria todos os arquivos JSON a partir de fragmentos e adiciona-os aos arquivos `component-*.json` apropriados. |
| `build:json:models` | Cria os fragmentos JSON do bloco e os compila em `/component-models.json`. |
| `build:json:definitions` | Cria os fragmentos JSON da página e os compila em `/component-definitions.json`. |
| `build:json:filters` | Cria os fragmentos JSON da página e os compila em `/component-filters.json`. |

>[!TIP]
>
> Execute `npm run build:json` depois de qualquer alteração nos arquivos de fragmento para regenerar os arquivos JSON principais.

## Limpeza

A limpeza garante a qualidade e a consistência do código, que são exigidas em projetos do Edge Delivery Services antes de mesclar alterações com a ramificação `main`.

Os scripts de NPM podem ser executados via `npm run`, como neste exemplo:

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint
```

| Script de NPM | Descrição |
|------------------|--------------------------------------------------|
| `lint:js` | Executa verificações de limpeza do JavaScript. |
| `lint:css` | Executa verificações de limpeza de CSS. |
| `lint` | Executa verificações de limpeza de JavaScript e CSS. |

### Corrigir problemas de limpeza automaticamente

É possível resolver problemas de limpeza automaticamente por adicionar estes `scripts` ao `package.json` do projeto, os quais podem ser executados via `npm run`:

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint:fix
```

Esses scripts não vêm pré-configurados com o modelo XWalk padronizado do AEM, mas podem ser adicionados ao arquivo `package.json`:

>[!BEGINTABS]

>[!TAB Scripts adicionais]

| Script de NPM | Comando | Descrição |
|------------------|------------------------------------------------|-------------------------------------------------------|
| `lint:js:fix` | `npm run lint:js -- --fix` | Corrige automaticamente problemas de limpeza de JavaScript. |
| `lint:css:fix` | `stylelint blocks/**/*.css styles/*.css -- --fix` | Corrige automaticamente problemas de limpeza de CSS. |
| `lint:fix` | `npm run lint:js:fix && npm run lint:css:fix` | Executa scripts de correção de JS e CSS para uma limpeza rápida. |

>[!TAB Exemplo de package.json]

As entradas de script a seguir podem ser adicionadas à matriz `scripts` do `package.json`.

```json
{
  ...
  "scripts": [
    ...,
    "lint:js:fix": "npm run lint:js -- --fix",
    "lint:css:fix": "npm run lint:css -- --fix",
    "lint:fix": "npm run lint:js:fix && npm run lint:css:fix",
    ...
  ]
  ...
}
```

>[!ENDTABS]
