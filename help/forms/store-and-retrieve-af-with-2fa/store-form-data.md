---
title: Armazenar dados de formulário
description: Armazenar dados do formulário junto com o novo mapa de anexos no banco de dados
feature: Adaptive Forms
type: Tutorial
version: 6.4,6.5
kt: 6538
thumbnail: 6538.jpg
topic: Development
role: Developer
level: Experienced
exl-id: 2bd9fe63-8f42-4b89-95a0-13ade49bc31b
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '75'
ht-degree: 2%

---

# Armazenar dados de formulário

A próxima etapa é criar um serviço para inserir uma nova linha no banco de dados para armazenar os dados do formulário adaptável e as informações de anexos associadas.
A captura de tela a seguir mostra uma linha no banco de dados.


![exemplo de linha](assets/sample-row.JPG)


O código a seguir insere uma nova linha no banco de dados com os dados apropriados

```java
public String storeFormData(String formData, String attachmentsInfo, String telephoneNumber) {
    log.debug("******Inside my AEMFormsWith DB service*****");
    log.debug("### Inserting data ... " + formData + "and the telephone number to insert is  " + telephoneNumber);
    String insertRowSQL = "INSERT INTO aemformstutorial.formdatawithattachments(guid,afdata,attachmentsInfo,telephoneNumber) VALUES(?,?,?,?)";
    UUID uuid = UUID.randomUUID();
    String randomUUIDString = uuid.toString();
    log.debug("The insert query is " + insertRowSQL);
    Connection c = getConnection();
    PreparedStatement pstmt = null;
    try {
        pstmt = null;
        pstmt = c.prepareStatement(insertRowSQL);
        pstmt.setString(1, randomUUIDString);
        pstmt.setString(2, formData);
        pstmt.setString(3, attachmentsInfo);
        pstmt.setString(4, telephoneNumber);
        log.debug("Executing the insert statment  " + pstmt.executeUpdate());
        c.commit();
    } catch (SQLException e) {

        log.error("unable to insert data in the table", e.getMessage());
    } finally {
        if (pstmt != null) {
            try {
                pstmt.close();
            } catch (SQLException e) {
                log.debug("error in closing prepared statement " + e.getMessage());
            }
        }
        if (c != null) {
            try {
                c.close();
            } catch (SQLException e) {
                log.debug("error in closing connection " + e.getMessage());
            }
        }
    }
    return randomUUIDString;
}
```

## Próximas etapas

[Implementar Salvar e Sair](./create-servlet.md)

