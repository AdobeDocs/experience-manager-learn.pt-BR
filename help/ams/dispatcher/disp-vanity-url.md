---
title: AEM recurso de URLs personalizadas do Dispatcher
description: Entenda como o AEM lida com URLs personalizados e técnicas adicionais usando regras de regravação para mapear o conteúdo mais próximo da borda do delivery.
version: 6.5
topic: Administration, Performance
feature: Dispatcher
role: Admin
level: Beginner
thumbnail: xx.jpg
source-git-commit: 04cd4002af7028ee9e3b1e1455b6346c56446245
workflow-type: tm+mt
source-wordcount: '994'
ht-degree: 4%

---


# URLs personalizadas do Dispatcher

[Índice](./overview.md)

[&lt;- Anterior: Liberação do Dispatcher](./disp-flushing.md)

## Visão geral

Este documento ajudará você a entender como o AEM lida com urls personalizados e algumas técnicas adicionais usando regras de regravação para mapear o conteúdo mais próximo da borda do delivery

## O que são URLs personalizadas

Quando você tem conteúdo em uma estrutura de pastas que faz sentido, ele nem sempre fica em um URL de fácil referência.  URLs personalizadas são como atalhos.  URLs menores ou exclusivos que fazem referência ao local em que o conteúdo real está.

Um exemplo: `/aboutus` pontiagudo `/content/we-retail/us/en/about-us.html`

Os autores do AEM têm a opção de definir propriedades de url personalizada em um conteúdo no AEM e publicá-lo.

Para que esse recurso funcione, é necessário ajustar os filtros do Dispatcher para permitir a personalização.  Isso não é razoável em relação ao ajuste dos arquivos de configuração do Dispatcher na taxa que os autores precisariam para configurar essas entradas de página personalizada.

Por esse motivo, o módulo Dispatcher tem um recurso para permitir automaticamente qualquer item listado como personalizado na árvore de conteúdo.


## Como funciona

### Criação de URLs personalizadas

O autor visita uma página em AEM e visita as propriedades da página e adiciona as entradas na seção url personalizado .

Depois que elas salvam as alterações e ativam a página, a personalização agora é atribuída a esta página.

#### Interface de toque:

![Menu suspenso de diálogo para a interface de criação de AEM na tela do editor do site](assets/disp-vanity-url/aem-page-properties-drop-down.png "aem-page-properties-drop-down")

![página de diálogo propriedades da página aem](assets/disp-vanity-url/aem-page-properties.png "aem-page-properties")

#### Localizador de conteúdo clássico:

![AEM propriedades da página de sidekick clássica do siteadmin](assets/disp-vanity-url/aem-page-properties-sidekick.png "aem-page-properties-sidekick")

![Caixa de diálogo Propriedades da página da interface clássica](assets/disp-vanity-url/aem-page-properties-classic.png "aem-page-properties-classic")

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Observação:</b>
Entenda que isso é muito propenso a nomear problemas de espaço.

Entradas personalizadas são globais para todas as páginas, esta é apenas uma das falhas que você precisa planejar para soluções alternativas que explicaremos mais tarde.
</div>

## Resolução/mapeamento de recursos

Cada entrada personalizada é uma entrada de mapa de sling para um redirecionamento interno.

Esses mapas são visíveis ao visitar o console Felix de instâncias AEM ( `/system/console/jcrresolver` )

Esta é uma captura de tela de uma entrada de mapa criada por uma entrada personalizada:
![captura de tela do console de uma entrada personalizada nas regras de resolução de recursos](assets/disp-vanity-url/vanity-resource-resolver-entry.png "vanity-resource-resolver-entry")

No exemplo acima, quando pedimos à instância de AEM para visitar `/aboutus` resolverá para `/content/we-retail/us/en/about-us.html`

## Filtros de permissão automática do Dispatcher

O Dispatcher em um estado seguro filtra solicitações no caminho `/` através do Dispatcher, pois essa é a raiz da árvore do JCR.

É importante garantir que os editores estejam permitindo somente o conteúdo da variável `/content` e outros caminhos seguros, etc.  e não caminhos como `/system` etc.

Aqui está a rolagem, os urls personalizados ativos na pasta base de `/` então, como permitimos que eles alcancem os editores enquanto permanecem seguros?

O Dispatcher Simples tem um mecanismo de permissão de filtro automático e você precisa instalar um pacote de AEM e depois configurar o Dispatcher para apontar para a página desses pacotes.

