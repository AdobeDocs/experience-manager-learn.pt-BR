---
title: Modelos e arquétipos de governança e equipe
description: Saiba mais sobre como operacionalizar sua plataforma Adobe Experience Manager (AEM) e aproveitar ao máximo seus esforços.
solution: Experience Manager
exl-id: 808ab7a6-5ec5-4bbd-9a6e-cfc0b447430d
source-git-commit: 89982f506a5e1ffc12f84a0f616aaa1dc2e00c5b
workflow-type: tm+mt
source-wordcount: '1115'
ht-degree: 0%

---

# Adobe Experience Manager (AEM) - Modelos e arquétipos de governança e equipe

Como líder na experiência do cliente, o Adobe entende o quão desafiante pode ser para você garantir que tenha as pessoas certas e a estrutura de governança para aumentar a eficiência operacional. Com modelos de governança e funcionários comprovados pelo Adobe, você tem as ferramentas para criar uma base sólida de gerenciamento de conteúdo e ativos. Neste artigo, discutiremos maneiras de operacionalizar sua plataforma Adobe Experience Manager (AEM) e aproveitar ao máximo seus esforços.

## Criar um quadro operacional superior

Para poder executar e operar, AEM considere os seguintes elementos:

* Executar marcos estratégicos - Haverá muitos marcos estratégicos (personalização, integração de vários canais, etc.) isso não pode ser executado, a menos que você tenha o modelo de equipe correto em vigor.
* Crie uma base para a transformação digital - o AEM é usado com frequência como a primeira etapa do processo de modernização de uma organização. Definir uma base permite que você aproveite a AEM para sua capacidade total.
* Engajamento do usuário - Ter uma equipe em vigor para executar trabalho tático (atualizar workflows, permissões, CSS etc.) Quanto mais você tiver lacunas entre o que os usuários desejam e o que lhes é dado, mais frustrados eles podem se tornar. É importante manter os usuários investidos no sistema, investidos na solução e que você tenha o modelo operacional correto em vigor.

Qual é então o modelo correto? Qual é a matriz correta de funções a serem criadas?

Não há uma única resposta específica porque, assim como as organizações variam muito, uma configuração de AEM também pode variar bastante, resultando na necessidade de diferentes funções de suporte. Cada vertical, cada setor, cada estrutura de equipe exigirá uma implementação diferente. Mas é possível criar uma linha de base estabelecendo arquétipos.

## Arquétipos

Os arquétipos são ideias de função específicas e de alto nível que mapeiam para atributos específicos. Isso, por sua vez, pode ser usado para criar uma premissa fundamental que ajuda a informar qual modelo você realmente precisa. É importante observar que os arquétipos não estão limitados a uma pessoa por arquétipo. Por exemplo, um bibliotecário DAM poderia ter alguma experiência técnica.

### Fluxos de operacionalização

Há dois fluxos de operação para [!DNL AEM Sites] e [!DNL AEM Assets]:

1. Execução básica e operação diária do trabalho (atualização de metadados)

1. Estratégia e trabalho de transformação, como grandes projetos interorganizações

![fluxos de operacionalização](assets/streams-of-operationalization.png)

### Funções de ativos AEM de alto nível

**Intervalo geral:** Esta linha de base suporta modelos centralizados e descentralizados. Se você tem um modelo descentralizado, AEM pode ser usado de forma abstrata. Observe que a função Proprietário do produto deve ser usada de forma criativa, mas você também precisa ter um Proprietário do produto que seja proprietário dos diferentes estilos para um tipo de ativo e outro que supervisione a totalidade da organização.

1. Funções básicas de execução e operação

   * Recurso técnico - Alguém que tenha AEM experiência entende a permissão e pode atualizar o esquema de metadados
   * Gerente de versão
   * Proprietário do produto - Essa é uma função alinhada à solução. Alguns proprietários de produtos podem estar envolvidos no Analytics.
   * Bibliotecário do DAM - essa é alguém que pode ajudar a acompanhar os processos de estrutura integrativa. Essa função criativa pode se sobrepor a outras funções. (Observação: este é um papel que tem explodido em popularidade nos últimos cinco anos.)
   * Equipe de criação

1. Estratégia e transformação

   * Equipe de desenvolvimento - essa equipe é necessária ao se envolver com um marco estratégico importante.
   * Arquiteto de negócios - desenvolve requisitos para auxiliar em marcos técnicos e iniciativas estratégicas; pode ser compensado com um Proprietário de produto adicional
   * Arquiteto técnico - alguém que tem compreensão no nível da empresa e é uma presença constante em toda a organização. Essa função serve como o ponto central da verdade do DAM.

**Cenários de exemplo**

1. **Executar e operar:**

