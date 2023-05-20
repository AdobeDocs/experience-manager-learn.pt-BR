---
title: Adobe Asset Link e AEM
description: Os ativos do Adobe Experience Manager podem ser usados por designers e usuários criativos nos aplicativos de desktop favoritos da Adobe Creative Cloud. A extensão Adobe Asset Link para o Adobe Creative Cloud for enterprise estende a capacidade de pesquisar e navegar, classificar, visualizar, carregar ativos, fazer check-out, modificar, fazer check-in e exibir metadados de ativos AEM em ferramentas do Creative Cloud, como Adobe XD, Photoshop, InDesign e Illustrator.
feature: Adobe Asset Link
version: 6.4, 6.5, Cloud Service
topic: Content Management
role: User
level: Beginner
thumbnail: 28988.jpg
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '990'
ht-degree: 1%

---


# Adobe Asset Link 3.0

Os ativos do Adobe Experience Manager podem ser usados por designers e usuários criativos nos aplicativos de desktop favoritos da Adobe Creative Cloud.

A extensão Adobe Asset Link para o Adobe Creative Cloud for enterprise estende a capacidade de pesquisar e navegar, classificar, visualizar, carregar ativos, fazer check-out, modificar, fazer check-in e exibir metadados de ativos AEM em aplicativos Creative Cloud.

## Recursos do Adobe Asset Link

+ O Adobe Asset Link integra-se ao AEM Assets e ao Assets Essentials.
+ O Adobe Asset Link configura automaticamente a conexão com ambientes AEM baseados em nuvem (AEM Assets as a Cloud Service e Assets Essentials)
+ O Adobe Asset Link é uma extensão que funciona em aplicativos Adobe Creative Cloud:

   + Adobe XD
   + Adobe Photoshop
   + Adobe Illustrator
   + Adobe InDesign

+ Autenticação automática para AEM usando seu Enterprise ID ou Federated ID de Adobe
+ Procurar e pesquisar ativos digitais no AEM
+ Acesse os detalhes do arquivo para ativos que residem em AEM no painel:
   + Miniatura 
   + Metadados básicos
   + Versões
+ Inserir, baixar ou arrastar e soltar ativos no layout
+ Modifique ativos verificando-os do AEM e trabalhando neles (WIP) na conta de ativos Creative Cloud
+ Verificar novamente um ativo no AEM depois que ele terminar de modificá-lo e a nova versão refletida no AEM
+ Pesquisar por ativos no AEM no painel Adobe Asset Link no aplicativo
+ Procurar coleções do AEM Assets e coleções inteligentes diretamente no painel Link de ativos
+ Adicionar ativos recém-criados ao AEM diretamente do painel
+ Arraste e solte ativos diretamente nos quadros do InDesign

## Colocação de ativos no InDesign

O Adobe Asset Link fornece suporte à vinculação direta de InDesigns entre o Adobe Asset Link e o AEM. Com o suporte de vinculação direta do InDesign, você pode colocar (__Local vinculado__ ou __Inserir cópia__) ou arraste e solte ativos digitais no InDesign a partir do AEM por meio do painel Adobe Asset Link. Além disso, introduz a representação *Somente para posicionamento+ (FPO).

