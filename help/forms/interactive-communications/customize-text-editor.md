---
title: Personalizar editor de texto
description: Saiba como personalizar o editor de texto.
doc-type: article
version: 6.5
topic: Development
role: Developer
level: Beginner
feature: Interactive Communication
last-substantial-update: 2023-04-19T00:00:00Z
jira: KT-13126
exl-id: e551ac8d-0bfc-4c94-b773-02ff9bba202e
duration: 139
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 0%

---

# Personalizar editor de texto{#customize-text-editor}

## Visão geral {#overview}

É possível personalizar o editor de texto usado para criar fragmentos de documento para adicionar mais fontes e tamanhos de fonte. Essas fontes incluem as fontes em inglês e não em inglês, como japonês.

Você pode personalizar o para alterar o seguinte nas configurações de fonte:

* Família e tamanho da fonte
* Propriedades como altura e espaçamento entre letras
* Valores padrão de família de fontes e tamanho, altura, espaçamento entre letras e formato de data
* Recuos com marcadores

Para fazer isso, é necessário:

1. [Personalizar fontes editando o arquivo tbxeditor-config.xml no CRX](#customizefonts)
1. [Adicionar fontes personalizadas ao computador cliente](#addcustomfonts)

## Personalizar fontes editando o arquivo tbxeditor-config.xml no CRX {#customizefonts}

Para personalizar fontes editando o arquivo tbxeditor-config.xml, faça o seguinte:

1. Vá para `https://'[server]:[port]'/[ContextPath]/crx/de` e faça logon como Administrador.
1. Na pasta apps, crie uma pasta chamada config com caminho/estrutura semelhante à pasta config, que está em libs/fd/cm/config, usando as seguintes etapas:

   1. Clique com o botão direito do mouse na pasta de itens no seguinte caminho e selecione **Sobrepor Nó**:

      `/libs/fd/cm/config`

      ![Sobrepor nó](assets/overlay.png)

   1. Certifique-se de que a caixa de diálogo Sobrepor nó tenha os seguintes valores:

      **Caminho:** /libs/fd/cm/config

      **Local:** /apps/

      **Corresponder Tipos De Nó:** Selecionados

      ![Sobrepor nó](assets/overlay1.png)

   1. Clique em **OK**. A estrutura de pastas é criada na pasta de aplicativos.

   1. Clique em **Salvar tudo**.

1. Crie uma cópia do arquivo tbxeditor-config.xml na pasta de configuração recém-criada, usando as seguintes etapas:

   1. Clique com o botão direito no arquivo tbxeditor-config.xml em libs/fd/cm/config e selecione **Copiar**.
   1. Clique com o botão direito do mouse na seguinte pasta e selecione **Colar:**

      `apps/fd/cm/config`

   1. O nome do arquivo colado, por padrão, é `copy of tbxeditor-config.xml.` Renomeie o arquivo para `tbxeditor-config.xml` e clique em **Salvar Tudo**.

1. Abra o arquivo tbxeditor-config.xml em apps/fd/cm/config e faça as alterações necessárias.

   1. Clique duas vezes no arquivo tbxeditor-config.xml em apps/fd/cm/config. O arquivo é aberto.

      ```xml
      <editorConfig>
         <bulletIndent>0.25in</bulletIndent>
      
         <defaultDateFormat>DD-MM-YYYY</defaultDateFormat>
      
         <fonts>
            <default>Times New Roman</default>
            <font>_sans</font>
            <font>_serif</font>
            <font>_typewriter</font>
            <font>Arial</font>
            <font>Courier</font>
            <font>Courier New</font>
            <font>Geneva</font>
            <font>Georgia</font>
            <font>Helvetica</font>
            <font>Tahoma</font>
            <font>Times New Roman</font>
            <font>Times</font>
            <font>Verdana</font>
         </fonts>
      
         <fontSizes>
            <default>12</default>
            <fontSize>8</fontSize>
            <fontSize>9</fontSize>
            <fontSize>10</fontSize>
            <fontSize>11</fontSize>
            <fontSize>12</fontSize>
            <fontSize>14</fontSize>
            <fontSize>16</fontSize>
            <fontSize>18</fontSize>
            <fontSize>20</fontSize>
            <fontSize>22</fontSize>
            <fontSize>24</fontSize>
            <fontSize>26</fontSize>
            <fontSize>28</fontSize>
            <fontSize>36</fontSize>
            <fontSize>48</fontSize>
            <fontSize>72</fontSize>
         </fontSizes>
      
         <lineHeights>
            <default>2</default>     
            <lineHeight>2</lineHeight>
            <lineHeight>3</lineHeight>
            <lineHeight>4</lineHeight>
            <lineHeight>5</lineHeight>
            <lineHeight>6</lineHeight>
            <lineHeight>7</lineHeight>
            <lineHeight>8</lineHeight>
            <lineHeight>9</lineHeight>
            <lineHeight>10</lineHeight>
            <lineHeight>11</lineHeight>
            <lineHeight>12</lineHeight>
            <lineHeight>13</lineHeight>
            <lineHeight>14</lineHeight>
            <lineHeight>15</lineHeight>
            <lineHeight>16</lineHeight>
         </lineHeights>
      
         <letterSpacings>
            <default>0</default>
            <letterSpacing>0</letterSpacing>
            <letterSpacing>1</letterSpacing>
            <letterSpacing>2</letterSpacing>
            <letterSpacing>3</letterSpacing>
            <letterSpacing>4</letterSpacing>
            <letterSpacing>5</letterSpacing>
            <letterSpacing>6</letterSpacing>
            <letterSpacing>7</letterSpacing>
            <letterSpacing>8</letterSpacing>
            <letterSpacing>9</letterSpacing>
            <letterSpacing>10</letterSpacing>
            <letterSpacing>11</letterSpacing>
            <letterSpacing>12</letterSpacing>
            <letterSpacing>13</letterSpacing>
            <letterSpacing>14</letterSpacing>
            <letterSpacing>15</letterSpacing>
            <letterSpacing>16</letterSpacing>
         </letterSpacings>
      </editorConfig>
      ```

   1. Faça as alterações necessárias no arquivo para alterar o seguinte nas configurações de fonte:

      * Adicionar ou remover a família e o tamanho da fonte
      * Propriedades como altura e espaçamento entre letras
      * Valores padrão de família de fontes e tamanho, altura, espaçamento entre letras e formato de data
      * Recuos com marcadores

      Por exemplo, para adicionar uma fonte em japonês chamada Sazanami Mincho Medium, é necessário criar a seguinte entrada no arquivo XML: `<font>Sazanami Mincho Medium</font>`. Também é necessário ter essa fonte instalada na máquina cliente usada para acessar e trabalhar com a personalização de fontes. Para obter mais informações, consulte [Adicionar fontes personalizadas ao computador cliente](#addcustomfonts).

      Também é possível alterar os padrões de vários aspectos do texto e, removendo as entradas, remover as fontes do editor de texto.

   1. Clique em **Salvar tudo**.

## Adicionar fontes personalizadas ao computador cliente {#addcustomfonts}

Quando você acessa uma fonte no editor de texto de Comunicações interativas, ela precisa estar presente na máquina cliente que você está usando para acessar a Comunicação interativa. Para poder usar uma fonte personalizada no editor de texto, primeiro é necessário instalar a mesma na máquina cliente.

Para obter mais informações sobre a instalação de fontes, consulte o seguinte:

* [Instalar ou desinstalar fontes no Windows](https://windows.microsoft.com/en-us/windows-vista/install-or-uninstall-fonts)
* [Noções básicas do Mac: Livro de fontes](https://support.apple.com/en-us/HT201749)

## Acessar personalizações de fonte {#access-font-customizations}

Depois de fazer alterações nas fontes no arquivo tbxeditor-config.xml no CRX e instalar as fontes necessárias na máquina cliente usada para acessar o AEM Forms, as alterações serão exibidas no editor de texto.

Por exemplo, a fonte Sazanami Mincho Medium adicionada ao [Personalizar fontes editando o arquivo tbxeditor-config.xml no procedimento CRX](#customizefonts) aparece na interface do editor de texto da seguinte maneira:

![sazanamiminchointext](assets/sazanamiminchointext.png)

>[!NOTE]
>
>Para ver o texto em japonês, primeiro é necessário inserir o texto com caracteres japoneses. A aplicação de uma fonte japonesa personalizada só formata o texto de uma determinada maneira. A aplicação de uma fonte japonesa personalizada não altera o inglês ou outros caracteres para caracteres japoneses.
