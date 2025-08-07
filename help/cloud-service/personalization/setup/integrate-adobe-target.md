---
title: Integrar o Adobe Target
description: Saiba como integrar o AEM as a Cloud Service ao Adobe Target para gerenciar e ativar conteúdo personalizado (Fragmentos de experiência) como ofertas.
version: Experience Manager as a Cloud Service
feature: Personalization, Integrations
topic: Personalization, Integrations, Architecture
role: Developer, Architect, Leader, Data Architect, User
level: Beginner
doc-type: Tutorial
last-substantial-update: 2025-08-07T00:00:00Z
jira: KT-18718
thumbnail: null
source-git-commit: 70665c019f63df1e736292ad24c47624a3a80d49
workflow-type: tm+mt
source-wordcount: '782'
ht-degree: 1%

---


# Integrar o Adobe Target

Saiba como integrar o AEM as a Cloud Service (AEMCS) ao Adobe Target para ativar conteúdo personalizado, como fragmentos de experiência, como ofertas no Adobe Target.

A integração permite que sua equipe de marketing crie e gerencie conteúdo personalizado centralmente no AEM. Esse conteúdo pode ser ativado facilmente como ofertas no Adobe Target.

>[!IMPORTANT]
>
>A etapa de integração é opcional se sua equipe preferir gerenciar ofertas totalmente no Adobe Target, sem usar o AEM como um repositório de conteúdo centralizado.

## Etapas de alto nível

O processo de integração envolve quatro etapas principais que estabelecem a conexão entre o AEM e o Adobe Target:

1. **Criar e configurar um projeto do Adobe Developer Console**
2. **Criar uma configuração do Adobe IMS para o Target no AEM**
3. **Criar uma configuração herdada do Adobe Target no AEM**
4. **Aplicar a configuração do Adobe Target aos Fragmentos de experiência**

## Criar e configurar um projeto do Adobe Developer Console

Para permitir que o AEM se comunique com segurança com o Adobe Target, você deve configurar um projeto do Adobe Developer Console usando a autenticação de servidor para servidor do OAuth. Você pode usar um projeto existente ou criar um novo.

