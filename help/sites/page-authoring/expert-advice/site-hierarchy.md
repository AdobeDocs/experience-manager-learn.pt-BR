---
title: Guia de marcação, taxonomia e hierarquia do site
seo-title: AEM Sites Site Hierarchy, Taxonomy, and Tagging Guide
description: Uma visão geral completa dos metadados, marcação, taxonomia e hierarquia do AEM Sites. Use este guia para garantir que sua estratégia de conteúdo seja consistente e siga as práticas recomendadas
seo-description: A full overview of AEM Sites metadata, tagging, taxonomy, and hierarchy. Use this guide to ensure your content strategy is consistent and following best practices
audience: author, marketer
topic: Content Management
feature: Learn From Your Peers
role: Admin, User
jira: KT-14254
level: Beginner, Intermediate
exl-id: c88c3ec7-9060-43e2-a6a2-d47bba6f7cf3
source-git-commit: 3752e22455020b58d23524f7e6a99414e773422d
workflow-type: tm+mt
source-wordcount: '2034'
ht-degree: 0%

---

# Práticas recomendadas de tags, taxonomia e metadados: resumo de alto nível

Metadados e tags são fundamentais para impulsionar a eficiência no AEM. Usuários, líderes e gerentes percebem a necessidade de uma estratégia holística, mas acham difícil fazer avançar. Muitas vezes, o conhecimento é isolado entre os usuários, dificultando a estratégia holística — e torna os ajustes ainda mais problemáticos.

Qual é a diferença entre metadados e tags? Quais são os aspectos comerciais a serem considerados ao orientar sua estratégia?

## Qual é a finalidade dos metadados?

Os metadados adicionam estrutura ao conteúdo menos estruturado.
Exemplo: uma imagem básica tem pixels. Podemos chamá-los de &quot;dados principais&quot;. São os metadados que descrevem o formato, a categoria, os detalhes de licenciamento etc.
Os metadados são usados com mais frequência para ativos. Mas também há um grande número de casos de uso de metadados em páginas de conteúdo ou fragmentos de experiência.

## Fontes de metadados

Estas são as categorias para as quais os metadados podem ser gerados:

* Metadados extraídos - as informações já estão disponíveis no documento, por exemplo, em linguagem natural.
* Metadados derivados - as informações não estão disponíveis nos dados originais, mas podem ser derivadas fazendo referência cruzada com o conhecimento anterior.
* Metadados adicionados manualmente - São metadados que não se encaixam em nenhuma das primeiras categorias e precisam ser adicionados manualmente por um humano.

## Tipos de metadados

Dentro das categorias listadas acima, há quatro tipos principais:

* Metadados técnicos e descritivos: fornece informações sobre detalhes técnicos do conteúdo (ou seja, título, idioma etc.)
* Metadados operacionais: documenta o ciclo de vida de um ativo (ou seja, aprovado, em criação, campanha)
* Metadados administrativos: o status ou o estado de um ativo em uma organização (ou seja, informações de licença, propriedade)
* Metadados estruturais: ajuda a categorizar ativos ou páginas para facilitar o processo de negócios (aplica-se à maioria das tags e taxonomias)

## Nomes de Pasta e Arquivo

As pastas são uma maneira natural de navegar e navegar pelo conteúdo no AEM. Como suas partes interessadas vão interagir com o AEM? Isso determinará como as pastas são estruturadas. Normalmente, a estrutura de pastas é projetada com um (ou dois) dos seguintes itens em mente:

* Navegação
* Navegação
* Categorização
* Controle de acesso

Para o AEM Sites, a navegação é a chave. As pastas são usadas para controlar o acesso aos ativos e páginas.

Quais níveis de autores precisarão acessar as páginas iniciais? E as páginas do produto? Ou campanha? Use as permissões e a estrutura de pastas para colocar no controle correto.

## Armazenamento de metadados

Há três maneiras de armazenar metadados:

