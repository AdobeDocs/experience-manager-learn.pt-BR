---
title: Como entender arquivos do InDesign e modelos de ativos no AEM Assets
description: Este tutorial em vídeo aborda a definição de um arquivo do InDesign e todas as considerações que o acompanham, para uso no recurso Modelos de ativos do AEM Assets.
version: 6.3, 6.4, 6.5
topic: Content Management
role: User
level: Intermediate
exl-id: c418e94a-b18e-429a-b41c-2bf32e158598
source-git-commit: bf5b2fca04c09fd52df8ef8d9fca8b4b7bd2de2f
workflow-type: tm+mt
source-wordcount: '507'
ht-degree: 1%

---

# Como entender arquivos do InDesign e modelos de ativos no AEM Assets {#understanding-indesign-files-and-asset-templates-in-aem-assets}

Este tutorial em vídeo aborda a definição de um arquivo do InDesign e todas as considerações que o acompanham, para uso no recurso Modelos de ativos do AEM Assets.

## Construir o arquivo de modelo de InDesign {#constructing-the-indesign-template-file}

>[!VIDEO](https://video.tv.adobe.com/v/19293/?quality=9&learn=on)

1. Baixe e abra o [**Modelo de arquivo de InDesign**](assets/asset-templates-tutorial-video--supporting-files.zip)
2. **Abra o painel Tags ,** revise a convenção de nomenclatura de tag e observe que os elementos que podem ser criados para autor no arquivo de InDesign já estão marcados. Lembre-se, somente os elementos marcados podem ser editados em AEM.

   * **Janela > Utilitários > Tags**

3. Na página, adicione um novo elemento de texto, forneça o texto &quot;Cabeçalho&quot; e aplique a variável **Cabeçalho** Estilo do parágrafo.

   * **Janela > Estilos > Estilos de parágrafo**

   Em seguida, crie e aplique uma nova tag chamada **Page2Cabeçalho.**

4. Adicione a imagem do logotipo FPO ([fornecido no CEP](assets/asset-templates-tutorial-video--supporting-files.zip)) ao elemento Logotipo na página Principal.

   * **Clique com o botão direito** e selecione **Ajuste > Opções de ajuste do quadro... > Ajuste do conteúdo > Preencher quadro proporcionalmente**
   [Saiba mais sobre as opções de ajuste de quadro](https://helpx.adobe.com/indesign/using/frames-objects.html#fitting_objects_to_frames), e que é o correto para o seu caso de uso.

5. Copie o cabeçalho (Logotipo e Nome da empresa) do modelo Principal na Página e na Página por meio da opção Colar no local.

   * Na página 1, pressione Shift-Cmd-Clique no macOS ou Shift-Alt-Clique no Windows para selecionar o Cabeçalho exposto na página Principal e excluí-lo.
   * Na página Principal, copie o cabeçalho para a Página 1 por meio de Colar no local
   * Repita as etapas para a Página 2

6. Abra o painel Estrutura, clicando duas vezes em cada, para garantir que todos os elementos estruturais correspondam aos elementos reais no arquivo do InDesign. Remova todos os elementos não usados ou não necessários. Certifique-se de que toda a marcação seja semântica e os elementos estejam marcados corretamente.

   >[!NOTE]
   >
   >Lembre-se de que um arquivo de InDesign mal construído é a causa mais comum de problemas com modelos de ativos AEM, portanto, certifique-se de que a marcação e a estrutura estão limpas e corretas.

## Criação e criação de um modelo de ativo no AEM Assets {#creating-and-authoring-an-asset-template-in-aem-assets}

>[!VIDEO](https://video.tv.adobe.com/v/19294/?quality=9&learn=on)

1. **Iniciar InDesign Server** no porto 8080.
2. Verifique se a **A instância do autor do AEM está configurada para interagir com seu InDesign Server**(e vice-versa).

   * [Configuração do Cloud Service do IDS Worker](http://localhost:4502/etc/cloudservices/proxy/ids.html)
   * [Configuração do Cloud Service do Cloud Proxy](http://localhost:4502/etc/cloudservices/proxy.html)
   * [Configuração OSGi do AEM Externalizador](http://localhost:4502/system/console/configMgr)

3. **Carregado o arquivo do InDesign para o AEM Assets** e permitem que AEM fluxo de trabalho e o InDesign Server processem totalmente os ativos.
4. **Criar um novo modelo** under **Ativos > Modelos** e selecione o arquivo do InDesign carregado no AEM na Etapa 4.
5. **Editar o modelo de ativo** criado na Etapa 5 e crie os campos editáveis.
6. Clique em **Concluído** para gerar as renderizações finais de alta fidelidade do Modelo de ativo.
7. Clique no cartão Modelo de ativo para abrir e revise as Representações de ativo para baixar as representações de alta fidelidade.

## Recursos adicionais {#additional-resources}

Arquivo de modelo de InDesign e imagens de suporte

Baixar [Arquivo de modelo de InDesign e imagens de suporte](assets/asset-templates-tutorial-video--supporting-files-1.zip)

* [Download de avaliação do InDesign CC](https://creative.adobe.com/products/download/indesign)
* A avaliação do InDesign Server pode ser baixada de [Site de pré-lançamento do Adobe](https://www.adobeprerelease.com/) ou [Os clientes da CC Enterprise podem entrar em contato com o Executivo de conta para solicitar uma licença de avaliação de InDesign Server](https://www.adobe.com/products/indesignserver/faq.html)
