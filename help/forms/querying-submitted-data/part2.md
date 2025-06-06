---
title: AEM Forms com esquema e dados JSON[Part2]
description: Tutorial em várias partes para orientá-lo pelas etapas envolvidas na criação do Formulário adaptável com esquema JSON e na consulta dos dados enviados.
feature: Adaptive Forms
doc-type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Experienced
exl-id: 29195c70-af12-4a22-8484-3c87a1e07378
duration: 110
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '330'
ht-degree: 0%

---

# Armazenamento de Dados Enviados no Banco de Dados


>[!NOTE]
>
>É recomendável usar o MySQL 8 como banco de dados, pois ele tem suporte para tipos de dados JSON. Você também precisará instalar o driver apropriado para MySQL DB. Usei o driver disponível neste local https://mvnrepository.com/artifact/mysql/mysql-connector-java/8.0.12

Para armazenar os dados enviados no banco de dados, gravaremos um servlet para extrair os dados vinculados e o nome e o armazenamento do formulário. O código completo para lidar com o envio do formulário e armazenar o afBoundData no banco de dados é fornecido abaixo.

Criamos um envio personalizado para lidar com o envio do formulário. No post.POST.jsp deste envio personalizado, encaminhamos a solicitação para nosso servlet.

Para saber mais sobre envio personalizado, leia este [artigo](https://helpx.adobe.com/experience-manager/kt/forms/using/custom-submit-aem-forms-article.html)

com.adobe.aemds.guide.utils.GuideSubmitUtils.setForwardPath(slingRequest,&quot;/bin/storeafsubmit&quot;,null,null);

```java
package com.aemforms.json.core.servlets;

import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;

import javax.servlet.Servlet;
import javax.servlet.ServletException;
import javax.sql.DataSource;

import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.json.JSONException;
import org.json.JSONObject;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

@Component(service = Servlet.class, property = {

"sling.servlet.methods=get", "sling.servlet.methods=post",

"sling.servlet.paths=/bin/storeafsubmission"

})
public class HandleAdaptiveFormSubmission extends SlingAllMethodsServlet {
 private static final Logger log = LoggerFactory.getLogger(HandleAdaptiveFormSubmission.class);
 private static final long serialVersionUID = 1L;
 @Reference(target = "(&(objectclass=javax.sql.DataSource)(datasource.name=aemformswithjson))")
 private DataSource dataSource;

 protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) throws ServletException {
  JSONObject afSubmittedData;
  try {
   afSubmittedData = new JSONObject(request.getParameter("jcr:data"));
   // we will only store the data bound to schema
   JSONObject dataToStore = afSubmittedData.getJSONObject("afData").getJSONObject("afBoundData")
     .getJSONObject("data");
   String formName = afSubmittedData.getJSONObject("afData").getJSONObject("afSubmissionInfo")
     .getString("afPath");
   log.debug("The form name is " + formName);
   insertData(dataToStore, formName);

  } catch (JSONException e) {
   // TODO Auto-generated catch block
   e.printStackTrace();
  }

 }

 public void insertData(org.json.JSONObject jsonData, String formName) {
  log.debug("The json object I got to insert was " + jsonData.toString());
  String insertTableSQL = "INSERT INTO aemformswithjson.formsubmissions(formdata,formname) VALUES(?,?)";
  log.debug("The query is " + insertTableSQL);
  Connection c = getConnection();
  PreparedStatement pstmt = null;
  try {
   pstmt = null;
   pstmt = c.prepareStatement(insertTableSQL);
   pstmt.setString(1, jsonData.toString());
   pstmt.setString(2, formName);
   log.debug("Executing the insert statment  " + pstmt.executeUpdate());
   c.commit();
  } catch (SQLException e) {

   log.error("Getting errors", e);
  } finally {
   if (pstmt != null) {
    try {
     pstmt.close();
    } catch (SQLException e) {
     // TODO Auto-generated catch block
     e.printStackTrace();
    }
   }
   if (c != null) {
    try {
     c.close();
    } catch (SQLException e) {
     // TODO Auto-generated catch block
     e.printStackTrace();
    }
   }
  }
 }

 public Connection getConnection() {
  log.debug("Getting Connection ");
  Connection con = null;
  try {

   con = dataSource.getConnection();
   log.debug("got connection");
   return con;
  } catch (Exception e) {
   log.error("not able to get connection ", e);
  }
  return null;
 }

}
```

![connectionpool](assets/connectionpooled.gif)

Para fazer com que isso funcione em seu sistema, siga as etapas a seguir

* [Baixe e descompacte o arquivo zip](assets/aemformswithjson.zip)
* Criar formulário adaptável com esquema JSON. Você pode usar o esquema JSON fornecido como parte dos ativos deste artigo. Verifique se a ação de envio do formulário está configurada corretamente. A ação Enviar precisa ser configurada como &quot;CustomSubmitHelpx&quot;.
* Crie um esquema na instância do MySQL importando o arquivo schema.sql usando a ferramenta MySQL Workbench. O arquivo schema.sql também é fornecido como parte deste tutorial de ativos.
* Configure a fonte de dados agrupada da conexão Apache Sling no console da Web Felix
* Certifique-se de nomear o nome da fonte de dados como &quot;aemformswithjson&quot;. Este é o nome usado pelo pacote OSGi de amostra fornecido a você
* Consulte as propriedades na imagem acima. Isso pressupõe que você usará o MySQL como banco de dados.
* Implante os pacotes OSGi fornecidos como parte deste artigo de ativos.
* Pré-visualize o formulário e envie.
* Os dados JSON são armazenados no banco de dados criado ao importar o arquivo &quot;schema.sql&quot;.
