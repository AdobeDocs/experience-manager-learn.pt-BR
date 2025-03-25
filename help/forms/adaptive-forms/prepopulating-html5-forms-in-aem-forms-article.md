---
title: Preencha previamente o HTML5 Forms usando o atributo de dados.
description: Preencha os formulários do HTML5 buscando dados da fonte de back-end.
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: ab0f5282-383b-4be6-9c57-cded6ab37528
last-substantial-update: 2020-01-09T00:00:00Z
duration: 94
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '449'
ht-degree: 0%

---

# Preencher previamente o HTML5 Forms usando o atributo de dados {#prepopulate-html-forms-using-data-attribute}


Os modelos XDP renderizados no formato HTML usando o AEM Forms são chamados de HTML5 ou Forms móvel. Um caso de uso comum é preencher previamente esses formulários quando eles estiverem sendo renderizados.

Há duas maneiras de mesclar dados com o modelo xdp quando ele estiver sendo renderizado como HTML.

**dataRef**: você pode usar o parâmetro dataRef na URL. Esse parâmetro especifica o caminho absoluto do arquivo de dados que é mesclado com o modelo. Esse parâmetro pode ser um URL para um serviço rest que retorna os dados no formato XML.

**dados**: este parâmetro especifica os bytes de dados codificados em UTF-8 que são mesclados com o modelo. Se esse parâmetro for especificado, o formulário HTML5 ignorará o parâmetro dataRef. Como prática recomendada, recomendamos usar a abordagem de dados.

A abordagem recomendada é definir o atributo de dados na solicitação com os dados que você deseja preencher previamente o formulário.

slingRequest.setAttribute(&quot;data&quot;, content);

Neste exemplo, estamos definindo o atributo de dados com o conteúdo. O conteúdo representa os dados com os quais você deseja preencher o formulário previamente. Normalmente, você buscaria o &quot;conteúdo&quot; fazendo uma chamada REST para um serviço interno.

Para obter esse caso de uso, você precisa criar um perfil personalizado. Os detalhes sobre como criar um perfil personalizado estão claramente documentados na [documentação do AEM Forms aqui](https://helpx.adobe.com/aem-forms/6/html5-forms/custom-profile.html).

Depois de criar o perfil personalizado, você criará um arquivo JSP que buscará os dados fazendo chamadas para o sistema de back-end. Depois que os dados forem buscados, você usará o slingRequest.setAttribute(&quot;data&quot;, content); para preencher previamente o formulário

Quando o XDP está sendo renderizado, você também pode transmitir alguns parâmetros para o xdp e, com base no valor do parâmetro, buscar os dados do sistema de back-end.

[Por exemplo, esta URL tem o parâmetro de nome](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=john)

O JSP gravado terá acesso ao parâmetro name por meio do request.getParameter(&quot;name&quot;) . Em seguida, você pode passar o valor desse parâmetro para o processo de back-end para buscar os dados necessários.
Para que esse recurso funcione em seu sistema, siga as etapas mencionadas abaixo:

* [Baixe e importe os ativos para a AEM usando o gerenciador de pacotes](assets/prepopulatemobileform.zip)
O pacote instalará o seguinte

   * CustomProfile
   * XDP de exemplo
   * Exemplo de terminal POST que retornará dados para preencher o formulário

>[!NOTE]
>
>Se quiser preencher o formulário chamando o processo do Workbench, inclua callWorkbenchProcess.jsp em seu /apps/AEMFormsDemoListings/customprofiles/PrepopulateForm/html.jsp em vez de setdata.jsp

* [Aponte seu navegador favorito para esta url](http://localhost:4502/content/dam/formsanddocuments/PrepopulateMobileForm.xdp/jcr:content?name=Adobe%20Systems). O formulário deve ser pré-preenchido com o valor do parâmetro name
