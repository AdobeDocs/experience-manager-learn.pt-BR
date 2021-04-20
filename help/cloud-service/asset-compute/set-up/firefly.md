---
title: Configurar o Adobe Project Firefly para extensibilidade do Asset Compute
description: Os projetos do Asset Compute são projetos do Adobe Project Firefly especialmente definidos e, como tal, exigem acesso ao Adobe Project Firefly no Developer Console para configurá-los e implantá-los.
feature: Asset Compute Microservices
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6268
thumbnail: 40183.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '553'
ht-degree: 0%

---


# Configurar o Adobe Project Firefly

Os projetos do Asset Compute são projetos do Adobe Project Firefly especialmente definidos e, como tal, exigem acesso ao Adobe Project Firefly no Developer Console para configurá-los e implantá-los.

## Criar e configurar o Adobe Project Firefly no Console do desenvolvedor{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183/?quality=12&learn=on)

_Click-through da configuração do Adobe Project Firefly (Sem áudio)_

1. Faça logon em [Console do desenvolvedor da Adobe](https://console.adobe.io) usando a Adobe ID associada às contas e serviços provisionados [e ](./accounts-and-services.md). Certifique-se de que você é um __Administrador do sistema__ ou está na __Função do desenvolvedor__ para a Adobe Org correta.
1. Crie um projeto do Firefly tocando em __Criar novo projeto > Projeto a partir de modelo > Projeto do Firefly__

   _Se o botão__ Criar novo __projeto ou o__ Project __Fireflytype não estiver disponível, isso significa que sua Adobe Org não foi  [provisionada com o Project Firefly](#request-adobe-project-firefly)._

   + __Título__ do projeto:  `WKND AEM Asset Compute`
   + __Nome__ do aplicativo:  `wkndAemAssetCompute<YourName>`
      + O __Nome do aplicativo__ deve ser exclusivo em todos os projetos do Firefly e não pode ser modificado posteriormente. Prefixar o nome da sua empresa ou organização e fazer o postfix com um sufixo significativo é uma boa abordagem, como: `wkndAemAssetCompute`.
      + Para autoativação, geralmente é melhor colocar seu nome no __Nome do aplicativo__, como `wkndAemAssetComputeJaneDoe` para evitar colisões com outros projetos do Project Firefly.
   + Em __Espaços de trabalho__ adicione um novo ambiente chamado `Development`
   + Em __Adobe I/O Runtime__, verifique se __Incluir Runtime com cada espaço de trabalho__ está selecionado
   + Toque em __Salvar__ para salvar o projeto
1. No projeto Adobe Firefly, selecione `Development` no seletor de espaço de trabalho
1. Toque em __+ Adicionar serviço > API__ para abrir o assistente __Adicionar uma API__, use esta abordagem para adicionar as seguintes APIs:

   + __Experience Cloud > Asset Compute__
      + Selecione __Generate a key pair__ e toque no botão __Generate keypair__ e salve o `config.zip` baixado em um local seguro para [later use](#private-key)
      + Toque em __Próximo__
      + Selecione o perfil do produto __Integrações - Cloud Service__ e toque em __Salvar API configurada__
   + __Serviços da Adobe >__ Eventos de E/S e toque em  __Salvar API configurada__
   + __Serviços da Adobe >__ API de gerenciamento de E/S e toque em  __Salvar API configurada__

## Acesse a private.key{#private-key}

Ao configurar a [integração da API do Asset Compute](#set-up), um novo par de chaves foi gerado e um arquivo `config.zip` foi baixado automaticamente. Este `config.zip` contém o certificado público gerado e o arquivo `private.key` correspondente.

1. Descompacte `config.zip` em um local seguro no sistema de arquivos, pois `private.key` é [usado posteriormente](../develop/environment-variables.md)
   + Segredos e chaves privadas nunca devem ser adicionados ao Git como questão de segurança.

## Revise as credenciais da conta de serviço (JWT)

As credenciais do projeto do Adobe I/O são usadas pela [Ferramenta de desenvolvimento do Asset Compute](../develop/development-tool.md) local para interagir com o Adobe I/O Runtime e precisarão ser incorporadas ao projeto do Asset Compute. Familiarize-se com as credenciais da conta de serviço (JWT).

![Credenciais da conta do serviço de desenvolvedor da Adobe](./assets/firefly/service-account.png)

1. No projeto Adobe I/O Project Firefly, certifique-se de que o espaço de trabalho `Development` esteja selecionado
1. Toque em __Conta de Serviço (JWT)__ em __Credenciais__
1. Revise as credenciais do Adobe I/O exibidas
   + A __chave pública__ listada na parte inferior tem a contrapartida __private.key__ no `config.zip` baixado quando a __API do Asset Compute__ foi adicionada a este projeto.
      + Se a chave privada for perdida ou comprometida, a chave pública correspondente poderá ser removida e um novo par de chaves será gerado ou carregado no Adobe I/O usando essa interface.
