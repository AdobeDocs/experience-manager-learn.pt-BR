---
title: Instalar e configurar vídeo Tomcat
seo-title: Install and Configure Tomcat
description: Esta é a parte 1 do tutorial em várias etapas para criar seu primeiro documento de comunicações interativas.
uuid: 835e2342-82b6-4f0c-9a6b-467bbbd8527a
feature: Interactive Communication
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
thumbnail: 37815.jpg
discoiquuid: 5f68be3d-aa35-4a3f-aaea-b8ee213c87ae
topic: Development
role: Developer
level: Beginner
exl-id: faa9ca2d-6cfa-4abf-be5e-3e549202853a
source-git-commit: 48d9ddb870c0e4cd001ae49a3f0e9c547407c1e8
workflow-type: tm+mt
source-wordcount: '241'
ht-degree: 1%

---

# Instalar e configurar Tomcat {#install-and-configure-tomcat}

Nesta parte, instalamos o TOMCAT e implantamos o arquivo sampleRest.war no TOMCAT. O endpoint REST exposto por esse arquivo WAR é a base para nossa Fonte de dados e Modelo de dados de formulário.

>[!VIDEO](https://video.tv.adobe.com/v/37815?quality=12&learn=on)

Para configurar o tomcat, siga as seguintes instruções:

1. Baixe e instale o JDK1.8.
2. Defina JAVA_HOME para apontar para JDK1.8.
3. Baixar [tomcat](https://tomcat.apache.org/). Esse arquivo WAR foi testado com a versão 8.5.x e 9.0.x do Tomcat.
4. Baixe a versão tomcat de sua preferência. Você pode baixar o zip do Windows de 64 bits na seção principal.
5. Descompacte o conteúdo em seu c:\tomcat.
6. Você deve ver algo como isso em sua unidade c **c:\tomcat\apache-tomcat-8.5.27** dependendo da versão do seu tomcat
7. Crie uma variável de ambiente chamada &quot;CATALINA_HOME&quot; e defina seu valor para a pasta de instalação do tomcat exemplo c:\tomcat\apache-8.5.27
8. Copie o arquivo SampleRest.war para a pasta webapps da sua instalação do Tomcat
9. Iniciar nova janela de prompt de comando.
10. Navegue até &lt;tomcat install=&quot;&quot; folder=&quot;&quot;>\bin e execute o startup.bat
11. Depois que o tomcat for iniciado, teste o endpoint exposto pelo arquivo WAR ao [clicando aqui](http://localhost:8080/SampleRest/webapi/getStatement/9586)
12. Você deve obter dados de amostra como o resultado desta chamada.

Parabéns !!!. Você configurou o tomcat e implantou o arquivo SampleRest.war.

O vídeo a seguir explica a implantação do aplicativo de amostra no Tomcat
>[!VIDEO](https://video.tv.adobe.com/v/37815?quality=12&learn=on)

## Próximas etapas

[Criar fonte de dados RESTful](./create-data-source.md)