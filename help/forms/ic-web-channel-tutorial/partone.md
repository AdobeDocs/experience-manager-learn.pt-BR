---
title: Instalar e configurar o Tomcat
seo-title: Instalar e configurar o Tomcat
description: Esta é a parte 1 do tutorial de várias etapas para criar seu primeiro documento de comunicações interativas.Nesta parte, instalaremos o TOMCAT e implantaremos o arquivo sampleRest.war no TOMCAT. O terminal REST exposto por esse arquivo WAR será a base para nossa Fonte de dados e Modelo de dados de formulário.
seo-description: Esta é a parte 1 do tutorial de várias etapas para criar seu primeiro documento de comunicações interativas.Nesta parte, instalaremos o TOMCAT e implantaremos o arquivo sampleRest.war no TOMCAT. O terminal REST exposto por esse arquivo WAR será a base para nossa Fonte de dados e Modelo de dados de formulário.
uuid: c6d4c74c-ea16-4c63-92c9-182d087fd88c
feature: comunicação interativa
topics: development
audience: developer
doc-type: tutorial
activity: implement
version: 6.4,6.5
discoiquuid: 4f400c22-6c96-4018-851c-70d988ce7c6c
translation-type: tm+mt
source-git-commit: b040bdf97df39c45f175288608e965e5f0214703
workflow-type: tm+mt
source-wordcount: '309'
ht-degree: 0%

---


# Instalar e configurar o Tomcat {#install-and-configure-tomcat}

Nessa parte, instalaremos o TOMCAT e implantaremos o arquivo sampleRest.war no TOMCAT. O terminal REST exposto por esse arquivo WAR será a base para nossa Fonte de dados e Modelo de dados de formulário.

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
