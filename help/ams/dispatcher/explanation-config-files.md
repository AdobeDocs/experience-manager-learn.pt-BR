---
title: Explicação dos arquivos de configuração do Dispatcher
description: Entenda arquivos de configuração, convenções de nomenclatura e muito mais.
version: 6.5
topic: Administration
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
exl-id: ec8e2804-1fd6-4e95-af6d-07d840069c8b
source-git-commit: da0b536e824f68d97618ac7bce9aec5829c3b48f
workflow-type: tm+mt
source-wordcount: '1705'
ht-degree: 0%

---

# Explicação dos arquivos de configuração

[Índice](./overview.md)

[&lt;- Anterior: Layout básico do arquivo](./basic-file-layout.md)

Este documento detalha e explica cada um dos arquivos de configuração implantados em um servidor Dispatcher padrão criado no Adobe Managed Services. Seu uso, convenção de nomenclatura etc...

## Convenção de nomeação

Na verdade, o Apache Web Server não se importa com a extensão do arquivo ao direcioná-lo com um `Include` ou `IncludeOptional` declaração.  A nomenclatura correta com nomes que eliminam conflitos e confusão ajuda a <b>ton</b>. Os nomes usados descreverão o escopo de aplicação do arquivo, facilitando a vida. Se tudo for nomeado `.conf` isso fica muito confuso. Queremos evitar arquivos e extensões com nomes inadequados.  Abaixo está uma lista das diferentes extensões de arquivo personalizadas e convenções de nomenclatura usadas em um Dispatcher típico configurado para AMS.

## Arquivos contidos em conf.d/

| Arquivo | Destino do arquivo | Descrição |
| ---- | ---------------- | ----------- |
| NOME DO ARQUIVO`.conf` | `/etc/httpd/conf.d/` | Uma instalação padrão do Enterprise Linux usa essa extensão de arquivo e inclui a pasta como um local para substituir as configurações declaradas em httpd.conf e permitir que você adicione funcionalidades adicionais em nível global no Apache. |
| NOME DO ARQUIVO`.vhost` | Preparado: `/etc/httpd/conf.d/available_vhosts/`<br>Ativo: `/etc/httpd/conf.d/enabled_vhosts/`<br/><br/><div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b> Os arquivos .vhost não devem ser copiados para a pasta enabled_vhosts, mas use symlinks para um caminho relativo para o arquivo available_vhosts/\*.vhost</div></u><br><br> | Os arquivos \*.vhost (Host Virtual) são `<VirtualHosts>`  entradas para corresponder aos nomes de host e permitir que o Apache manipule cada tráfego de domínio com regras diferentes. No `.vhost` arquivo, outros arquivos como `rewrites`, `whitelisting`, `etc` serão incluídos. |
| NOME DO ARQUIVO`_rewrite.rules` | `/etc/httpd/conf.d/rewrites/` | `*_rewrite.rules` armazenamento de arquivos `mod_rewrite` regras a serem incluídas e consumidas explicitamente por um `vhost` arquivo |
| NOME DO ARQUIVO`_whitelist.rules` | `/etc/httpd/conf.d/whitelists/` | `*_ipwhitelist.rules` os arquivos são incluídos de dentro do `*.vhost` arquivos. Ele contém regex IP ou permite regras de negação para permitir a lista de permissões de IP. Se estiver tentando restringir a visualização de um host virtual com base em endereços IP, você gerará um desses arquivos e o incluirá a partir de seu `*.vhost` arquivo |

## Arquivos contidos em conf.dispatcher.d/

