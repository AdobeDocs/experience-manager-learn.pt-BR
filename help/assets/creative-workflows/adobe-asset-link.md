---
title: Uso da extensão do Adobe Asset Link com o AEM Assets
description: 'Os ativos do Adobe Experience Manager agora podem ser usados por designers e usuários criativos em seus aplicativos favoritos de desktop da Adobe Creative Cloud. A extensão do Adobe Asset Link para a Adobe Creative Cloud Enterprise estende a capacidade de pesquisar e navegar, classificar, visualizar, carregar ativos, sair, modificar, fazer check-in e exibir metadados de ativos AEM nas ferramentas da Creative Cloud, como Adobe Photoshop, InDesign e Illustrator. '
feature: Adobe Asset Link
version: 6.4, 6.5
topic: Content Management
role: Business Practitioner
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1102'
ht-degree: 1%

---


# Uso da extensão do Adobe Asset Link com o AEM Assets{#using-adobe-asset-link-extension-with-aem-assets}

Os ativos do Adobe Experience Manager agora podem ser usados por designers e usuários criativos em seus aplicativos favoritos de desktop da Adobe Creative Cloud. A extensão do Adobe Asset Link para a Adobe Creative Cloud Enterprise estende a capacidade de pesquisar e navegar, classificar, visualizar, carregar ativos, sair, modificar, fazer check-in e exibir metadados de ativos AEM nas ferramentas da Creative Cloud, como Adobe Photoshop, InDesign e Illustrator.


## Adobe Asset Link 1.1

O Adobe Asset Link v1.1 agora fornece suporte a vinculação direta do InDesign entre o Adobe Asset Link e o AEM Assets. Com o suporte ao link direto do InDesign, agora é possível colocar (Inserir link ou Inserir cópia) ou arrastar e soltar ativos digitais no InDesign a partir do AEM Assets por meio do painel Adobe Asset Link. Além disso, introduz a representação *For Placement Only* (FPO).

