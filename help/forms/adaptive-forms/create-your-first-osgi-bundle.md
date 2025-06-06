---
title: Criação do primeiro pacote OSGi com formulários do AEM
description: Crie seu primeiro pacote OSGi usando o maven e o eclipse
feature: Adaptive Forms
doc-type: article
version: Experience Manager 6.4, Experience Manager 6.5
topic: Development
role: Developer
level: Beginner
last-substantial-update: 2021-06-09T00:00:00Z
duration: 177
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '816'
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
* Baixe o [arquivo zip binário](http://maven.apache.org/download.cgi)
* Extrair o conteúdo do arquivo zip para `c:\maven`
* Crie uma variável de ambiente chamada `M2_HOME` com um valor de `C:\maven\apache-maven-3.6.0`. No meu caso, a versão **mvn** é a 3.6.0. No momento em que este artigo foi escrito, a versão mais recente do Maven era a 3.6.3
* Adicionar o `%M2_HOME%\bin` ao seu caminho
* Salve as alterações
* Abra um novo prompt de comando e digite `mvn -version`. Você deve ver a versão **mvn** listada como mostrado na captura de tela abaixo

![fonte-de-dados](assets/mvn-version.JPG)

## Settings.xml

Um arquivo Maven `settings.xml` define valores que configuram a execução do Maven de várias maneiras. Normalmente, é usado para definir um local de repositório local, servidores de repositório remotos alternativos e informações de autenticação para repositórios privados.

Navegue até `C:\Users\<username>\.m2 folder`
Extraia o conteúdo do arquivo [settings.zip](assets/settings.zip) e coloque-o na pasta `.m2`.

## Instalar o Eclipse

Instale a última versão do [eclipse](https://www.eclipse.org/downloads/)

## Crie seu primeiro projeto

O arquétipo é um kit de ferramentas de modelos de projeto Maven. Um arquétipo é definido como um padrão ou modelo original a partir do qual todas as outras coisas do mesmo tipo são feitas. O nome se encaixa à medida que tentamos fornecer um sistema que fornece um meio consistente de gerar projetos Maven. O Arquétipo ajuda os autores a criar modelos de projeto Maven para usuários e fornece aos usuários os meios de gerar versões parametrizadas desses modelos de projeto.
Para criar seu primeiro projeto Maven, siga as seguintes etapas:

* Crie uma nova pasta chamada `aemformsbundles` na unidade C
* Abra um prompt de comando e navegue até `c:\aemformsbundles`
* Execute o seguinte comando no prompt de comando
* `mvn archetype:generate  -DarchetypeGroupId=com.adobe.granite.archetypes  -DarchetypeArtifactId=aem-project-archetype -DarchetypeVersion=19`

O projeto Maven é gerado interativamente e você é solicitado a fornecer valores para várias propriedades, como

| Nome de propriedade | Significância | Valor |
|------------------------|---------------------------------------|---------------------|
| groupId | O groupId identifica exclusivamente seu projeto em todos os projetos | com.learningaemforms.adobe |
| appsFolderName | Nome da pasta que contém a estrutura do projeto | learningaemforms |
| artifactId | artifactId é o nome do jar sem versão. Se você o criou, poderá escolher qualquer nome que desejar com letras minúsculas e sem símbolos estranhos. | learningaemforms |
| version | Se você distribuí-lo, poderá escolher qualquer versão típica com números e pontos (1.0, 1.1, 1.0.1, ...). | 1.0 |

Aceite os valores padrão para as outras propriedades pressionando a tecla Enter.
Se tudo correr bem, você verá uma mensagem de sucesso da build na janela de comando

## Crie um projeto do eclipse a partir do seu projeto maven

Altere seu diretório de trabalho para `learningaemforms`.
Executando `mvn eclipse:eclipse` a partir da linha de comando
O comando acima lê o arquivo pom e cria projetos Eclipse com metadados corretos para que o Eclipse entenda os tipos de projeto, relacionamentos, classpath etc.

## Importar o projeto para o eclipse

Iniciar **Eclipse**

Acesse **Arquivo -> Importar** e selecione **Projetos Maven existentes**, conforme mostrado aqui

![fonte-de-dados](assets/import-mvn-project.JPG)

Clique em Avançar

Selecione o `c:\aemformsbundles\learningaemform`s clicando no botão **Procurar**

![fonte-de-dados](assets/select-mvn-project.JPG)

>[!NOTE]
>Você pode optar por importar os módulos apropriados de acordo com suas necessidades. Selecione e importe apenas o módulo principal, se você só vai criar código Java no seu projeto.

Clique em **Concluir** para iniciar o processo de importação

O projeto é importado para o Eclipse e você vê várias `learningaemforms.xxxx` pastas

Expanda o `src/main/java` na pasta `learningaemforms.core`. Essa é a pasta na qual você está escrevendo a maior parte do código.

![fonte-de-dados](assets/learning-core.JPG)

## Crie seu projeto

Depois de escrever o serviço OSGi ou servlet, é necessário criar o projeto para gerar o pacote OSGi que pode ser implantado usando o console da Web Felix. Consulte [AEMFD Client SDK](https://repo.adobe.com/nexus/content/repositories/public/com/adobe/aemfd/aemfd-client-sdk/) para incluir o SDK cliente apropriado em seu projeto Maven. Você deve incluir o AEM FD Client SDK na seção de dependências de `pom.xml` do projeto principal, como mostrado abaixo.

```xml
<dependency>
    <groupId>com.adobe.aemfd</groupId>
    <artifactId>aemfd-client-sdk</artifactId>
    <version>6.0.122</version>
</dependency>
```

Para criar seu projeto, siga as seguintes etapas:

* Abrir **janela do prompt de comando**
* Navegue até `c:\aemformsbundles\learningaemforms\core`
* Executar o comando `mvn clean install`
Se tudo correr bem, você deverá ver o pacote no seguinte local `C:\AEMFormsBundles\learningaemforms\core\target`. Esse pacote agora está pronto para ser implantado no AEM usando o Felix web console.
