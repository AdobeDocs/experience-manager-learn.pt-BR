---
title: Configurar ferramentas do Dispatcher para AEM como um desenvolvimento de Cloud Service
description: AEM Ferramentas do Dispatcher do SDK facilita o desenvolvimento local de projetos do Adobe Experience Manager (AEM), facilitando a instalação, execução e solução de problemas do Dispatcher localmente.
sub-product: fundação
feature: dispatcher
topics: development, caching, security
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4679
thumbnail: 30603.jpg
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '1508'
ht-degree: 2%

---


# Configurar ferramentas locais do Dispatcher

O Dispatcher da Adobe Experience Manager (AEM) é um módulo de servidor da Web Apache HTTP que fornece uma camada de segurança e desempenho entre a camada CDN e a camada de publicação do AEM. O Dispatcher é parte integrante da arquitetura de Experience Manager geral e deve fazer parte do desenvolvimento local criado.

O AEM como um SDK do Cloud Service inclui a versão recomendada das Ferramentas do Dispatcher, que facilita a configuração, validação e simulação do Dispatcher localmente. As Ferramentas do Dispatcher são compostas de:

+ um conjunto básico de arquivos de configuração do Apache HTTP Web Server e Dispatcher, localizados em `.../dispatcher-sdk-x.x.x/src`
+ uma ferramenta CLI do validador de configuração, localizada em `.../dispatcher-sdk-x.x.x/bin/validator`
+ uma ferramenta CLI de implantação de configuração, localizada em `.../dispatcher-sdk-x.x.x/bin/docker_run`
+ uma imagem Docker que executa o servidor Web Apache HTTP com o módulo Dispatcher

Observe que `~` é usado como abreviação para o Diretório do usuário. No Windows, isso é o equivalente a `%HOMEPATH%`.

>[!NOTE]
>
> Os vídeos desta página foram gravados no macOS. Os usuários do Windows podem acompanhar, mas usar os comandos equivalentes do Dispatcher Tools Windows, fornecidos com cada vídeo.

## Pré-requisitos

