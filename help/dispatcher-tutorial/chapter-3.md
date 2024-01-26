---
title: Capítulo 3 - Tópicos avançados de armazenamento em cache do Dispatcher
description: Esta é a Parte 3 de uma série de três partes do armazenamento em cache no AEM. Onde as duas primeiras partes se concentraram no armazenamento em cache http simples no Dispatcher e quais limitações existem. Esta parte discute algumas ideias sobre como superar essas limitações.
feature: Dispatcher
topic: Architecture
role: Architect
level: Intermediate
doc-type: Tutorial
exl-id: 7c7df08d-02a7-4548-96c0-98e27bcbc49b
duration: 1653
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '6172'
ht-degree: 0%

---

# Capítulo 3 - Tópicos avançados de armazenamento em cache

*&quot;Só há duas coisas difíceis na Ciência da Computação: invalidação de cache e nomear coisas.&quot;*

— PHIL KARLTON

## Visão geral

Esta é a Parte 3 de uma série de três partes para armazenamento em cache no AEM. Onde as duas primeiras partes se concentraram no armazenamento em cache http simples no Dispatcher e quais limitações existem. Esta parte discute algumas ideias sobre como superar essas limitações.

## Armazenamento em cache em geral

[Capítulo 1](chapter-1.md) e [Capítulo 2](chapter-2.md) Essa série se concentrou principalmente no Dispatcher. Explicamos as noções básicas, as limitações e onde você precisa fazer certas compensações.

A complexidade e as complexidades do armazenamento em cache não são problemas exclusivos do Dispatcher. O armazenamento em cache é difícil em geral.

Ter o Dispatcher como sua única ferramenta na caixa de ferramentas seria, na verdade, uma limitação real.

Neste capítulo, queremos ampliar ainda mais nossa visão do armazenamento em cache e desenvolver algumas ideias sobre como superar algumas das deficiências do Dispatcher. Não há bala de prata - você terá que fazer compensações em seu projeto. Lembre-se de que com precisão de armazenamento em cache e invalidação sempre vem a complexidade, e com complexidade há a possibilidade de erros.

Você precisará fazer compensações nessas áreas,

* Desempenho e latência
* Consumo de recursos / Carga de CPU / Uso de disco
* Precisão / Moeda / Persistência / Segurança
* Simplicidade/complexidade/custo/capacidade de manutenção/prontidão para erros

Estas dimensões estão interligadas num sistema bastante complexo. Não há um simples se-isso-então-aquilo. Simplificar um sistema pode torná-lo mais rápido ou mais lento. Ele pode reduzir seus custos de desenvolvimento, mas aumentar os custos no helpdesk, por exemplo, se os clientes virem conteúdo obsoleto ou reclamarem de um site lento. Todos estes fatores devem ser considerados e contrabalançados. Mas, por agora, você já deve ter uma boa ideia, que não há bala de prata ou uma única &quot;prática recomendada&quot; - apenas um monte de práticas ruins e algumas boas.

## Armazenamento em cache encadeado

### Visão geral

#### Fluxo de dados

A entrega de uma página de um servidor para o navegador de um cliente passa por uma variedade de sistemas e subsistemas. Se você observar com cuidado, há vários dados de hops que precisam ser levados da origem para o escoamento, sendo que cada um deles é um possível candidato ao armazenamento em cache.

![Fluxo de dados de um aplicativo CMS típico](assets/chapter-3/data-flow-typical-cms-app.png)

*Fluxo de dados de um aplicativo CMS típico*

<br> 

Vamos começar nossa jornada com dados que estão em um disco rígido e precisam ser exibidos em um navegador.

#### Hardware e sistema operacional

Primeiro, a unidade de disco rígido (HDD) em si tem algum cache integrado no hardware. Em segundo lugar, o sistema operacional que monta o disco rígido, usa a memória livre para armazenar em cache os blocos acessados com frequência para acelerar o acesso.

#### Repositório de conteúdo

O próximo nível é o CRX ou Oak, o banco de dados de documentos usado pelo AEM. O CRX e o Oak dividem os dados em segmentos que podem ser armazenados em cache na memória, bem como para evitar acesso mais lento ao HDD.

#### Dados de terceiros

A maioria das instalações maiores da Web também tem dados de terceiros; dados provenientes de um sistema de informações de produtos, um sistema de gerenciamento de relações com o cliente, um banco de dados herdado ou qualquer outro serviço da Web arbitrário. Esses dados não precisam ser obtidos da origem sempre que necessário - especialmente quando se sabe que não precisam ser alterados com muita frequência. Portanto, ele pode ser armazenado em cache se não estiver sincronizado no banco de dados do CRX.

#### Camada de negócios - aplicativo/modelo

Normalmente, os scripts de modelo não renderizam o conteúdo bruto proveniente do CRX por meio da API JCR. Provavelmente, você tem uma camada comercial no meio que mescla, calcula e/ou transforma dados em um objeto de domínio comercial. Adivinha só: se essas operações forem caras, considere armazená-las em cache.

#### Fragmentos de marcação

O modelo agora é a base para a renderização da marcação de um componente. Por que não armazenar o modelo renderizado em cache também?

#### Dispatcher, CDN e outros proxies

