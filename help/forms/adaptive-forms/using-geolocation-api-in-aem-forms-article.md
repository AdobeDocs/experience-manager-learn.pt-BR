---
title: Uso de APIs de geolocalização no Forms adaptável
description: Preencha campos de endereço no seu formulário usando as APIs de localização geográfica
feature: Adaptive Forms
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
exl-id: 50db6155-ee83-4ddb-9e3a-56e8709222db
last-substantial-update: 2020-03-20T00:00:00Z
duration: 127
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '366'
ht-degree: 0%

---

# Uso de APIs de geolocalização no Forms adaptável{#using-geolocation-api-s-in-adaptive-forms}

Neste artigo, analisaremos o uso da API de geolocalização do Google para preencher campos de um formulário adaptável. Esse caso de uso geralmente é usado quando você deseja preencher os campos de endereço atuais em um formulário.

As etapas a seguir foram seguidas para usar a API de geolocalização no Adaptive Forms.

1. [Obter chave de API](https://developers.google.com/maps/documentation/javascript/get-api-key) do Google para usar a plataforma Google Maps. Você pode obter uma chave de avaliação válida por 1 ano.

1. O fragmento de formulário adaptável foi criado com campos para manter o endereço atual

1. A API de geolocalização foi invocada no evento de clique do objeto de imagem do Formulário adaptável

1. Os dados JSON retornados pela chamada à API foram analisados e os valores dos campos de Formulário adaptável foram definidos de acordo.

```javascript
navigator.geolocation.getCurrentPosition(showPosition);
function showPosition(position) 
{
console.log(" I am inside the showPosition in fragment");
console.log("Latitude: " + position.coords.latitude + "Longitude " + position.coords.longitude);
var url = "https://maps.googleapis.com/maps/api/geocode/json?latlng="+position.coords.latitude+","+position.coords.longitude+"&key=<your_api_key>";
  console.log(url);
  
  $.getJSON(url,function (data, textStatus){
    
    var location=data.results[0].formatted_address;
    console.log(location);
    
    for(i=0;i<data.results[0].address_components.length;i++)
        {
          if(data.results[0].address_components[i].types[0] == "street_number")
            {
              streetNumber.value = data.results[0].address_components[i].long_name;
            }
          if(data.results[0].address_components[i].types[0] == "route")
            {
              streetName.value = data.results[0].address_components[i].long_name;
            }
            if(data.results[0].address_components[i].types[0] == "postal_code")
            {
              
              zipCode.value = data.results[0].address_components[i].long_name;
            }
            if(data.results[0].address_components[i].types[0] == "locality")
            {
              
              city.value = data.results[0].address_components[i].long_name;
            }
          if(data.results[0].address_components[i].types[0] == "administrative_area_level_1")
            {
              
              state.value = data.results[0].address_components[i].long_name;
            }
        }
    
  });
}
```

![Campos preenchidos com a API de geolocalização](assets/capture-4.gif)

Na linha 1, usamos a API de geolocalização do HTML para obter a localização atual. Depois que a localização atual é obtida, passamos a localização atual para a função showPosition.

Na função showPosition, usamos a API do Google para buscar os detalhes do endereço para a latitude e a longitude fornecidas.

O JSON retornado pela API é analisado para definir os campos do Formulário adaptável.

>[!NOTE]
>
>Para fins de teste, você pode usar o protocolo HTTP com localhost no URL.
>
>Para o servidor de produção, será necessário habilitar o SSL para o servidor AEM para obter esse recurso.
>
>A amostra associada a este artigo foi testada com o endereço dos EUA. Se você quiser usar esse recurso em outras localizações geográficas, talvez seja necessário ajustar a análise JSON.

Para colocar esse recurso no servidor, siga as etapas a seguir

* Instale e inicie o servidor do AEM Forms.
> Esse recurso foi testado no AEM Forms 6.3 e posterior
* [Obter a chave de API do Google](https://developers.google.com/maps/documentation/javascript/get-api-key).
* [Importe os ativos relacionados a este artigo para o AEM.](assets/geolocationapi.zip)
* [Abra o fragmento do Formulário adaptável no modo de edição.](http://localhost:4502/editor.html/content/forms/af/currentaddressfragment.html)
* Abra o editor de regras para o componente Opção de imagem.
* Substitua o &lt;your_api_key> com a chave de API do Google.
* Salve as alterações.
* [Visualizar o formulário](http://localhost:4502/content/dam/formsanddocuments/currentaddressfragment/jcr:content?wcmmode=disabled).
* Clique no ícone &quot;geolocalização&quot;.
* O formulário deve ser preenchido com a localização atual.
