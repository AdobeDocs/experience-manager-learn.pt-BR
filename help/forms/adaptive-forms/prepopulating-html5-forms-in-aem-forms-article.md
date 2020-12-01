---
title: Preencha o HTML5 Forms usando o atributo de dados.
seo-title: Preencha o HTML5 Forms usando o atributo de dados.
description: Preencha formulários HTML5 buscando dados da fonte de backend.
seo-description: Preencha formulários HTML5 buscando dados da fonte de backend.
feature: integrations
topics: mobile-forms
audience: developer
doc-type: article
activity: implement
version: 6.3,6.4,6.5.
uuid: 889d2cd5-fcf2-4854-928b-0c2c0db9dbc2
discoiquuid: 3aa645c9-941e-4b27-a538-cca13574b21c
translation-type: tm+mt
source-git-commit: 1e615d1c51fa0c4c0db335607c29a8c284874c8d
workflow-type: tm+mt
source-wordcount: '510'
ht-degree: 0%

---


# Preencher HTML5 Forms usando o atributo de dados {#prepopulate-html-forms-using-data-attribute}

Visite a página [AEM Forms samples](https://forms.enablementadobe.com/content/samples/samples.html?query=0) para obter um link para uma demonstração ao vivo desse recurso.

Os modelos XDP renderizados no formato HTML usando o AEM Forms são chamados de HTML5 ou Mobile Forms. Um caso de uso comum é o preenchimento prévio desses formulários ao serem renderizados.

Há duas maneiras de unir dados ao modelo xdp quando ele está sendo renderizado como HTML.

**dataRef**: Você pode usar o parâmetro dataRef no URL. Esse parâmetro especifica o caminho absoluto do arquivo de dados que é unido ao modelo. Esse parâmetro pode ser um URL para um serviço de descanso que retorna os dados no formato XML.

**dados**: Esse parâmetro especifica os bytes de dados codificados UTF-8 que são unidos ao modelo. Se esse parâmetro for especificado, o formulário HTML5 ignorará o parâmetro dataRef. Como prática recomendada, recomendamos usar a abordagem de dados.

A abordagem recomendada é definir o atributo de dados na solicitação com os dados com os quais você deseja pré-preencher o formulário.

slingRequest.setAttribute(&quot;data&quot;, conteúdo);

Neste exemplo, estamos configurando o atributo de dados com o conteúdo. O conteúdo representa os dados com os quais você deseja pré-preencher o formulário. Normalmente, você buscaria o &quot;conteúdo&quot; fazendo uma chamada REST para um serviço interno.

Para obter esse caso de uso, é necessário criar um perfil personalizado. Os detalhes sobre a criação de perfis personalizados estão claramente documentados na [documentação da AEM Forms aqui](https://helpx.adobe.com/aem-forms/6/html5-forms/custom-profile.html).

Depois de criar o perfil personalizado, você criará um arquivo JSP que buscará os dados fazendo chamadas para o sistema de backend. Quando os dados forem obtidos, você usará slingRequest.setAttribute(&quot;data&quot;, content); para preencher previamente o formulário

Quando o XDP está sendo renderizado, você também pode passar alguns parâmetros para o xdp e, com base no valor do parâmetro, pode buscar os dados do sistema de backend.

[Por exemplo, este url tem parâmetro name](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=john)

O JSP que você gravar terá acesso ao parâmetro name por meio do request.getParameter(&quot;name&quot;). Você pode passar o valor desse parâmetro para o processo de backend para buscar os dados necessários.
Para que esse recurso funcione no seu sistema, siga as etapas mencionadas abaixo:

* [Baixe e importe os ativos para AEM usando o ](assets/prepopulatemobileform.zip)
gerenciador de pacotesO pacote instalará o seguinte

   * CustomProfile
   * XDP de exemplo
   * Amostra de ponto de extremidade POST que retornará dados para preencher o formulário

>[!NOTE]
>
>Se você quiser preencher seu formulário chamando o processo de análise de big data, inclua callWorkbenchProcess.jsp em seu /apps/AEMFormsDemoListings/customprofiles/PrepopulateForm/html.jsp em vez de set.jsp

* [Aponte seu navegador favorito para este url](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=Adobe%20Systems). O formulário deve ser preenchido previamente com o valor do parâmetro name
