---
title: Criar serviço OSGi
description: Criar serviço OSGi para armazenar os formulários a serem assinados
feature: Workflow
version: 6.4,6.5
thumbnail: 6886.jpg
jira: KT-6886
topic: Development
role: Developer
level: Experienced
exl-id: 49e7bd65-33fb-44d4-aaa2-50832dffffb0
duration: 150
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '364'
ht-degree: 1%

---

# Criar serviço OSGi

O código a seguir foi gravado para armazenar os formulários que precisam ser assinados. Cada formulário a ser assinado está associado a um guid exclusivo e um id do cliente. Portanto, um ou mais formulários podem ser associados à mesma ID do cliente, mas terão uma GUID exclusiva atribuída ao formulário.

## Interface

Esta é a declaração de interface que foi usada

```java
package com.aem.forms.signmultipleforms;
import com.adobe.granite.workflow.WorkflowSession;
import com.adobe.granite.workflow.exec.WorkItem;
public interface SignMultipleForms
{
    public void insertData(String []formNames,String formData,String serverURL,WorkItem workItem, WorkflowSession workflowSession);
    public String getNextFormToSign(int customerID);
    public void updateSignatureStatus(String formData,String guid);
    public String getFormData(String guid);
}
```



## Inserir dados

O método insert data insere uma linha no banco de dados identificado pela fonte de dados. Cada linha no banco de dados corresponde a um formulário e é identificada exclusivamente por um GUID e uma ID do cliente. Os dados de formulário e a URL do formulário também são armazenados nessa linha. A coluna de status é para indicar se o formulário foi preenchido e assinado. O valor 0 indica que o formulário ainda não foi assinado.

```java
@Override
public void insertData(String[] formNames, String formData, String serverURL, WorkItem workItem, WorkflowSession workflowSession) {
  String insertTableSQL = "INSERT INTO aemformstutorial.signingforms(formName,formData,guid,status,customerID) VALUES(?,?,?,?,?)";
  Random r = new Random();
  Connection con = getConnection();
  PreparedStatement pstmt = null;
  int customerIDGnerated = r.nextInt((1000 - 1) + 1) + 1;
  log.debug("The number of forms to insert are  " + formNames.length);
  try {
    for (int i = 0; i < formNames.length; i++) {
      log.debug("Inserting form name " + formNames[i]);
      UUID uuid = UUID.randomUUID();
      String randomUUIDString = uuid.toString();
      String formUrl = serverURL + "/content/dam/formsanddocuments/formsandsigndemo/" + formNames[i] + "/jcr:content?wcmmode=disabled&guid=" + randomUUIDString + "&customerID=" + customerIDGnerated;
      pstmt = con.prepareStatement(insertTableSQL);
      pstmt.setString(1, formUrl);
      pstmt.setString(2, formData.replace("<guid>3938</guid>", "<guid>" + uuid + "</guid>"));
      pstmt.setString(3, uuid.toString());
      pstmt.setInt(4, 0);
      pstmt.setInt(5, customerIDGnerated);
      log.debug("customerIDGnerated the insert statment  " + pstmt.execute());
      if (i == 0) {
        WorkflowData wfData = workItem.getWorkflowData();
        wfData.getMetaDataMap().put("formURL", formUrl);
        workflowSession.updateWorkflowData(workItem.getWorkflow(), wfData);
        log.debug("$$$$ Done updating the map");

}

}
    con.commit();
  }
  catch(Exception e) {
    log.debug(e.getMessage());
  }
  finally {
    if (pstmt != null) {
      try {
        pstmt.close();
      } catch(SQLException e) {

log.debug(e.getMessage());
      }
    }
    if (con != null) {
      try {
        con.close();
      } catch(SQLException e) {
        log.debug(e.getMessage());
      }
    }
  }

}
```


## Obter dados do formulário

O código a seguir é usado para buscar dados de formulários adaptáveis associados à GUID fornecida. Os dados do formulário são usados para preencher previamente o formulário adaptável.

```java
@Override
public String getFormData(String guid) {
  log.debug("### Getting form data asscoiated with guid " + guid);
  Connection con = getConnection();
  try {
    Statement st = con.createStatement();
    String query = "SELECT formData FROM aemformstutorial.signingforms where guid = '" + guid + "'" + "";
    log.debug(" The query being consrtucted " + query);
    ResultSet rs = st.executeQuery(query);
    while (rs.next()) {
      return rs.getString("formData");
    }
  } catch(SQLException e) {
    // TODO Auto-generated catch block
    log.debug(e.getMessage());
  }

  return null;

}
```

## Atualizar Status da Assinatura

A conclusão com êxito da cerimônia de assinatura aciona um fluxo de trabalho de AEM associado ao formulário. A primeira etapa do fluxo de trabalho é uma etapa do processo que atualiza o status no banco de dados para a linha identificada pela guid e ID do cliente. Também definimos o valor do elemento assinado nos dados do formulário como Y para indicar que o formulário foi preenchido e assinado. O formulário adaptável é preenchido com esses dados e o valor do elemento de dados assinado nos dados xml é usado para exibir a mensagem apropriada. O código updateSignatureStatus é chamado da etapa de processo personalizada.


```java
public void updateSignatureStatus(String formData, String guid) {
  String updateStatment = "update aemformstutorial.signingforms SET formData = ?, status = ? where guid = ?";
  PreparedStatement updatePS = null;
  Connection con = getConnection();
  try {
    updatePS = con.prepareStatement(updateStatment);
    updatePS.setString(1, formData.replace("<signed>N</signed>", "<signed>Y</signed>"));
    updatePS.setInt(2, 1);
    updatePS.setString(3, guid);
    log.debug("Updated the signature status " + String.valueOf(updatePS.execute()));
  } catch(SQLException e) {
    log.debug(e.getMessage());
  }
  finally {
    try {
      con.commit();
      updatePS.close();
      con.close();
    } catch(SQLException e) {
      
      log.debug(e.getMessage());
    }

  }

}
```

## Obter próximo formulário para assinar

O código a seguir foi usado para obter o próximo formulário para assinatura de determinada customerID com status 0. Se a consulta sql não retornar nenhuma linha, retornaremos a cadeia de caracteres **&quot;AllDone&quot;**, que indica que não há mais formulários para assinatura para a ID de cliente fornecida.

```java
@Override
public String getNextFormToSign(int customerID) {
  System.out.println("### inside my next form to sign " + customerID);
  String selectStatement = "SELECT formName FROM aemformstutorial.signingforms where status = 0 and customerID=" + customerID;
  Connection con = getConnection();
  try
   {
    Statement st = con.createStatement();
    ResultSet rs = st.executeQuery(selectStatement);
    while (rs.next()) {
      log.debug("Got result set object");
      return rs.getString("formName");
    }
    if (!rs.next()) {
      return "AllDone";
    }
  } catch(SQLException e) {
    log.debug(e.getMessage());
  }
  finally {
    try {
      con.close();
    } catch(SQLException e) {
      // TODO Auto-generated catch block
      log.debug(e.getMessage());
    }
  }

  return null;

}
```



## Ativos

O pacote OSGi com os serviços mencionados acima pode ser [baixado daqui](assets/sign-multiple-forms.jar)

## Próximas etapas

[Criar um fluxo de trabalho principal para lidar com o envio inicial do formulário](./create-main-workflow.md)
