---
title: Capítulo 1 - Configuração e downloads do tutorial
seo-title: Introdução ao AEM Content Services - Capítulo 1 - Configuração do tutorial
description: O Capítulo 1 do tutorial sem cabeçalho AEM a configuração da linha de base para a instância AEM do tutorial.
seo-description: O Capítulo 1 do tutorial sem cabeçalho AEM a configuração da linha de base para a instância AEM do tutorial.
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '17502'
ht-degree: 0%

---


# Capítulo 1 - Conceitos, padrões e antipadrões do dispensador

## Visão geral

Este capítulo apresenta uma breve introdução sobre a história e a mecânica do Dispatcher e discute como isso influencia a maneira como um desenvolvedor AEM projetaria seus componentes.

## Por que os desenvolvedores devem se preocupar com a infraestrutura

O Dispatcher é uma parte essencial da maioria das instalações - se não todas AEM. Você pode encontrar muitos artigos online que discutem como configurar o Dispatcher, bem como dicas e truques.

No entanto, esses pedaços e pedaços de informação sempre se start em um nível muito técnico - supondo que você já saiba o que deseja fazer e, assim, forneça apenas detalhes sobre como alcançar o que deseja. Nunca encontramos nenhum documento conceitual descrevendo o _o que é e por que é_ quando se trata do que você pode ou não fazer com o expedidor.

### Antirpadrão: Dispatcher como um Aterrissagem

Esta falta de informação básica leva a uma série de padrões anti-padrão que vimos em vários projetos AEM:

1. Como o Dispatcher está instalado no servidor Web Apache, é tarefa dos &quot;deuses Unix&quot; no projeto configurá-lo. Um &quot;desenvolvedor de java mortal&quot; não precisa se preocupar com isso.

2. O desenvolvedor Java precisa garantir que seu código funcione... o despachante mais tarde o agilizará magicamente. O despachante é sempre um pensamento secundário. Mas isso não está funcionando. Um desenvolvedor deve projetar seu código pensando no expedidor. E ele precisa saber seus conceitos básicos para fazer isso.

### &quot;Primeiro faça funcionar - depois faça rápido&quot; Não está sempre certo

Você pode ter ouvido o conselho de programação _&quot;Primeiro, faça funcionar - depois faça rápido.&quot;_. Não é totalmente errado. No entanto, sem o contexto correto, tende a ser mal interpretado e não aplicado corretamente.

O conselho deve impedir o desenvolvedor de otimizar prematuramente o código, que pode nunca ser executado - ou é executado tão raramente, que uma otimização não teria impacto suficiente para justificar o esforço que está sendo colocado na otimização. Além disso, a otimização poderia levar a um código mais complexo e, portanto, a introduzir erros. Então, se você for desenvolvedor, não gaste muito tempo na microotimização de cada linha de código. Certifique-se de escolher as estruturas de dados, os algoritmos e as bibliotecas corretos e aguarde a análise de um ponto de conexão do perfil para ver onde uma otimização mais detalhada poderia aumentar o desempenho geral.

### Decisões e artefatos de arquitetura

No entanto, o conselho &quot;Primeiro, faça funcionar - depois faça-o rapidamente&quot; está completamente errado quando se trata de decisões &quot;arquitetônicas&quot;. O que são decisões arquitetônicas? Simplificando, são as decisões que são caras, difíceis e/ou impossíveis de mudar depois. Lembre-se que &quot;caro&quot; às vezes é o mesmo que &quot;impossível&quot;.  Por exemplo, quando seu projeto está acabando, mudanças caras são impossíveis de implementar. Mudanças infraestruturais são o primeiro tipo de mudanças nessa categoria que vêm à mente da maioria das pessoas. Mas há também outro tipo de artefatos &quot;arquitetônicos&quot; que podem se tornar muito desagradáveis para mudar:

1. Partes de código no &quot;centro&quot; de um aplicativo, nas quais muitas outras partes dependem. Para alterá-las, é necessário que todas as dependências sejam alteradas e testadas novamente ao mesmo tempo.

2. Artefatos, que estão envolvidos em algum cenário assíncrono, dependendo do momento em que a entrada - e, portanto, o comportamento do sistema pode variar muito aleatoriamente. As alterações podem ter efeitos imprevisíveis e podem ser difíceis de testar.

3. Padrões de software que são usados e reutilizados repetidamente, em todas as peças e partes do sistema. Se o padrão do software se revelar não ideal, todos os artefatos que usam o padrão precisam ser recodificados.

Lembrar? Acima desta página, dissemos que o Dispatcher é uma parte essencial de um aplicativo AEM. O acesso a um aplicativo da Web é muito aleatório - os usuários estão chegando e indo em momentos imprevisíveis. No final - todo o conteúdo será (ou deverá) armazenado em cache no Dispatcher. Então, se você prestasse atenção, você poderia ter percebido que o cache poderia ser visto como um artefato &quot;arquitetônico&quot; e, portanto, deveria ser entendido por todos os membros da equipe, desenvolvedores e administradores, da mesma forma.

Não estamos dizendo que um desenvolvedor deveria configurar o Dispatcher. Eles precisam conhecer os conceitos - especialmente os limites - para garantir que seu código possa ser aproveitado pelo Dispatcher também.

O Dispatcher não melhora magicamente a velocidade do código. Um desenvolvedor precisa criar seus componentes pensando no Dispatcher. Portanto, ele precisa saber como funciona.

## Cache do Dispatcher - Princípios Básicos

### Dispatcher como Http de Cache - Balanceador de Carga

O que é o Dispatcher e por que ele se chama &quot;Dispatcher&quot; em primeiro lugar?

O Dispatcher é

* Primeiro e acima de tudo um cache

* Um proxy reverso

* Um módulo para o servidor Web Apache httpd, adicionando AEM recursos relacionados à versatilidade do Apache e funcionando sem problemas juntamente com todos os outros módulos Apache (como SSL ou até SSI inclui como veremos mais tarde)

Nos primeiros dias da web, você esperaria algumas centenas de visitantes para um site. Uma configuração de um Dispatcher, &quot;despachado&quot; ou balanceada a carga de solicitações para vários servidores de publicação AEM e que geralmente era suficiente - portanto, o nome &quot;Dispatcher&quot;. Atualmente, porém, essa configuração não é mais usada com muita frequência.

Veremos diferentes maneiras de configurar o Dispatchers e os sistemas de publicação mais adiante neste artigo. Primeiro vamos nos start com algumas noções básicas sobre cache http.

![Funcionalidade básica de um Cache do Dispatcher](assets/chapter-1/basic-functionality-dispatcher.png)

*Funcionalidade básica de um Cache do Dispatcher*

<br> 

O básico do expedidor é explicado aqui. O dispatcher é um proxy reverso de cache simples com a capacidade de receber e criar solicitações HTTP. Um ciclo normal de solicitação/resposta é o seguinte:

1. Um usuário solicita uma página
2. O Dispatcher verifica se já tem uma versão renderizada dessa página. Suponhamos que seja a primeira solicitação para esta página e que o Dispatcher não encontre uma cópia em cache local.
3. O Dispatcher solicita a página do sistema de publicação
4. No sistema de publicação, a página é renderizada por um modelo JSP ou HTL
5. A página é retornada ao Dispatcher
6. O Dispatcher armazena em cache a página
7. O Dispatcher retorna a página para o navegador
8. Se a mesma página for solicitada pela segunda vez, ela poderá ser fornecida diretamente do cache do Dispatcher sem a necessidade de renderizá-la novamente na instância Publicar. Isso economiza tempo de espera para os ciclos de usuário e CPU na instância Publicar.

Estávamos falando de &quot;páginas&quot; na última seção. Mas o mesmo esquema também se aplica a outros recursos, como imagens, arquivos CSS, downloads de PDFs e assim por diante.

#### Como os dados são armazenados em cache

O módulo Dispatcher aproveita os recursos que o servidor Apache de hospedagem fornece. Recursos como páginas HTML, downloads e imagens são armazenados como arquivos simples no sistema de arquivos Apache. É tão simples assim.

O nome do arquivo é derivado pelo URL do recurso solicitado. Se você solicitar um arquivo `/foo/bar.html` ele será armazenado, por exemplo, em /`var/cache/docroot/foo/bar.html`.

Em princípio, se todos os arquivos forem armazenados em cache e, portanto, armazenados estaticamente no Dispatcher, você poderá puxar o plug do sistema de publicação e o Dispatcher funcionará como um servidor da Web simples. Mas isto é apenas para ilustrar o princípio. A vida real é mais complicada. Não é possível armazenar tudo em cache, e o cache nunca estará completamente &quot;cheio&quot;, pois o número de recursos pode ser infinito devido à natureza dinâmica do processo de renderização. O modelo de um sistema de arquivos estático ajuda a gerar uma imagem aproximada dos recursos do dispatcher. E ajuda a explicar as limitações do expedidor.

#### A estrutura do URL AEM e o mapeamento do sistema de arquivos

Para entender o Dispatcher com mais detalhes, vamos revisitar a estrutura de um URL de amostra simples.  Vejamos o exemplo abaixo.

`http://domain.com/path/to/resource/pagename.selectors.html/path/suffix.ext?parameter=value&amp;otherparameter=value#fragment`

* `http` denota o protocolo

* `domain.com` é o nome do domínio

* `path/to/resource` é o caminho sob o qual o recurso é armazenado no CRX e, subsequentemente, no sistema de arquivos do servidor Apache

Daqui, as coisas diferem um pouco entre o sistema de arquivos AEM e o sistema de arquivos Apache.

Em AEM,

* `pagename` é a etiqueta de recursos

* `selectors` significa vários seletores usados no Sling para determinar como o recurso é renderizado. Um URL pode ter um número arbitrário de seletores. Eles são separados por um ponto. Uma seção de seletores poderia, por exemplo, ser algo como &quot;french.mobile.fancy&quot;. Os seletores devem conter apenas letras, dígitos e traços.

* `html` como sendo o último dos &quot;seletores&quot; é chamado de extensão. No AEM/Sling, também determina parcialmente o script de renderização.

* `path/suffix.ext` é uma expressão semelhante a caminho que pode ser um sufixo do URL.  Ele pode ser usado em scripts AEM para controlar ainda mais como um recurso é renderizado. Teremos uma seção inteira sobre esta parte mais tarde. Por enquanto, basta saber que você pode usá-lo como um parâmetro adicional. Sufixos devem ter uma extensão.

* `?parameter=value&otherparameter=value` é a seção query do URL. É usado para passar parâmetros arbitrários para AEM. URLs com parâmetros não podem ser armazenados em cache e, portanto, os parâmetros devem ser limitados aos casos em que são absolutamente necessários.

* `#fragment`, a parte do fragmento de um URL não é transmitida para AEM é usada somente no navegador; em estruturas JavaScript como &quot;parâmetros de roteamento&quot; ou para pular para uma parte específica da página.

No Apache (*referencie o diagrama seguinte*),

* `pagename.selectors.html` é usado como o nome de arquivo no sistema de arquivos do cache.

Se o URL tiver um sufixo `path/suffix.ext`, então,

* `pagename.selectors.html` é criada como uma pasta

* `path` uma pasta na  `pagename.selectors.html` pasta

* `suffix.ext` é um arquivo na  `path` pasta. Observação: Se o sufixo não tiver uma extensão, o arquivo não será armazenado em cache.

![Layout do sistema de arquivos após obter URLs do Dispatcher](assets/chapter-1/filesystem-layout-urls-from-dispatcher.png)

*Layout do sistema de arquivos após obter URLs do Dispatcher*

<br> 

#### Limitações básicas

O mapeamento entre um URL, o recurso e o nome do arquivo é muito simples.

No entanto, podem ter reparado em algumas armadilhas.

1. URLs podem se tornar muito longos. Adicionar a parte &quot;path&quot; de um `/docroot` no sistema de arquivos local pode facilmente exceder os limites de alguns sistemas de arquivos. Executar o Dispatcher no NTFS no Windows pode ser um desafio. No entanto, você está seguro com o Linux.

2. Os URLs podem conter caracteres e trechos especiais. Normalmente, isso não é um problema para o expedidor. No entanto, lembre-se de que o URL é interpretado em muitos lugares do aplicativo. Frequentemente, temos visto comportamentos estranhos de um aplicativo - apenas para descobrir que um pedaço de código raramente usado (personalizado) não foi testado completamente para caracteres especiais. Você deveria evitá-los, se puder. E se você não puder, planeje um teste minucioso.

3. No CRX, os recursos têm subrecursos. Por exemplo, uma página terá várias subpáginas. Isso não pode ser correspondido em um sistema de arquivos, pois os sistemas de arquivos têm arquivos ou pastas.

#### URLs sem extensão não são armazenados em cache

Os URLs sempre devem ter uma extensão. Embora seja possível enviar URLs sem extensões no AEM. Esses URLs não serão armazenados em cache no Dispatcher.

**Exemplos**

`http://domain.com/home.html` é  **armazenável em cache**

`http://domain.com/home` não é  **armazenável em cache**

A mesma regra se aplica quando o URL contém um sufixo. O sufixo precisa ter uma extensão para poder ser armazenado em cache.

**Exemplos**

`http://domain.com/home.html/path/suffix.html` é  **armazenável em cache**

`http://domain.com/home.html/path/suffix` não é  **armazenável em cache**

Você pode se perguntar, o que acontece se a parte do recurso não tiver uma extensão, mas o sufixo tiver uma? Bem, neste caso o URL não tem nenhum sufixo. Veja o próximo exemplo:

**Exemplo**

`http://domain.com/home/path/suffix.ext`

O `/home/path/suffix` é o caminho para o recurso... portanto, não há sufixo no URL.

**Conclusão**

Sempre adicione extensões ao caminho e ao sufixo. As pessoas sensíveis a SEO às vezes argumentam que isto está a classificar-vos nos resultados da pesquisa. Mas uma página sem cache seria super lenta e ainda mais detalhada.

#### URLs de sufixo conflitantes

Considere que você tem dois URLs válidos

`http://domain.com/home.html`

e

`http://domain.com/home.html/suffix.html`

Eles são absolutamente válidos em AEM. Você não veria nenhum problema em sua máquina de desenvolvimento local (sem um Dispatcher). Provavelmente você também não encontrará nenhum problema no teste de UAT ou de carga. O problema que estamos a enfrentar é tão sutil que cai pela maioria dos testes.  Isso lhe atingirá muito quando você estiver no horário de pico e você estará limitado no tempo para solucioná-lo, provavelmente não terá acesso ao servidor nem recursos para corrigi-lo. Estivemos lá...

Então... qual é o problema?

`home.html` em um sistema de arquivos pode ser um arquivo ou uma pasta. Não tanto ao mesmo tempo como em AEM.

Se você solicitar `home.html` primeiro, ele será criado como um arquivo.

As solicitações subsequentes para `home.html/suffix.html` retornam resultados válidos, mas como o arquivo `home.html` &quot;bloqueia&quot; a posição no sistema de arquivos, `home.html` não pode ser criado uma segunda vez como pasta e, portanto, `home.html/suffix.html` não é armazenado em cache.

![Posição de bloqueio de arquivos no sistema de arquivos impedindo que subrecursos sejam armazenados em cache](assets/chapter-1/file-blocking-position-in-filesystem.png)

*Posição de bloqueio de arquivos no sistema de arquivos impedindo que subrecursos sejam armazenados em cache*

<br> 

Se você fizer o contrário, primeiro solicitando `home.html/suffix.html`, `suffix.html` será armazenado em cache em uma pasta `/home.html` no início. No entanto, essa pasta é excluída e substituída por um arquivo `home.html` quando você solicitar `home.html` posteriormente como um recurso.

![Excluindo uma estrutura de caminho quando um pai é buscado como um recurso](assets/chapter-1/deleting-path-structure.png)

*Excluindo uma estrutura de caminho quando um pai é buscado como um recurso*

<br> 

Portanto, o resultado do que é armazenado em cache é inteiramente aleatório e depende da ordem das solicitações recebidas. O que torna as coisas ainda mais complicadas, é o fato de você geralmente ter mais de um expedidor. Além disso, o desempenho, a taxa de ocorrência e o comportamento do cache podem variar de forma diferente de um Dispatcher para outro. Se você quiser descobrir por que seu site não responde, verifique se você está olhando para o Dispatcher correto com a infeliz ordem de armazenamento em cache. Se você estiver olhando para o Dispatcher que - por sorte - teve um padrão de solicitação mais favorável, você se perderá ao tentar encontrar o problema.

