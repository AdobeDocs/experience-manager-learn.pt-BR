---
title: Exibição de imagens em linha no Forms adaptável
seo-title: Exibição de imagens em linha no Forms adaptável
description: Exibir imagens carregadas em linha no Forms adaptável
seo-description: Exibir imagens carregadas em linha no Forms adaptável
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: defefc1451e2873e81cd81e3cccafa438aa062e3
workflow-type: tm+mt
source-wordcount: '238'
ht-degree: 0%

---


# Imagens em linha no Forms adaptável

Um caso de uso comum é exibir a imagem carregada como uma imagem embutida no Formulário adaptável. Por padrão, a imagem carregada é mostrada como um link e essa experiência pode ser aprimorada exibindo a imagem em Formulário adaptável. Este artigo o guiará pelas etapas envolvidas na exibição da imagem em linha.

## Adicionar imagem de espaço reservado

A primeira etapa é anexar um espaço reservado div ao componente de anexo do arquivo. No código abaixo, o componente de anexo de arquivo é identificado pelo nome de classe CSS do upload de foto. A função JavaScript faz parte da biblioteca do cliente associada aos formulários adaptáveis. Essa função é chamada no evento initialize do componente de anexo de arquivo.

```javascript
/**
* Add Placeholder Image
* @return {string} 
 */
function addTempImage(){
  $(".photo-upload").prepend(" <div class='preview'' style='border:2px solid;height:225px;width:175px;text-align:center'><br><br><div class='text'>3.5mm * 4.5mm<br>2Mb max<br>Min 600dpi</div></div><br>");

}
```

### Exibir imagem em linha

Depois que o usuário carregou a imagem, a função listada abaixo é chamada no evento de confirmação do componente de anexo do arquivo. A função recebe o objeto de arquivo carregado como argumento.

```javascript
/**
* Consume Image
* @return {string} 
 */
function consumeImage (file) {
  var reader = new FileReader();
    console.log("Reading file");
  reader.addEventListener("load", function (e) {
    console.log("in the event listener");
    var image  = new Image();
    $(".photo-upload .preview .imageP").remove();
    $(".photo-upload .preview .text").remove();
    image.width = 170;image.height = 220;
    image.className = "imageP";
    image.addEventListener("load", function () {
      $(".photo-upload .preview")[0].prepend(this);
    });
    image.src = window.URL.createObjectURL(file);
  });
  reader.readAsDataURL(file); 
}
```

### Implantar em seu servidor

* Baixe e instale a biblioteca [do](assets/inline-image-client-library.zip) cliente em sua instância AEM usando AEM gerenciador de pacote.
* Baixe e instale o formulário [de](assets/inline-image-af.zip) amostra em sua instância AEM usando AEM gerenciador de pacote.
* Aponte seu navegador para [Adicionar imagem em linha](http://localhost:4502/content/dam/formsanddocuments/addinlineimage/jcr:content?wcmmode=disabled)
* Clique no botão &quot;Anexar sua foto&quot; para adicionar uma imagem