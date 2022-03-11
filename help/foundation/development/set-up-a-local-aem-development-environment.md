---
title: Configurar um Ambiente de desenvolvimento do AEM local
description: Guia para configurar um desenvolvimento local para o Adobe Experience Manager, AEM. Abrange tópicos importantes de instalação local, Apache Maven, ambientes de desenvolvimento integrados e depuração/solução de problemas. O desenvolvimento com o Eclipse IDE, CRXDE-Lite, Visual Studio Code e IntelliJ são discutidos.
version: 6.4, 6.5
feature: Developer Tools
topics: development
activity: develop
audience: developer
topic: Development
role: Developer
level: Beginner
exl-id: 58851624-71c9-4745-aaaf-305acf6ccb14
source-git-commit: 25a1a40f42d37443db9edc0e09b1691b1c19e848
workflow-type: tm+mt
source-wordcount: '2582'
ht-degree: 2%

---

# Configurar um Ambiente de desenvolvimento do AEM local

Guia para configurar um desenvolvimento local para o Adobe Experience Manager, AEM. Abrange tópicos importantes de instalação local, Apache Maven, ambientes de desenvolvimento integrados e depuração/solução de problemas. Desenvolvimento com **[!DNL Eclipse IDE], [!DNL CRXDE Lite], [!DNL Visual Studio Code] e[!DNL IntelliJ]** são discutidas.

## Visão geral

A configuração de um ambiente de desenvolvimento local é a primeira etapa do desenvolvimento para Adobe Experience Manager ou AEM. Reserve tempo para configurar um ambiente de desenvolvimento de qualidade para aumentar a produtividade e escrever melhor código, mais rapidamente. Podemos dividir um ambiente AEM de desenvolvimento local em quatro áreas:

* Instâncias de AEM locais
* [!DNL Apache Maven] projeto
* Ambientes de desenvolvimento integrados (IDE)
* Resolução de problemas

## Instalar instâncias de AEM locais

Quando nos referimos a uma instância de AEM local, estamos falando de uma cópia do Adobe Experience Manager que está sendo executada em uma máquina pessoal de desenvolvedor. ***Todos*** AEM desenvolvimento deve começar por escrever e executar código em uma instância de AEM local.

Se você não AEM, há dois modos de execução básicos que podem ser instalados: ***Autor*** e ***Publicar***. O ***Autor*** [runmode](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/configure-runmodes.html)  é o ambiente que os profissionais de marketing digital usarão para criar e gerenciar conteúdo. Ao desenvolver **more** do tempo em que você implantará o código em uma instância do autor. Isso permite criar novas páginas, bem como adicionar e configurar componentes. O AEM Sites é um CMS de criação WYSIWYG e, portanto, a maioria do CSS e do JavaScript pode ser testada em relação a uma instância de criação.

É também *crítico* código de teste em relação a um local ***Publicar*** instância. O ***Publicar*** é o ambiente de AEM com o qual os visitantes do site interagirão. Enquanto a variável ***Publicar*** A instância é a mesma pilha de tecnologia que a ***Autor*** Por exemplo, há algumas distinções importantes com configurações e permissões. O código deve *always* ser testado contra um ***Publicar*** antes de ser promovida para ambientes de nível superior.

### Etapas

