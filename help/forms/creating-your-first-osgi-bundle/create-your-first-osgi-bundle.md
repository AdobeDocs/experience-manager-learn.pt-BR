---
title: Criação do primeiro pacote OSGi com o AEM Forms
description: Criar seu primeiro pacote OSGi usando Maven e Eclipse
version: Experience Manager 6.4, Experience Manager 6.5
feature: Adaptive Forms
topic: Development
role: Developer
level: Beginner
exl-id: 307cc3b2-87e5-4429-8f21-5266cf03b78f
last-substantial-update: 2021-04-23T00:00:00Z
duration: 145
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '665'
ht-degree: 0%

---

# Criar seu primeiro pacote OSGi

Um pacote OSGi é um arquivo Java™ que contém código Java, recursos e um manifesto que descreve o pacote e suas dependências. O pacote é a unidade de implantação de um aplicativo. Este artigo destina-se aos desenvolvedores que desejam criar um serviço OSGi ou um servlet usando o AEM Forms 6.4 ou 6.5. Para criar seu primeiro pacote OSGi, siga as seguintes etapas:


## Instalar JDK

Instale a versão suportada do JDK. Eu usei o JDK1.8. Verifique se você adicionou **JAVA_HOME** em suas variáveis de ambiente e se está apontando para a pasta raiz da sua instalação do JDK.
Adicionar o %JAVA_HOME%/bin ao caminho

![fonte-de-dados](assets/java-home.JPG)

>[!NOTE]
> Não use o JDK 15. Ele não é compatível com o AEM.

### Testar sua versão do JDK

Abra uma nova janela de prompt de comando e digite: `java -version`. Você deve recuperar a versão do JDK identificada pela variável `JAVA_HOME`

![fonte-de-dados](assets/java-version.JPG)

## Instalar o Maven

Maven é uma ferramenta de automação de build usada principalmente para projetos Java. Siga as etapas a seguir para instalar o Maven no sistema local.

* Crie uma pasta chamada `maven` na unidade C
* Baixe o [arquivo zip binário](https://maven.apache.org/download.cgi)
* Extrair o conteúdo do arquivo zip para `c:\maven`
* Crie uma variável de ambiente chamada `M2_HOME` com um valor de `C:\maven\apache-maven-3.6.0`. No meu caso, a versão **mvn** é a 3.6.0. No momento em que este artigo foi escrito, a versão mais recente do Maven era a 3.6.3
* Adicionar o `%M2_HOME%\bin` ao seu caminho
* Salve as alterações
* Abra um novo prompt de comando e digite `mvn -version`. Você deve ver a versão **mvn** listada como mostrado na captura de tela abaixo

![fonte-de-dados](assets/mvn-version.JPG)


## Instalar o Eclipse

Instale a última versão do [eclipse](https://www.eclipse.org/downloads/)

## Crie seu primeiro projeto

O arquétipo é um kit de ferramentas de modelos de projeto Maven. Um arquétipo é definido como um padrão ou modelo original a partir do qual todas as outras coisas do mesmo tipo são feitas. O nome se encaixa à medida que tentamos fornecer um sistema que fornece um meio consistente de gerar projetos Maven. O Arquétipo ajuda os autores a criar modelos de projeto Maven para usuários e fornece aos usuários os meios de gerar versões parametrizadas desses modelos de projeto.
Para criar seu primeiro projeto Maven, siga as seguintes etapas:

* Crie uma nova pasta chamada `aemformsbundles` na unidade C
* Abra um prompt de comando e navegue até `c:\aemformsbundles`
* Execute o seguinte comando no prompt de comando

```java
mvn -B org.apache.maven.plugins:maven-archetype-plugin:3.2.1:generate -D archetypeGroupId=com.adobe.aem -D archetypeArtifactId=aem-project-archetype -D archetypeVersion=36 -D appTitle="My Site" -D appId="mysite" -D groupId="com.mysite" -D aemVersion=6.5.13
```

Ao concluir com êxito, você deve ver uma mensagem de sucesso de criação na janela de comando

## Crie um projeto do eclipse a partir do seu projeto maven

* Alterar seu diretório de trabalho para `mysite`
* Executar `mvn eclipse:eclipse` a partir da linha de comando. O comando lê o arquivo pom e cria projetos Eclipse com metadados corretos para que o Eclipse entenda os tipos de projeto, relacionamentos, classpath etc.

## Importar o projeto para o eclipse

Iniciar **Eclipse**

Acesse **Arquivo -> Importar** e selecione **Projetos Maven existentes**, conforme mostrado aqui

![fonte-de-dados](assets/import-mvn-project.JPG)

Clique em Avançar

Selecione c:\aemformsbundles\mysite clicando no botão **Procurar**

![fonte-de-dados](assets/mysite-eclipse-project.png)

>[!NOTE]
>Você pode optar por importar os módulos apropriados de acordo com suas necessidades. Selecione e importe apenas o módulo principal, se você só vai criar código Java no seu projeto.

Clique em **Concluir** para iniciar o processo de importação

O projeto é importado para o Eclipse e você vê várias `mysite.xxxx` pastas

Expanda o `src/main/java` na pasta `mysite.core`. Essa é a pasta na qual você está escrevendo a maior parte do código.

![fonte-de-dados](assets/mysite-core-project.png)

## Incluir AEMFD Client SDK

É necessário incluir o sdk do cliente AEMFD no projeto para aproveitar os vários serviços fornecidos com o AEM Forms. Consulte [AEMFD Client SDK](https://mvnrepository.com/artifact/com.adobe.aemfd/aemfd-client-sdk) para incluir o SDK cliente apropriado em seu projeto Maven. Você precisa incluir o SDK do Cliente FD do AEM na seção de dependências do `pom.xml` do projeto principal, como mostrado abaixo.

```xml
<dependency>
    <groupId>com.adobe.aemfd</groupId>
    <artifactId>aemfd-client-sdk</artifactId>
    <version>6.0.122</version>
</dependency>
```

Para criar seu projeto, siga as seguintes etapas:

* Abrir **janela do prompt de comando**
* Navegue até `c:\aemformsbundles\mysite\core`
* Executar o comando `mvn clean install -PautoInstallBundle`
O comando acima compila e instala o pacote no servidor do AEM em execução em `http://localhost:4502`. O pacote também está disponível no sistema de arquivos em
  `C:\AEMFormsBundles\mysite\core\target` e pode ser implantado usando o [Felix web console](http://localhost:4502/system/console/bundles)

## Próximas etapas

[Criar serviço OSGi](./create-osgi-service.md)

