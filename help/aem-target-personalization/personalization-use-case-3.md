---
title: Personalização usando o Adobe Target Visual Experience Composer
seo-title: Personalization using Adobe Target Visual Experience Composer (VEC)
description: Um tutorial completo mostrando como criar e fornecer experiência personalizada usando o Adobe Target Visual Experience Composer (VEC).
seo-description: An end-to-end tutorial showing how to create and deliver personalized experience using Adobe Target Visual Experience Composer (VEC).
feature: Experience Fragments
topic: Personalization
role: Developer
level: Intermediate
exl-id: 1550e6a7-04b5-4a40-9d7b-88074283402f
source-git-commit: b069d958bbcc40c0079e87d342db6c5e53055bc7
workflow-type: tm+mt
source-wordcount: '582'
ht-degree: 2%

---

# Personalização usando o Visual Experience Composer

Neste capítulo, exploraremos a criação de experiências usando **Visual Experience Composer** ao arrastar e soltar, trocar e modificar o layout e o conteúdo de uma página da Web no Target.

## Visão geral do cenário

A página inicial do site WKND exibe atividades locais ou a melhor coisa a fazer em uma cidade, na forma de layouts de cartão. Como profissional de marketing, você recebeu a tarefa de modificar a página inicial, reorganizando os layouts dos cartões para ver como isso afeta a participação do usuário e a conversão de drives.

### Usuários envolvidos

Para este exercício, os seguintes usuários precisam estar envolvidos e para executar algumas tarefas que você pode precisar de acesso administrativo.

* **Produtor de conteúdo/Editor de conteúdo** (Adobe Experience Manager)
* **Profissional de marketing** (Adobe Target / Equipe de otimização)

### Página inicial do site WKND

![AEM Cenário de Destino 1](assets/personalization-use-case-3/aem-target-use-case-3.png)

### Pré-requisitos

* **AEM**
   * [Instância de publicação de AEM](./implementation.md#getting-aem) em execução no 4503
   * [AEM integrado ao Adobe Target usando o Adobe Experience Platform Launch](./using-launch-adobe-io.md#aem-target-using-launch-by-adobe)
* **Experience Cloud**
   * Acesso à Adobe Experience Cloud de suas organizações - `https://<yourcompany>.experiencecloud.adobe.com`
   * Experience Cloud provisionado com [Adobe Target](https://experiencecloud.adobe.com)

## Atividades do profissional de marketing

1. O profissional de marketing cria uma atividade de direcionamento A/B no Adobe Target.
   1. Na janela do Adobe Target, navegue até **Atividades** guia .
   2. Clique em **Criar atividade** e selecione o tipo de atividade como **Teste A/B**

      ![Adobe Target - Criar atividade](assets/personalization-use-case-2/create-ab-activity.png)
   3. Selecione o **Web** e escolha o **Visual Experience Composer**.
   4. Insira o **URL da atividade** e clique em **Próximo** para abrir o Visual Experience Composer.
      ![Adobe Target - Criar atividade](assets/personalization-use-case-2/create-activity-ab-name.png)
   5. Para **Visual Experience Composer** para carregar, habilite **Permitir Carregar Scripts Não Seguros** no navegador e recarregue a página.
      ![Atividade de Direcionamento de experiência](assets/personalization-use-case-1/load-unsafe-scripts.png)
   6. Observe a página inicial do Site WKND aberta no editor do Visual Experience Composer.
      ![VEC](assets/personalization-use-case-2/vec.png)
   7. **Experiência A** fornece a página inicial WKND padrão e vamos editar o layout de conteúdo para **Experiência B**.
      ![Experiência B](assets/personalization-use-case-3/use-case3-experience-b.png)
   8. Clique em um dos contêineres de layout de cartão (*Melhores Roasters*) e selecione **Reorganizar** opção.
      ![Seleção de contêiner](assets/personalization-use-case-3/container-selection.png)
   9. Clique no contêiner que deseja reorganizar e arraste e solte no local desejado. Vamos reorganizar o *Melhores Roasters* contêiner da primeira linha da primeira coluna para a primeira linha da terceira coluna. Agora a variável *Melhores Roasters* contêiner está ao lado de *Exposições de fotografia* contêiner.
      ![Troca de contêiner](assets/personalization-use-case-3/container-swap.png)

      **Depois da troca**
      ![Contêiner Trocado](assets/personalization-use-case-3/after-swap-1-3.png)
   10. Da mesma forma, reorganize as posições para os outros contêineres de cartão.
      ![Contêiner Trocado](assets/personalization-use-case-3/after-swap-all.png)
   11. Também vamos adicionar um texto de cabeçalho abaixo do componente carrossel e acima do layout do cartão.
   12. Clique no contêiner do carrossel e selecione o **Inserir depois > HTML** para adicionar HTML.
      ![Adicionar texto](assets/personalization-use-case-3/add-text.png)

      ```html
      <h1 style="text-align:center">Check Out the Hot Spots in Town</h1>
      ```

      ![Adicionar texto](assets/personalization-use-case-3/after-changes.png)
   13. Clique em **Próximo** para continuar com sua atividade.
   14. Selecione o **Método de alocação de tráfego** como manual e alocar 100% de tráfego para **Experiência B**.
      ![Tráfego da Experiência B](assets/personalization-use-case-2/traffic.png)
   15. Clique em **Avançar**.
   16. Fornecer **Métricas de meta** para sua atividade e salve e feche o teste A/B.
      ![Métrica de meta do teste A/B](assets/personalization-use-case-2/goal-metric.png)
   17. Forneça um nome (**Atualização da Página Inicial WKND**) para a Atividade e salve as alterações.
   18. Na tela Detalhes da atividade , certifique-se de **Ativar** sua atividade.
      ![Ativar atividade](assets/personalization-use-case-3/save-activity.png)
   19. Navegue até a página inicial da WKND (http://localhost:4503/content/wknd/en.html) e observe as alterações que adicionamos à atividade de Teste A/B de Atualização da Página Inicial da WKND.
      ![Página Inicial WKND Atualizada](assets/personalization-use-case-3/activity-result.png)
   20. Abra o console do navegador e inspecione a guia rede para procurar a resposta do target para a atividade WKND Home Page Refresh A/B Test .
      ![Atividade de rede](assets/personalization-use-case-3/activity-result.png)

## Resumo

Neste capítulo, um profissional de marketing foi capaz de criar uma experiência usando o Visual Experience Composer ao arrastar e soltar, trocar e modificar o layout e o conteúdo de uma página da Web sem alterar nenhum código para executar um teste.