Desativado, leva a HTML-Page renderizada para o Dispatcher. Já discutimos, que o principal objetivo do Dispatcher é armazenar em cache páginas de HTML e outros recursos da Web (apesar do nome). Antes que os recursos cheguem ao navegador, ele pode passar um proxy reverso, que pode armazenar em cache e um CDN, que também é usado para armazenamento em cache. O cliente pode ficar em um escritório, o que concede acesso à Web somente por meio de um proxy - e esse proxy pode decidir armazenar em cache também para salvar o tráfego.

#### Cache do navegador

Por último, mas não menos importante, o navegador também é armazenado em cache. Esse é um ativo fácil de ser deixado de lado. Mas é o cache mais próximo e mais rápido que você tem na cadeia de armazenamento em cache. Infelizmente, ela não é compartilhada entre usuários, mas ainda entre diferentes solicitações de um usuário.

### Onde armazenar em cache e por quê

Essa é uma longa cadeia de caches em potencial. E todos enfrentamos problemas em que vimos conteúdo desatualizado. Mas levando em conta quantos estágios há, é um milagre que na maior parte do tempo esteja funcionando.

Mas onde nessa cadeia faz sentido armazenar em cache? No começo? No final? Em todos os lugares? Depende... e depende de um grande número de fatores. Mesmo dois recursos no mesmo site podem desejar uma resposta diferente para essa pergunta.

Para dar uma ideia aproximada de quais fatores você pode considerar,

**Tempo de vida** - Se os objetos tiverem um tempo de vida inerente curto (os dados de tráfego podem ter um tempo de vida menor do que os dados meteorológicos), talvez não valha a pena armazenar em cache.

**Custo de Produção -** Quão caro (em termos de ciclos de CPU e E/S) é a reprodução e a entrega de um objeto. Se for barato, o armazenamento em cache pode não ser necessário.

**Tamanho** - Objetos grandes exigem mais recursos para serem armazenados em cache. Isso poderia ser um fator limitante e deve ser ponderado em relação aos benefícios.

**Frequência de acesso** - Se os objetos forem acessados raramente, o armazenamento em cache pode não ser eficaz. Eles simplesmente ficariam obsoletos ou seriam invalidados antes de serem acessados pela segunda vez do cache. Esses itens apenas bloqueariam os recursos de memória.

**Acesso compartilhado** - Os dados usados por mais de uma entidade devem ser armazenados em cache mais acima da cadeia. Na verdade, a cadeia de armazenamento em cache não é uma cadeia, mas uma árvore. Um pedaço de dados no repositório pode ser usado por mais de um modelo. Esses modelos, por sua vez, podem ser usados por mais de um script de renderização para gerar fragmentos de HTML. Esses fragmentos são incluídos em várias páginas que são distribuídas a vários usuários com seus caches privados no navegador. Então &quot;compartilhar&quot; não significa compartilhar apenas entre pessoas, mas entre softwares. Se você quiser encontrar um cache potencial &quot;compartilhado&quot;, basta rastrear a árvore até a raiz e encontrar um ancestral comum; é aqui que você deve armazenar em cache.

**Distribuição geoespacial** - Se seus usuários estiverem distribuídos pelo mundo, usar uma rede distribuída de caches pode ajudar a reduzir a latência.

**Largura de banda e latência da rede** - Por falar em latência, quem são seus clientes e que tipo de rede eles estão usando? Talvez seus clientes sejam clientes móveis em um país subdesenvolvido usando conexão 3G de smartphones de gerações mais antigas? Considere a criação de objetos menores e armazene-os em cache nos caches do navegador.

Essa lista de longe não é abrangente, mas achamos que vocês já entenderam a ideia.

### Regras básicas para cache encadeado

Novamente - o armazenamento em cache é difícil. Vamos compartilhar algumas regras básicas que extraímos de projetos anteriores que podem ajudá-lo a evitar problemas em seu projeto.

#### Evite o armazenamento em cache duplo

Cada uma das camadas introduzidas no último capítulo fornece algum valor na cadeia de armazenamento em cache. Economizando ciclos de computação ou aproximando os dados do consumidor. Não é errado armazenar em cache dados em vários estágios da cadeia, mas você deve sempre considerar quais são os benefícios e os custos do próximo estágio. O armazenamento em cache de uma página inteira no sistema de publicação geralmente não fornece nenhum benefício, pois isso já é feito no Dispatcher.

#### Misturar estratégias de invalidação

Existem três estratégias básicas de invalidação:

* **TTL, Tempo de vida:** Um objeto expira após um período fixo (por exemplo, &quot;daqui a 2 horas&quot;)
* **Data de expiração:** O objeto expira em um horário definido no futuro (por exemplo, &quot;17h de 10 de junho de 2019&quot;)
* **Baseado em evento:** O objeto é invalidado explicitamente por um evento que aconteceu na plataforma (por exemplo, quando uma página é alterada e ativada)

Agora, você pode usar diferentes estratégias em diferentes camadas de cache, mas há algumas &quot;tóxicas&quot;.

#### Invalidação baseada em eventos

![Invalidação baseada em evento puro](assets/chapter-3/event-based-invalidation.png)

*Invalidação baseada em evento puro: invalidar do cache interno para a camada externa*

<br> 

A invalidação pura baseada em eventos é a mais fácil de compreender, a mais fácil de se obter teoricamente correta e a mais precisa.

Simplificando, os caches são invalidados um por um após a alteração do objeto.

Você só precisa ter uma regra em mente:

