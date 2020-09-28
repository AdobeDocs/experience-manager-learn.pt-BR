---
title: 'Entendendo arquivos de InDesign e modelos de ativos no AEM Assets '
description: Este tutorial em vídeo aborda a definição de um arquivo de InDesign e todas as considerações que o acompanham, para uso no recurso Modelos de ativos do AEM Assets.
feature: catalogs, asset-templates
topics: authoring, renditions, documents
audience: all
doc-type: tutorial
activity: understand
version: 6.3, 6.4, 6.5
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '483'
ht-degree: 0%

---


# Entendendo arquivos de InDesign e modelos de ativos no AEM Assets {#understanding-indesign-files-and-asset-templates-in-aem-assets}

Este tutorial em vídeo aborda a definição de um arquivo de InDesign e todas as considerações que o acompanham, para uso no recurso Modelos de ativos do AEM Assets.

## Construção do arquivo de modelo de InDesign {#constructing-the-indesign-template-file}

>[!VIDEO](https://video.tv.adobe.com/v/19293/?quality=9&learn=on)

1. Baixar e abrir o modelo de arquivo de [**InDesign**](assets/asset-templates-tutorial-video--supporting-files.zip)
2. **Abra o painel Tags,** reveja a convenção de nomenclatura de tags e observe que os elementos que podem ser criados pelo autor no arquivo de InDesign já estão marcados. Lembre-se, somente os elementos marcados são editáveis no AEM.

   * **Janela > Utilitários > Tags**

3. Na página, adicione um novo elemento de texto, forneça o texto &quot;Cabeçalho&quot; e aplique o Estilo de parágrafo do **cabeçalho** .

   * **Janela > Estilos > Estilos de parágrafo**

   Em seguida, crie e aplique uma nova tag chamada **Page2Title.**

4. Adicione a imagem do logotipo FPO ([fornecida no zip](assets/asset-templates-tutorial-video--supporting-files.zip)) ao elemento Logotipo na página Principal.

   * **Clique com o botão direito do mouse** e selecione **Ajustar > Opções de ajuste ao quadro... > Ajuste do conteúdo > Preencher quadro proporcionalmente**
   [Saiba mais sobre as opções](https://helpx.adobe.com/indesign/using/frames-objects.html#fitting_objects_to_frames)de Ajuste de quadro e qual é a escolha certa para seu caso de uso.

5. Copie o cabeçalho (Logotipo e Nome da Empresa) do modelo Principal na Página e Página por meio da opção Colar no local.

   * Na Página 1, Shift-Cmd-Clique em macOS ou Shift-Alt-Clique no Windows para selecionar o cabeçalho exposto na página Principal e excluí-lo.
   * Na página Principal, copie o cabeçalho para a Página 1 por meio da opção Colar no local
   * Repita as etapas para a Página 2

6. Abra o painel Estrutura, clicando com o duplo em cada, para garantir que todos os elementos estruturais correspondam aos elementos reais do arquivo de InDesign. Remova todos os elementos não utilizados ou desnecessários. Verifique se toda a marcação é semântica e se os elementos estão marcados corretamente.

   >[!NOTE]
   >
   >Lembre-se de que um arquivo de InDesign mal construído é a causa mais comum para problemas com modelos de ativos AEM, portanto, verifique se a marcação e a estrutura estão limpas e corretas.

## Criação e criação de um modelo de ativo no AEM Assets {#creating-and-authoring-an-asset-template-in-aem-assets}

>[!VIDEO](https://video.tv.adobe.com/v/19294/?quality=9&learn=on)

1. **InDesign Server** do start na porta 8080.
2. Verifique se a instância do autor de **AEM está configurada para interagir com seu InDesign Server**(e vice-versa).

   * [Configuração do Cloud Service do IDS Worker](http://localhost:4502/etc/cloudservices/proxy/ids.html)
   * [Configuração do Cloud Service do Proxy Cloud](http://localhost:4502/etc/cloudservices/proxy.html)
   * [Configuração do AEM Externalizer OSGi](http://localhost:4502/system/console/configMgr)

3. **Carregado o arquivo de InDesign para a AEM Assets** e permite que AEM fluxo de trabalho e InDesign Server processem totalmente os ativos.
4. **Crie um novo modelo** em **Ativos > Modelos** e selecione o arquivo de InDesign carregado para AEM na Etapa 4.
5. **Edite o Modelo** de ativo criado na Etapa 5 e crie os campos editáveis.
6. Clique em **Concluído** para gerar as renderizações finais de alta fidelidade do Modelo de ativo.
7. Clique no cartão Modelo de ativo para abrir e revisar as Representações de ativo para baixar as representações de alta fidelidade.

## Recursos adicionais {#additional-resources}

Arquivo de modelo de InDesign e imagens de suporte

Baixar o arquivo de modelo de [InDesign e as imagens de suporte](assets/asset-templates-tutorial-video--supporting-files-1.zip)

* [Download de avaliação do InDesign CC](https://creative.adobe.com/products/download/indesign)
* [Download da versão de avaliação do InDesign Server](https://www.adobe.com/devnet/indesign/indesign-server-trial-downloads.html)
