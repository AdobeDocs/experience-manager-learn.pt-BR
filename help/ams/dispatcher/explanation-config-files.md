---
title: Explicação dos arquivos de configuração do Dispatcher
description: Entenda os arquivos de configuração, as convenções de nomenclatura e muito mais.
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 04cd4002af7028ee9e3b1e1455b6346c56446245
workflow-type: tm+mt
source-wordcount: '1705'
ht-degree: 0%

---


# Explicação dos arquivos de configuração

[Índice](./overview.md)

[&lt;- Anterior: Layout básico do arquivo](./basic-file-layout.md)

Este documento detalhará e explicará cada um dos arquivos de configuração implantados em um servidor Dispatcher padrão criado no Adobe Managed Services. Seu uso, convenção de nomenclatura etc...

## Convenção de nomenclatura

Na verdade, o Apache Web Server não se importa com a extensão de arquivo de um arquivo ao direcioná-lo com um `Include` ou `IncludeOptional` instrução.  Nomeá-los corretamente com nomes que eliminam conflitos e confusão ajuda a <b>ton</b>. Os nomes usados descreverão o escopo de aplicação do arquivo, facilitando a vida. Se tudo for nomeado `.conf` isso fica muito confuso. Queremos evitar arquivos e extensões com nomes inadequados.  Abaixo está uma lista das diferentes extensões de arquivo personalizadas e convenções de nomenclatura usadas em um Dispatcher típico configurado pelo AMS.

## Arquivos contidos em conf.d/

| Arquivo | Destino do arquivo | Descrição |
| ---- | ---------------- | ----------- |
| NOME DO ARQUIVO`.conf` | `/etc/httpd/conf.d/` | Uma instalação padrão do Enterprise Linux usa essa extensão de arquivo e inclui a pasta como um local para substituir as configurações declaradas em httpd.conf e permitir adicionar funcionalidades adicionais em um nível global no Apache. |
| NOME DO ARQUIVO`.vhost` | Preparado: `/etc/httpd/conf.d/available_vhosts/`<br>Ativo: `/etc/httpd/conf.d/enabled_vhosts/`<br/><br/><div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Observação:</b> Os arquivos .vhost não devem ser copiados para a pasta enabled_vhosts, mas usam links simbólicos para um caminho relativo para o arquivo available_vhosts/\*.vhost</div></u><br><br> | Os arquivos \*.vhost (Host virtual) são `<VirtualHosts>`  para corresponder aos nomes dos hosts e permitir que o Apache gerencie cada tráfego de domínio com regras diferentes. No `.vhost` arquivo, outros arquivos como `rewrites`, `whitelisting`, `etc` serão incluídas. |
| NOME DO ARQUIVO`_rewrite.rules` | `/etc/httpd/conf.d/rewrites/` | `*_rewrite.rules` armazenamento de arquivos `mod_rewrite` regras a serem incluídas e consumidas explicitamente por uma `vhost` arquivo |
| NOME DO ARQUIVO`_whitelist.rules` | `/etc/httpd/conf.d/whitelists/` | `*_ipwhitelist.rules` os arquivos são incluídos de dentro da variável `*.vhost` arquivos. Ele contém regex de IP ou permite regras de negação para permitir a lista de permissões de IP. Se estiver tentando restringir a visualização de um host virtual com base em endereços IP, você gerará um desses arquivos e o incluirá em seu `*.vhost` arquivo |

## Arquivos contidos em conf.modules.d/

