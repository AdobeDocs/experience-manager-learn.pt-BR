---
title: Configurar o App Builder para extensibilidade do Asset compute
description: Os projetos do Asset compute são projetos do App Builder especialmente definidos e, como tal, exigem acesso ao App Builder no Adobe Developer Console para configurá-los e implantá-los.
feature: Asset Compute Microservices
topics: renditions, development
version: Cloud Service
activity: develop
audience: developer
doc-type: tutorial
kt: 6268
thumbnail: 40183.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 2b1d8786-592e-41f2-80cc-bc0b1c7e1b49
source-git-commit: eb6a7ef343a43000855f8d5cc69bde0fae81d3e6
workflow-type: tm+mt
source-wordcount: '539'
ht-degree: 0%

---

# Configurar o App Builder

Os projetos do Asset compute são projetos do App Builder especialmente definidos e, como tal, exigem acesso ao App Builder no Adobe Developer Console para configurá-los e implantá-los.

## Criar e configurar o App Builder no Console do desenvolvedor do Adobe{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183/?quality=12&learn=on)

_Click-through da configuração do App Builder (sem áudio)_

1. Faça logon em [Console do desenvolvedor do Adobe](https://console.adobe.io) usando a Adobe ID associada ao [contas e serviços](./accounts-and-services.md). Certifique-se de que você é uma __Administrador do sistema__ ou na __Função do desenvolvedor__ para a Org Adobe correta.
1. Crie um projeto do App Builder ao tocar em __Criar novo projeto > Projeto a partir de modelo > App Builder__

   _Se__ Criar novo projeto __ou o botão__ App Builder __O tipo não está disponível, isso significa que a sua Org de Adobe não está disponível [provisionado com o App Builder](#request-adobe-project-app-builder)._

   + __Título do projeto__: `WKND AEM Asset Compute`
   + __Nome do aplicativo__: `wkndAemAssetCompute<YourName>`
      + O __Nome do aplicativo__ deve ser exclusiva em todos os projetos do FApp Builderirefly e não pode ser modificada posteriormente. Prefixar o nome da sua empresa ou organização e fazer o postfix com um sufixo significativo é uma boa abordagem, como: `wkndAemAssetCompute`.
      + Para autoativação, geralmente é melhor colocar seu nome no __Nome do aplicativo__, como `wkndAemAssetComputeJaneDoe` para evitar colisões com outros projetos do App Builder.
   + Em __Áreas de trabalho__ adicione um novo ambiente chamado `Development`
   + Em __Adobe I/O Runtime__ garantir __Incluir tempo de execução a cada espaço de trabalho__ está selecionado
   + Toque __Salvar__ para salvar o projeto
1. No projeto do App Builder, selecione `Development` no seletor de espaço de trabalho
1. Toque __+ Adicionar serviço > API__ para abrir o __Adicionar uma API__ use essa abordagem para adicionar as seguintes APIs:

   + __Experience Cloud > Asset compute__
      + Selecionar __Gerar um par de chaves__ e toque no __Gerar par de chaves__ e salve o arquivo baixado `config.zip` para um local seguro para [uso posterior](#private-key)
      + Toque __Próximo__
      + Selecione o perfil do produto __Integrações - Cloud Service__ e tocar __Salvar API configurada__
   + __Serviços da Adobe > Eventos de E/S__ e tocar __Salvar API configurada__
   + __Serviços da Adobe > API de gerenciamento de E/S__ e tocar __Salvar API configurada__

## Acessar o private.key{#private-key}

Ao configurar o [Integração da API do Asset compute](#set-up) um novo par de chaves foi gerado e um `config.zip` foi baixado automaticamente. Essa `config.zip` contém o certificado público gerado e a correspondência `private.key` arquivo.

1. Descompactar `config.zip` para um local seguro em seu sistema de arquivos como `private.key` é [usado posteriormente](../develop/environment-variables.md)
   + Segredos e chaves privadas nunca devem ser adicionados ao Git como questão de segurança.

## Revise as credenciais da conta de serviço (JWT)

As credenciais deste projeto do Adobe I/O são usadas pelo [Ferramenta de desenvolvimento de assets compute](../develop/development-tool.md) para interagir com o Adobe I/O Runtime e precisará ser incorporado ao projeto do Asset compute. Familiarize-se com as credenciais da conta de serviço (JWT).

![Credenciais da conta do serviço de desenvolvedor do Adobe](./assets/app-builder/service-account.png)

1. No projeto do Adobe I/O Project App Builder, verifique se `Development` espaço de trabalho selecionado
1. Toque em __Conta de serviço (JWT)__ under __Credenciais__
1. Revise as credenciais do Adobe I/O exibidas
   + O __chave pública__ listado na parte inferior tem __private.key__ contrapartida na `config.zip` baixado quando a variável __API do Asset compute__ foi adicionado a este projeto.
      + Se a chave privada for perdida ou comprometida, a chave pública correspondente poderá ser removida e um novo par de chaves será gerado ou carregado no Adobe I/O usando essa interface.
