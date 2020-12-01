---
title: Capítulo 3 - Tópicos avançados de cache
seo-title: Cache do Dispatcher AEM Desmistificado - Capítulo 3 - Tópicos de Cache Avançado
description: O Capítulo 3 do tutorial Desmistificado do Cache do Dispatcher AEM aborda como superar as limitações discutidas no Capítulo 2.
seo-description: O Capítulo 3 do tutorial Desmistificado do Cache do Dispatcher AEM aborda como superar as limitações discutidas no Capítulo 2.
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '6187'
ht-degree: 0%

---


# Capítulo 3 - Tópicos avançados de cache

*&quot;Há apenas duas coisas difíceis na Ciência da Computação: a invalidação do cache e os nomes dos itens.&quot;*

— PHIL KARLTON

## Visão geral

Esta é a Parte 3 de uma série de três partes para o armazenamento em AEM. Onde as duas primeiras partes se concentraram no cache http simples no Dispatcher e quais limitações existem. Esta parte discute algumas ideias sobre como superar essas limitações.

## Cache em geral

[O Capítulo 1](chapter-1.md) e o  [Capítulo 2 ](chapter-2.md) desta série incidiram principalmente sobre o Dispatcher. Explicámos as noções básicas, as limitações e onde é necessário fazer certas escolhas.

A complexidade e as complexidades do cache não são problemas exclusivos do Dispatcher. Cache é difícil em geral.

Ter o Dispatcher como sua única ferramenta em sua caixa de ferramentas seria na verdade uma limitação real.

Neste capítulo, queremos ampliar ainda mais a visualização sobre armazenamento em cache e desenvolver algumas ideias sobre como você pode superar algumas deficiências do Dispatcher. Não há nenhuma bala de prata - você terá que fazer compensações no seu projeto. Lembre-se, com precisão de armazenamento em cache e invalidação sempre vem complexidade, e com complexidade vem a possibilidade de erros.

Terão de fazer escolhas nestas áreas.

* Desempenho e latência
* Consumo de recursos / Carga da CPU / Uso do disco
* Precisão / Moeda / Estabilidade / Segurança
* Simplicidade / Complexidade / Custo / Manutenção / Prontidão de erro

Estas dimensões estão interligadas num sistema bastante complexo. Não há nada simples se-isto-então-aquilo. Tornar um sistema mais simples pode torná-lo mais rápido ou mais lento. Ele pode reduzir os custos de desenvolvimento, mas aumentar os custos no helpdesk, por exemplo, se os clientes visualizarem conteúdo obsoleto ou se queixarem de um site lento. Todos estes fatores têm de ser considerados e equilibrados entre si. Mas, neste momento, já deveríamos ter uma boa ideia, de que não há uma bala de prata ou uma única &quot;prática recomendada&quot; - apenas um monte de práticas ruins e algumas boas.

## Armazenamento em Cache Encadeado

### Visão geral

#### Fluxo de dados

Fornecer uma página de um servidor para o navegador de um cliente cruza uma grande variedade de sistemas e subsistemas. Se você olhar atentamente, há uma série de saltos que os dados precisam levar da fonte para o dreno, cada um dos quais é um potencial candidato para armazenamento em cache.

![Fluxo de dados de um aplicativo CMS típico](assets/chapter-3/data-flow-typical-cms-app.png)

*Fluxo de dados de um aplicativo CMS típico*

<br> 

Vamos start nossa jornada com um pedaço de dados que fica em um disco rígido e que precisa ser exibido em um navegador.

#### Hardware e sistema operacional

Primeiro, a própria unidade de disco rígido tem algum cache integrado no hardware. Segundo, o sistema operacional que monta o disco rígido usa memória gratuita para armazenar em cache blocos acessados com frequência para acelerar o acesso.

#### Repositório de conteúdo

O próximo nível é o CRX ou Oak - o banco de dados do documento usado pela AEM. O CRX e o Oak dividem os dados em segmentos que podem ser armazenados em cache na memória, bem como para evitar um acesso mais lento à unidade de disco rígido.

#### Dados de terceiros

A maioria das instalações da Web maiores também tem dados de terceiros; dados provenientes de um sistema de informações sobre produtos, de um sistema de gerenciamento de relação com o cliente, de um banco de dados herdado ou de qualquer outro serviço da Web arbitrário. Esses dados não precisam ser extraídos da fonte sempre que forem necessários - especialmente não, quando se sabe que mudam com pouca frequência. Portanto, ele pode ser armazenado em cache, se não for sincronizado no banco de dados do CRX.

#### Camada comercial - Aplicativo/Modelo

Geralmente, os scripts de modelo não renderizam o conteúdo bruto proveniente do CRX por meio da API JCR. Provavelmente você tem uma camada de negócios entre ela que une, calcula e/ou transforma dados em um objeto de domínio de negócios. Adivinhem - se essas operações são caras, vocês devem considerar armazená-las em cache.

#### Fragmentos de marcação

O modelo agora é a base para a renderização da marcação para um componente. Por que não armazenar em cache o modelo renderizado também?

#### Dispatcher, CDN e outros Proxies

