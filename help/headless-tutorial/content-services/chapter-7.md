---
title: Capítulo 7 - Consumo dos serviços de conteúdo do AEM a partir de um aplicativo móvel - Serviços de conteúdo
description: O Capítulo 7 do tutorial executa o aplicativo móvel do Android para consumir conteúdo criado do AEM Content Services.
feature: Fragmentos de conteúdo, APIs
topic: Sem periféricos, gerenciamento de conteúdo
role: Desenvolvedor
level: Iniciante
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '1416'
ht-degree: 0%

---


# Capítulo 7 - Consumo dos serviços de conteúdo do AEM a partir de um aplicativo móvel

O Capítulo 7 do tutorial usa um aplicativo móvel Android nativo para consumir conteúdo do AEM Content Services.

## O aplicativo móvel do Android

Este tutorial usa um **aplicativo móvel nativo simples do Android** para consumir e exibir o conteúdo do evento exposto pelos Serviços de conteúdo do AEM.

O uso do [Android](https://developer.android.com/) é pouco importante e o aplicativo móvel que está sendo consumido pode ser gravado em qualquer estrutura para qualquer plataforma móvel, por exemplo, iOS.

O Android é usado para tutorial devido à capacidade de executar um Emulador Android no Windows, macOs e Linux, sua popularidade e por ser gravado como Java, uma linguagem bem compreendida pelos desenvolvedores do AEM.

*O aplicativo móvel Android do tutorial ****não tem como objetivo instruir como criar aplicativos móveis Android ou transmitir práticas recomendadas de desenvolvimento do Android, mas ilustrar como os serviços de conteúdo AEM podem ser consumidos de um aplicativo móvel.*

### Como o AEM Content Services orienta a experiência do aplicativo móvel

![Mapeamento de aplicativos móveis para serviços de conteúdo](assets/chapter-7/content-services-mapping.png)

1. O logotipo **** conforme definido pelo [!DNL Events API] componente de imagem **da página.**
1. A **linha de tag** conforme definido no [!DNL Events API] componente de Texto **da página.**
1. Essa **Lista de eventos** é derivada da serialização dos Fragmentos de conteúdo do evento, expostos por meio do **componente Lista de fragmentos de conteúdo** configurado.

## Demonstração do aplicativo móvel

>[!VIDEO](https://video.tv.adobe.com/v/28345/?quality=12&learn=on)

### Configuração do aplicativo móvel para uso não local

Se AEM Publish não estiver sendo executado em **http://localhost:4503** o host e a porta podem ser atualizados no [!DNL Settings] do Aplicativo móvel para apontar para a propriedade Host/porta de publicação do AEM.

>[!VIDEO](https://video.tv.adobe.com/v/28344/?quality=12&learn=on)

## Execução do aplicativo móvel localmente

1. Baixe e instale o [Android Studio](https://developer.android.com/studio/install) para instalar o Emulador do Android.
1. **** Baixe o  [!DNL APK] arquivo Android  [GitHub > Assets > wknd-mobile.x.x.xapk](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
1. Abra **Android Studio**
   * Na primeira inicialização do Android Studio, um prompt para instalar o [!DNL Android SDK] aparecerá. Aceite os padrões e conclua a instalação.
1. Abra o Android Studio e selecione **Profile ou Debug APK**
1. Selecione o arquivo APK (**wknd-mobile.x.x.apk**) baixado na Etapa 2 e clique em **OK**
   * Se solicitado a **Criar uma nova pasta** ou **Usar existente**, selecione **Usar existente**.
1. Na primeira inicialização do Android Studio, clique com o botão direito do mouse em **wknd-mobile.x.x** na lista Projetos e selecione **Abrir configurações do módulo**.
   * Em **Modules > wknd-mobile.x.x > guia Dependências**, selecione **Plataforma Android API 29**. Toque em OK para fechar e salvar as alterações.
   * Se não fizer isso, você verá um erro &quot;Selecione Android SDK&quot; ao tentar iniciar o emulador.
1. Abra o **Gerenciador AVD** selecionando **Ferramentas > Gerenciador AVD** ou tocando no ícone **Gerenciador AVD** na barra superior.
1. Na janela **AVD Manager**, clique em **+ Criar Dispositivo Virtual...** se você ainda não tiver um dispositivo registrado.
   1. À esquerda, selecione a categoria **Telefone**.
   1. Selecione um **Pixel 2**.
   1. Clique no botão **Next**.
   1. Selecione **Q** com **Nível de API 29**.
      * Na primeira inicialização do AVD Manager, você será solicitado a Baixar a API com versão. Clique no link Download ao lado da versão &quot;Q&quot; e conclua o download e a instalação.
   1. Clique no botão **Next**.
   1. Clique no botão **Finish**.
1. Feche a janela **AVD Manager**.
1. Na barra de menu superior, selecione **wknd-mobile.x.x** no menu suspenso **Executar/Editar configurações**.
1. Toque no botão **Executar** ao lado do **Executar/Editar Configuração** selecionado
1. Na janela pop-up, selecione o dispositivo virtual **[!DNL Pixel 2 API 29]** recém-criado e toque em **OK**
1. Se o aplicativo [!DNL WKND Mobile] não carregar imediatamente, encontre e toque no ícone **[!DNL WKND]** na tela inicial do Android no emulador.
   * Se o emulador for iniciado, mas a tela do emulador permanecer preta, toque no botão **power** na janela de ferramentas do emulador, ao lado da janela do emulador.
   * Para rolar dentro do dispositivo virtual, clique e segure e arraste.
   * Para atualizar o conteúdo do AEM, puxe para baixo a partir da parte superior até o ícone Atualizar
e versão.

>[!VIDEO](https://video.tv.adobe.com/v/28341/?quality=12&learn=on)

## O código do aplicativo móvel

Esta seção destaca o código do aplicativo móvel do Android que mais interage e depende do AEM Content Services e da saída JSON.

Após a carga, o Aplicativo móvel faz `HTTP GET` para `/content/wknd-mobile/en/api/events.model.json`, que é o ponto final do AEM Content Services configurado para fornecer o conteúdo para direcionar o Aplicativo móvel.

Como o Modelo editável da API de eventos (`/content/wknd-mobile/en/api/events.model.json`) está bloqueado, o Aplicativo móvel pode ser codificado para procurar informações específicas em locais específicos na resposta JSON.

### Fluxo de código de alto nível

1. Abrir o [!DNL WKND Mobile] aplicativo chama uma solicitação `HTTP GET` para a Publicação do AEM em `/content/wknd-mobile/en/api/events.model.json` para coletar o conteúdo e preencher a interface do usuário do aplicativo móvel.
2. Ao receber o conteúdo do AEM, cada um dos três elementos de visualização do Aplicativo móvel, o logotipo **, a linha de tag e a lista de eventos**, são inicializados com o conteúdo do AEM.
   * Para se vincular ao conteúdo do AEM ao elemento de exibição do aplicativo móvel, o JSON que representa cada componente do AEM é um objeto mapeado para um POJO Java, que, por sua vez, é vinculado ao elemento de Exibição do Android.
      * Componente de imagem JSON → POJO de logotipo → Imagem do logotipo
      * Componente de texto JSON → TagLine POJO → ImageView de texto
      * Lista de fragmentos de conteúdo JSON → Eventos POJO >Visualização de eventos
   * *O código do aplicativo móvel pode mapear o JSON para os POJOs devido aos locais conhecidos na maior resposta JSON. Lembre-se, as chaves JSON de &quot;imagem&quot;, &quot;texto&quot; e &quot;contentfragmentlist&quot; são ditadas pelos nomes de nó dos Componentes do AEM de backup. Se esses nomes de nó forem alterados, o Aplicativo móvel será interrompido, pois não saberá como extrair o conteúdo necessário dos dados JSON.*

#### Chamar o ponto final dos serviços de conteúdo do AEM

A seguir encontra-se uma destilação do código no `MainActivity` do Aplicativo Móvel responsável por chamar os Serviços de Conteúdo do AEM para coletar o conteúdo que direciona a experiência do Aplicativo Móvel.

```
protected void onCreate(Bundle savedInstanceState) {
    ...
    // Create the custom objects that will map the JSON -> POJO -> View elements
    final List<ViewBinder> viewBinders = new ArrayList<>();

    viewBinders.add(new LogoViewBinder(this, getAemHost(), (ImageView) findViewById(R.id.logo)));
    viewBinders.add(new TagLineViewBinder(this, (TextView) findViewById(R.id.tagLine)));
    viewBinders.add(new EventsViewBinder(this, getAemHost(), (RecyclerView) findViewById(R.id.eventsList)));
    ...
    initApp(viewBinders);
}

private void initApp(final List<ViewBinder> viewBinders) {
    final String aemUrl = getAemUrl(); // -> http://localhost:4503/content/wknd-mobile/en/api/events.mobile.json
    JsonObjectRequest request = new JsonObjectRequest(aemUrl, null,
        new Response.Listener<JSONObject>() {
            @Override
            public void onResponse(JSONObject response) {
                for (final ViewBinder viewBinder : viewBinders) {
                    viewBinder.bind(response);
                }
            }
        },
        ...
    );
}
```

`onCreate(..)` é o gancho de inicialização do aplicativo móvel e registra o 3 personalizado  `ViewBinders` responsável pela análise do JSON e pelo vínculo dos valores aos  `View` elementos.

`initApp(...)` é chamada de , que faz a solicitação HTTP GET para o ponto final do AEM Content Services em AEM Publish para coletar o conteúdo. Ao receber uma Resposta JSON válida, a resposta JSON é passada para cada `ViewBinder` que é responsável por analisar o JSON e vinculá-lo aos elementos móveis `View`.

#### Análise da resposta JSON

Em seguida, analisaremos o `LogoViewBinder`, que é simples, mas destaca várias considerações importantes.

```
public class LogoViewBinder implements ViewBinder {
    ...
    public void bind(JSONObject jsonResponse) throws JSONException, IOException {
        final JSONObject components = jsonResponse.getJSONObject(":items").getJSONObject("root").getJSONObject(":items");

        if (components.has("image")) {
            final Image image = objectMapper.readValue(components.getJSONObject("image").toString(), Image.class);

            final String imageUrl = aemHost + image.getSrc();
            Glide.with(context).load(imageUrl).into(logo);
        } else {
            Log.d("WKND", "Missing Logo");
        }
    }
}
```

A primeira linha de `bind(...)` navega até a Resposta JSON por meio das chaves **:items → root → items** que representa o Contêiner de layout do AEM ao qual os componentes foram adicionados.

Aqui é feita uma verificação para uma chave chamada **image**, que representa o componente Imagem (novamente, é importante esse nome de nó → A chave JSON é estável). Se esse objeto existir, ele será lido e mapeado para o [POJO de imagem personalizado](#image-pojo) por meio da biblioteca Jackson `ObjectMapper`. O Image POJO é explorado abaixo.

Por fim, o logotipo `src` é carregado na ImageView do Android usando a biblioteca auxiliar [!DNL Glide].

Observe que devemos fornecer o esquema, o host e a porta do AEM (via `aemHost`) para a instância de publicação do AEM, pois os Serviços de conteúdo do AEM fornecerão apenas o caminho JCR (ou seja, `/content/dam/wknd-mobile/images/wknd-logo.png`) ao conteúdo referenciado.

#### A imagem POJO{#image-pojo}

Embora seja opcional, o uso do [Jackson ObjectMapper](https://fasterxml.github.io/jackson-databind/javadoc/2.9/com/fasterxml/jackson/databind/ObjectMapper.html) ou recursos semelhantes fornecidos por outras bibliotecas como Gson ajuda a mapear estruturas JSON complexas para POJOs Java sem o tédio de lidar diretamente com os próprios objetos JSON nativos. Nesse caso simples, mapeamos a chave `src` do objeto JSON `image` para o atributo `src` no POJO da imagem diretamente por meio da anotação `@JSONProperty`.

```
package com.adobe.aem.guides.wknd.mobile.android.models;

import com.fasterxml.jackson.annotation.JsonProperty;

public class Image {
    @JsonProperty(value = "src")
    private String src;

    public String getSrc() {
        return src;
    }
}
```

O POJO do evento, que requer selecionar muito mais pontos de dados do objeto JSON, se beneficia dessa técnica mais do que a Imagem simples, que tudo o que queremos é o `src`.

## Explore a experiência do aplicativo móvel

Agora que você tem uma ideia de como os AEM Content Services podem conduzir experiências nativas de dispositivos móveis, use o que aprendeu para executar as etapas a seguir e ver suas alterações refletidas no aplicativo móvel.

Após cada etapa, atualize o Aplicativo móvel e verifique a atualização da experiência móvel.

1. Criar e publicar **novo [!DNL Event] Fragmento de conteúdo**
1. Cancelar a publicação de um **Fragmento de conteúdo [!DNL Event] existente**
1. Publicar uma atualização para a **Linha de Tag**

## Parabéns

**Você concluiu com o tutorial AEM Headless!**

Para saber mais sobre os serviços de conteúdo do AEM e o AEM as a Headless, visite a outra documentação e materiais de ativação da Adobe:

* [Uso de fragmentos de conteúdo](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/understand-content-fragments-and-experience-fragments.html)
* [Guia do usuário dos Componentes principais do WCM AEM](https://docs.adobe.com/content/help/pt-BR/experience-manager-core-components/using/introduction.html)
* [Biblioteca de componentes principais do WCM AEM](https://opensource.adobe.com/aem-core-wcm-components/library.html)
* [Projeto GitHub dos componentes principais do WCM AEM](https://github.com/adobe/aem-core-wcm-components)
* [Componentes principais do WCM AEM - Pergunte ao especialista](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-content-services.html)
* [Amostra de código do exportador de componentes](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/bundle/src/main/java/com/adobe/acs/samples/models/SampleComponentExporter.java)
