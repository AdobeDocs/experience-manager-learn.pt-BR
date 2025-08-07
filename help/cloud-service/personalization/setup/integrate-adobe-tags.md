---
title: Integrar tags na Adobe Experience Platform
description: Saiba como integrar o AEM as a Cloud Service com tags na Adobe Experience Platform. A integração permite implantar o Adobe Web SDK e inserir JavaScript personalizado para coleta e personalização de dados em suas páginas do AEM.
version: Experience Manager as a Cloud Service
feature: Personalization, Integrations
topic: Personalization, Integrations, Architecture, Content Management
role: Developer, Architect, Leader, Data Architect, User
level: Beginner
doc-type: Tutorial
last-substantial-update: 2025-08-07T00:00:00Z
jira: KT-18719
thumbnail: null
source-git-commit: 70665c019f63df1e736292ad24c47624a3a80d49
workflow-type: tm+mt
source-wordcount: '745'
ht-degree: 1%

---


# Integrar tags na Adobe Experience Platform

Saiba como integrar o AEM as a Cloud Service (AEMCS) com tags na Adobe Experience Platform. A integração de Tags (também conhecida como Launch) permite implantar a Adobe Web SDK e inserir JavaScript personalizado para coleta e personalização de dados em suas páginas do AEM.

A integração permite que sua equipe de marketing ou desenvolvimento gerencie e implante o JavaScript para personalização e coleta de dados, sem precisar reimplantar o código do AEM.

## Etapas de alto nível

O processo de integração envolve quatro etapas principais que estabelecem a conexão entre o AEM e as tags:

1. **Criar, configurar e publicar uma propriedade de marcas no Adobe Experience Platform**
2. **Verificar uma configuração do Adobe IMS para Marcas no AEM**
3. **Criar uma configuração de Marcas no AEM**
4. **Aplicar a configuração de Marcas às suas páginas do AEM**

## Criar, configurar e publicar uma propriedade de tags no Adobe Experience Platform

Comece criando uma propriedade de Tags no Adobe Experience Platform. Essa propriedade ajuda você a gerenciar a implantação do Adobe Web SDK e de qualquer JavaScript personalizada necessária para personalização e coleta de dados.

