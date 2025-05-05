---
title: Explicação dos arquivos de configuração do Dispatcher
description: Entenda arquivos de configuração, convenções de nomenclatura e muito mais.
version: Experience Manager 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: ec8e2804-1fd6-4e95-af6d-07d840069c8b
duration: 379
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1694'
ht-degree: 0%

---

# Explicação dos arquivos de configuração

[Índice](./overview.md)

[&lt;- Anterior: Layout básico do arquivo](./basic-file-layout.md)

Este documento detalha e explica cada um dos arquivos de configuração implantados em um servidor Dispatcher padrão criado no Adobe Managed Services. Seu uso, convenção de nomenclatura etc...

## Convenção de nomeação

Na verdade, o Apache Web Server não se importa com a extensão do arquivo ao direcioná-lo com uma instrução `Include` ou `IncludeOptional`.  A nomenclatura correta com nomes que eliminam conflitos e confusão ajuda em <b>ton</b>. Os nomes usados descreverão o escopo de aplicação do arquivo, facilitando a vida. Se tudo é nomeado como `.conf` isso fica realmente confuso. Queremos evitar arquivos e extensões com nomes inadequados.  Abaixo está uma lista das diferentes extensões de arquivo personalizadas e convenções de nomenclatura usadas em uma Dispatcher configurada típica do AMS.

## Arquivos contidos em conf.d/

| Arquivo | Destino do arquivo | Descrição |
| ---- | ---------------- | ----------- |
| NOME DE ARQUIVO`.conf` | `/etc/httpd/conf.d/` | Uma instalação padrão do Enterprise Linux usa essa extensão de arquivo e inclui a pasta como um local para substituir as configurações declaradas em httpd.conf e permitir que você adicione funcionalidades adicionais em nível global no Apache. |
| NOME DE ARQUIVO`.vhost` | Preparado: `/etc/httpd/conf.d/available_vhosts/`<br>Ativo: `/etc/httpd/conf.d/enabled_vhosts/`<br/><br/><b>Observação:</b> arquivos .vhost não devem ser copiados para a pasta enabled_vhosts, mas use symlinks para um caminho relativo para o arquivo available_vhosts/\*.vhost</u><br><br> | Os arquivos \*.vhost (Host Virtual) são `<VirtualHosts>`  entradas para corresponder aos nomes de host e permitir que o Apache manipule cada tráfego de domínio com regras diferentes. A partir do arquivo `.vhost`, outros arquivos como `rewrites`, `whitelisting`, `etc` serão incluídos. |
| NOME DE ARQUIVO`_rewrite.rules` | `/etc/httpd/conf.d/rewrites/` | `*_rewrite.rules` arquivos armazenam `mod_rewrite` regras a serem incluídas e consumidas explicitamente por um arquivo `vhost` |
| NOME DE ARQUIVO`_whitelist.rules` | `/etc/httpd/conf.d/whitelists/` | `*_ipwhitelist.rules` arquivos estão incluídos de dentro dos `*.vhost` arquivos. Ele contém regex IP ou permite regras de negação para permitir a lista de permissões de IP. Se estiver tentando restringir a exibição de um host virtual com base em endereços IP, você gerará um desses arquivos e o incluirá do seu arquivo `*.vhost` |

## Arquivos contidos em conf.dispatcher.d/