#### Como evitar URLs conflitantes

Você pode evitar &quot;URLs conflitantes&quot;, onde um nome de pasta e um nome de arquivo &quot;compete&quot; pelo mesmo caminho no sistema de arquivos, quando estiver usando uma extensão diferente para o recurso quando tiver um sufixo.

**Exemplo**

* `http://domain.com/home.html`

* `http://domain.com/home.dir/suffix.html`

Ambos são perfeitamente acessíveis.

![](assets/chapter-1/cacheable.png)

Escolhendo uma extensão dedicada &quot;dir&quot; para um recurso quando você solicita um sufixo ou evita usar o sufixo completamente. Há casos raros em que são úteis. E é fácil implementar esses casos corretamente.  Como veremos no próximo capítulo quando falamos de invalidação e descarga de cache.

#### Solicitações Não Acessíveis ao Cache

Vamos revisar um resumo rápido do último capítulo mais algumas exceções. O Dispatcher pode armazenar um URL em cache se ele estiver configurado como sendo armazenável em cache e se for uma solicitação de GET. Ele não pode ser armazenado em cache com uma das exceções a seguir.

**Solicitações de cache**

* A solicitação está configurada para ser armazenada em cache na configuração do Dispatcher
* Solicitação é uma solicitação de GET simples

**Solicitações ou Respostas Não-acessíveis**

* Solicitação negada ao armazenar em cache por configuração (Caminho, Padrão, Tipo MIME)
* Respostas que retornam um &quot;Dispatcher: no-cache&quot; cabeçalho
* Resposta que retorna um &quot;Cache-Control: cabeçalho &quot;no-cache|private&quot;
* Resposta que retorna um &quot;Pragma: no-cache&quot; cabeçalho
* Solicitação com parâmetros de query
* URL sem extensão
* URL com um sufixo que não tem uma extensão
* Resposta que retorna um código de status diferente de 200
* POST

## Invalidando e liberando o cache

### Visão geral

O último capítulo listou um grande número de exceções, quando o Dispatcher não pode armazenar uma solicitação em cache. Mas há mais coisas a considerar: Como o Dispatcher _pode_ armazena uma solicitação em cache, isso não significa necessariamente que _deveria_.

A questão é: O cache geralmente é fácil. O Dispatcher só precisa armazenar o resultado de uma resposta e retorná-la na próxima vez que a mesma solicitação for recebida. Direito? Errado!

A parte difícil é a _invalidation_ ou _descarga_ do cache. O Dispatcher precisa descobrir quando um recurso mudou e precisa ser renderizado novamente.

Esta parece ser uma tarefa trivial à primeira vista... mas não é. Leia mais a fundo e você descobrirá algumas diferenças delicadas entre recursos simples e simples e páginas que dependem de uma estrutura altamente interligada de vários recursos.

### Recursos simples e descarga

Configuramos nosso sistema AEM para criar dinamicamente uma execução em miniatura para cada imagem quando solicitado com um seletor especial de &quot;polegar&quot;:

`/content/dam/path/to/image.thumb.png`

E - claro - fornecemos um URL para servir a imagem original com um URL sem seletor:

`/content/dam/path/to/image.png`

Se baixarmos ambos, a miniatura e a imagem original, acabaremos com algo como:

```
/var/cache/dispatcher/docroot/content/dam/path/to/image.thumb.png

/var/cache/dispatcher/docroot/content/dam/path/to/image.png
```

no sistema de arquivos do Dispatcher.

Agora, o usuário carrega e ativa uma nova versão desse arquivo. Em última análise, um pedido de invalidação é enviado do AEM para o Dispatcher,

```
GET /invalidate
invalidate-path:  /content/dam/path/to/image

<no body>
```

A invalidação é tão fácil quanto: Uma solicitação de GET simples para um URL especial &quot;/invalidate&quot; no Dispatcher. Não é necessário um corpo HTTP, a &quot;carga&quot; é apenas o cabeçalho &quot;invalidate-path&quot;. Observe também que o invalidate-path no cabeçalho é o recurso que AEM conhecido - e não o arquivo ou arquivos que o Dispatcher armazenou em cache. AEM só sabe dos recursos. Extensões, seletores e sufixos são usados em tempo de execução quando um recurso é solicitado. AEM não executa nenhuma contabilidade sobre quais seletores foram usados em um recurso, portanto, o caminho do recurso é tudo o que ele sabe ao ativar um recurso.

No nosso caso, isso é suficiente. Se um recurso mudou, podemos assumir com segurança que todas as renderizações desse recurso também mudaram. Em nosso exemplo, se a imagem mudou, uma nova miniatura também será renderizada.

O Dispatcher pode excluir com segurança o recurso com todas as execuções que ele armazenou em cache. Vai fazer algo como:

`$ rm /content/dam/path/to/image.*`

removendo `image.png` e `image.thumb.png` e todas as outras execuções que correspondem a esse padrão.

Muito simples... desde que você use um recurso apenas para responder a uma solicitação.

### Referências e conteúdo em malha

#### O problema de conteúdo em malha

Em contraste com imagens ou outros arquivos binários carregados em AEM, as páginas HTML não são animais solitários. Vivem em bandos e estão altamente interligados entre si por hiperlinks e referências. A simples ligação é inofensiva, mas torna-se complicada quando falamos de referências de conteúdo. A navegação superior onipresente ou os teasers nas páginas são referências de conteúdo.

#### Referências de conteúdo e por que elas são um problema

Vejamos um exemplo simples. Uma agência de viagens tem uma página que promove uma viagem ao Canadá. Esta promoção aparece na seção do teaser em duas outras páginas, na página &quot;Início&quot; e em uma página &quot;Especiais de inverno&quot;.

Como ambas as páginas exibem o mesmo teaser, seria desnecessário solicitar ao autor que crie o teaser várias vezes para cada página em que ele deve ser exibido. Em vez disso, a página de público alvo &quot;Canadá&quot; reserva uma seção nas propriedades da página para fornecer as informações para o teaser - ou melhor, fornecer um URL que renderize totalmente esse teaser:

`<sling:include resource="/content/home/destinations/canada" addSelectors="teaser" />`

ou

`<sling:include resource="/content/home/destinations/canada/jcr:content/teaser" />`

![](assets/chapter-1/content-references.png)

Somente AEM funciona como charm, mas se você usar um Dispatcher na instância Publicar algo estranho acontece.

Imaginem, vocês publicaram o vosso website. O título na sua página do Canadá é &quot;Canadá&quot;. Quando um visitante solicita seu home page - que tem uma referência teaser àquela página - o componente na página &quot;Canadá&quot; renderiza algo como

```
<div class="teaser">
  <h3>Canada</h3>
  <img …>
</div>
```

*no* home page. O home page é armazenado pelo Dispatcher como um arquivo .html estático, incluindo o teaser e seu título no arquivo.

Agora o comerciante aprendeu que as manchetes de teaser devem ser acionáveis. Então, ele decide mudar o título de &quot;Canadá&quot; para &quot;Visitar Canadá&quot; e também atualiza a imagem.

Ele publica a página editada &quot;Canadá&quot; e revisita o home page publicado anteriormente para ver suas mudanças. Mas nada mudou lá. Ainda exibe o velho teaser. O duplo verifica o &quot;Especial de inverno&quot;. Essa página nunca foi solicitada antes e, portanto, não é armazenada em cache estaticamente no Dispatcher. Portanto, esta página é renderizada recentemente pelo Publish e agora esta página contém o novo teaser &quot;Visit Canada&quot;.

![O Dispatcher que armazena conteúdo obsoleto incluído no home page](assets/chapter-1/dispatcher-storing-stale-content.png)

*O Dispatcher que armazena conteúdo obsoleto incluído no home page*

<br> 

O que aconteceu? O Dispatcher armazena uma versão estática de uma página que contém todo o conteúdo e marcação que foram obtidos de outros recursos durante a renderização.

O Dispatcher, sendo um mero servidor web baseado em sistema de arquivos, é rápido, mas também relativamente simples. Se um recurso incluído mudar, ele não percebe isso. Ele ainda se prende ao conteúdo que estava lá quando a página de inclusão foi renderizada.

A página &quot;Especial de inverno&quot; ainda não foi renderizada, portanto, não há versão estática no Dispatcher e, portanto, será exibida com o novo teaser, pois ele será renderizado recentemente mediante solicitação.

Você pode pensar que o Dispatcher deve rastrear todos os recursos que ele toca ao renderizar e liberar todas as páginas que usaram esse recurso, quando esse recurso for alterado. Mas o Dispatcher não renderiza as páginas. A renderização é executada pelo sistema de publicação. O Dispatcher não sabe quais recursos vão para um arquivo .html renderizado.

Ainda não está convencido? Você pode pensar *&quot;deve haver uma maneira de implementar algum tipo de rastreamento de dependência&quot;*. Bem, há, ou mais precisamente, *estava*. Comuniqué 3 o bisavô do AEM tinha um rastreador de dependência implementado na _sessão_ que foi usada para renderizar uma página.

Durante uma solicitação, cada recurso adquirido por meio desta sessão foi rastreado como uma dependência do URL que estava sendo renderizado no momento.

Mas acontece que rastrear as dependências era muito caro. As pessoas logo descobriram que o site é mais rápido se desligarem completamente o recurso de rastreamento de dependência e dependerem da renderização de todas as páginas html após uma página html ser alterada. Além disso, esse regime também não era perfeito - existiam várias armadilhas e exceções no caminho. Em alguns casos, você não estava usando a sessão padrão de solicitações para obter um recurso, mas uma sessão de administrador para obter alguns recursos auxiliares para renderizar uma solicitação. Essas dependências normalmente não eram rastreadas e resultavam em dores de cabeça e chamadas telefônicas para a equipe de operações solicitando o liberação manual do cache. Você teve sorte se eles tivessem um procedimento padrão para fazer isso. Havia mais coisas no caminho, mas... vamos parar de lembrar. Isto leva-nos a 2005. Em última análise, esse recurso foi desativado no Comunicado 4 por padrão e não voltou ao sucessor do CQ5, que então se tornou AEM.

### Invalidação automática

#### Quando A Limpeza Total É Mais Barata Do Que O Rastreamento De Dependência

Como o CQ5 depende inteiramente da invalidação, mais ou menos, do site inteiro se apenas uma das páginas mudar. Esse recurso é chamado de &quot;Invalidação automática&quot;.

Mas novamente - como pode ser, que jogar fora e renderizar centenas de páginas é mais barato do que fazer um rastreamento adequado de dependência e renderização parcial?

Há duas razões principais:

1. Em um site médio, apenas um pequeno subconjunto das páginas é frequentemente solicitado. Então mesmo, se você jogar fora todo o conteúdo renderizado, apenas algumas dúzias serão solicitadas imediatamente depois. A renderização da parte longa das páginas pode ser distribuída ao longo do tempo, quando elas são realmente solicitadas. Então, na verdade, a carga nas páginas de renderização não é tão alta quanto você poderia esperar. É claro que sempre há exceções... discutiremos alguns truques para lidar com a mesma distribuição de disponibilidade em sites maiores com caches Dispatcher vazios, mais tarde.

2. Todas as páginas são conectadas pela navegação principal assim mesmo. Assim, quase todas as páginas são dependentes umas das outras. Isso significa que até o rastreador de dependência mais inteligente descobrirá o que já sabemos: Se uma das páginas mudar, você terá que invalidar todas as outras.

Você não acredita? Vamos ilustrar o último ponto.

Estamos usando o mesmo argumento do último exemplo com teasers que se referem ao conteúdo de uma página remota. Só agora estamos usando um exemplo mais extremo: Uma Navegação principal automaticamente renderizada. Assim como no teaser, o título de navegação é extraído da página vinculada ou &quot;remota&quot; como uma referência de conteúdo. Os títulos de navegação remota não são armazenados na página renderizada no momento. Lembre-se de que a navegação é renderizada em cada página do site. Assim, o título de uma página é usado várias vezes em todas as páginas que têm uma navegação principal. E se você quiser alterar um título de navegação, você deseja fazer isso apenas uma vez na página remota - não em cada página que faz referência à página.

Assim, no nosso exemplo, a navegação une todas as páginas usando &quot;NavTitle&quot; da página do público alvo para renderizar um nome na navegação. O título de navegação para &quot;Islândia&quot; é extraído da página &quot;Islândia&quot; e renderizado em cada página que tem uma navegação principal.

![Navegação principal inevitavelmente mesclando o conteúdo de todas as páginas, puxando seus &quot;NavTítulos&quot;](assets/chapter-1/nav-titles.png)

*Navegação principal inevitavelmente mesclando o conteúdo de todas as páginas, puxando seus &quot;NavTítulos&quot;*

<br> 

Se você alterar o NavTitle na página da Islândia de &quot;Islândia&quot; para &quot;Bela Islândia&quot;, esse título será imediatamente alterado no menu principal de todas as outras páginas. Assim, as páginas renderizadas e armazenadas em cache antes dessa alteração, todas se tornam obsoletas e precisam ser invalidadas.

#### Como a Invalidação Automática é implementada: O arquivo .stat

Agora, se você tiver um site grande com milhares de páginas, levaria algum tempo para executar um loop em todas as páginas e excluí-las fisicamente. Durante esse período, o Dispatcher poderia servir, de forma não intencional, conteúdo obsoleto. Pior ainda, podem ocorrer alguns conflitos ao acessar os arquivos de cache, talvez uma página seja solicitada enquanto está sendo excluída ou uma página seja excluída novamente devido a uma segunda invalidação que ocorreu após uma ativação subsequente imediata. Considere que confusão isso seria. Felizmente, não é isso que acontece. O Dispatcher usa um truque inteligente para evitar que: Em vez de excluir centenas e milhares de arquivos, coloca um arquivo simples e vazio na raiz do sistema de arquivos quando um arquivo é publicado e, portanto, todos os arquivos dependentes são considerados inválidos. Esse arquivo é chamado de &quot;statfile&quot;. O arquivo de status é um arquivo vazio - o que importa sobre o arquivo de status é apenas a data de criação.

Todos os arquivos no dispatcher, que têm uma data de criação anterior ao arquivo de status, foram renderizados antes da última ativação (e invalidação) e, portanto, são considerados &quot;inválidos&quot;. Eles ainda estão fisicamente presentes no sistema de arquivos, mas o Dispatcher os ignora. Eles são &quot;obsoletos&quot;. Sempre que uma solicitação para um recurso obsoleto é feita, o Dispatcher solicita que o sistema AEM renderize novamente a página. A página renderizada recentemente é armazenada no sistema de arquivos - agora com uma nova data de criação e é atualizada novamente.

![A data de criação do arquivo .stat define qual conteúdo é obsoleto e qual é atualizado](assets/chapter-1/creation-date.png)

*A data de criação do arquivo .stat define qual conteúdo é obsoleto e qual é atualizado*

<br> 

Você pode perguntar por que se chama &quot;.stat&quot;? E talvez não &quot;invalidado&quot;? Bem, você pode imaginar, ter esse arquivo no seu sistema de arquivos ajuda o Dispatcher a determinar quais recursos podem *estaticamente* ser oferecidos - exatamente como em um servidor Web estático. Esses arquivos não precisam mais ser renderizados dinamicamente.

A verdadeira natureza do nome, porém, é menos metafórica. É derivado da chamada do sistema Unix `stat()`, que retorna a hora de modificação de um arquivo (entre outras propriedades).

#### Misturar validação simples e automática

Mas esperem... mais cedo dissemos que os recursos únicos são excluídos fisicamente. Agora dizemos que um arquivo de status mais recente praticamente os tornaria inválidos aos olhos do Dispatcher. Por que a exclusão física, primeiro?

A resposta é simples. Geralmente se usam ambas as estratégias em paralelo - mas para diferentes tipos de recursos. Ativos binários, como imagens são independentes. Eles não estão conectados a outros recursos de uma forma que eles precisam que suas informações sejam disponibilizadas.

