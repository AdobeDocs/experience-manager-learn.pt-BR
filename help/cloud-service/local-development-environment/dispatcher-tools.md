---
title: Configurar ferramentas do Dispatcher para desenvolvimento em AEM as a Cloud Service
description: As Ferramentas do Dispatcher do AEM SDK facilitam o desenvolvimento local de projetos do Adobe Experience Manager (AEM), facilitando a instalação, a execução e a solução de problemas do Dispatcher localmente.
version: Experience Manager as a Cloud Service
topic: Development
feature: Dispatcher, Developer Tools
role: Developer
level: Beginner
jira: KT-4679
thumbnail: 30603.jpg
last-substantial-update: 2023-03-14T00:00:00Z
exl-id: 9320e07f-be5c-42dc-a4e3-aab80089c8f7
duration: 624
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1620'
ht-degree: 4%

---

# Configurar ferramentas locais do Dispatcher {#set-up-local-dispatcher-tools}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_dispatcher"
>title="Ferramentas locais do Dispatcher"
>abstract="O Dispatcher é parte integrante da arquitetura geral do Experience Manager e deve fazer parte da configuração de desenvolvimento local. O SDK do AEM as a Cloud Service inclui a versão recomendada das ferramentas do Dispatcher, o que facilita a configuração, validação e simulação do Dispatcher localmente."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/disp-overview.html?lang=pt-BR" text="Dispatcher na nuvem"
>additional-url="https://experienceleague.adobe.com/docs/experience-cloud/software-distribution/home.html?lang=pt-br" text="Baixar SDK do AEM as a Cloud Service"

O Dispatcher do Adobe Experience Manager (AEM) é um módulo de servidor Web Apache HTTP que fornece uma camada de segurança e desempenho entre a camada do CDN e do AEM Publish. O Dispatcher é parte integrante da arquitetura geral do Experience Manager e deve fazer parte da configuração de desenvolvimento local.

O AEM as a Cloud Service SDK inclui a versão recomendada das Ferramentas do Dispatcher, que facilita a configuração, validação e simulação do Dispatcher localmente. O Dispatcher Tools é composto por:

+ um conjunto de linhas de base de arquivos de configuração do Apache HTTP Web Server e do Dispatcher, localizado em `.../dispatcher-sdk-x.x.x/src`
+ uma ferramenta CLI do validador de configuração, localizada em `.../dispatcher-sdk-x.x.x/bin/validate`
+ uma ferramenta de CLI de geração de configuração, localizada em `.../dispatcher-sdk-x.x.x/bin/validator`
+ uma ferramenta de CLI de implantação de configuração, localizada em `.../dispatcher-sdk-x.x.x/bin/docker_run`
+ arquivos de configuração imutáveis substituindo a ferramenta CLI, localizados em `.../dispatcher-sdk-x.x.x/bin/update_maven`
+ uma imagem Docker que executa o Apache HTTP Web Server com o módulo Dispatcher

Observe que `~` é usado como abreviação para o Diretório do Usuário. No Windows, é equivalente a `%HOMEPATH%`.

>[!NOTE]
>
> Os vídeos desta página foram gravados no macOS. Os usuários do Windows podem seguir, mas usar os comandos equivalentes do Windows Ferramentas do Dispatcher, fornecidos com cada vídeo.

## Pré-requisitos

1. Os usuários do Windows devem usar o Windows 10 Professional (ou uma versão que ofereça suporte ao Docker)
1. Instale o [Experience Manager Publish Quickstart Jar](./aem-runtime.md) no computador de desenvolvimento local.

