---
title: Solucionar problemas do pipeline de CI/CD usando o agente de desenvolvimento do AEM
description: Saiba como solucionar problemas e corrigir uma falha no pipeline de CI/CD usando o Agente de desenvolvimento do AEM.
version: Experience Manager as a Cloud Service
role: Developer, Admin
level: Beginner
doc-type: tutorial
duration: null
jira: KT-20279
thumbnail: KT-20279.png
last-substantial-update: null
source-git-commit: 6fa0f88c231f7b68392a77a60491d4f741140a5a
workflow-type: tm+mt
source-wordcount: '1227'
ht-degree: 1%

---


# Solucionar problemas do pipeline de CI/CD usando o agente de desenvolvimento do AEM

Saiba como solucionar problemas e corrigir uma falha no pipeline de CI/CD usando o Agente de desenvolvimento do AEM.

O Agente de Desenvolvimento do AEM ajuda equipes técnicas, incluindo desenvolvedores, engenheiros de DevOps e administradores a **acelerar os fluxos de trabalho**, fornecendo _orientação e ações alimentadas por IA_.

>[!TIP]
>
> Consulte também [Visão Geral dos Agentes no AEM](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/ai-in-aem/agents/overview) para obter uma lista completa dos Agentes disponíveis no AEM as a Cloud Service, suas funcionalidades e como obter acesso a eles.


## Visão geral

O AEM Development Agent oferece vários recursos, incluindo a capacidade de listar, solucionar problemas e corrigir falhas nos pipelines de CI/CD. Você pode chamar o Agente de desenvolvimento do AEM por meio do Assistente de IA para resolver seus casos de uso específicos.