>[!VIDEO](https://video.tv.adobe.com/v/28988?quality=12&learn=on)

>[!NOTE]
>
>Use apenas o Enterprise ID ou Federated ID Adobe Creative Cloud. Verifique se você [configurar o AEM para o Adobe Asset Link](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/adobe-asset-link.ug.html).

Você pode colocar um ativo no layout do InDesign usando uma das opções abaixo:

+ **Inserir cópia** : incorporar um ativo (usando a opção Inserir cópia ) coloca uma cópia do ativo original no layout do InDesign depois de baixar os binários no sistema local. O Adobe Asset Link não mantém nenhum vínculo entre a cópia incorporada e o ativo original. Se o ativo original for modificado no AEM, você deverá excluir o ativo incorporado do arquivo do InDesign e incorporar novamente o ativo do AEM.

+ **Local vinculado** - Ao trabalhar com documentos do InDesign, você tem a opção de fazer referência aos ativos do AEM, além de incorporar diretamente os ativos (usando a opção Inserir cópia no menu de contexto). A referência a ativos permite colaborar com outros usuários e incorporar todas as atualizações feitas no ativo original no AEM. Para fazer referência a um ativo do AEM, use a opção Inserir vinculado no menu de contexto.

### Imagens somente para posicionamento

Quando arquivos de ativos grandes são colocados em Documentos do InDesign a partir do AEM usando o Adobe Asset Link, os usuários de criação precisam aguardar alguns segundos após iniciar a operação de inserção. Isso afeta a experiência geral do usuário. Com o Adobe Asset Link, você pode colocar temporariamente uma imagem de baixa resolução do ativo original do AEM, reduzindo assim o tempo necessário para colocar uma imagem. Ao mesmo tempo, aumenta a experiência geral do usuário e a produtividade. A imagem de resolução mais baixa é colocada temporariamente e, quando a saída final for necessária para impressão ou publicação, será necessário substituir as representações FPO pelos originais. Se quiser substituir várias imagens FPO pelas respectivas imagens originais, navegue até **_Janelas > Links_** e baixe os ativos originais. Depois que as imagens originais forem baixadas, escolha Substituir todas as FPOs por originais.

As representações FPO são substitutos leves dos ativos originais. Elas têm a mesma proporção, mas são de tamanho menor em comparação às imagens originais. Atualmente, o InDesign oferece suporte à importação de representações FPO somente para os seguintes tipos de imagem:

+ JPEG
+ GIF
+ PNG
+ TIFF
+ PSD
+ BMP

Se uma representação FPO não estiver disponível para um ativo específico no AEM, o ativo original de alta resolução será referenciado. Para imagens FPO, o status FPO é exibido no painel Vínculos de InDesign.

## Autenticação do Adobe Asset Link com o AEM Assets

Como a autenticação do Adobe Asset Link funciona no contexto do Adobe Identity Management Services (IMS) e do Adobe Experience Manager Author.

![Arquitetura do Adobe Asset Link](assets/adobe-asset-link-article-understand.png)

1. A extensão Adobe Asset Link faz uma solicitação de autorização, por meio do aplicativo de desktop da Adobe Creative Cloud, para o Adobe Identity Manager Service (IMS) e, após ser bem-sucedida, recebe um token de portador.
1. A extensão Adobe Asset Link conecta-se ao AEM Author por HTTP(S), incluindo o token do portador obtido em **Etapa 1**, usando o esquema (HTTP/HTTPS), o host e a porta fornecidos nas configurações de JSON da extensão.
1. O Manipulador de autenticação de portador do AEM extrai o token do portador da solicitação e o valida com o Adobe IMS.
1. Depois que o Adobe IMS valida o token do portador, um usuário é criado no AEM (se ele ainda não existir) e sincroniza dados de perfil e de grupos/associações do Adobe IMS. O usuário AEM recebe um token de logon AEM padrão, que é enviado de volta para a extensão Adobe Asset Link como um Cookie na resposta HTTP(S).
1. Interações subsequentes (ou seja, navegar, pesquisar, fazer check-in/check-out de ativos etc.) com a extensão Adobe Asset Link, o resultado são solicitações HTTP(S) para o AEM Author, validadas usando o token de logon AEM, usando o Manipulador de autenticação de token de AEM padrão.

>[!NOTE]
>
>Após a expiração do token de logon, **Etapas 1-5** O chamará automaticamente o, autenticando a extensão Adobe Asset Link usando o token de portador e emitirá novamente um token de logon novo e válido.

## Recursos adicionais

+ [Site do Adobe Asset Link](https://www.adobe.com/br/creativecloud/business/enterprise/adobe-asset-link.html)
