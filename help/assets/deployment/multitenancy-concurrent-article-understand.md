---
title: Noções básicas sobre multilocação e desenvolvimento simultâneo
description: Saiba mais sobre os benefícios, os desafios e as técnicas para gerenciar uma implementação de vários locatários com o Adobe Experience Manager Assets.
feature: Connected Assets
version: 6.5
topic: Development
role: Developer
level: Intermediate
exl-id: c9ee29d4-a8a5-4e61-bc99-498674887da5
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '2017'
ht-degree: 0%

---

# Noções básicas sobre multilocação e desenvolvimento simultâneo {#understanding-multitenancy-and-concurrent-development}

## Introdução {#introduction}

Quando várias equipes estão implantando seu código nos mesmos ambientes de AEM, há práticas que elas devem seguir para garantir que as equipes possam trabalhar da maneira mais independente possível, sem pisar nos dedos das outras equipes. Embora nunca possam ser totalmente eliminadas, essas técnicas minimizarão as dependências entre equipes. Para que um modelo de desenvolvimento simultâneo seja bem-sucedido, é essencial uma boa comunicação entre as equipes de desenvolvimento.

Além disso, quando várias equipes de desenvolvimento trabalham no mesmo ambiente AEM, provavelmente há algum grau de multilocação em jogo. Muito tem sido escrito sobre as considerações práticas da tentativa de oferecer suporte a vários locatários em um ambiente AEM, especialmente sobre os desafios enfrentados ao gerenciar governança, operações e desenvolvimento. Este documento explora alguns dos desafios técnicos relacionados à implementação do AEM em um ambiente de vários locatários, mas muitas dessas recomendações serão aplicadas a qualquer organização com várias equipes de desenvolvimento.

É importante observar desde já que, embora o AEM possa suportar vários sites e até mesmo várias marcas sendo implantadas em um único ambiente, ele não oferece uma verdadeira multilocação. Algumas configurações de ambiente e recursos de sistema serão sempre compartilhados em todos os locais implantados em um ambiente. Este documento fornece orientação para minimizar os impactos desses recursos compartilhados e oferece sugestões para simplificar a comunicação e a colaboração nessas áreas.

## Benefícios e desafios {#benefits-and-challenges}

Há muitos desafios para implementar um ambiente com vários locatários.

Dentre elas:

* Complexidade técnica adicional
* Maior sobrecarga de desenvolvimento
* Dependências entre organizações no recurso compartilhado
* Maior complexidade operacional

Apesar dos desafios enfrentados, a execução de um aplicativo com vários locatários tem benefícios, como:

* Custos reduzidos de hardware
* Tempo de entrada no mercado reduzido para sites futuros
* Custos de implementação mais baixos para futuros locatários
* Práticas padrão de arquitetura e desenvolvimento na empresa
* Uma base de código comum

Se o negócio exigir uma verdadeira multilocação, com zero conhecimento de outros locatários e nenhum código compartilhado, conteúdo ou autores comuns, as instâncias de autor separadas são a única opção viável. O aumento global do esforço de desenvolvimento deve ser comparado com as economias em infraestrutura e custos de licença para determinar se esta abordagem é a melhor opção.

## Técnicas de desenvolvimento {#development-techniques}

### Gerenciamento de dependências {#managing-dependencies}

Ao gerenciar as dependências do projeto Maven, é importante que todas as equipes usem a mesma versão de um determinado pacote OSGi no servidor. Para ilustrar o que pode dar errado quando projetos Maven são mal gerenciados, apresentamos um exemplo:

O projeto A depende da versão 1.0 da biblioteca, foo; foo versão 1.0 é incorporado em sua implantação no servidor. O projeto B depende da versão 1.1 da biblioteca, foo; foo versão 1.1 é incorporado em sua implantação.

Além disso, vamos supor que uma API tenha sido alterada nesta biblioteca entre as versões 1.0 e 1.1. Nesse ponto, um desses dois projetos não funcionará mais direito.

Para resolver essa preocupação, recomendamos tornar todos os projetos Maven filhos de um projeto principal de reator. Esse projeto de reator serve dois propósitos: permite a construção e implantação de todos os projetos juntos se desejado e contém as declarações de dependência para todos os projetos secundários. O projeto principal define as dependências e suas versões, enquanto os projetos secundários declaram apenas as dependências que exigem, herdando a versão do projeto principal.

Nesse cenário, se a equipe que trabalha no Projeto B exigir a funcionalidade na versão 1.1 do foo, rapidamente se tornará evidente no ambiente de desenvolvimento que essa alteração quebrará o Projeto A. Neste ponto, as equipes podem discutir essa alteração e tornar o Projeto A compatível com a nova versão ou procurar uma solução alternativa para o Projeto B.

Observe que isso não elimina a necessidade de essas equipes compartilharem essa dependência. Basta destacar os problemas de forma rápida e antecipada para que as equipes possam discutir quaisquer riscos e chegar a um acordo sobre uma solução.