Sempre invalide do cache interno para o externo. Se você invalidar um cache externo primeiro, ele poderá rearmazenar em cache o conteúdo obsoleto de um cache interno. Não faça suposições em que momento um cache está atualizado novamente; verifique se está. Melhor, acionando a invalidação do cache externo _após_ invalidar o interno.

Essa é a teoria. Mas, na prática, existem várias armadilhas. Os eventos devem ser distribuídos - possivelmente em uma rede. Na prática, isto torna o regime de invalidação mais difícil de implementar.

#### Auto - Recuperação

Com a invalidação baseada em eventos, você deve ter um plano de contingência. E se um evento de invalidação for perdido? Uma estratégia simples pode ser invalidar ou limpar após um determinado período. Portanto, talvez você tenha perdido esse evento e agora forneça conteúdo obsoleto. Mas seus objetos também têm um TTL implícito de várias horas (dias) somente. Então eventualmente o sistema se autorrecupera.

#### Invalidação puramente baseada em TTL

![Invalidação baseada em TTL não sincronizado](assets/chapter-3/ttl-based-invalidation.png)

*Invalidação baseada em TTL não sincronizado*

<br> 

Esse também é um esquema bastante comum. Você empilha várias camadas de caches, cada uma com direito a servir um objeto por um determinado período.

É fácil de implementar. Infelizmente, é difícil prever a duração de vida efetiva de um dado.

![Perigo externo que prolonga a vida útil de um objeto interno](assets/chapter-3/outer-cache.png)

*Cache externo que prolonga a duração de um objeto interno*

<br> 

Considere a ilustração acima. Cada camada de armazenamento em cache apresenta um TTL de 2 minutos. Agora - o TTL geral deve 2 minutos também, certo? Não exatamente. Se a camada externa buscar o objeto pouco antes de se tornar obsoleto, ela realmente prolonga o tempo de vida efetivo do objeto. Nesse caso, o tempo de vida efetivo pode estar entre 2 e 4 minutos. Considere que você concordou com o departamento de negócios, um dia é tolerável - e você tem quatro camadas de caches. O TTL real em cada camada não deve exceder seis horas... aumentando a taxa de erro do cache...

Não estamos dizendo que é um mau esquema. Você só deveria saber os limites. E é uma estratégia legal e fácil de começar. Somente se o tráfego do site aumentar é possível considerar uma estratégia mais precisa.

*Sincronizar o horário de invalidação definindo uma data específica*

#### Invalidação com base na data de expiração

Você obterá um tempo de vida efetivo mais previsível se estiver definindo uma data específica no objeto interno e propagando-a para os caches externos.

![Sincronização de datas de expiração](assets/chapter-3/synchronize-expiration-dates.png)

*Sincronização de datas de expiração*

<br> 

No entanto, nem todos os caches podem propagar as datas. E pode se tornar desagradável, quando o cache externo agrega dois objetos internos com datas de expiração diferentes.

#### Combinação de invalidação baseada em eventos e baseada em TTL

![Combinação de estratégias baseadas em eventos e em TTL](assets/chapter-3/mixing-event-ttl-strategies.png)

*Combinação de estratégias baseadas em eventos e em TTL*

<br> 

Um esquema comum no mundo do AEM também é usar a invalidação baseada em eventos nos caches internos (por exemplo, caches na memória em que os eventos podem ser processados em tempo quase real) e caches baseados em TTL na parte externa - onde talvez você não tenha acesso à invalidação explícita.

No mundo do AEM, você teria um cache na memória para objetos de negócios e fragmentos de HTML nos sistemas de publicação, ou seja, invalidado, quando os recursos subjacentes mudam e você propaga esse evento de alteração para o dispatcher, que também funciona com base em eventos. À frente disso, você teria, por exemplo, um CDN com base em TTL.

Ter uma camada de cache (curto) baseado em TTL na frente de um Dispatcher poderia efetivamente suavizar um pico que normalmente ocorreria após uma invalidação automática.

#### Combinação de TTL e invalidação baseada em eventos

![Combinação de TTL - e invalidação baseada em eventos](assets/chapter-3/toxic.png)

*Tóxico: TTL de mistura - e invalidação baseada em eventos*

<br> 

Esta combinação é tóxica. Nunca coloque um cache com base em eventos após um TTL ou Expiry. Lembra-se do efeito colateral que tivemos na estratégia &quot;TTL puro&quot;? O mesmo efeito pode ser observado aqui. Somente se o evento de invalidação do cache externo já tiver acontecido pode não ocorrer novamente - nunca, isso pode expandir a duração do objeto em cache para o infinito.

![Combinação baseada em TTL e em eventos: Repercussão para o infinito](assets/chapter-3/infinity.png)

*Combinação baseada em TTL e em eventos: Repercussão para o infinito*

<br> 

## Cache parcial e cache na memória

Você pode conectar-se ao estágio do processo de renderização para adicionar camadas de cache. Desde a obtenção de objetos de transferência remota de dados ou a criação de objetos de negócios locais até o armazenamento em cache da marcação renderizada de um único componente. Deixaremos as implementações concretas para um tutorial posterior. Mas talvez você já tenha implementado algumas dessas camadas de armazenamento em cache por conta própria. Portanto, o mínimo que podemos fazer aqui é introduzir os princípios básicos - e as armadilhas.

