---
title: Uso do caminho de carregamento de itens para preencher a lista suspensa
description: Configure e preencha uma lista suspensa para ler valores de um nó crx
feature: Adaptive Forms
version: 6.4,6.5
kt: 10961
topic: Development
role: Developer
level: Beginner
source-git-commit: 614db8b03a823b60846ab8ccfa8fbc29a41f7791
workflow-type: tm+mt
source-wordcount: '175'
ht-degree: 0%

---

# Propriedade de carregamento de item no AEM Forms

Configure e preencha a lista suspensa usando a propriedade de caminho de carregamento do item.
O campo Caminho de carga do item permite que um autor forneça um url do qual carregue as opções disponíveis em uma lista suspensa.
Para criar esse nó no crx, siga as etapas mencionadas abaixo

* Logon no crx
* Crie um nó chamado assets(você pode nomear esse nó de acordo com sua necessidade) e digite sling:folder no conteúdo.
* Salvar
* Clique no nó de ativos recém-criados e defina suas propriedades conforme mostrado abaixo
* Será necessário criar uma propriedade do tipo String chamada asset types (você pode nomeá-la de acordo com seu requisito) com vários valores. Forneça os valores desejados e salve.

![item-load-path](assets/item-load-path-crx.png)

Para carregar esses valores na lista suspensa, forneça o seguinte caminho na propriedade de caminho de carregamento de item  **/content/assets/assets/assets**

O pacote de amostra pode ser [baixado aqui](assets/item-load-path-package.zip)
