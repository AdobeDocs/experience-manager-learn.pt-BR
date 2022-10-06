---
title: Personalização usando AEM fragmentos de experiência e Adobe Target
seo-title: Personalization using Adobe Experience Manager (AEM) Experience Fragments and Adobe Target
description: Um tutorial completo mostrando como criar e fornecer experiência personalizada usando Fragmentos de experiência do Adobe Experience Manager e Adobe Target.
seo-description: An end-to-end tutorial showing how to create and deliver personalized experience using Adobe Experience Manager Experience Fragments and Adobe Target.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
exl-id: 47446e2a-73d1-44ba-b233-fa1b7f16bc76
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '1691'
ht-degree: 1%

---

# Personalização usando AEM fragmentos de experiência e Adobe Target

Com a capacidade de exportar AEM Fragmentos de experiência para o Adobe Target como ofertas do HTML, você pode combinar a facilidade de uso e o poder do AEM com os poderosos recursos de Inteligência automatizada (AI) e Aprendizagem de máquina (ML) no Target para testar e personalizar experiências em escala.

AEM reúne todo o conteúdo e ativos em um local central para alimentar sua estratégia de personalização. AEM permite criar conteúdo facilmente para desktops, tablets e dispositivos móveis em um único local, sem gravar código. Não há necessidade de criar páginas para cada dispositivo. AEM ajusta automaticamente cada experiência usando seu conteúdo.

O Target permite fornecer experiências personalizadas em escala com base em uma combinação de abordagens de aprendizado de máquina baseadas em regras e AI que incorporam variáveis comportamentais, contextuais e offline.  Com o Target, você pode configurar e executar facilmente atividades A/B e multivariadas (MVT) para determinar as melhores ofertas, conteúdo e experiências.

Fragmentos de experiência representam um grande passo em frente para vincular criadores de conteúdo a profissionais de marketing que estão gerando resultados comerciais usando o Target.

## Visão geral do cenário

O site WKND está planejando anunciar um **Desafio SkateFest** por toda a América através de seu site e gostaria que os usuários do site se inscrevessem para a audiência realizada em cada estado. Como comerciante, você recebeu a tarefa para executar uma campanha na página inicial do site WKND, com mensagens de banners relevantes para a localização dos usuários e um link para a página de detalhes do evento. Vamos explorar a página inicial do site WKND e saber como criar e fornecer uma experiência personalizada para um usuário com base na localização atual dele/dela.

### Usuários envolvidos

Para este exercício, os seguintes usuários precisam estar envolvidos e para executar algumas tarefas que você pode precisar de acesso administrativo.

* **Produtor de conteúdo/Editor de conteúdo** (Adobe Experience Manager)
* **Profissional de marketing** (Adobe Target / Equipe de otimização)

### Pré-requisitos

