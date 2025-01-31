---
title: Configurar um ambiente de desenvolvimento do local
description: Configure um ambiente de desenvolvimento local para sites fornecidos com Edge Delivery Services e editáveis com o Universal Editor.
version: Cloud Service
feature: Edge Delivery Services
topic: Development
role: Developer
level: Beginner
doc-type: Tutorial
jira: KT-15832
duration: 700
exl-id: 187c305a-eb86-4229-9896-a74f5d9d822e
source-git-commit: 6f0cbdd638ed909b5897521557b65dcf74ac1012
workflow-type: tm+mt
source-wordcount: '967'
ht-degree: 1%

---

# Configuração de um ambiente de desenvolvimento local

Um ambiente de desenvolvimento local é essencial para o desenvolvimento rápido de sites entregues por Edge Delivery Services. O ambiente usa código desenvolvido localmente enquanto fornece conteúdo de Edge Delivery Services, permitindo que os desenvolvedores visualizem instantaneamente as alterações de código. Essa configuração oferece suporte para desenvolvimento e teste rápidos e iterativos.

As ferramentas e os processos de desenvolvimento de um projeto de site do Edge Delivery Services foram projetados para familiarizar os desenvolvedores da Web e fornecer uma experiência de desenvolvimento rápida e eficiente.

## Topologia de desenvolvimento

A topologia de desenvolvimento para um projeto de site do Edge Delivery Services que é editável com o Universal Editor consiste nos seguintes aspectos:

- **Repositório do GitHub**:
   - **Propósito**: hospeda o código do site (CSS e JavaScript).
   - **Estrutura**: a **ramificação principal** contém o código pronto para produção, enquanto outras ramificações contêm o código de trabalho.
   - **Funcionalidade**: o código de qualquer ramificação pode ser executado nos ambientes **produção** ou **pré-visualização** sem afetar o site ativo.

- **Serviço de autoria do AEM**:
   - **Propósito**: serve como o repositório de conteúdo canônico no qual o conteúdo do site é editado e gerenciado.
   - **Funcionalidade**: o conteúdo é lido e gravado pelo **Editor Universal**. O conteúdo editado é publicado em **Edge Delivery Services** nos ambientes de **produção** ou **visualização**.

- **Editor Universal**:
   - **Propósito**: fornece uma interface WYSIWYG para editar o conteúdo do site.
   - **Funcionalidade**: lê e grava no **serviço de Autor do AEM**. Pode ser configurado para usar o código de qualquer ramificação no **repositório do GitHub** para testar e validar as alterações.

- **Edge Delivery Services**:
   - **Ambiente de produção**:
      - **Propósito**: fornece o código e o conteúdo do site ao vivo para os usuários finais.
      - **Funcionalidade**: veicula o conteúdo publicado do **serviço de Autor do AEM** usando o código da **ramificação principal** do **repositório do GitHub**.
   - **Ambiente de visualização**:
      - **Propósito**: fornece um ambiente de preparo para testar e visualizar o conteúdo e o código antes da implantação.
      - **Funcionalidade**: veicula o conteúdo publicado do **serviço de Autor do AEM** usando o código de qualquer ramificação do **repositório do GitHub**, permitindo testes completos sem afetar o site ativo.

- **Ambiente de desenvolvedor local**:
   - **Propósito**: permite aos desenvolvedores gravar e testar o código (CSS e JavaScript) localmente.
   - **Estrutura**:
      - Um clone local do **repositório GitHub** para desenvolvimento baseado em ramificação.
      - A **CLI do AEM**, que atua como um servidor de desenvolvimento, aplica alterações de código local ao **ambiente de Visualização** para obter uma experiência de recarregamento a quente.
   - **Fluxo de trabalho**: desenvolvedores gravam código localmente, confirmam alterações em uma ramificação de trabalho, enviam a ramificação para o GitHub, validam-na no **Editor Universal** (usando a ramificação especificada) e mesclam-na na **ramificação principal** quando pronta para implantação de produção.

## Pré-requisitos

Antes de iniciar o desenvolvimento, instale o seguinte no computador:

