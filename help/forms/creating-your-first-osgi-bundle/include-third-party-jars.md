---
title: Inclusão de jars de terceiros
description: Saiba como usar o arquivo jar de terceiros no projeto do AEM
version: 6.4,6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
kt: 11245
last-substantial-update: 2022-10-15T00:00:00Z
thumbnail: third-party.jpg
source-git-commit: 4af14b7d72ebdbea04e68a9a64afa1a96d1c1aeb
workflow-type: tm+mt
source-wordcount: '282'
ht-degree: 0%

---

# Inclusão de pacotes de terceiros em seu projeto do AEM

Neste artigo, abordaremos as etapas envolvidas na inclusão do pacote OSGi de terceiros no seu projeto do AEM.Para a finalidade deste artigo, vamos incluir o [jsch-0.1.55.jar](https://repo1.maven.org/maven2/com/jcraft/jsch/0.1.55/jsch-0.1.55.jar) no nosso projeto AEM.  Se o OSGi estiver disponível no repositório Maven, inclua a dependência do pacote no arquivo POM.xml do projeto.

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

Se o seu pacote OSGi estiver em seu sistema de arquivos, crie uma pasta chamada **localjar** no diretório base do seu projeto (C:\aemformsbundles\AEMFormsProcessStep\localjar) a dependência será semelhante a esta

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

Estamos adicionando este pacote ao nosso projeto AEM **AEMormsProcessStep** que residam no **c:\aemformsbundles** pasta

* Abra o **filter.xml** no diretório C:\aemformsbundles\AEMFormsProcessStep\all\src\main\content\META-INF\vault folder of your project Make a note of the root attribute of the filter element.

* Crie a seguinte estrutura de pastas C:\aemformsbundles\AEMFormsProcessStep\all\src\main\content\jcr_root\apps\AEMFormsProcessStep-vendor-packages\application\install
* O **apps/AEMFormsProcessStep-vendor-packages** é o valor do atributo raiz em filter.xml
* Atualize a seção de dependências do POM.xml do projeto
* Abra o prompt de comando. No meu caso, acesse a pasta do seu projeto (c:\aemformsbundles\AEMFormsProcessStep). Execute o seguinte comando

```java
mvn clean install -PautoInstallSinglePackage
```

Se tudo correr bem, o pacote será instalado junto com o pacote de terceiros na instância do AEM. Você pode verificar o pacote usando [console da web felix](http://localhost:4502/system/console/bundles). O pacote de terceiros está disponível na pasta /apps do `crx` repositório como mostrado abaixo
![terceiros](assets/custom-bundle1.png)



