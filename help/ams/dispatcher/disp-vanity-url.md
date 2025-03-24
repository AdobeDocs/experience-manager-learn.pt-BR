---
title: Recurso de URLs personalizados do AEM Dispatcher
description: Entenda como o AEM lida com URLs personalizados e técnicas adicionais usando regras de regravação para mapear o conteúdo mais próximo da borda do delivery.
version: Experience Manager 6.5
topic: Administration, Performance
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
doc-type: Article
exl-id: 53baef9c-aa4e-4f18-ab30-ef9f4f5513ee
duration: 244
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1159'
ht-degree: 0%

---

# URLs personalizados do Dispatcher

[Índice](./overview.md)

[&lt;- Anterior: Liberação de Dispatcher](./disp-flushing.md)

## Visão geral

Este documento ajuda você a entender como o AEM lida com urls personalizados e algumas técnicas adicionais usando regras de regravação para mapear o conteúdo mais próximo da borda do delivery

## O que são URLs personalizados

Quando você tem conteúdo em uma estrutura de pastas lógica, ele nem sempre está em um URL de fácil referência. URLs personalizados são como atalhos. URLs menores ou exclusivos que fazem referência ao local em que o conteúdo real está.

Um exemplo: `/aboutus` apontou para `/content/we-retail/us/en/about-us.html`

Os AEM Author têm a opção de definir as propriedades de url personalizado em um conteúdo no AEM e publicá-lo.

Para que esse recurso funcione, é necessário ajustar os filtros do Dispatcher para permitir a personalização. Isso não é aceitável para ajustar os arquivos de configuração do Dispatcher na taxa que os autores teriam para configurar essas entradas de página personalizada.

Por esse motivo, o módulo Dispatcher tem um recurso para permitir automaticamente qualquer item listado como personalizado na árvore de conteúdo.


## Como funciona

### Criação de URLs personalizados

O autor visita uma página no AEM, clica nas propriedades da página e adiciona entradas na seção _URL personalizado_. Ao salvar as alterações e ativar a página, a personalização é atribuída à página.

Os autores também podem marcar a caixa de seleção _Redirecionar URL personalizado_ ao adicionar entradas de _URL personalizado_, isso faz com que os URLs personalizados se comportem como redirecionamentos 302. Isso significa que o navegador é instruído a ir para a nova URL (via cabeçalho de resposta `Location`) e o navegador faz uma nova solicitação para a nova URL.

#### Interface do usuário para toque:

![Menu suspenso de diálogo para a interface de usuário de criação do AEM na tela do editor de sites](assets/disp-vanity-url/aem-page-properties-drop-down.png "menu suspenso de aem-page-properties")

![página de diálogo de propriedades da página do aem](assets/disp-vanity-url/aem-page-properties.png "aem-page-properties")

#### Localizador de conteúdo clássico:

![propriedades da página de sidekick da interface clássica do AEM siteadmin](assets/disp-vanity-url/aem-page-properties-sidekick.png "aem-page-properties-sidekick")

![Caixa de diálogo Propriedades da página da interface clássica](assets/disp-vanity-url/aem-page-properties-classic.png "aem-page-properties-classic")


>[!NOTE]
>
>Entenda que isso é propenso a problemas de espaço de nome. Entradas personalizadas são globais para todas as páginas, esta é apenas uma das falhas que você precisa planejar para soluções alternativas que explicaremos mais tarde.


## Resolução/mapeamento de recursos

Cada entrada personalizada é uma entrada de mapa de sling para um redirecionamento interno.

Os mapas são visíveis ao visitar o console Felix de instâncias do AEM ( `/system/console/jcrresolver` ).

Esta é uma captura de tela de uma entrada de mapa criada por uma entrada personalizada:
![captura de tela do console de uma entrada personalizada nas regras de resolução de recursos](assets/disp-vanity-url/vanity-resource-resolver-entry.png "vanity-resource-resolver-entry")

No exemplo acima, quando solicitamos à instância do AEM que visite `/aboutus`, ele é resolvido como `/content/we-retail/us/en/about-us.html`

## Filtros de permissão automática do Dispatcher

O Dispatcher em um estado seguro filtra solicitações no caminho `/` por meio do Dispatcher, pois essa é a raiz da árvore do JCR.

É importante garantir que os editores só estejam permitindo o conteúdo do `/content` e outros caminhos seguros e assim por diante, e não caminhos como `/system`.

Aqui estão os urls personalizados na pasta base de `/`. Então, como permitimos que eles alcancem os editores enquanto permanecem seguros?

O Dispatcher simples tem um mecanismo de permissão de filtro automático e você precisa instalar um pacote do AEM e configurar o Dispatcher para apontar para essa página de pacote.

