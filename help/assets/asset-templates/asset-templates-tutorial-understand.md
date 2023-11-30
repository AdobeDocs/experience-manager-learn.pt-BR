---
title: Noções básicas sobre arquivos InDesign e modelos de ativos no AEM Assets
description: Este tutorial em vídeo aborda a definição de um arquivo do InDesign e todas as considerações que o acompanham, para uso no recurso Modelos de ativos do AEM Assets.
version: 6.4, 6.5
topic: Content Management
feature: Templates
role: User
level: Intermediate
doc-type: Tutorial
exl-id: c418e94a-b18e-429a-b41c-2bf32e158598
source-git-commit: 30d6120ec99f7a95414dbc31c0cb002152bd6763
workflow-type: tm+mt
source-wordcount: '507'
ht-degree: 1%

---

# Noções básicas sobre arquivos InDesign e modelos de ativos no AEM Assets {#understanding-indesign-files-and-asset-templates-in-aem-assets}

Este tutorial em vídeo aborda a definição de um arquivo do InDesign e todas as considerações que o acompanham, para uso no recurso Modelos de ativos do AEM Assets.

## Construção do arquivo de modelo de InDesign {#constructing-the-indesign-template-file}

>[!VIDEO](https://video.tv.adobe.com/v/19293?quality=12&learn=on)

1. Baixe e abra o [**modelo de arquivo de InDesign**](assets/asset-templates-tutorial-video--supporting-files.zip)
2. **Abra o painel &#39;Tags&#39;,** analise a convenção de nomenclatura de tags e observe que os elementos que podem ser criados no arquivo do InDesign já estão marcados. Lembre-se de que somente os elementos marcados são editáveis no AEM.

   * **&#39;Janela&#39; > &#39;Utilitários&#39; > &#39;Tags&#39;**

3. Na página, adicione um novo elemento de texto, forneça o texto &quot;Cabeçalho&quot; e aplique a variável **Cabeçalho** Estilo de parágrafo.

   * **&#39;Janela&#39; > &#39;Estilos&#39; > &#39;Estilos de parágrafo&#39;**

   Em seguida, crie e aplique uma nova tag chamada **Page2Heading.**

4. Adicionar a imagem do logotipo FPO ([fornecido no zip](assets/asset-templates-tutorial-video--supporting-files.zip)) ao elemento de logotipo na página principal.

   * **Clique com o botão direito** e selecione **&#39;Ajuste&#39; > &#39;Opções de ajuste ao quadro&#39;... > &#39;Ajuste de conteúdo&#39; > &#39;Preencher quadro proporcionalmente&#39;**

   [Saiba mais sobre as opções de Ajuste de Quadro](https://helpx.adobe.com/indesign/using/frames-objects.html#fitting_objects_to_frames)e que é adequado para o seu caso de uso.

5. Copie para baixo o cabeçalho (Logotipo e Nome da empresa) do Modelo mestre em Página e Página por meio de Colar no local.

   * Na Página 1, clique com a tecla Shift pressionada Cmd no macOS ou com a tecla Shift pressionada Alt pressionada no Windows para selecionar o Cabeçalho exposto na página Mestra e excluí-lo.
   * Na página principal, copie o cabeçalho na Página 1 por meio de Colar no local
   * Repita as etapas para a Página 2

6. Abra o painel &#39;Estrutura&#39; clicando duas vezes em cada um deles para garantir que todos os elementos estruturais correspondam a elementos reais no arquivo do InDesign. Remova todos os elementos não utilizados ou desnecessários. Certifique-se de que toda a marcação seja semântica e que os elementos sejam marcados corretamente.

   >[!NOTE]
   >
   >Lembre-se de que um arquivo de InDesign mal construído é a causa mais comum de problemas com modelos de ativos de AEM. Portanto, verifique se a marcação e a estrutura estão limpas e corretas.

## Criação e criação de um modelo de ativo no AEM Assets {#creating-and-authoring-an-asset-template-in-aem-assets}

>[!VIDEO](https://video.tv.adobe.com/v/19294?quality=12&learn=on)

1. **Iniciar InDesign Server** na porta 8080.
2. Assegure a **A instância de autor do AEM está configurada para interagir com seu InDesign Server**(e vice-versa).

   * [Configuração de Cloud Service do trabalhador de IDS](http://localhost:4502/etc/cloudservices/proxy/ids.html)
   * [Configuração de Cloud Service do proxy de nuvem](http://localhost:4502/etc/cloudservices/proxy.html)
   * [Configuração OSGi do Externalizador de AEM](http://localhost:4502/system/console/configMgr)

3. **Carregado o arquivo InDesign para o AEM Assets** e permitir que o Fluxo de trabalho e o InDesign Server do AEM processem totalmente os ativos.
4. **Criar um Novo Modelo** em **Ativos > Modelos** e selecione o arquivo InDesign carregado para AEM na Etapa #4.
5. **Editar o modelo do ativo** criado na Etapa #5 e criar os campos editáveis.
6. Clique em **Concluído** para gerar as representações finais de alta fidelidade do Modelo de ativo.
7. Clique no cartão Modelo de ativo para abrir o, e revise as Representações de ativos para baixar as representações de alta fidelidade.

## Recursos adicionais {#additional-resources}

Arquivo de modelo do InDesign e imagens de suporte

Baixar [Arquivo de modelo do InDesign e imagens de suporte](assets/asset-templates-tutorial-video--supporting-files-1.zip)

* [Download de avaliação do InDesign CC](https://creative.adobe.com/products/download/indesign)
* A versão de avaliação do InDesign Server pode ser baixada de [Site de pré-lançamento do Adobe](https://www.adobeprerelease.com/) ou [Os clientes da CC Enterprise podem entrar em contato com o executivo da conta para solicitar uma licença de avaliação do InDesign Server](https://www.adobe.com/products/indesignserver/faq.html)
