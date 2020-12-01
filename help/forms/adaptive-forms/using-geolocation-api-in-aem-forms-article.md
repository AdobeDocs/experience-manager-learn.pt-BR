---
title: Usando APIs de localização geográfica no Forms adaptável
seo-title: Usando APIs de localização geográfica no Forms adaptável
description: Preencha campos de endereço em seu formulário usando a API de geolocalização
seo-description: Preencha campos de endereço em seu formulário usando a API de geolocalização
uuid: 5a461659-6873-4ea1-9f37-8296e5a9d895
feature: adaptive-forms,
topics: integrations
audience: developer
doc-type: article
activity: develop
version: 6.3,6.4,6.5
discoiquuid: 3400251b-aee0-4d69-994b-e1643fabc868
translation-type: tm+mt
source-git-commit: e99779b5d42bb9a3b258e2bbe815defde9d40bf7
workflow-type: tm+mt
source-wordcount: '429'
ht-degree: 0%

---


# Usando APIs de localização geográfica em Forms adaptável{#using-geolocation-api-s-in-adaptive-forms}

Visite a página [AEM Forms samples](https://forms.enablementadobe.com/content/samples/samples.html?query=0) para obter um link para uma demonstração ao vivo desse recurso.

Neste artigo, vamos observar o uso da API de localização geográfica do Google para preencher campos de um Formulário adaptável. Esse caso de uso costuma ser usado quando você deseja preencher os campos de endereço atuais em um formulário.

As etapas a seguir foram seguidas para usar a API de localização geográfica no Adaptive Forms.

1. [Obtenha o API ](https://developers.google.com/maps/documentation/javascript/get-api-key) Keyfrom Google para usar a plataforma Google Maps. Você pode obter uma chave de avaliação válida por 1 ano.

1. O fragmento de formulário adaptativo foi criado com campos para manter o endereço atual

1. A API de localização geográfica foi invocada no evento click do objeto de imagem do Formulário adaptável

1. Os dados JSON retornados pela chamada de API foram analisados e os valores dos campos Formulário adaptável foram definidos de acordo.

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

![Campos preenchidos com API de geoloaction](assets/capture-4.gif)

Na linha 1, usamos a HTML Geolocation API para obter o local atual. Quando o local atual for obtido, passamos o local atual para a função showPosition.

Na função showPosition, usamos a API do Google para buscar os detalhes do endereço para a latitude e a longitude especificadas.

O JSON retornado pela API é analisado para definir os campos do Formulário adaptável.

>[!NOTE]
>
>Para fins de teste, você pode usar o protocolo HTTP com localhost no URL.
>
>Para o servidor de produção, será necessário habilitar o SSL para o servidor AEM para obter esse recurso.
>
>A amostra associada a este artigo foi testada com o endereço dos EUA. Se quiser usar esse recurso em outros locais geográficos, talvez seja necessário ajustar a análise JSON.

Para colocar esse recurso em seu servidor, siga as etapas a seguir

* Instale e start o servidor AEM Forms.

>!![NOTE] Esse recurso foi testado no AEM Forms 6.3 e superior
* [Obtenha a chave](https://developers.google.com/maps/documentation/javascript/get-api-key) da API do Google.
* [Importe os ativos relacionados a este artigo para AEM.](assets/geolocationapi.zip)
* [Abra o fragmento Formulário adaptável no modo de edição.](http://localhost:4502/editor.html/content/forms/af/currentaddressfragment.html)
* Abra o editor de regras para o componente de Escolha de imagem.
* Substitua a &lt;your_api_key> pela chave da API do Google.
* Salve as alterações.
* [Visualizar o formulário](http://localhost:4502/content/dam/formsanddocuments/currentaddressfragment/jcr:content?wcmmode=disabled).
* Clique no ícone &quot;geolocalização&quot;.
* Seu formulário deve ser preenchido com a localização atual.
