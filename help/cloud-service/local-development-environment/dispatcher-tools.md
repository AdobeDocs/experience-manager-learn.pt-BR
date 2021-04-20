---
title: Configurar ferramentas do Dispatcher para desenvolvimento do AEM as a Cloud Service
description: As Ferramentas do Dispatcher do AEM SDK facilitam o desenvolvimento local de projetos do Adobe Experience Manager (AEM), facilitando a instalação, a execução e a solução de problemas do Dispatcher localmente.
sub-product: fundação
feature: Dispatcher, Developer Tools
topics: development, caching, security
version: cloud-service
doc-type: tutorial
activity: develop
audience: developer
kt: 4679
thumbnail: 30603.jpg
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1572'
ht-degree: 2%

---


# Configurar ferramentas locais do Dispatcher

O Dispatcher do Adobe Experience Manager (AEM) é um módulo de servidor Web Apache HTTP que fornece uma camada de segurança e desempenho entre a camada CDN e AEM Publish. O Dispatcher é parte integrante da arquitetura geral do Experience Manager e deve fazer parte da configuração de desenvolvimento local.

O SDK do AEM as a Cloud Service inclui a versão recomendada das Ferramentas do Dispatcher, que facilita a configuração, validação e simulação do Dispatcher localmente. As Ferramentas do Dispatcher são compostas por:

+ um conjunto de linhas de base do servidor Web Apache HTTP e arquivos de configuração do Dispatcher, localizados em `.../dispatcher-sdk-x.x.x/src`
+ uma ferramenta CLI do validador de configuração, localizada em `.../dispatcher-sdk-x.x.x/bin/validate` (Dispatcher SDK 2.0.29+)
+ uma ferramenta CLI de geração de configuração, localizada em `.../dispatcher-sdk-x.x.x/bin/validator`
+ uma ferramenta CLI de implantação de configuração, localizada em `.../dispatcher-sdk-x.x.x/bin/docker_run`
+ uma imagem Docker que executa o servidor Web Apache HTTP com o módulo Dispatcher

Observe que `~` é usado como abreviado para o Diretório do usuário. No Windows, este é o equivalente a `%HOMEPATH%`.

>[!NOTE]
>
> Os vídeos desta página foram gravados no macOS. Os usuários do Windows podem seguir, mas usam os comandos do Dispatcher Tools Windows equivalentes, fornecidos com cada vídeo.

## Pré-requisitos