>[!VIDEO](https://video.tv.adobe.com/v/28988/?quality=12&learn=on)

>[!NOTE]
>
>Use somente a Adobe Creative Cloud Enterprise ID ou Federated ID. Certifique-se de [configurar o AEM para o Adobe Asset Link](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/adobe-asset-link.ug.html).


### Recursos do Adobe Asset Link

* O Adobe Asset Link é uma extensão que funciona dentro do PS, AI, ID e fornece acesso direto a ativos digitais que residem no AEM Assets
* Os criadores serão conectados ao AEM automaticamente usando a Adobe IMS Enterprise ID ou a Federated ID
* Os Creative Cloud podem procurar ativos digitais que residem no AEM Assets, além de pesquisar no AEM Assets e no Creative Cloud Assets
* Os criadores podem acessar os detalhes do arquivo para ativos que residem no AEM Assets; miniatura, metadados básicos e versões no painel
* Os criadores podem colocar, baixar ou arrastar e soltar ativos em seu layout
* Os criadores podem modificar ativos fazendo check-out do AEM Assets e trabalhando neles (WIP) na conta do Creative Cloud Assets
* Os criadores podem verificar um ativo de volta no AEM Assets depois que terminarem de modificá-lo, e a nova versão será refletida nos AEM Assets
* Suporta aplicativos Creative Cloud 2020, 2019 e 2018 InDesign, Photoshop e Illustrator para desktop
* Um usuário pode realizar uma pesquisa de ativos no painel no aplicativo do Adobe Asset Link e classificá-los com base no tamanho, em ordem alfabética e por relevância
* Os usuários podem acessar e navegar pelas coleções de ativos AEM e coleções inteligentes diretamente do painel Link de ativos
* Adicionar ativos recém-criados ao AEM Assets diretamente do painel
* Um usuário pode Arrastar e soltar ativos diretamente nos quadros do InDesign

### Colocar AEM Assets no InDesign

Você pode colocar um ativo no layout do InDesign usando uma das opções abaixo:

* **Inserir cópia**  - a incorporação de um ativo (usando a opção Inserir cópia) coloca uma cópia do ativo original no layout do InDesign depois de baixar os binários no sistema local. O Adobe Asset Link não mantém nenhum link entre a cópia incorporada e o ativo original. Se o ativo original for modificado no AEM Assets, você deverá excluir o ativo incorporado do arquivo do InDesign e reincorporar o ativo do AEM Assets.

* **Local vinculado**  - Ao trabalhar com documentos do InDesign, agora há a opção de fazer referência aos ativos do AEM Assets, além de incorporar diretamente os ativos (usando a opção Inserir cópia no menu de contexto). Referenciar ativos permite colaborar com outros usuários e incorporar qualquer atualização feita ao ativo original nos AEM Assets. Para fazer referência a um ativo do AEM Assets, use a opção Inserir link no menu de contexto.

### Resolução Somente para posicionamento (FPO)

Quando grandes arquivos de ativos são colocados em Documentos do InDesign do AEM Assets usando o Adobe Asset Link, os usuários de criação precisam aguardar alguns segundos após iniciar a operação de local. Isso afeta a experiência geral do usuário. Com o Adobe Asset Link, agora é possível colocar temporariamente uma imagem de baixa resolução do ativo original dos Ativos AEM, reduzindo o tempo gasto para colocar uma imagem. Ao mesmo tempo, aumenta a experiência geral do usuário e a produtividade. A imagem de resolução mais baixa é colocada temporariamente e quando a saída final é necessária para impressão ou publicação, é necessário substituir as representações FPO pelos originais. Se quiser substituir várias imagens FPO pelas respectivas imagens originais, navegue até o painel **_Windows > Links_** e baixe os ativos originais. Após o download das imagens originais, escolha Substituir todas as FPOs por originais.

>[!NOTE]
>
> *Somente para posicionamento (FPO)* a representação funciona somente para a opção Inserir vinculado. Você também deve ativar o suporte de representação FPO no fluxo de trabalho do AEM Assets *Dam Update Asset*.

As representações de FPO são substitutos leves dos ativos originais. Elas têm a mesma proporção, mas têm tamanho menor em comparação às imagens originais. Atualmente, o InDesign suporta a importação de representações FPO somente para os seguintes tipos de imagem:

* JPEG
* GIF
* PNG
* TIFF
* PSD
* BMP

Se uma representação de FPO não estiver disponível para um ativo específico no AEM Assets, o ativo original de alta resolução será referenciado. Para imagens FPO, o status FPO é exibido no painel Links do InDesign .

## Noções básicas sobre a autenticação do Adobe Asset Link com o AEM Assets{#understanding-adobe-asset-link-authentication-with-aem-assets}

Como a autenticação do Adobe Asset Link funciona no contexto do Adobe Identity Management Services (IMS) e do Autor do Adobe Experience Manager.

![Arquitetura do Adobe Asset Link](assets/adobe-asset-link-article-understand.png)

Baixar [Arquitetura do Adobe Asset Link](assets/adobe-asset-link-article-understand-1.png)

1. A extensão Adobe Asset Link faz uma solicitação de autorização, por meio do aplicativo Adobe Creative Cloud Desktop, para o Adobe Identity Manager Service (IMS) e, após o sucesso, recebe um token de portador.
2. A extensão do Adobe Asset Link conecta-se ao Autor do AEM por HTTP(S), incluindo o token Portador obtido em **Etapa 1**, usando o esquema (HTTP/HTTPS), o host e a porta fornecidos nas configurações da extensão JSON.
3. O Manipulador de autenticação do portador do AEM extrai o token do portador da solicitação e o valida em relação ao Adobe IMS.
4. Depois que o Adobe IMS valida o token do portador, um usuário é criado no AEM (se ainda não existir) e sincroniza dados de perfil e grupo/associações do Adobe IMS. O usuário do AEM recebe um token de logon padrão do AEM, que é enviado de volta à extensão do Adobe Asset Link como um Cookie na resposta HTTP(S).
5. Interações subsequentes (ou seja, navegação, pesquisa, check-in/out de ativos etc.) com a extensão do Adobe Asset Link, o resulta em solicitações HTTP(S) para o autor do AEM, que são validadas usando o token de logon do AEM, usando o Manipulador de autenticação de token do AEM padrão.

>[!NOTE]
>
>Após a expiração do token de logon, as **Etapas 1-5** chamarão automaticamente, autenticarão a extensão do Adobe Asset Link usando o token Portador e reemitirão um token de logon novo e válido.

## Recursos adicionais{#additional-resources}

* [Site do Adobe Asset Link](https://www.adobe.com/br/creativecloud/business/enterprise/adobe-asset-link.html)