---
title: Dispatcher entendendo o armazenamento em cache
description: Entenda como o módulo Dispatcher opera seu cache.
topic: Administration, Performance
version: 6.5
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 04cd4002af7028ee9e3b1e1455b6346c56446245
workflow-type: tm+mt
source-wordcount: '1747'
ht-degree: 1%

---


# Como entender o armazenamento em cache

[Índice](./overview.md)

[&lt;- Anterior: Explicação dos arquivos de configuração](./explanation-config-files.md)

Este documento explicará como o armazenamento em cache do Dispatcher acontece e como ele pode ser configurado

## Diretórios de armazenamento em cache

Usamos os seguintes diretórios de cache padrão em nossas instalações de linha de base

- Autor
   - `/mnt/var/www/author`
- Editor
   - `/mnt/var/www/html`

Quando cada solicitação atravessa o Dispatcher, as solicitações seguem as regras configuradas para manter uma versão em cache local para responder aos itens elegíveis

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

Mantemos intencionalmente a carga de trabalho publicada separada da carga de trabalho do autor porque quando o Apache procura um arquivo no DocumentRoot, ele não sabe de qual instância AEM veio. Portanto, mesmo que o cache esteja desativado no farm do autor, se o DocumentRoot do autor for o mesmo que o editor, ele fornecerá arquivos do cache quando presentes. Isso significa que você fornecerá arquivos de autor para o a partir do cache publicado e proporcionará uma experiência de correspondência de mix muito ruim para seus visitantes.

Manter diretórios DocumentRoot separados para diferentes conteúdos publicados também é uma péssima ideia. Será necessário criar vários itens rearmazenados em cache que não sejam diferentes entre sites, como clientlibs, bem como configurar um agente de liberação de replicação para cada DocumentRoot configurado. Aumentar a quantidade de liberação indireta a cada ativação da página. Confie no namespace dos arquivos e em seus caminhos em cache completos e evite vários DocumentRoot para sites publicados.
</div>

## Arquivos de configuração

O Dispatcher controla o que é qualificado como armazenável em cache no `/cache {` de qualquer arquivo farm. 
Nos farms de configuração de linha de base do AMS, você encontrará nossas inclusões como mostrado abaixo:


```
/cache { 
    /rules { 
        $include "/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any" 
    }
```


Ao criar as regras para o que deve ser armazenado em cache ou não, consulte a documentação [here](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)


## Autor do armazenamento em cache

Vimos muitas implementações em que as pessoas não armazenam em cache o conteúdo do autor. 
Eles estão perdendo uma grande atualização em desempenho e capacidade de resposta para seus autores.

Vamos falar sobre a estratégia adotada na configuração do farm do autor para armazenar em cache corretamente.

Aqui está um autor básico `/cache {` seção do arquivo farm do autor:


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

O importante a ser observado aqui é que a variável `/docroot` é definido como o diretório de cache do autor.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

Certifique-se de que `DocumentRoot` no relatório `.vhost` arquivo corresponde aos farms `/docroot` parâmetro
</div>

A instrução de inclusão das regras de cache inclui o arquivo `/etc/httpd/conf.dispatcher.d/cache/ams_author_cache.any` que contém estas regras:

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

Em um cenário de criação, o conteúdo muda o tempo todo e de propósito. Você deseja armazenar em cache apenas itens que não serão alterados com frequência.
Temos regras para armazenar em cache `/libs` porque fazem parte da linha de base AEM instalar e seriam alteradas até que você tenha instalado um Service Pack, Cumulative Fix Pack, Upgrade ou Hotfix. Portanto, armazenar em cache esses elementos faz muito sentido e realmente tem enormes benefícios da experiência do autor de usuários finais que usam o site.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

Lembre-se de que essas regras também armazenam em cache <b>`/apps`</b> é aqui que o código de aplicativo personalizado está ativo. Se você estiver desenvolvendo seu código nessa instância, será muito confuso salvar seu arquivo e não ver se reflete na interface do usuário devido a ele servir uma cópia em cache. A intenção aqui é que, se você fizer uma implantação do seu código no AEM, isso também será pouco frequente e parte de suas etapas de implantação deverá ser limpar o cache do autor. Novamente, o benefício é enorme, tornando seu código armazenável em cache mais rápido para os usuários finais.
</div>


## ServeOnStale (AKA Serve no Stale / SOS)

