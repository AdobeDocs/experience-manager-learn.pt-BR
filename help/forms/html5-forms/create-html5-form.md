---
title: Criar HTML5 Forms
description: Criar e configurar formulários HTML5
feature: Mobile Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 4419
thumbnail: kt-4419.jpg
topic: Development
role: User
level: Beginner
exl-id: 67a01c41-d284-4518-adb5-21702e22ccfa
last-substantial-update: 2019-07-07T00:00:00Z
source-git-commit: 7a2bb61ca1dea1013eef088a629b17718dbbf381
workflow-type: tm+mt
source-wordcount: '481'
ht-degree: 4%

---

# Criar formulários HTML5

Formulários HTML5 é um novo recurso no Adobe Experience Manager que oferece a renderização de modelos de formulário XFA (xdp) no formato HTML5. Esse recurso permite a renderização de formulários em navegadores para dispositivos móveis e desktop, nos quais o PDF com base em XFA não ofereça suporte. Os formulários HTML5 não só oferecem suporte aos recursos existentes dos modelos de formulário XFA, como também adicionam novos recursos, como assinatura de rabisco, para dispositivos móveis.

## Pré-requisitos

Verifique se você tem uma instância de trabalho do AEM Forms. Siga as [guia de instalação](https://experienceleague.adobe.com/docs/experience-manager-65/forms/install-aem-forms/osgi-installation/installing-configuring-aem-forms-osgi.html) para instalar e configurar o AEM Forms

## Crie seu primeiro formulário HTML5

1. [Baixe e extraia o conteúdo do arquivo zip](assets/assets.zip). O arquivo zip contém xdp e arquivo de dados
2. [Navegar até Forms e Documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
3. Clique em Criar -> Upload de arquivo
4. Selecione o modelo xdp baixado na etapa 2

## Visualizar como HTML

O xdp pode ser visualizado no formato HTML5 ou PDF. Para visualizar o xdp no formato HTML5, siga as seguintes etapas

* Toque no xdp recém-carregado e clique em _Visualizar -> Visualizar como HTML_. Você deve ver o xdp renderizado como HTML5

>[!NOTE]
>Ao selecionar _Visualizar como PDF_ a opção PDF renderizado não será exibida no navegador porque o AEM Forms renderiza pdf dinâmicos que exigem plug-in do Acrobat.Você terá que baixar o PDF e abri-lo usando o Adobe Acrobat/Reader para exibir


## Exibir com dados

Para visualizar o xdp no formato HTML5 com o arquivo de dados, siga as seguintes etapas:

* Toque no xdp recém-carregado e clique em _Visualização -> Visualizar com dados_. Navegue, selecione o arquivo de dados e clique em _Visualizar_.
* Você deve ver o modelo renderizado no formato HTML5 pré-preenchido com os dados

## Explorar as propriedades avançadas do modelo xdp

As propriedades avançadas do modelo xdp permitem especificar a data de publicação, o manipulador de envio, o perfil de renderização do formulário, o serviço de preenchimento prévio etc. Para exibir as propriedades avançadas do modelo, toque em xdp e clique em _propriedades -> Avançado_. Aqui você encontrará várias propriedades do . Algumas dessas propriedades são abordadas aqui.

**Enviar URL** - Esse é o URL que processará o envio do formulário HTML5. Abordaremos esta questão na próxima lição. Se um URL de envio não for especificado, o manipulador de envio padrão será chamado, retornando os dados do formulário ao navegador.

**Perfil de renderização HTML** - Formulários HTML5 têm a noção de Perfis que são expostos como Pontos de extremidade REST para permitir a renderização móvel de modelos de formulário. A maioria das vezes o perfil de renderização padrão deve ser suficiente para renderizar o formulário. Se o perfil de renderização padrão não atender às suas necessidades, um [perfil personalizado](https://experienceleague.adobe.com/docs/experience-manager-64/forms/html5-forms/custom-profile.html) podem ser criadas e associadas ao formulário.

**Serviço de preenchimento prévio** - O serviço de preenchimento prévio geralmente é usado para preencher seu formulário com dados obtidos de uma fonte de dados de back-end.