| Arquivo | Destino do arquivo | Descrição |
| --- | --- | --- |
| NOME DO ARQUIVO`.any` | `/etc/httpd/conf.dispatcher.d/` | O módulo Apache do Dispatcher do AEM origina suas configurações `*.any` arquivos. O arquivo de inclusão pai padrão é `conf.dispatcher.d/dispatcher.any` |
| NOME DO ARQUIVO`_farm.any` | Preparado: `/etc/httpd/conf.dispatcher.d/available_farms/`<br>Ativo: `/etc/httpd/conf.dispatcher.d/enabled_farms/`<br><br><div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b> esses arquivos do farm não devem ser copiados para o `enabled_farms` pasta, mas usar `symlinks` para um caminho relativo para o `available_farms/*_farm.any` arquivo </div> <br/>`*_farm.any` os arquivos estão incluídos dentro do `conf.dispatcher.d/dispatcher.any` arquivo. Esses arquivos farm principais existem para controlar o comportamento do módulo para cada tipo de renderização ou site. Os arquivos são criados no `available_farms` e habilitado com um `symlink` no `enabled_farms` diretório.  <br/>Ele os inclui automaticamente pelo nome no campo `dispatcher.any` arquivo.<br/><b>Linha de base</b> os arquivos do farm começam com `000_` para verificar se eles foram carregados primeiro.<br><b>Personalizado</b> os arquivos do farm devem ser carregados depois de iniciar o esquema de número em `100_` para garantir o comportamento de inclusão adequado. |
| NOME DO ARQUIVO`_filters.any` | `/etc/httpd/conf.dispatcher.d/filters/` | `*_filters.any` os arquivos são incluídos de dentro do `conf.dispatcher.d/enabled_farms/*_farm.any` arquivos. Cada farm tem um conjunto de regras que alteram qual tráfego deve ser filtrado e não o processado. |
| NOME DO ARQUIVO`_vhosts.any` | `/etc/httpd/conf.dispatcher.d/vhosts/` | `*_vhosts.any` os arquivos são incluídos de dentro do `conf.dispatcher.d/enabled_farms/*_farm.any` arquivos. Esses arquivos são uma lista de nomes de host ou caminhos de uri a serem correspondidos pela correspondência de blob para determinar qual renderizador usar para atender a essa solicitação |
| NOME DO ARQUIVO`_cache.any` | `/etc/httpd/conf.dispatcher.d/cache/` | `*_cache.any` os arquivos são incluídos de dentro do `conf.dispatcher.d/enabled_farms/*_farm.any` arquivos. Esses arquivos especificam quais itens são armazenados em cache e quais não são |
| NOME DO ARQUIVO`_invalidate_allowed.any` | `/etc/httpd/conf.dispatcher.d/cache/` | `*_invalidate_allowed.any` os arquivos estão incluídos dentro do `conf.dispatcher.d/enabled_farms/*_farm.any` arquivos. Eles especificam quais endereços IP têm permissão para enviar solicitações de liberação e invalidação. |
| NOME DO ARQUIVO`_clientheaders.any` | `/etc/httpd/conf.dispatcher.d/clientheaders/` | `*_clientheaders.any` os arquivos estão incluídos dentro do `conf.dispatcher.d/enabled_farms/*_farm.any` arquivos. Eles especificam quais cabeçalhos do cliente devem ser transmitidos para cada renderizador. |
| NOME DO ARQUIVO`_renders.any` | `/etc/httpd/conf.dispatcher.d/renders/` | `*_renders.any` os arquivos estão incluídos dentro do `conf.dispatcher.d/enabled_farms/*_farm.any` arquivos. Eles especificam configurações de IP, porta e tempo limite para cada renderizador. Um renderizador adequado pode ser um servidor do LiveCycle ou qualquer sistema AEM no qual o Dispatcher possa buscar/aplicar proxy nas solicitações do |

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

Se a variável `vhost` arquivo for colocado acidentalmente no `rewrites` e a `rewrites file` é colocado na `vhosts` pasta.  Parece ser implantado pelo nome de arquivo corretamente, mas o Apache lançará um *ERRO* e o problema não será imediatamente aparente.

<b>Como isso normalmente se torna um problema</b>

Se a variável `two files` são baixados para o `same` local em que eles podem `overwrite themselves` ou torná-lo indistinguível, tornando o processo de implantação um pesadelo.

<b>As extensões de arquivo são as mesmas e são susceptíveis de inclusão automática</b>

As extensões de arquivo são as mesmas e usam a extensão incluída automaticamente que o Apache `auto include` qualquer `.conf` em muitas pastas padrão.

<b>Como isso normalmente se torna um problema</b>

Se o arquivo vhost com a extensão de `.conf` é colocado no `/etc/httpd/conf.d/` pasta ele tentará carregá-lo na memória no Apache, o que geralmente é ok, mas se o arquivo de regras de regravação com a extensão de `.conf` é colocado no `/etc/httpd/conf.d/` , ele será incluído automaticamente e aplicado globalmente, causando resultados confusos e indesejados.