1. Vá para a [Adobe Developer Console](https://developer.adobe.com/console) e entre com sua Adobe ID.

2. Crie um novo projeto ou selecione um existente.\
   ![Projeto do Adobe Developer Console](../assets/setup/adc-project.png)

3. Clique em **Adicionar API**. Na caixa de diálogo **Adicionar uma API**, filtre por **Experience Cloud**, selecione **Adobe Target** e clique em **Avançar**.\
   ![Adicionar API ao Projeto](../assets/setup/adc-add-api.png)

4. Na caixa de diálogo **Configurar API**, selecione o método de autenticação **Servidor para Servidor do OAuth** e clique em **Avançar**.\
   ![Configurar API](../assets/setup/adc-configure-api.png)

5. Na etapa **Selecionar Perfis de Produto**, selecione o **Workspace Padrão** e clique em **Salvar API Configurada**.\
   ![Selecionar Perfis de Produto](../assets/setup/adc-select-product-profiles.png)

6. Na navegação à esquerda, selecione **OAuth Server-to-Server** e revise os detalhes de configuração. Observe a ID do cliente e o Segredo do cliente - você precisa desses valores para configurar a integração do IMS no AEM.
   ![Detalhes de servidor para servidor do OAuth](../assets/setup/adc-oauth-server-to-server.png)

## Criar uma configuração do Adobe IMS para o Target no AEM

No AEM, crie uma configuração do Adobe IMS para o Target usando as credenciais da Adobe Developer Console. Essa configuração permite que o AEM se autentique com as APIs do Adobe Target.

1. No AEM, navegue até **Ferramentas** > **Segurança** e selecione **Configurações do Adobe IMS**.\
   ![Configurações do Adobe IMS](../assets/setup/aem-ims-configurations.png)

2. Clique em **Criar**.\
   ![Criar configuração do Adobe IMS](../assets/setup/aem-create-ims-configuration.png)

3. Na página **Configuração de conta técnica do Adobe IMS**, digite o seguinte:
   - **Solução da nuvem**: Adobe Target
   - **Título**: um rótulo para a configuração, como &quot;Adobe Target&quot;
   - **Servidor de autorização**: `https://ims-na1.adobelogin.com`
   - **ID do cliente**: da Adobe Developer Console
   - **Segredo do cliente**: da Adobe Developer Console
   - **Escopo**: da Adobe Developer Console
   - **ID da Organização**: da Adobe Developer Console

   Depois clique em **Criar**.

   ![Detalhes de configuração do Adobe IMS](../assets/setup/aem-ims-configuration-details.png)

4. Selecione a configuração e clique em **Verificar integridade** para verificar a conexão. Uma mensagem de sucesso confirma que o AEM pode se conectar ao Adobe Target.\
   ![Verificação de integridade da configuração do Adobe IMS](../assets/setup/aem-ims-configuration-health-check.png)

## Criar uma configuração herdada do Adobe Target no AEM

Para exportar Fragmentos de experiência como ofertas para o Adobe Target, crie uma configuração herdada do Adobe Target no AEM.

1. No AEM, navegue até **Ferramentas** > **Serviços na nuvem** e selecione **Serviços na nuvem herdados**.\
   ![Serviços na nuvem herdados](../assets/setup/aem-legacy-cloud-services.png)

2. Na seção **Adobe Target**, clique em **Configurar agora**.\
   ![Configurar o Adobe Target Legacy](../assets/setup/aem-configure-adobe-target-legacy.png)

3. Na caixa de diálogo **Criar Configuração**, digite um nome como &quot;Adobe Target Legacy&quot; e clique em **Criar**.\
   ![Criar configuração herdada do Adobe Target](../assets/setup/aem-create-adobe-target-legacy-configuration.png)

4. Na página **Configuração herdada do Adobe Target**, forneça o seguinte:
   - **Autenticação**: IMS
   - **Código do cliente**: seu código de cliente do Adobe Target (encontrado no Adobe Target em **Administração** > **Implementação**)
   - **Configuração do IMS**: a configuração do IMS criada anteriormente

   Clique em **Conectar-se ao Adobe Target** para validar a conexão.

   ![Configuração herdada do Adobe Target](../assets/setup/aem-target-legacy-configuration.png)

## Aplicar a configuração do Adobe Target aos fragmentos de experiência

Associe a configuração do Adobe Target aos Fragmentos de experiência para que eles possam ser exportados e usados como ofertas no Target.

1. No AEM, vá para **Fragmentos de experiência**.\
   ![Fragmentos de experiência](../assets/setup/aem-experience-fragments.png)

2. Selecione a pasta raiz que contém os Fragmentos de experiência (por exemplo, `WKND Site Fragments`) e clique em **Propriedades**.\
   ![Propriedades dos Fragmentos de experiência](../assets/setup/aem-experience-fragments-properties.png)

3. Na página **Propriedades**, abra a guia **Serviços da Nuvem**. Na seção **Configurações do Cloud Service**, selecione a configuração do Adobe Target.\
   ![Serviços de nuvem de fragmentos de experiência](../assets/setup/aem-experience-fragments-cloud-services.png)

4. Na seção **Adobe Target** exibida, conclua o seguinte:
   - **Formato de Exportação Adobe Target**: HTML
   - **Adobe Target Workspace**: selecione o espaço de trabalho a ser usado (por exemplo, &quot;Workspace padrão&quot;)
   - **Domínios Externalizer**: insira os domínios para gerar URLs externas

   ![Configuração de Adobe Target de Fragmentos de experiência](../assets/setup/aem-experience-fragments-adobe-target-configuration.png)

5. Clique em **Salvar e fechar** para aplicar a configuração.

## Verificar a integração

Para confirmar se a integração funciona corretamente, teste a funcionalidade de exportação:

1. No AEM, crie um novo Fragmento de experiência ou abra um existente. Clique em **Exportar para o Adobe Target** na barra de ferramentas.\
   ![Exportar fragmento de experiência para o Adobe Target](../assets/setup/aem-export-experience-fragment-to-adobe-target.png)

2. No Adobe Target, vá para a seção **Ofertas** e verifique se o Fragmento de experiência aparece como uma oferta.\
   ![Ofertas da Adobe Target](../assets/setup/adobe-target-xf-as-offer.png)

## Recursos adicionais

- [Visão geral da API do Target](https://experienceleague.adobe.com/pt-br/docs/target-dev/developer/api/target-api-overview)
- [Oferta do Target](https://experienceleague.adobe.com/pt-br/docs/target/using/experiences/offers/manage-content)
- [Adobe Developer Console](https://developer.adobe.com/developer-console/docs/guides/)
- [Fragmentos de experiência no AEM](https://experienceleague.adobe.com/pt-br/docs/experience-manager-learn/sites/experience-fragments/experience-fragments-feature-video-use)