| Arquivo | Destino do arquivo | Descrição |
| --- | --- | --- |
| NOME DE ARQUIVO`.any` | `/etc/httpd/conf.dispatcher.d/` | O módulo AEM Dispatcher Apache origina suas configurações de `*.any` arquivos. O arquivo de inclusão pai padrão é `conf.dispatcher.d/dispatcher.any` |
| NOME DE ARQUIVO`_farm.any` | Preparado: `/etc/httpd/conf.dispatcher.d/available_farms/`<br>Ativo: `/etc/httpd/conf.dispatcher.d/enabled_farms/`<br><br><b>Observação:</b> esses arquivos de farm não devem ser copiados para a pasta `enabled_farms`, mas use `symlinks` para um caminho relativo para o arquivo `available_farms/*_farm.any`. Os arquivos <br/>`*_farm.any` estão incluídos dentro do arquivo `conf.dispatcher.d/dispatcher.any`. Esses arquivos farm principais existem para controlar o comportamento do módulo para cada tipo de renderização ou site. Os arquivos são criados no diretório `available_farms` e habilitados com um `symlink` no diretório `enabled_farms`.  <br/>Ele os inclui automaticamente pelo nome do arquivo `dispatcher.any`.Os arquivos do farm da <br/><b>Linha de Base</b> começam com `000_` para verificar se foram carregados primeiro.Os arquivos de farm <br><b>personalizados</b> devem ser carregados após o início de seu esquema de número em `100_` para garantir o comportamento de inclusão adequado. |
| NOME DE ARQUIVO`_filters.any` | `/etc/httpd/conf.dispatcher.d/filters/` | `*_filters.any` arquivos estão incluídos de dentro dos `conf.dispatcher.d/enabled_farms/*_farm.any` arquivos. Cada farm tem um conjunto de regras que alteram qual tráfego deve ser filtrado e não o processado. |
| NOME DE ARQUIVO`_vhosts.any` | `/etc/httpd/conf.dispatcher.d/vhosts/` | `*_vhosts.any` arquivos estão incluídos de dentro dos `conf.dispatcher.d/enabled_farms/*_farm.any` arquivos. Esses arquivos são uma lista de nomes de host ou caminhos de uri a serem correspondidos pela correspondência de blob para determinar qual renderizador usar para atender a essa solicitação |
| NOME DE ARQUIVO`_cache.any` | `/etc/httpd/conf.dispatcher.d/cache/` | `*_cache.any` arquivos estão incluídos de dentro dos `conf.dispatcher.d/enabled_farms/*_farm.any` arquivos. Esses arquivos especificam quais itens são armazenados em cache e quais não são |
| NOME DE ARQUIVO`_invalidate_allowed.any` | `/etc/httpd/conf.dispatcher.d/cache/` | `*_invalidate_allowed.any` arquivos estão incluídos dentro dos `conf.dispatcher.d/enabled_farms/*_farm.any` arquivos. Eles especificam quais endereços IP têm permissão para enviar solicitações de liberação e invalidação. |
| NOME DE ARQUIVO`_clientheaders.any` | `/etc/httpd/conf.dispatcher.d/clientheaders/` | `*_clientheaders.any` arquivos estão incluídos dentro dos `conf.dispatcher.d/enabled_farms/*_farm.any` arquivos. Eles especificam quais cabeçalhos do cliente devem ser transmitidos para cada renderizador. |
| NOME DE ARQUIVO`_renders.any` | `/etc/httpd/conf.dispatcher.d/renders/` | `*_renders.any` arquivos estão incluídos dentro dos `conf.dispatcher.d/enabled_farms/*_farm.any` arquivos. Eles especificam configurações de IP, porta e tempo limite para cada renderizador. Um renderizador adequado pode ser um servidor do LiveCycle ou qualquer sistema do AEM no qual o Dispatcher pode buscar/criar um proxy das solicitações do |

## Problemas evitados

Ao seguir a convenção de nomenclatura, você pode evitar alguns erros muito fáceis de cometer que podem ter resultados catastróficos.  Abordaremos alguns exemplos.

### Exemplo de problema

Como um exemplo de site para ExampleCo, dois arquivos de configuração foram criados pelos desenvolvedores das configurações do Dispatcher.

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

Se o arquivo `vhost` for colocado acidentalmente na pasta `rewrites` e o `rewrites file` for colocado na pasta `vhosts`.  Parece que foi implantado pelo nome de arquivo corretamente, mas o Apache emitirá um *ERRO* e o problema não será imediatamente aparente.

<b>Como isso normalmente se torna um problema</b>

Se os `two files` forem baixados no local `same`, eles poderão `overwrite themselves` ou torná-los indistinguíveis, tornando o processo de implantação um pesadelo.

<b>As extensões de arquivo são iguais e têm inclusão automática</b>

As extensões de arquivo são as mesmas e usam a extensão incluída automaticamente que o Apache `auto include` qualquer arquivo `.conf` em muitas de suas pastas padrão.

<b>Como isso normalmente se torna um problema</b>

Se o arquivo vhost com a extensão de `.conf` for colocado na pasta `/etc/httpd/conf.d/`, ele tentará carregá-lo na memória no Apache, o que geralmente é ok, mas se o arquivo de regras de regravação com a extensão de `.conf` for colocado na pasta `/etc/httpd/conf.d/`, ele será incluído automaticamente e aplicado globalmente, causando resultados confusos e indesejados.

## Resolução

Nomeie os arquivos com base no que eles fazem e com segurança fora do namespace de regras de inclusão automática.

Se for um arquivo de host virtual, nomeie-o com `.vhost` como extensão.

Se for um arquivo de regra de regravação, nomeie-o com o site `_rewrite.rules` como sufixo e extensão. Essa convenção de nomenclatura deixará claro para qual site é destinado e que é um conjunto de regras de regravação.

