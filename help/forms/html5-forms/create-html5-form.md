---
title: Criar HTML5 Forms
description: Criar e configurar formulários HTML5
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
jira: KT-4419
thumbnail: kt-4419.jpg
topic: Development
role: User
level: Beginner
exl-id: 67a01c41-d284-4518-adb5-21702e22ccfa
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '481'
ht-degree: 0%

---

# Criar formulários HTML5

Os formulários HTML5 são um novo recurso no Adobe Experience Manager que oferece renderização de modelos de formulário XFA (xdp) no formato HTML5. Esse recurso permite a renderização de formulários em dispositivos móveis e navegadores de desktop nos quais o PDF baseado em XFA não é compatível. Os formulários HTML5 não só são compatíveis com os recursos existentes dos modelos de formulário XFA, como também adicionam novos recursos, como a assinatura à mão, para dispositivos móveis.

## Pré-requisitos

Certifique-se de que você tenha uma instância em funcionamento do AEM Forms. Siga as instruções [guia de instalação](https://experienceleague.adobe.com/docs/experience-manager-65/forms/install-aem-forms/osgi-installation/installing-configuring-aem-forms-osgi.html) para instalar e configurar o AEM Forms

## Crie seu primeiro formulário HTML5

1. [Baixe e extraia o conteúdo do arquivo zip](assets/assets.zip). O arquivo zip contém xdp e arquivo de dados
2. [Navegue até Forms e Documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
3. Clique em Criar -> Upload de arquivo
4. Selecione o modelo xdp baixado na etapa 2

## Visualizar como HTML

O xdp pode ser visualizado no formato HTML5 ou PDF. Para visualizar o xdp no formato HTML5, siga as etapas a seguir

* Toque no xdp recém-carregado e clique em _Visualizar -> Visualizar como HTML_. Você deve ver o xdp renderizado como HTML5

>[!NOTE]
>Ao selecionar _Visualizar como PDF_ opção o PDF renderizado não será exibido no navegador porque o AEM Forms renderiza pdf dinâmicos que exigem o plug-in do Acrobat.Você terá que baixar o PDF e abri-lo usando o Adobe Acrobat/Reader para visualizar


## Exibir com dados

Para visualizar o xdp no formato HTML5 com o arquivo de dados, siga as seguintes etapas:

* Toque no xdp recém-carregado e clique em _Visualizar -> Visualizar com dados_. Procure e selecione o arquivo de dados e clique em _Visualizar_.
* Você deve ver o modelo renderizado no formato HTML5 pré-preenchido com os dados

## Explorar propriedades avançadas do modelo xdp

As propriedades avançadas do modelo xdp permitem especificar a data de publicação, o manipulador de envio, o perfil de renderização do formulário, o serviço de preenchimento prévio etc. Para exibir as propriedades avançadas do modelo, toque no xdp e clique em _propriedades -> Avançado_. Aqui você encontrará várias propriedades. Algumas dessas propriedades são abordadas aqui.

**Enviar URL** - Este é o URL que lidará com o envio do formulário HTML5. Abordaremos isso na próxima lição. Se uma URL de envio não for especificada aqui, o manipulador de envio padrão é chamado, o que retorna os dados do formulário para o navegador.

**Perfil de renderização do HTML** - Os formulários HTML5 têm a noção de Perfis que são expostos como Terminais REST para permitir a Renderização móvel dos Modelos de formulário. A maioria das vezes o perfil de renderização padrão deve ser suficiente para renderizar o formulário. Se o perfil de renderização padrão não atender às suas necessidades, uma [perfil personalizado](https://experienceleague.adobe.com/docs/experience-manager-65/forms/html5-forms/custom-profile.html) podem ser criados e associados ao formulário.

**Preencher Serviço** - O serviço de preenchimento prévio geralmente é usado para preencher seu formulário com dados obtidos de uma fonte de dados de back-end.
