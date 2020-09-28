---
title: Introdução ao AEM sem cabeçalho - Capítulo 7 - Consumir AEM Content Services de um aplicativo móvel
description: O Capítulo 7 do tutorial executa o aplicativo Android Mobile para consumir conteúdo criado AEM Content Services.
translation-type: tm+mt
source-git-commit: 22ccd6627a035b37edb180eb4633bc3b57470c0c
workflow-type: tm+mt
source-wordcount: '1404'
ht-degree: 0%

---


# Capítulo 7 - Consumir AEM Content Services de um aplicativo móvel

O Capítulo 7 do tutorial usa um aplicativo Android Mobile nativo para consumir conteúdo AEM Content Services.

## O aplicativo Android Mobile

Este tutorial usa um aplicativo **Android Mobile nativo** simples para consumir e exibir conteúdo de Eventos exposto pelo AEM Content Services.

O uso do [Android](https://developer.android.com/) não é muito importante e o aplicativo móvel que consome pode ser gravado em qualquer estrutura para qualquer plataforma móvel, por exemplo, iOS.

O Android é usado para o tutorial devido à capacidade de executar um Emulador Android no Windows, MacOs e Linux, à sua popularidade, e pode ser escrito como Java, uma linguagem bem compreendida pelos desenvolvedores AEM.

*O aplicativo Android Mobile do tutorial **não**tem como objetivo ensinar como criar aplicativos Android Mobile ou transmitir práticas recomendadas de desenvolvimento para Android, mas ilustrar como AEM Content Services pode ser consumido de um aplicativo móvel.*

### Como AEM o Content Services orienta a experiência do aplicativo móvel

![Mapeamento de aplicativos móveis para serviços de conteúdo](assets/chapter-7/content-services-mapping.png)

1. O **logotipo** , conforme definido pelo componente [!DNL Events API] **Imagem da** página.
1. A linha **da** tag conforme definido no componente [!DNL Events API] **Texto da** página.
1. Esta lista **de** Evento é derivada da serialização dos Fragmentos de conteúdo do Evento, expostos através do componente **de Lista do fragmento de** conteúdo configurado.

## Demonstração do aplicativo móvel

>[!VIDEO](https://video.tv.adobe.com/v/28345/?quality=12&learn=on)

### Configuração do aplicativo móvel para uso fora do host local

Se o AEM Publish não estiver sendo executado em **http://localhost:4503** , o host e a porta podem ser atualizados no aplicativo móvel [!DNL Settings] para apontar para a propriedade host/porta do AEM Publish.

>[!VIDEO](https://video.tv.adobe.com/v/28344/?quality=12&learn=on)

## Execução local do aplicativo móvel

1. Baixe e instale o [Android Studio](https://developer.android.com/studio/install) para instalar o Android Emulator.
1. **Baixe** o arquivo Android [!DNL APK] [GitHub > Ativos > wknd-mobile.x.x.xapk](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
1. Abrir o **Android Studio**
   * Na primeira inicialização do Android Studio, um prompt para instalar o [!DNL Android SDK] será apresentado. Aceite os padrões e conclua a instalação.
1. Abra o Android Studio e selecione **Perfil ou Depurar APK**
1. Selecione o arquivo APK (**wknd-mobile.x.x.apk**) baixado na Etapa 2 e clique em **OK**
   * Se for solicitado a **Criar uma nova pasta** ou **Usar existente**, selecione **Usar existente**.
1. Na inicialização inicial do Android Studio, clique com o botão direito do mouse em **wknd-mobile.x.x** na lista Projetos e selecione **Abrir configurações** do módulo.
   * Em **Módulos > wknd-mobile.x.x > guia** Dependências, selecione Plataforma **** Android API 29. Toque em OK para fechar e salvar as alterações.
   * Se você não fizer isso, verá um erro &quot;Selecione o Android SDK&quot; ao tentar iniciar o emulador.
1. Abra o Gerenciador **de** AVD selecionando **Ferramentas > Gerenciador** de AVD ou tocando no ícone do Gerenciador **de** AVD na barra superior.
1. Na janela **AVD Manager** , clique em **+ Create Virtual Device (Criar dispositivo virtual)...** se você ainda não tiver um dispositivo registrado.
   1. À esquerda, selecione a categoria de **telefone** .
   1. Selecione um **Pixel 2**.
   1. Clique no botão **Avançar** .
   1. Selecione **P** com nível 29 **de** API.
      * Após a primeira inicialização do AVD Manager, você será solicitado a Baixar a API com versão. Clique no link Download ao lado da versão &quot;Q&quot; e conclua o download e a instalação.
   1. Clique no botão **Avançar** .
   1. Clique no botão **Concluir** .
1. Feche a janela **AVD Manager** .
1. Na barra de menus superior, selecione **wknd-mobile.x.x** no menu suspenso **Executar/Editar configurações** .
1. Toque no botão **Executar** ao lado da opção **Executar/Editar configuração selecionada**
1. No pop-up, selecione o dispositivo **[!DNL Pixel 2 API 29]** virtual recém-criado e toque em **OK**
1. Se o [!DNL WKND Mobile] aplicativo não for carregado imediatamente, localize e toque no **[!DNL WKND]** ícone na tela inicial do Android no emulador.
   * Se o emulador for iniciado, mas a tela do emulador permanecer preta, toque no botão **liga/desliga** na janela de ferramentas do emulador ao lado da janela do emulador.
   * Para rolar dentro do dispositivo virtual, clique e segure e arraste.
   * Para atualizar o conteúdo do AEM, puxe para baixo até que o ícone Atualizar seja exibido e solte.

>[!VIDEO](https://video.tv.adobe.com/v/28341/?quality=12&learn=on)

## O código do aplicativo móvel

Esta seção destaca o código do aplicativo móvel Android que mais interage e depende AEM Content Services e da saída JSON.

Quando carregado, o aplicativo móvel atinge `HTTP GET` `/content/wknd-mobile/en/api/events.model.json` o ponto final do AEM Content Services configurado para fornecer o conteúdo para direcionar o aplicativo móvel.

Como o Modelo editável da API de Eventos (`/content/wknd-mobile/en/api/events.model.json`) está bloqueado, o aplicativo móvel pode ser codificado para procurar informações específicas em locais específicos na resposta JSON.

### Fluxo de código de alto nível

1. A abertura do [!DNL WKND Mobile] aplicativo chama uma `HTTP GET` solicitação para o AEM Publish em `/content/wknd-mobile/en/api/events.model.json` para coletar o conteúdo para preencher a interface do usuário do aplicativo móvel.
2. Ao receber o conteúdo do AEM, cada um dos três elementos de visualização do aplicativo móvel, o **logotipo, a linha de tag e a lista** do evento são inicializados com o conteúdo do AEM.
   * Para vincular ao conteúdo AEM ao elemento de visualização do aplicativo móvel, o JSON que representa cada componente AEM é um objeto mapeado para um POJO Java, que por sua vez está vinculado ao elemento de Visualização Android.
      * Componente de imagem JSON → POJO de logotipo → ImageView de logotipo
      * Componente de texto JSON → TagLine POJO → ImageView de texto
      * Lista do fragmento do conteúdo JSON → Eventos POJO ÍN Eventos RecicladorView
   * *O código do aplicativo móvel pode mapear o JSON para os POJOs devido aos locais conhecidos na maior resposta JSON. Lembre-se, as teclas JSON de &quot;image&quot;, &quot;text&quot; e &quot;contentfragmentlist&quot; são ditadas pelo backup dos nomes dos nós dos Componentes AEM. Se esses nomes de nó forem alterados, o aplicativo móvel será quebrado, pois não saberá como gerar o conteúdo necessário dos dados JSON.*

#### Invocar o ponto final do AEM Content Services

A seguir está uma destilação do código no aplicativo móvel `MainActivity` responsável por chamar AEM Content Services para coletar o conteúdo que direciona a experiência do aplicativo móvel.

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

`onCreate(..)` é o gancho de inicialização do aplicativo móvel e registra o 3 personalizado `ViewBinders` responsável pela análise do JSON e pelo vínculo dos valores aos `View` elementos.

`initApp(...)` é então chamada, o que faz com que o GET HTTP solicite ao ponto final AEM Content Services no AEM Publish para coletar o conteúdo. Ao receber uma resposta JSON válida, a resposta JSON é transmitida a cada um `ViewBinder` que é responsável por analisar o JSON e vinculá-lo aos `View` elementos móveis.

#### Analisando a resposta JSON

A seguir vamos olhar para o `LogoViewBinder`, que é simples, mas destaca várias considerações importantes.

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

A primeira linha de `bind(...)` navega até a Resposta JSON por meio das teclas **:items → root → itens** que representam o Container Layout AEM ao qual os componentes foram adicionados.

Aqui, uma verificação é feita para uma chave chamada **imagem**, que representa o componente de Imagem (novamente, é importante esse nome de nó → A chave JSON é estável). Se este objeto existir, ele lê e mapeia para o POJO [de imagem](#image-pojo) personalizado por meio da `ObjectMapper` biblioteca Jackson. O Image POJO é explorado abaixo.

Por fim, o logotipo `src` é carregado no Android ImageView usando a biblioteca [!DNL Glide] auxiliar.

Observe que devemos fornecer o schema AEM, o host e a porta (via `aemHost`) para a instância de publicação de AEM, pois AEM Content Services fornecerá apenas o caminho JCR (ou seja, `/content/dam/wknd-mobile/images/wknd-logo.png`) ao conteúdo referenciado.

#### A imagem POJO{#image-pojo}

Embora opcional, o uso de [Jackson ObjectMapper](https://fasterxml.github.io/jackson-databind/javadoc/2.9/com/fasterxml/jackson/databind/ObjectMapper.html) ou recursos semelhantes fornecidos por outras bibliotecas, como o Gson, ajuda a mapear estruturas JSON complexas para POJOs Java sem o tédio de lidar diretamente com os próprios objetos JSON nativos. Neste caso simples, mapeamos a `src` chave do objeto `image` JSON para o `src` atributo no POJO de imagem diretamente através da `@JSONProperty` anotação.

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

O POJO do Evento, que requer selecionar muito mais pontos de dados do objeto JSON, se beneficia dessa técnica mais do que a Imagem simples, que só queremos `src`.

## Explore a experiência do aplicativo móvel

Agora que você tem uma ideia de como AEM Content Services pode conduzir a experiência nativa do Mobile, use o que você aprendeu para executar as etapas a seguir e ver suas alterações refletidas no Mobile App.

Após cada etapa, atualize o aplicativo móvel e verifique a atualização da experiência móvel.

1. Criar e publicar **novo[!DNL Event]fragmento de conteúdo**
1. Cancelar a publicação de um fragmento de **conteúdo[!DNL Event]existente**
1. Publicar uma atualização na linha de **tag**

## Parabéns

**Você concluiu com o tutorial AEM sem cabeçalho!**

Para saber mais sobre os Serviços de conteúdo e AEM como um CMS sem cabeçalho, visite a documentação do Adobe: outros materiais de habilitação e documentação do Content Services

* [Uso de fragmentos de conteúdo](../sites/content-fragments/understand-content-fragments-and-experience-fragments.md)
* [Guia do usuário dos componentes principais do AEM WCM](https://docs.adobe.com/content/help/pt-BR/experience-manager-core-components/using/introduction.html)
* [Biblioteca de componentes principais do AEM WCM](https://opensource.adobe.com/aem-core-wcm-components/library.html)
* [AEM Projeto GitHub de Componentes Principais do WCM](https://github.com/adobe/aem-core-wcm-components)
* [Componentes principais do AEM WCM - Pergunte ao especialista](https://helpx.adobe.com/experience-manager/kt/eseminars/ask-the-expert/aem-content-services.html)
* [Amostra de código do exportador componente](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/bundle/src/main/java/com/adobe/acs/samples/models/SampleComponentExporter.java)
