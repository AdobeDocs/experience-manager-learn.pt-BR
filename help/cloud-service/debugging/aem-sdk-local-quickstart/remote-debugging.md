---
title: Depuração remota do SDK do AEM
description: A inicialização rápida local do SDK do AEM permite a depuração remota do Java a partir do IDE, permitindo que você passe pela execução do código ativo no AEM para entender o fluxo de execução exato.
jira: KT-5251
topic: Development
feature: Developer Tools
role: Developer
level: Beginner, Intermediate
thumbnail: 34338.jpeg
exl-id: beac60c6-11ae-4d0c-a055-cd3d05aeb126
duration: 428
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '270'
ht-degree: 0%

---

# Depuração remota do SDK do AEM

>[!VIDEO](https://video.tv.adobe.com/v/34338?quality=12&learn=on)

A inicialização rápida local do SDK do AEM permite a depuração remota do Java a partir do IDE, permitindo que você passe pela execução do código ativo no AEM para entender o fluxo de execução exato.

Para conectar um depurador remoto ao AEM, a inicialização rápida local do SDK AEM deve ser iniciada com parâmetros específicos (`-agentlib:...`), permitindo que o IDE se conecte a ele.

```
$ java -agentlib:jdwp=transport=dt_socket,server=y,suspend=n,address=*:5005 -jar aem-author-p4502.jar   
```

+ O SDK do AEM é compatível apenas com o Java 11
+ `address` especifica a porta em que o AEM escuta para conexões de depuração remota e pode ser alterado para qualquer porta disponível no computador de desenvolvimento local.
+ O último parâmetro (por exemplo, `aem-author-p4502.jar`) é o Jar de início rápido do SDK do AEM. Pode ser o serviço de Autor AEM (`aem-author-p4502.jar`) ou o serviço AEM Publish (`aem-publish-p4503.jar`).


## Instruções de configuração do IDE

A maioria das IDEs Java fornece suporte para a depuração remota de programas Java, no entanto, as etapas de configuração exatas de cada IDE variam. Revise as instruções de configuração de depuração remota do IDE para obter as etapas exatas. Normalmente, as configurações de IDE exigem:

+ A inicialização rápida local do SDK do AEM do host está escutando, que é `localhost`.
+ A inicialização rápida local do SDK do AEM da porta está escutando a conexão de depuração remota, que é a porta especificada pelo parâmetro `address` ao iniciar a inicialização rápida local do SDK do AEM.
+ Ocasionalmente, os projetos Maven que fornecem o código-fonte para depuração remota devem ser especificados; este é seu(s) projeto(s) de pacote Maven OSGi.

### Configurar instruções

+ [Depurador Remoto Java do Código VS configurado](https://code.visualstudio.com/docs/java/java-debugging)
+ [Depurador Remoto IntelliJ IDEA configurado](https://www.jetbrains.com/help/idea/tutorial-remote-debug.html)
+ [Depurador remoto do Eclipse configurado](https://javapapers.com/core-java/java-remote-debug-with-eclipse/)