## Resolução

Nomeie os arquivos com base no que eles fazem e com segurança fora do namespace de regras de inclusão automática.

Se for um arquivo de host virtual, nomeie-o com `.vhost` como a extensão.

Se for um arquivo de regra de regravação, nomeie-o com site`_rewrite.rules` como sufixo e extensão. Essa convenção de nomenclatura deixará claro para qual site é destinado e que é um conjunto de regras de regravação.

Se for um arquivo de regra de lista de permissões de IP, nomeie-o como descrição`_whitelist.rules` como sufixo e extensão. Essa convenção de nomenclatura fornecerá alguma descrição do que é para e que é um conjunto de regras de correspondência de IP.

Usar essas convenções de nomenclatura evitará problemas se um arquivo for movido para um diretório de inclusão automática ao qual não pertence.

Por exemplo, colocar um arquivo chamado com `.rules`, `.any`ou `.vhost` na pasta de inclusão automática de `/etc/httpd/conf.d/` não teria nenhum efeito.

Se uma solicitação de alteração de implantação informar &quot;implante exampleco_rewrite.rules nos Dispatchers de produção&quot;, a pessoa que implanta as alterações já poderá saber que não está adicionando um novo site, mas apenas atualiza as regras de regravação conforme indicado pelo nome do arquivo.

### Incluir pedido

Ao estender a funcionalidade e as configurações no Apache Webserver instalado no Enterprise Linux, você tem alguns pedidos de inclusão importantes que desejará entender

### Inclusões da linha de base do Apache

![](assets/explanation-config-files/Apache-Webserver-Baseline-Includes.png)

Como visto no diagrama acima, o binário httpd procura somente o arquivo httpd.conf como seu arquivo de configuração.  Esse arquivo contém as seguintes declarações:

```
Include conf.modules.d/*.conf 
IncludeOptional conf.d/*.conf
```

### Inclusões de nível superior do AMS

Quando aplicamos nosso padrão, adicionamos alguns tipos de arquivos adicionais e inclusões próprias.

Estes são os diretórios da linha de base do AMS e os inclusões de nível superior
![A Linha de base do AMS inclui iniciar com um dispatcher_vhost.conf que incluirá qualquer arquivo com o *.vhost do diretório /etc/httpd/conf.d/enabled_vhosts/.  Os itens no diretório /etc/httpd/conf.d/enabled_vhosts/ são symlinks para o arquivo de configuração real que está em /etc/httpd/conf.d/available_vhosts/](assets/explanation-config-files/Apache-Webserver-AMS-Baseline-Includes.png "Apache-Webserver-AMS-Baseline-Includes")

Desenvolvendo a linha de base do Apache, mostramos como o AMS criou algumas pastas adicionais e inclusões de nível superior para o `conf.d` pastas, bem como diretórios específicos do módulo aninhados em `/etc/httpd/conf.dispatcher.d/`

Quando o Apache for carregado, ele extrairá o `/etc/httpd/conf.modules.d/02-dispatcher.conf` e esse arquivo incluirá o arquivo binário `/etc/httpd/modules/mod_dispatcher.so` no estado de execução.

```
LoadModule dispatcher_module modules /mod_dispatcher .so
```

Para usar o módulo em nossa `<VirtualHost />` soltamos um arquivo de configuração em `/etc/httpd/conf.d/` nomeado `dispatcher_vhost.conf` e dentro desse arquivo você verá usar a configuração dos parâmetros básicos necessários para que o módulo funcione:

```
<IfModule disp_apache2.c> 
    DispatcherConfig conf.dispatcher.d/dispatcher.any 
    ...SNIP... 
</IfModule>
```

Como você pode ver acima, isso inclui o nível superior `dispatcher.any` para que o módulo Dispatcher selecione os arquivos de configuração em `/etc/httpd/conf.dispatcher.d/dispatcher.any`

Preste atenção ao conteúdo deste arquivo:

```
/farms { 
    $include "enabled_farms/*_farm.any" 
}
```

O nível superior `dispatcher.any` arquivo inclui todos os arquivos do farm habilitados que residem `/etc/httpd/conf.dispatcher.d/enabled_farms/` com o nome de arquivo de `FILENAME_farm.any` que segue nossa convenção de nomenclatura padrão.

