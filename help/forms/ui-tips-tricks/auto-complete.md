---
title: Recurso de preenchimento automático no AEM Forms
description: Permite que os usuários localizem e selecionem rapidamente a partir de uma lista pré-preenchida de valores à medida que digitam, aproveitando a pesquisa e a filtragem.
feature: Adaptive Forms
type: Tutorial
version: 6.5
topic: Development
role: Developer
level: Beginner
kt: 11374
last-substantial-update: 2022-11-01T00:00:00Z
source-git-commit: 9229a92a0d33c49526d10362ac4a5f14823294ed
workflow-type: tm+mt
source-wordcount: '183'
ht-degree: 0%

---

# Implementação automática concluída

Implemente o recurso de preenchimento automático em formulários AEM usando o recurso de preenchimento automático do jquery.
A amostra incluída neste artigo usa uma variedade de fontes de dados (matriz estática, matriz dinâmica preenchida a partir de uma resposta REST API) para preencher as sugestões à medida que o usuário começa a digitar no campo de texto.

O código usado para realizar o recurso de preenchimento automático está associado ao evento initialize do campo.


## Fornecer sugestões para o nome do país

![sugestões de país](assets/auto-complete1.png)

## Fornecer sugestão para endereço

![sugestões de país](assets/auto-complete2.png)

Este é o código usado para fornecer sugestões de endereço de rua

```javascript
$(".streetAddress input").autocomplete({
    source: function(request, response) {
        $.ajax({
            url: "https://api.geoapify.com/v1/geocode/autocomplete?text=" + request.term + "&apiKey=Your API Key", //please get your own API key with geoapify.com
            responseType: "application/json",
            success: function(data) {
                console.log(data.features.length);
                response($.map(data.features, function(item) {
                    return {
                        label: [item.properties.formatted],
                        value: [item.properties.formatted]
                    };
                }));
            },
        });
    },
    minLength: 5,
    select: function(event, ui) {
        console.log(ui.item ?
            "Selected: " + ui.item.label :
            "Nothing selected, input was " + this.value);
    }

});
```

## Sugestões para emoji

![sugestões de país](assets/auto-complete3.png)

O código a seguir foi usado para exibir emojis na lista de sugestões

```javascript
var values=["Wolf \u{1F98A}", "Lion \u{1F981}","Puppy \u{1F436}","Giraffe \u{1F992}","Frog \u{1F438}"];
$(".Animals input").autocomplete( {
minLength: 1, source: values, delay: 0
}

);
```

O [é possível baixar o formulário de amostra](assets/auto-complete-form.zip) daqui. Certifique-se de fornecer seu próprio nome de usuário/chave de API usando o editor de código para o código para fazer chamadas REST bem-sucedidas.

>[!NOTE]
>
> Para que o preenchimento automático funcione, verifique se seu formulário usa a seguinte biblioteca do cliente **cq.jquery.ui**. Essa biblioteca do cliente vem com AEM.
