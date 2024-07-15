---
title: Configurar um ambiente de desenvolvimento do AEM local
description: Saiba como configurar um ambiente de desenvolvimento local para o Experience Manager. Familiarize-se com a instalação local, o Apache Maven, os ambientes de desenvolvimento integrados e a depuração e solução de problemas. Use o Eclipse IDE, o CRXDE-Lite, o Visual Studio Code e o IntelliJ.
version: 6.5
feature: Developer Tools
topic: Development
role: Developer
level: Beginner
exl-id: 58851624-71c9-4745-aaaf-305acf6ccb14
last-substantial-update: 2022-07-20T00:00:00Z
doc-type: Tutorial
thumbnail: aem-local-dev-env.jpg
duration: 4537
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '2413'
ht-degree: 0%

---

# Configurar um ambiente de desenvolvimento do AEM local

Guia para configurar um desenvolvimento local para o Adobe Experience Manager, AEM. Aborda tópicos importantes de instalação local, Apache Maven, ambientes de desenvolvimento integrados e depuração/solução de problemas. O desenvolvimento com **Eclipse IDE, CRXDE Lite, Visual Studio Code e IntelliJ** é discutido.

## Visão geral

Configurar um ambiente de desenvolvimento local é o primeiro passo ao desenvolver para Adobe Experience Manager ou AEM. Reserve tempo para configurar um ambiente de desenvolvimento de qualidade para aumentar sua produtividade e escrever um código melhor, mais rápido. Podemos dividir um ambiente de desenvolvimento local de AEM em quatro áreas:

* Instâncias locais do AEM
* Projeto [!DNL Apache Maven]
* IDE (Ambientes de desenvolvimento integrados)
* Resolução de problemas

## Instalar instâncias locais do AEM

Quando nos referimos a uma instância local do AEM, estamos falando de uma cópia do Adobe Experience Manager que está sendo executada na máquina pessoal de um desenvolvedor. ***Todos*** o desenvolvimento de AEM deve começar gravando e executando código em uma instância de AEM local.

Se você nunca usou o AEM, há dois modos de execução básicos que podem ser instalados: ***Autor*** e ***Publish***. O ***Author*** [runmode](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/configuring/configure-runmodes.html?lang=en) é o ambiente que os profissionais de marketing digital usam para criar e gerenciar conteúdo. Ao desenvolver o na maioria das vezes, você está implantando o código em uma instância de autor. Isso permite criar páginas e adicionar e configurar componentes. O AEM Sites é um CMS de criação WYSIWYG e, portanto, a maioria do CSS e do JavaScript pode ser testada em relação a uma instância de criação.

Também é um código de teste *crítico* em relação a uma instância ***Publish*** local. A instância do ***Publish*** é o ambiente AEM com o qual os visitantes do site interagem. Embora a instância ***Publish*** seja a mesma pilha de tecnologia que a instância ***Author***, há algumas distinções importantes entre configurações e permissões. O código deve ser testado em relação a uma instância ***Publish*** local antes de ser promovido para ambientes de nível superior.

### Etapas

