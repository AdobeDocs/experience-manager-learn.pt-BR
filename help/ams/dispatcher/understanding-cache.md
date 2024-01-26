---
title: 'Dispatcher: noções básicas sobre armazenamento em cache'
description: Entenda como o módulo Dispatcher opera seu cache.
topic: Administration, Performance
version: 6.5
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 66ce0977-1b0d-4a63-a738-8a2021cf0bd5
duration: 491
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '1716'
ht-degree: 0%

---

# Noções básicas sobre armazenamento em cache

[Índice](./overview.md)

[&lt;- Anterior: Explicação dos arquivos de configuração](./explanation-config-files.md)

Este documento explicará como ocorre o armazenamento em cache do Dispatcher e como ele pode ser configurado

## Armazenamento de Diretórios em Cache

Usamos os seguintes diretórios de cache padrão em nossas instalações de linha de base

- Autor
   - `/mnt/var/www/author`
- Editor
   - `/mnt/var/www/html`

Quando cada solicitação atravessa o Dispatcher, as solicitações seguem as regras configuradas para manter uma versão em cache localmente para responder a itens elegíveis

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

Intencionalmente, mantemos a carga de trabalho publicada separada da carga de trabalho do autor, pois quando o Apache procura um arquivo no DocumentRoot, ele não sabe de qual instância AEM ele veio. Portanto, mesmo que o cache esteja desativado no farm do autor, se DocumentRoot do autor for o mesmo que o publicador, ele fornecerá arquivos do cache quando presentes. Isso significa que você fornecerá arquivos de autor para do cache publicado e proporcionará uma experiência de combinação de combinações realmente horrível para seus visitantes.

Manter diretórios DocumentRoot separados para diferentes conteúdos publicados também é uma péssima ideia. Você terá que criar vários itens em cache novamente, que não diferem entre sites como clientlibs, além de ter que configurar um agente de limpeza de replicação para cada DocumentRoot configurado. Aumentar a quantidade de liberação sobre a cabeça com cada ativação de página. Conte com o namespace de arquivos e seus caminhos completos em cache e evite várias DocumentRoot&#39;s para sites publicados.
</div>

## Arquivos de configuração

O Dispatcher controla o que é qualificado como armazenável em cache no `/cache {` seção de qualquer arquivo farm. 
Nos farms de configuração de linha de base do AMS, você encontrará nossas inclusões, como mostrado abaixo:


```
/cache { 
    /rules { 
        $include "/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any" 
    }
```


Ao criar as regras para o que armazenar em cache ou não, consulte a documentação [aqui](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)


## Autor em cache

Vimos que há muitas implementações em que as pessoas não armazenam em cache conteúdo do autor. 
Eles estão perdendo um grande avanço no desempenho e na capacidade de resposta a seus autores.

Vamos falar sobre a estratégia adotada na configuração do farm do autor para armazenar em cache corretamente.

Aqui está um autor base `/cache {` seção do arquivo farm do autor:


```
/cache { 
    /docroot "/mnt/var/www/author" 
    /statfileslevel "2" 
    /allowAuthorized "1" 
    /rules { 
        $include "/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any" 
    } 
    /invalidate { 
        /0000 { 
            /glob "*" 
            /type "allow" 
        } 
    } 
    /allowedClients { 
        /0000 { 
            /glob "*.*.*.*" 
            /type "deny" 
        } 
        $include "/etc/httpd/conf.dispatcher.d/cache/ams_author_invalidate_allowed.any" 
    } 
}
```

O importante a ser observado aqui é que o `/docroot` está definido para o diretório de cache do autor.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

Verifique se o `DocumentRoot` no do autor `.vhost` o arquivo corresponde aos farms `/docroot` parâmetro
</div>

A instrução include das regras de cache inclui o arquivo `/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any` que contém estas regras:

