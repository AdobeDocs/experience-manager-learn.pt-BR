---
title: Criar seu primeiro pacote OSGi com formulários AEM
description: Crie seu primeiro pacote OSGi usando o maven e o eclipse
feature: administration
topics: development
audience: developer
doc-type: article
activity: setup
version: 6.4,6.5
translation-type: tm+mt
source-git-commit: 48060b4d8c4b502e0c099ae8081695f97b423037
workflow-type: tm+mt
source-wordcount: '830'
ht-degree: 1%

---


# Criar seu primeiro pacote OSGi

Um pacote OSGi é um arquivo de arquivamento Java™ que contém código Java, recursos e um manifesto que descreve o pacote e suas dependências. O pacote é a unidade de implantação de um aplicativo. Este artigo destina-se a desenvolvedores que desejam criar um serviço OSGi ou um servlet usando o AEM Forms 6.4 ou 6.5. Para criar seu primeiro pacote OSGi, siga as seguintes etapas:


## Instalar JDK

Instale a versão compatível do JDK. Eu usei o JDK1.8. Verifique se você adicionou **JAVA_HOME** às variáveis do ambiente e está apontando para a pasta raiz da instalação do JDK.
Adicionar o %JAVA_HOME%/bin ao caminho

![fonte de dados](assets/java-home.JPG)

>[!NOTE]
> Não use o JDK 15. Não é suportado pela AEM.

### Testar sua versão do JDK

Abra uma nova janela de prompt de comando e digite: `java -version`. Você deve recuperar a versão do JDK identificada pela variável `JAVA_HOME`

![fonte de dados](assets/java-version.JPG)

## Instalar o Maven

O Maven é uma ferramenta de automação de compilação usada principalmente para projetos Java. Siga as etapas a seguir para instalar o maven em seu sistema local.

* Crie uma pasta chamada `maven` na unidade C
* Baixe o [arquivo zip binário](http://maven.apache.org/download.cgi)
* Extraia o conteúdo do arquivo zip em `c:\maven`
* Crie uma variável de ambiente chamada `M2_HOME` com um valor de `C:\maven\apache-maven-3.6.0`. No meu caso, a versão **mvn** é 3.6.0. No momento em que este artigo é escrito, a versão mais recente do maven é 3.6.3
* Adicione `%M2_HOME%\bin` ao seu caminho
* Salvar suas alterações
* Abra um novo prompt de comando e digite `mvn -version`. Você deve ver a versão **mvn** listada conforme mostrado na captura de tela abaixo

![fonte de dados](assets/mvn-version.JPG)

## Settings.xml

Um arquivo Maven `settings.xml` define valores que configuram a execução Maven de várias maneiras. Normalmente, é usado para definir um local de repositório local, servidores de repositório remoto alternativos e informações de autenticação para repositórios privados.

Navegue até `C:\Users\<username>\.m2 folder`
Extraia o conteúdo do arquivo [settings.zip](assets/settings.zip) e coloque-o na pasta `.m2`.

## Instalar o Eclipse

Instale a versão mais recente de [eclipse](https://www.eclipse.org/downloads/)

## Criar seu primeiro projeto

Archetype é um kit de ferramentas de modelo de projeto Maven. Um arquétipo é definido como um padrão ou modelo original a partir do qual são feitas todas as outras coisas do mesmo tipo. O nome se encaixa como estamos tentando fornecer um sistema que fornece um meio consistente de gerar projetos Maven. O Archetype ajudará os autores a criar modelos de projeto Maven para usuários e fornece aos usuários os meios de gerar versões parametrizadas desses modelos de projeto.
Para criar seu primeiro projeto maven, siga as seguintes etapas:

* Crie uma nova pasta chamada `aemformsbundles` na unidade C
* Abra um prompt de comando e navegue até `c:\aemformsbundles`
* Execute o seguinte comando no prompt de comando
* `mvn archetype:generate  -DarchetypeGroupId=com.adobe.granite.archetypes  -DarchetypeArtifactId=aem-project-archetype -DarchetypeVersion=19`

O projeto maven será gerado interativamente e você será solicitado a fornecer valores para várias propriedades, como

| Nome da Propriedade | Significância | Valor |
------------------------|---------------------------------------|---------------------
| groupId | groupId identifica exclusivamente seu projeto em todos os projetos | com.learningaemforms.adobe |
| appsFolderName | Nome da pasta que manterá sua estrutura de projeto | learningaemforms |
| artifatoId | artifatoId é o nome do jar sem versão. Se você o criou, então você pode escolher qualquer nome que quiser com letras minúsculas e sem símbolos estranhos. | learningaemforms |
| version | Se distribuí-lo, você pode escolher qualquer versão típica com números e pontos (1.0, 1.1, 1.0.1, ...). | 1.0 |

Aceite os valores padrão para as outras propriedades pressionando a tecla enter.
Se tudo correr bem, você deverá ver uma mensagem de sucesso de compilação na sua janela de comando

## Criar projeto eclipse a partir do seu projeto maven

Altere o diretório de trabalho para `learningaemforms`.
Executando `mvn eclipse:eclipse` da linha de comando
O comando acima lê seu arquivo pom e cria projetos Eclipse com metadados corretos para que o Eclipse compreenda tipos de projetos, relacionamentos, classpath etc.

## Importar o projeto para o eclipse

Iniciar **Eclipse**

Vá para **Arquivo -> Importar** e selecione **Projetos Maven Existentes** conforme mostrado aqui

![fonte de dados](assets/import-mvn-project.JPG)

Clique em Avançar

Selecione `c:\aemformsbundles\learningaemform`s clicando no botão **Procurar**

![fonte de dados](assets/select-mvn-project.JPG)

>[!NOTE]
>Você pode optar por importar os módulos apropriados, dependendo de suas necessidades. Selecione e importe somente o módulo principal, se você só vai criar o código Java no seu projeto.

Clique em **Concluir** para start do processo de importação

O projeto é importado para o Eclipse e você verá várias pastas `learningaemforms.xxxx`

Expanda `src/main/java` na pasta `learningaemforms.core`. Esta é a pasta na qual você gravará a maior parte do seu código.

![fonte de dados](assets/learning-core.JPG)

## Crie seu projeto

Depois que você tiver escrito seu serviço OSGi, ou servlet, precisará criar seu projeto para gerar o pacote OSGi que pode ser implantado usando o console da Web Felix. Consulte [SDK do cliente AEMFD](https://repo.adobe.com/nexus/content/repositories/public/com/adobe/aemfd/aemfd-client-sdk/) para incluir o SDK do cliente apropriado no seu projeto Maven. Você terá que incluir o SDK do cliente AEM FD na seção dependências de `pom.xml` do projeto principal, conforme mostrado abaixo.

```xml
<dependency>
    <groupId>com.adobe.aemfd</groupId>
    <artifactId>aemfd-client-sdk</artifactId>
    <version>6.0.122</version>
</dependency>
```

Para criar seu projeto, siga as seguintes etapas:

* Abrir **janela de prompt de comando**
* Vá até `c:\aemformsbundles\learningaemforms\core`
* Execute o comando `mvn clean install`
Se tudo correr bem, você deverá ver o pacote no seguinte local `C:\AEMFormsBundles\learningaemforms\core\target`. Esse pacote está pronto para ser implantado no AEM usando o console da Web do Felix.