Esta é uma daquelas joias de um recurso do Dispatcher. Se o editor estiver sob carga ou não responder, ele normalmente lançará um código de resposta http 502 ou 503. Se isso acontecer e esse recurso estiver ativado, o Dispatcher será instruído a continuar a veicular o conteúdo que ainda estiver no cache como o melhor esforço, mesmo que não seja uma cópia nova. É melhor servir algo se tiver algo em mente do que apenas mostrar uma mensagem de erro que não oferece funcionalidade.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

Lembre-se de que, se o renderizador do editor tiver um tempo limite de soquete ou 500 mensagens de erro, esse recurso não será acionado. Se AEM estiver inacessível, esse recurso não fará nada
</div>

Essa configuração pode ser definida em qualquer farm, mas só faz sentido aplicá-la aos arquivos farm de publicação. Este é um exemplo de sintaxe do recurso ativado em um arquivo farm:

```
/cache { 
    /serveStaleOnError "1"
```

## Armazenamento de páginas em cache com parâmetros/argumentos de consulta

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

Um dos comportamentos normais do módulo Dispatcher é que, se uma solicitação tiver um parâmetro de consulta no URI (normalmente mostrado como `/content/page.html?myquery=value`) ignorará o armazenamento em cache do arquivo e irá diretamente para a instância de AEM. Ele considera essa solicitação uma página dinâmica e não deve ser armazenada em cache. Isso pode causar malefícios na eficiência do cache.
</div>
<br/>