Off (Desativado) vai para a página HTML renderizada no Dispatcher. Já discutimos que o objetivo principal do Dispatcher é armazenar em cache páginas HTML e outros recursos da Web (apesar do nome). Antes de os recursos chegarem ao navegador, ele pode passar por um proxy reverso - que pode armazenar em cache e um CDN - que também é usado para armazenar em cache. O cliente pode se sentar em um escritório, que concede acesso à Web somente por meio de um proxy - e esse proxy pode decidir armazenar em cache também para salvar o tráfego.

#### Cache do navegador

Por último, mas não menos importante, o navegador também armazena em cache. Este é um ativo facilmente negligenciado. Mas é o cache mais próximo e mais rápido que você tem na cadeia de cache. Infelizmente - não é compartilhado entre usuários - mas ainda entre solicitações diferentes de um usuário.

### Onde armazenar em cache e por que

Esta é uma longa cadeia de caches potenciais. E todos nós temos enfrentado problemas em que temos visto conteúdo desatualizado. Mas considerando quantos estágios existem, é um milagre que na maioria das vezes ele esteja funcionando.

Mas onde nessa cadeia faz sentido armazenar em cache? No começo? No fim? Em todo lugar? Depende... e depende de um grande número de fatores. Até dois recursos no mesmo site podem desejar uma resposta diferente para essa questão.

Para vos dar uma ideia aproximada dos fatores que podem levar em consideração.

**Tempo de vida**  - se os objetos tiverem um tempo de vida curto inerente (os dados de tráfego podem ter um tempo de vida útil menor do que os dados meteorológicos), talvez não valha a pena armazenar em cache.

**Custo de produção -** Quanto caro (em termos de ciclos de CPU e E/S) é a reprodução e o delivery de um objeto. Se for um caching barato pode não ser necessário.

**Tamanho**  - objetos grandes exigem mais recursos para serem armazenados em cache. Isso poderia ser um fator limitante e deve ser contrabalançado com os benefícios.

**Frequência**  de acesso - se os objetos forem acessados raramente, o cache pode não ser eficaz. Eles simplesmente ficariam obsoletos ou seriam invalidados antes de serem acessados pela segunda vez do cache. Tais itens apenas bloqueariam os recursos de memória.

**Acesso**  compartilhado - os dados usados por mais de uma entidade devem ser armazenados em cache mais acima da cadeia. Na verdade, a cadeia de caching não é uma cadeia, mas uma árvore. Um dado no repositório pode ser usado por mais de um modelo. Por sua vez, esses modelos podem ser usados por mais de um script de renderização para gerar fragmentos HTML. Esses fragmentos são incluídos em várias páginas que são distribuídas para vários usuários com seus caches privados no navegador. Então &quot;compartilhar&quot; não significa compartilhar apenas entre pessoas, mas entre softwares. Se você quiser encontrar um cache &quot;compartilhado&quot; em potencial, apenas rastreie a árvore até a raiz e localize um ancestral comum - é onde você deve armazenar em cache.

**Distribuição**  geoespacial - Se seus usuários estiverem distribuídos pelo mundo, o uso de uma rede distribuída de caches pode ajudar a reduzir a latência.

**Largura de banda e latência**  de rede - Falando de latência, quem são seus clientes e que tipo de rede eles estão usando? Talvez seus clientes sejam clientes móveis em um país subdesenvolvido usando conexão 3G de smartphones da geração mais antiga? Considere criar objetos menores e armazená-los em cache nos caches do navegador.

Esta lista de longe não é abrangente, mas pensamos que já percebemos a ideia.

### Regras básicas para armazenamento em cache encadeado

Mais uma vez - o cache é difícil. Vamos compartilhar algumas regras básicas, que extraímos de projetos anteriores que podem ajudar você a evitar problemas em seu projeto.

#### Evite o armazenamento em cache do Duplo

Cada uma das camadas introduzidas no último capítulo fornece algum valor na cadeia de armazenamento em cache. Ou economizando ciclos de computação ou aproximando os dados do consumidor. Não é errado armazenar um pedaço de dados em vários estágios da cadeia - mas você deve sempre considerar quais são os benefícios e os custos do próximo estágio. O armazenamento em cache de uma página inteira no sistema de publicação geralmente não oferece nenhum benefício, pois isso já é feito no Dispatcher.

#### Misturar estratégias de invalidação

Há três estratégias básicas de invalidação:

* **TTL, Tempo de vida:** um objeto expira após uma quantidade fixa de tempo (por exemplo, &quot;2 horas a partir de agora&quot;)
* **Data de expiração:** o objeto expira em um horário definido no futuro (por exemplo, &quot;17:00 PM em 10 de junho de 2019&quot;)
* **Baseado em eventos:** o objeto é invalidado explicitamente por um evento que ocorreu na plataforma (por exemplo, quando uma página é alterada e ativada)

Agora você pode usar estratégias diferentes em diferentes camadas de cache, mas há algumas &quot;tóxicas&quot;.

#### Invalidação Baseada Em evento

![Invalidação com base em Evento puro](assets/chapter-3/event-based-invalidation.png)

*Invalidação pura baseada em Eventos: Invalidar do cache interno para a camada externa*

<br> 

A invalidação pura baseada em eventos é a mais fácil de compreender, a mais fácil de se tornar teoricamente correta e a mais precisa.

Em termos simples, os caches são invalidados um por um após a alteração do objeto.

Você só precisa manter uma regra em mente:

