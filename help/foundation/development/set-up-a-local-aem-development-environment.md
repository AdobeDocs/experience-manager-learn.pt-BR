---
title: Configurar um Ambiente de desenvolvimento do AEM local
description: Guia para configurar um desenvolvimento local para Adobe Experience Manager, AEM. Abrange tópicos importantes de instalação local, Apache Maven, ambientes de desenvolvimento integrados e depuração/solução de problemas. O desenvolvimento com Eclipse IDE, CRXDE-Lite, Código do Visual Studio e IntelliJ é discutido.
version: 6.4, 6.5
feature: maven-archetype
topics: development
activity: develop
audience: developer
translation-type: tm+mt
source-git-commit: c85a59a8bd180d5affe2a5bf5939dabfb2776d73
workflow-type: tm+mt
source-wordcount: '2594'
ht-degree: 1%

---


# Configurar um Ambiente de desenvolvimento do AEM local

Guia para configurar um desenvolvimento local para Adobe Experience Manager, AEM. Abrange tópicos importantes de instalação local, Apache Maven, ambientes de desenvolvimento integrados e depuração/solução de problemas. O desenvolvimento com **[!DNL Eclipse IDE],[!DNL CRXDE Lite],[!DNL Visual Studio Code]e[!DNL IntelliJ]** é discutido.

## Visão geral

A configuração de um ambiente de desenvolvimento local é a primeira etapa do desenvolvimento para Adobe Experience Manager ou AEM. Aproveite o tempo para configurar um ambiente de desenvolvimento de qualidade para aumentar a sua produtividade e escrever melhor código, mais rapidamente. Podemos dividir um ambiente de desenvolvimento local AEM em quatro áreas:

* Instâncias de AEM locais
* [!DNL Apache Maven] projeto
* Ambientes de desenvolvimento integrado (IDE)
* Resolução de problemas

## Instalar instâncias de AEM locais

Quando nos referimos a uma instância AEM local, estamos falando de uma cópia do Adobe Experience Manager que está sendo executada em uma máquina pessoal do desenvolvedor. ***Todo*** o desenvolvimento AEM deve ser start escrevendo e executando código contra uma instância AEM local.

Se você for novo em AEM, dois modos básicos de execução podem ser instalados: ***Autor*** e ***publicação***. O ***Author*** [runmode](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/configure-runmodes.html) é o ambiente que os profissionais de marketing digital usarão para criar e gerenciar conteúdo. Ao desenvolver a **maioria** das vezes, você implantará o código em uma instância do autor. Isso permite criar novas páginas, além de adicionar e configurar componentes. A AEM Sites é um CMS de criação WYSIWYG e, portanto, a maioria do CSS e do JavaScript pode ser testada em relação a uma instância de criação.

Também é um código de teste *crítico* em relação a uma instância local ***Publicar*** . A instância ***Publicar*** é o ambiente AEM com o qual os visitantes do site interagirão. Embora a instância ***Publicar*** seja a mesma pilha de tecnologia que a instância ***Autor*** , há algumas distinções importantes com configurações e permissões. O código deve *sempre* ser testado em relação a uma instância local de ***publicação*** antes de ser promovido a ambientes de nível superior.

### Etapas

