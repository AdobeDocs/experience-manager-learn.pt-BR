---
title: Depuração remota do SDK AEM
description: O Início rápido local do SDK do AEM permite a depuração remota do Java a partir do seu IDE, permitindo que você passe pela execução do código ativo em AEM para entender o fluxo de execução exato.
feature: null
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5251
translation-type: tm+mt
source-git-commit: a3d3612713decefb5c7e1cf5b2d4d21afff0a2f5
workflow-type: tm+mt
source-wordcount: '275'
ht-degree: 0%

---


# Depuração remota do SDK AEM

>[!VIDEO](https://video.tv.adobe.com/v/34338/?quality=12&learn=on)

O Início rápido local do SDK do AEM permite a depuração remota do Java a partir do seu IDE, permitindo que você passe pela execução do código ativo em AEM para entender o fluxo de execução exato.

Para conectar um depurador remoto ao AEM, o início rápido local do SDK do AEM deve ser iniciado com parâmetros específicos (`-agentlib:...`) permitindo que o IDE se conecte a ele.

```
$ java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005 -jar aem-author-p4502.jar   
```

+ `address` especifica a porta AEM escuta para conexões de depuração remotas e pode ser alterada para qualquer porta disponível no computador de desenvolvimento local.
+ O último parâmetro (por exemplo, `aem-author-p4502.jar`) é o AEM SKD Quickstart Jar. Pode ser o serviço de autor de AEM (`aem-author-p4502.jar`) ou o serviço de publicação de AEM (`aem-publish-p4503.jar`).

## Instruções de configuração do IDE

A maioria dos IDEs Java fornecem suporte para a depuração remota de programas Java, no entanto, as etapas de configuração exatas de cada IDE variam. Consulte as instruções de configuração de depuração remota do IDE para saber mais sobre as etapas exatas. Geralmente, as configurações IDE exigem:

+ O host AEM início rápido local do SDK está acompanhando, o que é `localhost`.
+ A porta AEM início rápido local do SDK está acompanhando a conexão de depuração remota, que é a porta especificada pelo `address` parâmetro ao iniciar AEM início rápido local do SDK.
+ Ocasionalmente, os projetos Maven que fornecem o código fonte para a depuração remota devem ser especificados; este é o(s) projeto(s) de projetos maven do pacote OSGi.

### Instruções de configuração

+ [Configuração do depurador Java Remote do Código VS](https://code.visualstudio.com/docs/java/java-debugging)
+ [Configuração do depurador remoto IntelliJ IDEA](https://www.jetbrains.com/help/idea/run-debug-configuration-remote-debug.html)
+ [Configuração do depurador remoto Eclipse](https://javapapers.com/core-java/java-remote-debug-with-eclipse/)
