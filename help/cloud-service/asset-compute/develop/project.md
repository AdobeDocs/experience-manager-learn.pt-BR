---
title: Criar um projeto do Asset compute para extensibilidade do Asset compute
description: Os projetos do Asset compute são projetos Node.js, gerados usando a Adobe I/O CLI, que seguem uma estrutura específica, permitindo que eles sejam implantados no Adobe I/O Runtime e integrados ao AEM como um Cloud Service.
kt: 6269
thumbnail: 40197.jpg
topic: Integrations, Development
feature: Asset Compute Microservices
role: Developer
level: Intermediate, Experienced
source-git-commit: ac93d6ba636e64ba6d8bbdb0840810b8f47a25c8
workflow-type: tm+mt
source-wordcount: '896'
ht-degree: 1%

---


# Criar um projeto do Asset compute

Os projetos do Asset compute são projetos Node.js, gerados usando a Adobe I/O CLI, que seguem uma estrutura específica que permite a implantação no Adobe I/O Runtime e a integração do AEM como Cloud Service. Um único projeto do Asset compute pode conter um ou mais trabalhadores do Asset compute, cada um com um ponto final HTTP distinto referenciável de um AEM como um Perfil de processamento do Cloud Service.

## Gerar um projeto

>[!VIDEO](https://video.tv.adobe.com/v/40197/?quality=12&learn=on)

_Click-through da geração de um projeto de Asset compute (Sem áudio)_

Use o plug-in Asset compute [Adobe I/O CLI](../set-up/development-environment.md#aio-cli) para gerar um novo projeto de Asset compute vazio.

1. Na linha de comando, navegue até a pasta para conter o projeto.
1. Na linha de comando, execute `aio app init` para iniciar a CLI de geração de projeto interativa.
   + Esse comando pode gerar um navegador da Web solicitando a autenticação para o Adobe I/O. Se isso acontecer, forneça suas credenciais do Adobe associadas aos [serviços e produtos da Adobe necessários](../set-up/accounts-and-services.md). Se não conseguir fazer logon, siga [estas instruções sobre como gerar um projeto](https://www.adobe.io/project-firefly/docs/getting_started/first_app/#42-developer-is-not-logged-in-as-enterprise-organization-user).
1. __Selecionar Org__
   + Selecione a Organização do Adobe que tem AEM como Cloud Service, o Project Firefly está registrado com
1. __Selecionar projeto__
   + Localize e selecione o Projeto. Este é o [Título do projeto](../set-up/firefly.md) criado a partir do modelo de projeto do Firefly, neste caso `WKND AEM Asset Compute`
1. __Selecionar espaço de trabalho__
   + Selecione o espaço de trabalho `Development`
1. __Quais recursos do Adobe I/O App você deseja ativar para este projeto? Selecionar componentes para incluir__
   + Selecionar `Actions: Deploy runtime actions`
   + Use as teclas de setas para selecionar e espaçar para desmarcar/selecionar, e Enter para confirmar a seleção
1. __Selecione o tipo de ações a serem geradas__
   + Selecionar `Adobe Asset Compute Worker`
   + Use as teclas de setas para selecionar, espace para desmarcar/selecionar e Enter para confirmar a seleção
1. __Como deseja nomear esta ação?__
   + Use o nome padrão `worker`.
   + Se seu projeto contém vários trabalhadores que executam diferentes cálculos de ativos, nomeie-os semanticamente

## Gerar console.json

A ferramenta de desenvolvedor requer um arquivo chamado `console.json` que contém as credenciais necessárias para se conectar ao Adobe I/O. Esse arquivo é baixado do console do Adobe I/O.

1. Abra o projeto [Adobe I/O](https://console.adobe.io) do trabalhador do Asset compute
1. Selecione o espaço de trabalho do projeto para baixar as credenciais `console.json`, nesse caso, selecione `Development`
1. Vá para a raiz do projeto Adobe I/O e toque em __Baixar tudo__ no canto superior direito.
1. Um arquivo é baixado como um arquivo `.json` com o prefixo do projeto e do espaço de trabalho, por exemplo: `wkndAemAssetCompute-81368-Development.json`
1. Você pode
   + Renomeie o arquivo como `config.json` e mova-o para a raiz do projeto de trabalho do Asset compute. Esta é a abordagem neste tutorial.
   + Mova-a para uma pasta arbitrária E faça referência a essa pasta do arquivo `.env` com uma entrada de configuração `ASSET_COMPUTE_INTEGRATION_FILE_PATH`. O caminho do arquivo pode ser absoluto ou relativo à raiz do seu projeto. Por exemplo:
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=/Users/example-user/secrets/wkndAemAssetCompute-81368-Development.json`

      Ou
      + `ASSET_COMPUTE_INTEGRATION_FILE_PATH=../../secrets/wkndAemAssetCompute-81368-Development.json.json`


> OBSERVAÇÃO
> O arquivo contém credenciais. Se você armazenar o arquivo em seu projeto, adicione-o ao arquivo `.gitignore` para evitar que seja compartilhado. O mesmo se aplica ao arquivo `.env` — Esses arquivos de credenciais não devem ser compartilhados ou armazenados no Git.

## Revisar a anatomia do projeto

O projeto do Asset compute gerado é um projeto Node.js para uso como um projeto especializado do Adobe Project Firefly. Os seguintes elementos estruturais são idiossincráticos para o projeto do Asset compute:

+ `/actions` contém subpastas e cada subpasta define um trabalho do Asset compute.
   + `/actions/<worker-name>/index.js` define o JavaScript usado para executar o trabalho desse trabalhador.
      + O nome da pasta `worker` é um padrão e pode ser qualquer coisa, desde que esteja registrado no `manifest.yml`.
      + Mais de uma pasta de trabalho pode ser definida em `/actions` conforme necessário, no entanto, ela deve ser registrada no `manifest.yml`.
+ `/test/asset-compute` contém os conjuntos de teste para cada trabalhador. Semelhante à pasta `/actions`, `/test/asset-compute` pode conter várias subpastas, cada uma correspondente ao trabalhador testado.
   + `/test/asset-compute/worker`, representando um conjunto de testes para um trabalhador específico, contém subpastas que representam um caso de teste específico, juntamente com a entrada de teste, os parâmetros e a saída esperada.
+ `/build` contém a saída, os logs e os artefatos das execuções de caso de teste do Asset compute.
+ `/manifest.yml` define quais trabalhadores do Asset compute o projeto fornece. Cada implementação do trabalhador deve ser enumerada neste arquivo para torná-las disponíveis para o AEM como um Cloud Service.
+ `/console.json` define configurações do Adobe I/O
   + Esse arquivo pode ser gerado/atualizado usando o comando `aio app use`.
+ `/.aio` contém configurações usadas pela ferramenta aio CLI.
   + Esse arquivo pode ser gerado/atualizado usando o comando `aio app use`.
+ `/.env` O define variáveis de ambiente em uma  `key=value` sintaxe e contém segredos que não devem ser compartilhados. Para proteger esses segredos, esse arquivo NÃO deve ser verificado no Git e é ignorado por meio do arquivo `.gitignore` padrão do projeto.
   + Esse arquivo pode ser gerado/atualizado usando o comando `aio app use`.
   + As variáveis definidas neste arquivo podem ser substituídas por [exportando variáveis](../deploy/runtime.md) na linha de comando.

Para obter mais detalhes sobre a revisão da estrutura do projeto, consulte a [Anatomia de um projeto do Adobe Firefly](https://www.adobe.io/project-firefly/docs/guides/).

A maior parte do desenvolvimento ocorre na pasta `/actions` que desenvolve implementações de funcionários e em `/test/asset-compute` gravando testes para os trabalhadores personalizados do Asset compute.

## Projeto do Asset compute no GitHub

O projeto final do Asset compute está disponível no GitHub em:

+ [aem-guides-wknd-asset-compute](https://github.com/adobe/aem-guides-wknd-asset-compute)

_O GitHub contém o estado final do projeto, totalmente preenchido com os casos de trabalho e teste, mas não contém credenciais, ou seja,  `.env`ou  `console.json`   `.aio`._

