---
title: Incluindo jars de terceiros
description: Saiba como usar o arquivo jar de terceiros no seu projeto do AEM
version: Experience Manager 6.4, Experience Manager 6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
jira: KT-11245
last-substantial-update: 2022-10-15T00:00:00Z
thumbnail: third-party.jpg
exl-id: e8841c63-3159-4f13-89a1-d8592af514e3
duration: 53
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '269'
ht-degree: 0%

---

# Incluir pacotes de terceiros em seu projeto do AEM

Neste artigo, abordaremos as etapas envolvidas na inclusão do pacote OSGi de terceiros no projeto do AEM. Para o propósito deste artigo, incluiremos o [jsch-0.1.55.jar](https://repo1.maven.org/maven2/com/jcraft/jsch/0.1.55/jsch-0.1.55.jar) no nosso projeto do AEM.  Se o OSGi estiver disponível no repositório Maven, inclua a dependência do pacote no arquivo POM.xml do projeto.

>[!NOTE]
> Pressupõe-se que o jar de terceiros seja um pacote OSGi

```java
<!-- https://mvnrepository.com/artifact/com.jcraft/jsch -->
<dependency>
    <groupId>com.jcraft</groupId>
    <artifactId>jsch</artifactId>
    <version>0.1.55</version>
</dependency>
```

Se o pacote OSGi estiver no sistema de arquivos, crie uma pasta chamada **localjar** no diretório base do projeto (C:\aemformsbundles\AEMFormsProcessStep\localjar). A dependência será semelhante a esta

```java
<dependency>
    <groupId>jsch</groupId>
    <artifactId>jsch</artifactId>
    <version>1.0</version>
    <scope>system</scope>
    <systemPath>${project.basedir}/localjar/jsch-0.1.55-bundle.jar</systemPath>
</dependency>
```

## Criar a estrutura de pastas

Estamos adicionando este pacote ao nosso projeto do AEM **AEMFormsProcessStep**, que reside na pasta **c:\aemformsbundles**

* Abra o **filter.xml** da pasta C:\aemformsbundles\AEMFormsProcessStep\all\src\main\content\META-INF\vault do seu projeto
Anote o atributo raiz do elemento de filtro.

* Crie a seguinte estrutura de pastas C:\aemformsbundles\AEMFormsProcessStep\all\src\main\content\jcr_root\apps\AEMFormsProcessStep-vendor-packages\application\install
* **apps/AEMFormsProcessStep-vendor-packages** é o valor do atributo raiz no filter.xml
* Atualize a seção de dependências do POM.xml do projeto
* Abra o prompt de comando. Acesse a pasta do projeto (c:\aemformsbundles\AEMFormsProcessStep), no meu caso. Execute o seguinte comando

```java
mvn clean install -PautoInstallSinglePackage
```

Se tudo correr bem, o pacote será instalado junto com o pacote de terceiros na instância do AEM. Você pode verificar o pacote usando o [felix web console](http://localhost:4502/system/console/bundles). O pacote de terceiros está disponível na pasta /apps do repositório `crx`, como mostrado abaixo
![terceiros](assets/custom-bundle1.png)