### Palavras de aviso

#### Respeitar o controle de acesso

As técnicas descritas aqui são bastante poderosas e um _must-have_ em cada caixa de ferramentas do desenvolvedor do AEM. Mas não se empolgue muito, use-os sabiamente. Armazenar um objeto em um cache e compartilhá-lo com outros usuários em solicitações de acompanhamento realmente significa contornar o controle de acesso. Isso geralmente não é um problema em sites voltados ao público, mas pode ser, quando um usuário precisa fazer logon antes de obter acesso.

Considere armazenar uma marcação HTML do menu principal de sites em um cache de memória para compartilhá-la entre várias páginas. Na verdade, esse é um exemplo perfeito para armazenar HTML parcialmente renderizado, pois criar uma navegação geralmente é caro, pois requer percorrer muitas páginas.

Você não está compartilhando a mesma estrutura de menu entre todas as páginas, mas também com todos os usuários, o que a torna ainda mais eficiente. Mas aguarde... mas talvez existam alguns itens no menu que são reservados somente para um determinado grupo de usuários. Nesse caso, o armazenamento em cache pode se tornar um pouco mais complexo.

#### Armazenar Somente Objetos de Negócios Personalizados em Cache

Se houver - esse é o conselho mais importante, podemos dar-lhe:

>[!WARNING]
>
>Armazene em cache somente os objetos que são seus, que são imutáveis, que você mesmo criou, que são superficiais e não têm referências de saída.

O que isso significa?

1. Vocês não sabem sobre o ciclo de vida pretendido dos objetos dos outros. Considere obter uma referência a um objeto de solicitação e decida armazená-lo em cache. Agora, a solicitação foi encerrada e o container de servlet deseja reciclar esse objeto para a próxima solicitação recebida. Nesse caso, outra pessoa está alterando o conteúdo sobre o qual você achava que tinha controle exclusivo. Não ignore isso. Vimos algo assim acontecendo em um projeto. Os clientes estavam vendo os dados de outros clientes em vez dos próprios.

2. Desde que um objeto seja referenciado por uma cadeia de outras referências, ele não poderá ser removido do heap. Se você retiver um objeto supostamente pequeno no cache que faz referência a, digamos uma representação de 4 MB de uma imagem, você terá uma boa chance de ter problemas com vazamento de memória. Os caches devem ser baseados em referências fracas. Mas - referências fracas não funcionam como você pode esperar. Essa é a melhor maneira absoluta de produzir um vazamento de memória e terminar com um erro de falta de memória. E - você não sabe qual é o tamanho da memória retida dos objetos estranhos, certo?

3. Especialmente no Sling, é possível adaptar (quase) cada objeto um ao outro. Considere colocar um recurso no cache. A próxima solicitação (com direitos de acesso diferentes), busca esse recurso e o adapta em um resourceResolver ou em uma sessão para acessar outros recursos aos quais ele não teria acesso.

4. Mesmo que você crie um &quot;invólucro&quot; fino em torno de um recurso do AEM, não armazene em cache isso - mesmo que seja seu e imutável. O objeto envolvido seria uma referência (o que nós proibimos antes) e se olharmos afiado, que basicamente cria os mesmos problemas como descrito no último item.

5. Se quiser armazenar em cache, crie seus próprios objetos copiando dados primitivos nos seus próprios objetos shallo. Talvez você queira vincular seus próprios objetos por referências - por exemplo, talvez você queira armazenar em cache uma árvore de objetos. Isso é bom - mas apenas armazene em cache objetos que você acabou de criar exatamente na mesma solicitação - e nenhum objeto que tenha sido solicitado de outro lugar (mesmo que seja o namespace do objeto &quot;seu&quot;). _Copiando objetos_ é a chave. E certifique-se de limpar toda a estrutura de objetos vinculados de uma só vez e evitar referências de entrada e saída para sua estrutura.

6. Sim, e mantenha seus objetos imutáveis. Propriedades privadas, somente e sem setters.

São muitas regras, mas vale a pena segui-las. Mesmo que você seja experiente e super inteligente e tenha tudo sob controle. O jovem colega de seu projeto acabou de se formar na universidade. Ele não sabe de todas essas armadilhas. Se não há armadilhas, não há nada a evitar. Mantenha simples e compreensível.

### Ferramentas e bibliotecas

Esta série trata da compreensão de conceitos e do poder para criar uma arquitetura que melhor se adapta ao seu caso de uso.

Não estamos a promover qualquer instrumento em particular. Mas dê dicas de como avaliá-los. Por exemplo, o AEM tem um cache interno simples com um TTL fixo desde a versão 6.0. Você deve usá-lo? Provavelmente não na publicação em que um cache baseado em eventos se segue na cadeia (dica: O Dispatcher). Mas pode ser por uma escolha decente para um Autor. Há também um cache HTTP pelo Adobe ACS commons que pode ser útil considerar.

