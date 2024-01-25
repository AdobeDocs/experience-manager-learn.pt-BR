---
title: Logs comuns do Dispatcher do AEM
description: Consulte as entradas de log comuns do Dispatcher e saiba o que significam e como abordá-las.
version: 6.5
topic: Administration, Performance
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 7fe1b4a5-6813-4ece-b3da-40af575ea0ed
duration: 275
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '799'
ht-degree: 0%

---

# Logs comuns

[Índice](./overview.md)

[&lt;- Anterior: URLs personalizados](./disp-vanity-url.md)

## Visão geral

O documento descreverá as entradas de log comuns observadas, o que significam e como lidar com elas.

## Aviso GLOB

Exemplo de entrada de log:

```
Fri Jul 20 03:35:09 2018 W pid 8300 (tid 139937910880384) /etc/httpd/conf/publish-filters.any:5: Allowing requests with globs is considered unsafe.
Please consult the documentation at 'https://www.adobe.com/go/docs_dispatcher_config_en' on how to use attributes 
method/url/query/protocol/path/selectors/extension/suffix instead.
```

Desde o módulo Dispatcher 4.2.x, eles começaram a desencorajar as pessoas a usar o seguinte tipo de correspondência nos arquivos de filtros:

```
/0041 { /type "allow" /glob "* *.css *"   }
```

Em vez disso, é melhor usar a nova sintaxe como:

```
/0041 { /type "allow" /url "*.css" }
```

Ou melhor ainda, não corresponder a um marcador curinga:

```
/0041 { /type "allow" /extension "css" }
```

Seguir qualquer um dos métodos sugeridos eliminaria essa mensagem de erro dos logs.

## Filtrar rejeitos


<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>
Essas entradas nem sempre são exibidas, mesmo que as rejeições ocorram se o nível do log estiver definido como muito baixo. Defina-o como Info ou debug para garantir que você possa ver se os filtros estão rejeitando as solicitações.
</div>

Exemplo de entrada de log:

```
Fri Jul 20 17:25:48 2018 D pid 25939 (tid 139937517123328) Filter rejects: GET /libs/granite/core/content/login.html HTTP/1.1
```

ou:

```
Fri Jul 20 22:16:55 2018 I pid 128803 "GET /system/console/" ! - 8ms publishfarm/-
```

<div style="color: #000;border-left: 6px solid red;background-color:#ddffff;"><b>Atenção:</b>

Entenda que as regras do Dispatcher foram definidas para filtrar essa solicitação. Nesse caso, a página que tentou ser visitada foi rejeitada de propósito e não desejamos fazer nada com ela.
</div>

Se o seu log for semelhante a esta entrada:

```
Fri Jul 20 17:26:47 2018 D pid 20051 (tid 139937517123328) Filter rejects: 
GET /etc/designs/exampleco/fonts/montserrat-regular/montserrat-regular-webfont.eot HTTP/1.1
```

Isso nos informa que nosso arquivo de design `.eot` O está sendo bloqueado e queremos corrigir isso.
Portanto, devemos examinar nosso arquivo de filtros e adicionar a seguinte linha para permitir `.eot` arquivos até

```
/0011 { /type "allow" /method "GET" /extension 'eot' /path "/etc/designs/*" }
```

Isso permitiria a passagem do arquivo e impediria o logon.
Se quiser ver o que está sendo filtrado, execute este comando no arquivo de log:

```
$ grep "Filter rejects\|\!" /var/log/httpd/dispatcher.log | awk 'match($0, /\/.*\//, m){ print m0 }' | awk '{ print $1 }'| sort | uniq -c | sort -rn
```

## Tempo limite da renderização

Exemplo de entrada de log do tempo limite do soquete:

```
Fri Jul 20 22:31:15 2018 W pid 3648 Unable to connect socket to 10.43.3.40:4502: Connection timed out 
Fri Jul 20 22:31:15 2018 W pid 3648 Unable to connect to any backend in farm authorfarm
```

Isso ocorre quando o endereço IP incorreto é configurado na seção de renderizações do farm. Ou a instância do AEM parou de responder ou escutar e o Dispatcher não pode acessá-la.

Verifique as regras de firewall e se a instância do AEM está em execução e em bom funcionamento.

Entradas de log de amostra do tempo limite do gateway:

```
Fri Jul 20 22:32:42 2018 I pid 3648 "GET /favicon.ico" 502 - 54034ms authorfarm/- 
Fri Jul 20 22:35:45 2018 I pid 3648 "GET /favicon.ico" 503 - 54234ms authorfarm/-
```

Isso significa que a instância do AEM tinha um soquete aberto que podia ser acessado e o tempo limite de resposta foi excedido. Significa que a instância do AEM estava muito lenta ou não estava funcionando e o Dispatcher atingiu as configurações de tempo limite definidas na seção de renderização do farm. Aumente a configuração de tempo limite ou deixe a instância do AEM em bom funcionamento.

