---
title: Exibição de imagens do DAM em linha no Adaptive Forms
description: Exibir imagens DAM em linha no Adaptive Forms
feature: Adaptive Forms
topics: development
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
last-substantial-update: 2022-10-20T00:00:00Z
thumbnail: inline-dam.jpg
kt: kt-11307
exl-id: 339eb16e-8ad8-4b98-939c-b4b5fd04d67e
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '210'
ht-degree: 0%

---

# Exibir imagem DAM no Forms adaptável

Um caso de uso comum é exibir as imagens que residem no repositório crx em linha em um Formulário adaptável.

## Adicionar imagem de espaço reservado

A primeira etapa é anexar um espaço reservado div ao componente do painel. No código abaixo, o componente do painel é identificado pelo seu nome de classe CSS de upload de foto. A função JavaScript faz parte da biblioteca do cliente associada aos formulários adaptáveis. Esta função é chamada no evento de inicialização do componente de anexo de arquivo.

```javascript
/**
* Add Placeholder Div
*/
function addPlaceholderDiv(){

     $(".photo-upload").prepend(" <div class='preview'' style='border:1px dotted;height:225px;width:175px;text-align:center'><br><br><div class='text'>The Image will appear here</div></div><br>");
}
```

### Exibir imagem em linha

Depois que o usuário seleciona a imagem, o campo oculto ImageName é preenchido com o nome da imagem selecionada. Esse nome de imagem é então passado para a função damURLToFile que invoca a função createFile para converter um URL em um Blob para FileReader.readAsDataURL().

```javascript
/**
* DAM URL to File Object
* @return {string} 
 */
 function damURLToFile (imageName) {
   console.log("The image selected is "+imageName);
     createFile(imageName);
}
```

```javascript
async function createFile(imageName){
  let response = await fetch('/content/dam/formsanddocuments/fieldinspection/images/'+imageName);
  let data = await response.blob();
    console.log(data);
  let metadata = {
    type: 'image/jpeg'
  };
  let file = new File([data], "test.jpg", metadata);
     let reader = new FileReader();
    reader.readAsDataURL(file);
     reader.onload = function() {
    console.log("finished reading ...."+reader.result);
    
  };
    var image  = new Image();
    $(".photo-upload .preview .imageP").remove();
    $(".photo-upload .preview .text").remove();
    image.width = 484;image.height = 334;
    image.className = "imageP";
    image.addEventListener("load", function () {
      $(".photo-upload .preview")[0].prepend(this);
    });
    
    console.log(window.URL.createObjectURL(file));
    image.src = window.URL.createObjectURL(file);

  }
```

### Implantar no servidor

* Baixe e instale o [biblioteca do cliente e imagens de amostra](assets/InlineDAMImage.zip) na sua instância do AEM usando o Gerenciador de pacotes do AEM.
* Baixe e instale o [exemplo de formulário](assets/FieldInspectionForm.zip) em sua instância do AEM usando o gerenciador de pacotes AEM.
* Aponte seu navegador para [FormulárioDeInspeçãoDeCampo](http://localhost:4502/content/dam/formsanddocuments/fieldinspection/fieldinspection/jcr:content?wcmmode=disabled)
* Selecione uma das opções de fixação
* Você deve ver a imagem exibida no formulário
