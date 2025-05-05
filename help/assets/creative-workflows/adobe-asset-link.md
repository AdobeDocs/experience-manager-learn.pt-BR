---
title: Adobe Asset Link e AEM
description: Os ativos do Adobe Experience Manager podem ser usados por designers e usuários criativos nos aplicativos de desktop favoritos da Adobe Creative Cloud. A extensão Adobe Asset Link para a Adobe Creative Cloud para corporações estende a capacidade de pesquisar e navegar, classificar, visualizar, carregar ativos, fazer check-out, modificar, fazer check-in e exibir metadados de ativos da AEM em ferramentas do Creative Cloud, como Adobe XD, Photoshop, InDesign e Illustrator.
feature: Adobe Asset Link
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Content Management
role: User
level: Beginner
thumbnail: 28988.jpg
jira: KT-8413, KT-3707
last-substantial-update: 2022-06-25T00:00:00Z
doc-type: Feature Video
exl-id: 6c49f8c2-f468-4b29-b7b6-029c8ab39ce9
duration: 1027
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1039'
ht-degree: 1%

---

# Adobe Asset Link 3.0

Os ativos do Adobe Experience Manager podem ser usados por designers e usuários criativos nos aplicativos de desktop favoritos da Adobe Creative Cloud.

A extensão Adobe Asset Link para a Adobe Creative Cloud para corporações estende a capacidade de pesquisar e navegar, classificar, visualizar, carregar ativos, fazer check-out, modificar, fazer check-in e exibir metadados de ativos da AEM nos aplicativos da Creative Cloud.