+ Opcionalmente, instale o [site de referência do AEM](https://github.com/adobe/aem-guides-wknd/releases) mais recente no serviço de Publicação do AEM local. Este site é usado neste tutorial para visualizar um Dispatcher funcional.

1. Instale e inicie a versão mais recente do [Docker](https://www.docker.com/) (Docker Desktop 2.2.0.5+ / Docker Engine v19.03.9+) na máquina de desenvolvimento local.

## Baixe as Ferramentas do Dispatcher (como parte do AEM SDK)

O AEM as a Cloud Service SDK, ou AEM SDK, contém as Ferramentas do Dispatcher usadas para executar o Apache HTTP Web Server com o módulo Dispatcher localmente para desenvolvimento e o QuickStart Jar compatível.

Se o AEM as a Cloud Service SDK já tiver sido baixado para [configurar o tempo de execução local do AEM](./aem-runtime.md), ele não precisará ser baixado novamente.

1. Faça logon em [experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=list&amp;p.offset=0&amp;p.limit=1) com sua Adobe ID
   + Sua Organização da Adobe __deve__ ser provisionada para que a AEM as a Cloud Service baixe o AEM as a Cloud Service SDK
1. Clique na última linha de resultados __AEM SDK__ para baixar

## Extraia as ferramentas do Dispatcher do zip do AEM SDK

>[!TIP]
>
> Os usuários do Windows não podem ter espaços ou caracteres especiais no caminho para a pasta que contém as Ferramentas locais do Dispatcher. Se houver espaços no caminho, `docker_run.cmd` falhará.

A versão das Ferramentas do Dispatcher é diferente da versão do AEM SDK. Verifique se a versão das Ferramentas do Dispatcher é fornecida por meio da versão do AEM SDK correspondente à versão do AEM as a Cloud Service.

1. Descompacte o arquivo `aem-sdk-xxx.zip` baixado
1. Descompactar as Ferramentas do Dispatcher em `~/aem-sdk/dispatcher`

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh
$ ./aem-sdk-dispatcher-tools-x.x.x-unix.sh
```

>[!TAB Windows]

Descompacte `aem-sdk-dispatcher-tools-x.x.x-windows.zip` em `C:\Users\<My User>\aem-sdk\dispatcher` (criando pastas ausentes conforme necessário).

>[!TAB Linux®]

```shell
$ chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh
$ ./aem-sdk-dispatcher-tools-x.x.x-unix.sh
```

>[!ENDTABS]

Todos os comandos emitidos abaixo pressupõem que o diretório de trabalho atual contém o conteúdo de expansão das Ferramentas do Dispatcher.

>[!VIDEO](https://video.tv.adobe.com/v/30601?quality=12&learn=on)

*Este vídeo usa o macOS para fins ilustrativos. Os comandos equivalentes do Windows/Linux podem ser usados para obter resultados semelhantes.*

## Entender os arquivos de configuração do Dispatcher

>[!TIP]
> Os projetos do Experience Manager criados a partir do [Arquétipo Maven de projeto do AEM](https://github.com/adobe/aem-project-archetype) são preenchidos previamente com esse conjunto de arquivos de configuração do Dispatcher, portanto, não há necessidade de copiar da pasta src de Ferramentas do Dispatcher.

As Ferramentas do Dispatcher fornecem um conjunto de arquivos de configuração do Apache HTTP Web Server e do Dispatcher que definem o comportamento de todos os ambientes, incluindo o desenvolvimento local.

Esses arquivos devem ser copiados para um projeto Experience Manager Maven na pasta `dispatcher/src`, se não existirem no projeto Experience Manager Maven.

Uma descrição completa dos arquivos de configuração está disponível nas Ferramentas do Dispatcher descompactadas como `dispatcher-sdk-x.x.x/docs/Config.html`.

## Validar configurações

Como opção, as configurações do servidor Web Dispatcher e Apache (via `httpd -t`) podem ser validadas usando o script `validate` (não deve ser confundido com o executável `validator`). O script `validate` fornece uma maneira conveniente de executar as [três fases](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/implementing/content-delivery/validation-debug.html?lang=pt-BR) de `validator`.


>[!BEGINTABS]

>[!TAB macOS]

```shell
$ ./bin/validate.sh ./src
```

>[!TAB Windows]

```shell
$ bin\validate src
```

>[!TAB Linux®]

```shell
$ ./bin/validate.sh ./src
```

>[!ENDTABS]

## Executar o Dispatcher localmente

O AEM Dispatcher é executado localmente usando o Docker em relação aos arquivos de configuração do Dispatcher e do Apache Web Server `src`.


>[!BEGINTABS]

>[!TAB macOS]

```shell
$ ./bin/docker_run_hot_reload.sh <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>
```

O executável `docker_run_hot_reload` é preferível sobre `docker_run`, pois recarrega os arquivos de configuração à medida que são alterados, sem precisar encerrar e reiniciar manualmente o `docker_run`. Alternativamente, `docker_run` pode ser usado, mas requer o encerramento e a reinicialização manual de `docker_run` quando os arquivos de configuração forem alterados.

>[!TAB Windows]

```shell
$ bin\docker_run <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>
```

>[!TAB Linux®]

```shell
$ ./bin/docker_run_hot_reload.sh <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>
```

O executável `docker_run_hot_reload` é preferível sobre `docker_run`, pois recarrega os arquivos de configuração à medida que são alterados, sem precisar encerrar e reiniciar manualmente o `docker_run`. Alternativamente, `docker_run` pode ser usado, mas requer o encerramento e a reinicialização manual de `docker_run` quando os arquivos de configuração forem alterados.

>[!ENDTABS]

O `<aem-publish-host>` pode ser definido como `host.docker.internal`, um nome DNS especial que o Docker fornece no contêiner que é resolvido para o IP do computador host. Se o `host.docker.internal` não resolver, consulte a seção [solução de problemas](#troubleshooting-host-docker-internal) abaixo.

Por exemplo, para iniciar o contêiner do Dispatcher Docker usando os arquivos de configuração padrão fornecidos pelas Ferramentas do Dispatcher:

Inicie o contêiner do Dispatcher Docker fornecendo o caminho para a pasta src de configuração do Dispatcher:

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ ./bin/docker_run_hot_reload.sh ./src host.docker.internal:4503 8080
```

>[!TAB Windows]

```shell
$ bin\docker_run src host.docker.internal:4503 8080
```

>[!TAB Linux®]

```shell
$ ./bin/docker_run_hot_reload.sh ./src host.docker.internal:4503 8080
```

>[!ENDTABS]

O Serviço de Publicação do AEM as a Cloud Service SDK, executado localmente na porta 4503, está disponível por meio do Dispatcher em `http://localhost:8080`.

Para executar as Ferramentas do Dispatcher em uma configuração Dispatcher de projeto do Experience Manager, aponte para a pasta `dispatcher/src` do projeto.

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ ./bin/docker_run_hot_reload.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!TAB Windows]

```shell
$ bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!TAB Linux®]

```shell
$ ./bin/docker_run_hot_reload.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!ENDTABS]


