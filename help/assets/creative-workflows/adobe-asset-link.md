---
title: Uso do Adobe Asset Link Extension com o AEM Assets
description: 'Os ativos Adobe Experience Manager agora podem ser usados por designers e usuários criativos em seus aplicativos favoritos de desktop da Adobe Creative Cloud. A extensão Adobe Asset Link para Adobe Creative Cloud Enterprise amplia a capacidade de pesquisar e navegar, classificar, pré-visualização, fazer upload de ativos, fazer check-out, modificar, check-in e visualização de metadados de ativos AEM em ferramentas Creative Cloud como Adobe Photoshop, InDesign e Illustrator. '
feature: adobe-asset-link
topics: authoring, collaboration, operations, sharing, metadata, images
audience: all
doc-type: feature video
activity: use
version: 6.4, 6.5
translation-type: tm+mt
source-git-commit: 67ca08bf386a217807da3755d46abed225050d02
workflow-type: tm+mt
source-wordcount: '1092'
ht-degree: 1%

---


# Uso do Adobe Asset Link Extension com o AEM Assets{#using-adobe-asset-link-extension-with-aem-assets}

Os ativos Adobe Experience Manager agora podem ser usados por designers e usuários criativos em seus aplicativos favoritos de desktop da Adobe Creative Cloud. A extensão Adobe Asset Link para Adobe Creative Cloud Enterprise amplia a capacidade de pesquisar e navegar, classificar, pré-visualização, fazer upload de ativos, fazer check-out, modificar, check-in e visualização de metadados de ativos AEM em ferramentas Creative Cloud como Adobe Photoshop, InDesign e Illustrator.


## Link de ativo do Adobe 1.1

O Adobe Asset Link v1.1 agora oferece suporte a vinculação direta de InDesigns entre o Adobe Asset Link e o AEM Assets. Com o suporte a vinculação direta de InDesign, agora é possível colocar (Inserir vinculado ou Inserir cópia) ou arrastar e soltar ativos digitais no InDesign da AEM Assets por meio do painel Link de ativo do Adobe. Além disso, apresenta a execução *para somente* disposição (FPO).

