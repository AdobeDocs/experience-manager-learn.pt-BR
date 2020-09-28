---
title: Personalização usando o Adobe Target Visual Experience Composer
seo-title: Personalização usando o Adobe Target Visual Experience Composer (VEC)
description: Um tutorial completo mostrando como criar e fornecer experiência personalizada usando o Adobe Target Visual Experience Composer (VEC).
seo-description: Um tutorial completo mostrando como criar e fornecer experiência personalizada usando o Adobe Target Visual Experience Composer (VEC).
translation-type: tm+mt
source-git-commit: 0443c8ff42e773021ff8b6e969f5c1c31eea3ae4
workflow-type: tm+mt
source-wordcount: '610'
ht-degree: 2%

---


# Personalização usando o Visual Experience Composer

Neste capítulo, exploraremos a criação de experiências usando o **Visual Experience Composer** ao arrastar e soltar, trocar e modificar o layout e o conteúdo de uma página da Web a partir do Público alvo.

## Visão geral do cenário

O home page do site WKND exibe atividades locais ou a melhor coisa a se fazer em uma cidade na forma de layouts de cartão. Como comerciante, você recebeu a tarefa para modificar o home page, reorganizando os layouts do cartão para ver como isso afeta a participação do usuário e a conversão de drives.

### Usuários envolvidos

Para este exercício, os usuários a seguir precisam estar envolvidos e para executar algumas tarefas, você pode precisar de acesso administrativo.

* **Content Producer/Editor** de conteúdo (Adobe Experience Manager)
* **Comerciante** (Adobe Target / Equipe de otimização)

### Home page do site WKND

![Cenário de Público alvo AEM 1](assets/personalization-use-case-3/aem-target-use-case-3.png)

### Pré-requisitos

* **AEM**
   * [AEM instância](./implementation.md#getting-aem) de publicação em execução no 4503
   * [AEM integrado ao Adobe Target usando o Adobe Experience Platform Launch](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * Acesso à Adobe Experience Cloud de suas organizações - <https://>`<yourcompany>`.experience.ecloud.adobe.com
   * Experience Cloud fornecido com [Adobe Target](https://experiencecloud.adobe.com)

## Atividades do comerciante

1. O profissional de marketing cria uma atividade de público alvo A/B no Adobe Target.
   1. Na janela do Adobe Target, navegue até a guia **Atividade** .
   2. Clique no botão **Criar Atividade** e selecione o tipo de atividade como Teste **A/B**

      ![Adobe Target - Criar Atividade](assets/personalization-use-case-2/create-ab-activity.png)
   3. Selecione o canal **Web** e escolha o **Visual Experience Composer**.
   4. Insira o URL **da** Atividade e clique em **Avançar** para abrir o Visual Experience Composer.
      ![Adobe Target - Criar Atividade](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. Para que o **Visual Experience Composer** carregue, ative **Permitir carregamento de scripts** não seguros no seu navegador e recarregue sua página.
      ![Atividade de direcionamento de experiência](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. Observe que o home page do site WKND é aberto no editor do Visual Experience Composer.
      ![VEC](assets/personalization-use-case-2/vec.png)
   7. **A Experiência A** fornece o Home page WKND padrão e vamos editar o layout do conteúdo para a **Experiência B**.
      ![Experiência B](assets/personalization-use-case-3/use-case3-experience-b.png)
   8. Clique em um dos container de layout de cartão (*Best Roasters*) e selecione a opção **Reorganizar** .
      ![Seleção de container](assets/personalization-use-case-3/container-selection.png)
   9. Clique no container que deseja reorganizar e arraste e solte-o no local desejado. Vamos reorganizar o container *Best Roasters* da primeira linha da primeira coluna para a primeira linha da terceira. Agora o *melhor container de Roasters* estará ao lado do container de *Exposições* de Fotografia.
      ![Troca de container](assets/personalization-use-case-3/container-swap.png)

      **Após a troca**
      ![Container trocado](assets/personalization-use-case-3/after-swap-1-3.png)
   10. Da mesma forma, reorganize as posições dos outros container de cartão.
      ![Container trocado](assets/personalization-use-case-3/after-swap-all.png)
   11. Também vamos adicionar um texto de cabeçalho abaixo do componente de carrossel e acima do layout do cartão.
   12. Clique no container do carrossel e selecione a opção **Inserir depois > HTML** para adicionar HTML.
      ![Adicionar texto](assets/personalization-use-case-3/add-text.png)

      ```html
      <h1 style="text-align:center">Check Out the Hot Spots in Town</h1>
      ```

      ![Adicionar texto](assets/personalization-use-case-3/after-changes.png)
   13. Clique em **Avançar** para continuar com sua atividade.
   14. Selecione o Método **de alocação de** tráfego como manual e atribua 100% de tráfego à **Experiência B**.
      ![Tráfego da Experiência B](assets/personalization-use-case-2/traffic.png)
   15. Clique em **Avançar**.
   16. Forneça métricas **de** metas para sua Atividade e Salve e feche seu teste A/B.
      ![Métrica de meta de teste A/B](assets/personalization-use-case-2/goal-metric.png)
   17. Forneça um nome (Atualização **de Home page** WKND) para a sua Atividade e salve as alterações.
   18. Na tela de detalhes da Atividade, certifique-se de **Ativar** sua atividade.
      ![Ativar Atividade](assets/personalization-use-case-3/save-activity.png)
   19. Navegue até o Home page WKND (http://localhost:4503/content/wknd/en.html) e observe as alterações que adicionamos à atividade de Teste A/B de Atualização do Home page WKND.
      ![Home page WKND atualizado](assets/personalization-use-case-3/activity-result.png)
   20. Abra o console do navegador e inspecione a guia rede para procurar a resposta do público alvo para a atividade de teste A/B de atualização do Home page WKND.
      ![Atividade de rede](assets/personalization-use-case-3/activity-result.png)

## Resumo

Neste capítulo, um profissional de marketing conseguiu criar uma experiência usando o Visual Experience Composer arrastando e soltando, trocando e modificando o layout e o conteúdo de uma página da Web sem alterar nenhum código para executar um teste.
