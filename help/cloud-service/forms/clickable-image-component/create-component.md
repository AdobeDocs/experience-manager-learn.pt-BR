---
title: Criação de componente de imagem clicável
description: Criação do componente de imagem clicável no AEM Forms as a Cloud Service
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
feature: Adaptive Forms
topic: Development
jira: KT-15968
exl-id: b635f171-775d-480e-bf7a-c92ab4af0aee
source-git-commit: b4df652fcda0af5d01077b97aa7fa17cfe2abf4b
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 1%

---

# Criar componente

Este artigo supõe que você tenha alguma experiência em desenvolvimento no AEM Forms CS. Também presume que você tenha criado um projeto de arquétipo do AEM Forms.

Abra o projeto do AEM Forms no IntelliJ ou em qualquer outro IDE de sua escolha. Crie um novo nó chamado svg em

```
apps\corecomponent\components\adaptiveForm
```

>[!NOTE]
>
> ``corecomponent`` é o appId fornecido ao criar o projeto Maven. Esta appId pode ser diferente no seu ambiente.


## Criar arquivo .content.xml

Crie um arquivo chamado .content.xml no nó svg. Adicione o conteúdo a seguir ao arquivo recém-criado. Você pode alterar jcr:description,jcr:title e o componentGroup de acordo com seus requisitos.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<jcr:root xmlns:jcr="http://www.jcp.org/jcr/1.0" xmlns:cq="http://www.day.com/jcr/cq/1.0" xmlns:sling="http://sling.apache.org/jcr/sling/1.0"
    jcr:description="USA MAP"
    jcr:primaryType="cq:Component"
    jcr:title="USA MAP"
    sling:resourceSuperType="wcm/foundation/components/responsivegrid"
    componentGroup="CustomCoreComponent - Adaptive Form"/>
```

## Criar svg.html

Crie um arquivo chamado svg.html. Este arquivo renderizará o SVG do mapa dos EUA. Copie o conteúdo de [svg.html](assets/svg.html) no arquivo recém-criado. O que você copiou é o mapa da SVG dos EUA. Salve o arquivo.

## Implantar o projeto

Implante o projeto na instância pronta para nuvem local para testar o componente.

Para implantar o projeto, você precisará navegar até a pasta raiz do seu projeto na janela de prompt de comando e executar o comando a seguir.

```
mvn clean install -PautoInstallSinglePackage
```

Isso implantará o projeto na instância local do AEM Forms e o componente estará disponível para ser incluído no Formulário adaptável

![mapa dos eua](./assets/usa-map.png)
