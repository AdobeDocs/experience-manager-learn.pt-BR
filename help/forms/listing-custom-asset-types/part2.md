---
title: Listagem de tipos de ativos personalizados no AEM Forms
description: Parte 2 da lista de tipos de ativos personalizados no AEM Forms
feature: Adaptive Forms
doc-type: Tutorial
version: 6.4,6.5
discoiquuid: 4b940465-0bd7-45a2-8d01-e4d640c9aedf
topic: Development
role: Developer
level: Experienced
exl-id: f221d8ee-0452-4690-a936-74bab506d7ca
last-substantial-update: 2019-07-10T00:00:00Z
duration: 163
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '584'
ht-degree: 0%

---

# Listagem de tipos de ativos personalizados no AEM Forms {#listing-custom-asset-types-in-aem-forms}

## Criação de modelo personalizado {#creating-custom-template}

Para os fins deste artigo, estamos criando um modelo personalizado para exibir os tipos de ativos personalizados e os tipos de ativos OOTB na mesma página. Para criar um modelo personalizado, siga as instruções a seguir

1. Crie uma pasta sling: em /apps. Nomeie-o como &quot;myportalcomponent&quot;
1. Adicione uma propriedade &quot;fpContentType&quot;. Defina o valor como &quot;**/libs/fd/ fp/formTemplate&quot;.**
1. Adicione uma propriedade &quot;title&quot; e defina seu valor como &quot;template personalizado&quot;. Esse é o nome que você verá na lista suspensa do componente de Pesquisa e listagem
1. Crie um &quot;template.html&quot; nesta pasta. Esse arquivo manterá o código no estilo e exibirá os vários tipos de ativos.

![appsfolder](assets/appsfolder_.png)

O código a seguir lista os vários tipos de ativos que usam o componente de pesquisa e lista. Criamos elementos html separados para cada tipo de ativo, como mostrado pela tag data-type = &quot;videos&quot;. Para o tipo de ativo de &quot;vídeos&quot;, usamos o &lt;video> elemento para reproduzir o vídeo em linha. Para o tipo de ativo de &quot;documentos do Word&quot;, usamos diferentes marcações html.

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
>Para listar o Adaptive Forms neste modelo, crie uma nova div e defina seu atributo de tipo de dados como &quot;guide&quot;. Você pode copiar e colar o div cujos dados-type=&quot;printForm&quot; e definir o tipo de dados do div recém-copiado como &quot;guia&quot;

## Configurar Componente De Pesquisa E Lister {#configure-search-and-lister-component}

Depois de definir o modelo personalizado, precisamos associá-lo ao componente &quot;Pesquisa e Lister&quot;. Aponte seu navegador [para este url](http://localhost:4502/editor.html/content/AemForms/CustomPortal.html).

Alterne para o modo Design e configure o sistema de parágrafo para incluir o componente Pesquisa e Lister no grupo de componentes permitidos. O componente de Pesquisa e Lister faz parte do grupo de Serviços de documento.

Alterne para o modo de edição e adicione o componente de Pesquisa e Lister ao ParSys.

Abra as propriedades de configuração do componente &quot;Pesquisa e Lister&quot;. Verifique se a guia &quot;Pastas de ativos&quot; está selecionada. Selecione as pastas das quais deseja listar os ativos no componente de pesquisa e lista. Para os fins deste artigo, selecionei

* /content/dam/VideosAndWordDocuments
* /content/dam/formsanddocuments/assettypes

![assetfolder](assets/selectingassetfolders.png)

Vá até a guia &quot;Exibir&quot;. Aqui, você escolherá o modelo em que deseja exibir os ativos no componente de pesquisa e lista.

Selecione &quot;modelo personalizado&quot; no menu suspenso, como mostrado abaixo.

![searchandlister](assets/searchandlistercomponent.gif)

Configure os tipos de ativos que você deseja listar no portal. Para configurar os tipos de guia do ativo para a &quot;Listagem de ativos&quot; e configurar os tipos de ativos. Neste exemplo, configuramos os seguintes tipos de ativos

1. Arquivos MP4
1. Documentos do Word
1. Documento(Este é um tipo de ativo OOTB)
1. Modelo de formulário (este é o tipo de ativo OOTB)

A captura de tela a seguir mostra os tipos de ativos configurados para listagem

![assettypes](assets/assettypes.png)

Agora que você configurou o componente de Portal de Pesquisa e Lister, é hora de ver a lista em ação. Aponte seu navegador [para este url](http://localhost:4502/content/AemForms/CustomPortal.html?wcmmode=disabled). Os resultados devem ser algo como a imagem mostrada abaixo.

>[!NOTE]
>
>Se o portal estiver listando tipos de ativos personalizados em um servidor de publicação, certifique-se de fornecer permissão de &quot;leitura&quot; ao usuário &quot;fd-service&quot; para o nó **/apps/fd/fp/extensions/querybuilder**

![assettypes](assets/assettypeslistings.png)
[Baixe e instale este pacote usando o gerenciador de pacotes.](assets/customassettypekt1.zip) Ele contém documentos mp4 de amostra e do word, e arquivos xdp usados como tipos de ativos para listar usando o componente de pesquisa e lista
