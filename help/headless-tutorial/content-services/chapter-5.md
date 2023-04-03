---
title: Capítulo 5 - Criação de páginas de serviços de conteúdo - Serviços de conteúdo
description: O Capítulo 5 do tutorial AEM Headless cobre a criação de páginas a partir dos Modelos definidos no Capítulo 4. Essas páginas atuarão como pontos finais HTTP JSON.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: 873d8e69-5a05-44ac-8dae-bba21f82b823
source-git-commit: b3e9251bdb18a008be95c1fa9e5c79252a74fc98
workflow-type: tm+mt
source-wordcount: '596'
ht-degree: 0%

---

# Capítulo 5 - Criação das páginas dos serviços de conteúdo

O Capítulo 5 do tutorial AEM Headless cobre a criação da página com base nos Modelos definidos no Capítulo 4. A página criada neste capítulo atuará como o ponto de extremidade HTTP JSON para o aplicativo móvel.

>[!NOTE]
>
> A arquitetura de conteúdo da página de `/content/wknd-mobile/en/api` O foi pré-criado. As páginas base de `en` e `api` atendem a uma finalidade arquitetônica e organizacional, mas não são estritamente necessárias. Se o conteúdo da API puder ser localizado, é prática recomendada seguir as práticas recomendadas da organização comum da Cópia de idioma e do Gerenciador de vários sites, já que a página da API pode ser localizada como qualquer página do AEM Sites.

## Criação da página da API de evento

1. Navegar para **[!UICONTROL AEM] > [!UICONTROL Sites] > [!DNL WKND Mobile] > [!DNL English] >[!DNL API]**.
1. **Toque no rótulo da página da API** em seguida, toque no **Criar** na barra de ações superior e crie uma nova página de API de eventos na página de API.
   1. Toque **Criar** na barra de ação superior
   1. Selecionar **API Eventos** modelo
   1. No **Nome** campo enter **events**
   1. No **Título** campo enter **API Eventos**
   1. Toque **Criar** na barra de ações superior para criar a página
   1. Toque **Concluído** para retornar ao administrador do AEM Sites

>[!VIDEO](https://video.tv.adobe.com/v/28340?quality=12&learn=on)

## Criação da página da API Eventos

>[!NOTE]
>
> O projeto fornece CSS para fornecer alguns estilos básicos para a experiência do autor.

1. Edite o **API Eventos** navegando até **AEM > Sites > WKND Mobile > Inglês > API**, selecionando o **API Eventos** e tocar em **Editar** na barra de ação superior.
1. Adicione um **imagem do logotipo** para exibir no aplicativo, arraste-o e solte-o do Localizador de ativos no espaço reservado do componente de Imagem .
   * Use o logotipo fornecido em `/content/dam/wknd-mobile/images/wknd-logo.png`.

1. Adicionar **linha de tag** para exibir acima dos eventos.
   1. Edite o **Texto** componente
   1. Insira o texto:
      * `The WKND is here.`

1. Selecione o **events** para exibir:
   1. Defina a seguinte configuração no **Propriedades** guia :
      * Modelo: **Evento**
      * Caminho pai: **/content/dam/wknd-mobile/en/events**
      * Tags: **&lt;leave blank=&quot;&quot;>**
   1. Defina a seguinte configuração no **Elementos** guia :
      * Remova quaisquer elementos listados, para garantir que TODOS os elementos dos Fragmentos do conteúdo do evento sejam expostos.

>[!VIDEO](https://video.tv.adobe.com/v/28339?quality=12&learn=on)

## Revise a saída JSON da página da API

A saída JSON e seu formato podem ser revisados solicitando a página com a variável `.model.json` seletor.

Essa estrutura JSON (ou schema) deve ser bem compreendida pelos consumidores dessa API. É essencial que os consumidores de API entendam quais aspectos da estrutura são fixos (ou seja, o logotipo (imagem) e a tag ao vivo (texto) da API de evento e que são fluidos (ou seja, os eventos listados no componente Lista de fragmentos do conteúdo).

Interromper este contrato com uma API publicada pode resultar em comportamento incorreto nos aplicativos de consumo.

1. Em novas guias do navegador, solicite as páginas da API de eventos usando o `.model.json` seletor, que chama AEM Exportador JSON dos serviços de conteúdo e serializa a Página e os Componentes em uma estrutura JSON normalizada e bem definida.

   A estrutura JSON produzida por essas páginas é a estrutura para a qual os aplicativos que consomem estrutura devem se alinhar.

1. Solicite o **API Eventos** página como **JSON**.

   * [http://localhost:4502/content/wknd-mobile/en/api/events.model.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)

   O resultado deve aparecer de forma semelhante a:

![Saída JSON dos serviços de conteúdo de AEM](assets/chapter-5/json-output.png)

>[!NOTE]
>
> Esse JSON pode ser emitido em um **fenda** Moda (formatada) para legibilidade humana usando o `.tidy` seletor:
> * [http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json](http://localhost:4502/content/wknd-mobile/en/api/events.model.tidy.json)


## Próxima etapa

Como opção, instale a variável [com.adobe.aem.guides.wknd-mobile.content.chapter-5.zip](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest) pacote de conteúdo no AEM Author via [Gerenciador de pacotes de AEM](http://localhost:4502/crx/packmgr/index.jsp). Este pacote contém as configurações e o conteúdo descrito neste e nos capítulos anteriores do tutorial.

* [Capítulo 6 - Exposição do conteúdo na publicação do AEM como JSON](./chapter-6.md)