1. [Git](https://git-scm.com/)
1. [Node.js e npm](https://nodejs.org)
1. [Microsoft Visual Studio Code](https://code.visualstudio.com/) (ou editor de código semelhante)

## Clonar o repositório GitHub

Clonar o [repositório GitHub criado no novo capítulo de projeto de código](./1-new-code-project.md) que contém o projeto de código AEM Edge Delivery Services para o ambiente de desenvolvimento local.

![Clone do repositório GitHub](./assets/3-local-development-environment/github-clone.png)

```bash
$ cd ~/Code
$ git clone git@github.com:<YOUR_ORG>/aem-wknd-eds-ue.git
```

Uma nova pasta `aem-wknd-eds-ue` é criada no diretório `Code`, que serve como raiz do projeto. Embora o projeto possa ser clonado para qualquer local na máquina, este tutorial usa `~/Code` como diretório raiz.

## Instalar dependências do projeto

Navegue até a pasta do projeto e instale as dependências necessárias com `npm install`. Embora os projetos do Edge Delivery Services não usem sistemas de build Node.js tradicionais como Webpack ou Vite, eles ainda exigem várias dependências para o desenvolvimento local.

```bash
# ~/Code/aem-wknd-eds-ue

$ npm install
```

## Instalar a CLI do AEM

A CLI do AEM é uma ferramenta de linha de comando Node.js projetada para simplificar o desenvolvimento de sites do AEM baseados em Edge Delivery Services, fornecendo um servidor de desenvolvimento local para rápido desenvolvimento e teste do seu site.

Para instalar a CLI do AEM, execute:

```bash
# ~/Code/aem-wknd-eds-ue

$ npm install @adobe/aem-cli
```

A CLI do AEM também pode ser instalada globalmente usando o `npm install --global @adobe/aem-cli`.

## Iniciar o servidor de desenvolvimento local do AEM

O comando `aem up` inicia o servidor de desenvolvimento local e abre automaticamente uma janela do navegador para a URL do servidor. Esse servidor atua como um proxy reverso para o ambiente Edge Delivery Services, veicula conteúdo dali e usa sua base de código local para desenvolvimento.

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

A CLI do AEM abre o site no navegador em `http://localhost:3000/`. As alterações no projeto são automaticamente recarregadas no navegador da Web, enquanto as alterações de conteúdo [exigem a publicação no ambiente de visualização](./6-author-block.md) e a atualização do navegador da Web.

Se o site for aberto com uma página 404, é provável que o [fstab.yaml ou paths.json](https://experienceleague.adobe.com/en/docs/experience-manager-cloud-service/content/edge-delivery/wysiwyg-authoring/edge-dev-getting-started#create-github-project) atualizado em [novo projeto de código](./1-new-code-project.md) esteja configurado incorretamente, ou as alterações não foram confirmadas na ramificação `main`.

## Criar fragmentos JSON

Os projetos do Edge Delivery Services, criados usando o [modelo XWalk de Boilerplate do AEM](https://github.com/adobe-rnd/aem-boilerplate-xwalk), dependem de configurações JSON que permitem a criação de blocos no Editor Universal.

- **Fragmentos JSON**: armazenados com seus blocos associados e definidos os modelos de bloco, definições e filtros.
   - **Fragmentos do modelo**: armazenados em `/blocks/example/_example.json`.
   - **Fragmentos de definição**: armazenados em `/blocks/example/_example.json`.
   - **Fragmentos de filtro**: armazenados em `/blocks/example/_example.json`.

Os scripts NPM compilam esses fragmentos JSON e os colocam nos locais apropriados na raiz do projeto. Para criar arquivos JSON, use os scripts NPM fornecidos. Por exemplo, para compilar todos os fragmentos, execute:

```bash
# ~/Code/aem-wknd-eds-ue

npm run build:json
```

| Script NPM | Descrição |
|--------------------|-----------------------------------------------------------------------------|
| `build:json` | Cria todos os arquivos JSON a partir de fragmentos e os adiciona aos arquivos `component-*.json` apropriados. |
| `build:json:models` | Compila fragmentos JSON de bloco e os compila em `/component-models.json`. |
| `build:json:definitions` | Compila fragmentos JSON de página e os compila em `/component-definitions.json`. |
| `build:json:filters` | Compila fragmentos JSON de página e os compila em `/component-filters.json`. |

>[!TIP]
>
> Execute `npm run build:json` depois de qualquer alteração nos arquivos de fragmento para regenerar os arquivos JSON principais.

## Linting

A impressão garante a qualidade e a consistência do código, o que é necessário para projetos Edge Delivery Services antes de mesclar alterações na ramificação `main`.

Os scripts NPM podem ser executados via `npm run`, por exemplo:

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint
```

| Script NPM | Descrição |
|------------------|--------------------------------------------------|
| `lint:js` | Executa verificações de lista de JavaScript. |
| `lint:css` | Executa verificações de lista de CSS. |
| `lint` | Executa verificações de listagem de JavaScript e CSS. |

### Corrigir problemas de listagem automaticamente

Você pode resolver automaticamente problemas de listas adicionando o seguinte `scripts` ao `package.json` do projeto, e pode ser executado via `npm run`:

```bash
# ~/Code/aem-wknd-eds-ue

$ npm run lint:fix
```

Esses scripts não vêm pré-configurados com o modelo XWalk do Boilerplate do AEM, mas podem ser adicionados ao arquivo `package.json`:

>[!BEGINTABS]

>[!TAB Scripts adicionais]

| Script NPM | Comando | Descrição |
|------------------|------------------------------------------------|-------------------------------------------------------|
| `lint:js:fix` | `npm run lint:js --fix` | Corrige automaticamente problemas de listas do JavaScript. |
| `lint:css:fix` | `stylelint blocks/**/*.css styles/*.css --fix` | Corrige automaticamente problemas de lista de CSS. |
| `lint:fix` | `npm run lint:js:fix && npm run lint:css:fix` | Executa scripts de correção JS e CSS para limpeza rápida. |

>[!TAB exemplo de package.json]

As entradas de script a seguir podem ser adicionadas à matriz `package.json` `scripts`.

```json
{
  ...
  "scripts": [
    ...,
    "lint:js:fix": "npm run lint:js --fix",
    "lint:css:fix": "npm run lint:css --fix",
    "lint:fix": "npm run lint:js:fix && npm run lint:css:fix",
    ...
  ]
  ...
}
```

>[!ENDTABS]