### Como evitar a duplicação de código {#preventing-code-duplication-nbsp-br}

Ao trabalhar em vários projetos, é importante garantir que o código não seja duplicado. A duplicação de código aumenta a probabilidade de ocorrências de defeitos, o custo de alterações no sistema e a rigidez geral na base de código. Para evitar a duplicação, refatore a lógica comum em bibliotecas reutilizáveis que podem ser usadas em vários projetos.

Para dar suporte a essa necessidade, recomendamos o desenvolvimento e a manutenção de um projeto principal no qual todas as equipes possam depender e contribuir. Ao fazer isso, é importante garantir que esse projeto principal não dependa, por sua vez, de nenhum dos projetos de equipes individuais. Isso permite uma implantabilidade independente e, ao mesmo tempo, promove a reutilização do código.

Alguns exemplos de código que geralmente em um módulo principal incluem:

* Configurações em todo o sistema, como:
   * Configurações do OSGi
   * Filtros de servlet
   * Mapeamentos ResourceResolver
   * Pipelines do Sling Transformer
   * Handlers de erros (ou use o ACS AEM Commons Error Page Handler1)
   * Servlets de autorização para armazenamento em cache sensível a permissões
* Classes de utilitário
* Lógica de negócios principal
* Lógica de integração de terceiros
* Sobreposições da interface de criação
* Outras personalizações necessárias para criação, como widgets personalizados
* Iniciadores de fluxo de trabalho
* Elementos de design comuns usados em sites

*Arquitetura de projeto modular*

Isso não elimina a necessidade de várias equipes dependerem e possivelmente atualizarem o mesmo conjunto de códigos. Ao criar um projeto principal, diminuímos o tamanho da base de código que é compartilhado entre as equipes, diminuindo mas não eliminando a necessidade de recursos compartilhados.

Para garantir que as alterações feitas nesse pacote principal não interrompam a funcionalidade do sistema, recomendamos que um desenvolvedor ou equipe sênior de desenvolvedores mantenha a supervisão. Uma opção é ter uma única equipe que gerencie todas as alterações neste pacote; outra é ter equipes que enviem solicitações de pull que são revisadas e mescladas por esses recursos. É importante que um modelo de governança seja projetado e acordado pelas equipes e que os desenvolvedores o sigam.

## Gerenciando Escopo de Implantação  {#managing-deployment-scope}

À medida que equipes diferentes implantam seu código no mesmo repositório, é importante que elas não substituam as alterações umas das outras. O AEM tem um mecanismo para controlar isso ao implantar pacotes de conteúdo, o filtro. arquivo xml. É importante que não haja sobreposição entre os filtros.  arquivos xml, caso contrário, a implantação de uma equipe poderia apagar a implantação anterior de outra equipe. Para ilustrar esse ponto, consulte os seguintes exemplos de arquivos de filtro bem criados vs. problemáticos:

/apps/my-company vs. /apps/my-company/my-site

/etc/clientlibs/my-company vs. /etc/clientlibs/my-company/my-site

/etc/designs/my-company vs. /etc/designs/my-company/my-site

Se cada equipe configurar explicitamente o arquivo de filtro no(s) site(s) em que está trabalhando, cada equipe poderá implantar seus componentes, bibliotecas de clientes e designs de site de maneira independente, sem apagar as alterações umas das outras.

Como é um caminho de sistema global e não específico de um site, o servlet a seguir deve ser incluído no projeto principal, pois as alterações feitas aqui podem afetar qualquer equipe:

/apps/sling/servlet/errorhandler

### Sobreposições {#overlays}

Geralmente, as sobreposições são usadas para estender ou substituir a funcionalidade AEM predefinida, mas o uso de uma sobreposição afeta todo o aplicativo AEM (ou seja, todas as alterações de funcionalidade sobrepostas são disponibilizadas para todos os locatários). Isso seria ainda mais complicado se os locatários tivessem requisitos diferentes para a sobreposição. Idealmente, os grupos empresariais devem trabalhar em conjunto para chegar a um acordo sobre a funcionalidade e a aparência dos consoles administrativos do AEM.

Se não for possível chegar a um consenso entre as várias unidades de negócios, uma solução possível seria simplesmente não usar sobreposições. Em vez disso, crie uma cópia personalizada da funcionalidade e exponha-a por um caminho diferente para cada locatário. Isso permite que cada locatário tenha uma experiência do usuário completamente diferente, mas essa abordagem também aumenta o custo da implementação e dos esforços de atualização subsequentes.

### Inicializadores do fluxo de trabalho {#workflow-launchers}

O AEM usa iniciadores de fluxos de trabalho para acionar automaticamente a execução do fluxo de trabalho quando alterações especificadas são feitas no repositório. O AEM fornece vários iniciadores prontos para uso, por exemplo, para executar processos de geração de representação e extração de metadados em ativos novos e atualizados. Embora seja possível deixar esses iniciadores como estão, em um ambiente multilocatário, se os locatários tiverem requisitos diferentes de iniciador e/ou modelo de fluxo de trabalho, é provável que iniciadores individuais precisem ser criados e mantidos para cada locatário. Esses inicializadores precisarão ser configurados para serem executados nas atualizações de seus locatários, deixando o conteúdo de outros locatários intacto. Faça isso facilmente aplicando iniciadores a caminhos de repositório especificados que são específicos do locatário.