1. Os usuários do Windows devem usar o Windows 10 Professional
1. Instale o [Experience Manager Publish QuickStart](./aem-runtime.md) na máquina de desenvolvimento local.
   + Opcionalmente, instale o site [de referência mais recente](https://github.com/adobe/aem-guides-wknd/releases) AEM no serviço de publicação de AEM local. Este site é usado neste tutorial para visualizar um Dispatcher em funcionamento.
1. Instale e start a versão mais recente do [Docker](https://www.docker.com/) (Docker Desktop 2.2.0.5+ / Docker Engine v19.03.9+) no computador de desenvolvimento local.

## Baixe as ferramentas do Dispatcher (como parte do SDK AEM)

O AEM como Cloud Service SDK, ou AEM SDK, contém as Ferramentas do Dispatcher usadas para executar o servidor Web HTTP Apache com o módulo Dispatcher localmente para desenvolvimento, bem como o QuickStart Jar compatível.

Se o AEM como um SDK Cloud Service já tiver sido baixado para [configurar o tempo de execução](./aem-runtime.md)AEM local, ele não precisará ser baixado novamente.

1. Faça logon em [experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads) com seu Adobe ID
   + Observe que sua Organização Adobe __deve__ ser provisionada para AEM como Cloud Service para baixar o AEM como um SDK Cloud Service.
1. Navegue até a __AEM como uma guia Cloud Service__
1. Classificar por data __de__ publicação em ordem __decrescente__
1. Clique na linha de resultado mais recente do __AEM SDK__
1. Revise e aceite o EULA e toque no botão __Download__
1. Verifique se AEM as ferramentas do Dispatcher v2.0.21+ do SDK foram usadas

## Extraia as ferramentas do Dispatcher do zip do SDK AEM

>[!TIP]
>
> Os usuários do Windows não podem ter espaços ou caracteres especiais no caminho para a pasta que contém as Ferramentas do Dispatcher Local. Se houver espaços no caminho, o `docker_run.cmd` erro ocorrerá.

A versão das Ferramentas do Dispatcher é diferente da do SDK AEM. Verifique se a versão das Ferramentas do Dispatcher é fornecida pela versão do SDK AEM que corresponde ao AEM como uma versão do Cloud Service.

1. Descompacte o arquivo baixado `aem-sdk-xxx.zip`
1. Desempacotar as Ferramentas do Dispatcher em `~/aem-sdk/dispatcher`
   + Windows: Descompactar `aem-sdk-dispatcher-tools-x.x.x-windows.zip` em `C:\Users\<My User>\aem-sdk\dispatcher` (criando pastas ausentes, conforme necessário)
   + macOS / Linux: Execute o script de shell associado `aem-sdk-dispatcher-tools-x.x.x-unix.sh` para desempacotar as Ferramentas do Dispatcher
      + `chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh && ./aem-sdk-dispatcher-tools-x.x.x-unix.sh`

Observe que todos os comandos emitidos abaixo assumem que o diretório de trabalho atual contém o conteúdo expandindo as Ferramentas do Dispatcher.

>[!VIDEO](https://video.tv.adobe.com/v/30601/?quality=12&learn=on)
*Este vídeo usa o macOS para fins ilustrativos. Os comandos Windows/Linux equivalentes podem ser usados para obter resultados semelhantes*

## Entenda os arquivos de configuração do Dispatcher

>[!TIP]
> Os projetos de Experience Manager criados a partir do [AEM Project Maven Archetype](https://github.com/adobe/aem-project-archetype) são pré-preenchidos nesse conjunto de arquivos de configuração do Dispatcher, portanto não há necessidade de copiar da pasta src das Ferramentas do Dispatcher.

As Ferramentas do Dispatcher fornecem um conjunto de arquivos de configuração do Apache HTTP Web Server e do Dispatcher que definem o comportamento para todos os ambientes, incluindo o desenvolvimento local.

Esses arquivos devem ser copiados em um projeto Experience Manager Maven para a `dispatcher/src` pasta, caso ainda não existam no projeto Experience Manager Maven.

>[!VIDEO](https://video.tv.adobe.com/v/30602/?quality=12&learn=on)
*Este vídeo usa o macOS para fins ilustrativos. Os comandos Windows/Linux equivalentes podem ser usados para obter resultados semelhantes*

Uma descrição completa dos arquivos de configuração está disponível nas Ferramentas do Dispatcher descompactadas como `dispatcher-sdk-x.x.x/docs/Config.html`.

## Executar Dispatcher localmente

Para executar o Dispatcher localmente, os arquivos de configuração do Dispatcher a serem usados para configurá-lo devem ser validados com a ferramenta CLI `validator` das Ferramentas do Dispatcher.

+ Uso:
   + Windows: `bin\validator full -d out src`
   + macOS / Linux: `./bin/validator full -d ./out ./src`

A validação tem dupla finalidade:

+ Valida os arquivos de configuração do servidor Web Apache HTTP e do Dispatcher para corrigir
+ Transmite as configurações em um conjunto de arquivos compatível com o Pacote de container do Docker HTTP Web Server.

Depois de validadas, as configurações transpiladas são usadas para executar o Dispatcher localmente no container Docker. É importante garantir que as configurações mais recentes tenham sido validadas __e__ exibidas usando a `-d` opção do validador.

+ Uso:
   + Windows: `bin\docker_run <deployment-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`
   + macOS / Linux: `./bin/docker_run.sh <deployment-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`

O `aem-publish-host` pode ser definido como `host.docker.internal`, um nome DNS especial fornecido pelo Docker no container que resolve o IP da máquina host. Se ele `host.docker.internal` não resolver, consulte a seção [solução de problemas](#troubleshooting-host-docker-internal) abaixo.

Por exemplo, para start do container Dispatcher Docker usando os arquivos de configuração padrão fornecidos pelas Ferramentas do Dispatcher:

1. Gere o `deployment-folder`, nomeado `out` por convenção, do zero sempre que uma configuração for alterada:

   + Windows: `del /Q out && bin\validator full -d out src`
   + macOS / Linux: `rm -rf ./out && ./bin/validator full -d ./out ./src`

2. (Re-)container Dispatcher Docker do start que fornece o caminho para a pasta de implantação:

   + Windows: `bin\docker_run out host.docker.internal:4503 8080`
   + macOS / Linux: `./bin/docker_run.sh ./out host.docker.internal:4503 8080`

O AEM como um serviço de publicação do SDK do Cloud Service, executado localmente na porta 4503, estará disponível por meio do Dispatcher em `http://localhost:8080`.

Para executar as Ferramentas do Dispatcher em relação à configuração do Dispatcher de um projeto do Experience Manager, gere simplesmente o `deployment-folder` usando a `dispatcher/src` pasta do projeto.

+ Windows:

   ```shell
   $ del -/Q out && bin\validator full -d out <User Directory>/code/my-project/dispatcher/src
   $ bin\docker_run out host.docker.internal:4503 8080
   ```

+ macOS / Linux:

   ```shell
   $ rm -rf ./out && ./bin/validator full -d ./out ~/code/my-project/dispatcher/src
   $ ./bin/docker_run.sh ./out host.docker.internal:4503 8080
   ```

>[!VIDEO](https://video.tv.adobe.com/v/30603/?quality=12&learn=on)
*Este vídeo usa o macOS para fins ilustrativos. Os comandos Windows/Linux equivalentes podem ser usados para obter resultados semelhantes*

## Logs de ferramentas do Dispatcher

Os registros do Dispatcher são úteis durante o desenvolvimento local para entender se e por que as Solicitações HTTP estão bloqueadas. O nível de log pode ser definido prefixando a execução de `docker_run` com parâmetros de ambiente.

Os logs de Ferramentas do Dispatcher são emitidos para o padrão de saída quando `docker_run` é executado.

Parâmetros úteis para depurar o Dispatcher incluem:

+ `DISP_LOG_LEVEL=Debug` define o registro do módulo do Dispatcher como nível de Depuração
   + O valor padrão é: `Warn`
+ `REWRITE_LOG_LEVEL=Debug` define o registro do módulo de regravação do servidor Web Apache HTTP para o nível de Depuração
   + O valor padrão é: `Warn`
+ `DISP_RUN_MODE` define o &quot;modo de execução&quot; do ambiente Dispatcher, carregando os modos de execução correspondentes dos arquivos de configuração do Dispatcher.
   + O padrão é `dev`
+ Valores válidos: `dev`, `stage`ou `prod`

Um ou vários parâmetros podem ser transmitidos para `docker_run`

+ Windows:

   ```shell
   $ bin\validator full -d out <User Directory>/code/my-project/dispatcher/src
   $ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug bin\docker_run out host.docker.internal:4503 8080
   ```

+ macOS / Linux:

   ```shell
   $ ./bin/validator full -d out ~/code/my-project/dispatcher/src
   $ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug ./bin/docker_run.sh out host.docker.internal:4503 8080
   ```

>[!VIDEO](https://video.tv.adobe.com/v/30604/?quality=12&learn=on)
*Este vídeo usa o macOS para fins ilustrativos. Os comandos Windows/Linux equivalentes podem ser usados para obter resultados semelhantes*

## Quando atualizar as Ferramentas do Dispatcher{#dispatcher-tools-version}

As versões das Ferramentas do Dispatcher aumentam com menos frequência do que o Experience Manager e, portanto, as Ferramentas do Dispatcher exigem menos atualizações no ambiente de desenvolvimento local.

A versão recomendada das Ferramentas do Dispatcher é aquela que é fornecida com o AEM como um SDK Cloud Service que corresponde ao Experience Manager como uma versão Cloud Service. A versão do AEM como Cloud Service pode ser encontrada pelo [Cloud Manager](https://my.cloudmanager.adobe.com/).

+ __Gerenciador de nuvem > Ambientes__, por ambiente especificado pelo rótulo __AEM versão__

![Versão Experience Manager](./assets/dispatcher-tools/aem-version.png)

_Observe que a própria versão das Ferramentas do Dispatcher não corresponderá à versão do Experience Manager._

## Resolução de problemas

### docker_run resulta na mensagem &#39;Aguardando até que host.docker.internal esteja disponível&#39;{#troubleshooting-host-docker-internal}

`host.docker.internal` é um nome de host fornecido ao docking station que é resolvido para o host. Por docs.docker.com ([macOS](https://docs.docker.com/docker-for-mac/networking/#i-want-to-connect-from-a-container-to-a-service-on-the-host), [Windows](https://docs.docker.com/docker-for-windows/networking/)):

> A partir do Docker 18.03, nossa recomendação é conectar-se ao nome DNS especial host.docker.internal, que é resolvido para o endereço IP interno usado pelo host

Se, quando `bin/docker_run out host.docker.internal:4503 8080` resultar na mensagem __Aguardando até que host.docker.internal esteja disponível__, então:

1. Verifique se a versão instalada do Docker é 18.03 ou superior
2. Você pode ter uma configuração de máquina local que esteja impedindo o registro/resolução do `host.docker.internal` nome. Em vez disso, use seu IP local.
   + Windows:
      + No prompt de comando, execute `ipconfig`e registre o endereço ____ IPv4 do host do computador host.
      + Em seguida, execute `docker_run` usando este endereço IP:
         `bin\docker_run out <HOST IP>:4503 8080`
   + macOS / Linux:
      + No terminal, execute `ifconfig` e registre o endereço IP __indefinido__ do host, geralmente o dispositivo __en0__ .
      + Em seguida, execute `docker_run` usando o endereço IP do host:
         `bin/docker_run.sh out <HOST IP>:4503 8080`

#### Exemplo de erro

```shell
$ docker_run out host.docker.internal:4503 8080

Running script /docker_entrypoint.d/10-check-environment.sh
Running script /docker_entrypoint.d/20-create-docroots.sh
Running script /docker_entrypoint.d/30-wait-for-backend.sh
Waiting until host.docker.internal is available
```

### docker_run resulta no erro &#39;**: Pasta de implantação não encontrada&#39;

Durante a execução `docker_run.cmd`, é exibido um erro que indica __** erro: Pasta de implantação não encontrada:__. Isso normalmente ocorre porque há espaços no caminho. Se possível, remova os espaços na pasta ou mova a `aem-sdk` pasta para um caminho que não contenha espaços.

Por exemplo, as pastas de usuário do Windows geralmente são `<First name> <Last name>`as pastas, com um espaço entre elas. No exemplo abaixo, a pasta `...\My User\...` contém um espaço que quebra a execução local das Ferramentas do Dispatcher `docker_run` . Se os espaços estiverem em uma pasta de usuário do Windows, não tente renomear essa pasta, pois ela quebrará o Windows, em vez disso, mova a `aem-sdk` pasta para um novo local onde seu usuário tenha permissões para modificar totalmente. Observe que as instruções que presumem que a `aem-sdk` pasta está no diretório inicial do usuário precisarão ser ajustadas ao novo local.

#### Exemplo de erro

```shell
$ \Users\My User\aem-sdk\dispatcher>bin\docker_run.cmd out host.internal.docker:4503 8080

'User\aem-sdk\dispatcher\out\*' is not recognized as an internal or external command,
operable program or batch file.
** error: Deployment folder not found: c:\Users\My User\aem-sdk\dispatcher\out
```

### docker_run falha ao start no Windows{#troubleshooting-windows-compatible}

A execução `docker_run` no Windows pode resultar no seguinte erro, impedindo o Dispatcher de iniciar. Esse é um problema reportado com o Dispatcher no Windows e será corrigido em uma versão futura.

#### Exemplo de erro

```shell
$ \Users\MyUser\aem-sdk\dispatcher>bin\docker_run out host.docker.internal:4503 8080

Running script /docker_entrypoint.d/10-check-environment.sh
Running script /docker_entrypoint.d/20-create-docroots.sh
Running script /docker_entrypoint.d/30-wait-for-backend.sh
Waiting until host.docker.internal is available
host.docker.internal resolves to 192.168.65.2
Running script /docker_entrypoint.d/40-generate-allowed-clients.sh
Running script /docker_entrypoint.d/50-check-expiration.sh
Running script /docker_entrypoint.d/60-check-loglevel.sh
Running script /docker_entrypoint.d/70-check-forwarded-host-secret.sh
Starting httpd server
[Sun Feb 09 17:32:22.256020 2020] [dispatcher:warn] [pid 1:tid 140080096570248] Unable to obtain parent directory of /etc/httpd/conf.dispatcher.d/enabled_farms/farms.any: No such file or directory
[Sun Feb 09 17:32:22.256069 2020] [dispatcher:alert] [pid 1:tid 140080096570248] Unable to import config file: /etc/httpd/conf.dispatcher.d/dispatcher.any
[Sun Feb 09 17:32:22.256074 2020] [dispatcher:alert] [pid 1:tid 140080096570248] Dispatcher initialization failed.
AH00016: Configuration Failed
```

## Recursos adicionais

+ [Baixar AEM SDK](https://experience.adobe.com/#/downloads)
+ [Gerenciador da Adobe Cloud](https://my.cloudmanager.adobe.com/)
+ [Baixar o Docker](https://www.docker.com/)
+ [Download do site de referência AEM (WKND)](https://github.com/adobe/aem-guides-wknd/releases)
+ [Documentação do Dispatcher Experience Manager](https://docs.adobe.com/content/help/pt-BR/experience-manager-dispatcher/using/dispatcher.html)
