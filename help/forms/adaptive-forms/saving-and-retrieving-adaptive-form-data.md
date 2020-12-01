---
title: Como salvar e recuperar dados do formulário adaptável
seo-title: Como salvar e recuperar dados do formulário adaptável
description: Como salvar e recuperar dados de formulário adaptáveis do banco de dados. Esse recurso permite que os usuários salvem o formulário e continuem preenchendo o formulário posteriormente.
seo-description: Como salvar e recuperar dados de formulário adaptáveis do banco de dados. Esse recurso permite que os usuários salvem o formulário e continuem preenchendo o formulário posteriormente.
feature: adaptive-forms
topics: developing
audience: developer,implementer
doc-type: article
activity: setup
version: 6.3,6.4,6.5
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '645'
ht-degree: 0%

---


# Como salvar e recuperar dados do formulário adaptável

Este artigo o guiará pelas etapas envolvidas na gravação e recuperação de Dados de formulário adaptável do banco de dados. O banco de dados MySQL foi usado para armazenar os dados do Formulário adaptável. Em um nível alto, as etapas a seguir para obter o caso de uso são:

* [Configurar fonte de dados](#Configure-Data-Source)
* [Criar servlet para gravar dados no banco de dados](#create-servlet)
* [Criar serviço OSGI para buscar dados armazenados](#create-osgi-service)
* [Criar biblioteca de cliente](#create-client-library)
* [Criar modelo de formulário adaptável e componente de página](#form-template-and-page-component)
* [Demonstração de capacidade](#capability-demo)
* [Implantar em seu servidor](#deploy-on-your-server)

## Configurar Fonte de Dados {#Configure-Data-Source}

A Apache Sling Connection Pooling DataSource está configurada para apontar para o banco de dados que será usado para armazenar os dados do Formulário adaptável. A seguinte captura de tela mostra a configuração da minha instância. As seguintes propriedades podem ser copiadas e coladas

* Nome da fonte de dados:aemformstutorial - Este é o nome usado em meu código.

* Classe de driver JDBC:com.mysql.jdbc.Driver

* URL de conexão JDBC:jdbc:mysql://localhost:3306/aemformstutorial

![connectionpool](assets/storingdata.PNG)

### Criar servlet {#create-servlet}

A seguir está o código do servlet que insere/atualiza os dados do Formulário adaptável no banco de dados. A Fonte de Dados Apache Sling Pooling é configurada usando o AEM ConfigMgr e o mesmo é referenciado na linha 26. O resto do código é bastante simples. O código insere uma nova linha no banco de dados ou atualiza uma linha existente. Os dados armazenados do Formulário adaptativo estão associados a um GUID. O mesmo GUID é usado para atualizar os dados do formulário.

```java
package com.techmarketing.core.servlets;
import java.io.IOException;
import java.sql.Connection;
import java.sql.PreparedStatement;
import java.sql.SQLException;
import java.util.UUID;
import javax.servlet.Servlet;
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
 
@Component(service = { Servlet.class}, property = {"sling.servlet.methods=post","sling.servlet.paths=/bin/storeafdata"})
public class StoreDataInDB extends SlingAllMethodsServlet {
     private static final Logger log = LoggerFactory.getLogger(StoreDataInDB.class);
        private static final long serialVersionUID = 1L;
     @Reference(target = "(&(objectclass=javax.sql.DataSource)(datasource.name=aemformstutorial))")
        private DataSource dataSource;
    public String updateData(String afdata,String guid)
    {
         String updateTableSQL = "update aemformstutorial.formdata set afdata= ? where guid = ?";
         Connection c = getConnection();
            PreparedStatement pstmt = null;
            try {
      
                pstmt = null;
                pstmt = c.prepareStatement(updateTableSQL);
                pstmt.setString(1,afdata);
                pstmt.setString(2,guid);
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
                pstmt.setString(1,randomUUIDString);
                pstmt.setString(2,afdata);
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
    protected void doPost(SlingHttpServletRequest request,SlingHttpServletResponse response)
    {
        log.debug("Inside my save af data servlet");
        if(request.getParameter("operation").equalsIgnoreCase("update"))
        {
            log.debug("The operation is update");
            log.debug("The data I got was "+request.getParameter("formdata"));
            String guid = updateData(request.getParameter("formdata"),request.getParameter("guid"));
            log.debug("The guid I got was  "+guid);
            JSONObject jsonResponse = new JSONObject();
            try {
                jsonResponse.put("guid",guid);
                response.setContentType("application/json");
                response.setCharacterEncoding("UTF-8");
                response.getWriter().write(jsonResponse.toString());
            } catch (JSONException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            } catch (IOException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
        }
        if(request.getParameter("operation").equalsIgnoreCase("insert"))
        {
            log.debug("The data I got was +request.getParameter("formdata");
            String guid = insertData(request.getParameter("formdata"));
            log.debug("The guid on inserting data  "+guid);
            JSONObject jsonResponse = new JSONObject();
            try {
                jsonResponse.put("guid",guid);
                response.setContentType("application/json");
                response.setCharacterEncoding("UTF-8");
                response.getWriter().write(jsonResponse.toString());

} catch (JSONException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            } catch (IOException e) {
                // TODO Auto-generated catch block
                e.printStackTrace();
            }
        }
 
}
```

## Criar serviço OSGI para obter dados {#create-osgi-service}

O código a seguir foi gravado para buscar os dados armazenados do Formulário adaptativo. Um query simples é usado para buscar os dados do Formulário adaptável associados a um determinado GUID. Os dados obtidos são retornados para o aplicativo chamador. A mesma fonte de dados criada na primeira etapa referenciada neste código.

```java
package com.techmarketing.core.impl;
import java.sql.Connection;
import java.sql.ResultSet;
import java.sql.SQLException;
import java.sql.Statement;
 
import javax.sql.DataSource;
 
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
 
import com.techmarketing.core.AemFormsAndDB;
 
 
@Component(service=AemFormsAndDB.class,immediate = true)
public class AemformWithDB implements AemFormsAndDB {
    private final Logger log = LoggerFactory.getLogger(getClass());
     @Reference(target = "(&(objectclass=javax.sql.DataSource)(datasource.name=aemformstutorial))")
        private DataSource dataSource;
 
    @Override
    public String getData(String guid) {
        System.out.println("### inside my getData of AemformWithDB");
        Connection con = getConnection();
        try {
            Statement st = con.createStatement();
            String query = "SELECT afdata FROM aemformstutorial.formdata where guid = '"+guid+"'"+"";
            log.debug(" Got Result Set"+query);
            ResultSet rs = st.executeQuery(query);
            while(rs.next())
            {
                return rs.getString("afdata");
            }
        } catch (SQLException e) {
            // TODO Auto-generated catch block
            e.printStackTrace();
        }
 
        return null;
    }
     public Connection getConnection() {
            log.debug("Getting Connection ");
            Connection con = null;
            try {
                con = dataSource.getConnection();
                log.debug("got connection");
                return con;
            } catch (Exception e) {
                log.debug("not able to get connection ");
                e.printStackTrace();
            }
            return null;
        }
 
 
}
```

## Criar biblioteca de cliente {#create-client-library}

AEM Biblioteca de clientes gerencia todo o código javascript do lado do cliente. Para este artigo, criei um javascript simples para buscar os dados do formulário adaptável usando a API de ponte guia. Quando os dados do Formulário adaptável são obtidos, a chamada de POST é feita ao servlet para inserir ou atualizar os dados do formulário adaptável no banco de dados. A função getALLUrlParams retorna os parâmetros no URL. Isso é usado quando você deseja atualizar os dados. O restante da funcionalidade é tratado no código associado ao evento click da classe .savebutton. Se o parâmetro guid estiver presente no URL, então precisamos executar a operação de atualização, caso não seja uma operação de inserção.

```javascript
function getAllUrlParams(url) {
 
  // get query string from url (optional) or window
  var queryString = url ? url.split('?')[1] : window.location.search.slice(1);
 
  // we'll store the parameters here
  var obj = {};
 
  // if query string exists
  if (queryString) {
 
    // stuff after # is not part of query string, so get rid of it
    queryString = queryString.split('#')[0];
 
    // split our query string into its component parts
    var arr = queryString.split('&');
 
    for (var i = 0; i < arr.length; i++) {
      // separate the keys and the values
      var a = arr[i].split('=');
 
      // set parameter name and value (use 'true' if empty)
      var paramName = a[0];
      var paramValue = typeof (a[1]) === 'undefined' ? true : a[1];
 
      // (optional) keep case consistent
      paramName = paramName.toLowerCase();
      if (typeof paramValue === 'string') paramValue = paramValue.toLowerCase();
 
      // if the paramName ends with square brackets, e.g. colors[] or colors[2]
      if (paramName.match(/\[(\d+)?\]$/)) {
 
        // create key if it doesn't exist
        var key = paramName.replace(/\[(\d+)?\]/, '');
        if (!obj[key]) obj[key] = [];
 
        // if it's an indexed array e.g. colors[2]
        if (paramName.match(/\[\d+\]$/)) {
          // get the index value and add the entry at the appropriate position
          var index = /\[(\d+)\]/.exec(paramName)[1];
          obj[key][index] = paramValue;
        } else {
          // otherwise add the value to the end of the array
          obj[key].push(paramValue);
        }
      } else {
        // we're dealing with a string
        if (!obj[paramName]) {
          // if it doesn't exist, create property
          obj[paramName] = paramValue;
        } else if (obj[paramName] && typeof obj[paramName] === 'string'){
          // if property does exist and it's a string, convert it to an array
          obj[paramName] = [obj[paramName]];
          obj[paramName].push(paramValue);
        } else {
          // otherwise add the property
          obj[paramName].push(paramValue);
        }
      }
    }
  }
 
  return obj;
}
 
$(document).ready(function()
   {
        var linktext = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].info[0].linktxt[0]");
        var linktext1 = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].info[0].linktext1[0]");
       linktext.visible = false;
       linktext1.visible = false;
        $(".savebutton").click(function(){
           var params = getAllUrlParams(window.location.href);
           console.log(getAllUrlParams(window.location.href));
            window.guideBridge.getDataXML({
                 success:function(guideResultObject) 
                 {
                     console.log("The data is "+guideResultObject.data);
                     let xhr = new XMLHttpRequest();
                      xhr.open('POST','/bin/storeafdata');
                     let formData = new FormData();
                     if(typeof(params.guid)!="undefined")
                     {
                         formData.append("operation","update");
                         formData.append("guid",params.guid);
 
                     }
                     if(typeof(params.guid)=="undefined")
                     {
                         formData.append("operation","insert");
 
 
                     }
 
 
                formData.append("formdata",guideResultObject.data);
                xhr.send(formData);
                     xhr.onload = function(e)
                {
                    console.log("The data is ready");
                    if (this.status == 200)
                        {
                            var jsonResponse = JSON.parse(this.response);
                            console.log(jsonResponse.guid);
                            var linktext = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].info[0].linktxt[0]");
                            var linktext1 = guideBridge.resolveNode("guide[0].guide1[0].guideRootPanel[0].info[0].linktext1[0]");
                            linktext1.visible = true;
                            linktext.value = "http://localhost:4502/content/dam/formsanddocuments/saveformdata/jcr:content?wcmmode=disabled&guid="+jsonResponse.guid;
                            linktext.visible = true;
                            guideBridge.setFocus("guide[0].guide1[0].guideRootPanel[0].info[0].linktxt[0]");
                        }
 
                }
                  }
             });
 
 
       });
 
 
});
```

## Criar modelo de formulário adaptável e componente de página {#form-template-and-page-component}


>[!VIDEO](https://video.tv.adobe.com/v/27828?quality=9&learn=on)

### Demonstração do recurso {#capability-demo}

>[!VIDEO](https://video.tv.adobe.com/v/27829?quality=9&learn=on)

#### Implantar no servidor {#deploy-on-your-server}

Para testar esse recurso em sua instância do AEM Forms, siga as etapas a seguir

* [Baixe e descompacte o DemoAssets.zip em seu sistema local](assets/demoassets.zip)
* Implante e start os pacotes techmarketingdemos.jar e mysqldriver.jar usando o console da Web Felix.
*** Importe o aemformstutorial.sql usando o MYSQL Workbench. Isso criará o schema e as tabelas necessários no banco de dados
* Importe StoreAndRetrieve.zip usando AEM gerenciador de pacote. Este pacote contém o modelo de formulário adaptável, a biblioteca do cliente do componente de página e a amostra de configuração de formulário adaptável e fonte de dados.
* Faça logon no configMgr. Procure &quot;Apache Sling Connection Pooling DataSource. Abra a entrada da fonte de dados associada ao aemformstutorial e insira o nome de usuário e a senha específicos da instância do banco de dados.
* Abra o formulário adaptativo
* Preencha alguns detalhes e clique no botão &quot;Salvar e continuar mais tarde&quot;
* Você deve recuperar um URL com um GUID.
* Copie o URL e cole-o em uma nova guia do navegador
* O formulário adaptável deve ser preenchido com os dados da etapa anterior**
