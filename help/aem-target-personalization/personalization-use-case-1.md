---
title: Personalização usando AEM Fragmentos de experiência e Adobe Target
seo-title: Personalização usando fragmentos de experiência do Adobe Experience Manager (AEM) e Adobe Target
description: Um tutorial completo mostrando como criar e fornecer experiência personalizada usando os Adobe Experience Manager Experience Fragments e o Adobe Target.
seo-description: Um tutorial completo mostrando como criar e fornecer experiência personalizada usando os Adobe Experience Manager Experience Fragments e o Adobe Target.
translation-type: tm+mt
source-git-commit: 1209064fd81238d4611369b8e5b517365fc302e3
workflow-type: tm+mt
source-wordcount: '1729'
ht-degree: 1%

---


# Personalização usando AEM Fragmentos de experiência e Adobe Target

Com a capacidade de exportar AEM Fragmentos de experiência para o Adobe Target como ofertas HTML, você pode combinar a facilidade de uso e o poder do AEM com os poderosos recursos de Inteligência automatizada (AI) e Aprendizagem de máquina (ML) em Público alvo para testar e personalizar experiências em escala.

AEM reúne todo o seu conteúdo e ativos em um local central para alimentar sua estratégia de personalização. AEM permite que você crie facilmente conteúdo para desktops, tablets e dispositivos móveis em um único local, sem gravar código. Não há necessidade de criar páginas para cada dispositivo. AEM ajusta automaticamente cada experiência usando seu conteúdo.

O Público alvo permite que você forneça experiências personalizadas em escala com base em uma combinação de abordagens de aprendizado de máquina baseadas em regras e orientadas por IA que incorporam variáveis comportamentais, contextuais e offline.  Com o Público alvo, você pode facilmente configurar e executar atividades A/B e multivariadas (MVT) para determinar as melhores ofertas, conteúdo e experiências.

Os fragmentos de experiência representam um enorme passo em frente para vincular os criadores de conteúdo aos profissionais de marketing que estão gerando resultados comerciais usando o Público alvo.

## Visão geral do cenário

O site da WKND está planejando anunciar um Desafio **SkateFest** em toda a América através de seu site e gostaria que os usuários do site se inscrevessem para o teste em cada estado. Como comerciante, você recebeu a tarefa para executar uma campanha no home page do site da WKND, com mensagens de banners relevantes para o local dos usuários e um link para a página de detalhes do evento. Vamos explorar o home page do site da WKND e aprender a criar e fornecer uma experiência personalizada para um usuário com base em sua localização atual.

### Usuários envolvidos

Para este exercício, os usuários a seguir precisam estar envolvidos e para executar algumas tarefas, você pode precisar de acesso administrativo.

* **Content Producer / Editor** de conteúdo (Adobe Experience Manager)
* **Comerciante** (Adobe Target / Equipe de otimização)

### Pré-requisitos