1. Garantir [Java](https://downloads.experiencecloud.adobe.com/content/software-distribution/en/general.html) está instalado.
   * Preferência [Java JDK 11](https://experience.adobe.com/#/downloads/content/software-distribution/en/general.html?1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=&amp;p.offset=0&amp;p.limit=14) para AEM 6.5+
   * [Java JDK 8](https://www.oracle.com/technetwork/java/javase/downloads/index.html#JDK8) para versões AEM anteriores ao AEM 6.5
2. Obtenha uma cópia do [AEM QuickStart Jar e um [!DNL license.properties]](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html#GettingtheSoftware).
3. Crie uma estrutura de pastas no computador como a seguinte:

   ```plain
   ~/aem-sdk
       /author
       /publish
   ```

4. Renomeie o [!DNL QuickStart] JAR para ***aem-author-p4502.jar*** e coloque-a abaixo da `/author` diretório. Adicione o ***[!DNL license.properties]*** abaixo do `/author` diretório.
5. Faça uma cópia do [!DNL QuickStart] JAR, renomeie-o para ***aem-publish-p4503.jar*** e coloque-a abaixo da `/publish` diretório. Adicione uma cópia do ***[!DNL license.properties]*** abaixo do `/publish` diretório.

   ```plain
   ~/aem-sdk
       /author
           + aem-author-p4502.jar
           + license.properties
       /publish
           + aem-publish-p4503.jar
           + license.properties
   ```

6. Clique duas vezes no botão ***aem-author-p4502.jar*** para instalar o **Autor** instância. Isso iniciará a instância do autor, executando na porta **4502** no computador local.

   Clique duas vezes no botão ***aem-publish-p4503.jar*** para instalar o **Publicar** instância. Isso iniciará a instância de publicação, em execução na porta **4503** no computador local.

   >[!NOTE]
   >
   >Dependendo do hardware da máquina de desenvolvimento, pode ser difícil ter uma **Autor e publicação** instância em execução ao mesmo tempo. Raramente é necessário executar ambos simultaneamente em uma configuração local.

   Para obter mais informações, consulte [Implantação e manutenção de uma instância de AEM](https://helpx.adobe.com/experience-manager/6-5/sites/deploying/using/deploy.html).

## Instalar o Apache Maven

***[!DNL Apache Maven]*** O é uma ferramenta para gerenciar o procedimento de criação e implantação de projetos baseados em Java. AEM é uma plataforma baseada em Java e [!DNL Maven] é a maneira padrão de gerenciar o código de um projeto AEM. Quando dizemos ***Projeto AEM Maven*** ou apenas seu ***Projeto AEM***, estamos nos referindo a um projeto Maven que inclui todos os *custom* código do site.

Todos os projetos AEM devem ser construídos a partir da versão mais recente do **[!DNL AEM Project Archetype]**: [https://github.com/adobe/aem-project-archetype](https://github.com/adobe/aem-project-archetype). O [!DNL AEM Project Archetype] O criará um bootstrap de um projeto AEM com alguns códigos e conteúdos de amostra. O [!DNL AEM Project Archetype] também inclui **[!DNL AEM WCM Core Components]** configurado para ser usado em seu projeto.

>[!CAUTION]
>
>Ao iniciar um novo projeto, é uma prática recomendada usar a versão mais recente do arquétipo. Lembre-se de que há várias versões do arquétipo e nem todas as versões são compatíveis com as versões anteriores do AEM.

### Etapas

1. Baixar [Apache Maven](https://maven.apache.org/download.cgi)
2. Instalar [Apache Maven](https://maven.apache.org/install.html) e certifique-se de que a instalação foi adicionada à sua linha de comando `PATH`.
   * [!DNL macOS] os usuários podem instalar o Maven usando [Homebrew](https://brew.sh/)
3. Verifique se **[!DNL Maven]** O é instalado abrindo um novo terminal de linha de comando e executando o seguinte:

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
   > No passado, a adição de `adobe-public` O perfil Maven era necessário para apontar `nexus.adobe.com` para baixar artefatos AEM. Todos os artefatos AEM estão disponíveis no Maven Central e no `adobe-public` não é necessário.

## Configurar um ambiente de desenvolvimento integrado

Um ambiente de desenvolvimento integrado ou IDE é um aplicativo que combina um editor de texto, suporte a sintaxe e ferramentas de compilação. Dependendo do tipo de desenvolvimento que você está fazendo, um IDE pode ser preferível ao outro. Independentemente do IDE, será importante poder fazer periodicamente ***push*** código para uma instância de AEM local para testá-la. Também será importante ocasionalmente ***pull*** configurações de uma instância de AEM local no seu projeto AEM para persistir em um sistema de gerenciamento de controle de origem como o Git.

Abaixo estão alguns dos IDEs mais populares que são usados com AEM desenvolvimento com vídeos correspondentes que mostram a integração com uma instância de AEM local.

>[!NOTE]
>
> O Projeto WKND foi atualizado para funcionar AEM as a Cloud Service por padrão. Foi atualizado para ser [compatível com versões anteriores da 6.5/6.4](https://github.com/adobe/aem-guides-wknd#building-for-aem-6xx). Se estiver usando AEM 6.5 ou 6.4, anexe a `classic` para qualquer comando Maven.

```shell
$ mvn clean install -PautoInstallSinglePackage -Pclassic
```

Ao usar um IDE, verifique se `classic` na guia Perfil do Maven.

![Guia Perfil Maven](assets/set-up-a-local-aem-development-environment/intelliJMavenProfiles.png)

*Perfil do IntelliJ Maven*

### [!DNL Eclipse] IDE

O **[[!DNL Eclipse] IDE](https://www.eclipse.org/ide/)** é um dos IDEs mais populares para desenvolvimento de Java, em grande parte porque é de código aberto e ***grátis***! o Adobe fornece um plug-in, **[[!DNL AEM Developer Tools]](https://experienceleague.adobe.com/docs/experience-manager-64/developing/devtools/aem-eclipse.html)** para [!DNL Eclipse] para facilitar o desenvolvimento com uma GUI simpática para sincronizar o código com uma instância de AEM local. O [!DNL Eclipse] O IDE é recomendado para desenvolvedores novos a AEM em grande parte devido ao suporte da GUI por [!DNL AEM Developer Tools].

#### Instalação e configuração

1. Baixe e instale o [!DNL Eclipse] IDE para [!DNL Java EE Developers]: [https://www.eclipse.org](https://www.eclipse.org/)
1. Siga as instruções para instalar o [!DNL AEM Developer Tools] plug-in: [https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html](https://experienceleague.adobe.com/docs/experience-manager-65/developing/devtools/aem-eclipse.html)

>[!VIDEO](https://video.tv.adobe.com/v/25906?quality=12&learn=on)

* 00:30 - Importar projeto Maven
* 01:24 - Criar e implantar o código-fonte com o Maven
* 04:33 - Mudanças de código de push com a ferramenta AEM Developer
* 10:55 - Mudanças de código de pull com a ferramenta AEM Developer
* 13:12 - Uso das ferramentas de depuração integradas do Eclipse

### IntelliJ IDEA

O **[IntelliJ IDEA](https://www.jetbrains.com/idea/)** O é um IDE poderoso para desenvolvimento profissional de Java. [!DNL IntelliJ IDEA] vem em dois sabores, uma ***grátis*** [!DNL Community] edição e um anúncio comercial (pago) [!DNL Ultimate] versão. O livre [!DNL Community] versão de [!DNL IntellIJ IDEA] é suficiente para um desenvolvimento mais AEM, no entanto [!DNL Ultimate] [expande seu conjunto de recursos](https://www.jetbrains.com/idea/download).

#### [!DNL Installation and Setup]

1. Baixe e instale o [!DNL IntelliJ IDEA]: [https://www.jetbrains.com/idea/download](https://www.jetbrains.com/idea/download)
1. Instalar [!DNL Repo] (ferramenta de linha de comando): [https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#installation)

>[!VIDEO](https://video.tv.adobe.com/v/26089/?quality=12&learn=on)

* 00:00 - Importar projeto Maven
* 05:47 - Criar e implantar o código-fonte com o Maven
* 08:17 - Encaminhar as alterações com o Repo
* 14:39 - Puxe as alterações com o Repo
* 17:25 - Uso das ferramentas de depuração integradas do IntelliJ IDEA

### [!DNL Visual Studio Code]

**[Código do Visual Studio](https://code.visualstudio.com/)** tornou-se rapidamente uma ferramenta favorita para ***desenvolvedores de front-end*** com suporte avançado a JavaScript, [!DNL Intellisense]e suporte à depuração do navegador. **[!DNL Visual Studio Code]** O é de código aberto, gratuito, com muitas extensões poderosas. [!DNL Visual Studio Code] pode ser configurado para integrar com o AEM com a ajuda de uma ferramenta Adobe, **[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code).** Há também várias extensões compatíveis com a comunidade que podem ser instaladas para integração com o AEM.

[!DNL Visual Studio Code] A é uma ótima opção para desenvolvedores front-end que gravarão primariamente CSS/LESS e código JavaScript para criar bibliotecas AEM cliente. Essa ferramenta pode não ser a melhor opção para novos desenvolvedores de AEM, pois as definições de nó (caixas de diálogo, componentes) precisarão ser editadas em XML bruto. Há várias extensões Java disponíveis para [!DNL Visual Studio Code]no entanto, se você fizer principalmente desenvolvimento Java [!DNL Eclipse IDE] ou [!DNL IntelliJ] pode ser preferido.

#### Links importantes

* [**Baixar**](https://code.visualstudio.com/Download) **Código do Visual Studio**
* **[repo](https://github.com/Adobe-Marketing-Cloud/tools/tree/master/repo#integration-into-visual-studio-code)** - Ferramenta semelhante a FTP para conteúdo JCR
* **[amamentado](https://aemfed.io/)** - Acelere seu fluxo de trabalho AEM front-end
* **[Sincronização de AEM](https://marketplace.visualstudio.com/items?itemName=Yinkai15.aemsync)** - Extensão Community supported* para código Visual Studio

>[!VIDEO](https://video.tv.adobe.com/v/25907?quality=12&learn=on)

* 00:30 - Importar projeto Maven
* 00:53 - Criar e implantar o código-fonte com o Maven
* 04:03 - Empurrar alterações de código com a ferramenta de linha de comando do Repo
* 08:29 - Puxe as alterações no código com a ferramenta de linha de comando do Repo
* 10:40 - Mudanças no código de push com a ferramenta aprimorada
* 14:24 - Solução de problemas, Reconstruir bibliotecas de clientes

### [!DNL CRXDE Lite]

[CRXDE Lite](https://helpx.adobe.com/experience-manager/6-4/sites/developing/using/developing-with-crxde-lite.html) é uma visualização baseada em navegador do repositório de AEM. [!DNL CRXDE Lite] O está incorporado no AEM e permite que um desenvolvedor execute tarefas de desenvolvimento padrão, como editar arquivos, definir componentes, caixas de diálogo e modelos. [!DNL CRXDE Lite] é ***not*** O deve ser um ambiente de desenvolvimento completo, mas é muito eficaz como uma ferramenta de depuração. [!DNL CRXDE Lite] é útil ao estender ou simplesmente entender o código do produto fora da sua base de código. [!DNL CRXDE Lite] O fornece uma visualização eficiente do repositório e uma maneira de testar e gerenciar com eficácia as permissões.

[!DNL CRXDE Lite] O deve ser sempre usado junto com outros IDEs para testar e depurar o código, mas nunca como a ferramenta de desenvolvimento principal. Ele tem suporte de sintaxe limitado, não possui recursos de preenchimento automático e integração limitada com sistemas de gerenciamento de controle de origem.

>[!VIDEO](https://video.tv.adobe.com/v/25917?quality=12&learn=on)

## Resolução de problemas

***Ajuda!*** Meu código não está funcionando! Como em todo o desenvolvimento, haverá momentos (provavelmente muitos), em que o código não está funcionando como esperado. AEM é uma plataforma poderosa, mas com grande poder.. vem grande complexidade. Abaixo estão alguns pontos de partida de alto nível quando se trata de solucionar problemas e rastrear problemas (mas longe de uma lista exaustiva de coisas que podem dar errado):

### Verificar implantação do código

Uma boa primeira etapa, ao encontrar um problema, é verificar se o código foi implantado e instalado com êxito no AEM.

1. **Verificar [!UICONTROL Gerenciador de pacotes]** para garantir que o pacote de código tenha sido carregado e instalado: [http://localhost:4502/crx/packmgr/index.jsp](http://localhost:4502/crx/packmgr/index.jsp). Verifique o carimbo de data/hora para verificar se o pacote foi instalado recentemente.
1. Se estiver fazendo atualizações de arquivos incrementais usando uma ferramenta como [!DNL Repo] ou [!DNL AEM Developer Tools], **check[!DNL CRXDE Lite]** que o arquivo foi enviado para a instância de AEM local e que o conteúdo do arquivo é atualizado: [http://localhost:4502/crx/de/index.jsp](http://localhost:4502/crx/de/index.jsp)
1. **Verifique se o pacote foi carregado** se estiver vendo problemas relacionados ao código Java em um pacote OSGi. Abra o [!UICONTROL Adobe Experience Manager Web Console]: [http://localhost:4502/system/console/bundles](http://localhost:4502/system/console/bundles) e procure seu pacote. Certifique-se de que o pacote tenha um **[!UICONTROL Ativo]** status. Veja abaixo mais informações relacionadas à solução de problemas de um pacote em um **[!UICONTROL Instalado]** estado.

#### Verifique os logs

AEM é uma plataforma de bate-papo e registra muitas informações úteis na **error.log**. O **error.log** pode ser encontrada onde AEM foi instalado: &lt; `aem-installation-folder>/crx-quickstart/logs/error.log`.

Uma técnica útil para rastrear problemas é adicionar instruções de log no código Java:

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

Por padrão, a variável **error.log** está configurado para registrar *[!DNL INFO]* instruções. Se quiser alterar o nível do log, faça isso acessando [!UICONTROL Suporte de log]: [http://localhost:4502/system/console/slinglog](http://localhost:4502/system/console/slinglog). Você também pode achar que a variável **error.log** é conversa demais. Você pode usar o [!UICONTROL Suporte de log] para configurar instruções de log para apenas um pacote Java especificado. Essa é uma prática recomendada para projetos, a fim de separar facilmente os problemas de código personalizado dos problemas da plataforma de AEM OOTB.

![Configuração de logon no AEM](./assets/set-up-a-local-aem-development-environment/logging.png)

#### O pacote está em um estado Instalado {#bundle-active}

Todos os pacotes (exceto Fragmentos) devem estar em um **[!UICONTROL Ativo]** estado. Se você vir seu pacote de códigos em um [!UICONTROL Instalado] em seguida, há um problema que precisa ser resolvido. Na maioria das vezes, esse é um problema de dependência:

![Erro do pacote no AEM](assets/set-up-a-local-aem-development-environment/bundle-error.png)

Na captura de tela acima, adicione [!DNL WKND Core bundle] é um [!UICONTROL Instalado] estado. Isso ocorre porque o pacote espera uma versão diferente de `com.adobe.cq.wcm.core.components.models` que está disponível na instância de AEM.

Uma ferramenta útil que pode ser usada é a [!UICONTROL Localizador de dependências]: [http://localhost:4502/system/console/depfinder](http://localhost:4502/system/console/depfinder). Adicione o nome do pacote Java para verificar qual versão está disponível na instância do AEM:

![Componentes principais](assets/set-up-a-local-aem-development-environment/core-components.png)

Continuando com o exemplo acima, podemos ver que a versão instalada na instância de AEM é **n.º 2** vs **12,6** que o pacote estava esperando. A partir daí você pode trabalhar para trás e ver se a variável [!DNL Maven] as dependências no AEM correspondem à variável [!DNL Maven] no projeto AEM. No exemplo acima [!DNL Core Components] **v2.2.0** está instalado na instância de AEM, mas o pacote de códigos foi criado com uma dependência em **v2.2.2**, portanto, o motivo para o problema de dependência.

#### Verificar o registro de modelos Sling {#osgi-component-sling-models}

AEM componentes devem sempre ter um backup de [!DNL Sling Model] para encapsular qualquer lógica de negócios e garantir que o script de renderização do HTL permaneça limpo. Se tiver problemas em que o Modelo do Sling não pode ser encontrado, pode ser útil verificar a variável [!DNL Sling Models] do console: [http://localhost:4502/system/console/status-slingmodels](http://localhost:4502/system/console/status-slingmodels). Isso informará se o Modelo do Sling foi registrado e a que tipo de recurso (o caminho do componente) ele está vinculado.

![Status do Modelo Sling](assets/set-up-a-local-aem-development-environment/sling-model-status.png)

Mostra o registro de um [!DNL Sling Model], `BylineImpl` que está vinculado a um tipo de recurso componente de `wknd/components/content/byline`.

#### Problemas de CSS ou JavaScript

Para a maioria dos problemas de CSS e JavaScript, usar as ferramentas de desenvolvimento do navegador é a maneira mais eficaz de solucionar problemas. Para restringir o problema ao desenvolver em relação a uma instância de autor de AEM, é útil visualizar a página &quot;como publicada&quot;.

![Problemas de CSS ou JS](assets/set-up-a-local-aem-development-environment/css-and-js-issues.png)

Abra o [!UICONTROL Propriedades da página] e clique em [!UICONTROL Exibir como publicado]. Isso abrirá a página sem o editor de AEM e com um parâmetro de consulta definido como **wcmmode=disabled**. Isso desativará efetivamente a interface de criação de AEM e facilitará a solução de problemas/depuração de problemas de front-end.

Outro problema comumente encontrado ao desenvolver código front-end é o CSS/JS antigo ou desatualizado que está sendo carregado. Como primeiro passo, verifique se o histórico do navegador foi limpo e, se necessário, inicie um navegador incógnito ou uma nova sessão.

#### Depuração de bibliotecas de clientes

Com diferentes métodos de categorias e incorporações para incluir várias bibliotecas de clientes, pode ser difícil solucionar problemas. AEM expõe várias ferramentas para ajudar nisso. Uma das ferramentas mais importantes é [!UICONTROL Recriar bibliotecas de clientes] que forçará o AEM a recompilar quaisquer arquivos MENOS e gerar o CSS.

* [Despejar dedos](http://localhost:4502/libs/granite/ui/content/dumplibs.html) - Lista todas as bibliotecas de clientes registradas na instância de AEM. &lt;host>/libs/granite/ui/content/dumplibs.html
* [Saída de teste](http://localhost:4502/libs/granite/ui/content/dumplibs.test.html) - permite que um usuário veja a saída HTML esperada das inclusões de clientlib com base na categoria. &lt;host>/libs/granite/ui/content/dumplibs.test.html
* [Validação de dependências de bibliotecas](http://localhost:4502/libs/granite/ui/content/dumplibs.validate.html) - destaca as dependências ou categorias incorporadas que não podem ser encontradas. &lt;host>/libs/granite/ui/content/dumplibs.validate.html
* [Recriar bibliotecas de clientes](http://localhost:4502/libs/granite/ui/content/dumplibs.rebuild.html) - permite que um usuário force o AEM a reconstruir todas as bibliotecas do cliente ou invalidar o cache das bibliotecas do cliente. Essa ferramenta é particularmente eficaz ao desenvolver com MENOS, pois pode forçar o AEM a recompilar o CSS gerado. Em geral, é mais eficaz Invalidar caches e depois executar uma atualização de página em vez de reconstruir todas as bibliotecas. &lt;host>/libs/granite/ui/content/dumplibs.rebuild.html

![Depurando Clientlibs](assets/set-up-a-local-aem-development-environment/debugging-clientlibs.png)

>[!NOTE]
>
>Se você tiver que invalidar constantemente o cache usando o [!UICONTROL Recriar bibliotecas de clientes] pode valer a pena fazer uma recriação única de todas as bibliotecas de clientes. Isso pode levar cerca de 15 minutos, mas normalmente elimina problemas de armazenamento em cache no futuro.