>[!VIDEO](https://video.tv.adobe.com/v/28988/?quality=12&learn=on)

>[!NOTE]
>
>Use apenas seu Enterprise ID ou Federated ID Adobe Creative Cloud. Certifique-se de [configurar o AEM para o Link](https://helpx.adobe.com/enterprise/using/configure-aem-for-aal-prerelease.html)de ativo do Adobe.


### Recursos de link do ativo do Adobe

* O Link de ativo do Adobe é uma extensão que funciona dentro do PS, AI, ID e fornece acesso direto aos ativos digitais que residem no AEM Assets
* Os profissionais de criação serão conectados AEM automaticamente usando seu Enterprise ID ou Federated ID Adobe IMS
* Os profissionais de criação podem procurar ativos digitais que residem no AEM Assets, além de pesquisar no AEM Assets e no Creative Cloud Assets
* Os profissionais de criação podem acessar detalhes do arquivo para ativos residentes na AEM Assets; miniatura, metadados básicos e versões de dentro do painel
* Os profissionais de criação podem colocar, baixar ou arrastar e soltar ativos em seus layouts
* Os criativos podem modificar ativos fazendo check-out da AEM Assets e trabalhando neles (WIP) em sua conta de Ativos da Creative Cloud
* Os Criadores podem verificar um ativo de volta no AEM Assets depois que terminarem de modificá-lo, e a nova versão será refletida no AEM Assets
* Suporta aplicativos para desktop Creative Cloud 2020, 2019 e 2018, InDesign Photoshop e Illustrator
* Um usuário pode realizar uma pesquisa de ativos no painel Adobe Asset Link no aplicativo e classificá-los com base no tamanho, em ordem alfabética e por relevância
* Os usuários podem acessar e navegar pelas coleções e coleções inteligentes do AEM Assets diretamente do painel Link do ativo
* Adicionar ativos recém-criados à AEM Assets diretamente do painel
* Um usuário pode Arrastar e soltar ativos diretamente em quadros de InDesign

### Colocando o AEM Assets no InDesign

Você pode colocar um ativo no layout do seu InDesign usando uma das opções abaixo:

* **Inserir cópia** - a incorporação de um ativo (usando a opção Inserir cópia) coloca uma cópia do ativo original no layout do InDesign depois de baixar os binários no sistema local. O Link de ativo do Adobe não mantém nenhum link entre a cópia incorporada e o ativo original. Se o ativo original for modificado no AEM Assets, você deverá excluir o ativo incorporado do arquivo do InDesign e reincorporar o ativo da AEM Assets.

* **Local vinculado** - ao trabalhar com documentos de InDesign, agora você tem a opção de fazer referência aos ativos da AEM Assets, além de incorporar diretamente os ativos (usando a opção Inserir cópia no menu de contexto). Os ativos de referência permitem que você colabore com outros usuários e incorpore quaisquer atualizações feitas ao ativo original no AEM Assets. Para fazer referência a um ativo do AEM Assets, use a opção Inserir vinculado no menu de contexto.

### Resolução Somente para Posicionamento (FPO)

Quando grandes arquivos de ativos são colocados em Documentos de InDesign da AEM Assets usando o Link de ativo Adobe, os usuários devem aguardar alguns segundos após iniciarem a operação de colocação. Isso afeta a experiência geral do usuário. Com o Link de ativo Adobe, agora é possível colocar temporariamente uma imagem de baixa resolução do ativo original da AEM Assets, reduzindo o tempo gasto para inserir uma imagem. Ao mesmo tempo, aumenta a experiência geral do usuário e a produtividade. A imagem de resolução mais baixa é colocada temporariamente e quando a saída final é necessária para impressão ou publicação, é necessário substituir as execuções FPO pelos originais. Se você quiser substituir várias imagens FPO pelas respectivas imagens originais, navegue até **_Windows > Painel Vínculos_** e baixe os ativos originais. Depois que as imagens originais forem baixadas, escolha Substituir todos os FPOs por originais.

>[!NOTE]
>
> *Para execução Somente disposição (FPO)* , a execução funciona somente para a opção Local vinculado. Você também deve habilitar o suporte à execução do FPO no fluxo de trabalho do Ativo *de atualização do AEM Assets* Dam.

As representações FPO são substitutos leves dos ativos originais. Elas têm a mesma proporção, mas têm tamanho menor em comparação às imagens originais. Atualmente, o InDesign suporta a importação de execuções FPO somente para os seguintes tipos de imagem:

* JPEG
* GIF
* PNG
* TIFF
* PSD
* BMP

Se uma representação FPO não estiver disponível para um ativo específico no AEM Assets, o ativo original de alta resolução será referenciado. Para imagens FPO, o status FPO é exibido no painel Links de InDesign.



## Noções básicas sobre a autenticação do Link do ativo de Adobe com o AEM Assets{#understanding-adobe-asset-link-authentication-with-aem-assets}

Como a autenticação do Link do ativo do Adobe funciona no contexto do Adobe Identity Management Services (IMS) e do Autor do Adobe Experience Manager.

![Arquitetura do link do ativo Adobe](assets/adobe-asset-link-article-understand.png)

Baixar a arquitetura do link do ativo [Adobe](assets/adobe-asset-link-article-understand-1.png)

1. A extensão Adobe Asset Link faz uma solicitação de autorização, por meio do Adobe Creative Cloud Desktop App, para o Adobe Identity Manage Service (IMS) e, quando bem-sucedido, recebe um token de Portador.
2. A extensão do Link do ativo do Adobe se conecta ao autor do AEM em HTTP(S), incluindo o token do Portador obtido na **Etapa 1**, usando o esquema (HTTP/HTTPS), o host e a porta fornecidos nas configurações da extensão JSON.
3. O Manipulador de autenticação do portador da AEM extrai o token do portador da solicitação e o valida em relação ao Adobe IMS.
4. Depois que o Adobe IMS valida o token do Portador, um usuário é criado no AEM (se ainda não existir) e sincroniza os dados do perfil e do grupo/associação do Adobe IMS. O usuário AEM recebe um token de login AEM padrão, que é enviado de volta para a extensão Adobe Asset Link como um Cookie na resposta HTTP(S).
5. Interações subsequentes (ou seja, navegar, pesquisar, fazer check-in/check-out de ativos etc.) com a extensão Adobe Asset Link resulta em solicitações HTTP(S) para o autor de AEM, que são validadas usando o token de login AEM, usando o Manipulador de autenticação de token AEM padrão.

>[!NOTE]
>
>Após a expiração do token de login, as **Etapas 1 a 5** chamarão automaticamente, autenticarão a extensão do Adobe Asset Link usando o token do portador e reemitirão um token de login novo e válido.

## Recursos adicionais{#additional-resources}

* [Site do link do ativo Adobe](https://www.adobe.com/br/creativecloud/business/enterprise/adobe-asset-link.html)