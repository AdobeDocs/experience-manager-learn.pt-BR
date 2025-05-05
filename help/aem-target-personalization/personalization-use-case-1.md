---
title: Personalization usando fragmentos de experiência do AEM e o Adobe Target
description: Um tutorial completo mostrando como criar e fornecer experiência personalizada usando fragmentos de experiência do Adobe Experience Manager e do Adobe Target.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="Integração" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 47446e2a-73d1-44ba-b233-fa1b7f16bc76
duration: 1088
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1663'
ht-degree: 0%

---

# Personalization usando fragmentos de experiência do AEM e o Adobe Target

Com a capacidade de exportar fragmentos de experiência do AEM para o Adobe Target como ofertas do HTML, você pode combinar a facilidade de uso e o poder do AEM com os poderosos recursos de Inteligência automatizada (AI) e Aprendizado de máquina (ML) no Target para testar e personalizar experiências em escala.

O AEM reúne todo o seu conteúdo e ativos em um local central para alimentar sua estratégia de personalização. O AEM permite que você crie conteúdo facilmente para desktops, tablets e dispositivos móveis em um único local sem escrever código. Não há necessidade de criar páginas para cada dispositivo. O AEM ajusta automaticamente cada experiência usando seu conteúdo.

O Target permite entregar experiências personalizadas em escala com base em uma combinação de abordagens de aprendizagem de máquina baseadas em regras e AI que incorporam variáveis comportamentais, contextuais e offline.  Com o Target, você pode configurar e executar facilmente atividades A/B e multivariadas (MVT) para determinar as melhores ofertas, experiências e conteúdo.

Os fragmentos de experiência representam um enorme passo à frente para vincular criadores de conteúdo a profissionais de marketing que impulsionam os resultados de negócios usando o Target.

## Visão geral do cenário

O site da WKND está planejando anunciar um **Desafio SkateFest** em todos os Estados Unidos através de seu site e gostaria que seus usuários do site se inscrevessem para o teste realizado em cada estado. Como profissional de marketing, você recebeu a tarefa para executar uma campanha na página inicial do site WKND, com mensagens de banners relevantes ao local dos usuários e um link para a página de detalhes do evento. Vamos explorar a página inicial do site WKND e saber como criar e fornecer uma experiência personalizada para um usuário com base em sua localização atual.

### Usuários envolvidos

Para este exercício, os seguintes usuários precisam estar envolvidos e, para executar algumas tarefas, você pode precisar de acesso administrativo.

* **Produtor de conteúdo/Editor de conteúdo** (Adobe Experience Manager)
* **Profissional de marketing** (Adobe Target/Equipe de otimização)

### Pré-requisitos

