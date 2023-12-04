---
title: Criar configuração OSGi personalizada
description: Configuração OSGi personalizada para capturar detalhes específicos da nuvem de documentos
solution: Experience Manager, Experience Manager Forms
type: Documentation
role: Developer
level: Beginner, Intermediate
version: Cloud Service
topic: Development
thumbnail: 7818.jpg
jira: KT-7818
exl-id: 1f34c356-6c0c-46ff-9cea-7baacfc4bb7f
duration: 41
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '63'
ht-degree: 1%

---

# Introdução

Criar configuração OSGi personalizada para capturar as credenciais da conta da nuvem do documento


Para fazer uma configuração OSGi personalizada, precisamos primeiro criar uma interface cujos métodos públicos representarão os campos na configuração.

![doc-cloud-config](assets/doc-cloud-configuration.JPG)


Crie uma interface chamada DocumentCloudConfiguration e cole o seguinte código nela.

```java
package com.aemforms.doccloud.core;

import org.osgi.service.metatype.annotations.AttributeDefinition;
import org.osgi.service.metatype.annotations.ObjectClassDefinition;

@ObjectClassDefinition(name="Document Cloud Service Configuration", description = "Connect AEM Forms With Document Cloud")
public @interface DocumentCloudConfiguration {
	  @AttributeDefinition(name="Client ID", description="Client ID")
	  String getClientID() default "";
	  
	  @AttributeDefinition(name="Client Secret", description="Client Secret")
	  public String getClientSecret() default "26dc4de1-f7f0-46b6-9e8e-86270ad34f58";
	  
	  @AttributeDefinition(name="Technical Account ID", description="Technical Account ID")
	  public String getTechnicalAccount() default "42FF05A7606F71BB0A495FBE@techacct.adobe.com";

	  @AttributeDefinition(name="Organization ID", description="Organization ID")
	  String getOrganizationID() default "299805FF5FC9199D0A495EBC@AdobeOrg";
	  
	  @AttributeDefinition(name="Metascope", description="Metascope")
	  String getMetascope() default "ent_documentcloud_sdk";


}
```
