---
title: Depuração remota do SDK do AEM
description: A inicialização rápida local do SDK do AEM permite a depuração remota do Java no IDE, permitindo que você passe pela execução do código ativo no AEM para entender o fluxo de execução exato.
kt: 5251
topic: Development
feature: Developer Tools
role: Developer
level: Beginner, Intermediate
thumbnail: 34338.jpeg
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '275'
ht-degree: 0%

---


# Depuração remota do SDK do AEM

>[!VIDEO](https://video.tv.adobe.com/v/34338/?quality=12&learn=on)

A inicialização rápida local do SDK do AEM permite a depuração remota do Java no IDE, permitindo que você passe pela execução do código ativo no AEM para entender o fluxo de execução exato.

Para conectar um depurador remoto ao AEM, a inicialização rápida local do SDK AEM deve ser iniciada com parâmetros específicos (`-agentlib:...`) permitindo que o IDE se conecte a ele.

```
$ java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005 -jar aem-author-p4502.jar   
```

+ `address` especifica a porta AEM escuta para conexões de depuração remota e pode ser alterada para qualquer porta disponível na máquina de desenvolvimento local.
+ O último parâmetro (por exemplo, `aem-author-p4502.jar`) é o AEM SKD Quickstart Jar. Pode ser o serviço Autor do AEM (`aem-author-p4502.jar`) ou o serviço de Publicação do AEM (`aem-publish-p4503.jar`).

## Instruções de configuração do IDE

A maioria dos Java IDE oferece suporte para depuração remota de programas Java, no entanto, as etapas de configuração exatas de cada IDE variam. Revise as instruções de configuração da depuração remota do IDE para as etapas exatas. Normalmente, as configurações do IDE exigem:

+ O início rápido local do SDK do host AEM está escutando, que é `localhost`.
+ A porta AEM início rápido local do SDK está escutando a conexão de depuração remota, que é a porta especificada pelo parâmetro `address` ao iniciar AEM início rápido local do SDK.
+ Ocasionalmente, os projetos Maven que fornecem o código-fonte para depuração remota devem ser especificados; este é o(s) projeto(s) de projetos maven do pacote OSGi.

### Configurar instruções

+ [Configuração do depurador remoto Java do Código VS](https://code.visualstudio.com/docs/java/java-debugging)
+ [Configuração do depurador remoto IntelliJ IDEA](https://www.jetbrains.com/help/idea/tutorial-remote-debug.html)
+ [Configuração do Eclipse Remote Debugger](https://javapapers.com/core-java/java-remote-debug-with-eclipse/)