>[!TIP]
>
> Saiba mais sobre como o [Programa de treinamento Adobe XD Premium](https://helpx.adobe.com/br/support/xd.html) pode ajudar você a integrar o Asset Link ao seu fluxo de trabalho do Adobe Experience Manager.

## Fluxos de trabalho criativos do Adobe Asset Link e AEM

O vídeo a seguir ilustra um fluxo de trabalho comum usado por criadores que trabalham em aplicativos do Adobe Creative Cloud e que integram diretamente ao AEM usando o Adobe Asset Link.

>[!VIDEO](https://video.tv.adobe.com/v/335927?quality=12&learn=on)

## Recursos do Adobe Asset Link

+ O Adobe Asset Link integra-se ao AEM Assets e ao Assets Essentials.
+ O Adobe Asset Link configura automaticamente a conexão com ambientes AEM baseados em nuvem (AEM Assets as a Cloud Service e Assets Essentials)
+ O Adobe Asset Link é uma extensão que funciona em aplicativos da Adobe Creative Cloud:

   + Adobe XD
   + Adobe Photoshop
   + Adobe Illustrator
   + Adobe InDesign

+ Autenticação automática para o AEM usando o Adobe Enterprise ID ou Federated ID
+ Procurar e pesquisar ativos digitais no AEM
+ Acesse os detalhes do arquivo para ativos que residem no AEM pelo painel:
   + Miniatura 
   + Metadados básicos
   + Versões
+ Inserir, baixar ou arrastar e soltar ativos no layout
+ Modifique ativos fazendo check-out deles da AEM e trabalhando neles (WIP) em sua conta do Creative Cloud Assets
+ Fazer check-in de um ativo de volta no AEM depois que ele terminar de modificá-lo e a nova versão for refletida no AEM
+ Pesquisar ativos no AEM pelo painel No aplicativo do Adobe Asset Link
+ Procurar coleções do AEM Assets e coleções inteligentes diretamente no painel Link de ativos
+ Adicionar ativos recém-criados à AEM diretamente do painel
+ Arraste e solte ativos diretamente em quadros do InDesign

## Inserção de ativos no InDesign

O Adobe Asset Link fornece suporte à vinculação direta da InDesign entre o Adobe Asset Link e o AEM. Com o suporte para vinculação direta do InDesign, você pode colocar (__Colocar vinculados__ ou __Colocar cópia__) ou arrastar e soltar ativos digitais no InDesign a partir do AEM por meio do painel Link de ativos do Adobe. Além disso, introduz a representação *Somente para posicionamento+ (FPO).

>[!VIDEO](https://video.tv.adobe.com/v/37236?quality=12&learn=on&captions=por_br)

>[!NOTE]
>
>Use somente o Adobe Creative Cloud Enterprise ID ou o Federated ID. Certifique-se de [configurar o AEM para o Adobe Asset Link](https://helpx.adobe.com/br/enterprise/using/adobe-asset-link.html).

Você pode colocar um ativo no layout do InDesign usando uma das opções abaixo:

+ **Inserir cópia** - A incorporação de um ativo (usando a opção Inserir cópia) insere uma cópia do ativo original no layout do InDesign depois de baixar os binários no sistema local. O Adobe Asset Link não mantém nenhum vínculo entre a cópia incorporada e o ativo original. Se o ativo original for modificado no AEM, você deverá excluir o ativo incorporado do arquivo do InDesign e incorporar novamente o ativo do AEM.

+ **Inserir vinculado** - Ao trabalhar com documentos do InDesign, você tem a opção de fazer referência aos ativos do AEM, além de incorporar diretamente os ativos (usando a opção Inserir cópia no menu de contexto). A referência a ativos permite colaborar com outros usuários e incorporar todas as atualizações feitas no ativo original no AEM. Para referenciar um ativo do AEM, use a opção Inserir vinculado no menu de contexto.

### Imagens somente para posicionamento

Quando arquivos de ativos grandes são colocados em Documentos do InDesign pela AEM usando o Adobe Asset Link, os usuários de criação precisam aguardar alguns segundos após iniciar a operação de inserção. Isso afeta a experiência geral do usuário. Com o Adobe Asset Link, você pode colocar temporariamente uma imagem de baixa resolução do ativo original do AEM, reduzindo assim o tempo gasto para inserir uma imagem. Ao mesmo tempo, aumenta a experiência geral do usuário e a produtividade. A imagem de resolução mais baixa é colocada temporariamente e, quando a saída final for necessária para impressão ou publicação, será necessário substituir as representações FPO pelos originais. Se quiser substituir várias imagens FPO pelas respectivas imagens originais, navegue até o painel **_Windows > Links_** e baixe os ativos originais. Depois que as imagens originais forem baixadas, escolha Substituir todas as FPOs por originais.

As representações FPO são substitutos leves dos ativos originais. Elas têm a mesma proporção, mas são de tamanho menor em comparação às imagens originais. Atualmente, o InDesign oferece suporte à importação de representações FPO somente para os seguintes tipos de imagem:

+ JPEG
+ GIF
+ PNG
+ TIFF
+ PSD
+ BMP

Se uma representação FPO não estiver disponível para um ativo específico no AEM, o ativo original de alta resolução será referenciado. Para imagens FPO, o status FPO é exibido no painel Links do InDesign.

## Autenticação do Adobe Asset Link com o AEM Assets

Como a autenticação do Adobe Asset Link funciona no contexto do Adobe Identity Management Services (IMS) e do Adobe Experience Manager Author.

![Arquitetura do Adobe Asset Link](assets/adobe-asset-link-article-understand.png)

1. A extensão do Adobe Asset Link faz uma solicitação de autorização, por meio do aplicativo de desktop da Adobe Creative Cloud, para o Adobe Identity Manager Service (IMS) e, após ser bem-sucedida, recebe um token de portador.
1. A extensão do Adobe Asset Link se conecta ao AEM Author por HTTP(S), incluindo o token do Portador obtido na **Etapa 1**, usando o esquema (HTTP/HTTPS), o host e a porta fornecidos nas configurações JSON da extensão.
1. O Manipulador de autenticação de portador do AEM extrai o token do portador da solicitação e o valida com o Adobe IMS.
1. Depois que o Adobe IMS valida o token do portador, um usuário é criado no AEM (se ele ainda não existir) e sincroniza dados de perfil e de grupo/associações do Adobe IMS. O usuário do AEM recebe um token de logon padrão do AEM, que é enviado de volta para a extensão do Adobe Asset Link como um Cookie na resposta HTTP(S).
1. Interações subsequentes (ou seja, navegar, pesquisar, fazer check-in/check-out de ativos etc.) com a extensão do Adobe Asset Link resulta em solicitações HTTP(S) para o AEM Author, que são validadas usando o token de logon do AEM, usando o Manipulador de autenticação de token do AEM padrão.

>[!NOTE]
>
>Após a expiração do token de logon, as **Etapas 1-5** invocarão automaticamente, autenticando a extensão do Adobe Asset Link usando o token de Portador, e emitirão novamente um novo token de logon válido.

## Recursos adicionais

+ [Site do Adobe Asset Link](https://www.adobe.com/br/creativecloud/business/enterprise/adobe-asset-link.html)
