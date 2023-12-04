---
title: Adicionar itens da pasta DAM ao componente do grupo de opções
description: Adicionar itens ao componente do grupo de opções dinamicamente
feature: Adaptive Forms
version: 6.5
topic: Development
role: User
level: Beginner
last-substantial-update: 2023-01-01T00:00:00Z
exl-id: 29f56d13-c2e2-4bc2-bfdc-664c848dd851
duration: 122
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '230'
ht-degree: 0%

---

# Adicionar itens dinamicamente ao componente do grupo de opções

O AEM Forms 6.5 introduziu a capacidade de adicionar itens dinamicamente a um componente de grupo de opções do Adaptive Forms, como CheckBox, Botão de opção e Lista de imagens. Neste artigo, analisaremos o caso de uso de preenchimento de um componente de grupo de escolha com o conteúdo da pasta DAM. Na captura de tela, os 3 arquivos estão na pasta chamada informativo. Sempre que um novo informativo for adicionado à pasta, o componente do grupo de opções será atualizado para listar seu conteúdo automaticamente. O usuário pode selecionar um ou mais informativos para baixar.

![Editor de regras](assets/newsletters-download.png)

## Criar servlet para retornar o conteúdo da pasta DAM

O código a seguir foi gravado para retornar o conteúdo da pasta DAM no formato JSON.

```java
package com.newsletters.core.servlets;
import static com.day.cq.commons.jcr.JcrConstants.JCR_CONTENT;
import java.io.IOException;
import java.io.PrintWriter;
import java.util.ArrayList;
import java.util.List;
import javax.servlet.Servlet;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.servlets.SlingSafeMethodsServlet;
import org.osgi.service.component.annotations.Component;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.google.gson.Gson;
import com.google.gson.JsonObject;

@Component(service = {
  Servlet.class
}, property = {
  "sling.servlet.methods=get",
  "sling.servlet.paths=/bin/listfoldercontents"
})
public class ListFolderContent extends SlingSafeMethodsServlet {
  private static final long serialVersionUID = 1 L;
  private static final Logger log = LoggerFactory.getLogger(ListFolderContent.class);
  protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {
    Resource resource = request.getResourceResolver().getResource(request.getParameter("damFolder"));
    List < JsonObject > results = new ArrayList < > ();
    resource.getChildren().forEach(child -> {
      if (!JCR_CONTENT.equals(child.getName())) {
        JsonObject asset = new JsonObject();
        log.debug("##The child name is " + child.getName());
        asset.addProperty("assetname", child.getName());
        asset.addProperty("assetpath", child.getPath());
        results.add(asset);

      }
    });
    PrintWriter out = null;
    try {
      out = response.getWriter();
    } catch (IOException e) {

      log.debug(e.getMessage());
    }
    response.setContentType("application/json");
    response.setCharacterEncoding("UTF-8");
    Gson gson = new Gson();
    out.print(gson.toJson(results));
    out.flush();
  }

}
```

## Criar biblioteca do cliente com a função JavaScript

O servlet é chamado de uma função JavaScript. A função retorna um objeto de matriz que será usado para preencher o componente do grupo de opções

```javascript
/**
 * Populate drop down/choice group  with assets from specified folder
 * @return {string[]} 
 */
function getDAMFolderAssets(damFolder) {
   // strUrl is whatever URL you need to call
   var strUrl = '/bin/listfoldercontents?damFolder=' + damFolder;
   var documents = [];
   $.ajax({
      url: strUrl,
      success: function(jsonData) {
         for (i = 0; i < jsonData.length; i++) {
            documents.push(jsonData[i].assetpath + "=" + jsonData[i].assetname);
         }
      },
      async: false
   });
   return documents;
}
```

## Criar formulário adaptável

Criar um formulário adaptável e associá-lo à biblioteca do cliente **listfolderassets**. Adicione um componente de caixa de seleção ao formulário. Use o editor de regras para preencher as opções da caixa de seleção, conforme mostrado na captura de tela
![set- options](assets/set-options-newsletter.png)

Estamos chamando a função javascript chamada **getDAMFolderAssets** e transmitindo o caminho dos ativos da pasta DAM para a lista no formulário.

## Próximas etapas

[Montar ativos selecionados](./assemble-selected-newsletters.md)
