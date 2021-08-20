---
title: Preencha previamente o HTML5 Forms usando o atributo de dados.
description: Preencha formulários HTML5 buscando dados da fonte de backend.
feature: Formulários adaptáveis
version: 6.3,6.4,6.5.
topic: Desenvolvimento
role: Developer
level: Experienced
source-git-commit: 462417d384c4aa5d99110f1b8dadd165ea9b2a49
workflow-type: tm+mt
source-wordcount: '497'
ht-degree: 0%

---


# Preencha previamente o Forms HTML5 usando o atributo de dados {#prepopulate-html-forms-using-data-attribute}

Visite a página [AEM Forms samples](https://forms.enablementadobe.com/content/samples/samples.html?query=0) para obter um link para uma demonstração ao vivo desse recurso.

Os modelos XDP renderizados no formato HTML usando o AEM Forms são chamados de HTML5 ou Forms móvel. Um caso de uso comum é preencher previamente esses formulários ao serem renderizados.

Há 2 maneiras de mesclar dados com o modelo xdp quando ele é renderizado como HTML.

**dataRef**: Você pode usar o parâmetro dataRef no URL. Esse parâmetro especifica o caminho absoluto do arquivo de dados que é unido ao modelo. Esse parâmetro pode ser um URL para um serviço restante que retorna os dados no formato XML.

**dados**: Esse parâmetro especifica os bytes de dados codificados UTF-8 que são mesclados com o modelo. Se esse parâmetro for especificado, o formulário HTML5 ignorará o parâmetro dataRef . Como prática recomendada, recomendamos usar a abordagem de dados.

A abordagem recomendada é definir o atributo de dados na solicitação com os dados com os quais você deseja preencher previamente o formulário.

slingRequest.setAttribute(&quot;data&quot;, conteúdo);

Neste exemplo, estamos configurando o atributo de dados com o conteúdo. O conteúdo representa os dados com os quais você deseja preencher previamente o formulário. Normalmente, você buscaria o &quot;conteúdo&quot; fazendo uma chamada REST para um serviço interno.

Para obter esse caso de uso, você precisa criar um perfil personalizado. Os detalhes sobre a criação de perfil personalizado estão claramente documentados na [documentação do AEM Forms aqui](https://helpx.adobe.com/aem-forms/6/html5-forms/custom-profile.html).

Depois de criar o perfil personalizado, você criará um arquivo JSP que buscará os dados fazendo chamadas para o sistema de back-end. Depois que os dados forem obtidos, você usará o slingRequest.setAttribute(&quot;data&quot;, content); para preencher previamente o formulário

Quando o XDP está sendo renderizado, você também pode transmitir alguns parâmetros para o xdp e, com base no valor do parâmetro, buscar os dados do sistema de back-end.

[Por exemplo, este url tem parâmetro de nome](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=john)

O JSP gravado terá acesso ao parâmetro name por meio do request.getParameter(&quot;name&quot;) . Em seguida, você pode passar o valor desse parâmetro para o processo de back-end para buscar os dados necessários.
Para que esse recurso funcione em seu sistema, siga as etapas mencionadas abaixo:

* [Baixe e importe os ativos no AEM usando o ](assets/prepopulatemobileform.zip)
gerenciador de pacotes. O pacote instalará o seguinte

   * CustomProfile
   * Exemplo de XDP
   * Ponto de extremidade POST de exemplo que retornará dados para preencher o formulário

>[!NOTE]
>
>Se você quiser preencher seu formulário chamando o processo do workbench, convém incluir o callWorkbenchProcess.jsp em seu /apps/AEMFormsDemoListings/customprofiles/PrepopulateForm/html.jsp em vez do setdata.jsp

* [Aponte seu navegador favorito para este url](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=Adobe%20Systems). O formulário deve ser preenchido previamente com o valor do parâmetro name
