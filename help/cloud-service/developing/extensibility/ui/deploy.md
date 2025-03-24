---
title: Implantar uma extensão da interface do usuário do AEM
description: Saiba como implantar uma extensão da interface do usuário do AEM.
feature: Developer Tools
version: Experience Manager as a Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
jira: KT-11603
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: 2e37165d-c003-4206-8133-54e37ca35b8e
duration: 166
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '765'
ht-degree: 0%

---

# Implantar uma extensão

Para uso em ambientes do AEM as a Cloud Service, o aplicativo de extensão do App Builder deve ser implantado e aprovado.

Há várias considerações que devem ser levadas em conta ao implantar aplicativos de extensão do App Builder:

+ As extensões são implantadas no espaço de trabalho do projeto do Adobe Developer Console. Os espaços de trabalho padrão são:
   + O espaço de trabalho __Produção__ contém implantações de extensão que estão disponíveis em todas as AEM as a Cloud Service.
   + O espaço de trabalho __Preparo__ atua como um espaço de trabalho de desenvolvedor. As extensões implantadas no espaço de trabalho do Palco não estão disponíveis no AEM as a Cloud Service.
Os espaços de trabalho do Adobe Developer Console não têm correlação direta com os tipos de ambientes do AEM as a Cloud Service.
+ Uma extensão implantada no espaço de trabalho de Produção é exibida em todos os ambientes do AEM as a Cloud Service na Adobe Org em que a extensão existe.
Uma extensão não pode ser limitada aos ambientes com os quais foi registrada ao adicionar a [lógica condicional que verifica o nome de host do AEM as a Cloud Service](https://developer.adobe.com/uix/docs/guides/publication/#enabling-extension-only-on-specific-aem-environments).
+ Várias extensões podem ser usadas no AEM as a Cloud Service. A Adobe recomenda que cada extensão do aplicativo App Builder seja usada para resolver um único objetivo comercial. Dito isso, um único aplicativo de extensão do App Builder pode implementar vários pontos de extensão que oferecem suporte a um objetivo comercial comum.

## Implantação inicial

Para que uma extensão esteja disponível em ambientes AEM as a Cloud Service, ela deve ser implantada no Adobe Developer Console.

O processo de implantação se divide em duas etapas lógicas:

1. Implantação do aplicativo App Builder de extensão no Adobe Developer Console por um desenvolvedor.
1. Aprovação da extensão por um gerente de implantação ou proprietário da empresa.

### Implantar a extensão

Implante a extensão no espaço de trabalho Produção. As extensões implantadas no espaço de trabalho de Produção são adicionadas automaticamente a todos os serviços do AEM as a Cloud Service Author na Adobe Org em que a extensão foi implantada.

1. Abra uma linha de comando na raiz do aplicativo App Builder de extensão atualizado.
1. Verifique se o espaço de trabalho de Produção está ativo

   ```shell
   $ aio app use -w Production
   ```

   Mesclar alterações em `.env` e `.aio`.

1. Implante o aplicativo App Builder de extensão atualizado.

   ```shell
   $ aio app deploy
   ```

#### Solicitar aprovação de implantação

![Enviar extensão para aprovação](./assets/deploy/submit-for-approval.png){align="center"}

1. Fazer logon no [Adobe Developer Console](https://developer.adobe.com)
1. Selecionar __Console__
1. Navegue até __Projetos__
1. Selecione o projeto associado à extensão
1. Selecione o espaço de trabalho __Produção__
1. Selecione __Enviar para aprovação__
1. Preencha e envie o formulário, atualizando os campos conforme necessário.

### Aprovação de implantação

![Aprovação de extensão](./assets/deploy/adobe-exchange.png){align="center"}

1. Fazer logon no [Adobe Exchange](https://exchange.adobe.com/)
1. Navegue até __Gerenciar__ > __Aplicativos com revisão pendente__
1. __Revise__ a extensão do aplicativo App Builder
1. Se as alterações na extensão forem aceitáveis, __Aceite__ a revisão. Isso injeta imediatamente a extensão em todos os serviços do AEM as a Cloud Service Author na organização da Adobe.

Depois que a solicitação de extensão é aprovada, a extensão imediatamente se torna ativa nos serviços do autor do AEM as a Cloud Service.

## Atualizar uma extensão

A atualização e a extensão do aplicativo App Builder seguem o mesmo processo que a [implantação inicial](#initial-deployment), com o desvio de que a implantação de extensão existente deve ser revogada primeiro.

### Revogar extensão

Para implantar uma nova versão de uma extensão, ela deve primeiro ser revogada (ou removida). Embora a extensão seja Revogada, ela não está disponível nos consoles do AEM.

1. Fazer logon no [Adobe Exchange](https://exchange.adobe.com/)
1. Navegue até __Gerenciar__ > __Aplicativos App Builder__
1. __Revogar__ a extensão a ser atualizada

### Implantar a extensão

Implante a extensão no espaço de trabalho Produção. As extensões implantadas no espaço de trabalho de Produção são adicionadas automaticamente a todos os serviços do AEM as a Cloud Service Author na Adobe Org em que a extensão foi implantada.

1. Abra uma linha de comando na raiz do aplicativo App Builder de extensão atualizado.
1. Verifique se o espaço de trabalho de Produção está ativo

   ```shell
   $ aio app use -w Production
   ```

   Mesclar alterações em `.env` e `.aio`.

1. Implante o aplicativo App Builder de extensão atualizado.

   ```shell
   $ aio app deploy
   ```

#### Solicitar aprovação de implantação

![Enviar extensão para aprovação](./assets/deploy/submit-for-approval.png){align="center"}

1. Fazer logon no [Adobe Developer Console](https://developer.adobe.com)
1. Selecionar __Console__
1. Navegue até __Projetos__
1. Selecione o projeto associado à extensão
1. Selecione o espaço de trabalho __Produção__
1. Selecione __Enviar para aprovação__
1. Preencha e envie o formulário, atualizando os campos conforme necessário.

#### Aprovar a solicitação de implantação

![Aprovação de extensão](./assets/deploy/adobe-exchange.png){align="center"}

1. Fazer logon no [Adobe Exchange](https://exchange.adobe.com/)
1. Navegue até __Gerenciar__ > __Aplicativos com revisão pendente__
1. __Revise__ a extensão do aplicativo App Builder
1. Se as alterações na extensão forem aceitáveis, __Aceite__ a revisão. Isso injeta imediatamente a extensão em todos os serviços do AEM as a Cloud Service Author na organização da Adobe.

Depois que a solicitação de extensão é aprovada, a extensão imediatamente se torna ativa nos serviços do autor do AEM as a Cloud Service.

## Remover uma extensão

![Remover uma extensão](./assets/deploy/revoke.png)

Para remover uma extensão, cancele (ou remova) a extensão do Adobe Exchange. Quando a extensão é revogada, ela é removida de todos os serviços do AEM as a Cloud Service Author.

1. Fazer logon no [Adobe Exchange](https://exchange.adobe.com/)
1. Navegue até __Gerenciar__ > __Aplicativos App Builder__
1. __Revogar__ a Extensão a ser removida
