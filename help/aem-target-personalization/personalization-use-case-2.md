---
title: Personalização usando o Adobe Target
seo-title: Personalization using Adobe Target
description: Um tutorial completo mostrando como criar e fornecer experiência personalizada usando o Adobe Target.
seo-description: An end-to-end tutorial showing how to create and deliver personalized experience using Adobe Target.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
source-git-commit: ea7d49985e69ecf9713e17e51587125b3fb400ee
workflow-type: tm+mt
source-wordcount: '587'
ht-degree: 2%

---


# Personalização de experiências completas de página da Web usando o Adobe Target

No capítulo anterior, aprendemos a criar uma atividade com base em localização geográfica no Adobe Target usando conteúdo criado como Fragmentos de experiência e exportado do AEM como Ofertas HTML.

Neste capítulo, exploraremos a criação de atividades para redirecionar as páginas do site hospedadas em AEM para uma nova página usando o Adobe Target.

## Visão geral do cenário

O site WKND reprojetou sua página inicial e gostaria de redirecionar seus visitantes atuais da página inicial para a nova página inicial. Ao mesmo tempo, também compreenda como a página inicial reprojetada ajuda a melhorar a participação do usuário e a receita. Como comerciante, você recebeu a tarefa de criar uma atividade para redirecionar os visitantes para a nova página inicial. Vamos explorar a página inicial do site WKND e saber como criar uma atividade usando o Adobe Target.

### Usuários envolvidos

Para este exercício, os seguintes usuários precisam estar envolvidos e para executar algumas tarefas que você pode precisar de acesso administrativo.

* **Produtor de conteúdo/Editor de conteúdo**  (Adobe Experience Manager)
* **Profissional de marketing**  (Adobe Target / Equipe de otimização)

### Página inicial do site WKND

![AEM Cenário de Destino 1](assets/personalization-use-case-2/aem-target-use-case-2.png)

### Pré-requisitos

* **AEM**
   * [AEM criar e publicar ](./implementation.md#getting-aem) instruções no localhost 4502 e 4503, respectivamente.
   * [AEM integrado ao Adobe Target usando o Adobe Experience Platform Launch](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * Acesso à Adobe Experience Cloud de suas organizações - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud provisionado com as seguintes soluções
      * [Adobe Target](https://experiencecloud.adobe.com)

## Atividades do Editor de conteúdo

1. O profissional de marketing inicia a discussão de redesign da página inicial da WKND com AEM Editor de conteúdo e detalha os requisitos.
   * ***Requisito*** : Recrie a página inicial do site WKND com design baseado em cartão.
2. Com base nos requisitos, AEM Editor de conteúdo cria uma nova página inicial do Site WKND com um design baseado em cartão e publica a nova página inicial.

## Atividades do profissional de marketing

1. O profissional de marketing cria uma atividade de direcionamento A/B com a oferta de redirecionamento como uma Experiência e 100% do tráfego do site alocado para a nova página inicial com meta de sucesso e métricas adicionadas.
   1. Na janela Adobe Target, navegue até a guia **Activities**.
   2. Clique no botão **Criar atividade** e selecione o tipo de atividade como **Teste A/B**

      ![Adobe Target - Criar atividade](assets/personalization-use-case-2/create-ab-activity.png)
   3. Selecione o canal **Web** e escolha o **Visual Experience Composer**.
   4. Insira o **URL da atividade** e clique em **Avançar** para abrir o Visual Experience Composer.
      ![Adobe Target - Criar atividade](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. Para **Visual Experience Composer** carregar, ative **Permitir carregamento de scripts não seguros** no seu navegador e recarregue sua página.
      ![Atividade de Direcionamento de experiência](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. Observe a página inicial do Site WKND aberta no editor do Visual Experience Composer.
      ![VEC](assets/personalization-use-case-2/vec.png)
   7. Passe o mouse sobre **Experiência B** e selecione exibir outras opções.
      ![Experiência B](assets/personalization-use-case-2/redirect-url.png)
   8. Selecione a opção **Redirecionar para URL** e insira o URL para a nova Página Inicial WKND. (http://localhost:4503/content/wknd/en1.html)
      ![Experiência B](assets/personalization-use-case-2/redirect-url-2.png)
   9. **** Salve as alterações e continue com as próximas etapas da Criação da atividade.
   10. Selecione o **Método de alocação de tráfego** como manual e aloque 100% de tráfego para **Experiência B**.
      ![Tráfego da Experiência B](assets/personalization-use-case-2/traffic.png)
   11. Clique em **Avançar**.
   12. Forneça **Métricas de meta** para sua atividade e Salve e feche seu teste A/B.
      ![Métrica de meta do teste A/B](assets/personalization-use-case-2/goal-metric.png)
   13. Forneça um nome (**WKND Home Page Redesign**) para a Atividade e salve as alterações.
   14. Na tela Detalhes da atividade , certifique-se de **Ativar** sua atividade.
      ![Ativar atividade](assets/personalization-use-case-2/ab-activate.png)
   15. Navegue até a página inicial WKND (http://localhost:4503/content/wknd/en.html) e você será redirecionado para a página inicial do site WKND reprojetada (http://localhost:4503/content/wknd/en1.html).
      ![Página inicial WKND redesenhada](assets/personalization-use-case-2/WKND-home-page-redesign.png)

## Resumo

Neste capítulo, um profissional de marketing foi capaz de criar uma atividade para redirecionar as páginas do site hospedadas em AEM para uma nova página usando o Adobe Target.