1. Vá para a [Adobe Experience Platform](https://experience.adobe.com/platform), entre com sua Adobe ID e navegue até **Marcas** no menu à esquerda.\
   ![Marcas do Adobe Experience Platform](../assets/setup/aep-tags.png)

2. Clique em **Nova propriedade** para criar uma nova propriedade de marcas.\
   ![Criar Nova Propriedade de Marcas](../assets/setup/aep-create-tags-property.png)

3. Na caixa de diálogo **Criar Propriedade**, digite o seguinte:
   - **Nome da propriedade**: um nome para a propriedade de marcas
   - **Tipo de Propriedade**: Selecionar **Web**
   - **Domínio**: o domínio onde você implanta a propriedade (por exemplo, `.adobeaemcloud.com`)

   Clique em **Salvar**.

   ![Propriedade de Marcas do Adobe](../assets/setup/adobe-tags-property.png)

4. Abra a nova propriedade. A extensão **Core** já deve estar incluída. Posteriormente, você adicionará a extensão **Web SDK** ao configurar o caso de uso Experimentação, pois ela requer configuração adicional, como a **ID de sequência de dados**.\
   ![Extensão principal das Marcas do Adobe](../assets/setup/adobe-tags-core-extension.png)

5. Publique a propriedade Tags em **Fluxo de Publicação** e clicando em **Adicionar Biblioteca** para criar uma biblioteca de implantação.
   ![Fluxo de Publicação de Marcas do Adobe](../assets/setup/adobe-tags-publishing-flow.png)

6. Na caixa de diálogo **Criar Biblioteca**, forneça:
   - **Nome**: um nome para a biblioteca
   - **Ambiente**: Selecionar **Desenvolvimento**
   - **Alterações de Recursos**: Escolher **Adicionar Todos os Recursos Alterados**

   Clique em **Salvar e criar no desenvolvimento**.

   ![Criar biblioteca](../assets/setup/adobe-tags-create-library.png) de marcas do Adobe

7. Para publicar a biblioteca na produção, clique em **Aprovar e publicar na produção**. Quando a publicação estiver concluída, a propriedade estará pronta para uso no AEM.\
   ![Aprovar e publicar Marcas do Adobe](../assets/setup/adobe-tags-approve-publish.png)

## Verificar uma configuração do Adobe IMS para tags na AEM

Quando um ambiente do AEMCS é provisionado, ele inclui automaticamente uma configuração do Adobe IMS para tags, juntamente com um projeto correspondente do Adobe Developer Console. Essa configuração garante a comunicação de API segura entre o AEM e as tags.

1. No AEM, navegue até **Ferramentas** > **Segurança** > **Configurações do Adobe IMS**.\
   ![Configurações do Adobe IMS](../assets/setup/aem-ims-configurations.png)

2. Localize a configuração do **Adobe Launch**. Se disponível, selecione-a e clique em **Verificar Integridade** para verificar a conexão. Você deve ver uma resposta de sucesso.\
   ![Verificação de integridade da configuração do Adobe IMS](../assets/setup/aem-ims-configuration-health-check.png)

## Criar uma configuração de tags no AEM

Crie uma configuração de Tags no AEM para especificar a propriedade e as configurações necessárias para as páginas do site.

1. No AEM, vá para **Ferramentas** > **Serviços na Nuvem** > **Configurações do Adobe Launch**.\
   ![Configurações do Adobe Launch](../assets/setup/aem-launch-configurations.png)

2. Selecione a pasta raiz do site (por exemplo, Site WKND) e clique em **Criar**.\
   ![Criar configuração do Adobe Launch](../assets/setup/aem-create-launch-configuration.png)

3. Na caixa de diálogo, digite o seguinte:
   - **Título**: por exemplo, &quot;Marcas Adobe&quot;
   - **Configuração do IMS**: selecione a configuração do IMS verificada do **Adobe Launch**
   - **Empresa**: selecione a empresa vinculada à sua propriedade de marcas
   - **Propriedade**: escolha a propriedade Marcas criada anteriormente

   Clique em **Avançar**.

   ![Detalhes de configuração do Adobe Launch](../assets/setup/aem-launch-configuration-details.png)

4. Para fins de demonstração, mantenha os valores padrão para ambientes de **Preparo** e **Produção**. Clique em **Criar**.\
   ![Detalhes de configuração do Adobe Launch](../assets/setup/aem-launch-configuration-create.png)

5. Selecione a configuração recém-criada e clique em **Publicar** para disponibilizá-la para as páginas do site.\
   ![Publicação da configuração do Adobe Launch](../assets/setup/aem-launch-configuration-publish.png)

## Aplicar a configuração de tags ao seu site do AEM

Aplique a configuração de Tags para inserir o Web SDK e a lógica de personalização nas páginas do site.

1. No AEM, vá para **Sites**, selecione a pasta do site raiz (por exemplo, Site WKND) e clique em **Propriedades**.\
   ![Propriedades do Site do AEM](../assets/setup/aem-site-properties.png)

2. Na caixa de diálogo **Propriedades do Site**, abra a guia **Avançado**. Em **Configurações**, verifique se `/conf/wknd` está selecionado para **Configuração na Nuvem**.\
   ![Propriedades Avançadas do Site do AEM](../assets/setup/aem-site-advanced-properties.png)

## Verificar a integração

Para confirmar se a configuração de Tags está funcionando corretamente, você pode:

1. Verifique a origem de exibição de uma página de publicação do AEM ou inspecione-a usando as ferramentas de desenvolvedor do navegador
2. Usar o [Adobe Experience Platform Debugger](https://chromewebstore.google.com/detail/adobe-experience-platform/bfnnokhpnncpkdmbokanobigaccjkpob) para validar a injeção do Web SDK e do JavaScript

![Adobe Experience Platform Debugger](../assets/setup/aep-debugger.png)

## Recursos adicionais

- [visão geral do Adobe Experience Platform Debugger](https://experienceleague.adobe.com/en/docs/experience-platform/debugger/home)
- [Visão geral das tags](https://experienceleague.adobe.com/en/docs/experience-platform/tags/home)
