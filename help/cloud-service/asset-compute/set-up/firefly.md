---
title: Configurar o Adobe Project Firefly para a extensibilidade da Computação de ativos
description: Os projetos de Computação de ativos são projetos Adobe Project Firefly especialmente definidos e, como tal, exigem acesso ao Adobe Project Firefly no Adobe Developer Console para configurá-los e implantá-los.
feature: asset-compute
topics: renditions, development
version: cloud-service
activity: develop
audience: developer
doc-type: tutorial
kt: 6268
thumbnail: 40183.jpg
translation-type: tm+mt
source-git-commit: af610f338be4878999e0e9812f1d2a57065d1829
workflow-type: tm+mt
source-wordcount: '545'
ht-degree: 0%

---


# Configurar o Adobe Project Firefly

Os projetos de Computação de ativos são projetos Adobe Project Firefly especialmente definidos e, como tal, exigem acesso ao Adobe Project Firefly no Adobe Developer Console para configurá-los e implantá-los.

## Criar e configurar o Adobe Project Firefly no Adobe Developer Console{#set-up}

>[!VIDEO](https://video.tv.adobe.com/v/40183/?quality=12&learn=on)
_Click-through de configuração do Adobe Project Firefly (Sem áudio)_

1. Faça logon no [Adobe Developer Console](https://console.adobe.io) usando a Adobe ID associada às [contas e serviços](./accounts-and-services.md)provisionados. Certifique-se de ser um administrador __do__ sistema ou estar na função __de__ desenvolvedor para a organização Adobe correta.
1. Crie um projeto do Firefly tocando em __Criar novo projeto > Projeto de modelo > Projeto do Firefly__

   _Se o botão__ Criar novo projeto __ou o tipo__ Projeto Firefly __não estiver disponível, isso significa que sua Organização Adobe não foi[provisionada com o Project Firefly](#request-adobe-project-firefly)._

   + __Título__ do projeto: `WKND AEM Asset Compute`
   + __Nome__ do aplicativo: `wkndAemAssetCompute<YourName>`
      + O nome __do__ aplicativo deve ser exclusivo em todos os projetos do Firefly e não pode ser modificado posteriormente. Prefixar o nome da sua empresa ou organização e fazer a correção com um sufixo significativo é uma boa abordagem, como: `wkndAemAssetCompute`.
      + Para autoativação, geralmente é melhor colocar seu nome no nome __do__ aplicativo, como `wkndAemAssetComputeJaneDoe` evitar colisões com outros projetos do Project Firefly.
   + Em __Espaços de trabalho__ , adicione um novo ambiente chamado `Development`
   + Em __Adobe I/O Runtime__ , verifique se a opção __Incluir tempo de execução em cada espaço de trabalho__ está selecionada
   + Toque em __Salvar__ para salvar o projeto
1. No projeto Adobe Firefly, selecione `Development` do seletor de espaço de trabalho
1. Toque em __+ Adicionar serviço > API__ para abrir o assistente __Adicionar uma API__ , use esta abordagem para adicionar as seguintes APIs:

   + __Experience Cloud > Computação de ativos__
      + Selecione __Gerar um par__ de teclas e toque no botão __Gerar par__ de teclas e salve o download `config.zip` em um local seguro para uso [posterior](#private-key)
      + Toque em __Próximo__
      + Selecione Product perfil __Integrations - Cloud Service__ e toque em __Save configure API (Salvar integrações de  de produto - API configurada)__
   + __Adobe Services > Eventos__ de E/S e toque em __Salvar API configurada__
   + __Adobe Services > API__ de gerenciamento de E/S e toque em __Salvar API configurada__

## Acessar private.key{#private-key}

Ao configurar a integração [da API](#set-up) Asset Compute, um novo par de chaves foi gerado e um `config.zip` arquivo foi baixado automaticamente. Isso `config.zip` contém o certificado público gerado e o `private.key` arquivo correspondente.

1. Descompacte `config.zip` em um local seguro no sistema de arquivos, pois o arquivo `private.key` será [usado posteriormente](../develop/environment-variables.md)
   + Segredos e chaves privadas nunca devem ser adicionados ao Git como uma questão de segurança.

## Revise as credenciais da conta de serviço (JWT)

As credenciais deste projeto de E/S do Adobe são usadas pela Ferramenta [de Desenvolvimento de Computação de](../develop/development-tool.md) Ativos local para interagir com a Adobe I/O Runtime e precisarão ser incorporadas ao projeto Computação de Ativos. Familiarize-se com as credenciais da conta de serviço (JWT).

![Credenciais da Conta de Serviço de Desenvolvedor do Adobe](./assets/firefly/service-account.png)

1. No projeto Firefly de projeto de E/S de Adobe, verifique se a área de trabalho está selecionada `Development`
1. Toque em Conta __de serviço (JWT)__ em __Credenciais__
1. Revise as credenciais de E/S do Adobe exibidas
   + A chave ____ pública listada na parte inferior tem a contrapartida __private.key__ no `config.zip` baixado quando a API __Computação de__ ativos foi adicionada a este projeto.
      + Se a chave privada for perdida ou comprometida, a chave pública correspondente poderá ser removida e um novo par de chaves será gerado ou carregado para E/S do Adobe usando essa interface.