* Binário: formato binário relacionado à natureza do ativo (Photoshop, InDesign, PNG, JPG).
* Nó do ativo: são metadados no próprio ativo, independentemente do sistema ou processo que está sendo usado.
* Local externo: metadados que não estão diretamente no ativo, mas podem ser usados como um descritor do &quot;estado&quot; de um ativo (exemplo: um fluxo de trabalho que pode afetar um ativo, mas não aplicado diretamente a ele)

## Modelo de metadados

A estrutura de como os metadados são capturados e formatados é chamada de modelo ou esquema de metadados. Isso deve ser acordado antes que os ativos ou as páginas sejam assimilados no sistema.

Um modelo de metadados geralmente é projetado para atender aos seguintes casos de uso:

* Pesquisa e recuperação: ajuda a armazenar os principais aspectos do conteúdo, o que ajuda a facilitar a recuperação pela empresa.
* Reutilização: ajuda a utilizar ativos antigos para reutilização (economizando tempo e dinheiro)
* Gerenciamento de licenças: rastreia a propriedade do ativo da organização (geralmente por motivos legais)
* Distribuir: disponibilizar conteúdo para consumidores ou agregar ativos a parceiros de negócios.
* Arquivamento: os metadados que observam que um ativo está desatualizado (sempre é prática recomendada colocar um sinalizador &quot;arquivado&quot; no ativo para não perder informações vitais)/
* Referência cruzada: Metadados associativos que capturam a relação de dois ou mais ativos entre si (a síntese de metadados permite a referência cruzada e a organização coerente do grupo)
* Navegar: a estrutura de pastas na qual os ativos são armazenados (usada para recuperar informações ao navegar)

*Os metadados do autor suportam principalmente processos operacionais. A publicação aceita casos de uso de recuperação e distribuição.*

## Uso de tags como termos predefinidos

Uma tag é uma palavra-chave ou termo atribuído a uma informação.vPor exemplo, em vez de inserir &quot;carro&quot;, &quot;veículo&quot;, &quot;automóvel&quot;, um sistema de tags permite escolher apenas um valor, tornando a pesquisa mais previsível.  As tags normalizam e simplificam a categorização de ativos.

*Observação: embora o AEM permita a marcação ad-hoc, é prática recomendada deixar isso para trás, pois isso pode levar a uma taxonomia indefinida e difícil de controlar.*

Usos comuns de tags:

* Pesquisa por palavra-chave: uma tag pode descrever que um recurso pertence a um determinado grupo de entidades. Por exemplo, uma tag &quot;image/subject/car&quot; descreve o recurso que pertence ao conjunto de imagens que mostram um carro.
* Relações de direção: todos os recursos que compartilham a mesma tag podem ser considerados conectados. Marcar tags em vez de vincular diretamente é especialmente útil em sites que têm muito conteúdo dinâmico e conectado.
* Navegação de unidade: tags ordenadas em taxonomia hierárquica podem criar navegação, um link ou um link para documentos semelhantes.
As tags também devem ser buscadas como informações que conectam vários tipos de dados, com base em termos comerciais, em vez de em propriedades técnicas.

## Aplicativos comuns de tags

Quando usadas no AEM, as tags podem ajudar a obter uma implementação muito mais curta do recurso complexo, como:

* Pesquisa facetada
* Navegação personalizada
* Conteúdo relacionado
* Referências do conteúdo
* Otimização do mecanismo de pesquisa
* Destaque dos principais conceitos

## Taxonomias

Uma taxonomia é um sistema de organização de tags com base em características compartilhadas, que geralmente são hierarquizadas de acordo com a necessidade organizacional. A estrutura pode ajudar a encontrar uma tag mais rapidamente ou impor uma generalização.
Exemplo: é necessário classificar as imagens de stock de carros.  A taxonomia pode ser semelhante a:

/subject/car/ /subject/car/sportscar /subject/car/sportscar/porsche /subject/car/sportscar/ferrari ... /subject/car/minivan /subject/car/minivan/mercedes /subject/car/minivan/volkswagen ... /subject/car/limousine ...