```
/0000 { 
 /glob "*" 
 /type "deny" 
} 
/0001 { 
 /glob "/libs/*" 
 /type "allow" 
} 
/0002 { 
 /glob "/libs/*.html" 
 /type "deny" 
} 
/0003 { 
 /glob "/libs/granite/csrf/token.json" 
 /type "deny" 
} 
/0004 { 
 /glob "/apps/*" 
 /type "allow" 
} 
/0005 { 
 /glob "/apps/*.html" 
 /type "deny" 
} 
/0006 { 
 /glob "/libs/cq/core/content/welcome.*" 
 /type "deny" 
}
```

Em um cenário de criação, o conteúdo é alterado o tempo todo e de propósito. Você só deseja armazenar em cache itens que não serão alterados com frequência.
Temos regras para armazenar em cache `/libs` porque fazem parte da instalação básica do AEM e seriam alteradas até que você instalasse um Service Pack, Cumulative Fix Pack, Upgrade ou Hotfix. Portanto, armazenar esses elementos em cache faz muito sentido e realmente tem grandes benefícios da experiência do autor dos usuários finais que usam o site.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

Lembre-se de que essas regras também fazem cache <b>`/apps`</b> é aqui que reside o código de aplicativo personalizado. Se você estiver desenvolvendo seu código nesta instância, será muito confuso quando você salvar o arquivo e não verá se reflete na interface do usuário, pois ele serve uma cópia em cache. A intenção aqui é que, se você implantar seu código no AEM, também não seja frequente, e parte de suas etapas de implantação deve ser limpar o cache do autor. Novamente, o benefício é enorme, tornando seu código que pode ser armazenado em cache mais rápido para os usuários finais.
</div>


## ServeOnStale (também conhecido como Serve on Stale / SOS)

Esta é uma dessas pedras preciosas de um recurso do Dispatcher. Se o editor estiver sob carga ou não responder, normalmente emitirá um código de resposta http 502 ou 503. Se isso acontecer e esse recurso estiver ativado, o Dispatcher será instruído a continuar a fornecer o conteúdo que ainda estiver no cache, como um melhor esforço, mesmo que não seja uma cópia atualizada. É melhor servir algo se você o receber do que apenas mostrar uma mensagem de erro que não oferece nenhuma funcionalidade.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

Lembre-se de que, se o renderizador do editor tiver um tempo limite de soquete ou uma mensagem de erro 500, esse recurso não será acionado. Se o AEM estiver inacessível, esse recurso não fará nada
</div>

Essa configuração pode ser definida em qualquer farm, mas faz sentido aplicá-la somente nos arquivos de farm de publicação. Este é um exemplo de sintaxe do recurso habilitado em um arquivo farm:

```
/cache { 
    /serveStaleOnError "1"
```

## Armazenamento em cache de páginas com parâmetros/argumentos de consulta

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

Um dos comportamentos normais do módulo Dispatcher é que, se uma solicitação tiver um parâmetro de consulta no URI (normalmente mostrado como `/content/page.html?myquery=value`) ignorará o armazenamento em cache do arquivo e irá diretamente para a instância do AEM. Essa solicitação é considerada uma página dinâmica e não deve ser armazenada em cache. Isso pode causar efeitos negativos na eficiência do cache.
</div>
<br/>

