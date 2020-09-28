---
title: Criar HTML5 Forms
description: Criar e configurar formulários HTML5
feature: mobile-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 4419
thumbnail: kt-4419.jpg
translation-type: tm+mt
source-git-commit: c60a46027cc8d71fddd41aa31dbb569e4df94823
workflow-type: tm+mt
source-wordcount: '485'
ht-degree: 4%

---


# Criar formulários HTML5

Formulários HTML5 são um novo recurso no Adobe Experience Manager que oferta a renderização de modelos de formulário XFA (xdp) no formato HTML5. Esse recurso permite a renderização de formulários em navegadores para dispositivos móveis e desktop, nos quais o PDF com base em XFA não ofereça suporte. Formulários HTML5 não só oferecem suporte aos recursos existentes dos modelos de formulário XFA, como também acrescentam novos recursos, como assinatura em forma de script, para dispositivos móveis.

## Pré-requisitos

Certifique-se de que você tenha uma instância funcional do AEM Forms. Siga o guia [de](https://docs.adobe.com/content/help/en/experience-manager-65/forms/install-aem-forms/osgi-installation/installing-configuring-aem-forms-osgi.html) instalação para instalar e configurar o AEM Forms

## Criar seu primeiro formulário HTML5

1. [Baixe e extraia o conteúdo do arquivo](assets/assets.zip)zip. O arquivo zip contém xdp e o arquivo de dados
2. [Navegue até Forms e Documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
3. Clique em Criar -> Upload de arquivo
4. Selecione o modelo xdp baixado na etapa 2

## Pré-visualização como HTML

O xdp pode ser visualizado no formato HTML5 ou PDF. Para pré-visualização do xdp no formato HTML5, siga as seguintes etapas

* Toque no xdp recém-carregado e clique em _Pré-visualização -> Pré-visualização como HTML_. Você deve ver o xdp renderizado como HTML5

>[!NOTE]
>Quando você seleciona a opção _Pré-visualização como PDF_ , o PDF renderizado não será exibido no navegador porque o AEM Forms renderiza pdf dinâmicos que exigem o plug-in do Acrobat.É necessário baixar o PDF e abri-lo usando o Adobe Acrobat/Reader para visualização


## Exibir com dados

Para pré-visualização do xdp no formato HTML5 com o arquivo de dados, siga as seguintes etapas:

* Toque no xdp recém-carregado e clique em _Pré-visualização -> Pré-visualização com dados_. Navegue e selecione o arquivo de dados e clique em _Pré-visualização_.
* Você deve ver o modelo renderizado no formato HTML5 pré-preenchido com os dados

## Explorar as propriedades avançadas do modelo xdp

As propriedades avançadas do modelo xdp permitem especificar a data de publicação, o manipulador de envio, o perfil de renderização para o formulário, o serviço de preenchimento prévio etc. Para visualização das propriedades avançadas do modelo, toque em xdp e clique em _Propriedades -> Avançadas_. Aqui você encontrará várias propriedades. Algumas dessas propriedades estão cobertas aqui.

**Enviar URL** - este é o URL que processará o envio do formulário HTML5. Abordaremos esta questão na próxima lição. Se um URL de envio não for especificado, o manipulador de envio padrão será chamado, retornando os dados do formulário para o navegador.

**PERFIL** de renderização HTML - os formulários HTML5 têm a noção de Perfis que são expostos como Pontos finais REST para permitir a renderização móvel de modelos de formulário. A maioria das vezes que o perfil de renderização padrão deve ser suficiente para renderizar o formulário. Se o perfil de renderização padrão não atender às suas necessidades, um perfil [](https://docs.adobe.com/content/help/en/experience-manager-64/forms/html5-forms/custom-profile.html) personalizado pode ser criado e associado ao formulário.

**Serviço** de preenchimento prévio - O serviço de preenchimento prévio geralmente é usado para preencher o formulário com dados obtidos de uma fonte de dados de backend.

