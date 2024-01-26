---
title: Registrar um servlet usando o tipo de recurso
description: Mapear um servlet para um tipo de recurso no AEM Forms CS
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
feature: Developer Tools
jira: KT-14581
duration: 108
exl-id: 2a33a9a9-1eef-425d-aec5-465030ee9b74
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 2%

---

# Introdução

A vinculação de servlets por caminhos tem várias desvantagens em comparação à vinculação por tipos de recursos, a saber:

* Os servlets vinculados a caminhos não podem ser controlados pelo acesso usando as ACLs de repositório JCR padrão
* Os servlets vinculados a caminhos só podem ser registrados em um caminho e não em um tipo de recurso (ou seja, sem tratamento de sufixos)
* Se um servlet vinculado a caminho não estiver ativo, por exemplo, se o pacote estiver ausente ou não for iniciado, um POST pode resultar em resultados inesperados. geralmente criando um nó em `/bin/xyz` que subsequentemente sobrepõe o caminho de servlets vinculando o mapeamento não é transparente para um desenvolvedor que procura apenas no repositório Dadas essas desvantagens, é altamente recomendável vincular servlets a tipos de recursos em vez de caminhos

## Criar Servlet

Inicie seu projeto aem-banking no IntelliJ. Crie um servlet chamado GetFieldChoices na pasta de servlets, conforme mostrado na captura de tela abaixo.
![opções](assets/fetchchoices.png)

## Servlet de amostra

O servlet a seguir está vinculado ao tipo de recurso Sling: _**azure/fetchchoices**_



```java
import org.apache.sling.api.SlingHttpServletRequest;
import org.apache.sling.api.SlingHttpServletResponse;
import org.apache.sling.api.servlets.SlingAllMethodsServlet;
import org.apache.sling.servlets.annotations.SlingServletResourceTypes;
import org.osgi.framework.Constants;
import org.osgi.service.component.annotations.Component;
import org.osgi.service.component.annotations.Reference;
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;

import javax.jcr.Session;
import javax.servlet.Servlet;
import java.io.IOException;
import java.io.Serializable;

@Component(
        service={Servlet.class }
)

        @SlingServletResourceTypes(
                resourceTypes="azure/fetchchoices",
                methods= "GET",
                extensions="json"
                )


public class GetFieldChoices extends SlingAllMethodsServlet implements Serializable {
    private static final long serialVersionUID = 1L;
    private final  transient Logger log = LoggerFactory.getLogger(this.getClass());


   

    protected void doGet(SlingHttpServletRequest request, SlingHttpServletResponse response) {

        log.debug("The form path I got was "+request.getParameter("formPath"));

    }
}
```

## Criar recursos no CRX

* Faça logon no seu SDK AEM local.
* Crie um recurso chamado `fetchchoices` (você pode nomear esse nó como quiser) do tipo `cq:Page` no nó de conteúdo.
* Salve as alterações
* Crie um nó chamado `jcr:content` do tipo `cq:PageContent` e salve as alterações
* Adicione as seguintes propriedades à `jcr:content` nó

| Nome da Propriedade | Valor da propriedade |
|--------------------|--------------------|
| jcr:title | Servlets de utilitário |
| sling:resourceType | `azure/fetchchoices` |


A variável `sling:resourceType` o valor deve corresponder a resourceTypes=&quot;azure/fetchchoices especificados no servlet.

Agora você pode chamar seu servlet solicitando o recurso com `sling:resourceType` = `azure/fetchchoices` no caminho completo, com qualquer seletor ou extensão registrada no servlet Sling.

```html
http://localhost:4502/content/fetchchoices/jcr:content.json?formPath=/content/forms/af/forrahul/jcr:content/guideContainer
```

O caminho `/content/fetchchoices/jcr:content` é o caminho do recurso e da extensão `.json` é o que está especificado no servlet

## Sincronizar o projeto AEM

1. Abra o projeto AEM no seu editor favorito. Usei o IntelliJ para isso.
1. Crie uma pasta chamada `fetchchoices` em `\aem-banking-application\ui.content\src\main\content\jcr_root\content`
1. Clique com o botão direito `fetchchoices` e selecione `repo | Get Command` (Este item de menu é configurado em um capítulo anterior deste tutorial).

Isso deve sincronizar esse nó do AEM com o projeto AEM local.

A estrutura do projeto AEM deve ficar assim
![solucionador de recursos](assets/mapping-servlet-resource.png)
Atualize o filter.xml na pasta aem-banking-application\ui.content\src\main\content\META-INF\vault com a seguinte entrada

```xml
<filter root="/content/fetchchoices" mode="merge"/>
```

Agora você pode enviar suas alterações para um ambiente as a Cloud Service AEM usando o Cloud Manager.

## Próximas etapas

[Habilitar Componentes do Portal Forms](./forms-portal-components.md)
