---
title: Acionar o fluxo de trabalho do AEM no envio do formulário do HTML5 - Manipular o envio do formulário
description: Saiba como acionar o fluxo de trabalho do AEM quando o formulário do HTML5 é enviado e armazenar os dados enviados no repositório.
feature: Mobile Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
badgeVersions: label="AEM Forms 6.5" before-title="false"
level: Experienced
jira: kt-16215
exl-id: e0bde892-1da0-4b72-a408-ad7b84086939
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '171'
ht-degree: 1%

---

# Armazenar dados enviados

A próxima etapa é armazenar os dados enviados no repositório do autor do AEM. O servlet montado em `/bin/startworkflow` salva os dados enviados.
Um iniciador de fluxo de trabalho do AEM é configurado para disparar sempre que um novo recurso do tipo `nt:file` for criado no nó &lt;node_to_store_submit_data>. Esse fluxo de trabalho criará um PDF não interativo ou estático ao mesclar os dados enviados com o modelo xdp. O pdf gerado é então atribuído a um usuário para análise e aprovação.

Para armazenar os dados enviados no nó &lt;node_to_store_submit_data>, usamos o serviço OSGi `GetResolver`, que nos permite salvar os dados enviados usando o usuário do sistema `fd-service` disponível em cada instalação do AEM Forms.

O nó sob o qual os dados enviados são armazenados pode ser configurado usando o ConfigMgr, conforme explicado em [implantar ativos de amostra](./deploy-assets.md)

```java
package com.aemforms.mobileforms.core.servlets;

import java.io.ByteArrayInputStream;
import java.io.IOException;
import java.io.InputStream;
import java.io.InputStreamReader;
import java.util.UUID;

import javax.jcr.Binary;
import javax.jcr.Node;
import javax.jcr.RepositoryException;
import javax.jcr.Session;
import javax.servlet.Servlet;
import javax.servlet.ServletOutputStream;
import com.aemforms.mobileforms.core.configuration.service.AemServerCredentialsConfigurationService;
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.resource.Resource;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.apache.sling.servlets.annotations.SlingServletResourceTypes;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
import com.mergeandfuse.getserviceuserresolver.GetResolver;

import org.apache.sling.api.servlets.SlingAllMethodsServlet;

@Component(service={Servlet.class}, property={"sling.servlet.methods=post", "sling.servlet.paths=/bin/startworkflow"})
public class StartWorkflow extends SlingAllMethodsServlet {

    private static Logger logger = LoggerFactory.getLogger(StartWorkflow.class);

    @Reference
    private GetResolver getResolver;
    
    @Reference
    private AemServerCredentialsConfigurationService aemServerCredentialsConfigurationService;
    
    @Override
    protected void doPost(SlingHttpServletRequest request, SlingHttpServletResponse response)
    {
            String xmlData = null;
            logger.debug("in start workflow");
            response.setContentType("text/html;charset=UTF-8");
            if (request.getParameter("xmlData") != null)
                {
                    logger.debug("The form was submitted from Acrobat/Reader");
                    xmlData = request.getParameter("xmlData");
                    logger.debug("In start workflow" + xmlData);
                }
                logger.debug("Trying to get resource  "+aemServerCredentialsConfigurationService.getFolderPath());
                Resource r = getResolver.getFormsServiceResolver().getResource(aemServerCredentialsConfigurationService.getFolderPath());
                String responseMessage =null;
                if(r!= null)
                {

                    Session session = r.getResourceResolver().adaptTo(Session.class);
                    logger.debug("Got reosurce pdfsubmissions" + r.getPath());
                    UUID uidName = UUID.randomUUID();
                    Node xmlDataFilesNode = r.adaptTo(Node.class);
                    InputStream is = new ByteArrayInputStream(xmlData.getBytes());
                    Binary binary;
                    try {
                            Node xmlFileNode = xmlDataFilesNode.addNode(uidName.toString(), "nt:file");
                            logger.debug("Added nt file node");
                            Node jcrContent = xmlFileNode.addNode("jcr:content", "nt:resource");
                            logger.debug("Added jcr content");
                            binary = session.getValueFactory().createBinary(is);
                            jcrContent.setProperty("jcr:data", binary);
                            session.save();
                        } catch (RepositoryException e)
                        {
                            throw new RuntimeException(e);
                        }
                    responseMessage = "Your form was successfully submitted";
                }else
                {
                    logger.debug("The resource IS NULL");
                    responseMessage = "Error is processing your submission!!! Please contact the administrator";
                }

        try {
            response.setContentType("text/plain");
            response.getWriter().write(responseMessage);

        } catch (IOException e) {
            throw new RuntimeException(e);
        }
    }

}
```

## Próximas etapas

[Inicializador e fluxo de trabalho do fluxo de trabalho](./review-workflow.md)
