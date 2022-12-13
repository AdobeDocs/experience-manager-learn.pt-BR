---
title: Implantar uma extensão do console Fragmento de conteúdo AEM
description: Saiba como implantar uma extensão do console Fragmento de conteúdo AEM.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
kt: 11603
last-substantial-update: 2022-12-01T00:00:00Z
source-git-commit: a7b32982b547eb292384d2ebde80ba745091702a
workflow-type: tm+mt
source-wordcount: '802'
ht-degree: 0%

---


# Implantar uma extensão

Para uso em AEM ambientes as a Cloud Service, o aplicativo de extensão App Builder deve ser implantado e aprovado.

Há várias considerações a serem levadas em conta ao implantar aplicativos de extensão do App Builder:

+ As extensões são implantadas no espaço de trabalho do projeto do Console do Adobe Developer. Os espaços de trabalho padrão são:
   + __Produção__ o workspace contém implantações de extensão que estão disponíveis em todas as AEM as a Cloud Service.
   + __Fase__ O workspace atua como um espaço de trabalho do desenvolvedor. As extensões implantadas no espaço de trabalho de Preparo não estão disponíveis AEM as a Cloud Service.
Os espaços de trabalho do Console do Adobe Developer não têm correlação direta com AEM tipos de ambiente as a Cloud Service.
+ Uma extensão implantada no espaço de trabalho Produção é exibida em todos os ambientes AEM as a Cloud Service na Org do Adobe em que a extensão existe.
Uma extensão não pode ser limitada aos ambientes com os quais está registrada ao adicionar [lógica condicional que verifica o nome do host as a Cloud Service AEM](https://developer.adobe.com/uix/docs/guides/publication/#enabling-extension-only-on-specific-aem-environments).
+ Várias extensões podem ser usadas AEM as a Cloud Service. O Adobe recomenda que cada extensão do aplicativo App Builder seja usada para resolver um único objetivo comercial. Dito isso, um aplicativo App Builder de extensão única pode implementar vários pontos de extensão que oferecem suporte a um objetivo comercial comum.

## Implantação inicial

Para que uma extensão esteja disponível AEM ambientes as a Cloud Service, ela deve ser implantada no Adobe Developer Console.

O processo de implantação foi dividido em duas etapas lógicas:

1. Implantação do aplicativo App Builder de extensão no Adobe Developer Console por um desenvolvedor.
1. Aprovação da extensão por um gerente de implantação ou proprietário de negócios.

### Implantar o aplicativo App Builder de extensão

Implante a extensão para o Espaço de trabalho Produção. As extensões implantadas no espaço de trabalho Produção são adicionadas automaticamente a todos os serviços de Autor as a Cloud Service AEM no Adobe Org para os quais a extensão é implantada.

1. Abra uma linha de comando na raiz do aplicativo App Builder de extensão atualizado.
1. Certifique-se de que o espaço de trabalho Produção esteja ativo

   ```shell
   $ aio app use -w Production
   ```

   Mesclar quaisquer alterações no `.env` e `.aio`.

1. Implante o aplicativo App Builder de extensão atualizado.

   ```shell
   $ aio app deploy
   ```

#### Solicitar aprovação de implantação

![Enviar extensão para aprovação](./assets/deploy/submit-for-approval.png){align="center"}

1. Faça logon em [Console do Adobe Developer](https://developer.adobe.com)
1. Selecionar __Console__
1. Navegar para __Projetos__
1. Selecione o projeto associado à extensão
1. Selecione o __Produção__ espaço de trabalho
1. Selecionar __Enviar para aprovação__
1. Preencha e envie o formulário, atualizando os campos conforme necessário.

+ Um ícone é necessário. Se você não tiver um ícone, poderá usar [este ícone](./assets/deploy/icon.png).

### Aprovar a solicitação de implantação

![Aprovação de extensão](./assets/deploy/adobe-exchange.png){align="center"}

1. Faça logon em [Adobe Exchange](https://exchange.adobe.com/)
1. Navegar para __Gerenciar__ > __Apps pendente revisão__
1. __Revisão__ o aplicativo App Builder de extensão
1. Se a extensão for aceitável __Aceitar__ a revisão. Isso injeta imediatamente a extensão em todos AEM serviços de Autor as a Cloud Service na Org do Adobe.

Depois que a solicitação de extensão é aprovada, a extensão imediatamente se torna ativa nos AEM serviços do autor as a Cloud Service.

## Atualizar uma extensão

A atualização e a extensão do aplicativo App Builder seguem o mesmo processo da [implantação inicial](#initial-deployment), com o desvio de que a implantação de extensão existente deve ser revogada primeiro.

### Revogar a extensão

Para implantar uma nova versão de uma extensão, ela deve ser revogada (ou removida). Embora a extensão seja Revogada, ela não está disponível AEM consoles.

1. Faça logon em [Adobe Exchange](https://exchange.adobe.com/)
1. Navegar para __Gerenciar__ > __Aplicativos do App Builder__
1. __Revogar__ a Extensão para atualizar

### Implantar a extensão

Implante a extensão para o Espaço de trabalho Produção. As extensões implantadas no espaço de trabalho Produção são adicionadas automaticamente a todos os serviços de Autor as a Cloud Service AEM no Adobe Org para os quais a extensão é implantada.

1. Abra uma linha de comando na raiz do aplicativo App Builder de extensão atualizado.
1. Certifique-se de que o espaço de trabalho Produção esteja ativo

   ```shell
   $ aio app use -w Production
   ```

   Mesclar quaisquer alterações no `.env` e `.aio`.

1. Implante o aplicativo App Builder de extensão atualizado.

   ```shell
   $ aio app deploy
   ```

#### Solicitar aprovação de implantação

![Enviar extensão para aprovação](./assets/deploy/submit-for-approval.png){align="center"}

1. Faça logon em [Console do Adobe Developer](https://developer.adobe.com)
1. Selecionar __Console__
1. Navegar para __Projetos__
1. Selecione o projeto associado à extensão
1. Selecione o __Produção__ espaço de trabalho
1. Selecionar __Enviar para aprovação__
1. Preencha e envie o formulário, atualizando os campos conforme necessário.

#### Aprovar a solicitação de implantação

![Aprovação de extensão](./assets/deploy/adobe-exchange.png){align="center"}

1. Faça logon em [Adobe Exchange](https://exchange.adobe.com/)
1. Navegar para __Gerenciar__ > __Apps pendente revisão__
1. __Revisão__ o aplicativo App Builder de extensão
1. Se a extensão for aceitável __Aceitar__ a revisão. Isso injeta imediatamente a extensão em todos AEM serviços de Autor as a Cloud Service na Org do Adobe.

Depois que a solicitação de extensão é aprovada, a extensão imediatamente se torna ativa nos AEM serviços do autor as a Cloud Service.

## Remover uma extensão

![Remover uma extensão](./assets/deploy/revoke.png)

Para remover uma extensão, revogue-a (ou remova) do Adobe Exchange. Quando a extensão é revogada, ela é removida de todos os AEM serviços de Autor as a Cloud Service.

1. Faça logon em [Adobe Exchange](https://exchange.adobe.com/)
1. Navegar para __Gerenciar__ > __Aplicativos do App Builder__
1. __Revogar__ a extensão a ser removida
