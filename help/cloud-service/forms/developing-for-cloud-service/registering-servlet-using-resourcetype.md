---
title: Registrar um servlet usando o tipo de recurso
description: Mapear um servlet para um tipo de recurso no AEM Forms CS
solution: Experience Manager
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Experience Manager as a Cloud Service
topic: Development
feature: Developer Tools
jira: KT-14581
duration: 90
exl-id: 2a33a9a9-1eef-425d-aec5-465030ee9b74
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '378'
ht-degree: 2%

---

# Introdução

A vinculação de servlets por caminhos tem várias desvantagens em comparação à vinculação por tipos de recursos, a saber:

* Os servlets vinculados a caminhos não podem ser controlados pelo acesso usando as ACLs de repositório JCR padrão
* Os servlets vinculados a caminhos só podem ser registrados em um caminho e não em um tipo de recurso (ou seja, sem tratamento de sufixos)
* Se um servlet vinculado a caminho não estiver ativo, por exemplo, se o pacote estiver ausente ou não for iniciado, um POST pode resultar em resultados inesperados. normalmente cria um nó em `/bin/xyz` que subsequentemente sobrepõe a associação de caminho de servlets
o mapeamento não é transparente para um desenvolvedor que olha apenas para o repositório
Dadas essas desvantagens, é altamente recomendável vincular servlets a tipos de recursos em vez de caminhos

## Criar Servlet

Inicie seu projeto aem-banking no IntelliJ. Crie um servlet chamado GetFieldChoices na pasta de servlets, conforme mostrado na captura de tela abaixo.
![escolhas](assets/fetchchoices.png)

## Servlet de amostra

O seguinte servlet está associado ao tipo de recurso Sling: _**azure/fetchchoices**_



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

* Faça logon no AEM SDK local.
* Crie um recurso chamado `fetchchoices` (você pode nomear este nó como quiser) do tipo `cq:Page` no nó de conteúdo.
* Salve as alterações
* Crie um nó chamado `jcr:content` do tipo `cq:PageContent` e salve as alterações
* Adicionar as seguintes propriedades ao nó `jcr:content`

| Nome de propriedade | Valor de propriedade |
|--------------------|--------------------|
| jcr:title | Servlets de utilitário |
| sling:resourceType | `azure/fetchchoices` |


O valor `sling:resourceType` deve corresponder a resourceTypes=&quot;azure/fetchchoices especificados no servlet.

Agora você pode chamar seu servlet solicitando o recurso com `sling:resourceType` = `azure/fetchchoices` em seu caminho completo, com quaisquer seletores ou extensões registradas no servlet Sling.

```html
http://localhost:4502/content/fetchchoices/jcr:content.json?formPath=/content/forms/af/forrahul/jcr:content/guideContainer
```

O caminho `/content/fetchchoices/jcr:content` é o caminho do recurso e a extensão `.json` é a especificada no servlet

## Sincronizar seu projeto do AEM

1. Abra o projeto do AEM no seu editor favorito. Usei o IntelliJ para isso.
1. Criar uma pasta chamada `fetchchoices` em `\aem-banking-application\ui.content\src\main\content\jcr_root\content`
1. Clique com o botão direito do mouse na pasta `fetchchoices` e selecione `repo | Get Command` (Este item de menu está configurado em um capítulo anterior deste tutorial).

Isso deve sincronizar esse nó do AEM com o projeto AEM local.

A estrutura do projeto do AEM deve ficar assim
![resolvedor de recursos](assets/mapping-servlet-resource.png)
Atualize o filter.xml na pasta aem-banking-application\ui.content\src\main\content\META-INF\vault com a seguinte entrada

```xml
<filter root="/content/fetchchoices" mode="merge"/>
```

Agora você pode enviar as alterações para um ambiente do AEM as a Cloud Service usando o Cloud Manager.

## Próximas etapas

[Habilitar Componentes do Portal Forms](./forms-portal-components.md)