Ou você cria o seu próprio, com base em uma estrutura de armazenamento em cache bem desenvolvida, como [Ehcache](https://www.ehcache.org). Isso pode ser usado para armazenar em cache objetos Java e marcação renderizada (`String` objetos).

Em alguns casos simples, você também pode se dar bem com o uso de mapas de hash simultâneos - você verá rapidamente os limites aqui - na ferramenta ou em suas habilidades. A simultaneidade é tão difícil de dominar quanto a nomeação e o armazenamento em cache.

#### Referências

* [Cache http do ACS Commons](https://adobe-consulting-services.github.io/acs-aem-commons/features/http-cache/index.html)
* [Estrutura de cache do cache do cache do cache](https://www.ehcache.org)

### Termos básicos

Não entraremos muito fundo na teoria do armazenamento em cache aqui, mas nos sentimos obrigados a fornecer algumas palavras, para que você tenha um bom começo.

#### Remoção de cache

Falamos muito sobre invalidação e purga. _Remoção de cache_ está relacionado com estes termos: após uma entrada ser removida, ela não estará mais disponível. Mas a remoção acontece não quando uma entrada está desatualizada, mas quando o cache está cheio. Itens mais recentes ou &quot;mais importantes&quot; empurram itens mais antigos ou menos importantes para fora do cache. Quais entradas você terá que sacrificar é uma decisão caso a caso. Talvez você queira remover os mais antigos ou aqueles que foram usados muito raramente ou acessados por um longo tempo.

#### Armazenamento em cache preventivo

O armazenamento em cache preventivo significa recriar a entrada com conteúdo novo no momento em que ela for invalidada ou considerada desatualizada. É claro que você faria isso somente com alguns recursos, que você tem certeza de que são acessados com frequência e imediatamente. Caso contrário, você desperdiçaria recursos ao criar entradas de cache que talvez nunca fossem solicitadas. Ao criar entradas de cache preventivamente, é possível reduzir a latência da primeira solicitação para um recurso após a invalidação do cache.

#### Aquecimento de cache

O aquecimento do cache está intimamente relacionado ao armazenamento preemptivo em cache. Embora você não usaria esse termo para um sistema ativo. E tem menos restrições de tempo do que a primeira. Você não faz o rearmazenamento em cache imediatamente após a invalidação, mas preenche o cache gradualmente quando o tempo permitir.

Por exemplo, você remove um trecho Publicar/Dispatcher do balanceador de carga para atualizá-lo. Antes de reintegrá-la, você rastreia automaticamente as páginas acessadas com mais frequência para colocá-las no cache novamente. Quando o cache estiver &quot;quente&quot;, preenchido adequadamente, você reintegra o trecho no balanceador de carga.

Ou talvez você reintegre o trecho de uma só vez, mas controle o tráfego para esse trecho para que ele tenha a chance de aquecer seus caches pelo uso regular.

Ou talvez você também queira armazenar em cache algumas páginas acessadas com menos frequência nos momentos em que o sistema estiver ocioso, para diminuir a latência quando elas forem realmente acessadas por solicitações reais.

#### Identidade do Objeto de Cache, Carga, Dependência de Invalidação e TTL

De modo geral, um objeto armazenado em cache ou &quot;entrada&quot; tem cinco propriedades principais,

#### Chave

Essa é a identidade da propriedade pela qual você identifica e faz o objeto. Para recuperar sua carga ou para removê-la do cache. O dispatcher, por exemplo, usa o URL de uma página como a chave. Observe que o dispatcher não usa os caminhos de páginas. Isso não é suficiente para diferenciar renderizações. Outros caches podem usar chaves diferentes. Veremos alguns exemplos mais tarde.

#### Valor/carga útil

Este é o baú do tesouro do objeto, os dados que você deseja recuperar. No caso do dispatcher, é o conteúdo dos arquivos. Mas ela também pode ser uma árvore de objetos Java.

#### TTL

Já cobrimos o TTL. O tempo após o qual uma entrada é considerada obsoleta e não deve mais ser entregue.

#### Dependência

Isso está relacionado à invalidação baseada em eventos. De quais dados originais esse objeto depende? Na Parte I, já dissemos, que um rastreamento de dependência verdadeiro e preciso é muito complexo. Mas com nosso conhecimento do sistema você pode aproximar as dependências com um modelo mais simples. Invalidamos objetos suficientes para limpar conteúdo obsoleto... e talvez, inadvertidamente, mais do que seria necessário. Mas ainda assim tentamos manter abaixo &quot;limpar tudo&quot;.

Quais objetos dependem do que os outros são originais em cada aplicativo. Forneceremos alguns exemplos sobre como implementar uma estratégia de dependência posteriormente.

### Armazenamento em cache de fragmento de HTML

![Reutilização de um fragmento renderizado em páginas diferentes](assets/chapter-3/re-using-rendered-fragment.png)

*Reutilização de um fragmento renderizado em páginas diferentes*

<br> 

O armazenamento em cache de fragmentos de HTML é uma ferramenta poderosa. A ideia é armazenar em cache a marcação HTML que foi gerada por um componente em um cache de memória. Vocês podem perguntar, por que eu deveria fazer isso? Estou armazenando a marcação da página inteira em cache no dispatcher de qualquer maneira, incluindo a marcação desse componente. Nós concordamos. Você faz isso, mas uma vez por página. Você não está compartilhando essa marcação entre as páginas.

Imagine que você esteja renderizando uma navegação na parte superior de cada página. A marcação tem a mesma aparência em cada página. Mas você o está renderizando várias vezes para cada página, que não está no Dispatcher. E lembre-se: após a invalidação automática, todas as páginas precisam ser renderizadas. Então, basicamente, você está executando o mesmo código com os mesmos resultados centenas de vezes.

Por nossa experiência, renderizar uma navegação superior aninhada é uma tarefa muito cara. Normalmente, você percorre uma boa parte da árvore de documentos para gerar os itens de navegação. Mesmo que você precise apenas do título de navegação e do URL, as páginas precisam ser carregadas na memória. E aqui eles estão entupindo recursos preciosos. De novo e de novo.

Mas o componente é compartilhado entre muitas páginas. E compartilhar algo é um indício de usar um cache. Assim, o que você gostaria de fazer é verificar se o componente de navegação já foi renderizado e armazenado em cache e, em vez de renderizar novamente, apenas emitir o valor dos caches.

Há duas gentilezas maravilhosas desse esquema que facilmente se perdem:

1. Você está armazenando em cache uma sequência de caracteres Java. Uma cadeia de caracteres não tem referências de saída e é imutável. Então, considerando os avisos acima - isso é super-seguro.

2. A invalidação também é muito fácil. Sempre que algo mudar em seu site, você deseja invalidar essa entrada de cache. A reconstrução é relativamente barata, pois precisa ser executada apenas uma vez e depois é reutilizada por todas as centenas de páginas.

Isso é um grande alívio para seus servidores de publicação.

### Implementação de caches de fragmentos

#### Tags personalizadas

Antigamente, quando você usava o JSP como mecanismo de modelo, era bastante comum usar uma tag JSP personalizada vinculando o código de renderização dos componentes.

```
<!-- Pseudo Code -->

<myapp:cache
  key=' ${info.homePagePath} + ${component.path}'
  cache='main-navigation'
  dependency='${info.homePagePath}'>

… original components code ..

</myapp:cache>
```

A tag personalizada capturaria seu corpo e o gravaria no cache ou impediria a execução de seu corpo e geraria a carga útil da entrada do cache.

A &quot;Chave&quot; é o caminho de componentes que teria na página inicial. Não usamos o caminho do componente na página atual, pois isso criaria uma entrada de cache por página, o que contrariaria nossa intenção de compartilhar esse componente. Também não estamos usando apenas o caminho relativo dos componentes (`jcr:conten/mainnavigation`), pois isso nos impediria de usar diferentes componentes de navegação em sites diferentes.

&quot;Cache&quot; é um indicador para armazenar a entrada. Geralmente, há mais de um cache no qual você armazena itens. Cada uma delas pode se comportar de forma diferente. Portanto, é bom diferenciar o que está armazenado - mesmo que no final sejam apenas cadeias de caracteres.

&quot;Dependência&quot; é do que a entrada de cache depende. O cache de &quot;navegação principal&quot; pode ter uma regra, de que, se houver qualquer alteração abaixo do nó &quot;dependência&quot;, a entrada correspondente deverá ser removida. Assim, a implementação do cache precisaria se registrar como um ouvinte de eventos no repositório para estar ciente das alterações e aplicar as regras específicas do cache para descobrir o que precisa ser invalidado.

O exposto acima foi apenas um exemplo. Você também pode optar por ter uma árvore de caches. Sempre que o primeiro nível for usado para separar sites (ou locatários) e o segundo nível, então ramificações em tipos de conteúdo (por exemplo, &quot;navegação principal&quot;), isso poderá poupar a adição do caminho das páginas iniciais, como no exemplo acima.

A propósito, você também pode usar essa abordagem com componentes mais modernos baseados em HTL. Você teria um invólucro JSP em torno do script HTL.

#### Filtros do componente

Mas, em uma abordagem HTL pura, você preferiria criar o cache do fragmento com um filtro de componente Sling. Ainda não vimos isso completamente, mas essa é a abordagem que adotaríamos para resolver essa questão.

#### Sling Dynamic Include

O cache de fragmentos é usado se você tiver algo constante (a navegação) no contexto de um ambiente em alteração (páginas diferentes).

Mas você também pode ter o oposto, um contexto relativamente constante (uma página que raramente muda) e alguns fragmentos em constante mudança nessa página (por exemplo, um ticker em tempo real).

Nesse caso, você pode dar [Sling Dynamic Includes](https://sling.apache.org/documentation/bundles/dynamic-includes.html) uma chance. Basicamente, é um filtro de componente que envolve o componente dinâmico e, em vez de renderizá-lo na página, cria uma referência. Essa referência pode ser uma chamada de Ajax - para que o componente seja incluído pelo navegador e, portanto, a página ao redor possa ser armazenada estaticamente em cache. Ou, como alternativa, a Inclusão dinâmica do Sling pode gerar uma diretiva SSI (Inclusão no lado do servidor). Essa diretiva seria executada no servidor Apache. Você ainda pode usar as diretivas ESI - Edge Side Include se utilizar o verniz ou um CDN compatível com scripts ESI.

![Diagrama de sequência de uma solicitação usando Sling Dynamic Include](assets/chapter-3/sequence-diagram-sling-dynamic-include.png)

*Diagrama de sequência de uma solicitação usando Sling Dynamic Include*

<br> 

A documentação da SDI informa que você deve desativar o armazenamento em cache de URLs que terminam em &quot;*.nocache.html&quot;, o que faz sentido - já que você está lidando com componentes dinâmicos.

Você poderá ver outra opção sobre como usar o SDI: Se _não_ desative o cache do dispatcher para as inclusões, o Dispatcher atua como um cache de fragmento semelhante ao descrito no último capítulo: páginas e fragmentos de componentes de forma igual e independente são armazenados em cache no dispatcher e unidos pelo script SSI no servidor Apache quando a página é solicitada. Dessa forma, você pode implementar componentes compartilhados como a navegação principal (considerando que você sempre usa o mesmo URL de componente).

Isso deveria funcionar - em teoria. Mas...

Recomendamos não fazer isso: você perderia a capacidade de ignorar o cache dos componentes dinâmicos reais. O SDI é configurado globalmente e as alterações que você faria no seu &quot;cache de fragmento pobre&quot; também se aplicariam aos componentes dinâmicos.

Recomendamos que você analise cuidadosamente a documentação da SDI. Há algumas outras limitações, mas a SDI é uma ferramenta valiosa em alguns casos.

#### Referências

* [docs.oracle.com - Como escrever tags JSP personalizadas](https://docs.oracle.com/cd/E11035_01/wls100/taglib/quickstart.html)
* [Dominik Süß - Criação e uso de filtros de componentes](https://www.slideshare.net/connectwebex/prsentation-dominik-suess)
* [sling.apache.org - Sling Dynamic Includes](https://sling.apache.org/documentation/bundles/dynamic-includes.html)
* [helpx.adobe.com - Configuração do Sling Dynamic Includes no AEM](https://helpx.adobe.com/experience-manager/kt/platform-repository/using/sling-dynamic-include-technical-video-setup.html)


#### Armazenamento em cache do modelo

![Cache baseado em modelo: um objeto comercial com duas renderizações diferentes](assets/chapter-3/model-based-caching.png)

*Cache baseado em modelo: um objeto comercial com duas renderizações diferentes*

<br> 

Vamos rever o caso com a navegação novamente. Presumimos que cada página exigiria a mesma marcação da navegação.

Mas talvez, esse não seja o caso. Talvez você queira renderizar uma marcação diferente para o item na navegação que representa a _página atual_.

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

Estas são duas renderizações completamente diferentes. Ainda assim, a _objeto comercial_ - a árvore de navegação completa - é a mesma.  A variável _objeto comercial_  este seria um gráfico de objetos representando os nós na árvore. Esse gráfico pode ser facilmente armazenado em um cache de memória. Lembre-se, no entanto, de que este gráfico não deve conter nenhum objeto ou fazer referência a qualquer objeto que você não tenha criado - especialmente agora nós JCR.

#### Armazenamento em cache no navegador

Já abordamos a importância do armazenamento em cache no navegador, e há muitos tutoriais bons. No final, para o navegador, o Dispatcher é apenas um servidor Web que segue o protocolo HTTP.

No entanto - apesar da teoria - reunimos alguns fragmentos de conhecimento que não encontramos em nenhum outro lugar e que queremos compartilhar.

Basicamente, o armazenamento em cache do navegador pode ser aproveitado de duas maneiras diferentes:

1. O navegador tem um recurso em cache do qual sabe a data de expiração exata. Nesse caso, ele não solicitará o recurso novamente.

2. O navegador tem um recurso, mas não tem certeza se ele ainda é válido. Nesse caso, ele perguntaria ao servidor Web (o Dispatcher no nosso caso). Forneça o recurso se ele tiver sido modificado desde a última vez que você o entregou. Se não tiver sido alterado, o servidor responde com &quot;304 - não alterado&quot; e somente os metadados foram transmitidos.

#### Depuração

Se você estiver otimizando suas configurações do Dispatcher para armazenamento em cache do navegador, é extremamente útil usar um servidor proxy de desktop entre seu navegador e o servidor Web. Preferimos o &quot;Charles Web Debugging Proxy&quot; de Karl von Randow.

Com o Charles, você pode ler as solicitações e respostas, que são transmitidas de e para o servidor. E - você pode aprender muito sobre o protocolo HTTP. Navegadores modernos também oferecem alguns recursos de depuração, mas os recursos de um proxy de desktop são inéditos. Você pode manipular os dados transferidos, limitar a transmissão, repetir solicitações únicas e muito mais. E a interface do usuário é claramente organizada e bastante abrangente.

O teste mais básico é usar o site como um usuário normal - com o proxy intermediário - e verificar o proxy se o número de solicitações estáticas (para /etc/...) estiver ficando menor com o tempo - pois elas devem estar no cache e não devem ser mais solicitadas.

Descobrimos que um proxy pode fornecer uma visão geral mais clara, pois a solicitação em cache não aparece no log, enquanto alguns depuradores integrados do navegador ainda mostram essas solicitações com &quot;0 ms&quot; ou &quot;do disco&quot;. O que é correto e preciso, mas pode ofuscar sua visualização um pouco.

Em seguida, você pode detalhar e verificar os cabeçalhos dos arquivos transferidos para ver, por exemplo, se os cabeçalhos http &quot;Expira&quot; estão corretos. Você pode reproduzir solicitações com cabeçalhos if-modified-since definidos para ver se o servidor responde corretamente com um código de resposta 304 ou 200. Você pode observar o tempo das chamadas assíncronas e também pode testar suas suposições de segurança em um certo grau. Lembre-se de que dissemos para você não aceitar todos os seletores que não são explicitamente esperados? Aqui você pode brincar com o URL e os parâmetros e ver se seu aplicativo se comporta bem.

Há apenas uma coisa que pedimos que você não faça ao depurar seu cache:

Não recarregue páginas no navegador!

Um &quot;recarregamento de navegador&quot;, um _simple-reload_ bem como uma _recarregamento forçado_ (&quot;_shift-reload_&quot;) não é o mesmo que uma solicitação de página normal. Uma solicitação simples de recarregamento define um cabeçalho

```
Cache-Control: max-age=0
```

E Shift-Reload (mantendo pressionada a tecla &quot;Shift&quot; enquanto clica no botão de recarregamento) geralmente define um cabeçalho de solicitação

```
Cache-Control: no-cache
```

Ambos os cabeçalhos têm efeitos semelhantes, mas ligeiramente diferentes, mas o mais importante é que eles são completamente diferentes de uma solicitação normal quando você abre um URL no slot de URL ou usando links no site. A navegação normal não define cabeçalhos de Cache-Control, mas provavelmente um cabeçalho if-modified-since.

Portanto, se você quiser depurar o comportamento normal de navegação, faça exatamente isso: _Procurar normalmente_. Usar o botão de recarregamento do seu navegador é a melhor maneira de não ver erros de configuração do cache na sua configuração.

Use o Charles Proxy para ver sobre o que estamos falando. Sim - e enquanto estiver aberto - você pode reproduzir as solicitações nesse local. Não é necessário recarregar do navegador.

## Teste de desempenho

Ao usar um proxy, você tem uma noção do comportamento de tempo de suas páginas. É claro que isso não é de longe um teste de desempenho.  Um teste de desempenho exigiria que vários clientes solicitassem suas páginas em paralelo.

Um erro comum, visto com muita frequência, é que o teste de desempenho inclui apenas um número muito pequeno de páginas e essas páginas são entregues somente pelo cache do Dispatcher.

Se você estiver promovendo seu aplicativo para o sistema ativo, a carga será completamente diferente do que você testou.

No sistema em tempo real, o padrão de acesso não é um número tão pequeno de páginas igualmente distribuídas que você tem nos testes (página inicial e poucas páginas de conteúdo). O número de páginas é muito maior e as solicitações são distribuídas de forma muito desigual. E, é claro, as páginas ativas não podem ser atendidas 100% do cache: há solicitações de invalidação provenientes do sistema de publicação que invalidam automaticamente uma grande parte de seus preciosos recursos.

Ah, sim - e quando você estiver reconstruindo o cache do Dispatcher, descobrirá que o sistema de publicação também se comporta de forma bem diferente, dependendo se você solicita apenas algumas páginas - ou um número maior. Mesmo que todas as páginas sejam similarmente complexas, seu número desempenha um papel. Lembra o que dissemos sobre o armazenamento em cache encadeado? Se você sempre solicitar o mesmo pequeno número de páginas, as chances são boas de que os blocos correspondentes com os dados brutos estejam no cache dos discos rígidos ou que os blocos sejam armazenados em cache pelo sistema operacional. Além disso, há uma boa chance de o Repositório ter armazenado em cache o segmento correspondente em sua memória principal. Portanto, a nova renderização é significativamente mais rápida do que quando você tinha outras páginas se removendo agora e depois de vários caches.

O armazenamento em cache é difícil, assim como o teste de um sistema que depende do armazenamento em cache. Então, o que você pode fazer para ter um cenário real mais preciso?

Acreditamos que seria necessário realizar mais de um teste e fornecer mais de um índice de desempenho como medida da qualidade da solução.

Se você já tiver um site existente, meça o número de solicitações e como elas são distribuídas. Tente modelar um teste que use uma distribuição semelhante de solicitações. Adicionar alguma aleatoriedade não poderia doer. Não é necessário simular um navegador que carregaria recursos estáticos, como JS e CSS. Esses recursos não são realmente importantes. Eventualmente, eles são armazenados em cache no navegador ou no Dispatcher e não somam a carga significativamente. Mas as imagens referenciadas são importantes. Encontre também a distribuição nos arquivos de log antigos e modele um padrão de solicitação semelhante.

Agora, faça um teste com o Dispatcher sem armazenar em cache. Esse é o seu pior cenário. Descubra em que pico de carga seu sistema está ficando instável sob estas piores condições. Você também pode piorar removendo alguns trechos do Dispatcher/Publish, se desejar.

Em seguida, faça o mesmo teste com todas as configurações de cache necessárias para &quot;ativado&quot;. Aumente lentamente suas solicitações paralelas para aquecer o cache e ver quanto seu sistema pode suportar sob essas condições de melhor caso.

Um cenário de caso médio seria executar o teste com o Dispatcher ativado, mas também com algumas invalidações ocorrendo. Você pode simular isso tocando nos arquivos de status por um cronjob ou enviando as solicitações de invalidação em intervalos irregulares para o Dispatcher. Não se esqueça de limpar também alguns dos recursos não invalidados automaticamente de vez em quando.

Você pode variar o último cenário aumentando as solicitações de invalidação e a carga.

Isso é um pouco mais complexo do que apenas um teste de carga linear, mas oferece muito mais confiança em sua solução.

Você pode se esquivar do esforço. Mas pelo menos faça um teste do pior caso no sistema de publicação com um número maior de páginas (igualmente distribuídas) para ver os limites do sistema. Certifique-se de interpretar o número do melhor cenário e provisionar seus sistemas com espaço suficiente.