* **AEM**
   * [Instância de autoria e publicação do AEM](./implementation.md#getting-aem) em execução em localhost 4502 e 4503, respectivamente.
* **Experience Cloud**
   * Acesso à Adobe Experience Cloud de suas organizações - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud provisionado com as seguintes soluções
      * [Adobe Target](https://experiencecloud.adobe.com)

### Página inicial do site WKND

![Cenário de Destino do AEM 1](assets/personalization-use-case-1/aem-target-use-case-1-4.png)

1. O profissional de marketing inicia a discussão da campanha WKND SkateFest com o editor de conteúdo AEM e detalha os requisitos.
   * ***Requisito***: Promover a campanha WKND SkateFest na página inicial do site WKND com conteúdo personalizado para visitantes de cada estado nos Estados Unidos. Adicione um novo bloco de conteúdo abaixo do Carrossel da página inicial com uma imagem do plano de fundo, texto e um botão.
      * **Imagem de Plano de Fundo**: a imagem deve ser relevante para o estado do qual o usuário está visitando a página do Site WKND.
      * **Texto**: &quot;Inscrever-se para os Audition&quot;
      * **Botão**: &quot;Detalhes do evento&quot; apontando para a página WKND SkateFest
      * **Página do WKND SkateFest**: uma nova página com detalhes do evento, incluindo o local do teste, a data e a hora.
1. Com base nos requisitos, o Editor de conteúdo AEM cria um Fragmento de experiência para o bloco de conteúdo e o exporta para o Adobe Target como uma oferta. Para veicular conteúdo personalizado para todos os estados nos Estados Unidos, o autor de conteúdo pode criar uma variação mestre do Fragmento de experiência e, em seguida, criar 50 outras variações, uma para cada estado. O conteúdo de cada variação de estado com imagens e texto relevantes pode ser editado manualmente. Ao criar um fragmento de experiência, os editores de conteúdo podem acessar rapidamente todos os ativos disponíveis no AEM Assets usando a opção Localizador de ativos. Quando um fragmento de experiência é exportado para o Adobe Target, todas as variações também são enviadas para o Adobe Target como ofertas.

1. Depois de exportar o fragmento de experiência do AEM para o Adobe Target como ofertas, os profissionais de marketing podem criar atividades no Target usando essas ofertas. Com base na campanha SkateFest do site WKND, o profissional de marketing precisa criar e fornecer uma experiência personalizada para visitantes do site WKND de cada estado. Para criar uma atividade de Direcionamento de experiência, o profissional de marketing precisa identificar os públicos. Para nossa campanha WKND SkateFest, precisamos criar 50 públicos-alvo separados, com base na localização deles na qual eles estão visitando o site da WKND.
   * [Os públicos-alvo](https://experienceleague.adobe.com/docs/target/using/introduction/target-key-concepts.html?lang=pt-BR#section_3F32DA46BDF947878DD79DBB97040D01) definem o público-alvo para a sua atividade e são usados em qualquer lugar onde o direcionamento estiver disponível. Os públicos-alvo são um conjunto definido de critérios de visitante. As ofertas podem ser direcionadas a públicos (ou segmentos) específicos. Somente os visitantes que pertencem a esse público veem a experiência direcionada a eles.  Por exemplo, você pode fornecer uma oferta a um público-alvo composto por visitantes que usam um determinado navegador ou de uma localização geográfica específica.
   * Uma [Oferta](https://experienceleague.adobe.com/docs/target/using/introduction/target-key-concepts.html?lang=pt-BR#section_973D4CC4CEB44711BBB9A21BF74B89E9) é o conteúdo que é exibido nas suas páginas da Web durante campanhas ou atividades. Ao testar as páginas da Web, você mede o sucesso de cada experiência com diferentes ofertas nos locais. Uma oferta pode conter diferentes tipos de conteúdo, incluindo:
      * Imagem
      * Texto
      * **HTML**
         * *Ofertas HTML são usadas para a Atividade deste cenário*
      * Link
      * Botão

## Atividades do Editor de conteúdo

>[!VIDEO](https://video.tv.adobe.com/v/28596?quality=12&learn=on)

>[!NOTE]
>
>Publish o fragmento de experiência antes de exportá-lo para o Adobe Target.

## Atividades do profissional de marketing

### Criar um público-alvo com geolocalização {#marketer-audience}

1. Navegue até sua organização [Adobe Experience Cloud](https://experiencecloud.adobe.com/) (`<https://<yourcompany>.experiencecloud.adobe.com`)
1. Faça logon usando sua Adobe ID e verifique se você está na organização correta.
1. No alternador de soluções, clique em **Target** e em **launch** Adobe Target.

   ![Experience Cloud- Adobe Target](assets/personalization-use-case-1/exp-cloud-adobe-target.png)

1. Navegue até a guia **Ofertas** e procure ofertas &quot;WKND&quot;. Você pode ver a lista de variações de Fragmentos de experiência, exportada do AEM como Ofertas de HTML. Cada oferta corresponde a um estado. Por exemplo, *WKND SkateFest Califórnia* é a oferta que é apresentada a um visitante do site WKND da Califórnia.

   ![Experience Cloud- Adobe Target](assets/personalization-use-case-1/html-offers.png)

1. Na navegação principal, clique em **Audiences**.

   Um profissional de marketing precisa criar 50 públicos-alvo separados para os visitantes do site da WKND que vêm de cada estado nos Estados Unidos da América.

1. Para criar um público-alvo, clique no botão **Criar público-alvo** e forneça um nome para o seu público-alvo.

   **Formato do nome do público-alvo: WKND-\&lt;*state*\>**

   ![Experience Cloud- Adobe Target](assets/personalization-use-case-1/audience-target-1.png)

1. Clique em **Adicionar regra > Geografia**.
1. Clique em **Selecionar** e selecione uma das seguintes opções:
   * País
   * **Estado** *(Selecionar Estado para Campanha SkateFest do Site WKND)*
   * Cidade
   * Código postal
   * Latitude
   * Longitude
   * DMA
   * Operadora de celular

   **Geo** - Use públicos para direcionar usuários com base em sua localização geográfica, incluindo país, estado/província, cidade, código postal/CEP, DMA ou operadora de celular. Os parâmetros de geolocalização permitem definir atividades e experiências com base na localização geográfica de seus visitantes. Esses dados são enviados com cada solicitação do Target e baseiam-se no endereço IP do visitante. Selecione esses parâmetros exatamente como quaisquer outros valores de definição de metas.

   >[!NOTE]
   >O endereço IP de um visitante é transmitido com uma solicitação de mbox, uma vez por visita (sessão), para resolver parâmetros de direcionamento geográfico para esse visitante.

1. Selecione o operador como **corresponde**, forneça um valor apropriado (por exemplo: Califórnia) e **Salve** suas alterações. No nosso caso, forneça o nome do estado.

   ![Adobe Target- Regra geográfica](assets/personalization-use-case-1/audience-geo-rule.png)

   >[!NOTE]
   >É possível ter várias regras atribuídas a um público-alvo.

1. Repita as etapas 6 a 9 para criar públicos-alvo para os outros estados.

   ![Adobe Target- Públicos-alvo da WKND](assets/personalization-use-case-1/adobe-target-audiences-50.png)

Neste ponto, criamos públicos-alvo com sucesso para todos os visitantes do Site WKND em diferentes estados nos Estados Unidos da América e também temos a oferta HTML correspondente para cada estado. Agora vamos criar uma atividade de Direcionamento de experiência para direcionar o público-alvo com uma oferta correspondente para a página inicial do site WKND.

### Criar uma atividade com geolocalização

1. Na janela do Adobe Target, navegue até a guia **Atividades**.
1. Clique em **Criar atividade** e selecione o tipo de atividade **Direcionamento de experiência**.
1. Selecione o canal **Web** e escolha o **Visual Experience Composer**.
1. Insira o **URL da atividade** e clique em **Avançar** para abrir o Visual Experience Composer.

   URL do Publish da página inicial do site da WKND: http://localhost:4503/content/wknd/en.html

   ![Atividade de Direcionamento de Experiência](assets/personalization-use-case-1/target-activity.png)

1. Para o **Visual Experience Composer** carregar, habilite o **Permitir carregamento de scripts não seguros** no navegador e recarregue a página.

   ![Atividade de Direcionamento de Experiência](assets/personalization-use-case-1/load-unsafe-scripts.png)

1. Observe que a página inicial do site WKND é aberta no editor do Visual Experience Composer.

   ![VEC](assets/personalization-use-case-1/vec.png)

1. Para adicionar um público ao VEC, clique em **Adicionar direcionamento de experiência** em Públicos-alvo, selecione o público-alvo da WKND-Califórnia e clique em **Avançar**.

   ![VEC](assets/personalization-use-case-1/vec-select-audience.png)

1. Clique na página do site da WKND no VEC, selecione o elemento HTML para adicionar a oferta ao público-alvo da WKND-Califórnia, escolha a opção **Substituir por** e selecione a **Oferta de HTML**.

   ![Atividade de Direcionamento de Experiência](assets/personalization-use-case-1/vec-selecting-div.png)

1. Selecione a oferta de HTML **WKND SkateFest Califórnia** para o público-alvo **WKND-Califórnia** na interface de seleção da oferta e clique em **Concluído**.
1. Agora é possível ver a HTML Oferta **WKND SkateFest California** adicionada à página do Site WKND para o público-alvo da WKND-California.
1. Repita as etapas 7 a 10 para adicionar o Direcionamento de experiência para os outros estados e escolha a Oferta de HTML correspondente.
1. Clique em **Avançar** para continuar e você poderá ver um mapeamento de Públicos-alvo para Experiências.
1. Clique em **Avançar** para ir para Metas e Configurações.
1. Escolha sua fonte de relatórios e identifique uma meta principal para sua atividade. Para nosso Cenário, vamos selecionar o Source de relatórios como **Adobe Target**, medindo a atividade como **Conversão**, a ação como uma página visualizada e a URL que aponta para a página Detalhes do SkateFest do WKND.

   ![Meta e direcionamento - Target](assets/personalization-use-case-1/goal-metric-target.png)

   >[!NOTE]
   >Você também pode escolher o Adobe Analytics como sua fonte de geração de relatórios.

1. Passe o mouse sobre o nome da atividade atual, e você pode renomeá-la para **WKND SkateFest - USA**, e depois **Salvar e Fechar** suas alterações.
1. Na tela Detalhes da atividade, **Ative** sua atividade.

   ![Ativar Atividade](assets/personalization-use-case-1/activate-activity.png)

1. Sua campanha WKND SkateFest agora está disponível para todos os visitantes do site WKND.
1. Navegue até a [Página Inicial do Site WKND](http://localhost:4503/content/wknd/en.html) e você poderá ver a Oferta do WKND SkateFest com base na sua localização geográfica (*estado: Califórnia*).

   ![Controle de qualidade da atividade](assets/personalization-use-case-1/wknd-california.png)

### Controle de qualidade da atividade do Target

1. Na guia **Detalhes da atividade > Visão geral**, clique no botão **Controle de qualidade da atividade** e você poderá obter o link de controle de qualidade direto para todas as suas experiências.

   ![Controle de qualidade da atividade](assets/personalization-use-case-1/activity-qa.png)

1. Navegue até a [Página inicial do site do WKND](http://localhost:4503/content/wknd/en.html) e você poderá ver a oferta do WKND SkateFest com base na sua localização geográfica (estado).
1. Assista ao vídeo abaixo para entender como uma oferta é entregue à sua página, como personalizar tokens de resposta e executar uma verificação de qualidade.

>[!VIDEO](https://video.tv.adobe.com/v/28658?quality=12&learn=on)

## Resumo

Neste capítulo, um editor de conteúdo conseguiu criar todo o conteúdo para suportar a campanha WKND SkateFest no Adobe Experience Manager e exportá-lo para o Adobe Target como Ofertas HTML, para criar o Direcionamento de experiência, com base na localização geográfica dos usuários.
