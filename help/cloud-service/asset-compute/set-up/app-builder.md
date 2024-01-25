---
title: Configurar o App Builder para extensibilidade do Asset compute
description: Os projetos do Asset compute são projetos do App Builder especialmente definidos e, como tal, exigem acesso ao App Builder no Adobe Developer Console para configurá-los e implantá-los.
feature: Asset Compute Microservices
version: Cloud Service
doc-type: Tutorial
jira: KT-6268
thumbnail: 40183.jpg
topic: Integrations, Development
role: Developer
level: Intermediate, Experienced
exl-id: 2b1d8786-592e-41f2-80cc-bc0b1c7e1b49
duration: 216
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '538'
ht-degree: 0%

---

# Configurar o Construtor de aplicativos

Os projetos do Asset compute são projetos do App Builder especialmente definidos e, como tal, exigem acesso ao App Builder no Adobe Developer Console para configurá-los e implantá-los.

## Criar e configurar o App Builder no Adobe Developer Console{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183?quality=12&learn=on)

_Click-through da configuração do App Builder (sem áudio)_

1. Efetue logon no [Console do Adobe Developer](https://console.adobe.io) usando a Adobe ID associada ao provisionado [contas e serviços](./accounts-and-services.md). Verifique se você é um __Administrador do sistema__ ou no __Função do desenvolvedor__ para obter a organização de Adobe correta.
1. Crie um projeto do App Builder tocando __Criar novo projeto > Projeto a partir de modelo > Construtor de aplicativos__

   _Se um__ Criar novo projeto __ou o botão__ Construtor de aplicativos __tipo não estiver disponível, significa que a sua Organização Adobe não está [provisionado com o App Builder](#request-adobe-project-app-builder)._

   + __Título do projeto__: `WKND AEM Asset Compute`
   + __Nome do aplicativo__: `wkndAemAssetCompute<YourName>`
      + A variável __Nome do aplicativo__ deve ser exclusivo em todos os projetos do FApp Builderirefly e não pode ser modificado posteriormente. Prefixar o nome da sua empresa ou organização e postfixar com um sufixo significativo é uma boa abordagem, como: `wkndAemAssetCompute`.
      + Para autoativação, geralmente é melhor anexar seu nome à tag __Nome do aplicativo__, como `wkndAemAssetComputeJaneDoe` para evitar colisões com outros projetos do App Builder.
   + Em __Espaços de trabalho__ adicione um novo ambiente chamado `Development`
   + Em __Adobe I/O Runtime__ assegurar __Incluir tempo de execução a cada espaço de trabalho__ está selecionado
   + Toque __Salvar__ para salvar o projeto
1. No projeto do App Builder, selecione `Development` no seletor de espaço de trabalho
1. Toque __+ Adicionar serviço > API__ para abrir o __Adicionar uma API__ use essa abordagem para adicionar as seguintes APIs:

   + __EXPERIENCE CLOUD > ASSET COMPUTE__
      + Selecionar __Gerar um par de chaves__ e toque na guia __Gerar par de chaves__ e salve o arquivo baixado `config.zip` para um local seguro para [uso posterior](#private-key)
      + Toque __Próxima__
      + Selecione o perfil de produto __Integrações - Cloud Service__ e toque em __Salvar API configurada__
   + __Serviços da Adobe > Eventos de E/S__ e toque em __Salvar API configurada__
   + __Serviços da Adobe > API de gerenciamento de E/S__ e toque em __Salvar API configurada__

## Acessar a chave privada{#private-key}

Ao configurar o [Integração da API do Asset compute](#set-up) um novo par de chaves foi gerado e um `config.zip` o arquivo foi baixado automaticamente. Este `config.zip` contém o certificado público gerado e o certificado correspondente `private.key` arquivo.

1. Descompactar `config.zip` para um local seguro em seu sistema de arquivos como o `private.key` é [usado mais tarde](../develop/environment-variables.md)
   + Segredos e chaves privadas nunca devem ser adicionados ao Git por uma questão de segurança.

## Revisar as credenciais da conta de serviço (JWT)

As credenciais deste projeto Adobe I/O são usadas pelo local [Ferramenta de desenvolvimento de assets compute](../develop/development-tool.md) para interagir com o Adobe I/O Runtime, e precisarão ser incorporados ao projeto do Asset compute. Familiarize-se com as credenciais da Conta de serviço (JWT).

![Credenciais da conta de serviço do Adobe Developer](./assets/app-builder/service-account.png)

1. No projeto Adobe I/O Project App Builder, verifique se `Development` o espaço de trabalho está selecionado
1. Toque em __Conta de serviço (JWT)__ em __Credenciais__
1. Revise as credenciais de Adobe I/O exibidas
   + A variável __chave pública__ listado na parte inferior tem __chave.privada__ contrapartida na `config.zip` baixado quando a variável __API do Asset compute__ foi adicionado a este projeto.
      + Se a chave privada for perdida ou comprometida, a chave pública correspondente poderá ser removida e um novo par de chaves poderá ser gerado no ou carregado no Adobe I/O usando essa interface.
