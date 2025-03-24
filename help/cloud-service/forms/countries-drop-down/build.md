---
title: Criar, implantar e testar o componente de países
description: Criar, implantar e testar
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Adaptive Forms
badgeVersions: label="AEM Forms as a Cloud Service" before-title="false"
jira: KT-16517
exl-id: ab9bd406-e25e-4e3c-9f67-ad440a8db57e
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '209'
ht-degree: 0%

---

# Criar, implantar e testar o componente de países

Para compilar todos os módulos e implantar o pacote `all` em uma instância local do AEM, execute o seguinte comando no diretório raiz do projeto:

```mvn clean install -PautoInstallSinglePackage```

## Testar o componente

Para integrar o componente de Países à sua instância do AEM Forms Cloud Ready e configurá-la, siga estas etapas:

* Extraia o conteúdo do arquivo zip [países](assets/countries.zip). Cada arquivo deve conter os dados de um continente específico.
* Fazer upload dos arquivos json em content/dam/corecomponent.This é o local onde o código procura os arquivos json. Se você quiser armazenar os arquivos JSON em um local diferente, será necessário atualizar o código Java na classe ProductsDropDownImpl. Especificamente, atualize o caminho no método init() onde os arquivos JSON são carregados. Por exemplo, se você deseja armazenar os arquivos JSON em content/dam/mydata/, atualize o caminho desta forma

```java
String jsonPath = "/content/dam/mydata/" + getContinent() + ".json"; // Update path accordingly
```

* Faça logon na sua instância do AEM Forms Cloud Ready
* Crie um formulário adaptável e solte o componente Países no formulário
* Configure o componente Países usando o editor de diálogo e defina as várias propriedades, incluindo o continente
  ![continente](assets/select-continent.png)
* Pré-visualize o formulário e verifique se o menu suspenso dos países funciona conforme esperado
