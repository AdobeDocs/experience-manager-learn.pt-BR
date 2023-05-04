---
title: Criação do seu primeiro pacote OSGi com o AEM Forms
description: Crie seu primeiro pacote OSGi usando Maven e Eclipse
version: 6.4,6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
exl-id: 307cc3b2-87e5-4429-8f21-5266cf03b78f
last-substantial-update: 2021-04-23T00:00:00Z
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: tm+mt
source-wordcount: '672'
ht-degree: 1%

---

# Criar seu primeiro pacote OSGi

Um pacote OSGi é um arquivo de arquivo Java™ que contém código Java, recursos e um manifesto que descreve o pacote e suas dependências. O pacote é a unidade de implantação de um aplicativo. Este artigo destina-se aos desenvolvedores que desejam criar um serviço OSGi ou um servlet usando o AEM Forms 6.4 ou 6.5. Para criar seu primeiro pacote OSGi, siga as seguintes etapas:


## Instalar o JDK

Instale a versão compatível do JDK. Eu usei o JDK1.8. Verifique se você adicionou **JAVA_HOME** nas variáveis de ambiente e aponte para a pasta raiz da instalação do JDK.
Adicionar o %JAVA_HOME%/bin ao caminho

![fonte de dados](assets/java-home.JPG)

>[!NOTE]
> Não use o JDK 15. Ele não é compatível com o AEM.

### Testar a versão do JDK

Abra uma nova janela de prompt de comando e digite: `java -version`. Você deve recuperar a versão do JDK identificada pela variável `JAVA_HOME` variável

![fonte de dados](assets/java-version.JPG)

## Instalar o Maven

O Maven é uma ferramenta de automação de compilação usada principalmente para projetos Java. Siga as etapas a seguir para instalar o maven em seu sistema local.

* Crie uma pasta chamada `maven` na unidade C
* Baixe o [arquivo zip binário](https://maven.apache.org/download.cgi)
* Extraia o conteúdo do arquivo zip para `c:\maven`
* Crie uma variável de ambiente chamada `M2_HOME` com um valor de `C:\maven\apache-maven-3.6.0`. No meu caso, o **mvn** A versão é 3.6.0. No momento da escrita deste artigo, a versão mais recente do maven é 3.6.3
* Adicione o `%M2_HOME%\bin` para o seu caminho
* Salve as alterações
* Abra um novo prompt de comando e digite `mvn -version`. Você deve ver o **mvn** versão listada como mostrado na captura de tela abaixo

![fonte de dados](assets/mvn-version.JPG)


## Instalar o Eclipse

Instale a versão mais recente de [eclipse](https://www.eclipse.org/downloads/)

## Criar o primeiro projeto

O Archetype é um kit de ferramentas de modelos de projeto Maven. Um arquétipo é definido como um padrão ou modelo original a partir do qual são feitas todas as outras coisas do mesmo tipo. O nome se encaixa à medida que tentamos fornecer um sistema que fornece um meio consistente de gerar projetos Maven. O Arquétipo ajuda os autores a criar modelos de projeto Maven para usuários e fornece aos usuários os meios de gerar versões parametrizadas desses modelos de projeto.
Para criar seu primeiro projeto maven, siga as seguintes etapas:

* Crie uma nova pasta chamada `aemformsbundles` na unidade C
* Abra um prompt de comando e navegue até `c:\aemformsbundles`
* Execute o seguinte comando no prompt de comando

```java
mvn -B org.apache.maven.plugins:maven-archetype-plugin:3.2.1:generate -D archetypeGroupId=com.adobe.aem -D archetypeArtifactId=aem-project-archetype -D archetypeVersion=36 -D appTitle="My Site" -D appId="mysite" -D groupId="com.mysite" -D aemVersion=6.5.13
```

Ao concluir com sucesso, você deve ver uma mensagem de sucesso de criação em sua janela de comando

## Criar projeto do eclipse a partir do seu projeto Maven

* Altere seu diretório de trabalho para `mysite`
* Executar `mvn eclipse:eclipse` na linha de comando. O comando lê o arquivo pom e cria projetos Eclipse com metadados corretos para que o Eclipse compreenda os tipos de projeto, os relacionamentos, o classpath, etc.

## Importar o projeto para o eclipse

Launch **Eclipse**

Ir para **Arquivo -> Importar** e selecione **Projetos Maven existentes** como mostrado aqui

![fonte de dados](assets/import-mvn-project.JPG)

Clique em Avançar

Selecione o c:\aemformsbundles\mysite by clicking the **Procurar** botão

![fonte de dados](assets/mysite-eclipse-project.png)

>[!NOTE]
>Você pode optar por importar os módulos apropriados, dependendo de suas necessidades. Selecione e importe somente o módulo principal, caso você só crie o código Java no seu projeto.

Clique em **Concluir** para iniciar o processo de importação

O projeto é importado para o Eclipse e você vê vários `mysite.xxxx` pastas

Expanda o `src/main/java` nos termos do `mysite.core` pasta. Esta é a pasta na qual você está gravando a maior parte do seu código.

![fonte de dados](assets/mysite-core-project.png)

## Incluir o SDK do cliente do AEMFD

Você precisa incluir o sdk do cliente AEMFD em seu projeto para aproveitar os vários serviços fornecidos com o AEM Forms. Consulte [SDK do cliente AEMFD](https://mvnrepository.com/artifact/com.adobe.aemfd/aemfd-client-sdk) para incluir o SDK do cliente apropriado em seu projeto Maven. É necessário incluir o SDK do cliente do FD do AEM na seção de dependências de `pom.xml` do projeto principal, como mostrado abaixo.

```xml
<dependency>
    <groupId>com.adobe.aemfd</groupId>
    <artifactId>aemfd-client-sdk</artifactId>
    <version>6.0.122</version>
</dependency>
```

Para criar seu projeto, siga as seguintes etapas:

* Abrir **janela da tela de comandos**
* Vá até `c:\aemformsbundles\mysite\core`
* Execute o comando `mvn clean install -PautoInstallBundle`
O comando acima cria e instala o pacote no servidor AEM em execução em `http://localhost:4502`. O pacote também está disponível no sistema de arquivos em
   `C:\AEMFormsBundles\mysite\core\target` e pode ser implantado usando [Console da Web Felix](http://localhost:4502/system/console/bundles)

## Próximas etapas

[Criar serviço OSGi](./create-osgi-service.md)