Por outro lado, as páginas HTML são altamente interdependentes. Então, você aplicaria a invalidação automática neles. Essa é a configuração padrão no Dispatcher. Todos os arquivos que pertencem a um recurso invalidado são excluídos fisicamente. Além disso, os arquivos que terminam com &quot;.html&quot; são invalidados automaticamente.

O Dispatcher decide sobre a extensão de arquivo, se o esquema de invalidação automática deve ou não ser aplicado.

As terminações de arquivo para a invalidação automática são configuráveis. Em teoria, você poderia incluir todas as extensões para a invalidação automática. Mas lembrem-se, que isto tem um preço muito alto. Você não verá recursos obsoletos entregues de forma não intencional, mas o desempenho do delivery diminui consideravelmente devido à invalidação excessiva.

Imagine, por exemplo, que você implemente um esquema em que PNGs e JPGs são renderizados dinamicamente e dependem de outros recursos para isso. Talvez você queira redimensionar imagens de alta resolução para uma resolução menor compatível com a Web. Enquanto estiver nisso, altere também a taxa de compactação. A resolução e a taxa de compactação neste exemplo não são constantes fixas, mas parâmetros configuráveis no componente que usa a imagem. Agora, se esse parâmetro for alterado, será necessário invalidar as imagens.

Sem problemas - acabamos de saber que poderíamos adicionar imagens à invalidação automática e sempre ter imagens renderizadas recentemente sempre que algo mudasse.

#### Jogando fora o bebê com a água da praia

Isso mesmo - e isso é um grande problema. Leia o último parágrafo novamente. &quot;...imagens renderizadas recentemente sempre que tudo muda.&quot; Como sabem, um bom website é constantemente alterado. adicionar novo conteúdo aqui, corrigir um erro de digitação ali, fazer um teaser em outro lugar. Isso significa que todas as suas imagens são invalidadas constantemente e precisam ser renderizadas novamente. Não subestime isso. A renderização e transferência dinâmicas de dados de imagem funcionam em milissegundos na máquina de desenvolvimento local. Seu ambiente de produção precisa fazer isso cem vezes mais - por segundo.

E sejamos claros aqui, seus jpgs precisam ser renderizados novamente, quando uma página html muda e vice-versa. Há apenas um &quot;bucket&quot; de arquivos a serem invalidados automaticamente. É cortado como um todo. Sem qualquer forma de desagregação em outras estruturas detalhadas.

Há um bom motivo pelo qual a invalidação automática é mantida em &quot;.html&quot; por padrão. O objetivo é manter esse balde o mais pequeno possível. Não jogue fora o bebê com a água do banho simplesmente invalidando tudo - apenas para estar do lado seguro.

Os recursos autônomos devem ser servidos no caminho desse recurso. Isso ajuda muito a invalidar. Mantenha simples, não crie esquemas de mapeamento como &quot;recurso /a/b/c&quot; servido de &quot;/x/y/z&quot;. Faça com que seus componentes funcionem com as configurações padrão de invalidação automática do Dispatcher. Não tente reparar um componente mal projetado com invalidação excessiva no Dispatcher.

##### Exceções à Invalidação Automática: Invalidação ResourceOnly

A solicitação de invalidação do Dispatcher geralmente é acionada dos sistemas de publicação por um agente de replicação.

Se você se sentir super confiante sobre suas dependências, poderá tentar criar seu próprio agente de replicação invalidante.

Seria um pouco além desse guia para entrar nos detalhes, mas queremos dar a vocês pelo menos algumas dicas.

1. Realmente saiba o que você está fazendo. É muito difícil acertar a invalidação. Essa é uma das razões pelas quais a invalidação automática é tão rigorosa. para evitar o fornecimento de conteúdo obsoleto.