Veja isso [artigo](https://github.com/adobe/aem-dispatcher-optimizer-tool/blob/main/docs/Rules.md#dot---the-dispatcher-publish-farm-cache-should-have-its-ignoreurlparams-rules-configured-in-an-allow-list-manner) mostrar como parâmetros de consulta importantes podem afetar o desempenho do site.

Por padrão, você deseja definir a variável `ignoreUrlParams` regras para permitir `*`.  Isso significa que todos os parâmetros de consulta são ignorados e permitem que todas as páginas sejam armazenadas em cache, independentemente dos parâmetros usados.

Este é um exemplo em que alguém criou um mecanismo de referência de deep link de mídia social que usa a referência de argumento no URI para saber de onde a pessoa veio.

<b>Exemplo ignorável:</b>

- https://www.we-retail.com/home.html?reference=android
- https://www.we-retail.com/home.html?reference=facebook

A página é 100% armazenável em cache, mas não é armazenada em cache porque os argumentos estão presentes. 
Configurar a `ignoreUrlParams` o as a lista de permissões ajudará a corrigir esse problema:

```
/cache { 
    /ignoreUrlParams { 
        /0001 { /glob "*" /type "allow" }
    }
```

Agora, quando o Dispatcher visualizar a solicitação, ele ignorará o fato de que a solicitação tem a variável `query` parâmetro de `?` referencie e ainda armazene em cache a página

<b>Exemplo dinâmico:</b>

- https://www.we-retail.com/search.html?q=fruit
- https://www.we-retail.com/search.html?q=vegetables

Lembre-se de que, se você tiver parâmetros de consulta que fazem uma alteração de página, ela será renderizada, será necessário excluí-los da lista ignorada e tornar a página não armazenável em cache novamente.  Por exemplo, uma página de pesquisa que usa um parâmetro de consulta altera o html bruto renderizado.

Aqui está a fonte html de cada pesquisa:

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

Se você visitou `/search.html?q=fruit` primeiro, ele armazenaria em cache o html com resultados mostrando frutos.

Em seguida, você visita `/search.html?q=vegetables` segundo, mas mostraria resultados de frutos.
Isso ocorre porque o parâmetro de consulta de `q` está sendo ignorada no que diz respeito ao armazenamento em cache.  Para evitar esse problema, você precisará tomar nota das páginas que renderizam HTML diferentes com base em parâmetros de consulta e negar o armazenamento em cache para essas páginas.

Exemplo:

```
/cache { 
    /ignoreUrlParams { 
        /0001 { /glob "*" /type "allow" }
        /0002 { /glob "q" /type "deny"  }
    }
```

As páginas que usam parâmetros de consulta por meio do Javascript ainda funcionarão totalmente ao ignorar os parâmetros nessa configuração.  Porque eles não alteram o arquivo html em repouso.  Eles usam o javascript para atualizar os navegadores em tempo real no navegador local.  Isso significa que se você consumir os parâmetros de consulta com javascript, é altamente provável que você possa ignorar esse parâmetro para armazenamento em cache de página.  Permita que essa página armazene em cache e aproveite o ganho de desempenho!

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

Manter o controle dessas páginas requer alguma manutenção, mas vale a pena aumentar o desempenho.  Você pode solicitar que o CSE execute um relatório no tráfego de seus sites para fornecer uma lista de todas as páginas usando parâmetros de consulta nos últimos 90 dias para analisar e verificar se você sabe quais páginas analisar e quais parâmetros de consulta não ignorar
</div>
<br/>

## Armazenamento em cache de cabeçalhos de resposta

É bastante óbvio que o Dispatcher armazena em cache `.html` páginas e clientlibs (ou seja, `.js`, `.css`), mas você sabia que ele também pode armazenar em cache cabeçalhos de resposta específicos ao lado do conteúdo em um arquivo com o mesmo nome, mas com um `.h` extensão de arquivo. Isso permite que a próxima resposta não apenas para o conteúdo, mas também para os cabeçalhos de resposta que devem acompanhá-la do cache.

AEM pode lidar com mais do que apenas codificação UTF-8

Às vezes, os itens têm cabeçalhos especiais que ajudam a controlar os detalhes de codificação do TTL do cache e os carimbos de data e hora da última modificação.

Esses valores, quando armazenados em cache, são removidos por padrão e o servidor Web Apache httpd fará seu próprio trabalho de processar o ativo com seus métodos normais de manipulação de arquivos, que normalmente são limitados à adivinhação de tipo MIME com base nas extensões de arquivo.

Se você tiver o Dispatcher armazenando o ativo em cache e os cabeçalhos desejados, poderá expor a experiência adequada e garantir que todos os detalhes o cheguem ao navegador do cliente.

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


No exemplo, eles configuraram o AEM para fornecer cabeçalhos que o CDN procura para saber quando invalidar o cache. Isso significa que agora AEM pode ditar corretamente quais arquivos são invalidados com base em cabeçalhos.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

Lembre-se de que não é possível usar expressões regulares ou correspondência de glob. É uma lista literal dos cabeçalhos a serem armazenados em cache. Coloque apenas uma lista dos cabeçalhos literais que você deseja que ele armazene em cache.
</div>


## Período de carência de invalidação automática

Em sistemas de AEM que têm muita atividade de autores que fazem muitas ativações de página, você pode ter uma condição de corrida em que ocorrem invalidações repetidas. Solicitações de liberação com muitas repetições são desnecessárias e você pode criar alguma tolerância para não repetir uma liberação até que o período de carência tenha sido limpo.

### Exemplo de como isso funciona:

Se você tiver 5 solicitações para invalidar `/content/exampleco/en/` tudo acontece dentro de um período de 3 segundos.

Com esse recurso desativado, você invalidaria o diretório de cache `/content/exampleco/en/` 5 vezes

Com esse recurso ativado e definido como 5 segundos, invalidaria o diretório de cache `/content/exampleco/en/` <b>once</b>

Este é um exemplo de sintaxe desse recurso sendo configurado por 5 segundos de carência:

```
/cache { 
    /gracePeriod "5"
```

## Invalidação baseada em TTL

Um recurso mais recente do módulo Dispatcher foi `Time To Live (TTL)` opções de invalidação baseadas em itens em cache. Quando um item é armazenado em cache, ele procura a presença de cabeçalhos de controle de cache e gera um arquivo no diretório de cache com o mesmo nome e um `.ttl` extensão.

Este é um exemplo do recurso que está sendo configurado no arquivo de configuração farm:

```
/cache { 
    /enableTTL "1"
```

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Observação:</b>
Lembre-se de que AEM ainda precisa ser configurado para enviar cabeçalhos TTL para que o Dispatcher os honre. Alternar esse recurso permite que o Dispatcher saiba quando remover os arquivos para os quais o AEM enviou cabeçalhos de controle de cache. Se AEM não começar a enviar cabeçalhos TTL, o Dispatcher não fará nada de especial aqui.
</div>

## Regras de Filtro de Cache

Este é um exemplo de uma configuração de linha de base para os elementos que serão armazenados em cache em um editor:

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

Queremos tornar nosso site publicado o mais ganancioso possível e armazenar em cache tudo.

Se houver elementos que interrompem a experiência quando armazenados em cache, você poderá adicionar regras para remover a opção de armazenar em cache esse item. Como você vê no exemplo acima, os tokens csrf nunca devem ser armazenados em cache e foram excluídos. Mais detalhes sobre como escrever essas regras podem ser encontrados [here](https://experienceleague.adobe.com/docs/experience-manager-dispatcher/using/configuring/dispatcher-configuration.html?lang=en#configuring-the-dispatcher-cache-cache)

[Próximo -> Uso e noções básicas sobre variáveis](./variables.md)