Sempre invalidar de dentro para fora do cache. Se você invalidou um cache externo primeiro, ele pode fazer o cache novamente de conteúdo obsoleto de um cache interno. Não faça pressuposições de quando o cache estiver atualizado novamente - verifique se ele está. Melhor, acionando a invalidação do cache externo _depois de_ invalidar o cache interno.

Essa é a teoria. Mas, na prática, há um número de armadilhas. Os eventos devem ser distribuídos - possivelmente por uma rede. Na prática, isto torna mais difícil a implementação do sistema de invalidação.

#### Auto - Recuperação

Com a invalidação baseada em eventos, você deve ter um plano de contingência. E se um evento de invalidação for perdido? Uma estratégia simples poderia ser invalidar ou expurgar após um determinado período de tempo. Então - você pode ter perdido esse evento e agora servir conteúdo obsoleto. Mas seus objetos também têm um TTL implícito de várias horas (dias) apenas. Então eventualmente o sistema se cura automaticamente.

#### Invalidação pura baseada em TTL

![Invalidação não sincronizada com base em TTL](assets/chapter-3/ttl-based-invalidation.png)

*Invalidação não sincronizada com base em TTL*

<br> 

Este também é um regime bastante comum. Você empilha várias camadas de caches, cada uma com direito de servir um objeto por um determinado período.

É fácil de implementar. Infelizmente, é difícil prever o tempo de vida efetivo de um dado.

![Chace externo que prolonga a duração de um objeto interno](assets/chapter-3/outer-cache.png)

*Cache externo que prolonga a duração de um objeto interno*

<br> 

Considere a ilustração acima. Cada camada de cache apresenta um TTL de 2 minutos. Agora - o TTL global precisa de 2 minutos também, certo? Nem por isso. Se a camada externa buscar o objeto antes de ele ficar obsoleto, a camada externa realmente prolonga o tempo de vida efetivo do objeto. O tempo de vida efetivo pode estar entre 2 e 4 minutos nesse caso. Considere que você concordou com seu departamento de negócios, um dia é tolerável - e você tem quatro camadas de caches. O TTL real em cada camada não deve exceder seis horas... aumentando a taxa de erros do cache...

Não estamos a dizer que seja um mau esquema. Você só deveria saber seus limites. E é uma estratégia boa e fácil de se start. Somente se o tráfego do site aumentar, você poderá considerar uma estratégia mais precisa.

*Sincronizando o tempo de invalidação definindo uma data específica*

#### Data de expiração com base na anulação

Você obtém um tempo de vida efetivo mais previsível, se estiver configurando uma data específica no objeto interno e propagando isso para os caches externos.

![Sincronizando datas de expiração](assets/chapter-3/synchronize-expiration-dates.png)

*Sincronizando datas de expiração*

<br> 

No entanto, nem todos os caches podem propagar as datas. E pode se tornar desagradável, quando o cache externo agregação dois objetos internos com datas de expiração diferentes.

#### Combinação de invalidação baseada em Eventos e baseada em TTL

![Combinação de estratégias baseadas em eventos e baseadas em TTL](assets/chapter-3/mixing-event-ttl-strategies.png)

*Combinação de estratégias baseadas em eventos e baseadas em TTL*

<br> 

Um esquema comum no mundo AEM é usar a invalidação baseada em eventos nos caches internos (por exemplo, caches em memória onde eventos podem ser processados em tempo quase real) e caches baseados em TTL externos - onde talvez você não tenha acesso a invalidação explícita.

No mundo AEM você teria um cache na memória para objetos de negócios e fragmentos HTML nos sistemas de publicação, que é invalidado quando os recursos subjacentes mudam e você propaga esse evento de alteração para o dispatcher, que também funciona com base em eventos. Na frente disso você teria, por exemplo, um CDN baseado em TTL.

Ter uma camada de armazenamento em cache (curto) baseado em TTL na frente de um Dispatcher poderia, efetivamente, atenuar um pico que normalmente ocorria após uma invalidação automática.

#### Mistura de TTL - e anulação baseada em Eventos

![Mistura de TTL - e invalidação baseada em eventos](assets/chapter-3/toxic.png)

*Tóxico: Mistura de TTL - e invalidação baseada em eventos*

<br> 

Esta combinação é tóxica. Nunca coloque nem coloque o cache baseado em evento depois de um cache baseado em TTL ou Expiry. Lembram-se do efeito de repercussão que tivemos na estratégia &quot;TTL puro&quot;? O mesmo efeito pode ser observado aqui. Somente que o evento de invalidação do cache externo já tenha ocorrido pode não ocorrer novamente - isso pode expandir o tempo de vida do objeto em cache para infinito.

![Combinados baseados em TTL e em eventos: Transmissão ao infinito](assets/chapter-3/infinity.png)

*Combinados baseados em TTL e em eventos: Transmissão ao infinito*

<br> 

## Cache parcial e armazenamento em cache na memória

É possível conectar-se ao estágio do processo de renderização para adicionar camadas de cache. Da obtenção de objetos de transferência remota de dados ou da criação de objetos comerciais locais ao armazenamento em cache da marcação renderizada de um único componente. Deixaremos implementações concretas para um tutorial posterior. Mas talvez você queira já ter implementado algumas dessas camadas de armazenamento em cache. Portanto, o mínimo que podemos fazer aqui é introduzir os princípios básicos - e cegos.

### Palavras de aviso

#### Respeitar Controle de acesso

