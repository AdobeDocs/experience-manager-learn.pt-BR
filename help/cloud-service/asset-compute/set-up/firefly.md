---
title: Configurar o Adobe Project Firefly para extensibilidade de Asset compute
description: Os projetos de asset compute são projetos do Adobe Project Firefly especialmente definidos e, como tal, exigem acesso ao Adobe Project Firefly no Adobe Developer Console para configurá-los e implantá-los.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6268
thumbnail: 40183.jpg
translation-type: tm+mt
source-git-commit: 3a3832a05ed9598d970915adbc163254c6eb83f1
workflow-type: tm+mt
source-wordcount: '545'
ht-degree: 0%

---


# Configurar o Adobe Project Firefly

Os projetos de asset compute são projetos do Adobe Project Firefly especialmente definidos e, como tal, exigem acesso ao Adobe Project Firefly no Adobe Developer Console para configurá-los e implantá-los.

## Criar e configurar o Adobe Project Firefly no Adobe Developer Console{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183/?quality=12&learn=on)

_Click-through de configuração do Adobe Project Firefly (Sem áudio)_

1. Faça logon em [Adobe Developer Console](https://console.adobe.io) usando o Adobe ID associado às contas e serviços ](./accounts-and-services.md) provisionados. [ Certifique-se de ser um __Administrador do sistema__ ou __Função do desenvolvedor__ para a Organização de Adobe correta.
1. Crie um projeto Firefly tocando __Criar novo projeto > Projeto de modelo > Projeto Firefly__

   _Se o botão__ Criar novo __projeto ou o__ Project __Fireflytype não estiver disponível, isso significa que sua Organização de Adobe não foi  [provisionada com o Project Firefly](#request-adobe-project-firefly)._

   + __Título__ do projeto:  `WKND AEM Asset Compute`
   + __Nome__ do aplicativo:  `wkndAemAssetCompute<YourName>`
      + O __Nome do aplicativo__ deve ser exclusivo em todos os projetos do Firefly e não pode ser modificado posteriormente. Prefixar o nome da sua empresa ou organização e fazer a correção com um sufixo significativo é uma boa abordagem, como: `wkndAemAssetCompute`.
      + Para autoativação, geralmente é melhor colocar seu nome no __Nome do aplicativo__, como `wkndAemAssetComputeJaneDoe` para evitar colisões com outros projetos do Project Firefly.
   + Em __Espaços de trabalho__ adicione um novo ambiente chamado `Development`
   + Em __Adobe I/O Runtime__ verifique se __Incluir Tempo de Execução com cada espaço de trabalho__ está selecionado
   + Toque em __Salvar__ para salvar o projeto
1. No projeto Adobe Firefly, selecione `Development` no seletor de espaço de trabalho
1. Toque em __+ Adicionar serviço > API__ para abrir o assistente __Adicionar uma API__, use esta abordagem para adicionar as seguintes APIs:

   + __Experience Cloud > Asset compute__
      + Selecione __Gerar um par de teclas__ e toque no botão __Gerar um par de teclas__ e guarde o `config.zip` transferido por download para um local seguro para [utilização posterior](#private-key)
      + Toque em __Próximo__
      + Selecione o perfil de produto __Integrações - Cloud Service__ e toque em __Salvar API configurada__
   + __Serviços do Adobe >__ Eventos de E/S e toque em  __Salvar API configurada__
   + __Adobe Services >__ API de gerenciamento de E/S e toque em  __Salvar API configurada__

## Acesse private.key{#private-key}

Ao configurar a [integração da API do Asset compute](#set-up), um novo par de chaves foi gerado e um arquivo `config.zip` foi baixado automaticamente. Este `config.zip` contém o certificado público gerado e o arquivo `private.key` correspondente.

1. Descompacte `config.zip` em um local seguro no seu sistema de arquivos, pois `private.key` é [usado posteriormente](../develop/environment-variables.md)
   + Segredos e chaves privadas nunca devem ser adicionados ao Git como uma questão de segurança.

## Revise as credenciais da conta de serviço (JWT)

As credenciais deste projeto da Adobe I/O são usadas pela [Ferramenta de Desenvolvimento de Asset computes](../develop/development-tool.md) local para interagir com a Adobe I/O Runtime e precisarão ser incorporadas ao projeto do Asset compute. Familiarize-se com as credenciais da conta de serviço (JWT).

![Credenciais da Conta de Serviço de Desenvolvedor do Adobe](./assets/firefly/service-account.png)

1. No projeto Firefly do Adobe I/O Project, verifique se a área de trabalho `Development` está selecionada
1. Toque em __Conta de Serviço (JWT)__ em __Credenciais__
1. Revise as credenciais do Adobe I/O exibidas
   + A __chave pública__ listada na parte inferior tem a contrapartida __private.key__ no `config.zip` baixado quando a __API do Asset compute__ foi adicionada a este projeto.
      + Se a chave privada for perdida ou comprometida, a chave pública correspondente poderá ser removida e um novo par de chaves será gerado ou carregado no Adobe I/O usando essa interface.
