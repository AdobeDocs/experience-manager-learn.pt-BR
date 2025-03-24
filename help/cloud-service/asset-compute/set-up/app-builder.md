---
title: Configurar a extensibilidade do App Builder para Asset Compute
description: Os projetos do Asset Compute são projetos do App Builder especialmente definidos e, como tal, exigem acesso ao App Builder na Adobe Developer Console para configurá-los e implantá-los.
feature: Asset Compute Microservices
version: Experience Manager as a Cloud Service
doc-type: Tutorial
jira: KT-6268
thumbnail: 40183.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 2b1d8786-592e-41f2-80cc-bc0b1c7e1b49
duration: 197
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '538'
ht-degree: 0%

---

# Configurar o App Builder

Os projetos do Asset Compute são projetos do App Builder especialmente definidos e, como tal, exigem acesso ao App Builder na Adobe Developer Console para configurá-los e implantá-los.

## Criar e configurar o App Builder no Adobe Developer Console{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183?quality=12&learn=on)

_Click-through da configuração do Construtor de Aplicativos (Sem áudio)_

1. Faça logon no [Adobe Developer Console](https://console.adobe.io) usando a Adobe ID associada às [contas e serviços](./accounts-and-services.md) provisionados. Verifique se você é um __Administrador do Sistema__ ou está na __Função de Desenvolvedor__ para obter a Organização da Adobe correta.
1. Crie um projeto do App Builder tocando em __Criar novo projeto > Projeto a partir de modelo > App Builder__

   _Se o botão__ Criar novo projeto __ou o tipo__ App Builder __não estiverem disponíveis, significa que a sua Organização Adobe não está [provisionada com o App Builder](#request-adobe-project-app-builder)._

   + __Título do projeto__: `WKND AEM Asset Compute`
   + __Nome do aplicativo__: `wkndAemAssetCompute<YourName>`
      + O __nome do aplicativo__ deve ser exclusivo em todos os projetos FApp Builderirefly e não pode ser modificado posteriormente. Prefixar o nome da sua empresa ou organização e postfixar com um sufixo significativo é uma boa abordagem, como: `wkndAemAssetCompute`.
      + Para autoativação, geralmente é melhor anexar seu nome ao __nome do aplicativo__, como `wkndAemAssetComputeJaneDoe` para evitar colisões com outros projetos da App Builder.
   + Em __Workspaces__, adicione um novo ambiente chamado `Development`
   + Em __Adobe I/O Runtime__, verifique se __Incluir Tempo de Execução com cada espaço de trabalho__ está selecionado
   + Toque em __Salvar__ para salvar o projeto
1. No projeto do App Builder, selecione `Development` no seletor de espaço de trabalho
1. Toque em __+ Adicionar serviço > API__ para abrir o assistente __Adicionar uma API__. Use esta abordagem para adicionar as seguintes APIs:

   + __Experience Cloud > Asset Compute__
      + Selecione __Gerar um par de chaves__, toque no botão __Gerar par de chaves__ e salve o `config.zip` baixado em um local seguro para [uso posterior](#private-key)
      + Toque em __Avançar__
      + Selecione as __Integrações do perfil de produto - Cloud Service__ e toque em __Salvar API configurada__
   + __Serviços da Adobe > Eventos de E/S__ e toque em __Salvar API configurada__
   + __Adobe Services > API de gerenciamento de E/S__ e toque em __Salvar API configurada__

## Acessar a chave privada{#private-key}

Ao configurar a [integração de API do Asset Compute](#set-up), um novo par de chaves foi gerado e um arquivo `config.zip` foi baixado automaticamente. Este `config.zip` contém o certificado público gerado e o arquivo `private.key` correspondente.

1. Descompacte `config.zip` em um local seguro no seu sistema de arquivos, pois `private.key` foi [usado mais tarde](../develop/environment-variables.md)
   + Segredos e chaves privadas nunca devem ser adicionados ao Git por uma questão de segurança.

## Revisar as credenciais da conta de serviço (JWT)

As credenciais deste projeto do Adobe I/O são usadas pela [Ferramenta de Desenvolvimento do Asset Compute](../develop/development-tool.md) local para interagir com o Adobe I/O Runtime e precisarão ser incorporadas ao projeto do Asset Compute. Familiarize-se com as credenciais da Conta de serviço (JWT).

![Credenciais da Conta de Serviço do Adobe Developer](./assets/app-builder/service-account.png)

1. No projeto Adobe I/O Project App Builder, verifique se o espaço de trabalho `Development` está selecionado
1. Toque em __Conta de Serviço (JWT)__ em __Credenciais__
1. Revise as Credenciais do Adobe I/O exibidas
   + A __chave pública__ listada na parte inferior tem sua contraparte __chave privada__ no `config.zip` baixado quando a __API do Asset Compute__ foi adicionada a este projeto.
      + Se a chave privada for perdida ou comprometida, a chave pública correspondente poderá ser removida, e um novo par de chaves poderá ser gerado no Adobe I/O ou carregado por meio dessa interface.
