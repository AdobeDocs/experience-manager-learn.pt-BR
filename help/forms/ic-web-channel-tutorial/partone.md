---
title: Instalar e configurar Tomcat
description: Esta é a parte 1 do tutorial em várias etapas para criar seu primeiro documento de comunicações interativas.Nesta parte, instalaremos o TOMCAT e implantaremos o arquivo sampleRest.war no TOMCAT.
feature: Interactive Communication
doc-type: Tutorial
version: Experience Manager 6.4, Experience Manager 6.5
discoiquuid: 4f400c22-6c96-4018-851c-70d988ce7c6c
topic: Development
role: Developer
level: Beginner
exl-id: f0f19838-1ade-4eda-b736-a9703a3916c2
duration: 44
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '240'
ht-degree: 0%

---

# Instalar e configurar Tomcat {#install-and-configure-tomcat}

Nesta parte, instalamos o TOMCAT e implantamos o arquivo sampleRest.war no TOMCAT. O endpoint REST exposto por esse arquivo WAR é a base para nosso Source de dados e Modelo de dados de formulário.

Para configurar o tomcat, siga as seguintes instruções:

1. Baixe e instale o JDK1.8.
2. Defina JAVA_HOME para apontar para JDK1.8.
3. Baixe [tomcat](https://tomcat.apache.org/). Esse arquivo WAR foi testado com a versão 8.5.x e 9.0.x do Tomcat.
4. Baixe a versão tomcat de sua preferência. Você pode baixar o zip do Windows de 64 bits na seção principal.
5. Descompacte o conteúdo em seu c:\tomcat.
6. Você deve ver algo como isso na unidade c **c:\tomcat\apache-tomcat-8.5.27**, dependendo da versão do seu tomcat
7. Crie uma variável de ambiente chamada &quot;CATALINA_HOME&quot; e defina seu valor para a pasta de instalação do tomcat exemplo c:\tomcat\apache-8.5.27
8. Copie o arquivo SampleRest.war para a pasta webapps da sua instalação do Tomcat
9. Iniciar nova janela de prompt de comando.
10. Navegue até &lt;pasta de instalação do tomcat>\bin e execute o arquivo startup.bat
11. Depois que o tomcat for iniciado, teste o ponto de extremidade exposto pelo Arquivo WAR [clicando aqui](http://localhost:8080/SampleRest/webapi/getStatement/9586)
12. Você deve obter dados de amostra como o resultado desta chamada.

Parabéns!!!. Você configurou o tomcat e implantou o arquivo SampleRest.war.

## Próximas etapas

[Configurar fonte de dados RESTful](./parttwo.md)