| Arquivo | Destino do arquivo | Descrição |
| --- | --- | --- |
| NOME DO ARQUIVO`.any` | `/etc/httpd/conf.dispatcher.d/` | As configurações do Módulo AEM Dispatcher Apache são fornecidas `*.any` arquivos. O arquivo de inclusão principal padrão é `conf.dispatcher.d/dispatcher.any` |
| NOME DO ARQUIVO`_farm.any` | Preparado: `/etc/httpd/conf.dispatcher.d/available_farms/`<br>Ativo: `/etc/httpd/conf.dispatcher.d/enabled_farms/`<br><br><div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Observação:</b> esses arquivos farm não devem ser copiados para o `enabled_farms` pasta , mas use `symlinks` para um caminho relativo para a variável `available_farms/*_farm.any` arquivo </div> <br/>`*_farm.any` os arquivos são incluídos dentro da variável `conf.dispatcher.d/dispatcher.any` arquivo. Esses arquivos farm primários existem para controlar o comportamento do módulo para cada renderização ou tipo de site. Os arquivos são criados na `available_farms` e habilitado com um `symlink` na `enabled_farms` diretório.  <br/>Ele os inclui automaticamente por nome no `dispatcher.any` arquivo.<br/><b>Linha de base</b> os arquivos farm começam com `000_` para garantir que sejam carregadas primeiro.<br><b>Personalizado</b> Os arquivos do farm devem ser carregados depois, iniciando o seu esquema numérico em `100_` para garantir o comportamento de inclusão correto. |
| NOME DO ARQUIVO`_filters.any` | `/etc/httpd/conf.dispatcher.d/filters/` | `*_filters.any` os arquivos são incluídos de dentro da variável `conf.dispatcher.d/enabled_farms/*_farm.any` arquivos. Cada farm tem um conjunto de regras que alteram o tráfego que deve ser filtrado e não para os renderizadores. |
| NOME DO ARQUIVO`_vhosts.any` | `/etc/httpd/conf.dispatcher.d/vhosts/` | `*_vhosts.any` os arquivos são incluídos de dentro da variável `conf.dispatcher.d/enabled_farms/*_farm.any` arquivos. Esses arquivos são uma lista de nomes de host ou caminhos de uri que devem ser correspondidos pela correspondência de blob para determinar qual renderizador usar para fornecer essa solicitação |
| NOME DO ARQUIVO`_cache.any` | `/etc/httpd/conf.dispatcher.d/cache/` | `*_cache.any` os arquivos são incluídos de dentro da variável `conf.dispatcher.d/enabled_farms/*_farm.any` arquivos. Esses arquivos especificam quais itens são armazenados em cache e quais não são |
| NOME DO ARQUIVO`_invalidate_allowed.any` | `/etc/httpd/conf.dispatcher.d/cache/` | `*_invalidate_allowed.any` os arquivos são incluídos dentro da variável `conf.dispatcher.d/enabled_farms/*_farm.any` arquivos. Eles especificam quais endereços IP podem enviar solicitações de liberação e invalidação. |
| NOME DO ARQUIVO`_clientheaders.any` | `/etc/httpd/conf.dispatcher.d/clientheaders/` | `*_clientheaders.any` os arquivos são incluídos dentro da variável `conf.dispatcher.d/enabled_farms/*_farm.any` arquivos. Eles especificam quais cabeçalhos do cliente devem ser passados para cada renderizador. |
| NOME DO ARQUIVO`_renders.any` | `/etc/httpd/conf.dispatcher.d/renders/` | `*_renders.any` os arquivos são incluídos dentro da variável `conf.dispatcher.d/enabled_farms/*_farm.any` arquivos. Eles especificam as configurações de IP, porta e tempo limite para cada renderizador. Um renderizador adequado pode ser um servidor do livecycle ou qualquer sistema de AEM em que o Dispatcher possa buscar/proxy as solicitações de |

## Problemas evitados

Ao seguir a convenção de nomenclatura, você pode evitar erros bastante fáceis de cometer que podem ter resultados catastróficos.  Abordaremos alguns exemplos.

### Exemplo de problema

Como um Exemplo de site para ExampleCo, dois arquivos de configuração foram criados pelos desenvolvedores das configurações do Dispatcher.

<b>/etc/httpd/conf.d/exampleco.conf</b>

```
<VirtualHost *:80> 
    ServerName  "exampleco" 
    ServerAlias "www.exampleco.com" 
    .......... SNIP ............... 
    <IfModule mod_rewrite.c> 
        ReWriteEngine   on 
        LogLevel warn rewrite:trace1 
        Include /etc/httpd/conf.d/rewrites/exampleco.conf 
    </IfModule> 
</VirtualHost>
```

<b>/etc/httpd/conf.d/rewrites/exampleco.conf</b>

```
RewriteRule ^/$ /content/exampleco/en.html [PT,L] 
RewriteRule ^/robots.txt$ /content/dam/exampleco/robots.txt [PT,L]
```

#### `POTENTIAL DANGER - The file names are the same`

Se a variável `vhost` for colocado acidentalmente no `rewrites` e a `rewrites file` é colocado no `vhosts` pasta.  Parece que ele foi implantado corretamente pelo nome do arquivo, mas o Apache lançará um *ERRO* e o problema não será imediatamente aparente.