2. Se o agente enviar um cabeçalho HTTP `CQ-Action-Scope: ResourceOnly`, isso significa que essa única solicitação de invalidação não aciona uma invalidação automática. Este código ( [https://github.com/cqsupport/webinar-dispatchercache/tree/master/src/refetching-flush-agent/refetch-bundle](https://github.com/cqsupport/webinar-dispatchercache/tree/master/src/refetching-flush-agent/refetch-bundle)) pode ser um bom ponto de partida para o seu próprio agente de replicação.

3. `ResourceOnly`, só impede a invalidação automática. Para realmente fazer a resolução e as invalidações de dependência necessárias, você mesmo deve acionar as solicitações de invalidação. Verifique as regras de liberação do Dispatcher ([https://adobe-consulting-services.github.io/acs-aem-commons/features/dispatcher-flush-rules/index.html](https://adobe-consulting-services.github.io/acs-aem-commons/features/dispatcher-flush-rules/index.html)) do pacote para obter mais informações sobre como isso pode realmente acontecer.

Não recomendamos que você crie um esquema de resolução de dependência. Há apenas demasiado esforço e pouco ganho - e, como já foi dito, há demasiado que os senhores vão errar.

Em vez disso, o que você deve fazer é descobrir quais recursos não têm nenhuma dependência de outros recursos e podem ser invalidados sem a invalidação automática. Entretanto, não é necessário usar um Agente de Replicação personalizado. Basta criar uma regra personalizada na configuração do Dispatcher que exclua esses recursos da invalidação automática.

Dissemos que a navegação principal ou os teasers são uma fonte de dependências. Bem - se você carregar a navegação e os teasers de forma assíncrona ou incluí-los com um script SSI no Apache, você não terá essa dependência para rastrear. Nós vamos nos desenvolver sobre como carregar componentes de forma assíncrona mais adiante neste documento quando falarmos sobre &quot;Sling Dynamic Includes&quot;.

O mesmo se aplica para janelas pop-up ou conteúdo que é carregado em um lightbox. Essas peças também raramente têm navegação (também conhecidas como &quot;dependências&quot;) e podem ser invalidadas como um único recurso.

## Criando componentes com o Dispatcher em mente

### Aplicação da Mecânica do Dispatcher em um Exemplo Real

No último capítulo explicamos como a mecânica básica do Dispatcher, como ela funciona em geral e quais são as limitações.

Agora queremos aplicar esses mecanismos a um tipo de componentes que você provavelmente encontrará nos requisitos de seu projeto. Escolhemos o componente deliberadamente para demonstrar problemas que vocês também enfrentarão mais cedo ou mais tarde. O medo não é - nem todos os componentes precisam dessa quantidade de consideração que iremos apresentar. Mas se virem a necessidade de construir um tal componente, vocês estão bem cientes das consequências e sabem como lidar com elas.

### O Padrão do Componente de spooling (Anti)

#### O componente de imagem responsiva

Ilustremos um padrão comum (ou anti-padrão) de um componente com binários interconectados. Criaremos um componente &quot;respi&quot; - para &quot;imagem responsiva&quot;. Esse componente deve ser capaz de adaptar a imagem exibida ao dispositivo no qual ela é exibida. Em desktops e tablets, mostra a resolução total da imagem, em telefones, uma versão menor com um corte estreito - ou até mesmo um motivo completamente diferente (isto é chamado de &quot;direção artística&quot; no mundo responsivo).

Os ativos são carregados na área DAM do AEM e somente _referenciados_ no componente de imagem responsiva.

O respi-componente cuida da renderização da marcação e fornece os dados da imagem binária.

A forma como o implementamos aqui é um padrão comum que vimos em muitos projetos e até mesmo um dos componentes principais AEM é baseado nesse padrão. Portanto, é muito provável que você, como desenvolvedor, possa adaptar esse padrão. Ele tem seus pontos doces em termos de encapsulamento, mas requer muito esforço para prepará-lo para o Dispatcher. Discutiremos várias opções para mitigar o problema mais tarde.

Nós chamamos o padrão usado aqui de &quot;Padrão Spooler&quot;, porque o problema remonta aos primeiros dias do Comunicado 3 onde havia um método &quot;spool&quot; que poderia ser chamado em um recurso para transmitir seus dados brutos binários na resposta.

O termo original &quot;spooling&quot; refere-se, na verdade, a periféricos offline lentos compartilhados, como impressoras, por isso não é aplicado aqui corretamente. Mas nós gostamos do termo de qualquer forma porque raramente no mundo online é distinguível. E cada padrão deveria ter um nome distinguível de qualquer maneira, certo? Cabe a vocês decidir se isto é um padrão ou um padrão anti-padrão.

#### Implementação

Veja como nosso componente de imagem responsiva é implementado:

O componente tem duas partes; a primeira parte renderiza a marcação HTML da imagem, a segunda parte &quot;spool&quot; os dados binários da imagem referenciada. Como este é um site moderno com um design responsivo, não estamos renderizando uma tag `<img src"…">` simples, mas um conjunto de imagens na tag `<picture/>`. Para cada dispositivo, carregamos duas imagens diferentes no DAM e as referenciamos do componente de imagem.

O componente tem três scripts de renderização (implementados em JSP, HTL ou como um servlet) cada um endereçado com um seletor dedicado:

1. `/respi.jsp` - sem seletor para renderizar a marcação HTML
2. `/respi.img.java` para renderizar a versão da área de trabalho
3. `/respi.img.mobile.java` para renderizar a versão móvel.


O componente é colocado no parsys da página inicial. A estrutura resultante no CRX é ilustrada abaixo.

![Estrutura de recursos da imagem responsiva no CRX](assets/chapter-1/responsive-image-crx.png)

*Estrutura de recursos da imagem responsiva no CRX*

<br> 

A marcação de componentes é renderizada desta forma.

```plain
  #GET /content/home.html

  <html>

  …

  <div class="responsive-image>

  <picture>
    <source src="/content/home/jcr:content/par/respi.img.mobile.jpg" …/>
    <source src="/content/home/jcr:content/par/respi.img.jpg …/>

    …

  </picture>
  </div>
  …
```

e... terminamos com nosso componente bem encapsulado.

#### Componente de imagem responsiva em ação

Agora, um usuário solicita a página - e os ativos por meio do Dispatcher. Isso resulta em arquivos no sistema de arquivos Dispatcher, conforme ilustrado abaixo,

![Estrutura em cache do componente de imagem responsiva encapsulada](assets/chapter-1/cached-structure-encapsulated-image-comonent.png)

*Estrutura em cache do componente de imagem responsiva encapsulada*

<br> 

Considere que um usuário carregue e ativa uma nova versão das duas imagens em flor no DAM. AEM enviará de acordo com a solicitação de invalidação para

`/content/dam/flower.jpg`

e

`/content/dam/flower-mobile.jpg`

ao Dispatcher. No entanto, estes pedidos são em vão. O conteúdo foi armazenado em cache como arquivos abaixo da subestrutura do componente. Esses arquivos agora são obsoletos, mas ainda são atendidos mediante solicitações.

![Incompatibilidade de estrutura que leva a conteúdo obsoleto](assets/chapter-1/structure-mismatch.png)

*Incompatibilidade de estrutura que leva a conteúdo obsoleto*

<br> 

Há outra advertência para esta abordagem. Considere que você usa a mesma flor.jpg em várias páginas. Em seguida, você terá o mesmo ativo em cache em vários URLs ou arquivos,

```
/content/home/products/jcr:content/par/respi.img.jpg

/content/home/offers/jcr:content/par/respi.img.jpg

/content/home/specials/jcr:content/par/respi.img.jpg

…
```

Cada vez que uma página nova e não armazenada em cache é solicitada, os ativos são obtidos AEM em URLs diferentes. Nenhum armazenamento em cache do Dispatcher e nenhum armazenamento em cache do navegador pode acelerar o delivery.

#### Onde o padrão de Spooler brilha

Há uma exceção natural, em que esse padrão, mesmo em sua forma simples, é útil: Se o binário for armazenado no próprio componente - e não no DAM. No entanto, isso é útil apenas para imagens usadas uma vez no site, e não armazenar ativos no DAM significa que você tem dificuldade para gerenciar seus ativos. Imagine que sua licença de uso para um ativo específico está esgotada. Como descobrir em quais componentes você usou o ativo?

Está vendo? O &quot;M&quot; no DAM significa &quot;Gerenciamento&quot; - como no Gerenciamento de ativos digitais. Você não quer dar esse recurso.

#### Conclusão

Da perspectiva de um desenvolvedor AEM o padrão parecia super elegante. Mas com o Dispatcher levado em conta a equação, vocês podem concordar, que a abordagem ingênua pode não ser suficiente.

Deixamos que vocês decidam se este é um padrão ou um padrão anti-padrão por enquanto. E talvez você já tenha algumas boas ideias em mente sobre como mitigar os problemas explicados acima? Bom. Então, você estará ansioso para ver como outros projetos resolveram essas questões.

### Resolvendo problemas comuns do Dispatcher

#### Visão geral

Vamos falar sobre como isso poderia ter sido implementado com um pouco mais de cache. Há várias opções. Às vezes não se pode escolher a melhor solução. Talvez você entre em um projeto já em execução e tenha um orçamento limitado para apenas resolver o &quot;problema do cache&quot; disponível e não o suficiente para fazer uma refatoração completa. Ou você enfrenta um problema, que é mais complexo do que o componente de imagem de exemplo.

Iremos descrever os princípios e as advertências nas seções que se seguem.

Novamente, isso é baseado na experiência real. Já vimos todos esses padrões na natureza então não é um exercício acadêmico. É por isso que estamos a mostrar-vos alguns padrões antirpadrões, pelo que têm a oportunidade de aprender com os erros que outros já cometeram.

#### Matador de cache

>[!WARNING]
>
>Isto é um anti-padrão. Não o utilize. Nunca.

Você já viu parâmetros de query como `?ck=398547283745`? Eles são chamados de cache-killer (&quot;ck&quot;). A ideia é que, se você adicionar qualquer parâmetro de query, o recurso não será acessado. Além disso, se você adicionar um número aleatório como valor do parâmetro (como &quot;398547283745&quot;), o URL se tornará único e você se certificar de que nenhum outro cache entre o sistema de AEM e sua tela seja capaz de armazenar em cache. Suspeitos intermediários normais seriam um cache &quot;Varnish&quot; na frente do Dispatcher, um CDN ou até mesmo o cache do navegador. Novamente: Não faça isso. Você quer que seus recursos sejam armazenados em cache tanto quanto possível. O cache é seu amigo. Não mate os amigos.

#### Invalidação automática

>[!WARNING]
>
>Isto é um anti-padrão. Evite usá-lo para ativos digitais. Tente manter a configuração padrão do Dispatcher, que > é a invalidação automática para arquivos &quot;.html&quot;, somente

Em um curto prazo, você pode adicionar &quot;.jpg&quot; e &quot;.png&quot; à configuração de invalidação automática no Dispatcher. Isso significa que sempre que uma invalidação ocorre, todos os &quot;.jpg&quot;, &quot;.png&quot; e &quot;.html&quot; precisam ser renderizados novamente.

Esse padrão é implementado com muita facilidade se os proprietários de negócios se queixarem de não verem suas alterações se concretizarem no site ativo com rapidez suficiente. Mas isso só pode lhe dar algum tempo para encontrar uma solução mais sofisticada.

Certifique-se de entender os amplos impactos no desempenho. Isso irá retardar significativamente o seu site e poderá até mesmo afetar a estabilidade - se o seu site for um site de grande carga com mudanças frequentes - como um portal de notícias.

#### Impressão digital de URL

Uma impressão digital de URL parece um assassino de cache. Mas não é. Não é um número aleatório, mas um valor que caracteriza o conteúdo do recurso. Isso pode ser um hash do conteúdo do recurso ou - ainda mais simples - um carimbo de data e hora quando o recurso foi carregado, editado ou atualizado.

Um carimbo de data e hora Unix é suficiente para uma implementação real. Para melhorar a legibilidade, estamos usando um formato mais legível neste tutorial: `2018 31.12 23:59 or fp-2018-31-12-23-59`.

A impressão digital não deve ser usada como parâmetro de query, como URLs com parâmetros de query   não pode ser armazenado em cache. Você pode usar um seletor ou o sufixo para a impressão digital.

Suponhamos que o arquivo `/content/dam/flower.jpg` tenha uma data `jcr:lastModified` de 31 de dezembro em 2018, 23:59. O URL com a impressão digital é `/content/home/jcr:content/par/respi.fp-2018-31-12-23-59.jpg`.

Esse URL permanece estável, desde que o arquivo de recurso referenciado (`flower.jpg`) não seja alterado. Então ele pode ser armazenado em cache por um tempo indefinido e não é um assassino em cache.

Observe que esse URL precisa ser criado e servido pelo componente de imagem responsiva. Não é uma funcionalidade AEM pronta para uso.

Esse é o conceito básico. No entanto, há alguns detalhes que podem facilmente ser ignorados.

Em nosso exemplo, o componente foi renderizado e armazenado em cache às 23:59. Agora a imagem foi mudada, digamos às 00:00.  O componente _geraria_ um novo URL com impressão digital em sua marcação.

Você pode pensar que _deveria_... mas não acha. Como apenas o binário da imagem foi alterado e a página de inclusão não foi tocada, a renderização da marcação HTML não é necessária. Assim, o Dispatcher serve a página com a impressão digital antiga e, portanto, a versão antiga da imagem.

![Componente de imagem mais recente do que a imagem referenciada, nenhuma impressão digital fresca renderizada.](assets/chapter-1/recent-image-component.png)

*Componente de imagem mais recente do que a imagem referenciada, nenhuma impressão digital fresca renderizada.*

<br> 

Agora, se você reativasse o home page (ou qualquer outra página do site), o arquivo de status seria atualizado, o Dispatcher consideraria o arquivo home.html obsoleto e o renderizaria novamente com uma nova impressão digital no componente de imagem.

Mas não ativamos o home page, certo? E por que deveríamos ativar uma página que não tocamos mesmo assim? E além disso, talvez nós não tenhamos direitos suficientes para ativar páginas ou o fluxo de trabalho de aprovação é tão longo e demorado, que nós simplesmente não podemos fazer isso em breve. O que fazer?

#### A ferramenta de administrador lento - reduzindo os níveis de arquivo de status

>[!WARNING]
>
>Isto é um anti-padrão. Use-o apenas a curto prazo para ganhar algum tempo e encontrar uma solução mais sofisticada.

O administrador preguiçoso geralmente &quot;_define a invalidação automática para jpgs e o nível de arquivo de status como zero - o que sempre ajuda com problemas de armazenamento em cache de todos os tipos_.&quot; Você encontrará esse conselho em fóruns técnicos, e isso ajudará com seu problema de invalidação.

Até agora não discutimos o nível de arquivo de status. Basicamente, a invalidação automática só funciona para arquivos na mesma subárvore. Entretanto, o problema é que as páginas e os ativos normalmente não vivem na mesma subárvore. As páginas estão em algum lugar abaixo de `/content/mysite`, enquanto os ativos vivem abaixo de `/content/dam`.

O &quot;nível de arquivo de status&quot; define onde estão os nós raiz de profundidade das subárvores. No exemplo acima o nível seria &quot;2&quot; (1=/content, 2=/mysite,dam)

A ideia de &quot;diminuir&quot; o nível de arquivo de status para 0 basicamente é definir toda a árvore /conteúdo como uma única subárvore para que as páginas e os ativos vivam no mesmo domínio de invalidação automática. Então teríamos apenas uma árvore grande no nível (no ponto &quot;/&quot;). Mas isso invalida automaticamente todos os sites no servidor sempre que algo é publicado - mesmo em sites completamente independentes. Confie em nós: Essa é uma má ideia a longo prazo, pois você vai degradar bastante a taxa geral de ocorrência do cache. Tudo o que você pode fazer é esperar que seus servidores AEM tenham poder de fogo suficiente para serem executados sem cache.

Você entenderá todos os benefícios dos níveis mais profundos de arquivo de estado um pouco mais tarde.

#### Implementação de um agente de invalidação personalizado

De qualquer forma - precisamos informar ao Dispatcher de alguma forma, para invalidar as HTML-Pages se um &quot;.jpg&quot; ou &quot;.png&quot; tiver sido alterado para permitir a renderização com um URL atualizado.

O que vimos em projetos é, por exemplo, agentes de replicação especiais no sistema de publicação que enviam solicitações de invalidação para um site sempre que uma imagem desse site é publicada.

Aqui, ajuda muito se você puder derivar o caminho do site do caminho do ativo ao nomear a convenção.

Em geral, é uma boa ideia corresponder aos sites e caminhos de ativos como este:

**Exemplo**

```
/content/dam/site-a
/content/dam/site-b

/content/site-a
/content/site-b
```

Dessa forma, seu agente de Liberação do Dispatcher pode enviar e invalidar facilmente a solicitação para /content/site-a quando encontra uma alteração em `/content/dam/site-a`.

Na verdade, não importa qual caminho você diga ao Dispatcher para invalidar - contanto que ele esteja no mesmo site, na mesma &quot;subárvore&quot;. Você nem precisa usar um caminho de recurso real. Também pode ser &quot;virtual&quot;:

`GET /dispatcher-invalidate
Invalidate-path /content/mysite/dummy`

![](assets/chapter-1/resource-path.png)

1. Um ouvinte no sistema de publicação é acionado quando um arquivo no DAM é alterado

2. O listener envia uma solicitação de invalidação para o Dispatcher. Devido à invalidação automática, não importa qual caminho enviamos na invalidação automática, a menos que esteja na página inicial do site - ou seja mais preciso no nível de arquivo de status do site.

3. O arquivo de status é atualizado.

4. Na próxima vez que a página inicial for solicitada, ela será renderizada novamente. A nova impressão digital/data é tirada da propriedade lastModifiedda da imagem como um seletor adicional

5. Isso cria implicitamente uma referência para uma nova imagem

6. Se a imagem realmente for solicitada, uma nova representação será criada e armazenada no Dispatcher


#### A necessidade de limpeza

Ufa. Concluído. Rápido!

Bem... ainda não.

O caminho,

`/content/mysite/home/jcr:content/par/respi.img.fp-2018-31-12-23-59.jpg`

não diz respeito a nenhum dos recursos invalidados. Lembrar? Apenas invalidamos um recurso &quot;fictício&quot; e dependemos da invalidação automática para considerar &quot;início&quot; inválido. A própria imagem talvez nunca seja _fisicamente_ excluída. Assim, o cache crescerá, crescerá e crescerá. Quando as imagens são alteradas e ativadas, elas recebem novos nomes de arquivo no sistema de arquivos do Dispatcher.

Há três problemas para não excluir fisicamente os arquivos em cache e mantê-los indefinidamente:

1. Você está desperdiçando capacidade de armazenamento - obviamente. Concedido - O armazenamento tornou-se mais barato e mais barato nos últimos anos. Mas resoluções de imagens e tamanhos de arquivos também cresceram nos últimos anos - com o advento de telas tipo retina que estão famintas por imagens cristalinas.

2. Embora os discos rígidos tenham se tornado mais baratos, o &quot;armazenamento&quot; pode não ter se tornado mais barato. Vimos uma tendência de não ter um armazenamento de disco rígido (barato) sem metal, mas alugar armazenamento virtual em um NAS pelo seu provedor de data center. Este tipo de armazenamento é um pouco mais confiável e escalável, mas também um pouco mais caro. Talvez você não queira desperdiçá-lo armazenando lixo desatualizado. Isso não está relacionado apenas ao armazenamento principal - pense nos backups também. Se você tiver uma solução de backup predefinida, talvez não consiga excluir os diretórios de cache. No final, você também está fazendo backup dos dados de lixo.

3. Pior ainda: Você pode ter comprado licenças de uso para determinadas imagens apenas por um tempo limitado, contanto que precisasse delas. Agora, se você ainda armazenar a imagem depois que uma licença expirar, isso pode ser visto como uma violação de direitos autorais. Você pode não usar mais a imagem em suas páginas da Web, mas o Google ainda as encontrará.

Então, finalmente, você terá um trabalho de guarda para limpar todos os arquivos mais antigos que... digamos uma semana para manter esse tipo de lixo sob controle.

#### Abuso de impressões digitais de URL para ataques de negação de serviço

Mas esperem, há outra falha nesta solução:

Estamos meio que abusando de um seletor como parâmetro: fp-2018-31-12-23-59 é gerado dinamicamente como um tipo de &quot;cache-killer&quot;. Mas talvez algumas crianças entediadas (ou um rastreador de mecanismos de busca que enlouqueceu) start pedindo as páginas:

```
/content/mysite/home/jcr:content/par/img.fp-0000-00-00-00-00.jpg
/content/mysite/home/jcr:content/par/img.fp-0000-00-00-00-01.jpg
/content/mysite/home/jcr:content/par/img.fp-0000-00-00-00-02.jpg

…
```

Cada solicitação ignorará o Dispatcher, causando carregamento em uma instância de Publicação. E - pior ainda - criar um arquivo de acordo com o Dispatcher.

Então... em vez de usar a impressão digital como um simples matador de cache, você teria que verificar a data jcr:lastModifiedda da imagem e retornar um 404 se não fosse a data esperada. Isso leva algum tempo e ciclos de CPU no sistema de publicação... que é o que você queria evitar em primeiro lugar.

#### Detecções de impressões digitais de URL em versões de alta frequência

Você pode usar o schema de impressão digital não apenas para ativos que vêm do DAM, mas também para arquivos JS e CSS e recursos relacionados.

[O ](https://adobe-consulting-services.github.io/acs-aem-commons/features/versioned-clientlibs/index.html) Clientlibsis com versão é um módulo que usa essa abordagem.

Mas aqui você pode enfrentar outro problema com as impressões digitais do URL: Ela vincula o URL ao conteúdo. Não é possível alterar o conteúdo sem alterar o URL (por exemplo, atualizar a data de modificação). É para isso que as impressões digitais foram concebidas. Mas considere, você está lançando uma nova versão, com novos arquivos CSS e JS e, portanto, novos URLs com novas impressões digitais. Todas as suas páginas HTML ainda têm referências aos antigos URLs com impressão digital. Portanto, para que a nova versão funcione de forma consistente, é necessário invalidar todas as páginas HTML de uma só vez para forçar uma renderização com referências aos arquivos recém-impressos. Se você tiver vários sites confiando nas mesmas bibliotecas, isso pode ser uma quantidade considerável de renderização - e aqui você não pode aproveitar o `statfiles`. Esteja preparado para ver os picos de carga em seus sistemas de publicação após uma implementação. Você pode considerar uma implantação azul-esverdeada com aquecimento de cache ou talvez um cache baseado em TTL na frente do Dispatcher ... as possibilidades são infinitas.

#### Break Break

Uau - São muitos detalhes a serem considerados, certo? E recusa-se a ser entendida, testada e depurada facilmente. E tudo por uma solução aparentemente elegante. É certo que é elegante - mas apenas numa perspectiva AEM. Junto com o Dispatcher, fica desagradável.

E ainda assim - não resolve uma advertência básica, se uma imagem for usada várias vezes em páginas diferentes, elas serão armazenadas em cache nessas páginas. Não há muita sinergia em cache lá.

Em geral, a impressão digital de URL é uma boa ferramenta a ser utilizada no seu kit de ferramentas, mas é necessário aplicá-la com cuidado, pois pode causar novos problemas e, ao mesmo tempo, resolver apenas alguns problemas existentes.

Então... esse foi um longo capítulo. Mas nós temos visto este padrão com tanta frequência, que nós sentimos que é necessário dar a vocês toda a imagem com todos os prós e contras. As impressões digitais do URL resolvem alguns dos problemas inerentes no Padrão Spooler, mas o esforço para implementar é bastante alto e você precisa considerar outras soluções - mais fáceis - também. Nosso conselho é sempre verificar se você pode basear seus URLs nos caminhos de recursos fornecidos e se não tem um componente intermediário. Chegaremos a este ponto no próximo capítulo.

##### Resolução de Dependência de Tempo de Execução

A Resolução de Dependência de Tempo de Execução é um conceito que temos considerado em um projeto. Mas pensar através disso tornou-se bastante complexo e decidimos não implementá-lo.

Esta é a ideia básica:

O Dispatcher não sabe das dependências dos recursos. É só um monte de arquivos simples com pouca semântica.

AEM também pouco sobre dependências. Falta uma semântica adequada ou um &quot;rastreador de dependência&quot;.

AEM está ciente de algumas das referências. Ele usa esse conhecimento para avisá-lo quando você tenta excluir ou mover uma página ou ativo referenciado. Isso é feito consultando a pesquisa interna ao excluir um ativo. As referências de conteúdo têm um formulário muito específico. São expressões de caminho que começam com &quot;/content&quot;. Assim, eles podem facilmente ser indexados em texto completo - e consultados quando necessário.

Em nosso caso, precisaríamos de um agente de replicação personalizado no sistema de publicação, que acionasse uma pesquisa por um caminho específico quando esse caminho fosse alterado.

Vamos dizer

`/content/dam/flower.jpg`

Foi alterado em Publicar. O agente acionaria uma pesquisa por &quot;/content/dam/flower.jpg&quot; e encontraria todas as páginas que fazem referência a essas imagens.

Em seguida, ele pode emitir várias solicitações de invalidação para o Dispatcher. Uma para cada página que contém o ativo.

Em teoria, isso deveria funcionar. Mas apenas para dependências de primeiro nível. Você não gostaria de aplicar esse esquema para dependências de vários níveis, por exemplo, ao usar a imagem em um fragmento de experiência que é usado em uma página. Na verdade, acreditamos que a abordagem é demasiado complexa - e pode haver questões de tempo de execução. E geralmente o melhor conselho é não fazer computação cara nos manipuladores de eventos. E, especialmente, a procura pode tornar-se bastante dispendiosa.

##### Conclusão

Esperamos ter discutido o Spooler Pattern o suficiente para ajudá-lo a decidir quando usá-lo e não usá-lo na sua implementação.

## Evitar problemas do Dispatcher

### URLs baseados em recursos

Uma maneira muito mais elegante de resolver o problema de dependência é não ter dependências. Evite dependências artificiais que ocorrem ao usar um recurso para simplesmente proxy outro - como fizemos no último exemplo. Tente ver os recursos como entidades &quot;solitárias&quot; o mais frequentemente possível.

Nosso exemplo é facilmente resolvido:

![Colorir a imagem com um servlet vinculado à imagem, não ao componente.](assets/chapter-1/spooling-image.png)

*Colorir a imagem com um servlet vinculado à imagem, não ao componente.*

<br> 

Usamos os caminhos de recursos originais dos ativos para renderizar os dados. Se for necessário renderizar a imagem original como está, podemos usar AEM renderizador padrão para ativos.

Se precisarmos fazer algum processamento especial para um componente específico, registraríamos um servlet dedicado nesse caminho e o seletor para fazer a transformação em nome do componente. Fizemos isso aqui de forma exemplar com o &quot;.respi.&quot; seletor. É recomendável rastrear os nomes dos seletores usados no espaço global do URL (como `/content/dam`) e ter uma boa convenção de nomenclatura para evitar conflitos de nomes.

Aliás, não vemos problemas com a coerência do código. O servlet pode ser definido no mesmo pacote Java que o modelo de sling de componentes.

Podemos até usar seletores adicionais no espaço global como, por exemplo,

`/content/dam/flower.respi.thumbnail.jpg`

Fácil, certo? Então por que as pessoas inventam padrões complicados como o Spooler?

Bem, nós poderíamos resolver o problema evitando a referência de conteúdo interno porque o componente externo adicionou pouco valor ou informação à renderização do recurso interno, que poderia facilmente ser codificado em um conjunto de seletores estáticos que controlam a representação de um recurso solitário.

Mas há uma classe de casos que você não pode resolver facilmente com um URL baseado em recursos. Nós chamamos esse tipo de caso de &quot;Parâmetro que injeta componentes&quot; e os discutimos no próximo capítulo.

### Componentes injetáveis do parâmetro

#### Visão geral

O Spooler no último capítulo era apenas um fino invólucro em torno de um recurso. Causou mais problemas do que ajudar a resolver o problema.

Poderíamos facilmente substituir essa embalagem usando um seletor simples e adicionar um servlet de acordo para atender tais solicitações.

Mas e se o componente &quot;respi&quot; for mais do que apenas um proxy. E se o componente realmente contribuir para a renderização do componente?

Vamos introduzir uma pequena extensão do nosso componente de &quot;respi&quot;, que é um pouco de mudança no jogo. Mais uma vez, introduziremos, em primeiro lugar, algumas soluções ingênuas para enfrentar os novos desafios e mostrar onde ficam aquém.

#### O Componente Respi2

O componente respi2 é um componente que exibe uma imagem responsiva, assim como o componente respi. Mas tem um ligeiro suplemento.

![Estrutura CRX: componente respi2 adicionando uma propriedade de qualidade ao delivery](assets/chapter-1/respi2.png)

*Estrutura CRX: componente respi2 adicionando uma propriedade de qualidade ao delivery*

<br> 

As imagens são jpegs e os jpegs podem ser compactados. Ao compactar uma imagem jpeg, é possível trocar a qualidade pelo tamanho do arquivo. A compactação é definida como um parâmetro numérico de &quot;qualidade&quot; que varia de &quot;1&quot; a &quot;100&quot;. &quot;1&quot; significa &quot;qualidade pequena, mas ruim&quot;, &quot;100&quot; significa &quot;qualidade excelente, mas arquivos grandes&quot;. Então qual é o valor perfeito?

Como em todas as coisas de TI, a resposta é: &quot;Depende.&quot;

Aqui depende do motivo. Os motivos com bordas de alto contraste, como motivos, incluindo texto escrito, fotos de edifícios, ilustrações, rascunhos ou fotos de caixas de produtos (com contornos nítidos e texto escrito nele) normalmente caem nessa categoria. Motifs com transições mais suaves de cor e contraste, como paisagens ou retratos, podem ser compactados um pouco mais sem perda de qualidade visível. As fotografias da natureza geralmente se encaixam nessa categoria.

Além disso - dependendo de onde a imagem for usada - talvez você queira usar um parâmetro diferente. Uma pequena miniatura em um teaser pode suportar uma compactação melhor do que a mesma imagem usada em um banner herói de tela inteira. Isso significa que o parâmetro de qualidade não é inato à imagem, mas à imagem e ao contexto. E para o gosto do autor.

Resumindo: Não há configuração perfeita para todas as fotos. Não há um tamanho único para todos. É melhor o autor decidir. Ele definirá o parâmetro de &quot;qualidade&quot; como uma propriedade no componente até que esteja satisfeito com a qualidade e não vá mais longe para não sacrificar a largura de banda.

Agora temos um arquivo binário no DAM e um componente, que fornece uma propriedade de qualidade. Como deve ser o URL? Qual componente é responsável por spooling?

#### Abordagem ingênua 1: Enviar propriedades como parâmetros de Query

>[!WARNING]
>
>Isto é um anti-padrão. Não o utilize.

No último capítulo, a URL da imagem renderizada pelo componente era semelhante a:

`/content/dam/flower.respi.jpg`

Tudo o que falta é o valor da qualidade. O componente sabe qual propriedade é inserida pelo autor... Ela pode ser facilmente transmitida para o servlet de renderização de imagem como um parâmetro de query quando a marcação é renderizada, como `flower.respi2.jpg?quality=60`:

```plain
  <div class="respi2">
  <picture>
    <source src="/content/dam/flower.respi2.jpg?quality=60" …/>
    …
  </picture>
  </div>
  …
```

Esta é uma má ideia. Lembrar? As solicitações com parâmetros de query não podem ser armazenadas em cache.

#### Abordagem ingênua 2: Enviar informações adicionais como seletor

>[!WARNING]
>
>Isto pode tornar-se um anti-padrão. Use-o com cuidado.

![Transmissão das propriedades do componente como seletores](assets/chapter-1/passing-component-properties.png)

*Transmissão das propriedades do componente como seletores*

<br> 

Esta é uma pequena variação do último URL. Somente desta vez usamos um seletor para passar a propriedade para o servlet, de modo que o resultado possa ser obtido em cache:

`/content/dam/flower.respi.q-60.jpg`

Isto é muito melhor, mas lembram-se daquele sujeito mau do último capítulo que procura por tais padrões? Ele veria o quão longe ele pode chegar com o olhar sobre os valores:

```plain
  /content/dam/flower.respi.q-60.jpg
  /content/dam/flower.respi.q-61.jpg
  /content/dam/flower.respi.q-62.jpg
  /content/dam/flower.respi.q-63.jpg
  …
```

Isso novamente está ignorando o cache e criando carga no sistema de publicação. Então, pode ser uma má ideia. Você pode atenuar isso filtrando apenas um pequeno subconjunto de parâmetros. Você deseja permitir somente `q-20, q-40, q-60, q-80, q-100`.

#### Filtragem de solicitações inválidas ao usar seletores

Reduzir o número de seletores foi uma boa start. Como regra geral, você deve sempre limitar o número de parâmetros válidos a um mínimo absoluto. Se você fizer isso com inteligência, poderá até mesmo aproveitar uma Firewall de Aplicação web fora do AEM usando um conjunto estático de filtros sem o profundo conhecimento do sistema AEM subjacente para proteger seus sistemas:

`Allow: /content/dam/(-\_/a-z0-9)+/(-\_a-z0-9)+
\.respi\.q-(20|40|60|80|100)\.jpg`

Se você não tiver um Firewall de Aplicação web, será necessário filtrar no Dispatcher ou no próprio AEM. Se você fizer isso em AEM, certifique-se de que

1. O filtro é implementado de forma super eficiente, sem acessar o CRX demais e desperdiçar memória e tempo.

2. O filtro responde a uma mensagem de erro &quot;404 - Not found&quot; (404 - Não encontrado)

Vamos enfatizar o último ponto novamente. A conversa HTTP seria parecida com esta:

```plain
  GET /content/dam/flower.respi.q-41.jpg

  Response: 404 – Not found
  << empty response body >>
```

Também vimos implementações, que filtraram parâmetros inválidos, mas retornaram uma renderização de fallback válida quando um parâmetro inválido é usado. Vamos supor que só permitimos parâmetros de 20 a 100. Os valores entre eles são mapeados para os válidos. Então,

`q-41, q-42, q-43, …`

responderia sempre a mesma imagem que o q-40 teria:

```plain
  GET /content/dam/flower.respi.q-41.jpg

  Response: 200 – OK
  << flower.jpg with quality = 40 >>
```

Esta abordagem não está a ajudar de forma alguma. Essas solicitações são realmente válidas.  Eles consomem poder de processamento e ocupam espaço no diretório de cache no Dispatcher.

Melhor é retornar um `301 – Moved permanently`:

```plain
  GET /content/dam/flower.respi.q-41.jpg

  Response: 301 – Moved permanently
  Location: /content/dam/flower.respi.q-40.jpg
```

Aqui AEM dizendo ao navegador. &quot;Eu não tenho `q-41`. Mas ei - você pode me perguntar sobre `q-40` &quot;.

Isso adiciona um loop adicional de solicitação-resposta à conversa, o que é um pouco de sobrecarga, mas é mais barato do que fazer o processamento completo em `q-41`. E você pode aproveitar o arquivo que já está em cache em `q-40`. Você tem que entender, porém, que 302 respostas não são armazenadas em cache no Dispatcher, estamos falando de lógica que é executada no AEM. Uma e outra vez. Então é melhor você fazê-lo fino e rápido.

Pessoalmente, gostamos mais da resposta dos 404. Isso torna super óbvio o que está acontecendo. E ajuda a detectar erros em seu site quando você está analisando arquivos de log. 301s podem ser destinados, onde 404 devem ser sempre analisados e eliminados.

## Segurança - Excursão

### Solicitações de filtragem

#### Onde filtrar melhor

No final do último capítulo apontamos a necessidade de filtrar o tráfego de entrada de seletores conhecidos. Fica assim a questão: Onde devo realmente filtrar solicitações?

Depende. Quanto mais cedo melhor.

#### firewalls de aplicação web

Se você tiver um utilitário de Firewall de Aplicação web ou um &quot;WAF&quot; projetado para o Web Security, você deve absolutamente aproveitar esses recursos. Mas você pode descobrir que o WAF é operado por pessoas com conhecimento limitado de seu aplicativo de conteúdo e que elas filtram solicitações válidas ou deixam passar muitas solicitações prejudiciais. Talvez vocês descubram que as pessoas que operam a WAF são atribuídas a um departamento diferente com turnos diferentes e horários de lançamento, a comunicação pode não ser tão apertada quanto seus colegas diretos e nem sempre você consegue as mudanças no tempo, o que significa que, em última análise, seu desenvolvimento e velocidade de conteúdo sofrem.

Você pode acabar com algumas regras gerais ou até mesmo uma  lista de bloqueios, o que seu sentimento intestinal diz, poderia ser mais apertado.

#### Filtragem de Dispatcher e Publicação

A próxima etapa é adicionar regras de filtragem de URL no núcleo do Apache e/ou no Dispatcher.

Aqui, você tem acesso apenas a URLs. Você está limitado a filtros baseados em padrões. Se você precisar configurar uma filtragem mais baseada em conteúdo (como permitir arquivos somente com um carimbo de data e hora correto) ou desejar que parte da filtragem seja controlada em seu Autor - você acabará gravando algo como um filtro de servlet personalizado.

#### Monitoramento e depuração

Na prática, você terá alguma segurança em cada nível. Mas, por favor, certifique-se de que você tem meios para descobrir em que nível um pedido é filtrado. Verifique se você tem acesso direto ao sistema de publicação, ao Dispatcher e aos arquivos de registro no WAF para descobrir qual filtro na cadeia está bloqueando solicitações.

### Proliferação de seletores e seletores

A abordagem usando &quot;parâmetros-seletores&quot; no último capítulo é rápida e fácil e pode acelerar o tempo de desenvolvimento de novos componentes, mas tem limites.

Definir uma propriedade de &quot;qualidade&quot; é apenas um exemplo simples. Mas, digamos, o servlet também espera que os parâmetros de &quot;largura&quot; sejam mais versáteis.

Você pode reduzir o número de URLs válidos reduzindo o número de possíveis valores do seletor. Você também pode fazer o mesmo com a largura:

qualidade = q-20, q-40, q-60, q-80, q-100

largura = w-100, w-200, w-400, w-800, w-1000, w-1200

Mas todas as combinações agora são URLs válidos:

```
/content/dam/flower.respi.q-40.w-200.jpg
/content/dam/flower.respi.q-60.w-400.jpg
…
```

Agora já temos URLs válidos 5x6=30 para um recurso. Cada propriedade adicional aumenta a complexidade. E podem existir propriedades, que não podem ser reduzidas a uma quantidade razoável de valores.

Então, essa abordagem também tem limites.

#### Exposição inadvertida de uma API

O que está acontecendo aqui? Se olharmos atentamente, vemos que estamos gradualmente mudando de um site estaticamente renderizado para um site altamente dinâmico. E estamos inadvertidamente enviando uma API de renderização de imagem para o navegador do cliente que estava destinado a ser usado apenas pelos autores.

A configuração da qualidade e do tamanho de uma imagem deve ser feita pelo autor que editar a página. Ter os mesmos recursos expostos por um servlet pode ser visto como um recurso ou vetor para um ataque de negação de serviço. O que realmente é, depende do contexto. Qual é a importância do site para os negócios? Qual é a carga dos servidores? Quanto espaço de sobra? Qual é o seu orçamento para a execução? Você tem que equilibrar esses fatores. Você deve estar ciente dos prós e contras.

## Padrão de Spooler - Revisitado e Reabilitado

### Como o Spooler impede a exposição da API

Nós meio que desacreditamos o padrão Spooler no último capítulo. É hora de reabilitá-lo.

![](assets/chapter-1/spooler-pattern.png)

O padrão Spooler impede o problema ao expor uma API discutida no último capítulo. As propriedades são armazenadas e encapsuladas no componente. Tudo o que precisamos para acessar essas propriedades é o caminho para o componente. Não precisamos usar o URL como um veículo para transmitir os parâmetros entre marcação e renderização binária:

1. O cliente renderiza a marcação HTML quando o componente é solicitado dentro do loop de solicitação principal

2. O caminho do componente serve como uma referência retroativa da marcação para o componente

3. O navegador usa essa referência retroativa para solicitar o binário

4. Conforme a solicitação atinge o componente, temos todas as propriedades na mão para redimensionar, compactar e spool os dados binários

5. A imagem é transmitida pelo componente para o navegador cliente

O Padrão Spooler não é tão ruim afinal, é por isso que é tão popular. Se não for tão complicado quando se trata da invalidação do cache...

### O Spooler Invertido - O Melhor dos Dois Mundos?

Isto leva-nos à questão. Por que não conseguimos o melhor dos dois mundos? O bom encapsulamento do Padrão Spooler e as propriedades de cache agradáveis de um URL Baseado em Recursos?

Temos que admitir, que não vimos isso em um projeto real. Mas ousemos, de qualquer forma, fazer aqui uma pequena experiência de reflexão - como ponto de partida para a sua própria solução.

Chamaremos esse padrão de _Spooler Invertido_. O Spooler Invertido deve ser baseado no recurso de imagens, para ter todas as propriedades de invalidação de cache.

Mas não deve expor quaisquer parâmetros. Todas as propriedades devem ser encapsuladas no componente. Mas podemos expor o caminho dos componentes - como uma referência opaca às propriedades.

Isso resulta em um URL no formulário:

`/content/dam/flower.respi3.content-mysite-home-jcrcontent-par-respi.jpg`

`/content/dam/flower` é o caminho para o recurso da imagem

`.respi3` é um seletor para selecionar o servlet correto para entregar a imagem

`.content-mysite-home-jcrcontent-par-respi` é um seletor adicional. Ele codifica o caminho para o componente que armazena a propriedade necessária para a transformação da imagem. Os seletores são limitados a um intervalo menor de caracteres do que caminhos. O esquema de codificação aqui é apenas exemplar. Substitui &quot;/&quot; por &quot;-&quot;. Não é levando em conta que o caminho em si pode conter também &quot;-&quot;. Um esquema de codificação mais sofisticado seria aconselhável num exemplo real. Base64 deve estar bem. Mas torna a depuração um pouco mais difícil.

`.jpg` é o sufixo dos arquivos

### Conclusão

Nossa... a discussão do spooler ficou mais longa e mais complicada do que o esperado. Nós lhe devemos uma desculpa. Mas sentimos que é necessário apresentar-vos uma série de aspectos - bons e maus - para que possam desenvolver alguma intuição sobre o que funciona bem no Dispatcher-land e o que não funciona.

## Nível de arquivo de status e de arquivo de status

### Noções básicas

#### Introdução

Já mencionamos brevemente o _statfile_ antes. Está relacionado à invalidação automática:

Todos os arquivos de cache no sistema de arquivos do Dispatcher que estão configurados para serem invalidados automaticamente são considerados inválidos se a data da última modificação for anterior à data da última modificação de `statfile's`.

>[!NOTE]
>
>A data da última modificação de que estamos falando é a data em que o arquivo foi solicitado do navegador do cliente e, por fim, criado no sistema de arquivos. Não é a data `jcr:lastModified` do recurso.

A data da última modificação do arquivo de status (`.stat`) é a data em que a solicitação de invalidação do AEM foi recebida no Dispatcher.

Se você tiver mais de um Dispatcher, isso pode causar efeitos estranhos. Seu navegador pode ter uma versão mais recente de um Dispatchers (se você tiver mais de um Dispatcher). Ou um Dispatcher pode achar que a versão do navegador emitida pelo outro Dispatcher está desatualizada e envia uma nova cópia desnecessariamente. Esses efeitos não têm um impacto significativo no desempenho ou nos requisitos funcionais. E eles vão ficar nivelados ao longo do tempo, quando o navegador tiver a versão mais recente. No entanto, pode ser um pouco confuso quando você está otimizando e depurando o comportamento de cache do navegador. Então, seja avisado.

#### Configurando domínios de invalidação com /statfileslevel

Quando introduzimos a invalidação automática e o arquivo de status que informamos, os arquivos *all* são considerados inválidos quando há qualquer alteração e todos os arquivos são interdependentes mesmo assim.

Isso não é bem preciso. Normalmente, todos os arquivos que compartilham uma raiz de navegação principal comum são interdependentes. Mas uma instância AEM pode hospedar vários sites - *sites independentes*. Não compartilhar uma navegação comum - na verdade, não compartilhar nada.

Não seria um desperdício invalidar o Site B porque há uma mudança no Site A? Sim, é. E não precisa ser assim.

O Dispatcher fornece um meio simples de separar os sites entre si: O `statfiles-level`.

É um número que define a partir de qual nível no sistema de arquivos, duas subárvores são consideradas &quot;independentes&quot;.

Vejamos o caso padrão em que o nível de status é 0.

![/statfileslevel &quot;0&quot;: O_  _.stat_ _é criado no ponto. O domínio de invalidação abrange toda a instalação, incluindo todos os sites](assets/chapter-1/statfile-level-0.png)

`/statfileslevel "0":` O  `.stat` arquivo é criado no ponto. O domínio de invalidação abrange toda a instalação, incluindo todos os sites.

Qualquer que seja o arquivo que for invalidado, o arquivo `.stat` na parte superior do ponto dos despachantes será sempre atualizado. Assim, quando você invalidar `/content/site-b/home`, todos os arquivos em `/content/site-a` também serão invalidados, já que agora são mais antigos que o arquivo `.stat` no docroot. Claramente não é o que você precisa, quando você invalidar `site-b`.

Neste exemplo, você prefere definir `statfileslevel` como `1`.

Agora, se você publicar - e, portanto, invalidar `/content/site-b/home` ou qualquer outro recurso abaixo de `/content/site-b`, o arquivo `.stat` será criado em `/content/site-b/`.

O conteúdo abaixo de `/content/site-a/` não é afetado. Este conteúdo seria comparado a um arquivo `.stat` em `/content/site-a/`. Criamos dois domínios de invalidação separados.

![Um status &quot;1&quot; cria diferentes domínios de invalidação](assets/chapter-1/statfiles-level-1.png)

*Um status &quot;1&quot; cria diferentes domínios de invalidação*

<br> 

As grandes instalações normalmente são estruturadas de forma um pouco mais complexa e profunda. Um esquema comum é estruturar sites por marca, país e idioma. Nesse caso, é possível definir o nível de status ainda mais alto. _1_ criaria domínios de invalidação por marca,  _2_ por país e  _3_ por idioma.

### Necessidade de uma Estrutura do Site Homogênea

O nível de status é aplicado igualmente a todos os sites na sua configuração. Por conseguinte, é necessário que todos os sítios tenham a mesma estrutura e start ao mesmo nível.

Considere que você tem algumas marcas em seu portfólio que são vendidas apenas em alguns pequenos mercados enquanto outras são vendidas no mundo inteiro. Os pequenos mercados têm apenas uma língua local enquanto no mercado global há países onde se fala mais de uma língua:

```plain
  /content/tiny-local-brand/finland/home
  /content/tiny-local-brand/finland/products
  /content/tiny-local-brand/finland/about
                              ^
                          /statfileslevel "2"
  …

  /content/tiny-local-brand/norway
  …

  /content/shiny-global-brand/canada/en
  /content/shiny-global-brand/canada/fr
  /content/shiny-global-brand/switzerland/fr
  /content/shiny-global-brand/switzerland/de
  /content/shiny-global-brand/switzerland/it
                                          ^
                                /statfileslevel "3"
  ..
```

A primeira exigiria um `statfileslevel` de _2_, enquanto a segunda requer _3_.

Não é uma situação ideal. Se você o definir como _3_, a invalidação automática não funcionará nos sites menores entre as subramificações `/home`, `/products` e `/about`.

Definir como _2_ significa que nos sites maiores você está declarando `/canada/en` e `/canada/fr` dependentes, o que eles podem não ser. Assim, cada invalidação em `/en` também invalidaria `/fr`. Isso resultará em uma taxa de ocorrência de cache ligeiramente menor, mas ainda é melhor do que fornecer conteúdo obsoleto em cache.

A melhor solução, claro, é tornar as raízes de todos os sites igualmente profundas:

```
/content/tiny-local-brand/finland/fi/home
/content/tiny-local-brand/finland/fi/products
/content/tiny-local-brand/finland/fi/about
…
/content/tiny-local-brand/norway/no/home
                                 ^
                        /statfileslevel "3"
```

### Vinculação entre sites

Agora, qual é o nível certo? Isso depende do número de dependências entre os sites. As inclusões que você resolver para renderizar uma página são consideradas &quot;dependências difíceis&quot;. Demonstramos tal _inclusão_ quando apresentamos o componente _Teaser_ no início deste guia.

_Os_ hiperlinks são uma forma mais suave de dependências. É muito provável que você tenha hiperlinks dentro de um site... e não é improvável que você tenha links entre seus sites. Hiperlinks simples geralmente não criam dependências entre sites. Basta pensar em um link externo que você definiu de seu site para o Facebook... Você não teria que renderizar sua página se algo mudasse no Facebook e vice-versa, certo?

Uma dependência ocorre quando você lê o conteúdo do recurso vinculado (por exemplo, o título de navegação). Tais dependências podem ser evitadas se você depender apenas de títulos de navegação inseridos localmente e não os desenhar na página do público alvo (como faria com links externos).

#### Uma dependência inesperada

No entanto, pode haver uma parte da sua configuração, na qual - supostamente independentes - os sites se reúnem. Vejamos um cenário real que encontramos em um de nossos projetos.

O cliente tinha uma estrutura de site como a do último capítulo:

```
/content/brand/country/language
```

Por exemplo,

```
/content/shiny-brand/switzerland/fr
/content/shiny-brand/switzerland/de

/content/shiny-brand/france/fr

/content/shiny-brand/germany/de
```

Cada país tinha o seu próprio domínio.

```
www.shiny-brand.ch

www.shiny-brand.fr

www.shiny-brand.de
```

Não havia links navegáveis entre os sites de idioma e nenhuma inclusão aparente, então definimos o nível de status como 3.

Todos os sites basicamente serviam o mesmo conteúdo. A única grande diferença era a língua.

Mecanismos de pesquisa como o Google consideram ter o mesmo conteúdo em URLs diferentes &quot;enganadores&quot;. Um usuário pode querer tentar ser classificado com mais ou mais frequência criando fazendas que servem conteúdo idêntico. Os mecanismos de busca reconhecem essas tentativas e classificam as páginas abaixo que simplesmente reciclam o conteúdo.

Você pode evitar ficar menos classificado tornando transparente, ter mais de uma página com o mesmo conteúdo e não tentar &quot;jogar&quot; o sistema (consulte [&quot;Informar o Google sobre as versões localizadas da sua página&quot;](https://support.google.com/webmasters/answer/189077?hl=en)) ao configurar as tags `<link rel="alternate">` para cada página relacionada na seção de cabeçalho de cada página:

```
# URL: www.shiny-brand.fr/fr/home/produits.html

<head>

  <link rel="alternate" 
        hreflang="fr-ch" 
        href="http://www.shiny-brand.ch/fr/home/produits.html">
  <link rel="alternate" 
        hreflang="de-ch" 
        href="http://www.shiny-brand.ch/de/home/produkte.html">
  <link rel="alternate" 
        hreflang="de-de" 
        href="http://www.shiny-brand.de/de/home/produkte.html">

</head>

----

# URL www.shiny-brand.de/de/home/produkte.html

<head>

  <link rel="alternate" 
        hreflang="fr-fr" 
        href="http://www.shiny-brand.fr/fr/home/produits.html">
  <link rel="alternate" 
        hreflang="fr-ch" 
        href="http://www.shiny-brand.ch/fr/home/produits.html">
  <link rel="alternate" 
        hreflang="de-ch"
         href="http://www.shiny-brand.ch/de/home/produits.html">

</head>
```

![Intervinculação de tudo](assets/chapter-1/inter-linking-all.png)

*Intervinculação de tudo*

<br> 

Alguns especialistas do SEO até argumentam que isso poderia transferir a reputação ou &quot;sumo de link&quot; de um site classificado em uma língua para o mesmo site em uma língua diferente.

Este regime criou não só uma série de ligações, mas também alguns problemas. O número de links necessários para _p_ nos idiomas _n_ é _p x (n<sup>2</sup>-n)_: Cada página é vinculada entre si (_n x n_), exceto a si mesma (_-n_). Este esquema é aplicado a cada página. Se tivermos um site pequeno em 4 idiomas com 20 páginas, cada um terá _240_ links.

Primeiro, você não quer que um editor tenha que manter manualmente esses links - eles precisam ser gerados automaticamente pelo sistema.

Segundo, devem ser precisos. Sempre que o sistema detectar um novo &quot;relativo&quot;, você deseja vinculá-lo de todas as outras páginas com o mesmo conteúdo (mas em idioma diferente).

Em nosso projeto, novas páginas relativas apareciam com frequência. Mas eles não se materializaram como links &quot;alternativos&quot;. Por exemplo, quando a página `de-de/produkte` foi publicada no site alemão, ela não estava imediatamente visível nos outros sites.

A razão era que, na nossa configuração, os sites deveriam ser independentes. Assim, uma mudança no website alemão não provocou uma invalidação no website francês.

Você já conhece uma solução para resolver esse problema. Basta diminuir o nível de status para 2 para ampliar o domínio de invalidação. É claro que isso também diminui a taxa de acertos do cache - especialmente quando publicações - e, portanto, as invalidações ocorrem mais frequentemente.

No nosso caso, era ainda mais complicado:

Mesmo que tivéssemos o mesmo conteúdo, os nomes de marca não eram diferentes em cada país.

`shiny-brand` foi chamado  `marque-brillant` na França e  `blitzmarke` na Alemanha:

```
/content/marque-brillant/france/fr
/content/shiny-brand/switzerland/fr
/content/shiny-brand/switzerland/de
/content/blitzmarke/germany/de
…
```

Isso significaria definir o nível `statfiles` como 1 - o que resultaria em um domínio de invalidação muito grande.

A reestruturação do local teria corrigido isso. Unindo todas as marcas em uma única raiz comum. Mas nós não tínhamos a capacidade naquela época, e isso nos daria apenas um nível 2.

Decidimos manter o nível 3 e pagar o preço de nem sempre termos ligações &quot;alternativas&quot; atualizadas. Para mitigar, tínhamos um trabalho cron &quot;mais recente&quot; em execução no Dispatcher, que limparia arquivos com mais de uma semana. Então, finalmente, todas as páginas foram renderizadas novamente em algum momento do tempo. Mas essa é uma compensação que precisa ser decidida individualmente em cada projeto.

## Conclusão

Abordamos alguns princípios básicos de como o Dispatcher está funcionando em geral e demos alguns exemplos onde você pode ter que colocar um pouco mais de esforço de implementação para acertar e onde você pode querer fazer escolhas.

Não entramos em detalhes sobre como isso é configurado no Dispatcher. Queríamos que você entendesse os conceitos básicos e os problemas primeiro, sem perdê-lo para o console muito cedo. E - o trabalho de configuração real está bem documentado - se você entender os conceitos básicos, você deve saber para que servem os vários switches.

## Dicas e truques do Dispatcher

Concluiremos a primeira parte deste livro com uma coleção aleatória de dicas e truques que podem ser úteis numa ou noutra situação. Como fizemos antes, não estamos apresentando a solução, mas a ideia para que você tenha a chance de entender a ideia e o conceito e vincular a artigos que descrevem a configuração real com mais detalhes.

### Tempo de invalidação correto

Se você instalar um autor de AEM e publicar fora da caixa, a topologia será um pouco estranha. O autor envia o conteúdo para os sistemas de publicação e a solicitação de invalidação para os Dispatchers ao mesmo tempo. Como ambos, os sistemas de publicação e o Dispatcher, são dissociados do Autor por filas, o tempo pode ser um pouco infeliz. O Dispatcher pode receber a solicitação de invalidação do Autor antes que o conteúdo seja atualizado no sistema de publicação.

Se um cliente solicitar esse conteúdo enquanto isso, o Dispatcher solicitará e armazenará conteúdo obsoleto.

Uma configuração mais confiável está enviando a solicitação de invalidação dos sistemas de publicação _depois de_ que eles receberam o conteúdo. O artigo &quot;[Invalidando o Cache do Dispatcher de uma Instância de Publicação](https://helpx.adobe.com/experience-manager/dispatcher/using/page-invalidate.html#InvalidatingDispatcherCachefromaPublishingInstance)&quot; descreve os detalhes.

**Referências**

[helpx.adobe.com - Invalidando o Cache do Dispatcher de uma Instância de Publicação](https://helpx.adobe.com/experience-manager/dispatcher/using/page-invalidate.html#InvalidatingDispatcherCachefromaPublishingInstance)

### Cache de cabeçalho e cabeçalho HTTP

Antigamente, o Dispatcher estava apenas armazenando arquivos simples no sistema de arquivos. Se você precisasse que os cabeçalhos HTTP fossem entregues ao cliente, você faria isso configurando o Apache com base nas poucas informações que tinha do arquivo ou local. Isso foi especialmente irritante quando você implementou um aplicativo da Web em AEM que dependia muito de cabeçalhos HTTP. Tudo funcionava bem na instância somente AEM, mas não quando você usava um Dispatcher.

Geralmente, você começou a reaplicar os cabeçalhos ausentes aos recursos no servidor Apache com `mod_headers` usando as informações que você poderia derivar pelo caminho e sufixo dos recursos. Mas isso nem sempre foi suficiente.

Foi especialmente chocante que mesmo com o Dispatcher a primeira resposta _não armazenada em cache_ ao navegador tenha vindo do sistema de publicação com uma gama completa de cabeçalhos, enquanto as respostas subsequentes foram geradas pelo Dispatcher com um conjunto limitado de cabeçalhos.

A partir do Dispatcher 4.1.11, o Dispatcher pode armazenar cabeçalhos gerados pelos sistemas de publicação.

Isso evita a duplicação da lógica do cabeçalho no Dispatcher e libera todo o poder expressivo do HTTP e AEM.

**Referências**

* [helpx.adobe.com - Cache de cabeçalhos de resposta](https://helpx.adobe.com/experience-manager/kb/dispatcher-cache-response-headers.html)

### Exceções de Cache Individual

Você pode desejar armazenar em cache todas as páginas e imagens em geral, mas exceções em algumas circunstâncias. Por exemplo, você deseja armazenar em cache imagens PNG, mas não imagens PNG que exibem um captcha (o que supostamente mudará em cada solicitação). O Dispatcher talvez não reconheça um captcha como um captcha... mas AEM certamente reconhece. Ele pode solicitar que o Dispatcher não armazene essa solicitação em cache enviando um cabeçalho de acordo com a resposta:

```plain
  response.setHeader("Dispatcher", "no-cache");

  response.setHeader("Cache-Control: no-cache");

  response.setHeader("Cache-Control: private");

  response.setHeader("Pragma: no-cache");
```

Cache-Control e Pragma são cabeçalhos HTTP oficiais, que são propagados e interpretados por camadas de cache superior, como um CDN. O cabeçalho `Dispatcher` é apenas uma dica para o Dispatcher não armazenar em cache. Ele pode ser usado para informar o Dispatcher a não armazenar em cache, e ainda permitir que as camadas superiores de cache façam isso. Na verdade, é difícil encontrar um caso em que isso possa ser útil. Mas temos certeza de que há alguns, em algum lugar.

**Referências**

* [Dispatcher - Sem Cache](https://helpx.adobe.com/experience-manager/kb/DispatcherNoCache.html)

### Cache do navegador

A http-response mais rápida é a resposta fornecida pelo próprio navegador. Onde a solicitação e a resposta não precisam viajar pela rede para um servidor Web em alta carga.

Você pode ajudar o navegador a decidir quando solicitar uma nova versão do arquivo ao servidor, definindo uma data de expiração em um recurso.

Normalmente, você faz isso estaticamente usando o `mod_expires` do Apache ou armazenando o Cache-Control e o Cabeçalho Expira que vêm do AEM se precisar de um controle mais individual.

Um documento em cache no navegador pode ter três níveis de atualização.

1. _Atualização_  garantida - O navegador pode usar o documento em cache.

2. _Potencialmente obsoleto_  - o navegador deve perguntar ao servidor primeiro se o documento em cache ainda está atualizado,

3. _Stale_  - o navegador deve solicitar ao servidor uma nova versão.

A primeira é garantida pela data de expiração definida pelo servidor. Se um recurso não tiver expirado, não há necessidade de perguntar novamente ao servidor.

Se o documento atingir sua data de expiração, ele ainda poderá ser atualizado. A data de expiração é definida quando o documento é entregue. Mas muitas vezes você não sabe com antecedência quando novos conteúdos estão disponíveis - então essa é apenas uma estimativa conservadora.

Para determinar se o documento no cache do navegador ainda é o mesmo que seria entregue em uma nova solicitação, o navegador pode usar a data `Last-Modified` do documento. O navegador pergunta ao servidor:

&quot;_Tenho uma versão de 10 de junho... preciso de uma atualização?_&quot; E o servidor pode responder com

&quot;_304 - Sua versão ainda está atualizada_&quot; sem retransmitir o recurso, ou o servidor pode responder com

&quot;_200 - aqui está uma versão mais recente_&quot; no cabeçalho HTTP e o conteúdo mais recente no corpo HTTP.

Para que essa segunda parte funcione, certifique-se de transmitir a data `Last-Modified` ao navegador para que ele tenha um ponto de referência para solicitar atualizações.

Explicamos anteriormente que, quando a data `Last-Modified` é gerada pelo Dispatcher, pode variar entre solicitações diferentes porque o arquivo em cache - e sua data - é gerado quando o arquivo é solicitado pelo navegador. Uma alternativa seria usar &quot;e-tags&quot; - são números que identificam o conteúdo real (por exemplo, gerando um código de hash) em vez de uma data.

&quot;[Etag Support](https://adobe-consulting-services.github.io/acs-aem-commons/features/etag/index.html)&quot; do _Pacote de Comuns ACS_ utiliza esta abordagem. No entanto, isso tem um preço: Como a E-Tag deve ser enviada como um cabeçalho, mas o cálculo do código de hash requer a leitura completa da resposta, a resposta deve ser totalmente armazenada em buffer na memória principal antes de ser entregue. Isso pode ter um impacto negativo na latência quando seu site tem maior probabilidade de ter recursos não armazenados em cache e, claro, você precisa ficar de olho na memória consumida pelo seu sistema AEM.

Se você estiver usando impressões digitais de URL, é possível definir datas de expiração muito longas. Você pode armazenar em cache recursos de impressão digital para sempre no navegador. Uma nova versão é marcada com um novo URL e as versões mais antigas nunca precisam ser atualizadas.

Usamos impressões digitais de URL quando introduzimos o padrão de spooler. Os arquivos estáticos provenientes de `/etc/design` (CSS, JS) raramente são alterados, o que também os torna bons candidatos a usar como impressões digitais.

Para arquivos comuns, normalmente configuramos um esquema fixo, como verificar novamente o HTML a cada 30 minutos, imagens a cada 4 horas e assim por diante.

O cache do navegador é extremamente útil no sistema do Autor. Você deseja armazenar em cache o máximo possível no navegador para aprimorar a experiência de edição. Infelizmente, os ativos mais caros, as páginas html não podem ser armazenadas em cache... eles devem mudar frequentemente no autor.

As bibliotecas granitas, que formam AEM interface do usuário, podem ser armazenadas em cache por um bom tempo. Você também pode armazenar em cache os arquivos estáticos do site (fontes, CSS e JavaScript) no navegador. Mesmo as imagens em `/content/dam` geralmente podem ser armazenadas em cache por cerca de 15 minutos, pois não são alteradas com a mesma frequência que copiar texto nas páginas. As imagens não são editadas interativamente no AEM. Eles são editados e aprovados primeiro, antes de serem carregados para AEM. Portanto, você pode supor que eles não estão mudando tão frequentemente quanto o texto.

Armazenando arquivos da interface do usuário em cache, os arquivos e as imagens da biblioteca de sites podem acelerar o recarregamento de páginas significativamente quando você está no modo de edição.



**Referências**

*[developer.mozilla.org - Cache](https://developer.mozilla.org/en-US/docs/Web/HTTP/Caching)

* [apache.org - Mod Expira](https://httpd.apache.org/docs/current/mod/mod_expires.html)

* [Comuns ACS - Suporte Etag](https://adobe-consulting-services.github.io/acs-aem-commons/features/etag/index.html)

### Truncando URLs

Seus recursos são armazenados em

`/content/brand/country/language/…`

Mas, claro, este não é o URL que você deseja apresentar ao cliente. Para questões de estética, legibilidade e SEO, você pode truncar a parte que já está representada no nome do domínio.

Se você tiver um domínio

`www.shiny-brand.fi`

geralmente não há necessidade de colocar a marca e o país no caminho. Em vez de,

`www.shiny-brand.fi/content/shiny-brand/finland/fi/home.html`

você gostaria de ter,

`www.shiny-brand.fi/home.html`

Você deve implementar esse mapeamento no AEM - porque AEM precisa saber como renderizar links de acordo com esse formato truncado.

Mas não confie apenas em AEM. Se o fizer, você terá caminhos como `/home.html` no diretório raiz do cache. Agora, esse é o &quot;lar&quot; para o finlandês ou alemão ou o site canadense? E se houver um arquivo `/home.html` no Dispatcher, como o Dispatcher sabe que isso deve ser invalidado quando uma solicitação de invalidação para `/content/brand/fi/fi/home` entrar?

Vimos um projeto que tinha docroots separados para cada domínio. Foi um pesadelo para depurar e manter - e na verdade nunca o vimos funcionar perfeitamente.

Poderíamos resolver os problemas reestruturando o cache. Tínhamos um único ponto para todos os domínios, e as solicitações de invalidação podiam ser processadas 1:1, pois todos os arquivos no servidor começavam com `/content`.

A parte truncadora também foi muito fácil.  AEM gerou links truncados devido a uma configuração de acordo em `/etc/map`.

Agora, quando uma solicitação `/home.html` está acessando o Dispatcher, a primeira coisa que acontece é aplicar uma regra de regravação que expande internamente o caminho.

Essa regra foi configurada estaticamente em cada configuração de host. Resumindo, as regras pareciam com isto.

```plain
  # vhost www.shiny-brand.fi

  RewriteRule "^(.\*\.html)" "/content/shiny-brand/finland/fi/$1"
```

No sistema de arquivos, agora temos caminhos simples baseados em `/content`, que também seriam encontrados no Autor e no Publish - o que ajudou a depurar muito. Sem falar na invalidação correta - isso já não era um problema.

Observe que fizemos isso somente para URLs &quot;visíveis&quot;, URLs exibidos no slot do URL do navegador. URLs de imagens, por exemplo, ainda eram URLs puros de &quot;/content&quot;. Acreditamos que a beleza do URL &quot;principal&quot; é suficiente em termos de Otimização do mecanismo de pesquisa.

Ter um ponto comum também tinha outra boa característica. Quando algo deu errado no Dispatcher, pudemos limpar todo o cache executando.

`rm -rf /cache/dispatcher/*`

(algo que você pode não querer fazer em picos de carga alta).

**Referências**

* [apache.org - Substituição de Mod](https://httpd.apache.org/docs/2.4/mod/mod_rewrite.html)

* [helpx.adobe.com - Mapeamento de recursos](https://helpx.adobe.com/experience-manager/6-4/sites/deploying/using/resource-mapping.html)

### Reparação de erros

Em AEM classes, você aprenderá a programa de um manipulador de erros no Sling. Isso não é tão diferente de escrever um modelo comum. Você simplesmente escreve um modelo em JSP ou HTL, certo?

Sim - mas essa é a parte AEM, apenas. Lembre-se - o Dispatcher não armazena em cache as respostas `404 – not found` ou `500 – internal server error`.

Se estiver renderizando essas páginas dinamicamente em cada solicitação (com falha), você terá uma carga alta desnecessária nos sistemas de publicação.

O que achamos útil é não renderizar a página de erro completa quando ocorre um erro, mas apenas uma versão super simplificada e pequena - até mesmo estática dessa página, sem qualquer adorno ou lógica.

Isso, claro, não foi o que o cliente viu. No Dispatcher, registramos `ErrorDocuments` desta forma:

```
ErrorDocument 404 "/content/shiny-brand/fi/fi/edocs/error-404.html"
ErrorDocument 500 "/content/shiny-brand/fi/fi/edocs/error-500.html"
```

Agora o sistema AEM poderia simplesmente notificar o Dispatcher que algo estava errado, e o Dispatcher poderia fornecer uma versão brilhante e bela do documento de erro.

Há duas coisas que devem ser observadas aqui.

Primeiro, `error-404.html` sempre é a mesma página. Portanto, não há nenhuma mensagem individual como &quot;Sua busca por &quot;_produkten_&quot; não produziu um resultado&quot;. Poderíamos viver facilmente com isso.

Segundo... bem, se vemos um erro interno do servidor - ou pior ainda, encontramos uma interrupção do sistema AEM, não há como pedir AEM para renderizar uma página de erro, certo? A solicitação subsequente necessária, conforme definido na diretiva `ErrorDocument`, também falharia. Resolvemos esse problema executando um trabalho cron que extraía periodicamente as páginas de erro de seus locais definidos via `wget` e as armazenava em locais de arquivos estáticos definidos na diretiva `ErrorDocuments`.

**Referências**

* [apache.org - Documentos de erro personalizados](https://httpd.apache.org/docs/2.4/custom-error.html)

### Armazenamento de conteúdo protegido em cache

O Dispatcher não verifica as permissões quando fornece um recurso por padrão. É implementada assim de propósito - para acelerar seu site público. Se quiser proteger alguns recursos através de um login, basicamente você tem três opções:

1. A Protect do recurso antes que a solicitação atinja o cache - isto é, por um gateway SSO (Single Sign On) na frente do Dispatcher ou como um módulo no servidor Apache

2. Exclua recursos confidenciais de serem armazenados em cache e, assim, sempre os disponibilize ao vivo do sistema de publicação.

3. Usar o cache sensível a permissões no Dispatcher

E claro, você pode aplicar sua própria combinação de todas as três abordagens.

**Opção 1**. Um Gateway &quot;SSO&quot; pode ser aplicado pela sua organização mesmo assim. Se seu esquema de acesso estiver muito sobrecarregado, talvez você não precise de informações de AEM para decidir se concede ou nega acesso a um recurso.

>[!NOTE]
>
>Este padrão requer um _Gateway_ que _intercepta_ cada solicitação e executa a _autorização_ real - concedendo ou negando solicitações ao Dispatcher. Se seu sistema SSO for um _autenticador_, isso só estabelece a identidade de um usuário que você precisa implementar a Opção 3. Se você ler termos como &quot;SAML&quot; ou &quot;OAauth&quot; no manual do seu sistema SSO - esse é um indicador forte de que você precisa implementar a Opção 3.


**Opção 2**. &quot;Não armazenar em cache&quot; geralmente é uma má ideia. Se você for por esse caminho, verifique se a quantidade de tráfego e o número de recursos confidenciais excluídos são pequenos. Ou certifique-se de ter algum cache na memória instalado no sistema de publicação, de que os sistemas de publicação podem lidar com a carga resultante - mais sobre isso na Parte III desta série.

**Opção 3**. &quot;Cache sensível a permissões&quot; é uma abordagem interessante. O Dispatcher está armazenando um recurso em cache - mas antes de fornecê-lo, ele pergunta ao sistema AEM se ele pode fazê-lo. Isso cria uma solicitação extra do Dispatcher para o Publish - mas normalmente sobressai o sistema de publicação da renderização de uma página se já estiver em cache. No entanto, essa abordagem requer alguma implementação personalizada. Encontre detalhes aqui no artigo [Cache sensível a permissões](https://helpx.adobe.com/experience-manager/dispatcher/using/permissions-cache.html).

**Referências**

* [helpx.adobe.com - Cache sensível a permissões](https://helpx.adobe.com/experience-manager/dispatcher/using/permissions-cache.html)

### Configuração do período de carência

Se você estiver invalidando com frequência em breve - por exemplo, por uma ativação em árvore ou por simples necessidade de manter seu conteúdo atualizado, pode acontecer que você esteja constantemente descarregando o cache e que seus visitantes estejam quase sempre acessando um cache vazio.

O diagrama abaixo ilustra uma possível temporização ao acessar uma única página.  O problema, claro, fica pior quando o número de páginas diferentes solicitadas aumenta.

![Ativações frequentes que levam a cache inválido durante a maior parte do tempo](assets/chapter-1/frequent-activations.png)

*Ativações frequentes que levam a cache inválido durante a maior parte do tempo*

<br> 

Para atenuar o problema desta &quot;tempestade de invalidação de cache&quot;, como é chamada às vezes, você pode ser menos rigoroso com relação à interpretação `statfile`.

Você pode definir o Dispatcher para usar um `grace period` para a invalidação automática. Isso adicionaria internamente algum tempo extra à data de modificação `statfiles`.

Digamos que seu `statfile` tenha um horário de modificação de hoje às 12:00 e seu `gracePeriod` esteja definido como 2 minutos. Em seguida, todos os arquivos autoinvalidados serão considerados válidos às 12:01 e às 12:02. Eles serão renderizados novamente depois das 12h02.

A configuração de referência propõe um `gracePeriod` de dois minutos por um bom motivo. Vocês podem pensar &quot;Dois minutos? Isso é quase nada. Posso esperar 10 minutos para que o conteúdo apareça...&quot;.  Portanto, você pode estar tentado a definir um período mais longo - digamos 10 minutos, supondo que seu conteúdo apareça pelo menos após esses 10 minutos.

>[!WARNING]
>
>Não é assim que `gracePeriod` está funcionando. O período de carência é _e não_ o tempo após o qual um documento tem garantia de ser invalidado, mas um período de tempo sem invalidação acontece. Cada invalidação subsequente que se enquadre nesse quadro _prolonga_ o período - isso pode ser indefinidamente longo.

Vamos ilustrar como `gracePeriod` está realmente trabalhando com um exemplo:

Por exemplo, você está operando um site de mídia e sua equipe de edição fornece atualizações regulares de conteúdo a cada 5 minutos. Considere definir o período de carência como 5 minutos.

Nós vamos start com um exemplo rápido às 12:00.

12:00 - Statfile é definido como 12:00. Todos os arquivos em cache são considerados válidos até 12:05.

12:01 - Ocorre uma invalidação. Isso prolonga o tempo da grade para 12:06

12:05 - Outro editor publica seu artigo - prolongando o tempo de carência por outro período de carência para 12:10.

E assim por diante... o conteúdo nunca é invalidado. Cada invalidação *em* o GracePeriod prolonga efetivamente o tempo de carência. O `gracePeriod` foi projetado para enfrentar a tempestade de invalidação... mas você deve sair para a chuva eventualmente... então, mantenha o `gracePeriod` consideravelmente curto para evitar se esconder no abrigo para sempre.

#### Um período de carência determinista

Nós gostaríamos de apresentar outra ideia de como você poderia enfrentar uma tempestade de invalidação. É apenas uma ideia. Nós não tentamos na produção, mas achamos o conceito interessante o suficiente para compartilhar a ideia com vocês.

O `gracePeriod` pode tornar-se imprevisivelmente longo se o intervalo de replicação regular for menor que o `gracePeriod`.

A ideia alternativa é a seguinte: Invalidar somente em intervalos de tempo fixos. O intervalo de tempo significa sempre servir conteúdo obsoleto. A invalidação eventualmente ocorrerá, mas várias invalidações são coletadas em uma invalidação &quot;em massa&quot;, de modo que o Dispatcher tem a chance de fornecer algum conteúdo em cache enquanto isso e dar ao sistema de publicação algum ar para respirar.

A implementação seria desta forma:

Use um &quot;Script de invalidação personalizado&quot; (consulte a referência), que seria executado após a invalidação. Este script leria a última data de modificação `statfile's` e arredonda-a até a próxima parada de intervalo. O comando Unix shell `touch --time`, especifique uma hora.

Por exemplo, se você definir o período de carência como 30 segundos, o Dispatcher contornaria a última data de modificação do arquivo de status para os próximos 30 segundos. Solicitações de invalidação que ocorrem entre apenas definem os mesmos próximos 30 segundos completos.

![Adiar a invalidação para os próximos 30 segundos completos aumenta a taxa de ocorrência.](assets/chapter-1/postponing-the-invalidation.png)

*Adiar a invalidação para os próximos 30 segundos completos aumenta a taxa de ocorrência.*

<br> 

As ocorrências de cache que ocorrem entre a solicitação de invalidação e o próximo slot circular de 30 segundos são então consideradas obsoletas; Houve uma atualização em Publicar, mas o Dispatcher ainda serve conteúdo antigo.

Esta abordagem poderia ajudar a definir períodos de carência mais longos, sem recear que os pedidos subsequentes prolongem o período de forma indeterminada. Embora como já dissemos antes - é apenas uma ideia e não tivemos a chance de testá-la.

**Referências**

[helpx.adobe.com - Configuração do Dispatcher](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html)

### Busca automática

Seu site tem um padrão de acesso muito específico. Você tem uma grande quantidade de tráfego de entrada e a maior parte do tráfego se concentra em uma pequena fração de suas páginas. O home page, suas landings page de campanha e suas páginas de detalhes de produtos mais destacadas recebem 90% do tráfego. Ou se você operar um novo site, os artigos mais recentes têm números de tráfego mais altos em comparação aos mais antigos.

Agora, essas páginas são muito provavelmente armazenadas em cache no Dispatcher, pois são solicitadas com tanta frequência.

Uma solicitação de invalidação arbitrária é enviada ao Dispatcher, fazendo com que todas as páginas - incluindo a mais popular uma vez - sejam invalidadas.

Subsequentemente, como essas páginas são tão populares, há novas solicitações recebidas de navegadores diferentes. Vamos pegar a home page como exemplo.

Como agora o cache é inválido, todas as solicitações para o home page que entram ao mesmo tempo são encaminhadas para o sistema de publicação gerando uma carga alta.

![Solicitações paralelas ao mesmo recurso em cache vazio: As solicitações são encaminhadas para publicação](assets/chapter-1/parallel-requests.png)

*Solicitações paralelas ao mesmo recurso em cache vazio: As solicitações são encaminhadas para publicação*

Com a busca automática você pode mitigar isso até certo ponto. A maioria das páginas invalidadas ainda são armazenadas fisicamente no Dispatcher após a invalidação automática. Eles são apenas _considerados_ obsoletos. _A_ Refetch Automática significa que você ainda serve essas páginas obsoletas por alguns segundos, iniciando  _uma solicitação_ única ao sistema de publicação para buscar novamente o conteúdo obsoleto:

![Fornecer conteúdo obsoleto ao buscar novamente em segundo plano](assets/chapter-1/fetching-background.png)

*Fornecer conteúdo obsoleto ao buscar novamente em segundo plano*

<br> 

Para habilitar a rebusca, você deve informar ao Dispatcher quais recursos devem ser buscados novamente após uma invalidação automática. Lembre-se de que qualquer página que você ativar também invalida automaticamente todas as outras páginas - incluindo as páginas populares.

Rebuscar significa realmente avisar o Dispatcher em cada (!) solicitação de invalidação que você deseja recuperar as mais populares - e quais são as mais populares.

Isso é feito colocando uma lista de URLs de recursos (URLs reais - não apenas caminhos) no corpo das solicitações de invalidação:

```
POST /dispatcher/invalidate.cache HTTP/1.1

CQ-Action: Activate
CQ-Handle: /content/my-brand/home/path/to/some/resource
Content-Type: Text/Plain
Content-Length: 207

/content/my-brand/home.html
/content/my-brand/campaigns/landing-page-1.html
/content/my-brand/campaigns/landing-page-2.html
/content/my-brand/products/product-1.html
/content/my-brand/products/product-2.html
```

Quando o Dispatcher vir tal solicitação, ele acionará a invalidação automática como de costume e colocará imediatamente em fila solicitações para buscar novamente conteúdo novo no sistema de publicação.

Como agora estamos usando um corpo de solicitação, também precisamos definir o tipo de conteúdo e a duração do conteúdo de acordo com o padrão HTTP.

O Dispatcher também marca os URLs de acordo internamente para que ele saiba que pode entregar esses recursos diretamente, mesmo que sejam considerados inválidos pela invalidação automática.

Todos os URLs listados são solicitados um por um. Portanto, você não precisa se preocupar em criar uma carga muito alta nos sistemas de publicação. Mas você também não gostaria de colocar URLs demais nessa lista. No final, a fila precisa ser processada em algum momento delimitado para não fornecer conteúdo obsoleto por muito tempo. Basta incluir as 10 páginas acessadas com mais frequência.

Se você observar o diretório de cache do Dispatcher, verá arquivos temporários marcados com carimbos de data e hora. Esses são os arquivos que estão sendo carregados no momento em segundo plano.

**Referências**

[helpx.adobe.com - Invalidando páginas em cache do AEM](https://helpx.adobe.com/experience-manager/dispatcher/using/page-invalidate.html)

### Como proteger o sistema de publicação

O Dispatcher oferece um pouco de segurança extra protegendo o sistema de publicação de solicitações destinadas apenas para fins de manutenção. Por exemplo, você não deseja expor ao público os URLs `/crx/de` ou `/system/console`.

Não é prejudicial ter um firewall de aplicativo da Web (WAF) instalado no sistema. Mas isso acrescenta um número significativo ao seu orçamento e nem todos os projetos se encontram numa situação em que podem pagar e - para não esquecer - operar e manter um WAF.

O que vemos com frequência é um conjunto de regras de regravação do Apache na configuração do Dispatcher que impedem o acesso aos recursos mais vulneráveis.

Mas você também pode considerar uma abordagem diferente:

De acordo com a configuração do Dispatcher, o módulo Dispatcher está vinculado a um determinado diretório:

```
<Directory />
  SetHandler dispatcher-handler
  …
</Directory>
```

Mas por que vincular o manipulador a todo o ponto, quando você precisa filtrar depois?

Em primeiro lugar, você pode restringir a ligação do manipulador. `SetHandler` apenas vincula um manipulador a um diretório, é possível vincular o manipulador a um URL ou a um padrão de URL:

```
<LocationMatch "^(/content|/etc/design|/dispatcher/invalidate.cache)/.\*">
  SetHandler dispatcher-handler
</LocationMatch>

<LocationMatch "^/dispatcher/invalidate.cache">
  SetHandler dispatcher-handler
</LocationMatch>

…
```

Se você fizer isso, não se esqueça de vincular sempre o manipulador do dispatcher ao URL de invalidação do Dispatcher. Caso contrário, você não poderá enviar solicitações de invalidação do AEM para o Dispatcher.

Outra alternativa para usar o Dispatcher como filtro é configurar diretivas de filtro em `dispatcher.any`

```
/filter {
  /0001  { /glob "\*" /type "deny" }
  /0002  { /type "allow"  /url "/content\*"  }
```

Não estamos a impor a utilização de uma diretiva em vez da outra, pelo contrário, recomendamos uma combinação adequada de todas as diretivas.

Mas propomos que você considere restringir o espaço do URL o mais cedo possível na cadeia, o máximo que precisar, e faça isso da maneira mais simples possível. Lembre-se de que essas técnicas não substituem um WAF em sites altamente sensíveis. Algumas pessoas chamam essas técnicas de &quot;firewall do homem pobre&quot; - por uma razão.

**Referências**

[apache.org - diretiva do sethandler](https://httpd.apache.org/docs/2.4/mod/core.html#sethandler)

[helpx.adobe.com - Configuração do acesso ao filtro de conteúdo](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html#ConfiguringAccesstoContentfilter)

### Filtragem usando Expressões regulares e Globs

Nos primeiros dias você só podia usar &quot;globs&quot; - espaços reservados simples para definir filtros na configuração do Dispatcher.

Por sorte isso mudou nas versões posteriores do Dispatcher. Agora, você também pode usar expressões regulares POSIX e acessar várias partes de uma solicitação para definir um filtro. Para alguém que acaba de começar com o Dispatcher que pode ser considerado um dado adquirido. Mas se você está acostumado a ter globos apenas, é uma surpresa e facilmente pode ser ignorada. Além da sintaxe de globos e regex é muito semelhante. Vamos comparar duas versões que fazem o mesmo:

```
# Version A

/filter {
  /0001  { /glob "\*" /type "deny" }
  /0002  { /type "allow"  /url "/content\*"  }

# Version B

/filter {
  /0001  { /glob "\*" /type "deny" }
  /0002  { /type "allow"  /url '/content.\*'  }
```

Você vê a diferença?

A versão B usa aspas simples `'` para marcar um _padrão de expressão regular_. &quot;Qualquer caractere&quot; é expresso usando `.*`.

_Os padrões_ de globalização, em contraste, usam aspas de duplo  `"` e você só pode usar espaços reservados simples como  `*`.

Se você sabe essa diferença, é trivial - mas se não, você pode facilmente misturar as citações e passar uma tarde ensolarada depurando sua configuração. Agora você é avisado.

&quot;Reconheço `'/url'` na configuração... Mas o que é isso `'/glob'` no filtro que você pode perguntar?

Essa diretiva representa toda a sequência de solicitação, incluindo o método e o caminho. Poderia significar

`"GET /content/foo/bar.html HTTP/1.1"`

esta é a string com a qual seu padrão seria comparado. Os iniciantes tendem a esquecer a primeira parte, a `method` (GET, POST, ...). Então, um padrão

`/0002  { /glob "/content/\*" /type "allow" }`

Sempre falharia, pois &quot;/content&quot; não corresponde a &quot;GET..&quot; do pedido.

Então quando quiserem usar o Globs...

`/0002  { /glob "GET /content/\*" /type "allow" }`

estaria correto.

Para uma regra de negação inicial, como

`/0001  { /glob "\*" /type "deny" }`

isso é bom. Mas para as autorizações subsequentes, é melhor e mais claro e mais expressivo e muito mais seguro usar as partes individuais de um pedido:

```
/method
/url
/path
/selector
/extension
/suffix
```

Assim:

```
/005  {

  /type "allow"
  /method "GET"
  /extension '(css|gif|ico|js|png|swf|jpe?g)' }
```

Observe que você pode misturar expressões regex e globais em uma regra.

Uma última palavra sobre os &quot;números de linha&quot; como `/005` em frente a cada definição.

Eles não têm nenhum significado! É possível escolher denominadores arbitrários para regras. Usar números não requer muito esforço para pensar sobre um esquema, mas tenha em mente que a ordem é importante.

Se você tem centenas de regras como essa:

```
/001
/002
/003
…
/100
…
```

e você deseja inserir um entre /001 e /002, o que acontece com os números subsequentes? Você está aumentando seus números? Você está inserindo números intermediários?

```
/001
/001a
/002
/003
…
/100
…
```

Ou o que acontece se você mudar para reordenar /003 e /001, você vai alterar os nomes e suas identidades ou você

```
/003
/002
/001
…
/100
…
```

A numeração, ao mesmo tempo que parece uma escolha simples, atinge seus limites a longo prazo. Sejamos honestos, escolher números como identificadores é um mau estilo de programação de qualquer forma.

Gostaríamos de propor uma abordagem diferente: Provavelmente, você não encontrará identificadores significativos para cada regra de filtro individual. Mas eles provavelmente servem a um propósito maior, então eles podem ser agrupados de alguma forma de acordo com esse propósito. Por exemplo, &quot;configuração básica&quot;, &quot;exceções específicas do aplicativo&quot;, &quot;exceções globais&quot; e &quot;segurança&quot;.

Você pode nomear e agrupar as regras de acordo e fornecer ao leitor da configuração (seu querido colega) uma orientação no arquivo:

```plain
  # basic setup:

  /filter {

    # basic setup

    /basic_01  { /glob "\*"             /type "deny"  }
    /basic_02  { /glob "/content/\*"    /type "allow" }
    /basic_03  { /glob "/etc/design/\*" /type "allow" }

    /basic_04  { /extension '(json|xml)'  /type "deny"  }
    …


    # login

    /login_01 { /glob "/api/myapp/login/\*" /type "allow" }
    /login_02 { … }

    # global exceptions

    /global_01 { /method "POST" /url '.\*contact-form.html' }
```


Provavelmente você adicionará uma nova regra a um dos grupos - ou talvez até mesmo criará um novo grupo. Nesse caso, o número de itens para renomear/renumerar é limitado a esse grupo.

>[!WARNING]
>
>As configurações mais sofisticadas dividem as regras de filtragem em vários arquivos, incluídos pelo arquivo de configuração `dispatcher.any` principal. No entanto, um novo arquivo não apresenta uma nova namespace. Portanto, se você tiver uma regra &quot;001&quot; em um arquivo e &quot;001&quot; em outro, você receberá um erro. Mais razão ainda para criar nomes semânticos fortes.

**Referências**

[helpx.adobe.com - Criar padrões para propriedades globais](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html#DesigningPatternsforglobProperties)

### Especificação do protocolo

A última gorjeta não é uma gorjeta real, mas sentimos que valia a pena compartilhar isso com vocês de qualquer forma.

AEM e o Dispatcher na maioria dos casos funcionam prontamente. Portanto, você não encontrará uma especificação abrangente do protocolo Dispatcher sobre o protocolo de invalidação para criar seu próprio aplicativo no topo. A informação é pública, mas um pouco dispersa sobre uma série de recursos.

Tentamos preencher a lacuna até certo ponto aqui. Esta é a aparência de uma solicitação de invalidação:

```
POST /dispatcher/invalidate.cache HTTP/1.1
CQ-Action: <action>
CQ-Handle: <path-pattern>
[CQ-Action-Scope]
[Content-Type: Text/Plain]
[Content-Length: <bytes in request body>]

<newline>

<refetch-url-1>
<refetch-url-2>

…

<refetch-url-n>
```

`POST /dispatcher/invalidate.cache HTTP/1.1` - A primeira linha é o URL do ponto de extremidade de controle do Dispatcher e você provavelmente não o alterará.

`CQ-Action: <action>` - O que deve acontecer. `<action>` é:

* `Activate:` exclui  `/path-pattern.*`
* `Deactive:` excluir  `/path-pattern.*`
E excluir  `/path-pattern/*`
* `Delete:`   excluir  `/path-pattern.*`
E excluir 
`/path-pattern/*`
* `Test:`   Devolva &quot;ok&quot;, mas não faça nada

`CQ-Handle: <path-pattern>` - O caminho do recurso de conteúdo a ser invalidado. Observe que `<path-pattern>` é na verdade um &quot;caminho&quot; e não um &quot;padrão&quot;.

`CQ-Action-Scope: ResourceOnly` - Facultativo: Se este cabeçalho estiver definido, o  `.stat` arquivo não será tocado.

```
[Content-Type: Text/Plain]
[Content-Length: <bytes in request body>]
```

Defina esses cabeçalhos se você definir uma lista de URLs de rebusca automática. `<bytes in request body>` é o número de caracteres no corpo HTTP

`<newline>` - Se você tiver um corpo de solicitação, ele deverá ser separado do cabeçalho por uma linha vazia.

```
<refetch-url-1>
<refetch-url-2>
…
<refetch-url-n>
```

Lista os URLs, que você deseja recuperar imediatamente após a invalidação.

## Recursos adicionais

Uma boa visão geral e introdução ao Dispatcher caching: [https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher.html](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher.html)

Mais dicas e truques de otimização: [https://helpx.adobe.com/experience-manager/kb/optimizing-the-dispatcher-cache.html#use-ttls](https://helpx.adobe.com/experience-manager/kb/optimizing-the-dispatcher-cache.html#use-ttls)

Documentação do Dispatcher com todas as diretivas explicadas: [https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html](https://helpx.adobe.com/experience-manager/dispatcher/using/dispatcher-configuration.html)

Algumas perguntas frequentes: [https://helpx.adobe.com/experience-manager/using/dispatcher-faq.html](https://helpx.adobe.com/experience-manager/using/dispatcher-faq.html)

Gravação de um webinar sobre a otimização do Dispatcher - altamente recomendado: [https://my.adobeconnect.com/p7th2gf8k43?proto=true](https://my.adobeconnect.com/p7th2gf8k43?proto=true)

Apresentação &quot;O poder subestimado da invalidação de conteúdo&quot;, conferência &quot;adaptTo()&quot; em Potsdam 2018 [https://adapt.to/2018/en/schedule/the-underappreciated-power-of-content-invalidation.html](https://adapt.to/2018/en/schedule/the-underappreciated-power-of-content-invalidation.html)

A Invalidar Páginas Em Cache De AEM: [https://helpx.adobe.com/experience-manager/dispatcher/using/page-invalidate.html](https://helpx.adobe.com/experience-manager/dispatcher/using/page-invalidate.html)

## Próxima etapa

* [2 - Padrão de infraestrutura](chapter-2.md)