[https://experience.adobe.com/#/downloads/content/software-distribution/br/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/granite/vanityurls-components](https://experience.adobe.com/#/downloads/content/software-distribution/br/aem.html?package=/content/software-distribution/en/details.html/content/dam/aem/public/adobe/packages/granite/vanityurls-components)

O Dispatcher tem uma seção de configuração no arquivo farm:

```
/vanity_urls { 
    /url    "/libs/granite/dispatcher/content/vanityUrls.html" 
    /file   "/tmp/vanity_urls" 
    /delay  300 
}
```

Essa configuração informa ao Dispatcher para buscar esse URL da instância AEM que ele envia a cada 300 segundos para buscar a lista de itens que queremos permitir.

Ele armazena seu cache da resposta no `/file` argumento so neste exemplo `/tmp/vanity_urls`

Portanto, se você visitar a instância de AEM no URI, verá o que ela obtém:
![captura de tela do conteúdo renderizado de /libs/granite/dispatcher/content/vanityUrls.html](assets/disp-vanity-url/vanity-url-component.png "vanity-url-component")

É literalmente uma lista, super simples

## Reescrever regras como regras personalizadas

Por que mencionaríamos o uso de regras de regravação em vez do mecanismo padrão integrado no AEM, como descrito acima?

Explicados de forma simples, problemas de namespace, desempenho e lógica de nível superior que podem ser melhor tratados.

Vamos analisar um exemplo da entrada personalizada `/aboutus` ao conteúdo `/content/we-retail/us/en/about-us.html` uso do `mod_rewrite` para fazer isso.

```
RewriteRule ^/aboutus /content/we-retail/us/en/about-us.html [PT,L,NC]
```

Essa regra buscará a vaidade `/aboutus` e buscar o caminho completo do renderizador com o sinalizador PT (Pass Through).

Ele também deixará de processar todas as outras regras do sinalizador L (Last), o que significa que não terá que atravessar uma enorme lista de regras como a Resolução JCR tem que fazer.

Além de não ter que fazer o proxy da solicitação e esperar que o editor de AEM responda a esses dois elementos desse método, ele tem um desempenho muito maior.

Em seguida, a cereja do bolo aqui é o sinalizador NC (Sem distinção entre maiúsculas e minúsculas), o que significa que um cliente flui o URI com `/AboutUs` em vez de `/aboutus` ainda funcionará e permitirá a busca da página correta.

Para criar uma regra de regravação para fazer isso, você criaria um arquivo de configuração no Dispatcher (por exemplo: `/etc/httpd/conf.d/rewrites/examplevanity_rewrite.rules`) e inclua no `.vhost` arquivo que manipula o domínio que precisa desses urls personalizados para aplicar.

Este é um trecho de código de exemplo da inclusão dentro de `/etc/httpd/conf.d/enabled_vhosts/we-retail.vhost`

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

## Qual método e onde

Usar AEM para controlar entradas personalizadas tem os seguintes benefícios
- Os autores podem criá-los dinamicamente
- Eles vivem com o conteúdo e podem ser empacotados com o conteúdo

Usando `mod_rewrite` controlar entradas personalizadas tem os seguintes benefícios
- Solução de conteúdo mais rápida
- Mais próximo da borda das solicitações de conteúdo do usuário final
- Mais extensibilidade e opções para controlar como o conteúdo é mapeado em outras condições
- Pode não diferenciar maiúsculas de minúsculas

Use ambos os métodos, mas aqui estão os conselhos e critérios que devem ser usados quando:
- Se a personalização for temporária e tiver níveis baixos de tráfego planejado, use o recurso integrado de AEM
- Se a personalização for um endpoint básico que não muda com frequência e tem uso frequente, use um `mod_rewrite` regra.
- Se o namespace personalizado (por exemplo: `/aboutus`) deve ser reutilizada para muitas marcas na mesma instância do AEM e depois usar as regras de regravação.

<div style="color: #000;border-left: 6px solid #2196F3;background-color:#ddffff;"><b>Nota:</b>

Se quiser usar o recurso personalizado AEM e evitar o namespace, é possível criar uma convenção de nomenclatura.  Uso de urls personalizadas aninhadas como `/brand1/aboutus`, `brand2/aboutus`, `brand3/aboutus`.
</div>

[Próximo -> Registro comum](./common-logs.md)