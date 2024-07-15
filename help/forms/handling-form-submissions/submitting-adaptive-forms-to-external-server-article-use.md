---
title: Envio do formulário adaptável ao servidor externo
description: Envio do formulário adaptável para o endpoint REST em execução no servidor externo
feature: Adaptive Forms
doc-type: article
version: 6.4,6.5
discoiquuid: 9e936885-4e10-4c05-b572-b8da56fcac73
topic: Development
role: Developer
level: Beginner
exl-id: 5363c3f7-9006-4430-b647-f3283a366a64
last-substantial-update: 2020-07-07T00:00:00Z
duration: 78
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '340'
ht-degree: 0%

---

# Envio do formulário adaptável ao servidor externo {#submitting-adaptive-form-to-external-server}

Use a ação Enviar para endpoint REST para publicar os dados enviados em um URL REST. A URL pode ser de um servidor interno (o servidor no qual o formulário é renderizado) ou externo.

Normalmente, os clientes desejariam enviar os dados do formulário a um servidor externo para processamento adicional.

Para publicar dados em um servidor interno, forneça um caminho para o recurso. Os dados são publicados no caminho do recurso. Por exemplo, &lt;/content/restEndPoint> . Para essas solicitações de publicação, as informações de autenticação de solicitação de envio são usadas.

Para publicar dados em um servidor externo, forneça um URL. O formato da URL é <http://host:port/path_to_rest_end_point>. Certifique-se de ter configurado o caminho para lidar com a solicitação POST anonimamente.

Para o propósito deste artigo, escrevi um arquivo war simples que pode ser implantado em sua instância tomcat. Supondo que o tomcat esteja em execução na porta 8080, o URL do POST será

<http://localhost:8080/AemFormsEnablement/HandleFormSubmission>

ao configurar o formulário adaptável para enviar para esse endpoint, os dados do formulário e os anexos, se houver, podem ser extraídos no servlet pelo seguinte código

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

![formsubmit](assets/formsubmission.gif)
Para testar isso no servidor, faça o seguinte

1. Instale o Tomcat se você ainda não o tiver. [As instruções para instalar o tomcat estão disponíveis aqui](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html)
1. Baixe o [arquivo zip](assets/aemformsenablement.zip) associado a este artigo. Descompacte o arquivo para obter o arquivo WAR.
1. Implante o arquivo WAR no servidor Tomcat.
1. Crie um formulário adaptável simples com o componente de anexo de arquivo e configure a ação de envio como mostrado na captura de tela acima. A URL do POST é <http://localhost:8080/AemFormsEnablement/HandleFormSubmission>. Se o AEM e o tomcat não estiverem em execução no host local, altere o URL de acordo.
1. Para habilitar o envio de dados de formulário de várias partes para o tomcat, adicione o seguinte atributo ao elemento de contexto do &lt;tomcatInstallDir>\conf\context.xml e reinicie o servidor Tomcat.
1. **&lt;Contexto allowCasualMultipartParsing=&quot;true&quot;>**
1. Pré-visualize o formulário adaptável, adicione um anexo e envie. Verifique se há mensagens na janela do console do tomcat.
