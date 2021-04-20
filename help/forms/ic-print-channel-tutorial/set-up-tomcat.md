---
title: Instalar e configurar o Tomcat
seo-title: Instalar e configurar o Tomcat
description: Esta é a parte 1 do tutorial de várias etapas para criar seu primeiro documento de comunicações interativas.Nesta parte, instalaremos o TOMCAT e implantaremos o arquivo sampleRest.war no TOMCAT. O terminal REST exposto por esse arquivo WAR será a base para nossa Fonte de dados e Modelo de dados de formulário.
seo-description: Esta é a parte 1 do tutorial de várias etapas para criar seu primeiro documento de comunicações interativas.Nesta parte, instalaremos o TOMCAT e implantaremos o arquivo sampleRest.war no TOMCAT. O terminal REST exposto por esse arquivo WAR será a base para nossa Fonte de dados e Modelo de dados de formulário.
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
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '324'
ht-degree: 1%

---


# Instalar e configurar o Tomcat {#install-and-configure-tomcat}

Nessa parte, instalaremos o TOMCAT e implantaremos o arquivo sampleRest.war no TOMCAT. O terminal REST exposto por esse arquivo WAR será a base para nossa Fonte de dados e Modelo de dados de formulário.

>[!VIDEO](https://video.tv.adobe.com/v/37815/?quality=9&learn=on)

Para configurar o tomcat, siga as seguintes instruções:

1. Baixe e instale o JDK1.8.
2. Defina JAVA_HOME para apontar para JDK1.8.
3. Baixe [tomcat](https://tomcat.apache.org/). Este arquivo war foi testado com o Tomcat versão 8.5.x e 9.0.x.
4. Baixe a versão tomcat de sua preferência. Você pode baixar o zip do windows de 64 bits na seção principal.
5. Descompacte o conteúdo em sua c:\tomcat.
6. Você deve ver algo como isto na unidade c **c:\tomcat\apache-tomcat-8.5.27** dependendo da versão do seu tomcat
7. Crie uma variável de ambiente chamada &quot;CATALINA_HOME&quot; e defina seu valor para o exemplo da pasta de instalação do tomcat c:\tomcat\apache- tomcat-8.5.27
8. Copie o arquivo SampleRest.war na pasta webapps da instalação do Tomcat
9. Inicie a nova janela de prompt de comando.
10. Navegue até &lt;pasta de instalação tomcat>\bin e execute o startup.bat
11. Depois que o tomcat for iniciado, teste o terminal exposto pelo Arquivo WAR clicando [aqui](http://localhost:8080/SampleRest/webapi/getStatement/9586)
12. Você deve obter dados de amostra como resultado dessa chamada.

Parabéns !!!. Você configurou o tomcat e implantou o arquivo SampleRest.war.

O vídeo a seguir explica a implantação do aplicativo de amostra no Tomcat
>[!VIDEO](https://video.tv.adobe.com/v/37815)