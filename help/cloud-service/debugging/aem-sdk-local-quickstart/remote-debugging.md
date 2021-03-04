---
title: Depuração remota do SDK do AEM
description: A inicialização rápida local do SDK do AEM permite a depuração remota do Java no IDE, permitindo que você passe pela execução do código ativo no AEM para entender o fluxo de execução exato.
feature: Ferramentas do desenvolvedor
topics: development
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 5251
topic: Desenvolvimento
role: Desenvolvedor
level: Iniciante, Intermediário
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '281'
ht-degree: 1%

---


# Depuração remota do SDK do AEM

>[!VIDEO](https://video.tv.adobe.com/v/34338/?quality=12&learn=on)

A inicialização rápida local do SDK do AEM permite a depuração remota do Java no IDE, permitindo que você passe pela execução do código ativo no AEM para entender o fluxo de execução exato.

Para conectar um depurador remoto ao AEM, a inicialização rápida local do SDK do AEM deve ser iniciada com parâmetros específicos (`-agentlib:...`) permitindo que o IDE se conecte a ele.

```
$ java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=5005 -jar aem-author-p4502.jar   
```

+ `address` especifica a porta que o AEM escuta para conexões de depuração remota e pode ser alterada para qualquer porta disponível na máquina de desenvolvimento local.
+ O último parâmetro (por exemplo, `aem-author-p4502.jar`) é o AEM SDK Quickstart Jar. Pode ser o serviço Autor do AEM (`aem-author-p4502.jar`) ou o serviço de Publicação do AEM (`aem-publish-p4503.jar`).

## Instruções de configuração do IDE

A maioria dos Java IDE oferece suporte para depuração remota de programas Java, no entanto, as etapas de configuração exatas de cada IDE variam. Revise as instruções de configuração da depuração remota do IDE para as etapas exatas. Normalmente, as configurações do IDE exigem:

+ A inicialização rápida local do SDK do AEM host está ouvindo, que é `localhost`.
+ A porta de inicialização rápida local do SDK do AEM está escutando a conexão de depuração remota, que é a porta especificada pelo parâmetro `address` ao iniciar a inicialização rápida local do SDK do AEM.
+ Ocasionalmente, os projetos Maven que fornecem o código-fonte para depuração remota devem ser especificados; este é o(s) projeto(s) de projetos maven do pacote OSGi.

### Configurar instruções

+ [Configuração do depurador remoto Java do Código VS](https://code.visualstudio.com/docs/java/java-debugging)
+ [Configuração do depurador remoto IntelliJ IDEA](https://www.jetbrains.com/help/idea/run-debug-configuration-remote-debug.html)
+ [Configuração do Eclipse Remote Debugger](https://javapapers.com/core-java/java-remote-debug-with-eclipse/)