* **AEM**
   * [AEM criar e publicar instância](./implementation.md#getting-aem) em execução em localhost 4502 e 4503, respectivamente.
* **Experience Cloud**
   * Acesso à Adobe Experience Cloud de suas organizações - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud provisionado com as seguintes soluções
      * [Adobe Target](https://experiencecloud.adobe.com)

### Página inicial do site WKND

![AEM Cenário de Destino 1](assets/personalization-use-case-1/aem-target-use-case-1-4.png)

1. O profissional de marketing inicia a discussão da campanha WKND SkateFest com AEM Editor de conteúdo e detalha os requisitos.
   * ***Requisito***: Promova a campanha WKND SkateFest na página inicial do site WKND com conteúdo personalizado para visitantes de cada estado nos Estados Unidos. Adicione um novo bloco de conteúdo sob o carrossel da página inicial, contendo uma imagem do fundo, texto e um botão.
      * **Imagem de plano de fundo**: A imagem deve ser relevante para o estado a partir do qual o usuário está visitando a página do Site WKND.
      * **Texto**: &quot;Inscreva-se para receber os Audition&quot;
      * **Botão**: &quot;Detalhes do evento&quot; apontando para a página SkateFest da WKND
      * **Página SkateFest WKND**: uma nova página com detalhes do evento, incluindo o local do público-alvo, data e hora.
1. Com base nos requisitos, AEM Editor de conteúdo cria um Fragmento de experiência para o bloco de conteúdo e o exporta para o Adobe Target como uma oferta. Para fornecer conteúdo personalizado para todos os estados nos Estados Unidos, o autor de conteúdo pode criar uma variação principal do Fragmento de experiência e, em seguida, criar 50 outras variações, uma para cada estado. O conteúdo de cada variação de estado com imagens e texto relevantes pode ser editado manualmente. Ao criar um Fragmento de experiência, os editores de conteúdo podem acessar rapidamente todos os ativos disponíveis no AEM Assets usando a opção Localizador de ativos . Quando um Fragmento de experiência é exportado para o Adobe Target, todas as suas variações também são enviadas para o Adobe Target como Ofertas.

1. Após exportar o Fragmento de experiência do AEM para o Adobe Target como Ofertas, os profissionais de marketing podem criar atividades no Target usando essas Ofertas. Com base na campanha SkateFest do site WKND, o profissional de marketing precisa criar e entregar uma experiência personalizada para visitantes do site WKND de cada estado. Para criar uma atividade de Direcionamento de experiência, o profissional de marketing precisa identificar os públicos. Para nossa campanha WKND SkateFest, precisamos criar 50 públicos separados, com base em sua localização a partir da qual eles estão visitando o site da WKND.
   * [Públicos-alvo](https://experienceleague.adobe.com/docs/target/using/introduction/target-key-concepts.html#section_3F32DA46BDF947878DD79DBB97040D01) defina o target para sua atividade e seja usado em qualquer lugar onde o targeting estiver disponível. Os públicos-alvo do Target são um conjunto definido de critérios de visitante. As ofertas podem ser direcionadas a públicos-alvo específicos (ou segmentos). Somente os visitantes que pertencem a esse público-alvo visualizam a experiência direcionada a eles.  Por exemplo, é possível fornecer uma oferta a um público-alvo composto de visitantes que usam um determinado navegador ou de uma localização geográfica específica.
   * Um [Oferta](https://experienceleague.adobe.com/docs/target/using/introduction/target-key-concepts.html#section_973D4CC4CEB44711BBB9A21BF74B89E9) é o conteúdo que é exibido nas suas páginas da Web durante campanhas ou atividades. Ao testar suas páginas da Web, você mede o sucesso de cada experiência com diferentes ofertas em seus locais. Uma oferta pode conter diferentes tipos de conteúdo, incluindo:
      * Imagem
      * Texto
      * **HTML**
         * *As ofertas do HTML são usadas para a Atividade deste cenário*
      * Link
      * Botão

## Atividades do Editor de conteúdo

>[!VIDEO](https://video.tv.adobe.com/v/28596?quality=12&learn=on)

>[!NOTE]
>
>Publique o Fragmento de experiência antes de exportá-lo para o Adobe Target.

## Atividades do profissional de marketing

### Criar um público-alvo com geolocalização {#marketer-audience}

1. Navegue até suas organizações [Adobe Experience Cloud](https://experiencecloud.adobe.com/) (`<https://<yourcompany>.experiencecloud.adobe.com`)
1. Faça logon usando sua Adobe ID e verifique se você está na organização correta.
1. No alternador de soluções, clique em **Target** e depois **iniciar** Adobe Target.

   ![Experience Cloud - Adobe Target](assets/personalization-use-case-1/exp-cloud-adobe-target.png)

1. Navegue até o **Ofertas** e pesquise por ofertas &quot;WKND&quot;. Você pode ver a lista de variações dos Fragmentos de experiência, exportadas do AEM como Ofertas do HTML. Cada Oferta corresponde a um estado. Por exemplo, *WKND SkateFest Califórnia* é a oferta que é veiculada para um visitante do Site WKND da Califórnia.

   ![Experience Cloud - Adobe Target](assets/personalization-use-case-1/html-offers.png)

1. Na navegação principal, clique em **Públicos-alvo**.

   Um profissional de marketing precisa criar 50 públicos-alvo separados para visitantes do site WKND provenientes de cada estado nos Estados Unidos da América.

1. Para criar um público-alvo, clique em **Criar público-alvo** e forneça um nome para o público-alvo.

   **Formato do nome de público : WKND-\&lt;*state*\>**

   ![Experience Cloud - Adobe Target](assets/personalization-use-case-1/audience-target-1.png)

1. Clique em **Adicionar regra > Geografia**.
1. Clique em **Selecionar** e selecione uma das seguintes opções:
   * País
   * **Estado** *(Selecione o estado da campanha WKND Site SkateFest)*
   * Cidade
   * Código Postal
   * Latitude
   * Longitude
   * DMA
   * Operadora de celular

   **Geografia** - Use públicos para direcionar os usuários com base em sua localização geográfica, incluindo país, estado/província, cidade, código postal/CEP, DMA ou operadora de celular. Os parâmetros de geolocalização permitem direcionar atividades e experiências com base nas informações geográficas dos visitantes. Esses dados são enviados com cada solicitação do Target e baseiam-se no endereço IP do visitante. Selecione esses parâmetros exatamente como quaisquer outros valores de definição de metas.

   >[!NOTE]
   >O endereço IP de um visitante é enviado com uma solicitação de mbox, uma vez por visita (sessão), para resolver parâmetros de geolocalização para esse visitante.

1. Selecione o operador como **corresponde**, fornecer um valor adequado (por exemplo: Califórnia) e **Salvar** suas alterações. No nosso caso, forneça o nome do estado.

   ![Adobe Target - Regra geográfica](assets/personalization-use-case-1/audience-geo-rule.png)

   >[!NOTE]
   >Você pode ter várias regras atribuídas a um público-alvo.

1. Repita as etapas 6 a 9 para criar públicos-alvo para os outros estados.

   ![Adobe Target - Públicos-alvo WKND](assets/personalization-use-case-1/adobe-target-audiences-50.png)

Neste ponto, criamos públicos-alvo com sucesso para todos os visitantes do Site WKND em diferentes estados nos Estados Unidos da América e também temos a oferta de HTML correspondente para cada estado. Agora vamos criar uma atividade de Direcionamento de experiência para direcionar o público-alvo com uma oferta correspondente para a página inicial do site da WKND.

### Criar uma atividade com geolocalização

1. Na janela do Adobe Target, navegue até **Atividades** guia .
1. Clique em **Criar atividade** e selecione o **Direcionamento de experiência** tipo de atividade.
1. Selecione o **Web** e escolha o **Visual Experience Composer**.
1. Insira o **URL da atividade** e clique em **Próximo** para abrir o Visual Experience Composer.

   URL de publicação da página inicial do site WKND: http://localhost:4503/content/wknd/en.html

   ![Atividade de Direcionamento de experiência](assets/personalization-use-case-1/target-activity.png)

1. Para **Visual Experience Composer** para carregar, habilite **Permitir Carregar Scripts Não Seguros** no navegador e recarregue a página.

   ![Atividade de Direcionamento de experiência](assets/personalization-use-case-1/load-unsafe-scripts.png)

1. Observe a página inicial do Site WKND aberta no editor do Visual Experience Composer.

   ![VEC](assets/personalization-use-case-1/vec.png)

1. Para adicionar um público ao seu VEC, clique em **Adicionar direcionamento de experiência** em Públicos-alvo, selecione o público-alvo WKND-Califórnia e clique em **Próximo**.

   ![VEC](assets/personalization-use-case-1/vec-select-audience.png)

1. Clique na página do site WKND no VEC, selecione o elemento HTML para adicionar a oferta para o público-alvo WKND-Califórnia e escolha **Substituir por** e selecione a **HTML Offer**.

   ![Atividade de Direcionamento de experiência](assets/personalization-use-case-1/vec-selecting-div.png)

1. Selecione o **WKND SkateFest Califórnia** HTML para a oferta da **WKND-Califórnia** público-alvo na interface de usuário selecionada da oferta e clique em **Concluído**.
1. Agora você deve ser capaz de ver a variável **WKND SkateFest Califórnia** Oferta de HTML adicionada à página do site WKND para o público-alvo WKND-Califórnia.
1. Repita as etapas 7 a 10 para adicionar o Direcionamento de experiência aos outros estados e escolha a Oferta do HTML correspondente.
1. Clique em **Próximo** para continuar, e você pode ver um mapeamento para Públicos-alvo para experiências.
1. Clique em **Próximo** para mover para Metas e configurações.
1. Escolha sua fonte de relatórios e identifique uma meta principal para a atividade. Para nosso Cenário, vamos selecionar a Fonte de relatórios como **Adobe Target**, a atividade de medição como **Conversão**, ação como visualizada em uma página e URL apontando para a página Detalhes do SkateFest da WKND.

   ![Meta e direcionamento - Target](assets/personalization-use-case-1/goal-metric-target.png)

   >[!NOTE]
   >Você também pode escolher o Adobe Analytics como sua fonte de relatórios.

1. Passe o mouse sobre o nome atual da atividade e você pode renomeá-la para **WKND SkateFest - EUA** e depois **Salvar e fechar** suas alterações.
1. Na tela Detalhes da atividade , certifique-se de **Ativar** sua atividade.

   ![Ativar atividade](assets/personalization-use-case-1/activate-activity.png)

1. Sua Campanha WKND SkateFest agora está ao vivo para todos os visitantes do site WKND.
1. Navegue até o [Página inicial do site WKND](http://localhost:4503/content/wknd/en.html)e você deve ser capaz de ver a Oferta SkateFest WKND com base em sua geolocalização (*estado: Califórnia*).

   ![Controle de qualidade da atividade](assets/personalization-use-case-1/wknd-california.png)

### Controle de qualidade da atividade do Target

1. Em **Detalhes da atividade > Visão geral** clique no botão **Controle de qualidade da atividade** e você pode obter o link de controle de qualidade direto para todas as suas experiências.

   ![Controle de qualidade da atividade](assets/personalization-use-case-1/activity-qa.png)

1. Navegue até o [Página inicial do site WKND](http://localhost:4503/content/wknd/en.html)e você deve ser capaz de ver a Oferta SkateFest WKND com base em sua localização geográfica (estado).
1. Assista ao vídeo abaixo para entender como uma oferta é entregue em sua página, como personalizar tokens de resposta e executar uma verificação de qualidade.

>[!VIDEO](https://video.tv.adobe.com/v/28658?quality=12&learn=on)

## Resumo

Neste capítulo, um editor de conteúdo foi capaz de criar todo o conteúdo para suportar a campanha WKND SkateFest no Adobe Experience Manager e exportá-la para o Adobe Target como HTML Offers, para criar o Direcionamento de experiência, com base na geolocalização dos usuários.