Se for um arquivo de regra de lista de permissões de IP, nomeie-o como descrição`_whitelist.rules` como sufixo e extensão. Essa convenção de nomenclatura fornecerá alguma descrição do que é para e que é um conjunto de regras de correspondência de IP.

Usar essas convenções de nomenclatura evitará problemas se um arquivo for movido para um diretório de inclusão automática ao qual não pertence.

Por exemplo, colocar um arquivo com o nome `.rules`, `.any` ou `.vhost` na pasta de inclusão automática de `/etc/httpd/conf.d/` não terá nenhum efeito.

Se uma solicitação de alteração de implantação informar &quot;implante exampleco_rewrite.rules nos Dispatchers de produção&quot;, a pessoa que implanta as alterações já poderá saber que não está adicionando um novo site, mas apenas atualiza as regras de regravação conforme indicado pelo nome do arquivo.

### Incluir pedido

Ao estender a funcionalidade e as configurações no Apache Webserver instalado no Enterprise Linux, você tem alguns pedidos de inclusão importantes que desejará entender

### Inclusões da linha de base do Apache

![A linha de base do Apache HTTPD Web Server inclui](assets/explanation-config-files/Apache-Webserver-Baseline-Includes.png)

Como visto no diagrama acima, o binário httpd procura somente o arquivo httpd.conf como seu arquivo de configuração.  Esse arquivo contém as seguintes declarações:

```
Include conf.modules.d/*.conf 
IncludeOptional conf.d/*.conf
```

### Inclusões de nível superior do AMS

Quando aplicamos nosso padrão, adicionamos alguns tipos de arquivos adicionais e inclusões próprias.

Estes são os diretórios da linha de base do AMS e os inclusões de nível superior
![A Linha de base do AMS inclui iniciar com um dispatcher_vhost.conf que incluirá qualquer arquivo com o *.vhost do diretório /etc/httpd/conf.d/enabled_vhosts/.  Os itens no diretório /etc/httpd/conf.d/enabled_vhosts/ são symlinks para o arquivo de configuração real que está em /etc/httpd/conf.d/available_vhosts/](assets/explanation-config-files/Apache-Webserver-AMS-Baseline-Includes.png "Apache-Webserver-AMS-Baseline-Includes")

Desenvolvendo a linha de base do Apache, mostramos como o AMS criou algumas pastas adicionais e inclusões de nível superior para `conf.d` pastas, bem como diretórios específicos do módulo aninhados em `/etc/httpd/conf.dispatcher.d/`

Quando o Apache for carregado, ele extrairá o `/etc/httpd/conf.modules.d/02-dispatcher.conf` e esse arquivo incluirá o arquivo binário `/etc/httpd/modules/mod_dispatcher.so` em seu estado de execução.

```
LoadModule dispatcher_module modules /mod_dispatcher .so
```

Para usar o módulo em nosso `<VirtualHost />`, soltamos um arquivo de configuração no `/etc/httpd/conf.d/` chamado `dispatcher_vhost.conf` e, dentro desse arquivo, você verá usar a configuração dos parâmetros básicos necessários para que o módulo funcione:

```
<IfModule disp_apache2.c> 
    DispatcherConfig conf.dispatcher.d/dispatcher.any 
    ...SNIP... 
</IfModule>
```

Como você pode ver acima, isso inclui o arquivo `dispatcher.any` de nível superior para que o módulo Dispatcher selecione seus arquivos de configuração de `/etc/httpd/conf.dispatcher.d/dispatcher.any`

Preste atenção ao conteúdo deste arquivo:

```
/farms { 
    $include "enabled_farms/*_farm.any" 
}
```

O arquivo de nível superior `dispatcher.any` inclui todos os arquivos de farm habilitados que residem em `/etc/httpd/conf.dispatcher.d/enabled_farms/` com o nome de arquivo `FILENAME_farm.any` que segue nossa convenção de nomenclatura padrão.

Em uma parte posterior do arquivo `dispatcher_vhost.conf` mencionado anteriormente, também fazemos uma instrução include para habilitar cada arquivo de host virtual habilitado que esteja em `/etc/httpd/conf.d/enabled_vhosts/` com o nome de arquivo `FILENAME.vhost`, que segue nossa convenção de nomenclatura padrão.

```
IncludeOptional /etc/httpd/conf.d/enabled_vhosts/*.vhost
```

Em cada um de nossos arquivos .vhost, você observará que o módulo Dispatcher é inicializado como um manipulador de arquivo padrão para um diretório.  Este é um exemplo de arquivo .vhost para mostrar a sintaxe:

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

Depois que o nível superior inclui resolver eles têm outras sub-inclusões que valem a pena mencionar.  Este é um diagrama de alto nível sobre como os arquivos farms e vhosts incluem outros elementos secundários

