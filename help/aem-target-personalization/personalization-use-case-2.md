---
title: Personalização usando o Adobe Target
description: Um tutorial completo mostrando como criar e fornecer experiência personalizada usando o Adobe Target.
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="Integração" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 53cafd06-3a0a-4995-947d-179146b89234
duration: 159
source-git-commit: adf3fe30474bcfe5fc1a1e2a8a3d49060067726d
workflow-type: tm+mt
source-wordcount: '577'
ht-degree: 1%

---

# Personalização de experiências completas de página da Web usando o Adobe Target

No capítulo anterior, aprendemos a criar uma atividade baseada em localização geográfica no Adobe Target usando conteúdo criado como Fragmentos de experiência e exportado do AEM como Ofertas HTML.

Neste capítulo, exploraremos a criação de atividades para redirecionar as páginas do site hospedadas no AEM para uma nova página usando o Adobe Target.

## Visão geral do cenário

O site da WKND reprojetou sua página inicial e gostaria de redirecionar os visitantes da página inicial atual para a nova página inicial. Ao mesmo tempo, você também entende como a página inicial reprojetada ajuda a melhorar o envolvimento do usuário e a receita. Como profissional de marketing, você recebeu a tarefa de criar uma atividade para redirecionar os visitantes para a nova página inicial. Vamos explorar a página inicial do site WKND e aprender a criar uma atividade usando o Adobe Target.

### Usuários envolvidos

Para este exercício, os seguintes usuários precisam estar envolvidos e, para executar algumas tarefas, você pode precisar de acesso administrativo.

* **Produtor de conteúdo/Editor de conteúdo** (Adobe Experience Manager)
* **Profissional de marketing** (Adobe Target/Equipe de otimização)

### Página inicial do site WKND

![Cenário 1 do AEM Target](assets/personalization-use-case-2/aem-target-use-case-2.png)

### Pré-requisitos

* **AEM**
   * [Autor e instância de publicação do AEM](./implementation.md#getting-aem) executando localhost 4502 e 4503, respectivamente.
   * [AEM integrado ao Adobe Target usando tags](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * Acesso à Adobe Experience Cloud de suas organizações - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud provisionado com as seguintes soluções
      * [Adobe Target](https://experiencecloud.adobe.com)

## Atividades do editor de conteúdo

1. O profissional de marketing inicia a discussão de redesign da página inicial do WKND com o editor de conteúdo AEM e detalha os requisitos.
   * ***Requisito*** : recrie a página inicial do site WKND com design baseado em cartão.
2. Com base nos requisitos, o Editor de conteúdo AEM cria uma nova página inicial do site WKND com um design baseado em cartão e publica a nova página inicial.

## Atividades do profissional de marketing

1. O profissional de marketing cria uma atividade de direcionamento A/B com a oferta de redirecionamento como uma experiência e 100% do tráfego do site atribuído à nova página inicial com meta de sucesso e métricas adicionadas.
   1. Na janela do Adobe Target, navegue até **Atividades** guia.
   2. Clique em **Criar atividade** e selecione o tipo de atividade como **Teste A/B**
      ![Adobe Target - Criar atividade](assets/personalization-use-case-2/create-ab-activity.png)
   3. Selecione o **Web** e escolha a variável **Visual Experience Composer**.
   4. Insira o **URL da atividade** e clique em **Próxima** para abrir o Visual Experience Composer.
      ![Adobe Target - Criar atividade](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. Para **Visual Experience Composer** para carregar, habilite **Permitir carregamento de scripts não seguros** no navegador e recarregue a página.
      ![Atividade de direcionamento de experiência](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. Observe que a página inicial do site WKND é aberta no editor do Visual Experience Composer.
      ![VEC](assets/personalization-use-case-2/vec.png)
   7. Focalizar **Experiência B** e selecione exibir outras opções.
      ![Experiência B](assets/personalization-use-case-2/redirect-url.png)
   8. Selecionar **Redirecionar para URL** e insira o URL para a nova Página inicial do WKND. (http://localhost:4503/content/wknd/en1.html)
      ![Experiência B](assets/personalization-use-case-2/redirect-url-2.png)
   9. **Salvar** suas alterações e continue com as próximas etapas da Criação da atividade.
   10. Selecione o **Método de alocação de tráfego** como manual e atribuir 100% de tráfego para **Experiência B**.
      ![Tráfego da experiência B](assets/personalization-use-case-2/traffic.png)
   11. Clique em **Avançar**.
   12. Fornecer **Métricas de meta** para a atividade e Salvar e fechar o teste A/B.
      ![Métrica de meta de teste A/B](assets/personalization-use-case-2/goal-metric.png)
   13. Forneça um nome (**Redesign da página inicial do WKND**) para sua Atividade e salve as alterações.
   14. Na tela Detalhes da atividade, verifique se **Ativar** sua atividade.
      ![Ativar atividade](assets/personalization-use-case-2/ab-activate.png)
   15. Navegue até a página inicial da WKND (http://localhost:4503/content/wknd/en.html) e você será redirecionado para a página inicial do site da WKND reprojetada (http://localhost:4503/content/wknd/en1.html).
      ![Página inicial do WKND recriada](assets/personalization-use-case-2/WKND-home-page-redesign.png)

## Resumo

Neste capítulo, um profissional de marketing conseguiu criar uma atividade para redirecionar as páginas do site hospedadas no AEM para uma nova página usando o Adobe Target.