Veja isto [artigo](https://github.com/adobe/aem-dispatcher-optimizer-tool/blob/main/docs/Rules.md#dot---the-dispatcher-publish-farm-cache-should-have-its-ignoreurlparams-rules-configured-in-an-allow-list-manner) mostrando como parâmetros de consulta importantes podem afetar o desempenho do site.

Por padrão, você deseja definir a variável `ignoreUrlParams` regras para permitir `*`.  Isso significa que todos os parâmetros de consulta são ignorados e permitem que todas as páginas sejam armazenadas em cache, independentemente dos parâmetros usados.

Este é um exemplo em que alguém criou um mecanismo de referência de deep link de redes sociais que usa a referência de argumento no URI para saber de onde a pessoa veio.

<b>Exemplo ignorável:</b>

- https://www.we-retail.com/home.html?reference=android
- https://www.we-retail.com/home.html?reference=facebook

A página é 100% armazenável em cache, mas não armazena em cache porque os argumentos estão presentes. 
Configuração do `ignoreUrlParams` o as a lista de permissões ajudará a corrigir esse problema:

```
/cache { 
    /ignoreUrlParams { 
        /0001 { /glob "*" /type "allow" }
    }
```

Agora, quando o Dispatcher visualizar a solicitação, ele ignorará o fato de que a solicitação tem o `query` parâmetro de `?` referenciar e ainda armazenar a página em cache

<b>Exemplo dinâmico:</b>

- https://www.we-retail.com/search.html?q=fruit
- https://www.we-retail.com/search.html?q=vegetables

Lembre-se de que, se você tiver parâmetros de consulta que fazem uma alteração de página na qual a saída é renderizada, será necessário isentá-los da lista ignorada e tornar a página não armazenável em cache novamente.  Por exemplo, uma página de pesquisa que usa um parâmetro de consulta altera o html bruto renderizado.

Então aqui está a fonte html de cada pesquisa:

`/search.html?q=fruit`:

```
<html>
...SNIP...
    <div id='results'>
        <div class='result'>
            Orange
        </div>
        <div class='result'>
            Apple
        </div>
        <div class='result'>
            Strawberry
        </div>
    </div>
</html>
```

`/search.html?q=vegetables`:

```
<html>
...SNIP...
    <div id='results'>
        <div class='result'>
            Carrot
        </div>
        <div class='result'>
            Cucumber
        </div>
        <div class='result'>
            Celery
        </div>
    </div>
</html>
```

Se você visitou `/search.html?q=fruit` primeiro, armazenava o html em cache, com os resultados mostrando frutos.

Em seguida, você visita `/search.html?q=vegetables` segundo, mas mostraria resultados de frutas.
Isso ocorre porque o parâmetro de consulta de `q` O está sendo ignorado em relação ao armazenamento em cache.  Para evitar esse problema, você precisará anotar as páginas que renderizam HTML diferente com base nos parâmetros de consulta e negar o armazenamento em cache para eles.

Exemplo:

```
/cache { 
    /ignoreUrlParams { 
        /0001 { /glob "*" /type "allow" }
        /0002 { /glob "q" /type "deny"  }
    }
```

As páginas que usam parâmetros de consulta por meio do JavaScript ainda funcionarão plenamente, ignorando os parâmetros nessa configuração.  Porque eles não alteram o arquivo html em repouso.  Eles usam javascript para atualizar os navegadores dom em tempo real no navegador local.  Isso significa que se você consumir os parâmetros de consulta com javascript, é altamente provável que você possa ignorar esse parâmetro para o armazenamento em cache da página.  Permita que essa página armazene em cache e aproveite o ganho de desempenho!

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

Acompanhar essas páginas requer alguma manutenção, mas vale a pena obter ganhos de desempenho.  Você pode solicitar que seu CSE execute um relatório no tráfego dos sites para fornecer uma lista de todas as páginas que usam parâmetros de consulta nos últimos 90 dias para que você analise e verifique quais páginas serão exibidas e quais parâmetros de consulta não serão ignorados
</div>
<br/>

## Armazenamento em cache de cabeçalhos de resposta

É bastante óbvio que o Dispatcher armazene em cache `.html` páginas e clientlibs (ou seja, `.js`, `.css`), mas você sabia que também pode armazenar em cache cabeçalhos de resposta específicos ao lado do conteúdo em um arquivo com o mesmo nome, mas um `.h` extensão de arquivo. Isso permite que a próxima resposta não seja apenas ao conteúdo, mas aos cabeçalhos de resposta que devem acompanhá-lo do cache.

O AEM pode lidar com mais do que apenas a codificação UTF-8

Às vezes, os itens têm cabeçalhos especiais que ajudam a controlar os detalhes de codificação do TTL de cache e os carimbos de data e hora da última modificação.

Esses valores quando em cache são removidos por padrão e o servidor Web Apache httpd fará seu próprio trabalho de processar o ativo com seus métodos normais de manipulação de arquivos, que normalmente é limitado à adivinhação de tipo mime com base em extensões de arquivo.

Se você tiver o Dispatcher armazenado em cache no ativo e nos cabeçalhos desejados, poderá expor a experiência adequada e garantir que todos os detalhes cheguem ao navegador dos clientes.

Este é um exemplo de um farm com os cabeçalhos para armazenar em cache especificados:

```
/cache { 
 /headers { 
  "Cache-Control" 
  "Content-Disposition" 
  "Content-Type" 
  "Expires" 
  "Last-Modified" 
  "X-Content-Type-Options" 
 } 
}
```


No exemplo, eles configuraram o AEM para servir cabeçalhos que o CDN procura para saber quando invalidar seu cache. Isso significa que agora o AEM pode ditar corretamente quais arquivos são invalidados com base nos cabeçalhos.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

Lembre-se de que não é possível usar expressões regulares ou correspondência glob. É uma lista literal dos cabeçalhos a serem armazenados em cache. Coloque somente em uma lista dos cabeçalhos literais que deseja armazenar em cache.
</div>


## Invalidar automaticamente o período de carência

Nos sistemas AEM que têm muita atividade de autores que fazem muitas ativações de página, você pode ter uma condição de corrida em que ocorrem invalidações repetidas. Solicitações de liberação muito repetidas não são necessárias e você pode integrar alguma tolerância para não repetir uma liberação até que o período de carência tenha sido eliminado.

### Exemplo de como isso funciona:

Se você tiver 5 solicitações para invalidar `/content/exampleco/en/` tudo acontece dentro de um período de 3 segundos.

Com esse recurso desativado, você invalidaria o diretório do cache `/content/exampleco/en/` 5 vezes

Com esse recurso ativado e definido como 5 segundos, ele invalidaria o diretório de cache `/content/exampleco/en/` <b>uma vez</b>

Este é um exemplo de sintaxe desse recurso sendo configurado para um período de carência de 5 segundos:

```
/cache { 
    /gracePeriod "5"
```

## Invalidação baseada em TTL

Um recurso mais recente do módulo Dispatcher foi `Time To Live (TTL)` opções de invalidação com base em para itens em cache. Quando um item é armazenado em cache, ele procura a presença de cabeçalhos de controle de cache e gera um arquivo no diretório de cache com o mesmo nome e um `.ttl` extensão.

Este é um exemplo do recurso que está sendo configurado no arquivo de configuração do farm:

```
/cache { 
    /enableTTL "1"
```

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>
Lembre-se que o AEM ainda precisa ser configurado para enviar cabeçalhos TTL para que o Dispatcher os honre. Alternar esse recurso permite que o Dispatcher saiba apenas quando remover os arquivos para os quais o AEM enviou cabeçalhos de controle de cache. Se o AEM não começar a enviar cabeçalhos TTL, o Dispatcher não fará nada especial aqui.
</div>

## Regras de Filtro de Cache

Este é um exemplo de uma configuração de linha de base para quais elementos armazenar em cache em um publicador:

```
/cache{ 
    /0000 { 
        /glob "*" 
        /type "allow" 
    } 
    /0001 { 
        /glob "/libs/granite/csrf/token.json" 
        /type "deny" 
    }
```

Queremos tornar nosso site publicado ganancioso quanto possível e armazenar tudo em cache.

Se houver elementos que interrompem a experiência quando armazenados em cache, você poderá adicionar regras para remover a opção para armazenar esse item em cache. Como você vê no exemplo acima, os tokens csrf nunca devem ser armazenados em cache e foram excluídos. Mais detalhes sobre a gravação dessas regras podem ser encontrados [aqui](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)

[Próximo -> Uso e noções básicas sobre variáveis](./variables.md)