### URLs personalizadas {#vanity-urls}

O AEM fornece funcionalidade de URL personalizado que pode ser definida por página. A preocupação com essa abordagem em um cenário de vários locatários é que o AEM não garante a exclusividade entre os URLs personalizados configurados dessa maneira. Se dois usuários diferentes configurarem o mesmo caminho personalizado para páginas diferentes, um comportamento inesperado poderá ser encontrado. Por esse motivo, recomendamos usar as regras mod_rewrite nas instâncias do Apache Dispatcher, que permitem um ponto central de configuração em conjunto com as regras somente de saída do Resource Resolver.

### Grupos de componentes {#component-groups}

Ao desenvolver componentes e modelos para vários grupos de criação, é importante usar com eficiência as propriedades componentGroup e allowedPaths. Ao aproveitá-los de maneira eficiente com designs de site, podemos garantir que os autores da Marca A vejam apenas os componentes e modelos criados para seu site, enquanto os autores da Marca B veem apenas os deles.

### Testes {#testing}

Embora uma boa arquitetura e canais de comunicação abertos possam ajudar a evitar a introdução de defeitos em áreas inesperadas do site, essas abordagens não são infalíveis. Por isso, é importante testar totalmente o que está sendo implantado na plataforma antes de liberar qualquer item para produção. Isso requer coordenação entre as equipes em seus ciclos de lançamento e reforça a necessidade de um conjunto de testes automatizados que abranjam o máximo possível de funcionalidade. Além disso, como um sistema é compartilhado por várias equipes, o desempenho, a segurança e os testes de carga se tornam mais importantes do que nunca.

## Considerações operacionais {#operational-considerations}

### Recursos compartilhados {#shared-resources}

O AEM é executado em uma única JVM; qualquer aplicativo AEM implantado inerentemente compartilha recursos entre si, além de recursos já consumidos na execução normal do AEM. Dentro do próprio espaço JVM, não há separação lógica de threads, e os recursos finitos disponíveis para o AEM, como memória, CPU e E/S de disco também são compartilhados. Qualquer locatário que consuma recursos inevitavelmente afetará outros locatários do sistema.

### Desempenho {#performance}

Se não seguir as práticas recomendadas para o AEM, é possível desenvolver aplicativos que consumam recursos além do que é considerado normal. Exemplos disso são o acionamento de muitas operações pesadas de fluxo de trabalho (como Atualizar ativo do DAM), o uso de operações de push-on-modify do MSM em muitos nós ou o uso de consultas JCR dispendiosas para renderizar conteúdo em tempo real. Elas inevitavelmente terão um impacto no desempenho de outros aplicativos de locatários.

### Logs {#logging}

O AEM fornece interfaces prontas para uso para uma configuração robusta de logger que pode ser usada para o nosso benefício em cenários de desenvolvimento compartilhados. Ao especificar registradores separados para cada marca, por nome de pacote, podemos alcançar um certo grau de separação de registros. Embora as operações em todo o sistema, como replicação e autenticação, ainda sejam registradas em um local central, o código personalizado não compartilhado pode ser registrado separadamente, facilitando os esforços de monitoramento e depuração da equipe técnica de cada marca.

### Backup e restauração {#backup-and-restore}

Devido à natureza do repositório JCR, os backups tradicionais funcionam em todo o repositório, em vez de em um caminho de conteúdo individual. Portanto, não é possível separar facilmente os backups por locatário. Por outro lado, a restauração a partir de um backup reverterá o conteúdo e os nós do repositório para todos os locatários do sistema. Embora seja possível executar backups de conteúdo direcionados, usando ferramentas como o VLT ou selecionar o conteúdo a ser restaurado criando um pacote em um ambiente separado, esses\
As abordagens do não abrangem facilmente as definições de configuração ou a lógica do aplicativo e podem ser complicadas de gerenciar.

## Considerações sobre segurança {#security-considerations}

### ACLs {#acls}

É claro que é possível usar as Listas de controle de acesso (ACLs, Access Control Lists) para controlar quem tem acesso para exibir, criar e excluir conteúdo com base em caminhos de conteúdo, o que requer a criação e o gerenciamento de grupos de usuários. A dificuldade em manter as ACLs e os grupos depende da ênfase em garantir que cada locatário não tenha nenhum conhecimento sobre os outros e se os aplicativos implantados dependem de recursos compartilhados. Para garantir uma administração eficiente de ACL, usuários e grupos, recomendamos ter um grupo centralizado com a supervisão necessária para garantir que esses controles de acesso e principais se sobreponham (ou não se sobreponham) de uma forma que promova a eficiência e a segurança.