<b>Como isso normalmente se torna um problema</b>

Se a variável `two files` são baixados para o `same` podem `overwrite themselves` ou torná-lo indistinguível, tornando o processo de implantação um pesadelo.

<b>As extensões de arquivo são iguais e propensas à inclusão automática</b>

As extensões de arquivo são as mesmas e usam a extensão incluída automaticamente que o Apache irá `auto include` any `.conf` arquivos em muitas das pastas padrão.

<b>Como isso normalmente se torna um problema</b>

Se o arquivo vhost com a extensão de `.conf` é colocado no `/etc/httpd/conf.d/` a pasta tentará carregá-la na memória do Apache, o que normalmente é ok, mas se o arquivo de regras de regravação com a extensão de `.conf` é colocado no `/etc/httpd/conf.d/` , ele será incluído automaticamente e aplicado globalmente, causando resultados confusos e indesejados.

## Resolução

Nomeie os arquivos com base no que eles fazem e com segurança fora do namespace de regras de inclusão automática.

Se for um arquivo de host virtual, nomeie-o com `.vhost` como a extensão.

Se for um arquivo de regra de reescrita, nomeie-o com o site`_rewrite.rules` como sufixo e extensão. Essa convenção de nomenclatura deixará claro para qual site é e que é um conjunto de regras de regravação.

Se for um arquivo de regra da lista de permissões de IP, nomeie-o como descrição`_whitelist.rules` como sufixo e extensão. Essa convenção de nomenclatura fornecerá uma descrição do seu objetivo e que é um conjunto de regras de correspondência de IP.

O uso dessas convenções de nomenclatura evitará problemas, se um arquivo for movido para um diretório de inclusão automática ao qual não pertence.

Por exemplo, colocar um arquivo nomeado com `.rules`, `.any`ou `.vhost` na pasta de inclusão automática de `/etc/httpd/conf.d/` não teria nenhum efeito.

Se uma solicitação de alteração de implantação indicar &quot;implante exampleco_rewrite.rules nos Dispatchers de produção&quot;, a pessoa que implantou as alterações já poderá saber que não está adicionando um novo site, apenas atualizando as regras de regravação conforme indicado pelo nome do arquivo.

### Incluir pedido

Ao estender a funcionalidade e as configurações no servidor Web Apache instalado no Enterprise Linux, você tem alguns pedidos de inclusão importantes que deseja entender

### Inclusões da linha de base do Apache

![](assets/explanation-config-files/Apache-Webserver-Baseline-Includes.png)

Conforme visto no diagrama acima, o binário httpd somente é exibido no arquivo httpd.conf como arquivo de configuração.  Esse arquivo contém as seguintes instruções:

```
Include conf.modules.d/*.conf 
IncludeOptional conf.d/*.conf
```

### Inclusões de nível superior do AMS

Quando aplicamos nosso padrão, adicionamos alguns tipos de arquivo adicionais e inclusões próprias.

Aqui estão os diretórios de linha de base do AMS e as inclusões de nível superior
![A linha de base do AMS inclui iniciar com um dispatcher_vhost.conf que incluirá qualquer arquivo com o *.vhost do diretório /etc/httpd/conf.d/enabled_vhosts/.  Os itens no diretório /etc/httpd/conf.d/enabled_vhosts/ são links simbólicos para o arquivo de configuração real que está ativo em /etc/httpd/conf.d/available_vhosts/](assets/explanation-config-files/Apache-Webserver-AMS-Baseline-Includes.png "Apache-Webserver-AMS-Baseline-Includes")

Ao criar a linha de base do Apache, mostramos como o AMS criou algumas pastas adicionais e as inclusões de nível superior para o `conf.d` bem como diretórios específicos do módulo aninhados em `/etc/httpd/conf.dispatcher.d/`

Quando o Apache é carregado, ele recebe a variável `/etc/httpd/conf.modules.d/02-dispatcher.conf` e esse arquivo incluirá o arquivo binário `/etc/httpd/modules/mod_dispatcher.so` no estado de execução.

```
LoadModule dispatcher_module modules /mod_dispatcher .so
```

Para usar o módulo em `<VirtualHost />` soltamos um arquivo de configuração em `/etc/httpd/conf.d/` nomeado `dispatcher_vhost.conf` e dentro desse arquivo você verá usar a configuração dos parâmetros básicos necessários para que o módulo funcione:

```
<IfModule disp_apache2.c> 
    DispatcherConfig conf.dispatcher.d/dispatcher.any 
    ...SNIP... 
</IfModule>
```

Como você pode ver acima, isso inclui o nível superior `dispatcher.any` arquivo para que o módulo Dispatcher selecione os arquivos de configuração de `/etc/httpd/conf.dispatcher.d/dispatcher.any`

Preste atenção ao conteúdo deste arquivo:

```
/farms { 
    $include "enabled_farms/*_farm.any" 
}
```

O nível superior `dispatcher.any` O arquivo inclui todos os arquivos farm habilitados no `/etc/httpd/conf.dispatcher.d/enabled_farms/` com o nome de arquivo de `FILENAME_farm.any` que segue nossa convenção de nomenclatura padrão.

Mais tarde no `dispatcher_vhost.conf` arquivo mencionado anteriormente, também fazemos uma instrução include para habilitar cada arquivo de host virtual habilitado que esteja ativo em `/etc/httpd/conf.d/enabled_vhosts/` com o nome do arquivo de `FILENAME.vhost` que segue nossa convenção de nomenclatura padrão.

```
IncludeOptional /etc/httpd/conf.d/enabled_vhosts/*.vhost
```

Em cada um de nossos arquivos .vhost, você observará que o módulo Dispatcher é inicializado como um manipulador de arquivos padrão para um diretório.  Este é um exemplo de arquivo .vhost para mostrar a sintaxe:

```
<VirtualHost *:80> 
 ServerName "weretail" 
 ServerAlias www.weretail.com weretail.com 
 <Directory /> 
  <IfModule disp_apache2.c> 
   ....SNIP.... 
   SetHandler dispatcher-handler 
  </IfModule> 
  ....SNIP.... 
 </Directory> 
 ....SNIP.... 
</VirtualHost>
```

Depois que as inclusões de nível superior são resolvidas, elas têm outras subinclusões que valem a pena mencionar.  Este é um diagrama de alto nível sobre como os arquivos farms e vhosts incluem outros subelementos

### Inclusões de host virtual do AMS

![Esta imagem mostra como um arquivo .vhost inclui arquivos de variáveis, listas de permissões e pastas de regravação](assets/explanation-config-files/Apache-Webserver-AMS-Vhost-Includes.png "Apache-Webserver-AMS-Vhost-Includes")

Quando qualquer `.vhost` arquivos de `/etc/httpd/conf.d/availabled_vhosts/` o diretório é vinculado simbólico à `/etc/httpd/conf.d/enabled_vhosts/` diretory serão usados na configuração em execução.

O `.vhost` os arquivos têm subinclusões com base em partes comuns que encontramos.  Coisas como variáveis, listas de permissões e regras de regravação.

O `.vhost` o arquivo terá instruções include para cada arquivo com base em onde elas precisam ser incluídas na variável `.vhost` arquivo.  Este é um exemplo de sintaxe de um `.vhost` como uma boa referência:

```
Include /etc/httpd/conf.d/variables/weretail.vars 
<VirtualHost *:80> 
 ServerName "${MAIN_DOMAIN}" 
 <Directory /> 
  Include /etc/httpd/conf.d/whitelists/weretail*_whitelist.rules 
  <IfModule disp_apache2.c> 
   ....SNIP.... 
   SetHandler dispatcher-handler 
  </IfModule> 
  ....SNIP.... 
 </Directory> 
 ....SNIP.... 
 <IfModule mod_rewrite.c> 
  ReWriteEngine   on 
  LogLevel warn rewrite:trace1 
  Include /etc/httpd/conf.d/rewrites/weretail_rewrite.rules 
 </IfModule> 
</VirtualHost>
```

Como você pode ver no exemplo acima, há uma inclusão para as variáveis necessárias neste arquivo de configuração que são usadas posteriormente.

Dentro do arquivo `/etc/httpd/conf.d/variables/weretail.vars` podemos ver quais variáveis são definidas:

```
Define MAIN_DOMAIN dev.weretail.com
```

Você também pode ver uma linha que inclui uma lista de `_whitelist.rules` arquivos que restringem quem pode exibir esse conteúdo com base em critérios de lista de permissões diferentes.  Vamos analisar o conteúdo de um dos arquivos da lista branca `/etc/httpd/conf.d/whitelists/weretail_mainoffice_whitelist.rules`:

```
<RequireAny> 
  Require ip 192.150.16.0/23 
</RequireAny>
```

Você também pode ver uma linha que inclui um conjunto de regras de regravação.  Vamos analisar o conteúdo da variável `weretail_rewrite.rules` arquivo:

```
RewriteRule ^/robots.txt$ /content/dam/weretail/robots.txt [NC,PT] 
RewriteCond %{SERVER_NAME} brand1.weretail.net [NC] 
RewriteRule ^/favicon.ico$ /content/dam/weretail/favicon.ico [NC,PT] 
RewriteCond %{SERVER_NAME} brand2.weretail.com [NC] 
RewriteRule ^/sitemap.xml$ /content/weretail/general/sitemap.xml [NC,PT] 
RewriteRule ^/logo.jpg$ /content/dam/weretail/general/logo.jpg [NC,PT]
```

### Inclusões do farm do AMS

![<FILENAME>_farms.any incluirá arquivos sub.any para concluir uma configuração de farm.  Nesta imagem, você pode ver que um farm incluirá cada cache de arquivos de seção de nível superior, cabeçalhos de clientes, filtros, renderizações e arquivos vhosts .any](assets/explanation-config-files/Apache-Webserver-AMS-Farm-Includes.png "Apache-Webserver-AMS-Farm-Includes")

Quando qualquer arquivo FILENAME_farm.any de `/etc/httpd/conf.dispatcher.d/available_farms/` o diretório é vinculado simbólico à `/etc/httpd/conf.dispatcher.d/enabled_farms/` diretory serão usados na configuração em execução.

Os arquivos farm têm subinclusões com base em [seções de nível superior da exploração](https://docs.adobe.com/content/help/en/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html#defining-farms-farms) como cache, cabeçalhos de clientes, filtros, renderizações e vhosts.

O `FILENAME_farm.any` os arquivos terão instruções include para cada arquivo com base no local em que precisam ser incluídos no arquivo farm.  Este é um exemplo de sintaxe de um `FILENAME_farm.any` como uma boa referência:

```
/weretailfarm {   
 /clientheaders { 
  $include "/etc/httpd/conf.dispatcher.d/clientheaders/ams_publish_clientheaders.any" 
  $include "/etc/httpd/conf.dispatcher.d/clientheaders/ams_common_clientheaders.any" 
 } 
 /virtualhosts { 
  $include "/etc/httpd/conf.dispatcher.d/vhosts/weretail_vhosts.any" 
 } 
 /renders { 
  $include "/etc/httpd/conf.dispatcher.d/renders/ams_publish_renders.any" 
 } 
 /filter { 
  $include "/etc/httpd/conf.dispatcher.d/filters/ams_publish_filters.any" 
  $include "/etc/httpd/conf.dispatcher.d/filters/weretail_search_filters.any" 
 } 
 ....SNIP.... 
 /cache { 
  ....SNIP.... 
  /rules { 
   $include "/etc/httpd/conf.dispatcher.d/cache/ams_publish_cache.any" 
  } 
  ....SNIP.... 
  /allowedClients { 
   /0000 { 
    /glob "*.*.*.*" 
    /type "deny" 
   } 
   $include "/etc/httpd/conf.dispatcher.d/cache/ams_publish_invalidate_allowed.any" 
  } 
 ....SNIP.... 
 } 
}
```

Como você pode ver cada seção do weretail farm em vez de ter toda a sintaxe necessária, ela está usando uma instrução include.

Vamos analisar a sintaxe de algumas dessas inclusões para ter uma ideia de como cada subinclusão seria

`/etc/httpd/conf.dispatcher.d/vhosts/weretail_publish_vhosts.any`:

```
"brand1.weretail.com" 
"brand2.weretail.com" 
"www.weretail.comf"
```

Como você pode ver, é uma nova lista separada por linhas de nomes de domínio que deve renderizar deste farm sobre os outros.

Em seguida, vamos olhar para o `/etc/httpd/conf.dispatcher.d/filters/weretail_search_filters.any`:

```
/400 { /type "allow" /method "GET" /path "/bin/weretail/lists/*" /extension "json" } 
/401 { /type "allow" /method "POST" /path "/bin/weretail/search/' /extension "html" }
```

[Próximo -> Noções básicas sobre cache](./understanding-cache.md)