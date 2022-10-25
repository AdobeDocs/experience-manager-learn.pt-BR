---
title: Configurar ferramentas do Dispatcher para AEM desenvolvimento as a Cloud Service
description: AEM ferramentas do Dispatcher do SDK facilita o desenvolvimento local de projetos do Adobe Experience Manager (AEM), facilitando a instalação, a execução e a solução de problemas do Dispatcher localmente.
version: Cloud Service
topic: Development
feature: Dispatcher, Developer Tools
role: Developer
level: Beginner
kt: 4679
thumbnail: 30603.jpg
last-substantial-update: 2022-09-01T00:00:00Z
exl-id: 9320e07f-be5c-42dc-a4e3-aab80089c8f7
source-git-commit: d0b13fd37f1ed42042431246f755a913b56625ec
workflow-type: tm+mt
source-wordcount: '1340'
ht-degree: 3%

---

# Configurar ferramentas locais do Dispatcher {#set-up-local-dispatcher-tools}

>[!CONTEXTUALHELP]
>id="aemcloud_localdev_dispatcher"
>title="Ferramentas locais do Dispatcher"
>abstract="O Dispatcher é parte integrante da arquitetura de Experience Manager geral e deve fazer parte da configuração de desenvolvimento local. O AEM as a Cloud Service SDK inclui a versão recomendada das Ferramentas do Dispatcher, que facilita a configuração, validação e simulação do Dispatcher localmente."
>additional-url="https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/disp-overview.html" text="Dispatcher na nuvem"
>additional-url="https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html" text="Baixar AEM SDK as a Cloud Service"

O Dispatcher do Adobe Experience Manager (AEM) é um módulo de servidor Web Apache HTTP que fornece uma camada de segurança e desempenho entre a camada CDN e AEM Publish. O Dispatcher é parte integrante da arquitetura de Experience Manager geral e deve fazer parte da configuração de desenvolvimento local.

O AEM as a Cloud Service SDK inclui a versão recomendada das Ferramentas do Dispatcher, que facilita a configuração, validação e simulação do Dispatcher localmente. As Ferramentas do Dispatcher são compostas por:

+ um conjunto de linhas de base do servidor Web Apache HTTP e arquivos de configuração do Dispatcher, localizado em `.../dispatcher-sdk-x.x.x/src`
+ uma ferramenta CLI do validador de configuração, localizada em `.../dispatcher-sdk-x.x.x/bin/validate`
+ uma ferramenta CLI de geração de configuração, localizada em `.../dispatcher-sdk-x.x.x/bin/validator`
+ uma ferramenta CLI de implantação de configuração, localizada em `.../dispatcher-sdk-x.x.x/bin/docker_run`
+ uma imagem Docker que executa o servidor Web Apache HTTP com o módulo Dispatcher

Observe que `~` é usado como abreviado para o Diretório do usuário. No Windows, isso é equivalente a `%HOMEPATH%`.

>[!NOTE]
>
> Os vídeos desta página foram gravados no macOS. Os usuários do Windows podem seguir, mas usam os comandos do Dispatcher Tools Windows equivalentes, fornecidos com cada vídeo.

## Pré-requisitos