1. Os usuários do Windows devem usar o Windows 10 Professional
1. Instale o [Experience Manager Publish Quickstart Jar](./aem-runtime.md) na máquina de desenvolvimento local.
   + Opcionalmente, instale o [site da Web de referência do AEM](https://github.com/adobe/aem-guides-wknd/releases) no serviço de publicação do AEM local. Este site é usado neste tutorial para visualizar um Dispatcher em funcionamento.
1. Instale e inicie a versão mais recente de [Docker](https://www.docker.com/) (Docker Desktop 2.2.0.5+ / Docker Engine v19.03.9+) na máquina de desenvolvimento local.

## Baixar as Ferramentas do Dispatcher (como parte do SDK do AEM)

O SDK do AEM as a Cloud Service ou o SDK do AEM contém as Ferramentas do Dispatcher usadas para executar o servidor Web Apache HTTP com o módulo Dispatcher localmente para desenvolvimento, bem como o Jar do QuickStart compatível.

Se o SDK do AEM as a Cloud Service já tiver sido baixado para [configurar o tempo de execução local do AEM](./aem-runtime.md), ele não precisará ser baixado novamente.

1. Faça logon em [experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=&amp;p.offset=0&amp;p.limit=1) com sua Adobe ID
   + Sua organização da Adobe __deve__ ser provisionada para o AEM as a Cloud Service baixar o SDK do AEM as a Cloud Service
1. Clique na linha de resultados mais recente __AEM SDK__ para baixar
   + Verifique se as Ferramentas do Dispatcher v2.0.29+ do SDK do AEM estão anotadas na descrição de download

## Extraia as Ferramentas do Dispatcher do zip do SDK do AEM

>[!TIP]
>
> Os usuários do Windows não podem ter espaços ou caracteres especiais no caminho para a pasta que contém as Ferramentas do Dispatcher Locais. Se houver espaços no caminho, o `docker_run.cmd` falhará.

A versão das Ferramentas do Dispatcher é diferente da do SDK do AEM. Certifique-se de que a versão das Ferramentas do Dispatcher seja fornecida por meio da versão do SDK do AEM correspondente à versão do AEM as a Cloud Service.

1. Descompacte o arquivo `aem-sdk-xxx.zip` baixado
1. Descompacte as Ferramentas do Dispatcher em `~/aem-sdk/dispatcher`
   + Windows: Descompacte `aem-sdk-dispatcher-tools-x.x.x-windows.zip` em `C:\Users\<My User>\aem-sdk\dispatcher` (criando pastas ausentes, conforme necessário)
   + macOS / Linux: Execute o script de shell associado `aem-sdk-dispatcher-tools-x.x.x-unix.sh` para descompactar as Ferramentas do Dispatcher
      + `chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh && ./aem-sdk-dispatcher-tools-x.x.x-unix.sh`

Observe que todos os comandos emitidos abaixo pressupõem que o diretório de trabalho atual contém o conteúdo expandindo as Ferramentas do Dispatcher.

>[!VIDEO](https://video.tv.adobe.com/v/30601/?quality=12&learn=on)

*Este vídeo usa o macOS para fins ilustrativos. Os comandos equivalentes do Windows/Linux podem ser usados para obter resultados semelhantes*

## Entender os arquivos de configuração do Dispatcher

>[!TIP]
> Os projetos do Experience Manager criados a partir do [AEM Project Maven Archetype](https://github.com/adobe/aem-project-archetype) são preenchidos previamente com esse conjunto de arquivos de configuração do Dispatcher, portanto, não há necessidade de copiar da pasta src Ferramentas do Dispatcher.

As Ferramentas do Dispatcher fornecem um conjunto de arquivos de configuração Apache HTTP Web Server e Dispatcher que definem o comportamento para todos os ambientes, incluindo o desenvolvimento local.

Esses arquivos devem ser copiados em um projeto Maven do Experience Manager para a pasta `dispatcher/src`, se ainda não existirem no projeto Maven do Experience Manager.

>[!VIDEO](https://video.tv.adobe.com/v/30602/?quality=12&learn=on)

*Este vídeo usa o macOS para fins ilustrativos. Os comandos equivalentes do Windows/Linux podem ser usados para obter resultados semelhantes*

Uma descrição completa dos arquivos de configuração está disponível nas Ferramentas do Dispatcher descompactadas como `dispatcher-sdk-x.x.x/docs/Config.html`.

## Validar configurações

Opcionalmente, as configurações do Dispatcher e do Apache Web Server (via `httpd -t`) podem ser validadas usando o script `validate` (não confundir com o `validator` executável).

+ Uso:
   + Windows: `bin\validate src`
   + macOS / Linux: `./bin/validate.sh ./src`

## Executar o Dispatcher localmente

Para executar o Dispatcher localmente, os arquivos de configuração do Dispatcher devem ser gerados usando a ferramenta CLI `validator` das Ferramentas do Dispatcher.

+ Uso:
   + Windows: `bin\validator full -d out src`
   + macOS / Linux: `./bin/validator full -d ./out ./src`

Este comando transpila as configurações em um conjunto de arquivos compatível com o Apache HTTP Web Server do contêiner Docker.

Depois de geradas, as configurações transpiladas são usadas para executar o Dispatcher localmente no container Docker. É importante garantir que as configurações mais recentes tenham sido validadas usando a saída `validate` __e__ usando a opção `-d` do validador.

+ Uso:
   + Windows: `bin\docker_run <deployment-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`
   + macOS / Linux: `./bin/docker_run.sh <deployment-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`

O `aem-publish-host` pode ser definido como `host.docker.internal`, um nome DNS especial fornecido pelo Docker no container que resolve o IP da máquina host. Se `host.docker.internal` não for resolvido, consulte a seção [solução de problemas](#troubleshooting-host-docker-internal) abaixo.

Por exemplo, para iniciar o contêiner do Dispatcher Docker usando os arquivos de configuração padrão fornecidos pelas Ferramentas do Dispatcher:

1. Gere o `deployment-folder`, chamado `out` por convenção, do zero sempre que uma configuração for alterada:

   + Windows: `del /Q out && bin\validator full -d out src`
   + macOS / Linux: `rm -rf ./out && ./bin/validator full -d ./out ./src`

2. (Re-)Inicie o contêiner do Dispatcher Docker fornecendo o caminho para a pasta de implantação:

   + Windows: `bin\docker_run out host.docker.internal:4503 8080`
   + macOS / Linux: `./bin/docker_run.sh ./out host.docker.internal:4503 8080`

O Serviço de publicação do SDK do AEM as a Cloud Service, executado localmente na porta 4503, estará disponível por meio do Dispatcher em `http://localhost:8080`.

Para executar as Ferramentas do Dispatcher em relação à configuração do Dispatcher de um projeto do Experience Manager, basta gerar o `deployment-folder` usando a pasta `dispatcher/src` do projeto.

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

*Este vídeo usa o macOS para fins ilustrativos. Os comandos equivalentes do Windows/Linux podem ser usados para obter resultados semelhantes*

## Logs de ferramentas do Dispatcher

Os logs do Dispatcher são úteis durante o desenvolvimento local para entender se e por que as Solicitações HTTP são bloqueadas. O nível de log pode ser definido com o prefixo da execução de `docker_run` com parâmetros de ambiente.

Os logs de Ferramentas do Dispatcher são emitidos para o padrão quando `docker_run` é executado.

Os parâmetros úteis para depurar o Dispatcher incluem:

+ `DISP_LOG_LEVEL=Debug` define o log do módulo Dispatcher para o nível de Depuração
   + O valor padrão é: `Warn`
+ `REWRITE_LOG_LEVEL=Debug` define o registro do módulo de reescrita do servidor Web Apache HTTP para o nível de Depuração
   + O valor padrão é: `Warn`
+ `DISP_RUN_MODE` define o &quot;modo de execução&quot; do ambiente Dispatcher, carregando os modos de execução correspondentes dos arquivos de configuração do Dispatcher.
   + O padrão é `dev`
+ Valores válidos: `dev`, `stage` ou `prod`

Um ou vários parâmetros podem ser passados para `docker_run`

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

*Este vídeo usa o macOS para fins ilustrativos. Os comandos equivalentes do Windows/Linux podem ser usados para obter resultados semelhantes*

### Acesso ao arquivo de log

Os logs do servidor da Web Apache e do AEM Dispatcher podem ser acessados diretamente no container Docker:

+ [Acessar logs no container Docker](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-access-logs)
+ [Copiando os logs do Docker para o sistema de arquivos local](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-copy-logs)

## Quando atualizar as Ferramentas do Dispatcher{#dispatcher-tools-version}

As versões das Ferramentas do Dispatcher aumentam com menos frequência do que o Experience Manager e, portanto, as Ferramentas do Dispatcher exigem menos atualizações no ambiente de desenvolvimento local.

A versão recomendada das Ferramentas do Dispatcher é aquela que é fornecida com o SDK do AEM as a Cloud Service que corresponde à versão do Experience Manager as a Cloud Service. A versão do AEM as a Cloud Service pode ser encontrada em [Cloud Manager](https://my.cloudmanager.adobe.com/).

+ __Cloud Manager > Ambientes__, por ambiente especificado pelo  __AEM__ Release

![Versão do Experience Manager](./assets/dispatcher-tools/aem-version.png)

_Observe que a própria versão das Ferramentas do Dispatcher não corresponderá à versão do Experience Manager._

## Resolução de problemas

### docker_run resulta na mensagem &#39;Aguardando até que host.docker.internal esteja disponível&#39;{#troubleshooting-host-docker-internal}

`host.docker.internal` é um nome de host fornecido ao Docker que é resolvido para o host. De acordo com docs.docker.com ([macOS](https://docs.docker.com/docker-for-mac/networking/#i-want-to-connect-from-a-container-to-a-service-on-the-host), [Windows](https://docs.docker.com/docker-for-windows/networking/)):

> A partir do Docker 18.03, nossa recomendação é se conectar ao nome DNS especial host.docker.internal, que resolve para o endereço IP interno usado pelo host

Se, quando `bin/docker_run out host.docker.internal:4503 8080` resultar na mensagem __Aguardando até que host.docker.internal esteja disponível__, então:

1. Verifique se a versão instalada do Docker é 18.03 ou superior
2. Você pode ter uma máquina local configurada que está impedindo o registro/a resolução do nome `host.docker.internal`. Em vez disso, use seu IP local.
   + Windows:
      + No Prompt de Comando, execute `ipconfig` e registre o __Endereço IPv4__ do host.
      + Em seguida, execute `docker_run` usando este endereço IP:
         `bin\docker_run out <HOST IP>:4503 8080`
   + macOS / Linux:
      + No Terminal, execute `ifconfig` e registre o endereço IP do Host __inet__, geralmente o dispositivo __en0__.
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

Ao executar `docker_run.cmd`, é exibido um erro que lê __** erro: Pasta de implantação não encontrada:__. Isso normalmente ocorre porque há espaços no caminho. Se possível, remova os espaços na pasta ou mova a pasta `aem-sdk` para um caminho que não contenha espaços.

Por exemplo, as pastas de usuário do Windows geralmente são `<First name> <Last name>`, com um espaço entre elas. No exemplo abaixo, a pasta `...\My User\...` contém um espaço que quebra a execução local das Ferramentas do Dispatcher `docker_run`. Se os espaços estiverem em uma pasta de usuário do Windows, não tente renomear essa pasta, pois ela quebrará o Windows, em vez disso, mova a pasta `aem-sdk` para um novo local que o usuário tenha permissões para modificar totalmente. Observe que as instruções que consideram que a pasta `aem-sdk` está no diretório base do usuário precisarão ser ajustadas para o novo local.

#### Exemplo de erro

```shell
$ \Users\My User\aem-sdk\dispatcher>bin\docker_run.cmd out host.internal.docker:4503 8080

'User\aem-sdk\dispatcher\out\*' is not recognized as an internal or external command,
operable program or batch file.
** error: Deployment folder not found: c:\Users\My User\aem-sdk\dispatcher\out
```

### docker_run falha ao iniciar no Windows{#troubleshooting-windows-compatible}

A execução de `docker_run` no Windows pode resultar no seguinte erro, impedindo que o Dispatcher seja iniciado. Esse é um problema reportado com o Dispatcher no Windows e será corrigido em uma versão futura.

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

+ [Baixar o AEM SDK](https://experience.adobe.com/#/downloads)
+ [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)
+ [Baixar Docker](https://www.docker.com/)
+ [Baixe o site de referência do AEM (WKND)](https://github.com/adobe/aem-guides-wknd/releases)
+ [Documentação do Dispatcher do Experience Manager](https://docs.adobe.com/content/help/pt-BR/experience-manager-dispatcher/using/dispatcher.html)
