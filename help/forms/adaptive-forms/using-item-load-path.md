---
title: Utilização do caminho de carregamento de itens para preencher a lista suspensa
description: Configure e preencha uma lista suspensa para ler valores de um nó crx
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-10961
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2022-09-20T00:00:00Z
thumbnail: item-load.jpg
exl-id: 89c486c8-95c3-4cd4-bf8e-a1b3558f17d6
duration: 34
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '180'
ht-degree: 0%

---

# Propriedade de carregamento de item no AEM Forms

Configure e preencha a lista suspensa usando a propriedade de caminho de carregamento de item.
O campo Caminho de carregamento de item permite que um autor forneça um URL do qual ele carrega as opções disponíveis em uma lista suspensa.
Para criar esse nó no crx, siga as etapas mencionadas abaixo:
* Logon no crx
* Crie um nó chamado assets (você pode nomear esse nó de acordo com seu requisito) digite sling:folder em content.
* Salvar
* Clique no nó ativos recém-criado e defina as propriedades conforme mostrado abaixo
* Será necessário criar uma propriedade do tipo String chamada assettypes (você pode nomeá-la de acordo com seus requisitos). Verifique se a propriedade é um multivalor. Forneça os valores desejados e salve.
  ![caminho-de-carregamento-do-item](assets/item-load-path-crx.png)

Para carregar esses valores na lista suspensa, forneça o seguinte caminho na propriedade de caminho de carregamento de item **/content/assets/assettypes**

O pacote de exemplo pode ser [baixado daqui](assets/item-load-path-package.zip)