### O host virtual do AMS inclui

![Esta imagem mostra como um arquivo .vhost inclui arquivos de variáveis, listas de permissões e regrava pastas](assets/explanation-config-files/Apache-Webserver-AMS-Vhost-Includes.png "Apache-Webserver-AMS-Vhost-Includes")

Quando qualquer arquivo `.vhost` do diretório `/etc/httpd/conf.d/availabled_vhosts/` for vinculado ao diretório `/etc/httpd/conf.d/enabled_vhosts/`, ele será usado na configuração em execução.

Os arquivos `.vhost` têm sub-inclusões com base em partes comuns encontradas.  Coisas como variáveis, listas de permissões e regras de regravação.

O arquivo `.vhost` terá instruções de inclusão para cada arquivo com base em onde eles precisam ser incluídos no arquivo `.vhost`.  Este é um exemplo de sintaxe de um arquivo `.vhost` como uma boa referência:

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

Como você pode ver no exemplo acima, há uma inclusão para as variáveis necessárias nesse arquivo de configuração que serão usadas posteriormente.

Dentro do arquivo `/etc/httpd/conf.d/variables/weretail.vars`, podemos ver quais variáveis estão definidas:

```
Define MAIN_DOMAIN dev.weretail.com
```

Você também pode ver uma linha que inclui uma lista de `_whitelist.rules` arquivos que restringem quem pode exibir este conteúdo com base em diferentes critérios de lista de permissões.  Vamos examinar o conteúdo de um dos arquivos da lista de permissões `/etc/httpd/conf.d/whitelists/weretail_mainoffice_whitelist.rules`:

```
<RequireAny> 
  Require ip 192.150.16.0/23 
</RequireAny>
```

Você também pode ver uma linha que inclui um conjunto de regras de regravação.  Vamos analisar o conteúdo do arquivo `weretail_rewrite.rules`:

```
RewriteRule ^/robots.txt$ /content/dam/weretail/robots.txt [NC,PT] 
RewriteCond %{SERVER_NAME} brand1.weretail.net [NC] 
RewriteRule ^/favicon.ico$ /content/dam/weretail/favicon.ico [NC,PT] 
RewriteCond %{SERVER_NAME} brand2.weretail.com [NC] 
RewriteRule ^/sitemap.xml$ /content/weretail/general/sitemap.xml [NC,PT] 
RewriteRule ^/logo.jpg$ /content/dam/weretail/general/logo.jpg [NC,PT]
```

### AMS Farm Inclui

![&lt;FILENAME>_farms.any incluirá arquivos .any secundários para concluir uma configuração de farm.  Nesta imagem, você pode ver que um farm incluirá cada cache de arquivos de seção de nível superior, cabeçalhos de clientes, filtros, renderizações e arquivos vhosts .any](assets/explanation-config-files/Apache-Webserver-AMS-Farm-Includes.png "Apache-Webserver-AMS-Farm-Includes")

Quando qualquer arquivo FILENAME_farm.any do diretório `/etc/httpd/conf.dispatcher.d/available_farms/` for vinculado ao diretório `/etc/httpd/conf.dispatcher.d/enabled_farms/`, ele será usado na configuração em execução.

Os arquivos do farm têm subinclusões com base em [seções de nível superior do farm](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#defining-farms-farms), como cache, clientheaders, filtros, renderizadores e vhosts.

Os arquivos `FILENAME_farm.any` terão instruções de inclusão para cada arquivo com base em onde eles precisam ser incluídos no arquivo farm.  Este é um exemplo de sintaxe de um arquivo `FILENAME_farm.any` como uma boa referência:

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

Como você pode ver cada seção do farm weretail em vez de ter toda a sintaxe necessária, ela usará uma instrução include.

Vamos analisar a sintaxe de alguns desses includes para ter a ideia de como cada sub-include seria

`/etc/httpd/conf.dispatcher.d/vhosts/weretail_publish_vhosts.any`:

```
"brand1.weretail.com" 
"brand2.weretail.com" 
"www.weretail.comf"
```

Como você pode ver, é uma nova lista separada por linhas de nomes de domínio que deve ser renderizada deste farm em relação aos outros.

A seguir, vejamos o `/etc/httpd/conf.dispatcher.d/filters/weretail_search_filters.any`:

```
/400 { /type "allow" /method "GET" /path "/bin/weretail/lists/*" /extension "json" } 
/401 { /type "allow" /method "POST" /path "/bin/weretail/search/' /extension "html" }
```

[Próximo -> Noções básicas sobre cache](./understanding-cache.md)