## Logs de ferramentas do Dispatcher

Os logs do Dispatcher são úteis durante o desenvolvimento local para entender se e por que as solicitações HTTP são bloqueadas. O nível de log pode ser definido prefixando a execução de `docker_run` com parâmetros de ambiente.

Os logs das Ferramentas do Dispatcher são emitidos para o padrão quando `docker_run` é executado.

Parâmetros úteis para depuração do Dispatcher incluem:

+ `DISP_LOG_LEVEL=Debug` define o log do módulo Dispatcher para o nível de Depuração
   + O valor padrão é: `Warn`
+ `REWRITE_LOG_LEVEL=Debug` define o log do módulo de regravação do Apache HTTP Web server para o nível de Depuração
   + O valor padrão é: `Warn`
+ O `DISP_RUN_MODE` define o &quot;modo de execução&quot; do ambiente Dispatcher, carregando os arquivos de configuração Dispatcher dos modos de execução correspondentes.
   + O padrão é `dev`
+ Valores válidos: `dev`, `stage` ou `prod`

Um ou vários parâmetros, podem ser passados para `docker_run`

>[!BEGINTABS]

>[!TAB macOS]

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug ./bin/docker_run_hot_reload.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!TAB Windows]

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!TAB Linux®]

```shell
$ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug ./bin/docker_run_hot_reload.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
```

