---
title: Capítulo 7 - Consumir AEM Content Services de um aplicativo móvel - Serviços de conteúdo
description: O Capítulo 7 do tutorial executa o aplicativo Android Mobile para consumir conteúdo criado AEM Content Services.
translation-type: tm+mt
source-git-commit: 5012433a5f1c7169b1a3996453bfdbd5d78e5b1c
workflow-type: tm+mt
source-wordcount: '1408'
ht-degree: 0%

---


# Capítulo 7 - Consumir AEM Content Services de um aplicativo móvel

O Capítulo 7 do tutorial usa um aplicativo Android Mobile nativo para consumir conteúdo AEM Content Services.

## O aplicativo Android Mobile

Este tutorial usa um **aplicativo Android Mobile nativo simples** para consumir e exibir conteúdo de Eventos exposto pelo AEM Content Services.

O uso do [Android](https://developer.android.com/) é pouco importante, e o aplicativo móvel que consome pode ser gravado em qualquer estrutura para qualquer plataforma móvel, por exemplo, iOS.

O Android é usado para o tutorial devido à capacidade de executar um Emulador Android no Windows, MacOs e Linux, à sua popularidade, e pode ser escrito como Java, uma linguagem bem compreendida pelos desenvolvedores AEM.

*O aplicativo Android Mobile do tutorial ****não tem como objetivo ensinar como criar aplicativos Android Mobile ou transmitir práticas recomendadas de desenvolvimento para Android, mas sim ilustrar como AEM Content Services pode ser consumido de um aplicativo móvel.*

### Como AEM o Content Services orienta a experiência do aplicativo móvel

![Mapeamento de aplicativos móveis para serviços de conteúdo](assets/chapter-7/content-services-mapping.png)

1. O logotipo ****, conforme definido pelo [!DNL Events API] componente de imagem **da página**.
1. A **linha de tag**, conforme definido na [!DNL Events API] página **Componente de texto**.
1. Essa **lista do Evento** é derivada da serialização dos Fragmentos do conteúdo do Evento, expostos pelo **componente de Lista do fragmento do conteúdo** configurado.

## Demonstração do aplicativo móvel

>[!VIDEO](https://video.tv.adobe.com/v/28345/?quality=12&learn=on)

### Configuração do aplicativo móvel para uso fora do host local

Se o AEM Publish não estiver sendo executado em **http://localhost:4503** o host e a porta podem ser atualizados na [!DNL Settings] do aplicativo móvel para apontar para a propriedade host/porta do AEM Publish.

>[!VIDEO](https://video.tv.adobe.com/v/28344/?quality=12&learn=on)

## Execução local do aplicativo móvel

1. Baixe e instale o [Android Studio](https://developer.android.com/studio/install) para instalar o Android Emulator.
1. **** Baixe o  [!DNL APK] arquivo Android  [GitHub > Ativos > wknd-mobile.x.x.xapk](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
1. Abrir **Android Studio**
   * Na inicialização inicial do Android Studio, um prompt para instalar o [!DNL Android SDK] será apresentado. Aceite os padrões e conclua a instalação.
1. Abra o Android Studio e selecione **Perfil ou Depurar APK**
1. Selecione o arquivo APK (**wknd-mobile.x.x.apk**) baixado na Etapa 2 e clique em **OK**
   * Se for solicitado a **Criar uma nova pasta**, ou **Usar existente**, selecione **Usar existente**.
1. Na inicialização inicial do Android Studio, clique com o botão direito do mouse em **wknd-mobile.x.x.x** na lista Projetos e selecione **Abrir configurações do módulo**.
   * Em **Módulos > wknd-mobile.x.x.x > guia Dependências**, selecione **Plataforma Android API 29**. Toque em OK para fechar e salvar as alterações.
   * Se você não fizer isso, verá um erro &quot;Selecione o Android SDK&quot; ao tentar iniciar o emulador.
1. Abra o **Gerenciador AVD** selecionando **Ferramentas > Gerenciador AVD** ou tocando no ícone **Gerenciador AVD** na barra superior.
1. Na janela **AVD Manager**, clique em **+ Criar dispositivo virtual...** se você ainda não tiver o dispositivo registrado.
   1. À esquerda, selecione a categoria **Telefone**.
   1. Selecione um **Pixel 2**.
   1. Clique no botão **Next**.
   1. Selecione **Q** com **API Level 29**.
      * Após a primeira inicialização do AVD Manager, você será solicitado a Baixar a API com versão. Clique no link Download ao lado da versão &quot;Q&quot; e conclua o download e a instalação.
   1. Clique no botão **Next**.
   1. Clique no botão **Concluir**.
1. Feche a janela **AVD Manager**.
1. Na barra de menus superior, selecione **wknd-mobile.x.x** no menu suspenso **Executar/Editar configurações**.
1. Toque no botão **Executar** ao lado da **Executar/Editar Configuração** selecionada
1. No pop-up, selecione o dispositivo virtual **[!DNL Pixel 2 API 29]** recém-criado e toque em **OK**
1. Se o aplicativo [!DNL WKND Mobile] não for carregado imediatamente, localize e toque no ícone **[!DNL WKND]** na tela inicial do Android no emulador.
   * Se o emulador for iniciado, mas a tela do emulador permanecer preta, toque no botão **power** na janela de ferramentas do emulador ao lado da janela do emulador.
   * Para rolar dentro do dispositivo virtual, clique e segure e arraste.
   * Para atualizar o conteúdo do AEM, puxe para baixo até o ícone Atualizar
é exibido e liberado.

>[!VIDEO](https://video.tv.adobe.com/v/28341/?quality=12&learn=on)

## O código do aplicativo móvel

Esta seção destaca o código do aplicativo móvel Android que mais interage e depende AEM Content Services e da saída JSON.

Após a carga, o aplicativo móvel coloca `HTTP GET` em `/content/wknd-mobile/en/api/events.model.json`, que é o ponto final do AEM Content Services configurado para fornecer o conteúdo para direcionar o aplicativo móvel.

Como o Modelo editável da API de Eventos (`/content/wknd-mobile/en/api/events.model.json`) está bloqueado, o aplicativo móvel pode ser codificado para procurar informações específicas em locais específicos na resposta JSON.

### Fluxo de código de alto nível

1. Abrir o aplicativo [!DNL WKND Mobile] chama uma solicitação `HTTP GET` para o AEM Publish em `/content/wknd-mobile/en/api/events.model.json` para coletar o conteúdo para preencher a interface do usuário do aplicativo móvel.
2. Ao receber o conteúdo do AEM, cada um dos três elementos de visualização do aplicativo móvel, o logotipo **linha de tag e lista de evento** são inicializados com o conteúdo do AEM.
   * Para vincular ao conteúdo AEM ao elemento de visualização do aplicativo móvel, o JSON que representa cada componente AEM é um objeto mapeado para um POJO Java, que por sua vez está vinculado ao elemento de Visualização Android.
      * Componente de imagem JSON → POJO de logotipo → ImageView de logotipo
      * Componente de texto JSON → TagLine POJO → ImageView de texto
      * Lista do fragmento do conteúdo JSON → Eventos POJO ÍN Eventos RecicladorView
   * *O código do aplicativo móvel pode mapear o JSON para os POJOs devido aos locais conhecidos na maior resposta JSON. Lembre-se, as teclas JSON de &quot;image&quot;, &quot;text&quot; e &quot;contentfragmentlist&quot; são ditadas pelo backup dos nomes dos nós dos Componentes AEM. Se esses nomes de nó forem alterados, o aplicativo móvel será quebrado, pois não saberá como gerar o conteúdo necessário dos dados JSON.*

#### Invocar o ponto final do AEM Content Services

A seguir está uma destilação do código `MainActivity` do aplicativo móvel responsável por chamar AEM Content Services para coletar o conteúdo que direciona a experiência do aplicativo móvel.

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

`initApp(...)` é então chamada, o que faz com que o GET HTTP solicite ao ponto final AEM Content Services no AEM Publish para coletar o conteúdo. Ao receber uma resposta JSON válida, a resposta JSON é transmitida a cada `ViewBinder` responsável por analisar o JSON e vinculá-lo aos elementos `View` móveis.

#### Analisando a resposta JSON

Em seguida, vamos analisar o `LogoViewBinder`, que é simples, mas destaca várias considerações importantes.

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

A primeira linha de `bind(...)` navega até a Resposta JSON por meio das teclas **:items → root → items → items**, que representa o Container Layout AEM ao qual os componentes foram adicionados.

Aqui, uma verificação é feita para uma chave chamada **image**, que representa o componente de Imagem (novamente, é importante esse nome de nó → A chave JSON é estável). Se esse objeto existir, ele será lido e mapeado para o [POJO de imagem personalizado](#image-pojo) por meio da biblioteca Jackson `ObjectMapper`. O Image POJO é explorado abaixo.

Finalmente, o logotipo `src` é carregado no Android ImageView usando a biblioteca auxiliar [!DNL Glide].

Observe que devemos fornecer o schema AEM, o host e a porta (via `aemHost`) para a instância de publicação de AEM, pois AEM Content Services fornecerá apenas o caminho JCR (ou seja, `/content/dam/wknd-mobile/images/wknd-logo.png`) ao conteúdo referenciado.

#### A imagem POJO{#image-pojo}

Embora opcional, o uso de [Jackson ObjectMapper](https://fasterxml.github.io/jackson-databind/javadoc/2.9/com/fasterxml/jackson/databind/ObjectMapper.html) ou recursos semelhantes fornecidos por outras bibliotecas como Gson ajuda a mapear estruturas JSON complexas para POJOs Java sem o tédio de lidar diretamente com os próprios objetos JSON nativos. Neste caso simples, mapeamos a tecla `src` do objeto JSON `image` para o atributo `src` no POJO de imagem diretamente através da anotação `@JSONProperty`.

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

O POJO do Evento, que requer selecionar muito mais pontos de dados do objeto JSON, se beneficia dessa técnica mais do que a imagem simples, que só queremos que seja o `src`.

## Explore a experiência do aplicativo móvel

Agora que você tem uma ideia de como AEM Content Services pode conduzir a experiência nativa do Mobile, use o que você aprendeu para executar as etapas a seguir e ver suas alterações refletidas no Mobile App.

Após cada etapa, atualize o aplicativo móvel e verifique a atualização da experiência móvel.

1. Criar e publicar **novo [!DNL Event] Fragmento de conteúdo**
1. Cancelar a publicação de um **fragmento de conteúdo [!DNL Event] existente**
1. Publicar uma atualização na **Linha de tag**

## Parabéns

**Você concluiu com o tutorial AEM sem cabeçalho!**

Para saber mais sobre os Serviços de conteúdo e AEM como um CMS sem cabeçalho, visite a documentação do Adobe: outros materiais de habilitação e documentação do Content Services

* [Uso de fragmentos de conteúdo](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/understand-content-fragments-and-experience-fragments.html)
* [Guia do usuário dos componentes principais do AEM WCM](https://docs.adobe.com/content/help/pt-BR/experience-manager-core-components/using/introduction.html)
* [Biblioteca de componentes principais do AEM WCM](https://opensource.adobe.com/aem-core-wcm-components/library.html)
* [AEM Projeto GitHub de Componentes Principais do WCM](https://github.com/adobe/aem-core-wcm-components)
* [Componentes principais do AEM WCM - Pergunte ao especialista](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-content-services.html)
* [Amostra de código do exportador componente](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/bundle/src/main/java/com/adobe/acs/samples/models/SampleComponentExporter.java)
