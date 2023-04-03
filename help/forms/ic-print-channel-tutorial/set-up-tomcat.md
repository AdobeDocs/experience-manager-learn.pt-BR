---
title: Vídeo Instalar e configurar o Tomcat
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
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '235'
ht-degree: 0%

---

# Instalar e configurar o Tomcat {#install-and-configure-tomcat}

Nessa parte, instalamos o TOMCAT e implantamos o arquivo sampleRest.war no TOMCAT. O terminal REST exposto por esse arquivo WAR é a base para nossa Fonte de dados e Modelo de dados de formulário.

>[!VIDEO](https://video.tv.adobe.com/v/37815?quality=12&learn=on)

Para configurar o tomcat, siga as seguintes instruções:

1. Baixe e instale o JDK1.8.
2. Defina JAVA_HOME para apontar para JDK1.8.
3. Baixar [tomcat](https://tomcat.apache.org/). Este arquivo war foi testado com o Tomcat versão 8.5.x e 9.0.x.
4. Baixe a versão tomcat de sua preferência. Você pode baixar o zip do windows de 64 bits na seção principal.
5. Descompacte o conteúdo em sua c:\tomcat.
6. Você deve ver algo como isso na unidade c **c:\tomcat\apache-tomcat-8.5.27** dependendo da versão do seu tomcat
7. Crie uma variável de ambiente chamada &quot;CATALINA_HOME&quot; e defina seu valor para o exemplo da pasta de instalação do tomcat c:\tomcat\apache- tomcat-8.5.27
8. Copie o arquivo SampleRest.war na pasta webapps da instalação do Tomcat
9. Inicie a nova janela de prompt de comando.
10. Navegar para &lt;tomcat install=&quot;&quot; folder=&quot;&quot;>\bin e execute o startup.bat
11. Depois que o tomcat for iniciado, teste o terminal exposto pelo arquivo WAR por [clicando aqui](http://localhost:8080/SampleRest/webapi/getStatement/9586)
12. Você deve obter dados de amostra como resultado dessa chamada.

Parabéns !!!. Você configurou o tomcat e implantou o arquivo SampleRest.war.

O vídeo a seguir explica a implantação do aplicativo de amostra no Tomcat
>[!VIDEO](https://video.tv.adobe.com/v/37815?quality=12&learn=on)