1. Os usuários do Windows devem usar o Windows 10 Professional (ou uma versão compatível com Docker)
1. Instalar [Experience Manager Publish Quickstart Jar](./aem-runtime.md) na máquina de desenvolvimento local.
   + Opcionalmente, instale o mais recente [Site de referência do AEM](https://github.com/adobe/aem-guides-wknd/releases) no serviço local de publicação do AEM. Este site é usado neste tutorial para visualizar um Dispatcher em funcionamento.
1. Instale e inicie a versão mais recente de [Docker](https://www.docker.com/) (Docker Desktop 2.2.0.5+ / Docker Engine v19.03.9+) na máquina de desenvolvimento local.

## Baixar as Ferramentas do Dispatcher (como parte do SDK do AEM)

O AEM SDK as a Cloud Service, ou SDK AEM, contém as Ferramentas do Dispatcher usadas para executar o servidor Web Apache HTTP com o módulo Dispatcher localmente para desenvolvimento, bem como o QuickStart Jar compatível.

Se o SDK as a Cloud Service AEM já tiver sido baixado para [configurar o tempo de execução do AEM local](./aem-runtime.md), ele não precisa ser baixado novamente.

1. Faça logon em [experience.adobe.com/#/downloads](https://experience.adobe.com/#/downloads/content/software-distribution/en/aemcloud.html?fulltext=AEM*+SDK*&amp;1_group.propertyvalues.property=.%2Fjcr%3Acontent%2Fmetadata%2Fdc%3AsoftwareType&amp;1_group.propertyvalues.operation=equals&amp;1_group.propertyvalues.0_values=software-type%3Atooling&amp;orderby=%40jcr%3Acontent%2Fjcr%3AlastModified&amp;orderby.sort=desc&amp;layout=&amp;p.offset=0&amp;p.limit=1) com sua Adobe ID
   + Sua organização do Adobe __must__ ser provisionado para AEM as a Cloud Service para baixar o SDK as a Cloud Service AEM
1. Clique no mais recente __AEM SDK__ linha de resultado para download

## Extraia as Ferramentas do Dispatcher do zip AEM SDK

>[!TIP]
>
> Os usuários do Windows não podem ter espaços ou caracteres especiais no caminho para a pasta que contém as Ferramentas do Dispatcher Locais. Se houver espaços no caminho, a variável `docker_run.cmd` falhará.

A versão das Ferramentas do Dispatcher é diferente da do SDK do AEM. Certifique-se de que a versão das Ferramentas do Dispatcher seja fornecida por meio da versão do SDK AEM correspondente à versão as a Cloud Service AEM.

1. Descompacte o arquivo baixado `aem-sdk-xxx.zip` arquivo
1. Descompacte as Ferramentas do Dispatcher em `~/aem-sdk/dispatcher`
   + Windows: Descompactar `aem-sdk-dispatcher-tools-x.x.x-windows.zip` em `C:\Users\<My User>\aem-sdk\dispatcher` (criando pastas ausentes, conforme necessário)
   + macOS / Linux: Executar o script de shell associado `aem-sdk-dispatcher-tools-x.x.x-unix.sh` para descompactar as Ferramentas do Dispatcher
      + `chmod a+x aem-sdk-dispatcher-tools-x.x.x-unix.sh && ./aem-sdk-dispatcher-tools-x.x.x-unix.sh`

Observe que todos os comandos emitidos abaixo pressupõem que o diretório de trabalho atual contém o conteúdo expandindo as Ferramentas do Dispatcher.

>[!VIDEO](https://video.tv.adobe.com/v/30601/?quality=12&learn=on)

*Este vídeo usa o macOS para fins ilustrativos. Os comandos equivalentes do Windows/Linux podem ser usados para obter resultados semelhantes*

## Entender os arquivos de configuração do Dispatcher

>[!TIP]
> Projetos Experience Manager criados a partir do [Arquétipo de Maven do Projeto AEM](https://github.com/adobe/aem-project-archetype) são preenchidos previamente nesse conjunto de arquivos de configuração do Dispatcher, portanto, não há necessidade de copiar da pasta src Ferramentas do Dispatcher.

As Ferramentas do Dispatcher fornecem um conjunto de arquivos de configuração Apache HTTP Web Server e Dispatcher que definem o comportamento para todos os ambientes, incluindo o desenvolvimento local.

Esses arquivos devem ser copiados em um projeto Experience Manager Maven para o `dispatcher/src` , se eles ainda não existirem no projeto Experience Manager Maven.

Uma descrição completa dos arquivos de configuração está disponível nas Ferramentas do Dispatcher descompactadas como `dispatcher-sdk-x.x.x/docs/Config.html`.

## Validar configurações

Opcionalmente, as configurações do servidor Web Dispatcher e Apache (por meio de `httpd -t`) pode ser validado usando o `validate` script (não confundir com `validator` executável). O `validate` O script fornece uma maneira conveniente de executar o [3 fases](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/implementing/content-delivery/validation-debug.html?lang=en#local-validation-flexible-mode) do `validator`.

+ Uso:
   + Windows: `bin\validate src`
   + macOS / Linux: `./bin/validate.sh ./src`

## Executar o Dispatcher localmente

AEM Dispatcher é executado localmente usando o Docker em relação à `src` Arquivos de configuração do Dispatcher e do Apache Web Server.

+ Uso:
   + Windows: `bin\docker_run <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`
   + macOS / Linux: `./bin/docker_run.sh <src-folder> <aem-publish-host>:<aem-publish-port> <dispatcher-port>`

O `<aem-publish-host>` pode ser definido como `host.docker.internal`, um nome DNS especial que o Docker fornece no contêiner que resolve o IP da máquina host. Se ele `host.docker.internal` não resolver, consulte o [solução de problemas](#troubleshooting-host-docker-internal) abaixo.

Por exemplo, para iniciar o contêiner do Dispatcher Docker usando os arquivos de configuração padrão fornecidos pelas Ferramentas do Dispatcher:

Inicie o contêiner do Dispatcher Docker fornecendo o caminho para a pasta src de configuração do Dispatcher:

+ Windows: `bin\docker_run src host.docker.internal:4503 8080`
+ macOS / Linux: `./bin/docker_run.sh ./src host.docker.internal:4503 8080`

O Serviço de Publicação do SDK as a Cloud Service AEM, executado localmente na porta 4503, está disponível por meio do Dispatcher em `http://localhost:8080`.

Para executar as Ferramentas do Dispatcher em relação à configuração do Dispatcher de um projeto do Experience Manager, aponte para o `dispatcher/src` pasta.

+ Windows:

   ```shell
   $ bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
   ```

+ macOS / Linux:

   ```shell
   $ ./bin/docker_run.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
   ```

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
+ Valores válidos: `dev`, `stage`ou `prod`

Um ou vários parâmetros podem ser passados para `docker_run`

+ Windows:

   ```shell
   $ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug bin\docker_run <User Directory>/code/my-project/dispatcher/src host.docker.internal:4503 8080
   ```

+ macOS / Linux:

   ```shell
   $ DISP_LOG_LEVEL=Debug REWRITE_LOG_LEVEL=Debug ./bin/docker_run.sh ~/code/my-project/dispatcher/src host.docker.internal:4503 8080
   ```

### Acesso ao arquivo de log

O servidor Web Apache e AEM os logs do Dispatcher podem ser acessados diretamente no container Docker:

+ [Acessar logs no container Docker](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-access-logs)
+ [Copiando os logs do Docker para o sistema de arquivos local](../debugging/aem-sdk-local-quickstart/logs.md#dispatcher-tools-copy-logs)

## Quando atualizar as ferramentas do Dispatcher{#dispatcher-tools-version}

As versões das Ferramentas do Dispatcher aumentam com menos frequência do que o Experience Manager e, portanto, as Ferramentas do Dispatcher exigem menos atualizações no ambiente de desenvolvimento local.

A versão recomendada das Ferramentas do Dispatcher é aquela que é fornecida com o SDK as a Cloud Service AEM que corresponde à versão do Experience Manager as a Cloud Service. A versão do AEM as a Cloud Service pode ser encontrada por meio de [Cloud Manager](https://my.cloudmanager.adobe.com/).

+ __Cloud Manager > Ambientes__, por ambiente especificado pelo __Versão AEM__ label

![Versão do Experience Manager](./assets/dispatcher-tools/aem-version.png)

_Observe que a própria versão das Ferramentas do Dispatcher não corresponderá à versão do Experience Manager._

## Resolução de problemas

### docker_run resulta na mensagem &#39;Aguardando até que host.docker.internal esteja disponível&#39;{#troubleshooting-host-docker-internal}

`host.docker.internal` é um nome de host fornecido ao Docker que é resolvido para o host. Por docs.docker.com ([macOS](https://docs.docker.com/docker-for-mac/networking/#i-want-to-connect-from-a-container-to-a-service-on-the-host), [Windows](https://docs.docker.com/docker-for-windows/networking/)):

> A partir do Docker 18.03, nossa recomendação é se conectar ao nome DNS especial host.docker.internal, que resolve para o endereço IP interno usado pelo host

Se, quando `bin/docker_run src host.docker.internal:4503 8080` resulta na mensagem __Aguardar até que host.docker.internal esteja disponível__, em seguida:

1. Verifique se a versão instalada do Docker é 18.03 ou superior
2. Você pode ter uma configuração de máquina local que esteja impedindo o registro/a resolução do `host.docker.internal` nome. Em vez disso, use seu IP local.
   + Windows:
      + No Prompt de Comando, execute `ipconfig`e registre o __Endereço IPv4__ da máquina host.
      + Em seguida, execute `docker_run` usando este endereço IP:
         `bin\docker_run src <HOST IP>:4503 8080`
   + macOS / Linux:
      + No Terminal, execute `ifconfig` e registrar o host __inet__ Endereço IP, geralmente o __en0__ dispositivo.
      + Em seguida, execute `docker_run` usando o endereço IP do host:
         `bin/docker_run.sh src <HOST IP>:4503 8080`

#### Exemplo de erro

```shell
$ docker_run src host.docker.internal:4503 8080

Running script /docker_entrypoint.d/10-check-environment.sh
Running script /docker_entrypoint.d/20-create-docroots.sh
Running script /docker_entrypoint.d/30-wait-for-backend.sh
Waiting until host.docker.internal is available
```

## Recursos adicionais

+ [Baixar AEM SDK](https://experience.adobe.com/#/downloads)
+ [Adobe Cloud Manager](https://my.cloudmanager.adobe.com/)
+ [Baixar Docker](https://www.docker.com/)
+ [Download do site de referência do AEM (WKND)](https://github.com/adobe/aem-guides-wknd/releases)
+ [Documentação do Dispatcher do Experience Manager](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/dispatcher.html?lang=pt-BR)