Agora um usuário pode escolher se ele quer olhar para imagens de carros esportivos em geral ou um &quot;Porsche&quot; em particular. Afinal, ambos são carros esportivos.
Prática recomendada: evitar taxonomias simples. As taxonomias planas não apresentam os benefícios descritos acima e exigem manutenção constante

**Usando uma Taxonomia como Tesauro.**  Quando um usuário pesquisa uma palavra-chave, o sistema cria uma segunda pesquisa para todos os sinônimos encontrados lá.
Além disso, em vez de digitar &quot;car&quot; manualmente, o sistema pode fornecer uma lista de palavras-chave para melhorar a consistência.

**Usando uma taxonomia como dicionário.** Em vez de apenas imprimir o &quot;carro&quot;, é possível expandir a tag única e usar todos os sinônimos da tag.

**Várias Categorias.** Ao contrário de uma hierarquia de pastas, as tags podem ser usadas para expressar várias categorizações ao mesmo tempo. Um ativo marcado com:

/subject/car/minivan/mercedes /subject/people/family /color/red

## Metadados vs tag

Nem todos os metadados devem ser considerados como candidatos ao sistema de marcação. Os metadados técnicos podem duplicar desnecessariamente as informações. O melhor candidato para tags é o uso de metadados comerciais. . Tags são uma boa opção para impor um vocabulário consistente, pesquisa facetada e navegação.

## Gerenciamento de tags

Os benefícios do gerenciamento de tags são obtidos com uma equipe principal dedicada. Os novos membros devem conhecer a finalidade e a função da taxonomia primeiro antes de adicionar novas tags.  Especialistas experientes, atuando como guardiões das novas tags, reduzirão a inconsistência a longo prazo.

## Criação de tag

As taxonomias devem ser empregadas por autores de conteúdo e compreendidas pelos usuários finais. Eles devem ser criados antes do processo de criação do conteúdo. Quaisquer atalhos resultarão em esforço adicional para gerenciamento e manutenção.

## Manutenção contínua

As coisas mudam, e as necessidades da lista de tags também mudarão. Crie um processo de manutenção sólido que reduzirá a duplicação.

Certifique-se de que os colaboradores de conteúdo saibam como podem propor alterações e que os editores ou gerentes de conteúdo estejam revisando os termos regularmente.

## Práticas recomendadas com tags e taxonomias

**Padronizar tags.** Crie um glossário que forneça um vocabulário autorizado. Sem o estabelecimento de normas, a duplicação causará problemas. Além disso, é recomendável auditar não apenas a taxonomia, mas também o uso das tags.

**Não exagere.** As tags podem perder importância se forem distribuídas com muita frequência.Remova as tags irrelevantes para obter eficiência ideal.

**Reavalie As Tags Ao Longo Do Tempo.** Lembre-se de que a terminologia e o contexto de negócios raramente permanecem estáticos. Talvez você precise padronizar e reaplicar tags.

**Uso da marcação inteligente habilitada por IA.** Marcação inteligente [consulte o link] O é um recurso de IA no AEM que reduz o esforço de marcar ativos manualmente. A marcação inteligente usa uma IA para inferir informações sobre o assunto de uma imagem. Ele gera tags descritivas que descrevem o conteúdo de uma imagem.

## Qualidade e manutenção dos metadados

Entender os requisitos de negócios é uma etapa importante na execução de um modelo de gerenciamento de metadados. Sem definição, as informações não podem ser armazenadas. Será necessário rever o modelo periodicamente. Esta é uma atividade vital de controle de qualidade.

Além disso, os metadados devem ser capturados o mais cedo possível no processo de criação de conteúdo. Se os metadados não forem &#39;aplicados na hora certa, há pouca chance de aplicá-los retroativamente.

**Utilizar metadados** para aprimorar a colaboração: utilize o Adobe Asset Link, o Adobe Bridge e o AEM Desktop para unir o processo criativo e utilize metadados para simplificar fluxos de trabalho criativos. O uso dessas ferramentas enriquecerá os metadados e a experiência do usuário em todo o processo criativo.