## Nível de armazenamento em cache

Exemplo de entrada de log:

```
Fri Jul 20 23:00:19 2018 I pid 16004 (tid 140134145820416) Current cache hit ratio: 87.94 %
```

Significa que a busca do nível de renderização vs do cache foi medida. Você deseja atingir 80% ou mais do cache e deve seguir a ajuda [aqui](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17458.html%3Flang%3Den):

Para obter este número o mais alto possível.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>
Mesmo que as configurações de cache no arquivo do farm armazenem em cache tudo o que você estiver liberando com muita frequência ou intensidade, isso pode causar uma taxa de ocorrência de cache de porcentagem menor
</div>

## Diretório ausente

Exemplo de entrada de log:

```
Fri Jul 20 14:02:43 2018 E pid 4728 (tid 140662586435328) Unable to create parent directory /mnt/var/www/author/libs/dam/content/asseteditors/formitems.overlay.infinity.json/application: Not a directory
```

Normalmente, ela é exibida quando um item está sendo buscado enquanto ocorre uma limpeza do cache ao mesmo tempo.

Ou o diretório raiz do documento tem permissões incorretas ou o contexto de arquivo SELinux incorreto na pasta que está impedindo o apache de criar os novos subdiretórios necessários.

Para problemas de permissão, consulte as permissões do documento raiz e verifique se são semelhantes ao seguinte:

```
dispatcher$ ls -Z /var/www/
drwxr-xr-x+ apache apache system_u:object_r:httpd_sys_content_t:s0 html
```

## Vanity URL não encontrada

Exemplo de entrada de log:

```
Thu Sep 27 17:35:11 2018 D pid 18936 Checking vanity URLs 
Thu Sep 27 17:35:11 2018 D pid 18936 Vanity URL file (/tmp/vanity_urls) not found, fetching... 
Thu Sep 27 17:35:11 2018 W pid 18936 Unable to fetch vanity URLs from 10.43.0.42:4503/libs/granite/dispatcher/content/vanityUrls.html: remote server returned: HTTP/1.1 404 Not Found
```

Esse erro ocorre quando você configura o Dispatcher para usar o filtro automático dinâmico para permitir Vanity URLs, mas não conclui a configuração instalando o pacote no renderizador do AEM.

Para corrigir isso, instale o pacote de recursos url personalizado na instância do AEM e permita que seja preparado pelo usuário anônimo. Detalhes [aqui](https://experienceleague.adobe.com/docs/experience-cloud-kcs/kbarticles/KA-17463.html%3Flang%3Den)

Uma configuração de Vanity URL de trabalho é semelhante ao seguinte:

```
Thu Sep 27 17:40:29 2018 D pid 21844 Checking vanity URLs 
Thu Sep 27 17:40:29 2018 D pid 21844 Vanity URL file (/tmp/vanity_urls) not found, fetching... 
Thu Sep 27 17:40:29 2018 D pid 21844 Loaded 18 vanity URLs from file /tmp/vanity_urls
```

## Farm ausente

Exemplo de entrada de log:

```
Wed Nov 13 17:17:26 2019 W pid 19173:tid 140542738364160 No farm matches host 'we-retail.com', selected last farm 'publishfarm'
```

Esse erro indica que de todos os arquivos do farm disponíveis em `/etc/httpd/conf.dispatcher.d/enabled_farms/` não foi possível encontrar uma entrada correspondente do `/virtualhost` seção.

Os arquivos do farm correspondem ao tráfego com base no nome do domínio ou caminho em que a solicitação foi recebida. Ele usa a correspondência de glob e, se não corresponder, você não configurou o farm corretamente, digitou a entrada no farm ou a entrada está totalmente ausente. Quando o farm não corresponde às entradas, o padrão é o último farm incluído na pilha de arquivos do farm incluídos. Neste exemplo, foi `999_ams_publish_farm.any` que é chamado de nome genérico de publishfarm.

Este é um exemplo de arquivo farm `/etc/httpd/conf.dispatcher.d/enabled_farms/300_weretail_publish_farm.any` foi reduzido para realçar as partes relevantes.

## Item veiculado a partir de

Exemplo de entrada de log:

```
Tue Nov 26 16:41:34 2019 I pid 9208 (tid 140112092391168) "GET /content/we-retail/us/en.html" - + 24034ms publishfarm/0
```

A página foi buscada pelo método http do GET para o conteúdo `/content/we-retail/us/en.html` e levou 24034 milissegundos. A parte em que desejamos prestar atenção está no final `publishfarm/0`. Você verá que ele direcionou e correspondeu aos `publishfarm`. A solicitação foi obtida da renderização 0. Isso significa que essa página teve que ser solicitada ao AEM e depois armazenada em cache. Agora vamos solicitar esta página novamente e ver o que acontece com o log.

[Próximo -> Arquivos somente leitura](./immutable-files.md)
