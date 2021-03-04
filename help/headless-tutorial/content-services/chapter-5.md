---
title: Capítulo 5 - Criação de páginas de serviços de conteúdo - Serviços de conteúdo
description: O Capítulo 5 do tutorial AEM Headless cobre a criação de páginas com base nos Modelos definidos no Capítulo 4. Essas páginas atuarão como pontos finais HTTP JSON.
feature: '"Fragmentos de conteúdo, APIs"'
topic: '"Sem Cabeça, Gerenciamento De Conteúdo"'
role: Desenvolvedor
level: Iniciante
translation-type: tm+mt
source-git-commit: 7d7034026826a5a46a91b6425a5cebfffab2934d
workflow-type: tm+mt
source-wordcount: '604'
ht-degree: 0%

---


# Capítulo 5 - Criação das páginas dos serviços de conteúdo

O Capítulo 5 do tutorial AEM Headless cobre a criação da página com base nos Modelos definidos no Capítulo 4. A página criada neste capítulo atuará como o ponto de extremidade HTTP JSON para o aplicativo móvel.

>[!NOTE]
>
> A arquitetura de conteúdo da página de `/content/wknd-mobile/en/api` foi pré-criada. As páginas base de `en` e `api` atendem a um propósito arquitetônico e organizacional, mas não são estritamente necessárias. Se o conteúdo da API puder ser localizado, é prática recomendada seguir as práticas recomendadas da organização comum da Cópia de idioma e do Gerenciador de vários sites, já que a página da API pode ser localizada como qualquer página do AEM Sites.

## Criação da página da API de evento

1. Navegue até **[!UICONTROL AEM] > [!UICONTROL Sites] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**.
1. **Toque no rótulo da página** da API, em seguida, toque no  **** botão Criar na barra de ação superior e crie uma nova página da API de eventos na página da API.
   1. Toque em **Criar** na barra de ações superior
   1. Selecione o modelo **API de eventos**
   1. No campo **Nome**, digite **eventos**
   1. No campo **Título**, digite **API de Eventos**
   1. Toque em **Criar** na barra de ação superior para criar a página
   1. Toque em **Concluído** para retornar ao administrador do AEM Sites

>[!VIDEO](https://video.tv.adobe.com/v/28340/?quality=12&learn=on)

## Criação da página da API Eventos

>[!NOTE]
>
> O projeto fornece CSS para fornecer alguns estilos básicos para a experiência do autor.

1. Edite a página **API de eventos** navegando até **AEM > Sites > WKND Mobile > English > API**, selecionando a página **API de eventos** e tocando em **Editar** na barra de ação superior.
1. Adicione uma **imagem de logotipo** para exibir no aplicativo arrastando-a e soltando-a do Localizador de ativos no espaço reservado do componente de Imagem .
   * Use o logotipo fornecido encontrado em `/content/dam/wknd-mobile/images/wknd-logo.png`.

1. Adicione **linha de tag** para exibir acima dos eventos.
   1. Editar o componente **Texto**
   1. Insira o texto:
      * `The WKND is here.`

1. Selecione **events** para exibir:
   1. Defina a seguinte configuração na guia **Properties**:
      * Modelo: **Evento**
      * Caminho pai: **/content/dam/wknd-mobile/en/events**
      * Tags: **&lt;Deixe em branco>**
   1. Defina a seguinte configuração na guia **Elements**:
      * Remova quaisquer elementos listados, para garantir que TODOS os elementos dos Fragmentos do conteúdo do evento sejam expostos.

>[!VIDEO](https://video.tv.adobe.com/v/28339/?quality=12&learn=on)

## Revise a saída JSON da página da API

A saída JSON e seu formato podem ser revisados solicitando a Página com o seletor `.model.json`.

Essa estrutura JSON (ou schema) deve ser bem compreendida pelos consumidores dessa API. É essencial que os consumidores de API entendam quais aspectos da estrutura são fixos (ou seja, o logotipo (imagem) e a tag ao vivo (texto) da API de evento e que são fluidos (ou seja, os eventos listados no componente Lista de fragmentos do conteúdo).

Interromper este contrato com uma API publicada pode resultar em comportamento incorreto nos aplicativos de consumo.

1. Em novas guias do navegador, solicite as páginas da API de eventos usando o seletor `.model.json`, que chama o Exportador JSON dos Serviços de conteúdo do AEM, e serializa a Página e os Componentes em uma estrutura JSON normalizada e bem definida.

   A estrutura JSON produzida por essas páginas é a estrutura para a qual os aplicativos que consomem estrutura devem se alinhar.

1. Solicite a página **API de eventos** como **JSON**.

   * [http://localhost:4502/content/wknd-mobile/en/api/events.model.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

   O resultado deve aparecer de forma semelhante a:

![Saída JSON do AEM Content Services](assets/chapter-5/json-output.png)

>[!NOTE]
>
> Esse JSON pode ser emitido de maneira **programar** (formatado) para legibilidade humana usando o seletor `.tidy`:
> * [http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)


## Próxima etapa

Como opção, instale o pacote de conteúdo [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) no AEM Author por meio do [Gerenciador de pacotes do AEM](http://localhost:4502/crx/packmgr/index.jsp). Este pacote contém as configurações e o conteúdo descrito neste e nos capítulos anteriores do tutorial.

* [Capítulo 6 - Exposição do conteúdo na publicação do AEM como JSON](./chapter-6.md)
