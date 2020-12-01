---
title: Abrindo a interface do usuário do agente no envio do POST
seo-title: Abrindo a interface do usuário do agente no envio do POST
description: Esta é a parte 11 do tutorial de várias etapas para criar seu primeiro documento de comunicação interativo para o canal de impressão. Nesta parte, iniciaremos a interface de interface de interface do agente para criar correspondência ad-hoc no envio do formulário.
seo-description: Esta é a parte 11 do tutorial de várias etapas para criar seu primeiro documento de comunicação interativo para o canal de impressão. Nesta parte, iniciaremos a interface de interface de interface do agente para criar correspondência ad-hoc no envio do formulário.
uuid: 96f34986-a5c3-400b-b51b-775da5d2cbd7
feature: interactive-communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
kt: 6168
thumbnail: 40122.jpg
translation-type: tm+mt
source-git-commit: 824efde8d90dd77d41dce093998b4215db2532ae
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 0%

---


# Abrindo a interface do usuário do agente no envio do POST

Nesta parte, iniciaremos a interface de interface de interface do agente para criar correspondência ad-hoc no envio do formulário.

Este artigo o guiará pelas etapas envolvidas na abertura da interface de interface de interface do agente ao enviar um formulário. O caso de uso típico é o de o agente de atendimento ao cliente preencher um formulário com alguns parâmetros de entrada e na interface de usuário do agente de envio do formulário é aberto com dados pré-preenchidos do serviço de preenchimento prévio do modelo de dados do formulário. Os parâmetros de entrada para o serviço de preenchimento prévio do modelo de dados do formulário são extraídos do envio do formulário.

O vídeo a seguir mostra o caso de uso

>[!VIDEO](https://video.tv.adobe.com/v/40122/?quality=9&learn=on)

```java
String accountNumber = request.getParameter("accountnumber"))
ParameterMap parameterMap = new ParameterMap();
RequestParameter icLetterId[] = new RequestParameter[1];
icLetterId[0] = new FormFieldRequestParameter("/content/dam/formsanddocuments/retirementstatementprint");
parameterMap.put("documentId", icLetterId);
RequestParameter Random[] = new RequestParameter[1];
Random[0] = new FormFieldRequestParameter("209457");
parameterMap.put("Random", Random);
Map map = new HashMap();
map.put("accountnumber",accountNumber);
slingRequest.setAttribute("paramMap",map);
CustomParameterRequest wrapperRequest = new CustomParameterRequest(slingRequest,parameterMap,"GET");
wrapperRequest.getRequestDispatcher("/aem/forms/createcorrespondence.html").include(wrapperRequest, response);
```

Linha 1 : Obter o número da conta do parâmetro de solicitação

Linha 2-8: Crie o mapa de parâmetros e defina as chaves e os valores apropriados para refletir o documentId, Aleatório.

Linha 9-10: Crie outro objeto de Mapa para manter o parâmetro de entrada definido no Modelo de dados de formulário.

Linha 11: Defina o atributo slingRequest &quot;paramMap&quot;

Linha 12-13: Encaminhar a solicitação para o servlet

Para testar esse recurso no servidor

* [Importe e instale os ativos relacionados a este artigo usando o gerenciador de pacotes.](assets/launch-agent-ui.zip)
* [Logon no configMgr](http://localhost:4502/system/console/configMgr)
* Procure _Filtro CSRF de Adobe Granite_
* Adicionar _/content/getprintchannel_ nos Caminhos Excluídos
* Salve as alterações.
* [Abra POST.jsp](http://localhost:4502/apps/AEMForms/openprintchannel/POST.jsp). Verifique se a string transmitida para FormFieldRequestParameter é documentId válido.(Linha 19).
* [Abra a página da ](http://localhost:4502/content/OpenPrintChannel.html) Web, digite o número da conta e envie o formulário.
* A interface da interface do usuário do agente deve ser aberta com os dados pré-preenchidos específicos do número da conta inserido no formulário.

>[!NOTE]
>
>Certifique-se de que o parâmetro de entrada da operação Get do Modelo de Dados de Formulário esteja vinculado ao Atributo de Solicitação chamado &quot;accountnumber&quot; para que isso funcione. Se você alterar o nome do valor de vínculo para qualquer outro nome, verifique se a alteração está refletida na linha 25 do POST.jsp

