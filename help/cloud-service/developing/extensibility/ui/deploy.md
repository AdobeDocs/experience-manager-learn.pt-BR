---
title: Implantar uma extensão da interface do usuário para AEM
description: Saiba como implantar uma extensão da interface do usuário do AEM.
feature: Developer Tools
version: Cloud Service
topic: Development
role: Developer
level: Beginner
recommendations: noDisplay, noCatalog
jira: KT-11603
last-substantial-update: 2023-06-02T00:00:00Z
exl-id: 2e37165d-c003-4206-8133-54e37ca35b8e
duration: 214
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '765'
ht-degree: 0%

---

# Implantar uma extensão

Para uso em ambientes AEM as a Cloud Service, o aplicativo App Builder de extensão deve ser implantado e aprovado.

Há várias considerações que devem ser levadas em conta ao implantar aplicativos de extensão do App Builder:

+ As extensões são implantadas no espaço de trabalho do projeto do Adobe Developer Console. Os espaços de trabalho padrão são:
   + __Produção__ O espaço de trabalho contém implantações de extensão disponíveis em todos os AEM as a Cloud Service.
   + __Estágio__ o espaço de trabalho atua como um espaço de trabalho de desenvolvedor. As extensões implantadas no espaço de trabalho do Palco não estão disponíveis no AEM as a Cloud Service.
Os espaços de trabalho do Console do Adobe Developer não têm correlação direta com os tipos de ambientes as a Cloud Service AEM.
+ Uma extensão implantada no espaço de trabalho de Produção é exibida em todos os ambientes AEM as a Cloud Service na Adobe Org em que a extensão existe.
Uma extensão não pode se limitar aos ambientes nos quais está registrada ao adicionar [lógica condicional que verifica o nome de host as a Cloud Service do AEM](https://developer.adobe.com/uix/docs/guides/publication/#enabling-extension-only-on-specific-aem-environments).
+ Várias extensões podem ser usadas no AEM as a Cloud Service. A Adobe recomenda que cada aplicativo do App Builder de extensão seja usado para resolver um único objetivo comercial. Dito isso, um único aplicativo de extensão do App Builder pode implementar vários pontos de extensão que oferecem suporte a um objetivo comercial comum.

## Implantação inicial

Para que uma extensão esteja disponível em ambientes as a Cloud Service para AEM, ela deve ser implantada no console do Adobe Developer.

O processo de implantação se divide em duas etapas lógicas:

1. Implantação do aplicativo App Builder da extensão no Adobe Developer Console por um desenvolvedor.
1. Aprovação da extensão por um gerente de implantação ou proprietário da empresa.

### Implantar a extensão

Implante a extensão no espaço de trabalho Produção. As extensões implantadas no espaço de trabalho de Produção são adicionadas automaticamente a todos os serviços de autor as a Cloud Service do AEM na Adobe Org em que a extensão é implantada.

1. Abra uma linha de comando na raiz do aplicativo de extensão atualizado App Builder.
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

1. Efetue logon no [Console do Adobe Developer](https://developer.adobe.com)
1. Selecionar __Console__
1. Navegue até __Projetos__
1. Selecione o projeto associado à extensão
1. Selecione o __Produção__ espaço de trabalho
1. Selecionar __Enviar para aprovação__
1. Preencha e envie o formulário, atualizando os campos conforme necessário.

### Aprovação de implantação

![Extensão da aprovação](./assets/deploy/adobe-exchange.png){align="center"}

1. Efetue logon no [Adobe Exchange](https://exchange.adobe.com/)
1. Navegue até __Gerenciar__ > __Aplicativos com revisão pendente__
1. __Revisão__ o aplicativo App Builder da extensão
1. Se as alterações na extensão forem aceitáveis __Aceitar__ a revisão. Isso injeta imediatamente a extensão em todos os serviços de autor as a Cloud Service AEM na Organização Adobe.

Depois que a solicitação de extensão é aprovada, a extensão fica ativa imediatamente nos serviços do autor as a Cloud Service do AEM.

## Atualizar uma extensão

A atualização e a extensão do aplicativo App Builder seguem o mesmo processo que a [implantação inicial](#initial-deployment), com o desvio em que a implantação da extensão existente deve ser revogada primeiro.

### Revogar extensão

Para implantar uma nova versão de uma extensão, ela deve primeiro ser revogada (ou removida). Embora a extensão seja Revogada, ela não está disponível nos consoles AEM.

1. Efetue logon no [Adobe Exchange](https://exchange.adobe.com/)
1. Navegue até __Gerenciar__ > __Aplicativos do App Builder__
1. __Revogar__ a extensão a ser atualizada

### Implantar a extensão

Implante a extensão no espaço de trabalho Produção. As extensões implantadas no espaço de trabalho de Produção são adicionadas automaticamente a todos os serviços de autor as a Cloud Service do AEM na Adobe Org em que a extensão é implantada.

1. Abra uma linha de comando na raiz do aplicativo de extensão atualizado App Builder.
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

1. Efetue logon no [Console do Adobe Developer](https://developer.adobe.com)
1. Selecionar __Console__
1. Navegue até __Projetos__
1. Selecione o projeto associado à extensão
1. Selecione o __Produção__ espaço de trabalho
1. Selecionar __Enviar para aprovação__
1. Preencha e envie o formulário, atualizando os campos conforme necessário.

#### Aprovar a solicitação de implantação

![Extensão da aprovação](./assets/deploy/adobe-exchange.png){align="center"}

1. Efetue logon no [Adobe Exchange](https://exchange.adobe.com/)
1. Navegue até __Gerenciar__ > __Aplicativos com revisão pendente__
1. __Revisão__ o aplicativo App Builder da extensão
1. Se as alterações na extensão forem aceitáveis __Aceitar__ a revisão. Isso injeta imediatamente a extensão em todos os serviços de autor as a Cloud Service AEM na Organização Adobe.

Depois que a solicitação de extensão é aprovada, a extensão fica ativa imediatamente nos serviços do autor as a Cloud Service do AEM.

## Remover uma extensão

![Remover uma extensão](./assets/deploy/revoke.png)

Para remover uma extensão, cancele (ou remova) a extensão do Adobe Exchange. Quando a extensão é revogada, ela é removida de todos os serviços do autor as a Cloud Service AEM.

1. Efetue logon no [Adobe Exchange](https://exchange.adobe.com/)
1. Navegue até __Gerenciar__ > __Aplicativos do App Builder__
1. __Revogar__ a extensão a ser removida
