---
title: Adobe Asset Link e AEM
description: 'Os ativos do Adobe Experience Manager podem ser usados por designers e usuários criativos em seus aplicativos favoritos de desktop do Adobe Creative Cloud. A extensão Adobe Asset Link para o Adobe Creative Cloud for enterprise amplia a capacidade de pesquisar e navegar, classificar, visualizar, carregar ativos, sair, modificar, fazer check-in e exibir metadados de ativos AEM nas ferramentas do Creative Cloud, como Adobe XD, Photoshop, InDesign e Illustrator. '
feature: Adobe Asset Link
version: 6.4, 6.5, cloud-service
topic: Gerenciamento de conteúdo
role: User
level: Beginner
thumbnail: 28988.jpg
source-git-commit: 0cfa83bdbd534f0fa06b3fa0013971feb188224e
workflow-type: tm+mt
source-wordcount: '1029'
ht-degree: 2%

---


# Adobe Asset Link 3.0

Os ativos do Adobe Experience Manager podem ser usados por designers e usuários criativos em seus aplicativos favoritos de desktop do Adobe Creative Cloud.

A extensão Adobe Asset Link para o Adobe Creative Cloud for enterprise amplia a capacidade de pesquisar e navegar, classificar, visualizar, carregar ativos, sair, modificar, fazer check-in e exibir metadados de ativos AEM em aplicativos Creative Cloud.


## Adobe Asset Link e AEM fluxos de trabalho criativos

O vídeo a seguir ilustra um fluxo de trabalho comum usado por criadores que trabalham em aplicativos Adobe Creative Cloud e fazem a integração diretamente com AEM usando o Adobe Asset Link.

