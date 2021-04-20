---
title: 'Noções básicas sobre arquivos do InDesign e modelos de ativos no AEM Assets '
description: Este tutorial em vídeo aborda a definição de um arquivo do InDesign e todas as considerações que o acompanham, para uso no recurso Modelos de ativos do AEM Assets.
version: 6.3, 6.4, 6.5
topic: Content Management
role: Business Practitioner
level: Intermediate
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '488'
ht-degree: 0%

---


# Como entender arquivos do InDesign e modelos de ativos no AEM Assets {#understanding-indesign-files-and-asset-templates-in-aem-assets}

Este tutorial em vídeo aborda a definição de um arquivo do InDesign e todas as considerações que o acompanham, para uso no recurso Modelos de ativos do AEM Assets.

## Construindo o arquivo de modelo do InDesign {#constructing-the-indesign-template-file}

>[!VIDEO](https://video.tv.adobe.com/v/19293/?quality=9&learn=on)

1. Baixe e abra o [**modelo de arquivo do InDesign**](assets/asset-templates-tutorial-video--supporting-files.zip)
2. **Abra o painel Tags,** revise a convenção de nomenclatura da tag e observe que os elementos que podem ser criados para autor no arquivo InDesign já estão marcados. Lembre-se, somente elementos marcados podem ser editados no AEM.

   * **Janela > Utilitários > Tags**

3. Na página, adicione um novo elemento de texto, forneça o texto &quot;Cabeçalho&quot; e aplique o **Cabeçalho** Estilo de parágrafo.

   * **Janela > Estilos > Estilos de parágrafo**

   Em seguida, crie e aplique uma nova tag chamada **Page2Cabeçalho.**

4. Adicione a imagem do logotipo FPO ([fornecida no zip](assets/asset-templates-tutorial-video--supporting-files.zip)) ao elemento Logotipo na página Mestre.

   * **Clique com o botão direito** e **selecione Ajuste > Opções de ajuste do quadro... > Ajuste do conteúdo > Preencher quadro proporcionalmente.**
   [Saiba mais sobre as opções](https://helpx.adobe.com/indesign/using/frames-objects.html#fitting_objects_to_frames) de Ajuste do quadro, e qual é o correto para seu caso de uso.

5. Copie o cabeçalho (Logotipo e Nome da empresa) do modelo Mestre na Página e Página por meio de Colar no local.

   * Na página 1, pressione Shift-Cmd-Clique em macOS ou Shift-Alt-Clique no Windows para selecionar o Cabeçalho exposto na página Mestre e excluí-lo.
   * Na página Mestre, copie o cabeçalho para a Página 1 por meio de Colar no local
   * Repita as etapas para a Página 2

6. Abra o painel Estrutura, clicando duas vezes em cada, para garantir que todos os elementos estruturais correspondam a elementos reais no arquivo do InDesign. Remova todos os elementos não usados ou não necessários. Certifique-se de que toda a marcação seja semântica e os elementos estejam marcados corretamente.

   >[!NOTE]
   >
   >Lembre-se de que um arquivo do InDesign mal construído é a causa mais comum de problemas com os Modelos de ativos do AEM, portanto, assegure-se de que a marcação e a estrutura estejam limpas e corretas.

## Criação e criação de um modelo de ativo no AEM Assets {#creating-and-authoring-an-asset-template-in-aem-assets}

>[!VIDEO](https://video.tv.adobe.com/v/19294/?quality=9&learn=on)

1. **Inicie a porta 8080 do** Servidor InDesign.
2. Verifique se a instância **AEM Author está configurada para interagir com seu InDesign Server** (e vice-versa).

   * [Configuração do IDS Worker Cloud Service](http://localhost:4502/etc/cloudservices/proxy/ids.html)
   * [Configuração do Cloud Proxy Cloud Service](http://localhost:4502/etc/cloudservices/proxy.html)
   * [Configuração OSGi do AEM Externalizer](http://localhost:4502/system/console/configMgr)

3. **O arquivo do InDesign foi carregado no AEM** Assets e permite que o fluxo de trabalho do AEM e o InDesign Server processem totalmente os ativos.
4. **Crie um novo** modelo em  **Ativos >** Modelos e selecione o arquivo do InDesign carregado no AEM na etapa 4.
5. **Edite o** modelo de ativo criado na etapa 5 e crie os campos editáveis.
6. Clique em **Concluído** para gerar as representações finais de alta fidelidade do Modelo de Ativo.
7. Clique no cartão Modelo de ativo para abrir e revise as Representações de ativo para baixar as representações de alta fidelidade.

## Recursos adicionais {#additional-resources}

Arquivo de modelo do InDesign e imagens de suporte

Baixe [Arquivo de modelo do InDesign e imagens de suporte](assets/asset-templates-tutorial-video--supporting-files-1.zip)

* [Download de avaliação do InDesign CC](https://creative.adobe.com/products/download/indesign)
* [Download de avaliação do InDesign Server](https://www.adobe.com/devnet/indesign/indesign-server-trial-downloads.html)
