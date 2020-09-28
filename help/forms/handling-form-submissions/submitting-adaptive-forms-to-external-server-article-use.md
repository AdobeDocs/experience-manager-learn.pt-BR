---
title: Submetendo formulário adaptativo ao servidor externo
seo-title: Submetendo formulário adaptativo ao servidor externo
description: Submetendo formulário adaptável ao ponto de extremidade REST em execução no servidor externo
seo-description: Submetendo formulário adaptável ao ponto de extremidade REST em execução no servidor externo
uuid: 1a46e206-6188-4096-816a-d59e9fb43263
feature: adaptive-forms
topics: developing
audience: implementer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 9e936885-4e10-4c05-b572-b8da56fcac73
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '362'
ht-degree: 0%

---


# Submetendo formulário adaptativo ao servidor externo {#submitting-adaptive-form-to-external-server}

Use a ação Enviar para ponto de extremidade REST para postar os dados enviados em um URL REST. O URL pode ser de um servidor interno (o servidor no qual o formulário é renderizado) ou externo.

Normalmente, os clientes desejam enviar os dados do formulário para um servidor externo para processamento adicional.

Para publicar dados em um servidor interno, forneça um caminho do recurso. Os dados são postados no caminho do recurso. Por exemplo, &lt;/content/restEndPoint> . Para essas solicitações de publicação, as informações de autenticação da solicitação de envio são usadas.

Para postar dados em um servidor externo, forneça um URL. The format of the URL is <http://host:port/path_to_rest_end_point>. Verifique se você configurou o caminho para lidar com a solicitação de POST anonimamente.

Para a finalidade deste artigo, eu escrevi um arquivo de guerra simples que pode ser implantado na sua instância tomcat. Presumindo que seu tomcat esteja rodando na porta 8080, o url do POST será

<http://localhost:8080/AemFormsEnablement/HandleFormSubmission>

ao configurar seu Formulário adaptativo para enviar para esse terminal, os dados do formulário e os anexos, se houver, podem ser extraídos no servlet pelo seguinte código

```java
System.out.println("form was submitted");
Part attachment = request.getPart("attachments");
if(attachment!=null)
{
    System.out.println("The content type of the attachment added is "+attachment.getContentType());
}
Enumeration<String> params = request.getParameterNames();
while(params.hasMoreElements())
{
String paramName = params.nextElement();
System.out.println("The param Name is "+paramName);
String data = request.getParameter(paramName);System.out.println("The data  is "+data);
}
```

![envio de formulário](assets/formsubmission.gif)Para testar isso no servidor, faça o seguinte

1. Instale o Tomcat se você ainda não o tiver. [Instruções para instalar o tomcat estão disponíveis aqui](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html)
1. Baixe o arquivo [](assets/aemformsenablement.zip) zip associado a este artigo. Descompacte o arquivo para obter o arquivo de guerra.
1. Implante o arquivo de guerra em seu servidor tomcat.
1. Crie um formulário adaptável simples com o componente de anexo de arquivo e configure sua ação de envio, conforme mostrado na captura de tela acima. O POST é <http://localhost:8080/AemFormsEnablement/HandleFormSubmission>. Se o AEM e o tomcat não estiverem em execução no localhost, altere o URL de acordo.
1. Para habilitar o envio de dados de formulário multiparte para o tomcat, adicione o seguinte atributo ao elemento de contexto do &lt;tomcatInstallDir>\conf\context.xml e reinicie o servidor Tomcat.
1. **&lt;Context allowCasualMultipartParsing=&quot;true&quot;>**
1. Pré-visualização seu Formulário adaptativo, adicione um anexo e envie. Verifique se há mensagens na janela do console tomcat.