1. Verifique se o Java™ está instalado.
   * Preferir o [Java™ JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=14) para AEM 6.5+
   * [Java™ JDK 8](https://www.oracle.com/java/technologies/downloads/) para versões AEM antes do AEM 6.5
1. Obtenha uma cópia do [AEM QuickStart Jar e um [!DNL license.properties]](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/deploy.html?lang=pt-BR).
1. Crie uma estrutura de pastas no seu computador da seguinte maneira:

```plain
~/aem-sdk
    /author
    /publish
```

1. Renomeie o JAR [!DNL QuickStart] como ***aem-author-p4502.jar*** e coloque-o abaixo do diretório `/author`. Adicione o arquivo ***[!DNL license.properties]*** abaixo do diretório `/author`.

1. Faça uma cópia do JAR [!DNL QuickStart], renomeie-o para ***aem-publish-p4503.jar*** e coloque-o sob o diretório `/publish`. Adicione uma cópia do arquivo ***[!DNL license.properties]*** abaixo do diretório `/publish`.

```plain
~/aem-sdk
    /author
        + aem-author-p4502.jar
        + license.properties
    /publish
        + aem-publish-p4503.jar
        + license.properties
```

1. Clique duas vezes no arquivo ***aem-author-p4502.jar*** para instalar a instância **Author**. Isso inicia a instância do autor, em execução na porta **4502** do computador local.

Clique duas vezes no arquivo ***aem-publish-p4503.jar*** para instalar a instância do **Publish**. Isso inicia a instância do Publish, em execução na porta **4503** do computador local.

>[!NOTE]
>
>Dependendo do hardware da sua máquina de desenvolvimento, pode ser difícil ter uma instância do **Author e do Publish** em execução ao mesmo tempo. Raramente é necessário executar ambos simultaneamente em uma configuração local.

### Usando a linha de comando

Uma alternativa para clicar duas vezes no arquivo JAR é iniciar o AEM a partir da linha de comando ou criar um script (`.bat` ou `.sh`), dependendo do tipo do seu sistema operacional local. Veja abaixo um exemplo do comando de amostra:

```shell
$ java -Xmx2048M -Xdebug -Xnoagent -Djava.compiler=NONE -Xrunjdwp:transport=dt_socket,server=y,suspend=n,address=30303 -jar aem-author-p4502.jar -gui -r"author,localdev"
```

Aqui, as `-X` são opções JVM e `-D` são propriedades de estrutura adicionais. Para obter mais informações, consulte [Implantando e Mantendo uma instância AEM](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/deploy.html?lang=pt-BR) e [Outras opções disponíveis no arquivo Quickstart](https://experienceleague.adobe.com/docs/experience-manager-65/deploying/deploying/custom-standalone-install.html#further-options-available-from-the-quickstart-file).

## Instalar o Apache Maven

***[!DNL Apache Maven]*** é uma ferramenta para gerenciar o procedimento de compilação e implantação de projetos baseados em Java. O AEM é uma plataforma baseada em Java e o [!DNL Maven] é a maneira padrão de gerenciar o código de um projeto AEM. Quando dizemos ***Projeto AEM Maven*** ou apenas seu ***Projeto AEM***, estamos nos referindo a um projeto Maven que inclui todo o código *personalizado* para seu site.

Todos os Projetos AEM devem ser compilados na versão mais recente do **[!DNL AEM Project Archetype]**: [https://github.com/adobe/aem-project-archetype](https://github.com/adobe/aem-project-archetype). O [!DNL AEM Project Archetype] fornece uma inicialização de um projeto AEM com código e conteúdo de amostra. O [!DNL AEM Project Archetype] também inclui **[!DNL AEM WCM Core Components]** configurado para ser usado no seu projeto.

>[!CAUTION]
>
>Ao iniciar um novo projeto, é uma prática recomendada usar a versão mais recente do arquétipo. Lembre-se de que há várias versões do arquétipo e nem todas as versões são compatíveis com versões anteriores do AEM.

### Etapas

1. Baixar o [Apache Maven](https://maven.apache.org/download.cgi)
2. Instale o [Apache Maven](https://maven.apache.org/install.html) e verifique se a instalação foi adicionada à linha de comando `PATH`.
   * [!DNL macOS] usuários podem instalar o Maven usando o [Homebrew](https://brew.sh/)
3. Verifique se **[!DNL Maven]** está instalado abrindo um novo terminal de linha de comando e executando o seguinte:

```shell
$ mvn --version
Apache Maven 3.3.9
Maven home: /Library/apache-maven-3.3.9
Java version: 1.8.0_111, vendor: Oracle Corporation
Java home: /Library/Java/JavaVirtualMachines/jdk1.8.0_111.jdk/Contents/Home/jre
Default locale: en_US, platform encoding: UTF-8
```

>[!NOTE]
>
> Em, a adição anterior do perfil Maven `adobe-public` era necessária para apontar para `nexus.adobe.com` para baixar artefatos AEM. Todos os artefatos de AEM agora estão disponíveis via Maven Central e o perfil `adobe-public` não é necessário.

## Configurar um ambiente de desenvolvimento integrado

Um ambiente de desenvolvimento integrado ou IDE é um aplicativo que combina um editor de texto, suporte a sintaxe e ferramentas de construção. Dependendo do tipo de desenvolvimento que você está fazendo, um IDE pode ser preferível sobre outro. Independentemente do IDE, é importante poder ***enviar*** código periodicamente para uma instância de AEM local para testá-lo. É importante ***extrair*** configurações de uma instância de AEM local para o projeto AEM ocasionalmente para manter um sistema de gerenciamento de controle do código-fonte como o Git.

Abaixo estão algumas das IDEs mais populares usadas com o desenvolvimento de AEM, com vídeos correspondentes que mostram a integração com uma instância de AEM local.

>[!NOTE]
>
> O projeto WKND foi atualizado para padrão para funcionar no AEM as a Cloud Service. Foi atualizado para [compatível com versões anteriores com 6.5/6.4](https://github.com/adobe/aem-guides-wknd#building-for-aem-6xx). Se estiver usando AEM 6.5 ou 6.4, anexe o perfil `classic` a qualquer comando Maven.

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

Quando, usando um IDE, verifique `classic` na guia Perfil Maven.

![Guia Perfil Maven](assets/set-up-a-local-aem-development-environment/intelliJMavenProfiles.png)

*Perfil Maven IntelliJ*

### IDE [!DNL Eclipse]

O **[[!DNL Eclipse] IDE](https://www.eclipse.org/ide/)** é um dos IDEs mais populares para desenvolvimento em Java™, em grande parte porque é de código aberto e ***gratuito***! O Adobe fornece um plug-in, **[[!DNL AEM Developer Tools]](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html)**, para [!DNL Eclipse] para facilitar o desenvolvimento com uma interface gráfica agradável para sincronizar o código com uma instância de AEM local. O IDE [!DNL Eclipse] é recomendado para desenvolvedores novos no AEM em grande parte devido ao suporte de GUI do [!DNL AEM Developer Tools].

#### Instalação e configuração

1. Baixe e instale o IDE [!DNL Eclipse] para [!DNL Java™ EE Developers]: [https://www.eclipse.org](https://www.eclipse.org/)
1. Siga as instruções para instalar o plug-in [!DNL AEM Developer Tools]: [https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html)

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

* 00:30 - Importar projeto Maven
* 01:24 - Criar e implantar código-fonte com Maven
* 04:33 - Alterações no código de push com a Ferramenta de desenvolvedor de AEM
* 10:55 - Extrair alterações de código com a Ferramenta de desenvolvedor de AEM
* 13:12 - Uso das ferramentas de depuração integradas do Eclipse

### IntelliJ IDEA

A **[IntelliJ IDEA](https://www.jetbrains.com/idea/)** é um IDE poderoso para desenvolvimento profissional em Java™. O [!DNL IntelliJ IDEA] tem duas opções: uma edição ***gratuita*** [!DNL Community] e uma versão comercial (paga) [!DNL Ultimate]. A versão [!DNL Community] gratuita de [!DNL IntellIJ IDEA] é suficiente para mais desenvolvimento de AEM. No entanto, o [!DNL Ultimate] [expande seu conjunto de recursos](https://www.jetbrains.com/idea/download).

#### [!DNL Installation and Setup]

1. Baixe e instale o [!DNL IntelliJ IDEA]: [https://www.jetbrains.com/idea/download](https://www.jetbrains.com/idea/download)
1. Instalar [!DNL Repo] (ferramenta de linha de comando): [https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

>[!VIDEO](https://video.tv.adobe.com/v/26089?quality=12&learn=on)

* 00:00 - Importar projeto Maven
* 05:47 - Criar e implantar código-fonte com Maven
* 08:17 - Enviar alterações com o repositório
* 14:39 - Extrair alterações com o Repositório
* 17:25 - Uso das ferramentas de depuração integradas do IntelliJ IDEA

### [!DNL Visual Studio Code]

O **[Visual Studio Code](https://code.visualstudio.com/)** tornou-se rapidamente uma ferramenta favorita para ***desenvolvedores front-end*** com suporte aprimorado para JavaScript, [!DNL Intellisense] e depuração de navegador. **[!DNL Visual Studio Code]** é open source, gratuito, com muitas extensões poderosas. O [!DNL Visual Studio Code] pode ser configurado para se integrar ao AEM com a ajuda de uma ferramenta Adobe, o **[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code).** Também há várias extensões com suporte da comunidade que podem ser instaladas para integração com o AEM.

O [!DNL Visual Studio Code] é uma ótima opção para desenvolvedores de front-end que escrevem principalmente código CSS/LESS e JavaScript para criar bibliotecas de clientes AEM. Essa ferramenta pode não ser a melhor opção para novos desenvolvedores de AEM, pois as definições de nó (caixas de diálogo, componentes) precisam ser editadas em XML bruto. Há várias extensões Java™ disponíveis para [!DNL Visual Studio Code], no entanto, se você preferir fazer principalmente o desenvolvimento Java™ [!DNL Eclipse IDE] ou [!DNL IntelliJ].

#### Links importantes

* [**Baixar**](https://code.visualstudio.com/Download) **Código Visual Studio**
* **[repositório](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)** - Ferramenta semelhante a FTP para conteúdo JCR
* **[aemfed](https://aemfed.io/)** - Acelere seu fluxo de trabalho de front-end no AEM
* **[Sincronização com AEM](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)** - Extensão com suporte da comunidade&#42; para Visual Studio Code

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

* 00:30 - Importar projeto Maven
* 00:53 - Criar e implantar código-fonte com Maven
* 04:03 - Enviar alterações de código com a ferramenta de linha de comando do Repo
* 08:29 - Extrair alterações de código com a ferramenta de linha de comando do Repo
* 10:40 - Alterações no código de push com a ferramenta aemfed
* 14:24 - Solução de problemas, Reconstrução de bibliotecas de clientes

### [!DNL CRXDE Lite]

[CRXDE Lite](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/developing-with-crxde-lite.html) é uma exibição baseada em navegador do repositório AEM. O [!DNL CRXDE Lite] está inserido no AEM e permite que um desenvolvedor execute tarefas de desenvolvimento padrão, como editar arquivos, definir componentes, caixas de diálogo e modelos. [!DNL CRXDE Lite] ***não*** pretende ser um ambiente de desenvolvimento completo, mas é efetivo como uma ferramenta de depuração. O [!DNL CRXDE Lite] é útil para estender ou simplesmente entender o código do produto fora da sua base de código. O [!DNL CRXDE Lite] fornece uma exibição poderosa do repositório e uma maneira eficaz de testar e gerenciar permissões.

[!DNL CRXDE Lite] deve ser usado com outros IDEs para testar e depurar o código, mas nunca como a ferramenta de desenvolvimento principal. Ele tem suporte limitado à sintaxe, nenhum recurso de preenchimento automático e integração limitada com sistemas de gerenciamento de controle de origem.

>[!VIDEO](https://video.tv.adobe.com/v/25917?quality=12&learn=on)

## Resolução de problemas

***Ajuda!*** Meu código não está funcionando! Como em todo o desenvolvimento, há momentos (provavelmente muitos) em que seu código não está funcionando como esperado. AEM é uma plataforma poderosa, mas com grande poder... vem grande complexidade. Abaixo estão alguns pontos de partida de alto nível ao solucionar problemas e rastreá-los (mas longe de uma lista exaustiva de itens que podem dar errado):

### Verificar implantação do código

Um bom primeiro passo ao encontrar um problema é verificar se o código foi implantado e instalado com êxito no AEM.

1. **Verifique o [!UICONTROL Gerenciador de Pacotes]** para garantir que o pacote de códigos foi carregado e instalado: [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). Verifique o carimbo de data e hora para verificar se o pacote foi instalado recentemente.
1. Se estiver fazendo atualizações de arquivo incrementais usando uma ferramenta como o [!DNL Repo] ou o [!DNL AEM Developer Tools], **verifique[!DNL CRXDE Lite]** se o arquivo foi enviado por push para a instância AEM local e se o conteúdo do arquivo foi atualizado: [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)
1. **Verifique se o pacote foi carregado** se estiver vendo problemas relacionados ao código Java™ em um pacote OSGi. Abra o [!UICONTROL Console da Web do Adobe Experience Manager]: [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles) e procure seu pacote. Verifique se o lote tem o status **[!UICONTROL Ativo]**. Veja abaixo mais informações relacionadas à solução de problemas de um pacote no estado **[!UICONTROL Instalado]**.

#### Verifique os logs

O AEM é uma plataforma de conversa e registra informações úteis no **error.log**. O **error.log** pode ser encontrado onde o AEM foi instalado: &lt; `aem-installation-folder>/crx-quickstart/logs/error.log`.

Uma técnica útil para rastrear problemas é adicionar instruções de log no código Java™:

```java
import org.slf4j.Logger;
import org.slf4j.LoggerFactory;
...

public class MyClass {
    private final Logger log = LoggerFactory.getLogger(getClass());

    ...

    String myVariable = "My Variable";

    log.debug("Debug statement of myVariable {}", myVariable);

    log.info("Info statement of myVariable {}", myVariable);
}
```

Por padrão, o **error.log** está configurado para registrar *[!DNL INFO]* instruções. Se você quiser alterar o nível de log, acesse [!UICONTROL Suporte de Log]: [http://localhost:4502/system/console/slinglog](http://localhost:4502/system/console/slinglog). Você também pode achar que o **error.log** é muito tagarela. Você pode usar o [!UICONTROL Suporte de log] para configurar instruções de log apenas para um pacote Java™ especificado. Essa é uma prática recomendada para projetos do, a fim de separar facilmente problemas de código personalizado de problemas da plataforma OOTB AEM.

![Configuração de log no AEM](./assets/set-up-a-local-aem-development-environment/logging.png)

#### O pacote está em um estado Instalado {#bundle-active}

Todos os conjuntos (exceto Fragmentos) devem estar em um estado **[!UICONTROL Ativo]**. Se você vir o conjunto de códigos em um estado [!UICONTROL Instalado], há um problema que precisa ser resolvido. Na maioria das vezes, esse é um problema de dependência:

![Erro de pacote no AEM](assets/set-up-a-local-aem-development-environment/bundle-error.png)

Na captura de tela acima, o [!DNL WKND Core bundle] é um estado [!UICONTROL Instalado]. Isso ocorre porque o pacote está esperando uma versão diferente de `com.adobe.cq.wcm.core.components.models` da que está disponível na instância AEM.

Uma ferramenta útil que pode ser usada é o [!UICONTROL Localizador de Dependências]: [http://localhost:4502/system/console/depfinder](http://localhost:4502/system/console/depfinder). Adicione o nome do pacote Java™ para inspecionar qual versão está disponível na instância AEM:

![Componentes principais](assets/set-up-a-local-aem-development-environment/core-components.png)

Continuando com o exemplo acima, podemos ver que a versão instalada na instância do AEM é **12.2** vs **12.6** que o pacote estava esperando. A partir daí, você pode retroceder e ver se as dependências de [!DNL Maven] no AEM correspondem às dependências de [!DNL Maven] no projeto AEM. No, o exemplo acima [!DNL Core Components] **v2.2.0** está instalado na instância AEM, mas o pacote de códigos foi criado com uma dependência em **v2.2.2**, portanto, o motivo para o problema de dependência.

#### Verificar registro de modelos do Sling {#osgi-component-sling-models}

Os componentes do AEM devem ter o suporte de um [!DNL Sling Model] para encapsular qualquer lógica de negócios e garantir que o script de renderização do HTL permaneça limpo. Se estiver tendo problemas em que o Modelo do Sling não pode ser encontrado, pode ser útil verificar [!DNL Sling Models] no console: [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels). Isso informa se seu Modelo do Sling foi registrado e a qual tipo de recurso (o caminho do componente) ele está vinculado.

![Status do Modelo do Sling](assets/set-up-a-local-aem-development-environment/sling-model-status.png)

Mostra o registro de um [!DNL Sling Model], `BylineImpl` que está vinculado a um tipo de recurso de componente de `wknd/components/content/byline`.

#### Problemas de CSS ou JavaScript

Para a maioria dos problemas de CSS e JavaScript, o uso das ferramentas de desenvolvimento do navegador é a maneira mais eficaz de solucionar problemas. Para restringir o problema ao desenvolver em relação a uma instância de autor do AEM, é útil visualizar a página &quot;como publicada&quot;.

![Problemas de CSS ou JS](assets/set-up-a-local-aem-development-environment/css-and-js-issues.png)

Abra o menu [!UICONTROL Propriedades da Página] e clique em [!UICONTROL Exibir como Publicado]. A página será aberta sem o Editor AEM e com um parâmetro de consulta definido como **wcmmode=disabled**. Isso desativa com eficiência a interface de criação do AEM e facilita muito a solução de problemas/depuração do front-end.

Outro problema encontrado com frequência ao desenvolver o código front-end é o CSS/JS antigo ou desatualizado que está sendo carregado. Como primeira etapa, verifique se o histórico do navegador foi limpo e, se necessário, inicie navegadores incógnitos ou uma nova sessão.

#### Depuração de bibliotecas de clientes

Com os diferentes métodos de categorias e incorporações para incluir várias bibliotecas de clientes, pode ser complicado solucionar problemas. O AEM expõe várias ferramentas para ajudar nisso. Uma das ferramentas mais importantes é [!UICONTROL Recompilar Bibliotecas de Clientes], que força o AEM a recompilar qualquer arquivo MENOS e gerar o CSS.

* [Bibliotecas de Despejo](http://localhost:4502/libs/granite/ui/content/dumplibs.html) - Lista todas as bibliotecas de clientes registradas na instância do AEM. &lt;host>/libs/granite/ui/content/dumplibs.html
* [Saída de teste](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) - permite que um usuário veja a saída de HTML esperada de clientlib includes com base na categoria. &lt;host>/libs/granite/ui/content/dumplibs.test.html
* [Validação de dependências de bibliotecas](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) - destaca todas as dependências ou categorias inseridas que não podem ser encontradas. &lt;host>/libs/granite/ui/content/dumplibs.validate.html
* [Recompilar Bibliotecas de Clientes](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) - permite que um usuário force o AEM a recompilar todas as bibliotecas de clientes ou invalidar o cache das bibliotecas de clientes. Essa ferramenta é eficaz ao desenvolver com MENOS, pois isso pode forçar o AEM a recompilar o CSS gerado. Em geral, é mais eficaz Invalidar caches e executar uma atualização de página do que reconstruir todas as bibliotecas. &lt;host>/libs/granite/ui/content/dumplibs.rebuild.html

![Depurando Clientlibs](assets/set-up-a-local-aem-development-environment/debugging-clientlibs.png)

>[!NOTE]
>
>Se você precisar invalidar constantemente o cache usando a ferramenta [!UICONTROL Recompilar bibliotecas de clientes], talvez valha a pena realizar uma única recriação de todas as bibliotecas de clientes. Isso pode levar cerca de 15 minutos, mas normalmente elimina quaisquer problemas de cache no futuro.