* **AEM**
   * [AEM criação e publicação de instância](./implementation.md#getting-aem) em execução no localhost 4502 e 4503, respectivamente.
* **Experience Cloud**
   * Acesso à Adobe Experience Cloud de suas organizações - <https://>`<yourcompany>`.experience.ecloud.adobe.com
   * Experience Cloud fornecido com as seguintes soluções
      * [Adobe Target](https://experiencecloud.adobe.com)

### Home page do site WKND

![Cenário de Público alvo AEM 1](assets/personalization-use-case-1/aem-target-use-case-1-4.png)

1. O Marketer inicia a discussão da campanha WKND SkateFest com AEM Editor de conteúdo e detalha os requisitos.
   * ***Requisito***: Promova a campanha WKND SkateFest no home page do site WKND com conteúdo personalizado para visitantes de cada estado nos Estados Unidos. Adicione um novo bloco de conteúdo abaixo do carrossel do Home page que contém uma imagem de plano de fundo, um texto e um botão.
      * **Imagem** de plano de fundo: A imagem deve ser relevante para o estado a partir do qual o usuário está visitando a página do site da WKND.
      * **Texto**: &quot;Inscreva-se para os Audition&quot;
      * **Botão**: &quot;Detalhes do Evento&quot; apontando para a página WKND SkateFest
      * **Página** WKND SkateFest: uma nova página com detalhes do evento, incluindo o local do teste, a data e a hora.
2. Com base nos requisitos, AEM Editor de conteúdo cria um Fragmento de experiência para o bloco de conteúdo e o exporta para a Adobe Target como uma Oferta. Para fornecer conteúdo personalizado para todos os estados nos Estados Unidos, o autor de conteúdo pode criar uma variação principal do Fragmento de experiência e, em seguida, criar 50 outras variações, uma para cada estado. O conteúdo de cada variação de estado com imagens e texto relevantes pode ser editado manualmente. Ao criar um Fragmento de experiência, os editores de conteúdo podem acessar rapidamente todos os ativos disponíveis no AEM Assets usando a opção Localizador de ativos. Quando um Fragmento de experiência é exportado para a Adobe Target, todas as suas variações também são enviadas para a Adobe Target como Oferta.

3. Após exportar o Fragmento de experiência de AEM para Adobe Target como Oferta, os profissionais de marketing podem criar Atividades no Público alvo usando essas Ofertas. Com base na campanha SkateFest do site da WKND, o profissional de marketing precisa criar e fornecer uma experiência personalizada para visitantes do site da WKND de cada estado. Para criar uma atividade de direcionamento de experiência, o comerciante precisa identificar as audiências. Para nossa campanha WKND SkateFest, precisamos criar 50 audiências separadas, com base em sua localização a partir da qual elas estão visitando o site da WKND.
   * [As audiências](https://docs.adobe.com/content/help/en/target/using/introduction/target-key-concepts.html#section_3F32DA46BDF947878DD79DBB97040D01) definem o público alvo para a sua atividade e são usadas em qualquer lugar onde a definição de metas está disponível. Audiências públicos alvos são um conjunto definido de critérios de visitante. Ofertas podem ser direcionadas a audiências específicas (ou segmentos). Somente visitantes que pertencem a essa audiência veem a experiência direcionada a eles.  Por exemplo, você pode fornecer uma oferta para uma audiência composta de visitantes que usam um navegador específico ou de uma localização geográfica específica.
   * Uma [Oferta](https://docs.adobe.com/content/help/en/target/using/introduction/target-key-concepts.html#section_973D4CC4CEB44711BBB9A21BF74B89E9) é o conteúdo que é exibido em suas páginas da Web durante o campanha ou o atividade. Ao testar suas páginas da Web, você mede o sucesso de cada experiência com diferentes ofertas em seus locais. Uma Oferta pode conter diferentes tipos de conteúdo, incluindo:
      * Imagem
      * Texto
      * **HTML**
         * *OFERTAS HTML serão usadas para a Atividade deste cenário*
      * Link
      * Botão

## Atividades do Editor de conteúdo

>[!VIDEO](https://video.tv.adobe.com/v/28596?quality=12&learn=on)

>[!NOTE]
>
>Publique o Fragmento de experiência antes de exportá-lo para a Adobe Target.

## Atividades do comerciante

### Criar uma Audiência com GeoTargeting {#marketer-audience}

1. Navegue até a sua empresa [Adobe Experience Cloud](https://experiencecloud.adobe.com/) (<https://>`<yourcompany>`.experience.ecloud.adobe.com)
2. Faça logon usando seu Adobe ID e verifique se você está na organização correta.
3. No alternador de soluções, clique no **Público alvo** e **inicie** o Adobe Target.

   ![Experience Cloud - Adobe Target](assets/personalization-use-case-1/exp-cloud-adobe-target.png)

4. Navegue até a guia **Oferta** e pesquise por ofertas &quot;WKND&quot;. É possível visualizar a lista de variações de Fragmentos de experiência, exportadas de AEM como Ofertas HTML. Cada Oferta corresponde a um estado. Por exemplo, *WKND SkateFest California* é a oferta que é servida para um visitante WKND Site da Califórnia.

   ![Experience Cloud - Adobe Target](assets/personalization-use-case-1/html-offers.png)

5. Na navegação principal, clique em **Audiência**.

   Um profissional de marketing precisa criar 50 audiências separadas para visitantes de site WKND provenientes de cada estado nos Estados Unidos da América.

6. Para criar uma audiência, clique no botão **Criar Audiência** e forneça um nome para a audiência.

   **Formato do nome da audiência: WKND-\&lt;*estado*\>**

   ![Experience Cloud - Adobe Target](assets/personalization-use-case-1/audience-target-1.png)

7. Clique em **Adicionar regra > Geo**.
8. Clique em **Selecionar** e selecione uma das seguintes opções:
   * País
   * **Estado** *(selecione Estado para a Campanha SkateFest do site WKND)*
   * Cidade
   * CEP
   * Latitude
   * Longitude
   * DMA
   * Operadora de celular

   **Geo** - Use audiências para usuários públicos alvos com base em sua localização geográfica, incluindo seu país, estado/província, cidade, código postal/CEP, DMA ou operadora de celular. Os parâmetros de geolocalização permitem público alvo de atividades e experiências com base na geografia dos visitantes. Esses dados são enviados com cada solicitação de Público alvo e são baseados no endereço IP do visitante. Selecione esses parâmetros da mesma forma que qualquer valor de definição de metas.

   >[!NOTE]
   >Um endereço IP do visitante é enviado com uma solicitação de mbox, uma vez por visita (sessão), para resolver os parâmetros de geolocalização do visitante.

9. Selecione o operador como **correspondências**, forneça um valor apropriado (por exemplo: Califórnia) e **Salve** suas alterações. Em nosso caso, forneça o nome do estado.

   ![Adobe Target - Regra geográfica](assets/personalization-use-case-1/audience-geo-rule.png)

   >[!NOTE]
   >É possível ter várias regras atribuídas a uma audiência.

10. Repita as etapas de 6 a 9 para criar audiências para os outros estados.

   ![Adobe Target - Audiências WKND](assets/personalization-use-case-1/adobe-target-audiences-50.png)

Nesse ponto, criamos com êxito audiências para todos os visitantes do site WKND em diferentes estados nos Estados Unidos da América e também temos a oferta HTML correspondente para cada estado. Agora vamos criar uma atividade de direcionamento de experiência para público alvo da audiência com uma oferta correspondente para o Home page do site da WKND.

### Criar uma Atividade com GeoTargeting

1. Na janela do Adobe Target, navegue até a guia **Atividade** .
2. Clique em **Criar Atividade** e selecione o tipo de atividade de direcionamento de **experiência** .
3. Selecione o canal **Web** e escolha o **Visual Experience Composer**.
4. Insira o URL **da** Atividade e clique em **Avançar** para abrir o Visual Experience Composer.

   URL de publicação do Home page do site WKND: http://localhost:4503/content/wknd/en.html
   ![Atividade de direcionamento de experiência](assets/personalization-use-case-1/target-activity.png)
5. Para que o **Visual Experience Composer** carregue, ative **Permitir carregamento de scripts** não seguros no seu navegador e recarregue sua página.
   ![Atividade de direcionamento de experiência](assets/personalization-use-case-1/load-unsafe-scripts.png)
6. Observe que o home page do site WKND é aberto no editor do Visual Experience Composer.
   ![VEC](assets/personalization-use-case-1/vec.png)
7. Para adicionar uma audiência ao seu VEC, clique em **Adicionar direcionamento** de experiência em Audiência e selecione a audiência WKND-California e clique em **Avançar**.
   ![VEC](assets/personalization-use-case-1/vec-select-audience.png)
8. Clique na página do site WKND no VEC, selecione o elemento HTML para adicionar a oferta para a audiência WKND-California, escolha **Substituir por** e selecione a Oferta ****HTML.
   ![Atividade de direcionamento de experiência](assets/personalization-use-case-1/vec-selecting-div.png)
9. Selecione a oferta HTML **WKND SkateFest California** para a audiência **WKND-California** na oferta selecione UI e clique em **Concluído**.
10. Agora você pode ver a Oferta HTML **WKND SkateFest California** adicionada à sua página do Site WKND para a audiência WKND-California.
11. Repita as etapas de 7 a 10 para adicionar o direcionamento de experiência para os outros estados e escolha a Oferta HTML correspondente.
12. Clique em **Avançar** para continuar e você poderá ver um mapeamento de Audiências para experiências.
13. Clique em **Avançar** para ir para Metas e configurações.
14. Escolha sua fonte de relatórios e identifique uma meta principal para sua atividade. Para nosso Cenário, vamos selecionar a Fonte do Relatórios como **Adobe Target**, medindo a atividade como **Conversão**, a ação como visualizada em uma página e o URL apontando para a página Detalhes do SkateFest WKND.
   ![Meta e segmentação - Público alvo](assets/personalization-use-case-1/goal-metric-target.png)

   >[!NOTE]
   >Você também pode escolher Adobe Analytics como sua fonte de relatórios.

15. Passe o mouse sobre o nome da atividade atual e renomeie-o para **WKND SkateFest - EUA** e, em seguida, **Salve e feche** suas alterações.
16. Na tela de detalhes da Atividade, certifique-se de **Ativar** sua atividade.
   ![Ativar Atividade](assets/personalization-use-case-1/activate-activity.png)
17. Sua Campanha WKND SkateFest agora está ao vivo para todos os visitantes do site WKND.
18. Navegue até o Home page [do site](http://localhost:4503/content/wknd/en.html)WKND e você deverá ser capaz de ver a Oferta WKND SkateFest com base na sua localização geográfica (*estado: Califórnia*).
   ![QA da atividade](assets/personalization-use-case-1/wknd-california.png)

### QA da Atividade do público alvo

1. Em Detalhes da **Atividade > guia Visão geral** , clique no botão de QA **da** Atividade e você pode obter o link de QA direto para todas as suas experiências.
   ![QA da atividade](assets/personalization-use-case-1/activity-qa.png)
2. Navegue até o Home page [do site](http://localhost:4503/content/wknd/en.html)WKND e você deverá ser capaz de ver a Oferta WKND SkateFest com base em sua localização geográfica (estado).
3. Assista ao vídeo abaixo para entender como uma oferta é entregue à sua página, como personalizar tokens de resposta e executar uma verificação de qualidade.

>[!VIDEO](https://video.tv.adobe.com/v/28658?quality=12&learn=on)

## Resumo

Neste capítulo, um editor de conteúdo foi capaz de criar todo o conteúdo para suportar a campanha WKND SkateFest no Adobe Experience Manager e exportá-la para o Adobe Target como Ofertas HTML, para criar o direcionamento de experiência, com base na localização geográfica dos usuários.
