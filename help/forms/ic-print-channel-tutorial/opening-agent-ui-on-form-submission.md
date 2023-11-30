---
title: Erro ao abrir a interface do usuário do agente no envio do POST
seo-title: Opening Agent UI On POST Submission
description: Esta é a parte 11 do tutorial em várias etapas para criar seu primeiro documento de comunicações interativas para o canal de impressão. Nesta parte, iniciaremos a interface do agente para criar correspondência ad-hoc no envio do formulário.
seo-description: This is part 11 of multistep tutorial for creating your first interactive communications document for the print channel. In this part, we will launch the agent ui interface for creating ad-hoc correspondence on form submission.
uuid: 96f34986-a5c3-400b-b51b-775da5d2cbd7
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
jira: KT-6168
thumbnail: 40122.jpg
topic: Development
role: Developer
level: Intermediate
exl-id: 509b4d0d-9f3c-46cb-8ef7-07e831775086
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 1%

---

# Erro ao abrir a interface do usuário do agente no envio do POST

Nesta parte, iniciaremos a interface do agente para criar correspondência ad-hoc no envio do formulário.

Este artigo o guiará pelas etapas relativas à abertura da interface do usuário do agente no envio de um formulário. Um caso de uso típico é o agente de atendimento ao cliente preencher um formulário com alguns parâmetros de entrada, e uma interface do agente de envio de formulário é aberta com dados pré-preenchidos no serviço de preenchimento do modelo de dados de formulário. Os parâmetros de entrada para o serviço de preenchimento do modelo de dados de formulário são extraídos do envio do formulário.

O vídeo a seguir mostra casos de uso

>[!VIDEO](https://video.tv.adobe.com/v/40122?quality=12&learn=on)

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

Linha 1: obter o número da conta do parâmetro de solicitação

Linhas 2-8: crie o mapa de parâmetros e defina chaves e valores apropriados para refletir documentId, Random.

Linha 9-10: crie outro objeto Map para manter o parâmetro de entrada definido no Modelo de dados de formulário.

Linha 11: definir o atributo slingRequest &quot;paramMap&quot;

Linha 12-13: Encaminhar a solicitação para o servlet

Para testar esse recurso no servidor

* [Importe e instale os ativos relacionados a este artigo usando o gerenciador de pacotes.](assets/launch-agent-ui.zip)
* [Fazer logon no configMgr](http://localhost:4502/system/console/configMgr)
* Pesquisar por _Filtro CSRF do Adobe Granite_
* Adicionar _/content/getprintchannel_ nos Caminhos excluídos
* Salve as alterações.
* [Abra POST.jsp](http://localhost:4502/apps/AEMForms/openprintchannel/POST.jsp). Verifique se a cadeia de caracteres transmitida para FormFieldRequestParameter é um documentId válido.(Linha 19).
* [Abrir a página da Web](http://localhost:4502/content/OpenPrintChannel.html) e insira account number e envie o formulário.
* A interface do usuário do agente deve abrir com os dados pré-preenchidos específicos para o número da conta inserido no formulário.

>[!NOTE]
>
>Certifique-se de que o parâmetro de entrada da operação Obter do modelo de dados de formulário esteja vinculado ao Atributo de solicitação chamado &quot;accountnumber&quot; para que isso funcione. Se você alterar o nome do valor de vinculação para qualquer outro nome, verifique se a alteração é refletida na linha 25 do POST.jsp
