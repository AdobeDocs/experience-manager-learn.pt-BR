---
title: Capítulo 5 - Criação de páginas de serviços de conteúdo
description: O Capítulo 5 do tutorial AEM sem cabeçalho aborda a criação de páginas a partir dos Modelos definidos no Capítulo 4. Essas páginas atuarão como pontos finais HTTP JSON.
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '594'
ht-degree: 0%

---


# Capítulo 5 - Criação de páginas de serviços de conteúdo

O Capítulo 5 do tutorial AEM sem cabeçalho cobre a criação da página a partir dos Modelos definidos no Capítulo 4. A página criada neste capítulo atuará como o ponto final HTTP JSON para o aplicativo móvel.

>[!NOTE]
>
> A arquitetura de conteúdo da página do `/content/wknd-mobile/en/api` foi pré-criada. As páginas de base de `en` e servem `api` um propósito arquitetônico e organizacional, mas não são estritamente necessárias. Se o conteúdo da API puder ser localizado, a prática recomendada é seguir as práticas recomendadas de organização da organização da página Cópia de idioma e do Multi-site Manager, já que a página da API pode ser localizada como qualquer página do AEM Sites.

## Criar a página da API do Evento

1. Navegue até **[!UICONTROL AEM]>[!UICONTROL Sites]>[!DNL WKND Mobile]>[!DNL English]>[!DNL API]**.
1. **Toque no rótulo da página** da API, em seguida, toque no botão **Criar** na barra de ação superior e crie uma nova página da API de Eventos na página da API.
   1. Toque em **Criar** na barra de ação superior
   1. Selecionar modelo de API **de** Eventos
   1. No campo **Nome** , informe **eventos**
   1. No campo **Título** , insira a API **Eventos**
   1. Toque em **Criar** na barra de ação superior para criar a página
   1. Toque em **Concluído** para retornar ao administrador do AEM Sites

>[!VIDEO](https://video.tv.adobe.com/v/28340/?quality=12&learn=on)

## Página da API de Eventos

>[!NOTE]
>
> O projeto fornece CSS para fornecer alguns estilos básicos para a experiência do autor.

1. Edite a página da API **de** Eventos navegando até **AEM > Sites > WKND Mobile > Inglês > API**, selecionando a página da API **de** Eventos e tocando em **Editar** na barra de ação superior.
1. Adicione uma imagem **de** logotipo para exibir no aplicativo, arrastando-a e soltando-a do Localizador de ativos no espaço reservado do componente de Imagem.
   * Use o logotipo fornecido encontrado em `/content/dam/wknd-mobile/images/wknd-logo.png`.

1. Adicione uma linha **de** tag para exibir acima dos eventos.
   1. Editar o componente de **Texto**
   1. Insira o texto:
      1. `The WKND is here.`

1. Selecione os **eventos** a serem exibidos:
   1. Defina a seguinte configuração na guia **Propriedades** :
      * Model: **Event**
      * Caminho principal: **/content/dam/wknd-mobile/en/eventos**
      * Tags: **&lt;Deixe em branco>**
   1. Defina a seguinte configuração na guia **Elementos** :
      * Remova todos os elementos listados para garantir que TODOS os elementos dos Fragmentos de conteúdo do Evento sejam expostos.

>[!VIDEO](https://video.tv.adobe.com/v/28339/?quality=12&learn=on)

## Revisar a saída JSON da página da API

A saída JSON e seu formato podem ser revisados solicitando a Página com o `.model.json` seletor.

Essa estrutura JSON (ou schema) deve ser bem entendida pelos consumidores dessa API. É essencial que os consumidores de API entendam quais aspectos da estrutura são fixos (ou seja, o logotipo (imagem) e a tag live (texto) da API do Evento e que são fluidos (ou seja, os eventos listados em Componente de Lista do fragmento do conteúdo).

A quebra deste contrato em uma API publicada pode resultar em comportamento incorreto nos aplicativos de consumo.

1. Em novas guias do navegador, solicite as páginas da API de Eventos usando o `.model.json` seletor, que chama AEM Exportador JSON do Content Services, e serializa a Página e os Componentes em uma estrutura JSON normalizada e bem definida.

   A estrutura JSON produzida por essas páginas é a estrutura à qual os aplicativos que consomem conteúdo devem ser alinhados.

1. Solicite a página da API **de** Eventos como **JSON**.

   * [http://localhost:4502/content/wknd-mobile/en/api/events.model.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

   O resultado deve ser semelhante a:

![Saída JSON do AEM Content Services](assets/chapter-5/json-output.png)

>[!NOTE]
>
> Este JSON pode ser produzido de forma **arrumada** (formatada) para leitura humana usando o `.tidy` seletor:
> * [http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)


## Próxima etapa

Como opção, instale o pacote de conteúdo [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) no AEM Author por meio do [AEM Package Manager](http://localhost:4502/crx/packmgr/index.jsp). Este pacote contém as configurações e o conteúdo descrito neste e nos capítulos anteriores do tutorial.

* [Capítulo 6 - Exposição do conteúdo na publicação do AEM como JSON](./chapter-6.md)
