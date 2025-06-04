---
title: Adobe Asset Link e AEM
description: Os ativos do Adobe Experience Manager podem ser usados por designers e usuários criativos em seus aplicativos para desktop favoritos da Adobe Creative Cloud. A extensão Adobe Asset Link da Adobe Creative Cloud para corporações estende a capacidade de pesquisar e navegar, classificar, visualizar, carregar, remover, modificar e enviar ativos e exibir metadados de ativos do AEM em ferramentas da Creative Cloud, como Adobe XD, Photoshop, InDesign e Illustrator.
feature: Adobe Asset Link
version: Experience Manager 6.4, Experience Manager 6.5, Experience Manager as a Cloud Service
topic: Content Management
role: User
level: Beginner
thumbnail: 28988.jpg
duration: 673
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: ht
source-wordcount: '983'
ht-degree: 100%

---


# Adobe Asset Link 3.0

Os ativos do Adobe Experience Manager podem ser usados por designers e usuários criativos em seus aplicativos para desktop favoritos da Adobe Creative Cloud.

A extensão Adobe Asset Link da Adobe Creative Cloud para corporações estende a capacidade de pesquisar e navegar, classificar, visualizar, carregar, remover, modificar e enviar ativos e exibir metadados de ativos do AEM em aplicativos da Creative Cloud.

## Recursos do Adobe Asset Link

+ O Adobe Asset Link integra-se ao AEM Assets e ao Assets Essentials.
+ O Adobe Asset Link configura automaticamente a conexão com ambientes do AEM baseados na nuvem (AEM Assets as a Cloud Service e Assets Essentials)
+ O Adobe Asset Link é uma extensão que funciona em aplicativos da Adobe Creative Cloud:

   + Adobe XD
   + Adobe Photoshop
   + Adobe Illustrator
   + Adobe InDesign

+ Autenticação automática no AEM por meio da Enterprise ID ou Federated ID da Adobe
+ Procurar e pesquisar ativos digitais no AEM
+ Acesse os detalhes dos arquivos de ativos que residem no AEM pelo painel:
   + Miniatura 
   + Metadados básicos
   + Versões
+ Inserir, baixar ou arrastar e soltar ativos no layout
+ Para modificar ativos, remova-os do AEM e edite-os (por meio de WIP) em sua conta do Creative Cloud Assets
+ Envie um ativo de volta para o AEM quando terminar de modificá-lo e depois que a nova versão for aplicada no AEM
+ Pesquisar ativos no AEM pelo painel do Adobe Asset Link no aplicativo
+ Procurar coleções do AEM Assets e coleções inteligentes diretamente do painel do Asset Link
+ Adicionar ativos recém-criados ao AEM diretamente do painel
+ Arrastar e soltar ativos diretamente em quadros do InDesign

## Inserir ativos no InDesign

O Adobe Asset Link permite a vinculação direta do InDesign no Adobe Asset Link e no AEM. Com a possibilidade de vinculação direta do InDesign, você pode inserir (__Inserir vinculado__ ou __Inserir cópia__) ou arrastar e soltar ativos digitais no InDesign a partir do AEM por meio do painel do Adobe Asset Link. Além disso, ele introduz a representação *Somente para inserção+ (FPO).