Este tutorial usa o [Projeto de sites WKND](https://github.com/adobe/aem-guides-wknd) para demonstrar como solucionar problemas e corrigir um pipeline de CI/CD com falha usando o Agente de Desenvolvimento do AEM. Os mesmos princípios se aplicam a qualquer projeto do AEM.

Para simplificar, este tutorial apresenta uma falha de teste de unidade no arquivo `BylineImpl.java` para mostrar os recursos de solução de problemas de pipeline do Agente de desenvolvimento do AEM.

## Pré-requisitos

Para seguir este tutorial, você precisa:

- Assistente de IA e agentes no AEM ativados. Consulte [Configurar IA no AEM](./setup.md) para obter detalhes e observe que os playgrounds mencionados neste artigo não terão recursos do Agente de Desenvolvimento do AEM.
- Acesso ao Adobe [Cloud Manager](https://my.cloudmanager.adobe.com/) com uma função de Desenvolvedor ou Gerente de Programa. Consulte [definições de função](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-manager/content/requirements/users-and-roles#role-definitions) para obter mais informações.
- Um ambiente do AEM as a Cloud Service
- Acesso aos agentes no AEM através do [programa Beta](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/release-notes/release-notes/release-notes-current#aem-beta-programs)
- O [Projeto do WKND Sites](https://github.com/adobe/aem-guides-wknd) foi clonado em seu computador local

### Recursos atuais do agente de desenvolvimento do AEM

Antes de mergulhar no tutorial, vamos analisar os recursos atuais do Agente de desenvolvimento do AEM:

- Listar pipelines de CI/CD e seus status
- Falha na solução de problemas e na correção de **pilha completa** pipelines, incluindo os tipos _Qualidade de Código_ e _Implantação_.
- As etapas _Build_ (compilação do código para produzir um artefato implantável) e _Qualidade do Código_ (análise de código estático via regras SonarQube) dos pipelines **pilha completa** têm suporte.

Os recursos do Agente de desenvolvimento da AEM são continuamente expandidos e atualizados regularmente. Para comentários e sugestões, envie um email para [aem-devagent@adobe.com](mailto:aem-devagent@adobe.com).

## Configurar

Siga estas etapas de alto nível para concluir este tutorial:

1. Clonar o [Projeto de sites WKND](https://github.com/adobe/aem-guides-wknd) e enviá-lo para o repositório Git da Cloud Manager
2. Criar e configurar um pipeline de Qualidade de código
3. Execute o pipeline e observe a execução com falha
4. Use o Agente de desenvolvimento do AEM para solucionar problemas e corrigir o pipeline que falhou

Vamos analisar cada etapa em detalhes.

### Usar projeto do WKND Sites como um projeto de demonstração

Este tutorial usa a ramificação `tutorial/dev-agent/unit-test-failure` do projeto do Sites WKND para demonstrar como usar o Agente de desenvolvimento do AEM. Os mesmos princípios podem ser aplicados a qualquer projeto do AEM.

- Uma falha de teste de unidade foi introduzida no arquivo `BylineImpl.java` da seguinte maneira. Se estiver usando seu próprio projeto do AEM, você pode apresentar uma falha de teste de unidade semelhante.

  ```java
  ...
  @Override
  public String getName() {
      if (name != null) {
          return "Author: " + name; // This line is intentionally incorrect to introduce a unit test failure.
      }
      return name;
  }
  ...
  ```

- Clonar o [Projeto de Sites do WKND](https://github.com/adobe/aem-guides-wknd) no computador local, navegar até o diretório do projeto e alternar para a ramificação `tutorial/dev-agent/unit-test-failure`.

  ```shell
  git clone https://github.com/adobe/aem-guides-wknd.git
  cd aem-guides-wknd
  git checkout tutorial/dev-agent/unit-test-failure
  ```

- Crie um novo repositório Git do Cloud Manager para o projeto do WKND Sites e adicione-o como remoto ao seu repositório Git local:

   - Navegue até Adobe [Cloud Manager](https://my.cloudmanager.adobe.com/) e selecione seu programa.
   - Clique em **Repositórios** na barra lateral esquerda.
   - Clique em **Adicionar repositório** no canto superior direito.
   - Insira um **Nome do Repositório** (por exemplo, &quot;wknd-site-tutorial&quot;) e clique em **Salvar**. Aguarde a criação do repositório.

     ![Adicionar repositório](./assets/dev-agent/add-repository.png)

   - Clique em **Acessar informações do repositório** no canto superior direito e copie a URL do repositório.

     ![Acessar informações do repositório](./assets/dev-agent/access-repo-info.png)

   - Adicione o repositório Git do Cloud Manager recém-criado como remoto ao repositório Git local:

     ```shell
     git remote add adobe https://git.cloudmanager.adobe.com/<your-adobe-organization>/wknd-site-tutorial/
     ```

- Envie seu repositório Git local para o repositório Git da Cloud Manager:

  ```shell
  git push adobe
  ```

  Quando as credenciais forem solicitadas, forneça o **Nome de usuário** e a **Senha** do modal **Informações do repositório** da Cloud Manager.

### Criar e configurar um pipeline de qualidade de código

Este tutorial usa um pipeline de Qualidade de código (não produção) para acionar a falha do pipeline para a solução de problemas. Consulte [Introdução aos pipelines de CI/CD](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/implementing/using-cloud-manager/cicd-pipelines/introduction-ci-cd-pipelines#introduction) para obter mais informações sobre pipelines de qualidade do código.

- No Cloud Manager, navegue até a seção **Pipelines** e selecione **Adicionar** > **Adicionar pipeline de não produção**.
- Na caixa de diálogo **Adicionar pipeline de não produção**, configure o seguinte:

   - Etapa de **configuração**:
      - Mantenha os valores padrão como **Tipo de Pipeline** como `Code Quality Pipeline` e **Acionador de Implantação** como `Manual`.
      - Para **Nome do Pipeline de Não Produção**, digite `Code Quality::Fullstack`

     ![Adicionar configuração de pipeline de não produção](./assets/dev-agent/add-non-production-pipeline-configuration.png)

   - **Código Source** etapa:
      - Selecionar **Código de pilha completa**
      - Para **Repositório**, selecione o repositório Git do Cloud Manager recém-criado
      - Para a **Ramificação Git**, selecione `tutorial/dev-agent/unit-test-failure`
      - Clique em **Salvar**

     ![Adicionar código Source de não produção do pipeline](./assets/dev-agent/add-non-production-pipeline-source-code.png)

- Execute o pipeline de Qualidade de Código recém-criado clicando em **Executar** no menu de três pontos da entrada do pipeline.

  ![Executar Pipeline de Qualidade de Código](./assets/dev-agent/run-code-quality-pipeline.png)


>[!IMPORTANT]
>
> O pipeline de implantação não é abordado neste tutorial. No entanto, você pode seguir os mesmos princípios para solucionar problemas e corrigir um pipeline de implantação com falha.


### Observe a falha na execução do pipeline

O pipeline de Qualidade de Código falha na etapa **Preparação de Artefato** com um erro:

![Falha na execução do pipeline](./assets/dev-agent/failed-pipeline-execution.png)

Sem o Agente de desenvolvimento do AEM, essa falha de pipeline requer a solução de problemas manual. Um desenvolvedor precisaria verificar os registros e revisar o código, um processo entediante e demorado.

Em seguida, você verá como o Agentic AI pode solucionar problemas e corrigir a falha na execução do pipeline.

## Use o Agente de desenvolvimento do AEM para solucionar problemas e corrigir o pipeline que falhou

Você pode chamar o Agente de desenvolvimento do AEM usando o Assistente de IA no AEM descrevendo a falha do pipeline em linguagem natural.

- Clique no ícone do **Assistente de IA** no canto superior direito.

- Insira os detalhes de falha do pipeline em linguagem natural também conhecida como **Prompt**. Por exemplo:

  ```text
  I have a failed pipeline execution on %PROGRAM-NAME% program, help me to troubleshoot and fix it.
  ```

  ![Invocar Agente de Desenvolvimento do AEM](./assets/dev-agent/invoke-aem-development-agent.png)

  O **Agente de Desenvolvimento do AEM** é chamado para solucionar problemas e corrigir a falha na execução do pipeline.

  >[!NOTE]
  >
  > Se o prompt inserido não estiver claro, o Assistente de IA solicitará esclarecimentos e fornecerá informações para ajudá-lo a refinar o prompt.

- Quando o raciocínio estiver concluído, clique no ícone **Abrir em tela cheia** para exibir o processo detalhado de solução de problemas.

  ![Abrir em tela inteira](./assets/dev-agent/open-in-full-screen.png)

  Os resultados contêm insights valiosos, incluindo detalhes de erros, o arquivo de origem, o número da linha e uma seção **Como corrigir** com etapas claras para resolver o problema.

- Nesse caso, o agente sugeriu corretamente a alteração da implementação (método `getName()`) ou a atualização do teste de unidade (método `getNameTest()`) para corrigir o problema. Ele evitava alucinações e usava uma abordagem de &quot;humano no loop&quot; ao fornecer alterações de código acionáveis para o desenvolvedor.

  ![Copiar alterações de código](./assets/dev-agent/copy-code-changes.png)

- Atualize o arquivo `BylineImpl.java` com as alterações de código sugeridas e, em seguida, confirme e envie as alterações para o repositório Git do Cloud Manager.

  ```java
  ...
  @Override
  public String getName() {
      return name;
  }
  ...
  ```

- Execute o pipeline novamente e observe a execução bem-sucedida.

## Exemplos adicionais

O projeto de sites WKND inclui exemplos adicionais de problemas de código com falha e configuração, como dependências ausentes e configuração incorreta. Você pode explorar esses exemplos verificando as [ramificações que começam com `tutorial/dev-agent/`](https://github.com/adobe/aem-guides-wknd/branches/all?query=tutorial%2Fdev-agent&lastTab=overview). Para ver as alterações de quebra, você pode comparar a ramificação `tutorial/dev-agent/unit-test-failure` com a ramificação `main` clicando no botão **Comparar**. Em seguida, procure a seção _arquivo alterado_.

![Comparar ramificações](./assets/dev-agent/compare-branches.png)

Consulte também os [Pedidos de amostra](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/ai-in-aem/agents/development/overview#sample-prompts) para obter mais ideias sobre como usar o Agente de Desenvolvimento do AEM.

## Resumo

Neste tutorial, você aprendeu a usar o AEM Development Agent para solucionar problemas e corrigir um pipeline de CI/CD com falha usando o Assistente de IA. Você também aprendeu como o Agentic AI acelera os fluxos de trabalho técnicos, fornecendo insights acionáveis e alterações de código.

Comece a usar o Agente de Desenvolvimento da AEM e outros Agentes no AEM para acelerar seus fluxos de trabalho. Consulte [Visão geral dos Agentes no AEM](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/ai-in-aem/agents/overview) para obter mais informações.

## Recursos adicionais

- [IA no Experience Manager](./overview.md)
- [Visão geral dos agentes no AEM](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/ai-in-aem/agents/overview)
- [Visão geral do agente de desenvolvimento](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/ai-in-aem/agents/development/overview)
- [Visão geral dos agentes no AEM](https://experienceleague.adobe.com/pt-br/docs/experience-manager-cloud-service/content/ai-in-aem/agents/overview)