As técnicas descritas aqui são bastante poderosas e _devem ter_ em cada caixa de ferramentas do desenvolvedor AEM. Mas não se empolgue muito, use-os sabiamente. Armazenar um objeto em um cache e compartilhá-lo com outros usuários em solicitações de acompanhamento significa contornar o controle de acesso. Isso geralmente não é um problema em sites voltados para o público, mas pode ser, quando um usuário precisa fazer logon antes de obter acesso.

Considere armazenar uma marcação HTML do menu principal do site em um cache na memória para compartilhá-la entre várias páginas. Na verdade, esse é um exemplo perfeito para armazenar HTML parcialmente renderizado, já que a criação de uma navegação geralmente é cara, pois requer a passagem de muitas páginas.

Você não está compartilhando a mesma estrutura de menu entre todas as páginas, mas também com todos os usuários, o que a torna ainda mais eficiente. Mas espere... mas talvez haja alguns itens no menu que são reservados apenas para um determinado grupo de usuários. Nesse caso, o armazenamento em cache pode tornar-se um pouco mais complexo.

#### Armazenar em cache somente objetos comerciais personalizados

Se algum - esse é o conselho mais importante, podemos dar a você:

>[!WARNING]
>
>Apenas armazene em cache objetos que são seus, que são imutáveis, que você mesmo construiu, que são superficiais e não têm nenhuma referência de saída.

O que isso significa?

1. Você não sabe sobre o ciclo de vida pretendido dos objetos de outras pessoas. Considere que você obtém uma referência a um objeto de solicitação e decida armazená-lo em cache. Agora, a solicitação acabou, e o container de servlet quer reciclar esse objeto para a próxima solicitação de entrada. Nesse caso, outra pessoa está mudando o conteúdo sobre o qual você pensava que tinha controle exclusivo. Não ignore isso - vimos algo assim acontecendo em um projeto. Os clientes estavam vendo outros dados de clientes em vez de seus próprios.

2. Desde que um objeto seja referenciado por uma cadeia de outras referências, ele não poderá ser removido do heap. Se você retiver um objeto supostamente pequeno em seu cache que faz referência, digamos que uma representação de 4 MB de uma imagem você terá uma boa chance de ter problemas com o vazamento de memória. Os caches deveriam se basear em referências fracas. Mas as referências fracas não funcionam como é de esperar. Essa é a melhor maneira de produzir um vazamento de memória e terminar com um erro de falta de memória. E - você não sabe o tamanho da memória retida dos objetos estranhos, certo?

3. Especialmente no Sling, você pode adaptar (quase) cada objeto um ao outro. Considere colocar um recurso no cache. A próxima solicitação (com direitos de acesso diferentes) obtém esse recurso e o adapta em um resourceResolver ou uma sessão para acessar outros recursos aos quais ele não teria acesso.

4. Mesmo que você crie um &quot;invólucro&quot; fino ao redor de um recurso da AEM, não deverá armazená-lo em cache - mesmo que seja seu próprio e imutável. O objeto encapsulado seria uma referência (que nós proibimos antes) e se olharmos nítidos, isso basicamente cria os mesmos problemas descritos no último item.