[https://experience.adobe.com/#/downloads/content/software-distribution/en/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/granite/vanityurls-components](https://experience.adobe.com/#/downloads/content/software-distribution/br/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/granite/vanityurls-components)

O Dispatcher tem uma seção de configuração em seu arquivo farm:

```
/vanity_urls { 
    /url    "/libs/granite/dispatcher/content/vanityUrls.html" 
    /file   "/tmp/vanity_urls" 
    /delay  300 
}
```

O parâmetro `/delay`, medido em segundos, não opera em uma base de intervalo fixo, mas em uma verificação baseada em condição. A Dispatcher avalia o carimbo de data e hora de modificação de `/file` (que armazena a lista de URLs personalizados reconhecidos) ao receber uma solicitação para uma URL não listada. O `/file` não será atualizado se a diferença de tempo entre o momento atual e a última modificação de `/file` for menor que a duração de `/delay`. A atualização de `/file` ocorre sob duas condições:

1. A solicitação de entrada é para uma URL não armazenada em cache ou listada em `/file`.
1. Pelo menos `/delay` segundos se passaram desde a última atualização de `/file`.

Esse mecanismo foi projetado para proteger contra ataques de Negação de serviço (DoS), que poderiam sobrecarregar o Dispatcher com solicitações, explorando o recurso de URLs personalizados.

Em termos mais simples, a `/file` contendo URLs personalizados é atualizada somente se uma solicitação chegar para uma URL que ainda não esteja em `/file` e se a última modificação de `/file` tiver ocorrido há mais tempo que o período `/delay`.

Para acionar explicitamente uma atualização do `/file`, você pode solicitar uma URL inexistente após verificar se o `/delay` tempo necessário passou desde a última atualização. Exemplos de URLs para essa finalidade incluem:

- `https://dispatcher-host-name.com/this-vanity-url-does-not-exist`
- `https://dispatcher-host-name.com/please-hand-me-that-planet-maestro`
- `https://dispatcher-host-name.com/random-vanity-url`

Essa abordagem força o Dispatcher a atualizar o `/file`, desde que o intervalo `/delay` especificado tenha decorrido desde a última modificação.

Ele armazena seu cache de resposta no argumento `/file`, portanto neste exemplo `/tmp/vanity_urls`

Portanto, se você visitar a instância do AEM no URI, verá o que ela encontra:

![captura de tela do conteúdo renderizado de /libs/granite/dispatcher/content/vanityUrls.html](assets/disp-vanity-url/vanity-url-component.png "vanity-url-component")

É literalmente uma lista, super simples

## Substituir regras como regras personalizadas

Por que mencionaríamos o uso de regras de regravação em vez do mecanismo padrão integrado ao AEM, como descrito acima?

Explicados de forma simples, problemas de namespace, desempenho e lógica de nível superior que podem ser melhor tratados.

Vamos ver um exemplo da entrada personalizada `/aboutus` para seu conteúdo `/content/we-retail/us/en/about-us.html` usando o módulo `mod_rewrite` do Apache.

```
RewriteRule ^/aboutus /content/we-retail/us/en/about-us.html [PT,L,NC]
```

Essa regra procura a personalização `/aboutus` e busca o caminho completo do renderizador com o sinalizador PT (Pass Through).

Ele também para de processar todas as outras regras do sinalizador L (Last), o que significa que não precisa atravessar uma enorme lista de regras, como a Resolução JCR precisa fazer.

Além de não ter que fazer o proxy da solicitação e esperar que o editor do AEM responda a esses dois elementos desse método, ele tem um desempenho muito maior.

Em seguida, a cereja do bolo aqui é o sinalizador NC (Sem distinção entre maiúsculas e minúsculas), o que significa que se um cliente digitar o URI com `/AboutUs` em vez de `/aboutus`, ele ainda funcionará.

Para criar uma regra de regravação para fazer isso, você criaria um arquivo de configuração no Dispatcher (exemplo: `/etc/httpd/conf.d/rewrites/examplevanity_rewrite.rules`) e o incluiria no arquivo `.vhost` que lida com o domínio que precisa dessas urls personalizadas para serem aplicadas.

Este é um trecho de código de exemplo de inclusão dentro de `/etc/httpd/conf.d/enabled_vhosts/we-retail.vhost`

```
<VirtualHost *:80> 
 ServerName weretail.com 
 ServerAlias www.weretail.com 
        ........ SNIP ........ 
 <IfModule mod_rewrite.c> 
  ReWriteEngine on 
  LogLevel warn rewrite:info 
  Include /etc/httpd/conf.d/rewrites/examplevanity_rewrite.rules 
 </IfModule> 
        ........ SNIP ........ 
</VirtualHost>
```

## Que método e onde

Usar o AEM para controlar entradas personalizadas tem os seguintes benefícios

- Os autores podem criá-los dinamicamente
- Eles vivem com o conteúdo e podem ser empacotados com o conteúdo

Usar `mod_rewrite` para controlar entradas personalizadas tem os seguintes benefícios

- Solução de conteúdo mais rápida
- Mais próximo da borda das solicitações de conteúdo do usuário final
- Mais extensibilidade e opções para controlar como o conteúdo é mapeado em outras condições
- Pode não diferenciar maiúsculas de minúsculas

Use ambos os métodos, mas aqui estão os conselhos e critérios que devem ser usados quando:

- Se a personalização for temporária e tiver níveis baixos de tráfego planejado, use o recurso integrado do AEM
- Se a personalização for um endpoint básico que não muda com frequência e tem uso frequente, use uma regra `mod_rewrite`.
- Se o namespace personalizado (por exemplo: `/aboutus`) precisa ser reutilizado para muitas marcas na mesma instância do AEM e depois usar as regras de regravação.

>[!NOTE]
>
>Se você quiser usar o recurso personalizado do AEM e evitar o namespace, é possível criar uma convenção de nomenclatura. Usando URLs personalizados aninhados como `/brand1/aboutus`, `brand2/aboutus`, `brand3/aboutus`.

[Próximo -> Logon comum](./common-logs.md)
