---
title: Criar formulários HTML5
description: Criar e configurar formulários HTML5
feature: formulários móveis
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 4419
thumbnail: kt-4419.jpg
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '486'
ht-degree: 4%

---


# Criar formulários HTML5

Os formulários HTML5 são um novo recurso no Adobe Experience Manager que oferece a renderização de modelos de formulário XFA (xdp) no formato HTML5. Esse recurso permite a renderização de formulários em navegadores para dispositivos móveis e desktop, nos quais o PDF com base em XFA não ofereça suporte. Os formulários HTML5 não só oferecem suporte aos recursos existentes de modelos de formulário XFA, como também adicionam novos recursos, como assinatura de rabisco, para dispositivos móveis.

## Pré-requisitos

Verifique se você tem uma instância de trabalho do AEM Forms. Siga o [guia de instalação](https://docs.adobe.com/content/help/en/experience-manager-65/forms/install-aem-forms/osgi-installation/installing-configuring-aem-forms-osgi.html) para instalar e configurar o AEM Forms

## Crie seu primeiro formulário HTML5

1. [Baixe e extraia o conteúdo do arquivo](assets/assets.zip) zip. O arquivo zip contém xdp e arquivo de dados
2. [Navegar até Formulários e Documentos](http://localhost:4502/aem/forms.html/content/dam/formsanddocuments)
3. Clique em Criar -> Upload de arquivo
4. Selecione o modelo xdp baixado na etapa 2

## Visualizar como HTML

O xdp pode ser visualizado no formato HTML5 ou PDF. Para visualizar o xdp no formato HTML5, siga as seguintes etapas

* Toque no xdp recém-carregado e clique em _Preview -> Preview como HTML_. Você deve ver o xdp renderizado como HTML5

>[!NOTE]
>Ao selecionar a opção _Visualizar como PDF_, o PDF renderizado não será exibido no navegador porque o AEM Forms renderiza pdf dinâmicos que exigem plug-in do Acrobat.Será necessário baixar o PDF e abri-lo usando o Adobe Acrobat/Reader para exibir


## Exibir com dados

Para visualizar o xdp no formato HTML5 com o arquivo de dados, siga as seguintes etapas:

* Toque no xdp recém-carregado e clique em _Visualizar -> Visualizar com dados_. Navegue e selecione o arquivo de dados e clique em _Visualizar_.
* Você deve ver o modelo renderizado no formato HTML5 pré-preenchido com os dados

## Explorar as propriedades avançadas do modelo xdp

As propriedades avançadas do modelo xdp permitem especificar a data de publicação, o manipulador de envio, o perfil de renderização do formulário, o serviço de preenchimento prévio etc. Para exibir as propriedades avançadas do modelo, toque no xdp e clique em _properties -> Avançado_. Aqui você encontrará várias propriedades do . Algumas dessas propriedades são abordadas aqui.

**Enviar URL**  - esse é o URL que processará o envio do formulário HTML5. Abordaremos esta questão na próxima lição. Se um URL de envio não for especificado, o manipulador de envio padrão será chamado, retornando os dados do formulário ao navegador.

**Perfil de renderização HTML**  - Os formulários HTML5 têm a noção de Perfis expostos como Pontos de extremidade REST para permitir a renderização móvel de modelos de formulário. A maioria das vezes o perfil de renderização padrão deve ser suficiente para renderizar o formulário. Se o perfil de renderização padrão não atender às suas necessidades, um [perfil personalizado](https://docs.adobe.com/content/help/en/experience-manager-64/forms/html5-forms/custom-profile.html) poderá ser criado e associado ao formulário.

**Serviço de preenchimento prévio**  - O serviço de preenchimento prévio geralmente é usado para preencher seu formulário com dados obtidos de uma fonte de dados de back-end.