5. Se desejar armazenar em cache, crie seus próprios objetos copiando dados primitivos em seus próprios objetos shallo. Você pode querer vincular entre seus próprios objetos por referências - por exemplo, você pode querer armazenar em cache uma árvore de objetos. Isso é bom - mas apenas armazena em cache objetos que você acabou de criar na mesma solicitação - e nenhum objeto que foi solicitado de outro lugar (mesmo se for o espaço de nome do objeto &#39;seu&#39;). _Copiar_ objetos é a chave. E certifique-se de limpar toda a estrutura de objetos vinculados de uma só vez e evitar referências de entrada e saída à sua estrutura.

6. Sim - e mantenha seus objetos imutáveis. Propriedades privadas, somente e sem setters.

São muitas regras, mas vale a pena segui-las. Mesmo se você for experiente e super inteligente e tiver tudo sob controle. O jovem colega do seu projeto acabou de se formar na universidade. Ele não sabe de todas essas armadilhas. Se não há armadilhas, não há nada para evitar. Seja simples e compreensível.

### Ferramentas e bibliotecas

Esta série é sobre entender conceitos e capacitar você a construir uma arquitetura que melhor se ajuste ao seu caso de uso.

Não estamos a promover nenhum instrumento em particular. Mas deem-vos pistas sobre como avaliá-las. Por exemplo, AEM tem um cache integrado simples com um TTL fixo desde a versão 6.0. Você deve usá-lo? Provavelmente não na publicação em que um cache baseado em eventos segue na cadeia (dica: O Dispatcher). Mas pode ser por uma escolha decente para um Autor. Há também um cache HTTP por Adobe ACS comuns que pode valer a pena ser considerado.

Ou você cria seu próprio, com base em uma estrutura de cache madura, como [Ehcache](https://www.ehcache.org). Isso pode ser usado para armazenar em cache objetos Java e marcação renderizada (`String` objetos).

Em alguns casos simples, você também pode se dar bem com o uso de mapas de hash simultâneos - você verá rapidamente limites aqui - na ferramenta ou em suas habilidades. A simultaneidade é tão difícil de ser principal quanto nomear e armazenar em cache.

#### Referências

* [Cache http comum ACS  ](https://adobe-consulting-services.github.io/acs-aem-commons/features/http-cache/index.html)
* [Estrutura de cache do Ehcache](https://www.ehcache.org)

### Termos básicos

Não vamos entrar na teoria do caching muito fundo aqui, mas nos sentimos obrigados a fornecer algumas palavras-burro, para que você tenha um bom start.

#### Eliminação de Cache

Nós falamos sobre invalidação e purga muito. _A_ remoção de cache está relacionada aos seguintes termos: Depois de uma entrada, ela é despejada, não está mais disponível. Mas o despejo acontece não quando uma entrada está desatualizada, mas quando o cache está cheio. Itens mais recentes ou &quot;mais importantes&quot; empurram itens mais antigos ou menos importantes para fora do cache. Quais entradas você terá que sacrificar é uma decisão caso a caso. Você pode querer despejar os mais antigos ou aqueles que foram usados muito raramente ou acessados por muito tempo.

#### Cache preventivo

Cache preventivo significa recriar a entrada com conteúdo novo no momento em que é invalidada ou considerada desatualizada. É claro - fá-lo-ia apenas com alguns recursos, que tem a certeza de que é acessado com frequência e de imediato. Caso contrário, você desperdiçaria recursos ao criar entradas de cache que talvez nunca sejam solicitadas. Ao criar entradas de cache de forma preventiva, você pode reduzir a latência da primeira solicitação para um recurso após a invalidação do cache.

#### Aquecimento do cache

O aquecimento do cache está intimamente relacionado ao cache preventivo. Embora você não use esse termo para um sistema ao vivo. E é menos tempo limitado do que o primeiro. Você não faz o cache novamente imediatamente após a invalidação, mas preenche o cache gradualmente quando o tempo permitir.

Por exemplo, você retira um trecho Publicar / Dispatcher do balanceador de carga para atualizá-lo. Antes de reintegrá-lo, você rastreia automaticamente as páginas acessadas com mais frequência para colocá-las no cache novamente. Quando o cache estiver &quot;quente&quot; - preenchido adequadamente, você reintegrará a perna ao balanceador de carga.

Ou talvez você reintegre a perna de uma só vez, mas você diminui o tráfego para aquela perna para que ela tenha a chance de aquecer os caches por uso regular.

Ou talvez você também queira armazenar em cache algumas páginas acessadas com menos frequência em horários em que seu sistema está ocioso para diminuir a latência quando elas são acessadas por solicitações reais.

#### Identidade do objeto de cache, carga, dependência de invalidação e TTL

De modo geral, um objeto em cache ou uma &quot;entrada&quot; tem cinco propriedades principais,

#### Chave

Essa é a propriedade pela qual você identifica e objetos. Para recuperar a carga ou expurgá-la do cache. O dispatcher, por exemplo, usa o URL de uma página como a chave. Observe que o dispatcher não usa os caminhos de páginas. Isso não é suficiente para diferenciar renderizações diferentes. Outros caches podem usar chaves diferentes. Veremos alguns exemplos mais tarde.

#### Valor / Carga

Esse é o tesouro do objeto, os dados que você deseja recuperar. No caso do dispatcher, é o conteúdo dos arquivos. Mas também pode ser uma árvore de objetos Java.

#### TTL

Já cobrimos o TTL. A hora após a qual uma entrada é considerada obsoleta e não deve ser mais entregue.

#### Dependência

Isso está relacionado à invalidação baseada em eventos. Quais dados originais dependem desse objeto? Na Parte I, já dissemos, que um verdadeiro e preciso acompanhamento de dependência é demasiado complexo. Mas com nosso conhecimento do sistema você pode aproximar as dependências com um modelo mais simples. Invalidamos objetos suficientes para eliminar conteúdo obsoleto... e talvez inadvertidamente mais do que seria necessário. Mas ainda assim tentamos manter abaixo de &quot;expurgar tudo&quot;.

Os objetos que dependem dos outros são genuínos em cada aplicativo. Daremos alguns exemplos de como implementar uma estratégia de dependência posteriormente.

### Cache de fragmentos HTML

![Reuso de um fragmento renderizado em páginas diferentes](assets/chapter-3/re-using-rendered-fragment.png)

*Reuso de um fragmento renderizado em páginas diferentes*

<br> 

O HTML Fragment Caching é uma ferramenta poderosa. A ideia é armazenar em cache a marcação HTML gerada por um componente em um cache na memória. Vocês podem perguntar, por que eu deveria fazer isso? Estou armazenando em cache a marcação da página inteira no dispatcher de qualquer forma - incluindo a marcação do componente. Nós concordamos. Você faz - mas uma vez por página. Você não está compartilhando essa marcação entre as páginas.

Imagine, você está renderizando uma navegação na parte superior de cada página. A marcação tem a mesma aparência em cada página. Mas você está renderizando repetidamente para cada página, isso não está no Dispatcher. E lembre-se: Após a invalidação automática, todas as páginas precisam ser renderizadas novamente. Basicamente, você está executando o mesmo código com os mesmos resultados centenas de vezes.

De nossa experiência, renderizar uma navegação superior aninhada é uma tarefa muito cara. Geralmente, você atravessa uma boa parte da árvore de documentos para gerar os itens de navegação. Mesmo que você só precise do título de navegação e do URL, as páginas precisam ser carregadas na memória. E aqui eles estão entupindo recursos preciosos. Uma e outra vez.

Mas o componente é compartilhado entre muitas páginas. E compartilhar algo é um sinal de usar um cache. Portanto - o que você deseja fazer é verificar se o componente de navegação já foi renderizado e armazenado em cache e, em vez de renderizar novamente, emitir apenas o valor de caches.

Há duas maravilhosas sutilezas daquele esquema que facilmente perdem.

1. Você está armazenando uma sequência de caracteres Java em cache. Uma string não tem nenhuma referência de saída e é imutável. Portanto, considerando os avisos acima - isso é super seguro.

2. A invalidação também é muito fácil. Sempre que algo alterar seu site, você deseja invalidar essa entrada de cache. A reconstrução é relativamente barata, uma vez que precisa de ser executada apenas uma vez e depois é reutilizada por todas as centenas de páginas.

Isso é um grande alívio para seus servidores de publicação.

### Implementação de caches de fragmentos

#### Tags personalizadas

Antigamente, quando você usava o JSP como um mecanismo de formatação, era bastante comum usar uma tag JSP personalizada envolvida ao redor do código de renderização dos componentes.

```
<!-- Pseudo Code -->

<myapp:cache
  key=' ${info.homePagePath} + ${component.path}'
  cache='main-navigation'
  dependency='${info.homePagePath}'>

… original components code ..

</myapp:cache>
```

A tag personalizada do que capturaria seu corpo e o gravaria no cache ou impediria a execução de seu corpo e, em vez disso, produziria a carga da entrada do cache.

A &quot;Chave&quot; é o caminho de componentes que ela teria na página inicial. Não usamos o caminho do componente na página atual, pois isso criaria uma entrada de cache por página - o que contradizeria nossa intenção de compartilhar esse componente. Também não estamos usando apenas o caminho relativo dos componentes (`jcr:conten/mainnavigation`), pois isso impediria o uso de diferentes componentes de navegação em sites diferentes.

&quot;Cache&quot; é um indicador onde armazenar a entrada. Geralmente, você tem mais de um cache no qual armazena itens. Cada um deles pode se comportar de forma um pouco diferente. Portanto, é bom diferenciar o que está armazenado - mesmo que no final seja apenas cordas.

&quot;Dependência&quot; é isso que a entrada do cache depende. O cache de &quot;navegação principal&quot; pode ter uma regra, que se houver alguma alteração abaixo do nó &quot;dependência&quot;, a entrada de acordo deverá ser removida. Portanto - sua implementação de cache precisaria se registrar como um ouvinte de eventos no repositório para ter conhecimento das alterações e aplicar as regras específicas do cache para descobrir o que precisa ser invalidado.

O acima foi apenas um exemplo. Você também pode escolher ter uma árvore de caches. Quando o primeiro nível é usado para separar sites (ou locatários) e o segundo nível, ele se divide em tipos de conteúdo (por exemplo, &quot;navegação principal&quot;) - isso pode sobressaltar a adição do caminho de homes page, como no exemplo acima.

Aliás, você também pode usar essa abordagem com componentes baseados em HTL mais modernos. Em seguida, você teria um empacotador JSP ao redor do seu script HTL.

#### Filtros de componentes

Mas em uma abordagem HTL pura, é melhor criar o cache de fragmentos com um filtro de componente Sling. Nós ainda não vimos isso na natureza, mas essa é a abordagem que nós adotaríamos para esse problema.

#### Inclusão dinâmica do Sling

O cache de fragmentos será usado se você tiver algo constante (a navegação) no contexto de um ambiente em mudança (páginas diferentes).

Mas você também pode ter o oposto, um contexto relativamente constante (uma página que raramente muda) e alguns fragmentos que mudam constantemente nessa página (por exemplo, um ticker ao vivo).

Nesse caso, você pode dar uma chance a [Sling Dynamic Includes](https://sling.apache.org/documentation/bundles/dynamic-includes.html). Em essência, este é um filtro de componente, que envolve o componente dinâmico e, em vez de renderizar o componente na página, cria uma referência. Essa referência pode ser uma chamada Ajax, de modo que o componente seja incluído pelo navegador e, portanto, a página adjacente possa ser armazenada em cache estaticamente. Ou - alternativamente - a inclusão dinâmica de sling pode gerar uma diretiva SSI (Inclusão do lado do servidor). Esta diretiva seria executada no servidor Apache. Você pode até mesmo usar as diretivas ESI - Edge Side Include se você aproveitar o Varnish ou um CDN compatível com scripts ESI.

![Diagrama de sequência de uma solicitação usando a inclusão dinâmica Sling](assets/chapter-3/sequence-diagram-sling-dynamic-include.png)

*Diagrama de sequência de uma solicitação usando a inclusão dinâmica Sling*

<br> 

A documentação do SDI informa que você deve desativar o cache para URLs que terminam em &quot;*.nocache.html&quot;, o que faz sentido - já que você está lidando com componentes dinâmicos.

Você pode ver outra opção como usar o SDI: Se você _não_ desativar o cache do dispatcher para as inclusões, o Dispatcher atuará como um cache de fragmento semelhante ao que descrevemos no último capítulo: As páginas e os fragmentos de componente são armazenados em cache no dispatcher e agrupados pelo script SSI no servidor Apache quando a página é solicitada. Ao fazer isso, você pode implementar componentes compartilhados como a navegação principal (visto que você sempre usa o mesmo URL do componente).

Isso deveria funcionar - em teoria. Mas...

Aconselhamos a não o fazer: Você perderia a capacidade de ignorar o cache para os componentes dinâmicos reais. O SDI é configurado globalmente e as alterações que você faria para o seu &quot;cache-mans-fragment-cache&quot; também se aplicariam aos componentes dinâmicos.

Aconselhamos você a estudar cuidadosamente a documentação do SDI. Existem outras limitações, mas o SDI é uma ferramenta valiosa em alguns casos.

#### Referências

* [docs.oracle.com - Como gravar tags JSP personalizadas](https://docs.oracle.com/cd/E11035_01/wls100/taglib/quickstart.html)
* [Dominik Süß - Criação e uso de filtros de componentes](https://www.slideshare.net/connectwebex/prsentation-dominik-suess)
* [sling.apache.org - O Sling Dynamic Inclui](https://sling.apache.org/documentation/bundles/dynamic-includes.html)
* [helpx.adobe.com - Configuração de inclusão dinâmica Sling em AEM](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/sling-dynamic-include-technical-video-setup.html)


#### Cache de modelos

![Cache baseado em modelo: Um objeto comercial com duas renderizações diferentes](assets/chapter-3/model-based-caching.png)

*Cache baseado em modelo: Um objeto comercial com duas renderizações diferentes*

<br> 

Voltemos ao caso com a navegação. Estávamos assumindo que cada página exigiria a mesma marcação da navegação.

Mas talvez não seja esse o caso. Talvez você queira renderizar uma marcação diferente para o item na navegação que representa a _página atual_.

```
Travel Destinations

<ul class="maninnav">
  <li class="currentPage">Travel Destinations
    <ul>
      <li>Finland
      <li>Canada
      <li>Norway
    </ul>
  <li>News
  <li>About us
<ul>
```

```
News

<ul class="maninnav">
  <li>Travel Destinations
  <li class="currentPage">News
    <ul>
      <li>Winter is coming>
      <li>Calm down in the wild
    </ul>
  <li>About us
<is
```

Estas são duas representações completamente diferentes. Ainda assim, o _objeto de negócios_ - a árvore de navegação completa - é o mesmo.  O objeto _business_ aqui seria um gráfico de objetos que representa os nós na árvore. Esse gráfico pode ser facilmente armazenado em um cache na memória. No entanto, lembre-se de que esse gráfico não deve conter nenhum objeto ou fazer referência a qualquer objeto que você mesmo não criou - especialmente agora nós JCR.

#### Armazenamento em cache no navegador

Já tocamos na importância do cache no navegador, e há muitos bons tutoriais por aí. No fim - para o navegador - o Dispatcher é apenas um servidor Web que segue o protocolo HTTP.

No entanto - apesar da teoria - reunimos alguns conhecimentos que não encontrámos em outro lugar e que queremos partilhar.

Em essência, o cache do navegador pode ser aproveitado de duas maneiras diferentes.

1. O navegador tem um recurso em cache do qual sabe a data exata de expiração. Nesse caso, não solicita o recurso novamente.

2. O navegador tem um recurso, mas não tem certeza se ainda é válido. Nesse caso, perguntaria ao servidor Web (o Dispatcher, no nosso caso). Dê-me o recurso se ele foi modificado desde a última vez que você o entregou. Se não mudou, o servidor responde com &quot;304 - não alterado&quot; e somente os metadados foram transmitidos.

#### Depuração

Se você estiver otimizando as configurações do Dispatcher para o armazenamento em cache do navegador, é extremamente útil usar um servidor proxy de desktop entre o navegador e o servidor Web. Nós preferimos &quot;Charles Web Debugging Proxy&quot; de Karl von Randow.

Usando o Charles, você pode ler as solicitações e respostas, que são transmitidas para e do servidor. E você pode aprender muito sobre o protocolo HTTP. Navegadores modernos também ofertas alguns recursos de depuração, mas os recursos de um proxy de desktop são inéditos. É possível manipular os dados transferidos, limitar a transmissão, repetir solicitações únicas e muito mais. E a interface do usuário é claramente organizada e bastante abrangente.

O teste mais básico é usar o site como um usuário normal - com o proxy no intervalo - e verificar no proxy se o número de solicitações estáticas (para /etc/...) está ficando menor ao longo do tempo - pois elas devem estar no cache e não devem mais ser solicitadas.

Encontramos, um proxy pode fornecer uma visão geral mais clara, já que a solicitação em cache não aparece no log, enquanto alguns depuradores integrados do navegador ainda mostram essas solicitações com &quot;0 ms&quot; ou &quot;de disco&quot;. O que é correto e preciso, mas poderia enevoar sua visualização um pouco.

Em seguida, você pode detalhar e verificar os cabeçalhos dos arquivos transferidos para ver, por exemplo, se os cabeçalhos http &quot;Expira&quot; estão corretos. É possível reproduzir solicitações com cabeçalhos if-modified-Since definidos para ver se o servidor responde corretamente com um código de resposta 304 ou 200. Você pode observar o tempo de chamadas assíncronas e também pode testar suas premissas de segurança em um certo grau. Lembram-se de termos dito para você não aceitar todos os seletores que não são explicitamente esperados? Aqui você pode reproduzir com o URL e os parâmetros e ver se o aplicativo se comporta bem.

Há apenas uma coisa que pedimos que você não faça, quando estiver depurando seu cache:

Não recarregue as páginas no navegador!

Um &quot;recarregamento do navegador&quot;, um _simples-reload_, bem como um _forçado-reload_ (&quot;_shift-reload_&quot;) não é o mesmo que uma solicitação de página normal. Uma solicitação de recarregamento simples define um cabeçalho

```
Cache-Control: max-age=0
```

E uma Shift-Recarregar (mantendo pressionada a tecla &quot;Shift&quot; enquanto clica no botão de recarregamento) normalmente define um cabeçalho de solicitação

```
Cache-Control: no-cache
```

Ambos os cabeçalhos têm efeitos semelhantes, mas ligeiramente diferentes - mas, o mais importante, diferem completamente de uma solicitação normal quando você abre um URL do slot do URL ou usando links no site. A navegação normal não define cabeçalhos Cache-Control, mas provavelmente um cabeçalho if-modified-Since.

Portanto, se você deseja depurar o comportamento de navegação normal, faça exatamente isso: _Navegue normalmente_. O uso do botão de recarregamento do seu navegador é a melhor maneira de não ver erros de configuração de cache na sua configuração.

Use seu proxy Charles para ver do que estamos falando. Sim - e enquanto estiver aberto - você pode repetir as solicitações bem ali. Não é necessário recarregar a partir do navegador.

## Teste de desempenho

Ao usar um proxy, você tem uma ideia do comportamento de tempo das páginas. Claro que isso não é de longe um teste de desempenho.  Um teste de desempenho exigiria que vários clientes solicitassem suas páginas em paralelo.

Um erro comum, que vimos com muita frequência, é que o teste de desempenho inclui apenas um número muito pequeno de páginas e essas páginas são entregues apenas do cache do Dispatcher.

Se você estiver promovendo seu aplicativo para o sistema ativo, a carga será completamente diferente do que você testou.

No sistema ativo, o padrão de acesso não é aquele pequeno número de páginas igualmente distribuídas que você tem em testes (home page e poucas páginas de conteúdo). O número de páginas é muito maior e as solicitações são distribuídas de forma muito desigual. E - claro - as páginas ao vivo não podem ser servidas 100% do cache: Há solicitações de invalidação provenientes do sistema de publicação que estão invalidando automaticamente uma grande parte dos seus recursos preciosos.

Ah sim - e quando você estiver reconstruindo seu cache do Dispatcher, você descobrirá que o sistema de publicação também se comporta de forma bastante diferente, dependendo se você solicitar apenas algumas páginas - ou um número maior. Mesmo se todas as páginas forem complexas da mesma forma - seu número desempenha um papel. Lembram-se do que dissemos sobre o caching acorrentado? Se você sempre solicitar o mesmo número pequeno de páginas, provavelmente não haverá problemas, os blocos correspondentes aos dados brutos estarão no cache dos discos rígidos ou os blocos serão armazenados em cache pelo sistema operacional. Além disso, há uma boa chance de que o Repositório tenha armazenado em cache o segmento de acordo na memória principal. Assim, renderizar novamente é significativamente mais rápido do que quando outras páginas se removem agora e depois de vários caches.

O armazenamento em cache é difícil, assim como o teste de um sistema que depende do armazenamento em cache. Então, o que você pode fazer para ter um cenário de vida real mais preciso?

Pensamos que você teria que realizar mais de um teste e teria que fornecer mais de um índice de desempenho como medida da qualidade da sua solução.

Se você já tiver um site existente, meça o número de solicitações e como elas são distribuídas. Tente modelar um teste que use uma distribuição semelhante de solicitações. Adicionar alguma aleatoriedade não poderia doer. Você não precisa simular um navegador que carregaria recursos estáticos como JS e CSS - esses não importam realmente. Eles são armazenados em cache no navegador ou no Dispatcher eventualmente e não se somam significativamente à carga. Mas as imagens referenciadas importam mesmo. Encontre sua distribuição em arquivos de registro antigos e modele um padrão de solicitação semelhante.

Agora, realize um teste com o Dispatcher que não está em cache. Esse é o seu pior cenário. Descubra em qual carga máxima seu sistema está se tornando instável sob essas piores condições. Você também pode piorá-lo retirando algumas pernas do Dispatcher/Publish, se desejar.

Em seguida, realize o mesmo teste com todas as configurações de cache necessárias para &quot;ligado&quot;. Aumente lentamente suas solicitações paralelas para aquecer o cache e ver quanto seu sistema pode levar sob essas melhores condições de caso.

Um cenário de caso médio seria executar o teste com o Dispatcher ativado, mas também com algumas invalidações acontecendo. É possível simular isso tocando nos arquivos de status por um cronjob ou enviando as solicitações de invalidação em intervalos irregulares para o Dispatcher. Não se esqueça de expurgar alguns dos recursos não validados automaticamente de vez em quando.

Você pode variar o último cenário aumentando as solicitações de invalidação e aumentando a carga.

Isso é um pouco mais complexo do que um teste de carga linear - mas dá muito mais confiança na sua solução.

Você pode se afastar do esforço. Mas, pelo menos, realize um teste em caso pior no sistema de publicação com um número maior de páginas (igualmente distribuídas) para ver os limites do sistema. Certifique-se de interpretar o número do melhor cenário e fornecer espaço suficiente para os seus sistemas.