1. Verifique se o [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html) está instalado.
   * Preferir [Java JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=lista&amp;p.offset=0&amp;p.limit=14) para AEM 6.5+
   * [Java JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html#JDK8) para versões AEM anteriores ao AEM 6.5
2. Obtenha uma cópia do [AEM QuickStart Jar e um [!DNL license.properties]](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware).
3. Crie uma estrutura de pastas em seu computador como a seguinte:

   ```plain
   ~/aem-sdk
       /author
       /publish
   ```

4. Renomeie o [!DNL QuickStart] JAR para ***aem-author-p4502.jar*** e coloque-o abaixo do `/author` diretório. Adicione o ***[!DNL license.properties]*** arquivo abaixo do `/author` diretório.
5. Faça uma cópia do [!DNL QuickStart] JAR, renomeie-o para ***aem-publish-p4503.jar*** e coloque-o abaixo do `/publish` diretório. Adicione uma cópia do ***[!DNL license.properties]*** arquivo abaixo do `/publish` diretório.

   ```plain
   ~/aem-sdk
       /author
           + aem-author-p4502.jar
           + license.properties
       /publish
           + aem-publish-p4503.jar
           + license.properties
   ```

6. Clique com o duplo do mouse no arquivo ***aem-author-p4502.jar*** para instalar a instância **Author** . Isso start a instância do autor, que é executada na porta **4502** no computador local.

   Clique com o duplo do mouse no arquivo ***aem-publish-p4503.jar*** para instalar a instância de **publicação** . Isso start a instância de publicação, que é executada na porta **4503** no computador local.

   >[!NOTE]
   >
   >Dependendo do hardware do seu computador de desenvolvimento, pode ser difícil ter uma instância **Autor e Publicar** em execução ao mesmo tempo. Raramente é necessário executar ambos simultaneamente em uma configuração local.

   Para obter mais informações, consulte [Implantação e manutenção de uma instância](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html)AEM.

## Instalar o Apache Maven

***[!DNL Apache Maven]*** é uma ferramenta para gerenciar o procedimento de criação e implantação de projetos baseados em Java. AEM é uma plataforma baseada em Java e [!DNL Maven] é a maneira padrão de gerenciar o código de um projeto AEM. Quando dizemos ***AEM Projeto*** Maven ou apenas seu Projeto ****** AEM, estamos nos referindo a um projeto Maven que inclui todo o código *personalizado* para seu site.

Todos os projetos AEM devem ser construídos a partir da versão mais recente do **[!DNL AEM Project Archetype]**: [https://github.com/Adobe-Marketing-Cloud/aem-project-archetype](https://github.com/Adobe-Marketing-Cloud/aem-project-archetype). O [!DNL AEM Project Archetype] criará um bootstrap de um projeto AEM com algum código de amostra e conteúdo. O [!DNL AEM Project Archetype] também inclui **[!DNL AEM WCM Core Components]** configurado para ser usado em seu projeto.

>[!CAUTION]
>
>Ao iniciar um novo projeto, é uma prática recomendada usar a versão mais recente do arquétipo. Lembre-se de que há várias versões do arquétipo e nem todas são compatíveis com versões anteriores do AEM.

### Etapas

1. Baixar o [Apache Maven](https://maven.apache.org/download.cgi)
2. Instale o [Apache Maven](https://maven.apache.org/install.html) e verifique se a instalação foi adicionada à linha de comando `PATH`.
   * [!DNL macOS] os usuários podem instalar o Maven usando o [Homebrew](https://brew.sh/)
3. Verifique se **[!DNL Maven]** está instalado abrindo um novo terminal de linha de comando e executando o seguinte:

   ```shell
   $ mvn --version
   Apache Maven 3.3.9
   Maven home: /Library/apache-maven-3.3.9
   Java version: 1.8.0_111, vendor: Oracle Corporation
   Java home: /Library/Java/JavaVirtualMachines/jdk1.8.0_111.jdk/Contents/Home/jre
   Default locale: en_US, platform encoding: UTF-8
   ```

4. Adicione o **[!DNL adobe-public]** perfil ao seu arquivo [!DNL Maven] settings.xml [para adicionar automaticamente](https://maven.apache.org/settings.html) **[!DNL repo.adobe.com]** ao processo de compilação maven.

5. Crie um arquivo com o nome `settings.xml` em `~/.m2/settings.xml` se ele ainda não existir.

6. Adicione o **[!DNL adobe-public]** perfil ao `settings.xml` arquivo com base [nas instruções aqui](https://repo.adobe.com/).

   Uma amostra `settings.xml` está listada abaixo. *Observe que a convenção de nomenclatura de`settings.xml`e a disposição abaixo do`.m2`diretório do usuário é importante.*

   ```xml
   <settings xmlns="https://maven.apache.org/SETTINGS/1.0.0"
     xmlns:xsi="https://www.w3.org/2001/XMLSchema-instance"
     xsi:schemaLocation="https://maven.apache.org/SETTINGS/1.0.0
                         https://maven.apache.org/xsd/settings-1.0.0.xsd">
   <profiles>
    <!-- ====================================================== -->
    <!-- A D O B E   P U B L I C   P R O F I L E                -->
    <!-- ====================================================== -->
        <profile>
            <id>adobe-public</id>
            <activation>
                <activeByDefault>true</activeByDefault>
            </activation>
            <properties>
                <releaseRepository-Id>adobe-public-releases</releaseRepository-Id>
                <releaseRepository-Name>Adobe Public Releases</releaseRepository-Name>
                <releaseRepository-URL>https://repo.adobe.com/nexus/content/groups/public</releaseRepository-URL>
            </properties>
            <repositories>
                <repository>
                    <id>adobe-public-releases</id>
                    <name>Adobe Public Repository</name>
                    <url>https://repo.adobe.com/nexus/content/groups/public</url>
                    <releases>
                        <enabled>true</enabled>
                        <updatePolicy>never</updatePolicy>
                    </releases>
                    <snapshots>
                        <enabled>false</enabled>
                    </snapshots>
                </repository>
            </repositories>
            <pluginRepositories>
                <pluginRepository>
                    <id>adobe-public-releases</id>
                    <name>Adobe Public Repository</name>
                    <url>https://repo.adobe.com/nexus/content/groups/public</url>
                    <releases>
                        <enabled>true</enabled>
                        <updatePolicy>never</updatePolicy>
                    </releases>
                    <snapshots>
                        <enabled>false</enabled>
                    </snapshots>
                </pluginRepository>
            </pluginRepositories>
        </profile>
   </profiles>
    <activeProfiles>
        <activeProfile>adobe-public</activeProfile>
    </activeProfiles>
   </settings>
   ```

7. Para verificar se o perfil **adobe-public** está ativo, execute o seguinte comando:

   ```shell
   $ mvn help:effective-settings
   ...
   <activeProfiles>
       <activeProfile>adobe-public</activeProfile>
   </activeProfiles>
   <pluginGroups>
       <pluginGroup>org.apache.maven.plugins</pluginGroup>
       <pluginGroup>org.codehaus.mojo</pluginGroup>
   </pluginGroups>
   </settings>
   [INFO] ------------------------------------------------------------------------
   [INFO] BUILD SUCCESS
   [INFO] ------------------------------------------------------------------------
   [INFO] Total time:  0.856 s
   ```

   Se você não vir a mensagem, **[!DNL adobe-public]** isso indica que o acordo de recompra de Adobe não é devidamente referenciado no seu `~/.m2/settings.xml` arquivo. Revise as etapas anteriores e verifique se o arquivo settings.xml faz referência ao Adobe repo.

## Configurar um Ambiente de desenvolvimento integrado

Um ambiente de desenvolvimento integrado ou IDE é um aplicativo que combina um editor de texto, suporte a sintaxe e ferramentas de compilação. Dependendo do tipo de desenvolvimento que você está fazendo, um IDE pode ser preferível a outro. Independentemente do IDE, será importante poder ***enviar*** periodicamente o código para uma instância AEM local para testá-lo. Também será importante ***arrastar*** ocasionalmente configurações de uma instância AEM local para o seu projeto AEM para persistir em um sistema de gerenciamento de controle de origem como o Git.

Veja a seguir alguns dos IDEs mais populares usados com AEM desenvolvimento com vídeos correspondentes que mostram a integração com uma instância AEM local.

### [!DNL Eclipse] IDE

O **[[!DNL Eclipse] IDE](https://www.eclipse.org/ide/)** é um dos IDEs mais populares para o desenvolvimento de Java, em grande parte porque é de código aberto e ***gratuito***! O Adobe fornece um plug-in, **[[!DNL AEM Developer Tools]](https://eclipse.adobe.com/aem/dev-tools/)**, para [!DNL Eclipse] facilitar o desenvolvimento com uma GUI agradável para sincronizar o código com uma instância AEM local. O [!DNL Eclipse] IDE é recomendado para desenvolvedores novos a AEM em grande parte devido ao suporte da GUI por [!DNL AEM Developer Tools].

#### Instalação e configuração

1. Baixe e instale o [!DNL Eclipse] IDE para [!DNL Java EE Developers]: [https://www.eclipse.org](https://www.eclipse.org/)
1. Siga as instruções para instalar o [!DNL AEM Developer Tools] plug-in: [https://eclipse.adobe.com/aem/dev-tools/](https://eclipse.adobe.com/aem/dev-tools/)

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

* 00:30 - Importar projeto Maven
* 01:24 - Criar e implantar o código fonte com o Maven
* 04:33 - Mudanças no código de push com a ferramenta para desenvolvedores AEM
* 10:55 - Pull code changes with AEM Developer Tool
* 13:12 - Usar as ferramentas de depuração integradas do Eclipse

### IntelliJ IDEA

O **[IntelliJ IDEA](https://www.jetbrains.com/idea/)** é um IDE poderoso para o desenvolvimento profissional de Java. [!DNL IntelliJ IDEA] vem em dois sabores, uma edição ***gratuita*** [!DNL Community] e uma versão comercial (paga) [!DNL Ultimate] . A [!DNL Community] versão gratuita do [!DNL IntellIJ IDEA] é suficiente para um desenvolvimento mais AEM, no entanto, o [!DNL Ultimate] expande seu conjunto [](https://www.jetbrains.com/idea/download)de recursos.

#### [!DNL Installation and Setup]

1. Baixe e instale o [!DNL IntelliJ IDEA]: [https://www.jetbrains.com/idea/download](https://www.jetbrains.com/idea/download)
1. Instalar [!DNL Repo] (ferramenta de linha de comando): [https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

>[!VIDEO](https://video.tv.adobe.com/v/26089/?quality=12&learn=on)

* 00:00 - Importar projeto Maven
* 05:47 - Criar e implantar o código fonte com o Maven
* 08:17 - Mudanças de envio com o acordo de recompra
* 14:39 - Puxe as alterações com o acordo de recompra
* 17:25 - Uso das ferramentas de depuração integradas do IntelliJ IDEA

### [!DNL Visual Studio Code]

**[O Visual Studio Code](https://code.visualstudio.com/)** tornou-se rapidamente uma ferramenta favorita para desenvolvedores ****** front-end com suporte aprimorado a JavaScript [!DNL Intellisense]e suporte a depuração de navegador. **[!DNL Visual Studio Code]** é open source, gratuito, com muitas extensões poderosas. [!DNL Visual Studio Code] pode ser configurado para integrar com AEM com a ajuda de uma ferramenta de Adobe, o **[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code).** Há também várias extensões compatíveis com a comunidade que podem ser instaladas para integração com AEM.

[!DNL Visual Studio Code] é uma excelente opção para desenvolvedores front-end que estarão escrevendo código CSS/LESS e JavaScript para criar bibliotecas clientes AEM. Essa ferramenta pode não ser a melhor opção para novos desenvolvedores de AEM, já que as definições de nó (caixas de diálogo, componentes) precisarão ser editadas no XML bruto. Há várias extensões Java disponíveis para [!DNL Visual Studio Code], no entanto, se você fizer principalmente desenvolvimento Java [!DNL Eclipse IDE] ou [!DNL IntelliJ] puder ser preferido.

#### Links importantes

* [**Baixar**](https://code.visualstudio.com/Download) código do **Visual Studio**
* **[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)** - ferramenta semelhante a FTP para conteúdo JCR
* **[aemfeed](https://aemfed.io/)** - acelere o fluxo de trabalho de front-end AEM
* **[AEM Sync](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)** - Extensão compatível com a comunidade* para código do Visual Studio

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

* 00:30 - Importar projeto Maven
* 00:53 - Criar e implantar o código fonte com o Maven
* 04:03 - Mudanças no código de push com a ferramenta de linha de comando do Repo
* 08:29 - Retirar alterações de código com a ferramenta de linha de comando do Repo
* 10:40 - Mudanças no código de push com a ferramenta aprimorada
* 14:24 - Solução de problemas, reconstruir bibliotecas de clientes

### [!DNL CRXDE Lite]

[CRXDE Lite](https://helpx.adobe.com/experience-manager/6-4/sites/developing/using/developing-with-crxde-lite.html) é uma visualização baseada em navegador do repositório AEM. [!DNL CRXDE Lite] está incorporado no AEM e permite que um desenvolvedor execute tarefas de desenvolvimento padrão, como edição de arquivos, definição de componentes, caixas de diálogo e modelos. [!DNL CRXDE Lite] ***não*** se destina a ser um ambiente de desenvolvimento completo, mas é muito eficaz como uma ferramenta de depuração. [!DNL CRXDE Lite] é útil ao expandir ou simplesmente entender o código do produto fora da sua base de código. [!DNL CRXDE Lite] fornece uma visualização poderosa do repositório e uma maneira de testar e gerenciar com eficácia as permissões.

[!DNL CRXDE Lite] deve ser sempre usado em conjunto com outros IDEs para testar e depurar o código, mas nunca como a principal ferramenta de desenvolvimento. Ele tem suporte de sintaxe limitado, não tem recursos de autocompletar e integração limitada com sistemas de gerenciamento de controle de origem.

>[!VIDEO](https://video.tv.adobe.com/v/25917?quality=12&learn=on)

## Resolução de problemas

***Ajuda!*** Meu código não está funcionando! Como em todo o desenvolvimento, haverá momentos (provavelmente muitos) em que seu código não funcionará como esperado. AEM é uma plataforma poderosa, mas com grande poder... vem grande complexidade. Abaixo estão alguns pontos de partida de alto nível quando se trata de solucionar problemas e rastrear problemas (mas longe de uma lista exaustiva de coisas que podem dar errado):

### Verificar implantação do código

Uma boa primeira etapa, ao encontrar um problema, é verificar se o código foi implantado e instalado com êxito no AEM.

1. **Verifique o Gerenciador[!UICONTROL de pacotes]** para garantir que o pacote de códigos tenha sido carregado e instalado: [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). Verifique o carimbo de data/hora para verificar se o pacote foi instalado recentemente.
1. Se estiver fazendo atualizações de arquivos incrementais usando uma ferramenta como [!DNL Repo] ou [!DNL AEM Developer Tools], **verifique[!DNL CRXDE Lite]** se o arquivo foi enviado para a instância AEM local e se o conteúdo do arquivo foi atualizado: [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)
1. **Verifique se o pacote foi carregado** se estiver vendo problemas relacionados ao código Java em um pacote OSGi. Abra o Console [!UICONTROL da Web do]Adobe Experience Manager: [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles) e pesquise seu pacote. Verifique se o pacote tem um status **[!UICONTROL Ativo]** . Consulte abaixo para obter mais informações relacionadas à solução de problemas de um pacote em um estado **[!UICONTROL Instalado]** .

#### Verifique os registros

AEM é uma plataforma de bate-papo e registra muitas informações úteis no **error.log**. O **error.log** pode ser encontrado onde o AEM foi instalado: &lt; `aem-installation-folder>/crx-quickstart/logs/error.log`.

Uma técnica útil para rastrear problemas é adicionar declarações de log no código Java:

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

Por padrão, o **error.log** está configurado para registrar *[!DNL INFO]* instruções. Se quiser alterar o nível de log, você pode fazer isso indo para [!UICONTROL Log Support]: [http://localhost:4502/system/console/slinglog](http://localhost:4502/system/console/slinglog). Você também pode descobrir que o **error.log** é muito chatty. Você pode usar o suporte [!UICONTROL ao] registro para configurar declarações de registro para apenas um pacote Java especificado. Esta é uma prática recomendada para projetos, a fim de separar facilmente os problemas de código personalizado dos problemas da plataforma AEM OOTB.

![Configuração de registro em log no AEM](./assets/set-up-a-local-aem-development-environment/logging.png)

#### O pacote está em um estado instalado {#bundle-active}

Todos os pacotes (exceto Fragmentos) devem estar em um estado **[!UICONTROL Ativo]** . Se você vir seu conjunto de códigos em um estado [!UICONTROL Instalado] , então há um problema que precisa ser resolvido. Na maioria das vezes, esse é um problema de dependência:

![Erro de pacote no AEM](assets/set-up-a-local-aem-development-environment/bundle-error.png)

Na captura de tela acima, [!DNL WKND Core bundle] há um estado [!UICONTROL Instalado] . Isso ocorre porque o pacote espera uma versão diferente de `com.adobe.cq.wcm.core.components.models` que está disponível na instância AEM.

Uma ferramenta útil que pode ser usada é o Localizador [!UICONTROL de]Dependência: [http://localhost:4502/system/console/depfinder](http://localhost:4502/system/console/depfinder). Adicione o nome do pacote Java para verificar qual versão está disponível na instância AEM:

![Componentes principais](assets/set-up-a-local-aem-development-environment/core-components.png)

Continuando com o exemplo acima, podemos ver que a versão instalada na instância AEM é **12.2** vs **12.6** que o pacote esperava. A partir daí você pode trabalhar para trás e ver se as [!DNL Maven] dependências em AEM correspondem às [!DNL Maven] dependências no projeto AEM. No exemplo acima, a [!DNL Core Components] v2.2.0 **está instalada na instância AEM, mas o conjunto de códigos foi criado com uma dependência da** v2.2.2 ****, portanto, o motivo do problema de dependência.

#### Verifique o registro de modelos Sling {#osgi-component-sling-models}

AEM componentes devem ser sempre protegidos por um backup para encapsular qualquer lógica comercial e garantir que o script de renderização HTL permaneça limpo. [!DNL Sling Model] Se houver problemas em que o Modelo Sling não pode ser encontrado, talvez seja útil verificar o [!DNL Sling Models] no console: [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels). Isso informará se seu Modelo Sling foi registrado e a que tipo de recurso (o caminho do componente) ele está vinculado.

![Status do modelo Sling](assets/set-up-a-local-aem-development-environment/sling-model-status.png)

Mostra o registro de um [!DNL Sling Model], `BylineImpl` que está vinculado a um tipo de recurso de componente de `wknd/components/content/byline`.

#### Problemas de CSS ou JavaScript

Para a maioria dos problemas de CSS e JavaScript, usar as ferramentas de desenvolvimento do navegador é a maneira mais eficaz de solucionar problemas. Para restringir o problema ao desenvolver em relação a uma instância do autor AEM, é útil visualização a página &quot;como publicada&quot;.

![Problemas de CSS ou JS](assets/set-up-a-local-aem-development-environment/css-and-js-issues.png)

Open the [!UICONTROL Page Properties] menu and click [!UICONTROL View as Published]. Isso abrirá a página sem o editor de AEM e com um parâmetro de query definido como **wcmmode=disabled**. Isso desativará efetivamente a interface de criação de AEM e facilitará a solução de problemas/a depuração de problemas de front-end.

Outro problema frequentemente encontrado ao desenvolver o código front-end é o CSS/JS antigo ou desatualizado que está sendo carregado. Como primeira etapa, verifique se o histórico do navegador foi limpo e, se necessário, start um navegador incógnito ou uma sessão de limpeza.

#### Depuração das bibliotecas do cliente

Com diferentes métodos de categorias e incorporações para incluir várias bibliotecas de clientes, pode ser difícil solucionar problemas. AEM expõe várias ferramentas para ajudar nisso. Uma das ferramentas mais importantes é a [!UICONTROL reconstrução de bibliotecas] de clientes que forçará a AEM a recompilar quaisquer arquivos MENOS e a gerar o CSS.

* [Dump Libs](http://localhost:4502/libs/granite/ui/content/dumplibs.html) - Lista todas as bibliotecas clientes registradas na instância AEM. &lt;host>/libs/granite/ui/content/dumplibs.html
* [Saída](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) de teste - permite que um usuário veja a saída HTML esperada de clientlib include com base na categoria. &lt;host>/libs/granite/ui/content/dumplibs.test.html
* [Validação](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) de dependências de bibliotecas - realça quaisquer dependências ou categorias incorporadas que não possam ser encontradas. &lt;host>/libs/granite/ui/content/dumplibs.validate.html
* [Recriar bibliotecas](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) de clientes - permite que um usuário force AEM reconstruir todas as bibliotecas de clientes ou invalidar o cache das bibliotecas de clientes. Essa ferramenta é particularmente eficaz ao desenvolver com MENOS, pois pode forçar AEM a recompilar o CSS gerado. Em geral, é mais eficaz invalidar caches e, em seguida, executar uma atualização de página em vez de recriar todas as bibliotecas. &lt;host>/libs/granite/ui/content/dumplibs.rebuild.html

![Depuração de Clientlibs](assets/set-up-a-local-aem-development-environment/debugging-clientlibs.png)

>[!NOTE]
>
>Se você tiver que invalidar constantemente o cache usando a ferramenta [!UICONTROL Reconstruir bibliotecas] de clientes, pode ser útil fazer uma recriação única de todas as bibliotecas de clientes. Isso pode levar cerca de 15 minutos, mas normalmente elimina quaisquer problemas de cache no futuro.
