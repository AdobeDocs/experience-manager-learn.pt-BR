---
title: Noções básicas sobre arquivos e modelos de ativos do InDesign no AEM Assets
description: Este tutorial em vídeo aborda a definição de um arquivo InDesign e todas as considerações que o acompanham, para uso no recurso Modelos de ativos do AEM Assets.
version: Experience Manager 6.4, Experience Manager 6.5
topic: Content Management
feature: Templates
role: User
level: Intermediate
doc-type: Tutorial
exl-id: c418e94a-b18e-429a-b41c-2bf32e158598
duration: 909
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '482'
ht-degree: 0%

---

# Noções básicas sobre arquivos e modelos de ativos do InDesign no AEM Assets {#understanding-indesign-files-and-asset-templates-in-aem-assets}

Este tutorial em vídeo aborda a definição de um arquivo InDesign e todas as considerações que o acompanham, para uso no recurso Modelos de ativos do AEM Assets.

## Construção do arquivo de modelo do InDesign {#constructing-the-indesign-template-file}

>[!VIDEO](https://video.tv.adobe.com/v/19293?quality=12&learn=on)

1. Baixe e abra o [**modelo de arquivo do InDesign**](assets/asset-templates-tutorial-video--supporting-files.zip)
2. **Abra o painel &#39;Marcas de formatação&#39;**, examine a convenção de nomenclatura de marcas e observe que os elementos que podem ser criados no arquivo do InDesign já estão marcados. Lembre-se de que somente os elementos marcados são editáveis no AEM.

   * **Janela > Utilitários > Marcas**

3. Na Página, adicione um novo elemento de texto, forneça o texto &quot;Cabeçalho&quot; e aplique o Estilo de parágrafo **Cabeçalho**.

   * **Janela > Estilos > Estilos de parágrafo**

   Em seguida, crie e aplique uma nova tag chamada **Page2Heading.**

4. Adicione a imagem do Logotipo FPO ([fornecida no zip](assets/asset-templates-tutorial-video--supporting-files.zip)) ao elemento Logo na página Mestra.

   * **Clique com o botão direito do mouse em** e selecione **Ajuste > Opções de Ajuste de Quadro... > Ajuste de Conteúdo > Preencher Quadro Proporcionalmente**

   [Saiba mais sobre as opções de Ajuste de Quadro](https://helpx.adobe.com/br/indesign/using/frames-objects.html#fitting_objects_to_frames), e qual é a escolha certa para o seu caso de uso.

5. Copie para baixo o cabeçalho (Logotipo e Nome da empresa) do Modelo mestre em Página e Página por meio de Colar no local.

   * Na Página 1, clique com a tecla Shift pressionada Cmd no macOS ou com a tecla Shift pressionada Alt pressionada no Windows para selecionar o Cabeçalho exposto na página Mestra e excluí-lo.
   * Na página principal, copie o cabeçalho na Página 1 por meio de Colar no local
   * Repita as etapas para a Página 2

6. Abra o painel &#39;Estrutura&#39; clicando duas vezes em cada um deles para garantir que todos os elementos estruturais correspondam a elementos reais no arquivo do InDesign. Remova todos os elementos não utilizados ou desnecessários. Certifique-se de que toda a marcação seja semântica e que os elementos sejam marcados corretamente.

   >[!NOTE]
   >
   >Lembre-se de que um arquivo do InDesign mal construído é a causa mais comum de problemas com os modelos de ativos do AEM, portanto, verifique se a marcação e a estrutura estão limpas e corretas.

## Criação e criação de um modelo de ativo no AEM Assets {#creating-and-authoring-an-asset-template-in-aem-assets}

>[!VIDEO](https://video.tv.adobe.com/v/19294?quality=12&learn=on)

1. **Iniciar InDesign Server** na porta 8080.
2. Verifique se a instância do Autor do AEM **está configurada para interagir com o InDesign Server** (e vice-versa).

   * [Configuração do IDS Worker Cloud Service](http://localhost:4502/etc/cloudservices/proxy/ids.html)
   * [Configuração de Cloud Service do Proxy da Nuvem](http://localhost:4502/etc/cloudservices/proxy.html)
   * [Configuração OSGi do Externalizador do AEM](http://localhost:4502/system/console/configMgr)

3. **Carregou o arquivo do InDesign para o AEM Assets** e permitiu que o fluxo de trabalho do AEM e o InDesign Server processassem totalmente os ativos.
4. **Crie um novo Modelo** em **Assets > Modelos** e selecione o arquivo InDesign carregado para o AEM na Etapa #4.
5. **Edite o Modelo de ativo** criado na Etapa #5 e crie os campos editáveis.
6. Clique em **Concluído** para gerar as representações finais de alta fidelidade do Modelo de Ativo.
7. Clique no cartão Modelo de ativo para abrir o, e revise as Representações de ativos para baixar as representações de alta fidelidade.

## Recursos adicionais {#additional-resources}

Arquivo de modelo do InDesign e imagens de suporte

Baixar [arquivo de modelo do InDesign e imagens de suporte](assets/asset-templates-tutorial-video--supporting-files-1.zip)

* [Download da avaliação do InDesign CC](https://creative.adobe.com/products/download/indesign)
* A avaliação do InDesign Server pode ser baixada do [site de pré-lançamento do Adobe](https://www.adobeprerelease.com/) ou do [CC Enterprise, os clientes podem entrar em contato com o Executivo de Contas para solicitar uma licença de avaliação do InDesign Server](https://www.adobe.com/products/indesignserver/faq.html)