>[!VIDEO](https://video.tv.adobe.com/v/335927/?quality=12&learn=on)

## Recursos do Adobe Asset Link

+ O Adobe Asset Link integra-se ao AEM Assets e ao Assets Essentials.
+ O Adobe Asset Link configura automaticamente a conexão com ambientes de AEM baseados em nuvem (AEM Assets as a Cloud Service e Assets Essentials)
+ O Adobe Asset Link é uma extensão que funciona em aplicativos Adobe Creative Cloud:

   + Adobe XD
   + Adobe Photoshop
   + Adobe Illustrator
   + Adobe InDesign

+ Autenticação automática para AEM usando Adobe Enterprise ID ou Federated ID
+ Procure por ativos digitais no AEM
+ Acesse os detalhes do arquivo para ativos que residem no AEM com o painel :
   + Miniatura 
   + Metadados básicos
   + Versões
+ Inserir, baixar ou arrastar e soltar ativos em seu layout
+ Modifique ativos fazendo check-out de AEM e trabalhando neles (WIP) em sua conta Creative Cloud Assets
+ Verifique um ativo de volta no AEM após a conclusão da modificação, e a nova versão será refletida no AEM
+ Pesquise ativos no AEM no painel no aplicativo do Adobe Asset Link
+ Procurar coleções e coleções inteligentes do AEM Assets diretamente do painel Link de ativos
+ Adicionar ativos recém-criados ao AEM diretamente do painel
+ Arraste e solte ativos diretamente nos quadros do InDesign

## Colocar ativos no InDesign

O Adobe Asset Link fornece suporte a vinculação direta do InDesign entre o Adobe Asset Link e o AEM. Com o suporte a vinculação direta do InDesign, você pode colocar (__Colocar vinculadas__ ou __Inserir cópia__) ou arrastar e soltar ativos digitais no InDesign a partir do AEM por meio do painel Adobe Asset Link. Além disso, introduz a renderização *Para posicionamento somente+ (FPO) .

>[!VIDEO](https://video.tv.adobe.com/v/28988/?quality=12&learn=on)

>[!NOTE]
>
>Use somente o Enterprise ID ou Federated ID Adobe Creative Cloud. Certifique-se de [configurar AEM para Adobe Asset Link](https://helpx.adobe.com/enterprise/admin-guide.html/enterprise/using/adobe-asset-link.ug.html).

Você pode colocar um ativo no layout do InDesign usando uma das opções abaixo:

+ **Inserir cópia**  - a incorporação de um ativo (usando a opção Inserir cópia) coloca uma cópia do ativo original no layout do InDesign depois de baixar os binários no sistema local. O Adobe Asset Link não mantém nenhum link entre a cópia incorporada e o ativo original. Se o ativo original for modificado no AEM, você deverá excluir o ativo incorporado do arquivo do InDesign e reincorporar o ativo do AEM.

+ **Local vinculado**  - Ao trabalhar com documentos do InDesign, você tem a opção de fazer referência aos ativos do AEM, além de incorporar diretamente os ativos (usando a opção Inserir cópia no menu de contexto). Referenciar ativos permite colaborar com outros usuários e incorporar qualquer atualização feita ao ativo original no AEM. Para fazer referência a um ativo do AEM, use a opção Inserir link no menu de contexto.

### Para imagens somente posicionamento

Quando arquivos de ativos grandes são colocados em Documentos do InDesign a partir de AEM usando o Adobe Asset Link, os usuários de criação precisam aguardar alguns segundos após iniciar a operação place. Isso afeta a experiência geral do usuário. Com o Adobe Asset Link, é possível colocar temporariamente uma imagem de baixa resolução do ativo original a partir do AEM, reduzindo assim o tempo gasto para colocar uma imagem. Ao mesmo tempo, aumenta a experiência geral do usuário e a produtividade. A imagem de resolução mais baixa é colocada temporariamente e quando a saída final é necessária para impressão ou publicação, é necessário substituir as representações FPO pelos originais. Se quiser substituir várias imagens FPO pelas respectivas imagens originais, navegue até o painel **_Windows > Links_** e baixe os ativos originais. Após o download das imagens originais, escolha Substituir todas as FPOs por originais.

As representações de FPO são substitutos leves dos ativos originais. Elas têm a mesma proporção, mas têm tamanho menor em comparação às imagens originais. Atualmente, o InDesign suporta a importação de representações FPO somente para os seguintes tipos de imagem:

+ JPEG
+ GIF
+ PNG
+ TIFF
+ PSD
+ BMP

Se uma representação de FPO não estiver disponível para um ativo específico no AEM, o ativo original de alta resolução será referenciado. Para imagens FPO, o status FPO é exibido no painel Links do InDesign .

## Autenticação do Adobe Asset Link com o AEM Assets

Como a autenticação do Adobe Asset Link funciona no contexto do Adobe Identity Management Services (IMS) e do Autor do Adobe Experience Manager.

![Arquitetura do Adobe Asset Link](assets/adobe-asset-link-article-understand.png)

1. A extensão Adobe Asset Link faz uma solicitação de autorização, por meio do aplicativo de desktop do Adobe Creative Cloud, para o Adobe Identity Management Service (IMS) e, após o sucesso, recebe um token de portador.
1. A extensão Adobe Asset Link conecta-se ao Autor do AEM por HTTP(S), incluindo o token Portador obtido em **Etapa 1**, usando o esquema (HTTP/HTTPS), o host e a porta fornecidos nas configurações da extensão JSON.
1. O Manipulador de autenticação do portador da AEM extrai o token do portador da solicitação e o valida com o Adobe IMS.
1. Depois que o Adobe IMS valida o token do portador, um usuário é criado no AEM (caso ainda não exista) e sincroniza os dados de perfil e grupo/associações do Adobe IMS. O usuário AEM recebe um token de login AEM padrão, que é enviado de volta à extensão Adobe Asset Link como um Cookie na resposta HTTP(S).
1. Interações subsequentes (ou seja, navegação, pesquisa, check-in/out de ativos etc.) com a extensão do Adobe Asset Link resulta em solicitações HTTP(S) para o Autor do AEM, que são validadas usando o token de login AEM, usando o Manipulador de Autenticação de Token AEM padrão.

>[!NOTE]
>
>Após a expiração do token de logon, as **Etapas 1-5** chamarão automaticamente, autenticando a extensão do Adobe Asset Link usando o token Portador e emitirão novamente um token de logon novo e válido.

## Recursos adicionais

+ [Site do Adobe Asset Link](https://www.adobe.com/br/creativecloud/business/enterprise/adobe-asset-link.html)
