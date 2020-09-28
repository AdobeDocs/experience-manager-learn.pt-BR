---
title: Armazenamento e Recuperação de Dados de Formulário do Banco de Dados MySQL
description: Tutorial de várias peças para orientá-lo pelas etapas envolvidas no armazenamento e recuperação de dados do formulário
feature: adaptive-forms
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '101'
ht-degree: 0%

---

# Criar servlet para armazenar dados de formulário

A próxima etapa é criar um servlet que insira ou atualize os dados do formulário. A fonte de dados agrupados da conexão Apache Sling configurada na etapa anterior é referenciada na linha 26. O restante do código é bastante simples. O código insere uma nova linha no banco de dados ou atualiza uma linha existente. Os dados do Formulário adaptativo armazenados estão associados a um GUID. O mesmo GUID é usado para atualizar os dados do formulário.

```java
package com.techmarketing.core.servlets;

import com.google.gson.JsonObject;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.servlet.Servlet;
import javax.sql.DataSource;
import java.io.IOException;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;
import java.util.UUID;

@Component(
        service = {Servlet.class},
        property = {
                "sling.servlet.methods=post",
                "sling.servlet.paths=/bin/storeafdata"
        }
)
public class StoreDataInDB extends SlingAllMethodsServlet {

    private static final Logger log = LoggerFactory.getLogger(StoreDataInDB.class);
    private static final long serialVersionUID = 1L;
    
    @Reference(target = "(&(datasource.name=aemformstutorial))")
    private DataSource dataSource;

    public String updateData(String afdata, String guid) {
        String updateTableSQL = "update aemformstutorial.formdata set afdata= ? where guid = ?";
        Connection c = getConnection();
        PreparedStatement pstmt = null;
        try {

            pstmt = null;
            pstmt = c.prepareStatement(updateTableSQL);
            pstmt.setString(1, afdata);
            pstmt.setString(2, guid);
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
        return guid;


    }

    public String insertData(String afdata) {
        log.debug("### Insert Data #### The json object I got to insert was " + afdata);
        String insertTableSQL = "INSERT INTO aemformstutorial.formdata(guid,afdata) VALUES(?,?)";
        UUID uuid = UUID.randomUUID();
        String randomUUIDString = uuid.toString();
        log.debug("The query is " + insertTableSQL);
        Connection c = getConnection();
        PreparedStatement pstmt = null;
        try {

            pstmt = null;
            pstmt = c.prepareStatement(insertTableSQL);
            pstmt.setString(1, randomUUIDString);
            pstmt.setString(2, afdata);
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
        return randomUUIDString;
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

    protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response) {
        log.debug("Inside my save af data servlet");
        if (request.getParameter("operation").equalsIgnoreCase("update")) {
            log.debug("The operation is update");
            log.debug("The data I got was " + request.getParameter("formdata"));
            String guid = updateData(request.getParameter("formdata"), request.getParameter("guid"));
            log.debug("The guid I got was  " + guid);
            JsonObject jsonResponse = new JsonObject();
            try {
                jsonResponse.addProperty("guid", guid);
                response.setContentType("application/json");
                response.setCharacterEncoding("UTF-8");
                response.getWriter().write(jsonResponse.toString());

            } catch (IOException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
        }

        if (request.getParameter("operation").equalsIgnoreCase("insert")) {
            log.debug("The data I got was " + request.getParameter("formdata"));
            String guid = insertData(request.getParameter("formdata"));
            log.debug("The guid on inserting data  " + guid);
            JsonObject jsonResponse = new JsonObject();
            try {
                jsonResponse.addProperty("guid", guid);
                response.setContentType("application/json");
                response.setCharacterEncoding("UTF-8");
                response.getWriter().write(jsonResponse.toString());

            } catch (IOException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
        }
    }
}
```
