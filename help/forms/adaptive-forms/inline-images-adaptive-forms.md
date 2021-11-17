---
title: Exibição de imagens em linha no Adaptive Forms
description: Exibir imagens carregadas em linha no Adaptive Forms
feature: Adaptive Forms
topics: development
version: 6.3,6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 4a69513d-992c-435a-a520-feb9085820e7
source-git-commit: 9529b1f6d1a863fc570822c8ecd6c4be01b36729
workflow-type: tm+mt
source-wordcount: '0'
ht-degree: 0%

---

# Imagens em linha no Adaptive Forms

Um caso de uso comum é exibir a imagem carregada como uma imagem embutida no Formulário adaptável. Por padrão, a imagem carregada é mostrada como um link e essa experiência pode ser aprimorada ao exibir a imagem no formulário adaptável. Este artigo o guiará pelas etapas envolvidas na exibição da imagem em linha.

## Adicionar imagem de espaço reservado

A primeira etapa é anexar um espaço reservado div ao componente de anexo de arquivo. No código abaixo, o componente de anexo de arquivo é identificado por seu nome de classe CSS de carregamento de foto. A função JavaScript faz parte da biblioteca do cliente associada aos formulários adaptáveis. Essa função é chamada no evento initialize do componente de anexo de arquivo.

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

Após o usuário ter carregado a imagem, a função listada abaixo é chamada no evento commit do componente de anexo de arquivo. A função recebe o objeto de arquivo carregado como argumento.

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

### Implantar no servidor

* Baixe e instale o [biblioteca do cliente](assets/inline-image-client-library.zip) na sua instância AEM usando AEM gerenciador de pacotes.
* Baixe e instale o [formulário de amostra](assets/inline-image-af.zip) em sua instância do AEM usando AEM gerenciador de pacotes.
* Aponte seu navegador para [Adicionar imagem em linha](http://localhost:4502/content/dam/formsanddocuments/addinlineimage/jcr:content?wcmmode=disabled)
* Clique no botão &quot;Anexar sua foto&quot; para adicionar a imagem