>[!ENDTABS]

### Acesso ao arquivo de log

O servidor Web Apache e os logs do AEM Dispatcher podem ser acessados diretamente no contêiner Docker:

+ [Acesso a logs no container do Docker](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-access-logs)
+ [Copiar os logs do Docker para o sistema de arquivos local](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-copy-logs)

## Quando atualizar as Ferramentas do Dispatcher{#dispatcher-tools-version}

As versões das Ferramentas do Dispatcher são incrementadas com menos frequência do que o Experience Manager. Portanto, as Ferramentas do Dispatcher exigem menos atualizações no ambiente de desenvolvimento local.

A versão recomendada das Ferramentas do Dispatcher é aquela fornecida com o AEM as a Cloud Service SDK que corresponde à versão do Experience Manager as a Cloud Service. A versão do AEM as a Cloud Service pode ser encontrada via [Cloud Manager](https://my.cloudmanager.adobe.com/).

+ __Cloud Manager > Ambientes__, por ambiente especificado pelo rótulo __Versão do AEM__

![Versão do Experience Manager](./assets/dispatcher-tools/aem-version.png)

*Observe que a versão do Dispatcher Tools não corresponde à versão do Experience Manager.*

## Como atualizar o conjunto de linhas de base de configurações do Apache e Dispatcher

O conjunto de linhas de base da configuração do Apache e do Dispatcher é aprimorado regularmente e lançado com a versão do AEM as a Cloud Service SDK. A prática recomendada é incorporar as melhorias na configuração da linha de base ao seu projeto do AEM e evitar [validação local](#validate-configurations) e falhas de pipeline do Cloud Manager. Atualize-os usando o script `update_maven.sh` da pasta `.../dispatcher-sdk-x.x.x/bin`.

>[!VIDEO](https://video.tv.adobe.com/v/33176?quality=12&learn=on&captions=por_br)

*Este vídeo usa o macOS para fins ilustrativos. Os comandos equivalentes do Windows/Linux podem ser usados para obter resultados semelhantes.*


Suponhamos que você tenha criado um projeto AEM no passado usando o [Arquétipo de Projetos AEM](https://github.com/adobe/aem-project-archetype), as configurações de linha de base do Apache e do Dispatcher eram atuais. Usando essas configurações de linha de base, as configurações específicas do seu projeto foram criadas com a reutilização e a cópia de arquivos como `*.vhost`, `*.conf`, `*.farm` e `*.any` das pastas `dispatcher/src/conf.d` e `dispatcher/src/conf.dispatcher.d`. A validação local do Dispatcher e os pipelines do Cloud Manager estavam funcionando bem.

Enquanto isso, as configurações de linha de base do Apache e Dispatcher foram aprimoradas por vários motivos, como novos recursos, correções de segurança e otimização. Eles são lançados com uma versão mais recente das Ferramentas do Dispatcher como parte da versão do AEM as a Cloud Service.

Agora, ao validar as configurações específicas do Dispatcher do projeto em relação à versão mais recente das Ferramentas do Dispatcher, elas começam a falhar. Para resolver isso, as configurações de linha de base precisam ser atualizadas usando as etapas abaixo:

+ Verifique se a validação está falhando em relação à versão mais recente das Ferramentas do Dispatcher

  ```shell
  $ ./bin/validate.sh ${YOUR-AEM-PROJECT}/dispatcher/src
  
  ...
  Phase 3: Immutability check
  empty mode param, assuming mode = 'check'
  ...
  ** error: immutable file 'conf.d/available_vhosts/default.vhost' has been changed!
  ```

+ Atualizar os arquivos imutáveis usando o script `update_maven.sh`

  ```shell
  $ ./bin/update_maven.sh ${YOUR-AEM-PROJECT}/dispatcher/src
  
  ...
  Updating dispatcher configuration at folder 
  running in 'extract' mode
  running in 'extract' mode
  reading immutable file list from /etc/httpd/immutable.files.txt
  preparing 'conf.d/available_vhosts/default.vhost' immutable file extraction
  ...
  immutable files extraction COMPLETE
  fd72f4521fa838daaaf006bb8c9c96ed33a142a2d63cc963ba4cc3dd228948fe
  Cloud manager validator 2.0.53
  ```

+ Verifique os arquivos imutáveis atualizados, como `dispatcher_vhost.conf`, `default.vhost` e `default.farm` e, se necessário, faça alterações relevantes em seus arquivos personalizados derivados desses arquivos.

+ Revalidar a configuração, ela deve passar

```shell
$ ./bin/validate.sh ${YOUR-AEM-PROJECT}/dispatcher/src

...
checking 'conf.dispatcher.d/renders/default_renders.any' immutability (if present)
checking existing 'conf.dispatcher.d/renders/default_renders.any' for changes
checking 'conf.dispatcher.d/virtualhosts/default_virtualhosts.any' immutability (if present)
checking existing 'conf.dispatcher.d/virtualhosts/default_virtualhosts.any' for changes
no immutable file has been changed - check is SUCCESSFUL
Phase 3 finished
```

+ Após a verificação local das alterações, confirme os arquivos de configuração atualizados

## Resolução de problemas

### docker_run resulta na mensagem &#39;Aguardando até que host.docker.internal esteja disponível&#39;{#troubleshooting-host-docker-internal}

O `host.docker.internal` é um nome de host fornecido para o Docker que é resolvido para o host. Por docs.docker.com ([macOS](https://docs.docker.com/desktop/networking/), [Windows](https://docs.docker.com/desktop/networking/)):

> A partir do Docker 18.03, a recomendação é se conectar ao nome DNS especial host.docker.internal, que é resolvido para o endereço IP interno usado pelo host

Quando `bin/docker_run src host.docker.internal:4503 8080` resulta na mensagem __Aguardando até que host.docker.internal esteja disponível__, então:

1. Verifique se a versão instalada do Docker é 18.03 ou superior
1. Talvez você tenha um computador local configurado que esteja impedindo o registro/resolução do nome `host.docker.internal`. Em vez disso, use o IP local.

>[!BEGINTABS]

>[!TAB macOS]

+ No Terminal, execute `ifconfig` e registre o endereço IP do Host __inet__, geralmente o dispositivo __en0__.
+ Em seguida, execute `docker_run` usando o endereço IP do host: `$ bin/docker_run_hot_reload.sh src <HOST IP>:4503 8080`

>[!TAB Windows]

+ No Prompt de Comando, execute `ipconfig` e registre o __Endereço IPv4__ do host do computador host.
+ Em seguida, execute `docker_run` usando este endereço IP: `$ bin\docker_run src <HOST IP>:4503 8080`

>[!TAB Linux®]

+ No Terminal, execute `ifconfig` e registre o endereço IP do Host __inet__, geralmente o dispositivo __en0__.
+ Em seguida, execute `docker_run` usando o endereço IP do host: `$ bin/docker_run_hot_reload.sh src <HOST IP>:4503 8080`

>[!ENDTABS]

#### Exemplo de erro

```shell
$ docker_run src host.docker.internal:4503 8080

Running script /docker_entrypoint.d/10-check-environment.sh
Running script /docker_entrypoint.d/20-create-docroots.sh
Running script /docker_entrypoint.d/30-wait-for-backend.sh
Waiting until host.docker.internal is available
```

## Recursos adicionais

+ [Baixar o AEM SDK](https://experience.adobe.com/#/downloads)
+ [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)
+ [Baixar Docker](https://www.docker.com/)
+ [Baixar o Site de Referência da AEM (WKND)](https://github.com/adobe/aem-guides-wknd/releases)
+ [Documentação do Experience Manager Dispatcher](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=pt-BR)