>[!VIDEO](https://video.tv.adobe.com/v/37236?quality=12&learn=on&captions=por_br)

>[!NOTE]
>
>Use somente a sua Enterprise ID ou Federated ID da Adobe Creative Cloud. Certifique-se de [configurar o AEM para o Adobe Asset Link](https://helpx.adobe.com/br/enterprise/admin-guide.html/enterprise/using/adobe-asset-link.ug.html).

Você pode inserir um ativo no layout do InDesign por meio de uma das opções abaixo:

+ **Inserir cópia**: a incorporação de um ativo (usando a opção Inserir cópia) insere uma cópia do ativo original no layout do InDesign depois de baixar os binários para o seu sistema local. O Adobe Asset Link não mantém nenhum vínculo entre a cópia incorporada e o ativo original. Se o ativo original for modificado no AEM, você precisará excluir o ativo incorporado do arquivo do InDesign e incorporar novamente o ativo a partir do AEM.

+ **Inserir vinculado**: ao trabalhar com documentos do InDesign, você tem a opção de fazer referência aos ativos do AEM, além de incorporar diretamente os ativos (usando a opção “Inserir cópia” no menu de contexto). A referência a ativos permite colaborar com outros usuários e incorporar todas as atualizações feitas no ativo original no AEM. Para referenciar um ativo do AEM, use a opção “Inserir vinculado” no menu de contexto.

### Imagens “Somente para inserção”

Ao inserir arquivos de ativos grandes em documentos do InDesign a partir do AEM por meio do Adobe Asset Link, os usuários criativos precisam aguardar alguns segundos após iniciar a operação de inserção. Isso afeta a experiência geral do usuário. Com o Adobe Asset Link, é possível inserir temporariamente uma imagem de baixa resolução do ativo original do AEM, reduzindo o tempo necessário para inserir uma imagem. Ao mesmo tempo, isso aprimora a experiência geral do usuário e aumenta a produtividade. Uma imagem de resolução mais baixa é inserida temporariamente e, quando o resultado final for necessário para impressão ou publicação, será necessário substituir as representações de FPO pelas originais. Se quiser substituir várias imagens de FPO pelas respectivas imagens originais, navegue até o painel **_Windows > Links_** e baixe os ativos originais. Depois que as imagens originais forem baixadas, escolha “Substituir todas as FPOs pelas originais”.

As representações de FPO são versões mais leves dos ativos originais. Elas têm a mesma proporção, mas possuem um tamanho menor em comparação com as imagens originais. Atualmente, o InDesign permite a importação de representações de FPO somente para os seguintes tipos de imagem:

+ JPEG
+ GIF
+ PNG
+ TIFF
+ PSD
+ BMP

Se não existir uma representação de FPO disponível para um ativo específico no AEM, o ativo original de alta resolução será referenciado no lugar dela. Para imagens de FPO, o status da imagem é exibido no painel “Links” do InDesign.

## Autenticação do Adobe Asset Link com o AEM Assets

Como a autenticação do Adobe Asset Link funciona no contexto do Adobe Identity Management Service (IMS) e no processo de criação do Adobe Experience Manager.

![Arquitetura do Adobe Asset Link](assets/adobe-asset-link-article-understand.png)

1. Utilizando o aplicativo para desktop da Adobe Creative Cloud, a extensão do Adobe Asset Link faz uma solicitação de autorização ao Adobe Identity Manager Service (IMS) e, após obter êxito, recebe um token de portador.
1. A extensão do Adobe Asset Link conecta-se ao ambiente de criação do AEM por HTTP(S), inclui o token de portador obtido na **etapa 1** e utiliza o esquema (HTTP/HTTPS), o host e a porta fornecidos nas configurações de JSON da extensão.
1. O manipulador de autenticação de portador do AEM extrai o token de portador da solicitação e valida-o com o Adobe IMS.
1. Depois que o Adobe IMS valida o token de portador, um usuário é criado no AEM (caso ainda não exista) e sincroniza dados de perfis e grupos/associações do Adobe IMS. O usuário do AEM recebe um token de logon padrão do AEM, que é enviado de volta à extensão do Adobe Asset Link como um cookie na resposta HTTP(S).
1. Interações subsequentes (ou seja, navegar, pesquisar, enviar, remover ativos etc.) com a extensão do Adobe Asset Link resulta em solicitações HTTP(S) para o ambiente de criação do AEM, que são validadas por meio do token de logon do AEM usando o manipulador de autenticação de token padrão do AEM.

>[!NOTE]
>
>Após a expiração do token de logon, as **etapas 1 a 5** são invocadas automaticamente, autenticando a extensão do Adobe Asset Link por meio do token de portador, a fim de emitir um novo token de logon válido.

## Recursos adicionais

+ [Site do Adobe Asset Link](https://www.adobe.com/br/creativecloud/business/enterprise/adobe-asset-link.html)
