---
title: Listando tipos de ativos personalizados no AEM Forms
seo-title: Listando tipos de ativos personalizados no AEM Forms
description: Parte 2 da listagem de tipos de ativos personalizados no AEM Forms
seo-description: Parte 2 da listagem de tipos de ativos personalizados no AEM Forms
uuid: 6467ec34-e452-4c21-9bb5-504f9630466a
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
discoiquuid: 4b940465-0bd7-45a2-8d01-e4d640c9aedf
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '611'
ht-degree: 0%

---


# Listando tipos de ativos personalizados no AEM Forms {#listing-custom-asset-types-in-aem-forms}

## Criando modelo personalizado {#creating-custom-template}


Para a finalidade deste artigo, estaremos criando um modelo personalizado para exibir os tipos de ativos personalizados e os tipos de ativos OOTB na mesma página. Para criar um modelo personalizado, siga as instruções a seguir

1. Criar um sling: em /apps. Nomeie-o como &quot; myportalcomponent &quot;
1. Adicione uma propriedade &quot;fpContentType&quot;. Defina seu valor como &quot;**/libs/fd/ fp/formTemplate&quot;.**
1. Adicione uma propriedade &quot;title&quot; e defina seu valor para &quot;custom template&quot;. Esse é o nome que você verá na lista suspensa do Componente de pesquisa e lister
1. Crie um &quot;template.html&quot; nesta pasta. Esse arquivo manterá o código com estilo e exibirá os vários tipos de ativos.

![appsfolder](assets/appsfolder_.png)

O código a seguir lista os vários tipos de ativos usando o componente de pesquisa e lister. Criamos elementos html separados para cada tipo de ativo, conforme mostrado pela tag data-type = &quot;videos&quot;. Para o tipo de ativo de &quot;vídeos&quot;, usamos o elemento &lt;vídeo> para reproduzir o vídeo em linha. Para o tipo de ativo de &quot;documentos do Word&quot;, usamos uma marcação html diferente.

```html
<div class="__FP_boxes-container __FP_single-color">
   <div  data-repeatable="true">
     <div class = "__FP_boxes-thumbnail" style="float:left;margin-right:20px;" data-type = "videos">
   <video width="400" controls>
       <source src="${path}" type="video/mp4">
    </video>
         <h3 class="__FP_single-color" title="${name}" tabindex="0">${name}</h3>
     </div>
     <div class="__FP_boxes-thumbnail" style="float:left;margin-right:20px;" data-type = "worddocuments">
       <a href="/assetdetails.html${path}" target="_blank">
           <img src ="/content/dam/worddocuments/worddocument.png/jcr:content/renditions/cq5dam.thumbnail.319.319.png"/>
          </a>
          <h3 class="__FP_single-color" title="${name}" tabindex="0">${name}</h3>
     </div>
  <div class="__FP_boxes-thumbnail" style="float:left;margin-right:20px;" data-type = "xfaForm">
       <a href="/assetdetails.html${path}" target="_blank">
           <img src ="${path}/jcr:content/renditions/cq5dam.thumbnail.319.319.png"/>
          </a>
          <h3 class="__FP_single-color" title="${name}" tabindex="0">${name}</h3>
                <a href="{formUrl}"><img src="/content/dam/html5.png"></a><p>

     </div>
  <div class="__FP_boxes-thumbnail" style="float:left;margin-right:20px;" data-type = "printForm">
       <a href="/assetdetails.html${path}" target="_blank">
           <img src ="${path}/jcr:content/renditions/cq5dam.thumbnail.319.319.png"/>
          </a>
          <h3 class="__FP_single-color" title="${name}" tabindex="0">${name}</h3>
                <a href="{pdfUrl}"><img src="/content/dam/pdf.png"></a><p>
     </div>
   </div>
</div>
```

>[!NOTE]
>
>Linha 11 - Altere a imagem src para apontar para uma imagem de sua escolha no DAM.
>
>Para lista do Forms adaptável neste modelo, crie um novo div e defina seu atributo de tipo de dados como &quot;guia&quot;. É possível copiar e colar o div cujo data-type=&quot;printForm e definir o tipo de dados div copiado recentemente como &quot;guide&quot;

## Configurar Componente de Pesquisa e Lister {#configure-search-and-lister-component}

Depois de definir o modelo personalizado, agora temos que associar esse modelo personalizado ao componente &quot;Pesquisar e Lister&quot;. Aponte seu navegador [para este url ](http://localhost:4502/editor.html/content/AemForms/CustomPortal.html).

Alterne para o modo Design e configure o sistema de parágrafo para incluir o componente de Pesquisa e Lister no grupo de componentes permitidos. O componente de Pesquisa e Lister faz parte do grupo Serviços de Documento.

Alterne para o modo de edição e adicione o componente de Pesquisa e Lister ao ParSys.

Abra as propriedades de configuração do componente &quot;Pesquisar e Lister&quot;. Verifique se a guia &quot;Pastas de ativos&quot; está selecionada. Selecione as pastas das quais deseja lista os ativos no componente de pesquisa e lister. Para a finalidade deste artigo, selecionei

* /content/dam/VideosAndWordDocuments
* /content/dam/formsanddocuments/assettypes

![assetfolder](assets/selectingassetfolders.png)

Pressione para a guia &quot;Exibir&quot;. Aqui, você escolherá o modelo que deseja exibir os ativos no componente de pesquisa e lister.

Selecione &quot;modelo personalizado&quot; na lista suspensa, como mostrado abaixo.

![pesquisador](assets/searchandlistercomponent.gif)

Configure os tipos de ativos que você deseja lista no portal. Para configurar os tipos de guia do ativo para &quot;Lista de ativos&quot; e configurar os tipos de ativos. Neste exemplo, configuramos os seguintes tipos de ativos

1. Arquivos MP4
1. Documentos do Word
1. Documento (este é o tipo de ativo OOTB)
1. Modelo de formulário (este é o tipo de ativo OOTB)

A seguinte captura de tela mostra os tipos de ativos configurados para listagem

![assettypes](assets/assettypes.png)

Agora que você configurou seu Componente do Portal de Pesquisa e Lister, é hora de ver o lister em ação. Aponte seu navegador [para este url ](http://localhost:4502/content/AemForms/CustomPortal.html?wcmmode=disabled). Os resultados devem ser algo como a imagem mostrada abaixo.

>[!NOTE]
>
>Se seu portal estiver listando tipos de ativos personalizados em um servidor de publicação, certifique-se de conceder permissão de &quot;leitura&quot; para o usuário &quot;fd-service&quot; ao nó **/apps/fd/fp/extensions/querybuilder**

![](assets/assettypeslistings.png)
[tipos de ativosBaixe e instale este pacote usando o gerenciador de pacotes.](assets/customassettypekt1.zip) Ele contém documentos mp4 e word de amostra e arquivos xdp que serão usados como tipos de ativos para a lista usando o componente de pesquisa e lister
