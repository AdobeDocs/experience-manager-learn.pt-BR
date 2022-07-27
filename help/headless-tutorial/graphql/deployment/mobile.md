---
title: Implantações móveis sem periféricos AEM
description: null
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: null
thumbnail: KT-.jpg
mini-toc-levels: 2
source-git-commit: 10032375a23ba99f674fb59a6f6e48bf93908421
workflow-type: tm+mt
source-wordcount: '83'
ht-degree: 4%

---


# Implantações móveis sem periféricos AEM

AEM implantações móveis headless são aplicativos móveis nativos para iOS, Android etc. que consomem e interagem com conteúdo em AEM sem interface.

As implantações móveis exigem configuração mínima, pois as conexões HTTP para AEM APIs sem cabeçalho não são iniciadas no contexto de um navegador.

## Configurações de implantação

| O aplicativo móvel se conecta a | Autor do AEM | AEM Publish | Visualização de AEM |
|-----------------------:|:----------:|:-----------:|:-----------:|
| [Filtros do Dispatcher](./dispatcher-fitlers.md) | ✘ | ✔ | ✔ |
| [Configuração do CORS](./cors.md) | ✘ | ✘ | ✘ |
| Nome do host do URL da imagem | ✔ | ✔ | ✔ |

## Exemplo de aplicativos móveis

O Adobe fornece exemplos de aplicativos móveis iOS e Android.


