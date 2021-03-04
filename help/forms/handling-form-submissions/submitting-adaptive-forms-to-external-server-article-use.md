---
title: Envio do formulário adaptável ao servidor externo
seo-title: Envio do formulário adaptável ao servidor externo
description: Envio do formulário adaptável ao terminal REST em execução no servidor externo
seo-description: Envio do formulário adaptável ao terminal REST em execução no servidor externo
uuid: 1a46e206-6188-4096-816a-d59e9fb43263
feature: Formulários adaptáveis
topics: developing
audience: implementer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
discoiquuid: 9e936885-4e10-4c05-b572-b8da56fcac73
topic: Desenvolvimento
role: Desenvolvedor
level: Iniciante
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '367'
ht-degree: 1%

---


# Envio do formulário adaptável ao servidor externo {#submitting-adaptive-form-to-external-server}

Use a ação Enviar para o terminal REST para postar os dados enviados em um URL REST. O URL pode ser de um servidor interno (o servidor no qual o formulário é renderizado) ou externo.

Normalmente, os clientes desejam enviar os dados do formulário para um servidor externo para processamento adicional.

Para postar dados em um servidor interno, forneça um caminho do recurso. Os dados são postados no caminho do recurso. Por exemplo, &lt;/content/restEndPoint> . Para essas solicitações de publicação, as informações de autenticação da solicitação de envio são usadas.

Para postar dados em um servidor externo, forneça um URL. O formato do URL é <http://host:port/path_to_rest_end_point>. Verifique se você configurou o caminho para lidar com a solicitação POST anonimamente.

Para o propósito deste artigo, escrevi um arquivo war simples que pode ser implantado em sua instância tomcat. Supondo que seu tomcat esteja sendo executado na porta 8080, o url do POST será

<http://localhost:8080/AemFormsEnablement/HandleFormSubmission>

ao configurar o Formulário adaptável para enviar para esse ponto de extremidade, os dados do formulário e os anexos, se houver, podem ser extraídos no servlet pelo seguinte código

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

![](assets/formsubmission.gif)
formsubmitPara testar isso em seu servidor, faça o seguinte

1. Instale o Tomcat se ainda não o tiver. [Instruções para instalar o tomcat estão disponíveis aqui](https://helpx.adobe.com/experience-manager/kt/forms/using/preparing-datasource-for-form-data-model-tutorial-use.html)
1. Baixe o [arquivo zip](assets/aemformsenablement.zip) associado a este artigo. Descompacte o arquivo para obter o arquivo war.
1. Implante o arquivo war no servidor tomcat.
1. Crie um formulário adaptável simples com o componente de anexo de arquivo e configure sua ação de envio, conforme mostrado na captura de tela acima. O URL de POST é <http://localhost:8080/AemFormsEnablement/HandleFormSubmission>. Se o AEM e o tomcat não estiverem em execução no host local, altere o URL de acordo.
1. Para habilitar o envio de dados de formulário de várias partes para o tomcat, adicione o seguinte atributo ao elemento de contexto de &lt;tomcatInstallDir>\conf\context.xml e reinicie o servidor Tomcat.
1. **&lt;context allowCasualMultipartParsing=&quot;true&quot;>**
1. Visualize o formulário adaptável, adicione um anexo e envie. Verifique se há mensagens na janela do console tomcat.