## Práticas recomendadas para gerenciamento de metadados

* Atribuir a equipe principal com mandato executivo forte: forme uma equipe principal de metadados que tenha total compreensão do ecossistema de negócios e uma forte exigência da administração da organização.
* Definir a estratégia e a governança de metadados: uma boa estratégia de metadados pode ajudar as organizações a explicar a necessidade e os benefícios dos metadados.  Uma estratégia consiste em esquemas de metadados, taxonomia, processos de negócios (para qualidade e captura de dados), funções e responsabilidades e processos de governança. *
* Definir e comunicar o modelo de metadados consistente: a estratégia e o raciocínio definidos devem ser bem documentados e comunicados nas organizações.
* Convenção de nomenclatura padrão: crie uma convenção de nomenclatura de arquivo consistente e descritiva para melhorar a identidade visual, o gerenciamento de informações e a usabilidade.
* Caracteres de segurança em nomes de arquivo: o nome de arquivo deve poder ser interpretado por todos os sistemas operacionais comuns. É seguro usar caracteres, números, tremas, espaços e sublinhados. O símbolo de menos também é seguro, mas se você recortar e colar, poderá parecer um &quot;traço&quot;.
* Convenção de nomenclatura de versão: o AEM oferece alguns recursos para manter versões anteriores de ativos. Em alguns casos, convém manter várias versões. No entanto, verifique se o esquema de controle de versão é consistente.

## Metadados organizacionais vs. descritivos

Algumas diretrizes podem ajudar você a decidir como categorizar metadados:

**Descrição** - Se os dados descreverem o ativo ou parte do conteúdo, ele deverá fazer parte dos metadados anexados.

**Pesquisar** - Se os metadados forem utilizados na pesquisa, devem ser anexados.

**Exposição** - Se estiver expondo os metadados em uma plataforma de distribuição a terceiros, tenha cuidado para não expor também metadados &quot;internos&quot;.

**Duração** - Quanto mais tempo os metadados tiverem de viver, mais provável será que sejam um bom candidato para os metadados anexados.

**Processos de negócios relacionados** - Definitivamente, é útil ter uma ID de produto permanente como parte dos metadados. Mas a categoria de um item em relação ao catálogo de produtos é um metadado questionável para o ativo.

**Organização e processamento** - Se a natureza dos metadados for de natureza organizacional, como estado em um fluxo de trabalho de aprovação ou propriedade de um determinado departamento, os metadados externos devem ser considerados ao anexar os metadados ao ativo.

*Para criar a estratégia, faça as seguintes perguntas:*

* Que tipo de conteúdo e &quot;informações adicionais&quot; (= metadados) são necessários para resolver problemas de negócios/perguntas de negócios/problemas de negócios?
* Quais são as variáveis, os &quot;campos&quot; no esquema e quais são os valores possíveis? Quais variáveis precisam de uma entrada de texto livre, quais podem ser reduzidas por tipo (número, data, booleano, ...), um conjunto de valores fixos (por exemplo, países) ou tags de uma determinada taxonomia. Quantas tags são necessárias, permitidas?
* Quais problemas/problemas/perguntas técnicos podem ser resolvidos pelos metadados?
* Como você pode obter/criar esse conteúdo/metadados? Quanto custará para obter/criar esses metadados?
* Quais tipos de metadados são necessários para um grupo de usuários específico?
* Como os metadados serão mantidos e atualizados?
* Quem é responsável por que parte do processo?
* Como você pode garantir que os processos de negócios acordados sejam seguidos?
* Quais padrões você deve seguir? Você deve adotar e modificar um padrão do setor (Dublin Core, ISO 19115, PRISM etc.) ou a organização deve criar seus próprios padrões?
* Onde a estratégia está documentada? Como você pode garantir que todas as partes interessadas tenham acesso? Como você pode ter certeza de que a equipe recém-integrada segue o padrão acordado (por exemplo, visitar o treinamento antes de obter acesso)?
