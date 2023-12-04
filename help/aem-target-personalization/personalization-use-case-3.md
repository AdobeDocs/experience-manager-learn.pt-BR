---
title: Personalização usando o Adobe Target Visual Experience Composer
description: Um tutorial completo mostrando como criar e fornecer experiência personalizada usando o Adobe Target Visual Experience Composer (VEC).
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
badgeIntegration: label="Integração" type="positive"
badgeVersions: label="AEM Sites 6.5" before-title="false"
doc-type: Tutorial
exl-id: 1550e6a7-04b5-4a40-9d7b-88074283402f
duration: 167
source-git-commit: af928e60410022f12207082467d3bd9b818af59d
workflow-type: tm+mt
source-wordcount: '584'
ht-degree: 1%

---

# Personalização usando o Visual Experience Composer

Neste capítulo, exploraremos a criação de experiências usando **Visual Experience Composer** arrastando e soltando, alternando e modificando o layout e o conteúdo de uma página da Web no Target.

## Visão geral do cenário

A página inicial do site WKND exibe atividades locais ou a melhor coisa a ser feita em torno de uma cidade na forma de layouts de cartão. Como profissional de marketing, você recebeu a tarefa de modificar a página inicial, reorganizando os layouts do cartão para ver como isso afeta o engajamento do usuário e impulsiona a conversão.

### Usuários envolvidos

Para este exercício, os seguintes usuários precisam estar envolvidos e, para executar algumas tarefas, você pode precisar de acesso administrativo.

* **Produtor de conteúdo/Editor de conteúdo** (Adobe Experience Manager)
* **Profissional de marketing** (Adobe Target/Equipe de otimização)

### Página inicial do site WKND

![Cenário 1 do AEM Target](assets/personalization-use-case-3/aem-target-use-case-3.png)

### Pré-requisitos

* **AEM**
   * [Instância de publicação do AEM](./implementation.md#getting-aem) em execução no 4503
   * [AEM integrado ao Adobe Target usando o Adobe Experience Platform Launch](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * Acesso à Adobe Experience Cloud de suas organizações - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud provisionado com [Adobe Target](https://experiencecloud.adobe.com)

## Atividades do profissional de marketing

1. O profissional de marketing cria uma atividade de público-alvo A/B no Adobe Target.
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
   7. **Experiência A** fornece a página inicial padrão do WKND e vamos editar o layout de conteúdo para **Experiência B**.
      ![Experiência B](assets/personalization-use-case-3/use-case3-experience-b.png)
   8. Clique em um dos contêineres de layout do cartão (*Melhores assadores*) e selecione **Reorganizar** opção.
      ![Seleção de contêiner](assets/personalization-use-case-3/container-selection.png)
   9. Clique no contêiner que você deseja reorganizar e arraste-o e solte-o no local desejado. Vamos reorganizar as *Melhores assadores* contêiner da 1ª linha, 1ª coluna, para a 1ª linha, 3ª coluna. Agora a variável *Melhores assadores* o container está ao lado de *Exposições de fotografia* recipiente.
      ![Troca de contêiner](assets/personalization-use-case-3/container-swap.png)
      **Após a troca**
      ![Contêiner trocado](assets/personalization-use-case-3/after-swap-1-3.png)
   10. Da mesma forma, reorganize as posições para os outros contêineres de cartão.
      ![Contêiner trocado](assets/personalization-use-case-3/after-swap-all.png)
   11. Também vamos adicionar um texto de cabeçalho abaixo do componente Carrossel e acima do layout do cartão.
   12. Clique no contêiner do carrossel e selecione a **Inserir após > HTML** opção para adicionar HTML.
      ![Adicionar texto](assets/personalization-use-case-3/add-text.png)

      ```html
      <h1 style="text-align:center">Check Out the Hot Spots in Town</h1>
      ```

      ![Adicionar texto](assets/personalization-use-case-3/after-changes.png)
   13. Clique em **Próxima** para continuar com sua atividade.
   14. Selecione o **Método de alocação de tráfego** como manual e atribuir 100% de tráfego para **Experiência B**.
      ![Tráfego da experiência B](assets/personalization-use-case-2/traffic.png)
   15. Clique em **Avançar**.
   16. Fornecer **Métricas de meta** para a atividade e Salvar e fechar o teste A/B.
      ![Métrica de meta de teste A/B](assets/personalization-use-case-2/goal-metric.png)
   17. Forneça um nome (**Atualização da Página Inicial do WKND**) para sua Atividade e salve as alterações.
   18. Na tela Detalhes da atividade, verifique se **Ativar** sua atividade.
      ![Ativar atividade](assets/personalization-use-case-3/save-activity.png)
   19. Navegue até a página inicial da WKND (http://localhost:4503/content/wknd/en.html) e observe as alterações que adicionamos à atividade Atualização de página inicial A/B de teste WKND.
      ![Página inicial do WKND atualizada](assets/personalization-use-case-3/activity-result.png)
   20. Abra o console do navegador e inspecione a guia de rede para procurar a resposta do público-alvo para a atividade de Atualização de página inicial A/B de WKND.
      ![Atividade de rede](assets/personalization-use-case-3/activity-result.png)

## Resumo

Neste capítulo, um profissional de marketing conseguiu criar uma experiência usando o Visual Experience Composer arrastando e soltando, alternando e modificando o layout e o conteúdo de uma página da Web sem alterar nenhum código para executar um teste.