A seguir estão exemplos de funções para um cenário leve (empresa de artigos esportivos) e pesado (empresa de cosméticos):

1. Light - Funções da empresa de vestuário esportivo:

   * Desenvolvedores a tempo parcial - Hora parcial, offshore
   * 1 Proprietário do produto - Tempo completo, onshore
   * Bibliotecário DAM 1 - em tempo integral, em terra
   * 1 Arquiteto técnico - Tempo parcial, onshore
   * 1 Gerente de versão - Hora parcial, onshore

1. Pesada - Empresa de cosméticos (multimarcas)

   * 3 desenvolvedores em tempo integral - Em tempo integral, em offshore
   * 4 Proprietários do produto - 3 específico da marca, 1 principal
   * Bibliotecário DAM 1 - em tempo integral, em terra
   * 4 principais PME de administradores por marca
   * 1 Arquiteto técnico

### Alto nível [!DNL AEM Sites] funções

1. Execução básica e operação

   **Intervalo geral:** Os desenvolvedores de CSS criam novas capas para componentes. O consultor de negócios Adobe Sr, Joseph Van Buskirk, recomenda &quot;obter componentes desconectados e sistemas de estilos. Essa é a função que gera economia de custos. 80% das experiências criadas devem ser feitas usando componentes principais ou criados anteriormente.&quot; O objetivo é redefinir os componentes principais ou personalizados com novos estilos usando um desenvolvedor de CSS (ou uma equipe de desenvolvimento front-end) .

   Exemplos de função:

   * Desenvolvimento de CSS - cria artefatos de experiência, redefinindo componentes com novos estilos.
   * Desenvolvimento de back-end - cria novos componentes ou pode estender um componente principal. Se feito corretamente, essa função não deve ter mais de uma pessoa, a menos que haja necessidade de tarefas de animação grandes.
   * Gerenciamento de versões - supervisiona a implantação de código e serve como o engenheiro de sucesso do cliente atual.
   * Proprietário do produto - colabora com a BU para se casar com visões técnicas e estratégicas; O cria tarefas de manutenção e melhorias e atua como proprietário da solução.
   * Autores do administrador - atualiza a capa do CSS e fornece orientação aos autores que estão atualizando e aplicando conteúdo. Essa função funciona em configurações de fluxo de trabalho e cria uma documentação de orientação para os autores de conteúdo a serem aplicados. OBSERVAÇÃO: Na versão 6.5, o Adobe recomenda usar modelos editáveis.
   * Autores de conteúdo - aplica conteúdo, propriedade hierárquica e fornece problemas de comunicação e preocupações à medida que surgem com o CSM.

1. Estratégia e transformação

   Exemplos de função:

   * Equipe de desenvolvimento - fornece conhecimento AEM e executa novos marcos transformativos com o Arquiteto técnico.
   * Arquiteto técnico - fornece conhecimento de integração, trabalha com o proprietário do produto para mapear marcos técnicos e fornece profundo conhecimento técnico de AEM.
   * Arquiteto da empresa - cria tarefas para histórias de usuários e ajuda o proprietário do produto a gerenciar marcos técnicos e de negócios.

### Cenários de exemplo

A seguir estão exemplos de funções para um cenário de cliente leve e pesado:

1. Clara

   * 2 desenvolvedores de CSS - onshore
   * 1 Proprietário do produto - tempo inteiro, onshore
   * 1 Desenvolvedor de backend - off-shore
   * 1 Arquiteto técnico - em terra
   * 1 Gerente de versão - tempo parcial, onshore

1. Pesado (centrado em campanhas)

   * 4 desenvolvedores de CSS - tempo integral, onshore
   * 2 Desenvolvedores de back-end - tempo completo, onshore
   * 1 Arquiteto técnico - em terra
   * 1 Proprietário do produto
   * 2 arquitetos de negócios - offshore

### Takeaways de chave

**Entender os arquétipos** — Comece lento, entenda e analise os arquétipos. Seja criativo e flexível, tendo em mente que não há um modelo correto a ser seguido.

**Entenda seu roteiro** - Algumas organizações têm muitos marcos que desejam executar. Esteja preparado para alocar mais recursos técnicos do que você pode estimar.

**Aproveite os recursos internos** - as lacunas podem surgir inesperadamente. Você pode preenchê-los mais rapidamente, fornecendo membros internos da equipe, em vez de pesquisar fora da organização.

Para uma discussão mais aprofundada sobre Modelos e Arquétipos de Governança e Pessoal, assista a esta discussão do painel de uma hora: [Arquétipos de funções e Criação de um Quadro Operacional para [!DNL AEM Assets] e [!DNL Sites]](https://adobecustomersuccess.adobeconnect.com/p8ml5nmy0758mp4/)