Mais tarde no `dispatcher_vhost.conf` arquivo mencionado anteriormente, também fazemos uma instrução include para habilitar cada arquivo de host virtual habilitado que esteja `/etc/httpd/conf.d/enabled_vhosts/` com o nome de arquivo de `FILENAME.vhost` que segue nossa convenção de nomenclatura padrão.

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

![Esta figura mostra como um arquivo .vhost inclui arquivos de variáveis, listas de permissão e regrava pastas](assets/explanation-config-files/Apache-Webserver-AMS-Vhost-Includes.png "Apache-Webserver-AMS-Vhost-Includes")

Quando houver `.vhost` arquivos de `/etc/httpd/conf.d/availabled_vhosts/` o diretório é vinculado ao `/etc/httpd/conf.d/enabled_vhosts/` diretório em que serão usados na configuração em execução.

A variável `.vhost` os arquivos têm sub-inclusões com base em partes comuns encontradas.  Coisas como variáveis, listas de permissões e regras de regravação.

A variável `.vhost` arquivo terá instruções de inclusão para cada arquivo com base em onde eles precisam ser incluídos no `.vhost` arquivo.  Este é um exemplo de sintaxe de `.vhost` arquivar como uma boa referência:

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

Dentro do arquivo `/etc/httpd/conf.d/variables/weretail.vars` podemos ver quais variáveis estão definidas:

```
Define MAIN_DOMAIN dev.weretail.com
```

Você também pode ver uma linha que inclui uma lista de `_whitelist.rules` arquivos que restringem quem pode exibir esse conteúdo com base em diferentes critérios de lista de permissões.  Vamos analisar o conteúdo de um dos arquivos da lista branca `/etc/httpd/conf.d/whitelists/weretail_mainoffice_whitelist.rules`:

```
<RequireAny> 
  Require ip 192.150.16.0/23 
</RequireAny>
```

Você também pode ver uma linha que inclui um conjunto de regras de regravação.  Vamos analisar o conteúdo da `weretail_rewrite.rules` arquivo:

```
RewriteRule ^/robots.txt$ /content/dam/weretail/robots.txt [NC,PT] 
RewriteCond %{SERVER_NAME} brand1.weretail.net [NC] 
RewriteRule ^/favicon.ico$ /content/dam/weretail/favicon.ico [NC,PT] 
RewriteCond %{SERVER_NAME} brand2.weretail.com [NC] 
RewriteRule ^/sitemap.xml$ /content/weretail/general/sitemap.xml [NC,PT] 
RewriteRule ^/logo.jpg$ /content/dam/weretail/general/logo.jpg [NC,PT]
```

### AMS Farm Inclui

![<FILENAME>_farms.any incluirá arquivos .any secundários para concluir uma configuração de farm.  Nesta imagem, você pode ver que um farm incluirá cada cache de arquivos de seção de nível superior, cabeçalhos de clientes, filtros, renderizadores e arquivos vhosts .any](assets/explanation-config-files/Apache-Webserver-AMS-Farm-Includes.png "Apache-Webserver-AMS-Farm-Includes")

Quando qualquer arquivo FILENAME_farm.any de `/etc/httpd/conf.dispatcher.d/available_farms/` o diretório é vinculado ao `/etc/httpd/conf.dispatcher.d/enabled_farms/` diretório em que serão usados na configuração em execução.

Os arquivos do farm têm subinclusões com base em [seções de nível superior do farm](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#defining-farms-farms) como cache, clientheaders, filtros, renderizações e vhosts.

A variável `FILENAME_farm.any` os arquivos terão instruções include para cada arquivo com base em onde eles precisam ser incluídos no arquivo farm.  Este é um exemplo de sintaxe de `FILENAME_farm.any` arquivar como uma boa referência:

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

A seguir, vejamos o `/etc/httpd/conf.dispatcher.d/filters/weretail_search_filters.any`:

```
/400 { /type "allow" /method "GET" /path "/bin/weretail/lists/*" /extension "json" } 
/401 { /type "allow" /method "POST" /path "/bin/weretail/search/' /extension "html" }
```

[Próximo -> Noções básicas sobre cache](./understanding-cache.md)
