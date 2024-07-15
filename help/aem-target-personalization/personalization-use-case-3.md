---
title: Personalization usando o Adobe Target Visual Experience Composer
description: Um tutorial completo mostrando como criar e fornecer experiência personalizada usando o Adobe Target Visual Experience Composer (VEC).
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="Integração" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 1550e6a7-04b5-4a40-9d7b-88074283402f
duration: 112
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '581'
ht-degree: 1%

---

# Personalization usando o Visual Experience Composer

Neste capítulo, exploraremos a criação de experiências usando o **Visual Experience Composer** arrastando e soltando, alternando e modificando o layout e o conteúdo de uma página da Web no Target.

## Visão geral do cenário

A página inicial do site WKND exibe atividades locais ou a melhor coisa a ser feita em torno de uma cidade na forma de layouts de cartão. Como profissional de marketing, você recebeu a tarefa de modificar a página inicial, reorganizando os layouts do cartão para ver como isso afeta o engajamento do usuário e impulsiona a conversão.

### Usuários envolvidos

Para este exercício, os seguintes usuários precisam estar envolvidos e, para executar algumas tarefas, você pode precisar de acesso administrativo.

* **Produtor/Editor de Conteúdo** (Adobe Experience Manager)
* **Profissional de marketing** (Adobe Target/Equipe de otimização)

### Página inicial do site WKND

![Cenário de Destino do AEM 1](assets/personalization-use-case-3/aem-target-use-case-3.png)

### Pré-requisitos

* **AEM**
   * [Instância de publicação do AEM](./implementation.md#getting-aem) em execução no 4503
   * [AEM integrado ao Adobe Target usando tags](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * Acesso à Adobe Experience Cloud de suas organizações - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud provisionado com [Adobe Target](https://experiencecloud.adobe.com)

## Atividades do profissional de marketing

1. O profissional de marketing cria uma atividade de público-alvo A/B no Adobe Target.
   1. Na janela do Adobe Target, navegue até a guia **Atividades**.
   2. Clique no botão **Criar atividade** e selecione o tipo de atividade como **Teste A/B**
      ![Adobe Target - Criar Atividade](assets/personalization-use-case-2/create-ab-activity.png)
   3. Selecione o canal **Web** e escolha o **Visual Experience Composer**.
   4. Insira o **URL da atividade** e clique em **Avançar** para abrir o Visual Experience Composer.
      ![Adobe Target - Criar Atividade](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. Para o **Visual Experience Composer** carregar, habilite o **Permitir carregamento de scripts não seguros** no navegador e recarregue a página.
      ![Atividade de Direcionamento de Experiência](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. Observe que a página inicial do site WKND é aberta no editor do Visual Experience Composer.
      ![VEC](assets/personalization-use-case-2/vec.png)
   7. A **Experiência A** fornece a Página Inicial WKND padrão e vamos editar o layout de conteúdo para a **Experiência B**.
      ![Experiência B](assets/personalization-use-case-3/use-case3-experience-b.png)
   8. Clique em um dos contêineres de layout do cartão (*Melhores Roasters*) e selecione a opção **Reorganizar**.
      ![Seleção de Contêiner](assets/personalization-use-case-3/container-selection.png)
   9. Clique no contêiner que você deseja reorganizar e arraste-o e solte-o no local desejado. Vamos reorganizar o contêiner *Melhores Roasters* da primeira linha, primeira coluna, para a primeira linha, terceira coluna. Agora o contêiner *Melhores assadores* está ao lado do contêiner *Exposições de Fotografia*.
      ![Troca de Contêineres](assets/personalization-use-case-3/container-swap.png)
      **Após a troca**
      ![Contêiner Trocado](assets/personalization-use-case-3/after-swap-1-3.png)
   10. Da mesma forma, reorganize as posições para os outros contêineres de cartão.
      ![Contêiner Trocado](assets/personalization-use-case-3/after-swap-all.png)
   11. Também vamos adicionar um texto de cabeçalho abaixo do componente Carrossel e acima do layout do cartão.
   12. Clique no contêiner do carrossel e selecione a opção **Inserir após > HTML** para adicionar HTML.
      ![Adicionar texto](assets/personalization-use-case-3/add-text.png)

      ```html
      <h1 style="text-align:center">Check Out the Hot Spots in Town</h1>
      ```

      ![Adicionar texto](assets/personalization-use-case-3/after-changes.png)
   13. Clique em **Avançar** para continuar com sua atividade.
   14. Selecione o **Método de Alocação de Tráfego** como manual e aloque 100% de tráfego para a **Experiência B**.
      ![Tráfego da Experiência B](assets/personalization-use-case-2/traffic.png)
   15. Clique em **Avançar**.
   16. Forneça as **Métricas de meta** para sua Atividade e Salve e Feche o Teste A/B.
      ![Métrica de meta de teste A/B](assets/personalization-use-case-2/goal-metric.png)
   17. Forneça um nome (**Atualização da Página Inicial do WKND**) para a Atividade e salve as alterações.
   18. Na tela Detalhes da atividade, **Ative** sua atividade.
      ![Ativar Atividade](assets/personalization-use-case-3/save-activity.png)
   19. Navegue até a página inicial da WKND (http://localhost:4503/content/wknd/en.html) e observe as alterações que adicionamos à atividade Atualização de página inicial A/B de teste WKND.
      ![Página Inicial do WKND Atualizada](assets/personalization-use-case-3/activity-result.png)
   20. Abra o console do navegador e inspecione a guia de rede para procurar a resposta do público-alvo para a atividade de Atualização de página inicial A/B de WKND.
      ![Atividade de Rede](assets/personalization-use-case-3/activity-result.png)

## Resumo

Neste capítulo, um profissional de marketing conseguiu criar uma experiência usando o Visual Experience Composer arrastando e soltando, alternando e modificando o layout e o conteúdo de uma página da Web sem alterar nenhum código para executar um teste.
