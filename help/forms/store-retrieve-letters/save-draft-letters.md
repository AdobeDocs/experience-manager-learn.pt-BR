---
title: Salvar e retomar cartas
seo-title: Save and resume letters
description: Saiba como salvar e recuperar letras de rascunho
seo-description: Learn how to save and retrieve draft letters
feature: Interactive Communication
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.5
topic: Development
role: Developer
level: Intermediate
kt: 10208
source-git-commit: 0a52ea9f5a475814740bb0701a09f1a6735c6b72
workflow-type: tm+mt
source-wordcount: '223'
ht-degree: 0%

---

# Salvar rascunhos

O código a seguir é usado para salvar a instância da carta. Os metadados da instância da carta são armazenados no _icrascunhos_ tabela. Uma string exclusiva (rascunhoID) é gerada e retornada. Essa string exclusiva é então usada para recuperar a instância da letra salva.

```java
public String save(CCRDocumentInstance letterToSave) throws CCRDocumentException {
  String insertRowSQL = "INSERT INTO aemformstutorial.icdrafts(draftID,documentID,status,owner,name) VALUES(?,?,?,?,?)";
  log.debug(" in save IC Draft" + letterToSave.getDocumentId() + letterToSave.getName());
  UUID uuid = UUID.randomUUID();
  String uuidString = uuid.toString();
  Connection connection = getConnection();
  PreparedStatement pstmt = null;
  Document icData = letterToSave.getData();
  try {
    pstmt = connection.prepareStatement(insertRowSQL);
    pstmt.setString(1, uuidString);
    pstmt.setString(2, letterToSave.getDocumentId());
    pstmt.setString(3, "DRAFT");
    pstmt.setString(4, letterToSave.getCreatedBy());
    pstmt.setString(5, letterToSave.getName());
    icData.copyToFile(new File(uuidString + ".xml"));
    log.debug("Executing the insert statment  " + pstmt.executeUpdate());
    connection.commit();
  } catch (IOException | SQLException e) {
    log.debug("The error is " + e.getMessage());
  } finally {
    if (pstmt != null) {
      try {
        pstmt.close();
      } catch (SQLException e) {
        log.debug("Error in closing prepared statment" + e.getMessage());
      }
    }
    if (connection != null) {
      try {
        log.debug("Closing the connection in Save Letter Draft");
        connection.close();
      } catch (SQLException e) {
        log.debug("Error in closing connection" + e.getMessage());
      }
    }

  }

  return uuidString;
}
```

## Obter Carta

O código a seguir foi gravado para buscar a carta de rascunho salva.
Para carregar instâncias de carta salvas, é necessário fornecer o rascunhoID. Com base nesse rascunhoID, consultamos o banco de dados para obter os metadados adicionais sobre a carta. A mesma DraftID é usada para criar os dados da carta lendo o xml apropriado do sistema de arquivos. Um objeto CCRDocumentInstance é então construído e retornado.


```java
@Override
public CCRDocumentInstance get(String draftID) throws CCRDocumentException {

  String selectStatement = "Select documentID from aemformstutorial.icdrafts where draftID='" + draftID + "'";
  log.debug("The select statement is " + selectStatement);
  Connection connection = getConnection();
  Statement statement = null;
  String documentID = "";
  try {
    statement = connection.createStatement();
    ResultSet rs = statement.executeQuery(selectStatement);
    while (rs.next()) {
      documentID = rs.getString("documentID");

    }
  } catch (SQLException e) {
    log.debug("The error is " + e.getMessage());
  }
  Document draftData = new Document(new File(draftID + ".xml"));
  CCRDocumentInstance draftInstance = new CCRDocumentInstance(draftData, "abc", documentID, CCRDocumentInstance.Status.DRAFT);
  draftInstance.setId(draftID);
  return draftInstance;
}
```

### Atualizar carta

O código a seguir foi usado para atualizar a instância da carta salva. Os dados atualizados da carta são gravados no sistema de arquivos usando a ID da carta.

```java
public void update(CCRDocumentInstance letterInstanceToUpdate) throws CCRDocumentException {
		Document icData = letterInstanceToUpdate.getData();
		String draftID = letterInstanceToUpdate.getId();
		log.debug("updating letter instance with draft id =  "+draftID);
		try
			{
				icData.copyToFile(new File(draftID+".xml"));
			} 
		catch (IOException e)
			{
				log.debug("Error updating "+e.getMessage());;
			}
		
	}
```

### Obter todas as cartas salvas

O AEM Forms não fornece nenhuma interface de usuário pronta para listar as letras salvas. Para este artigo, listo as instâncias de carta salvas em um formato de tabela usando um formulário adaptável.
Você pode personalizar a consulta para buscar as instâncias de carta salvas. Neste exemplo, estou consultando a instância da carta salva por &quot;administrador&quot;.

```java
	public List < CCRDocumentInstance > getAll(String arg0, Date arg1, Date arg2, Map < String, Object > arg3) throws CCRDocumentException {
	  String selectStatement = "Select * from aemformstutorial.icdrafts where owner = 'admin'";
	  Connection connection = getConnection();
	  Statement statement = null;
	  String documentID = "";
	  List < CCRDocumentInstance > listOfDrafts = new ArrayList < CCRDocumentInstance > ();
	  String draftID;
	  String savedInstanceName = "";
	  try {
	    statement = connection.createStatement();
	    ResultSet rs = statement.executeQuery(selectStatement);
	    while (rs.next()) {
	      documentID = rs.getString("documentID");
	      draftID = rs.getString("draftID");
	      savedInstanceName = rs.getString("name");
	      Document draftData = new Document(new File(draftID + ".xml"));
	      CCRDocumentInstance draftLetter = new CCRDocumentInstance(draftData, savedInstanceName, documentID, CCRDocumentInstance.Status.DRAFT);
	      listOfDrafts.add(draftLetter);
	    }
	  } catch (SQLException e) {
	    log.debug("The error is " + e.getMessage());
	  } finally {
	    if (statement != null) {
	      try {
	        statement.close();
	      } catch (SQLException e) {
	        log.debug("error in closing statement" + e.getMessage());
	      }
	    }
	    if (connection != null) {
	      try {
	        connection.close();
	      } catch (SQLException e) {
	        log.debug("error in closing connection" + e.getMessage());
	      }
	    }
	  }

	  return listOfDrafts;
	}
```

### Projeto Eclipse

O projeto do eclipse com implementação de amostra pode ser [baixado aqui](assets/icdrafts-eclipse-project.zip)
