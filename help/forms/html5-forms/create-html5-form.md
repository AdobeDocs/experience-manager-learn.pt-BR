---
title: Criar HTML5 Forms
description: Criar e configurar formulários HTML5
feature: Mobile Forms
doc-type: article
version: Experience Manager 6.5
jira: KT-4419
thumbnail: kt-4419.jpg
topic: Development
role: User
level: Beginner
exl-id: 67a01c41-d284-4518-adb5-21702e22ccfa
last-substantial-update: 2019-07-07T00:00:00Z
duration: 101
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '462'
ht-degree: 0%

---

# Criar formulários HTML5

O HTML5 Forms é um novo recurso no Adobe Experience Manager que oferece renderização de modelos de formulário XFA (xdp) no formato HTML5. Esse recurso permite a renderização de formulários em dispositivos móveis e navegadores de desktop nos quais o PDF baseado em XFA não é compatível. Os formulários HTML5 não só oferecem suporte aos recursos existentes de modelos de formulário XFA, como também adicionam novos recursos, como assinatura à mão, para dispositivos móveis.

## Pré-requisitos

Certifique-se de que você tenha uma instância em funcionamento do AEM Forms. Siga o [guia de instalação](https://experienceleague.adobe.com/docs/experience-manager-65/forms/install-aem-forms/osgi-installation/installing-configuring-aem-forms-osgi.html) para instalar e configurar o AEM Forms

## Crie seu primeiro formulário HTML5

1. [Baixe e extraia o conteúdo do arquivo zip](assets/assets.zip). O arquivo zip contém xdp e arquivo de dados
2. [Navegar até o Forms e Documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
3. Clique em Criar -> Upload de arquivo
4. Selecione o modelo xdp baixado na etapa 2

## Visualizar como HTML

O xdp pode ser visualizado no formato HTML5 ou PDF. Para visualizar o xdp no formato HTML5, siga as etapas a seguir

* Toque no xdp recém-carregado e clique em _Visualizar -> Visualizar como HTML_. Você deve ver o xdp renderizado como HTML5

>[!NOTE]
>Ao selecionar a opção _Visualizar como PDF_, o PDF renderizado não será exibido no navegador porque o AEM Forms renderiza PDFs dinâmicos que exigem o plug-in do Acrobat.Será necessário baixar o PDF e abri-lo usando o Adobe Acrobat/Reader para exibir


## Exibir com dados

Para visualizar o xdp no formato HTML5 com o arquivo de dados, siga as seguintes etapas:

* Toque no xdp recém-carregado e clique em _Visualizar -> Visualizar com Dados_. Procure e selecione o arquivo de dados e clique em _Visualizar_.
* Você deve ver o modelo renderizado no formato HTML5 pré-preenchido com os dados

## Explorar propriedades avançadas do modelo xdp

As propriedades avançadas do modelo xdp permitem especificar a data de publicação, o manipulador de envio, o perfil de renderização do formulário, o serviço de preenchimento prévio etc. Para exibir as propriedades avançadas do modelo, toque no xdp e clique em _propriedades -> Avançado_. Aqui você encontrará várias propriedades. Algumas dessas propriedades são abordadas aqui.

**Enviar URL** - Esta é a URL que lidará com o envio do formulário HTML5. Abordaremos isso na próxima lição. Se uma URL de envio não for especificada aqui, o manipulador de envio padrão é chamado, o que retorna os dados do formulário para o navegador.

**Perfil de Renderização do HTML** - Os formulários HTML5 têm a noção de Perfis que são expostos como Pontos de Extremidade REST para habilitar a Renderização Móvel de Modelos de Formulário. A maioria das vezes o perfil de renderização padrão deve ser suficiente para renderizar o formulário. Se o perfil de renderização padrão não atender às suas necessidades, um [perfil personalizado](https://experienceleague.adobe.com/docs/experience-manager-65/forms/html5-forms/custom-profile.html) poderá ser criado e associado ao formulário.

**Serviço de preenchimento prévio** - O serviço de preenchimento prévio geralmente é usado para preencher o formulário com dados obtidos de uma fonte de dados de back-end.
