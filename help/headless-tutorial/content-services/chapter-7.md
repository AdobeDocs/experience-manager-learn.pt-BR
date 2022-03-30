---
title: Capítulo 7 - Consumo AEM serviços de conteúdo de um aplicativo Mobile - Serviços de conteúdo
description: O Capítulo 7 do tutorial executa o aplicativo Android Mobile para consumir conteúdo criado AEM Content Services.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
exl-id: d6b6d425-842a-43a9-9041-edf78e51d962
source-git-commit: 631fef25620c84e04c012c8337c9b76613e3ad46
workflow-type: tm+mt
source-wordcount: '1392'
ht-degree: 0%

---

# Capítulo 7 - Consumo AEM serviços de conteúdo de um aplicativo Mobile

O Capítulo 7 do tutorial usa um aplicativo Android Mobile nativo para consumir conteúdo AEM Serviços de conteúdo.

## O aplicativo Android Mobile

Este tutorial usa um **aplicativo Mobile nativo simples** para consumir e exibir o conteúdo do Evento exposto pelos Serviços de conteúdo AEM.

O uso de [Android](https://developer.android.com/) O é pouco importante e o aplicativo móvel que está sendo consumido pode ser escrito em qualquer estrutura para qualquer plataforma móvel, por exemplo, iOS.

O Android é usado para tutorial devido à capacidade de executar um Emulador Android no Windows, macOs e Linux, sua popularidade e por ser gravado como Java, uma linguagem bem compreendida pelos desenvolvedores AEM.

*O aplicativo Android Mobile do tutorial é&#x200B;**not**destinado a instruir como criar aplicativos Android Mobile ou transmitir práticas recomendadas de desenvolvimento do Android, mas ilustrar como os Serviços de conteúdo AEM podem ser consumidos de um aplicativo Mobile.*

### Como os AEM Content Services conduzem a experiência do aplicativo Mobile

![Mapeamento de aplicativos do Mobile para serviços de conteúdo](assets/chapter-7/content-services-mapping.png)

1. O **logotipo** conforme definido pelo [!DNL Events API] da página **Componente de imagem**.
1. O **linha de tag** conforme definido no [!DNL Events API] da página **Componente de texto**.
1. Essa **Lista de eventos** é derivada da serialização dos Fragmentos do conteúdo do evento, expostos por meio do **Componente da lista de fragmentos do conteúdo**.

## Demonstração do aplicativo Mobile

>[!VIDEO](https://video.tv.adobe.com/v/28345/?quality=12&learn=on)

### Configuração do aplicativo Mobile para uso não local

Se a publicação do AEM não estiver sendo executada em **http://localhost:4503** o host e a porta podem ser atualizados no aplicativo Mobile [!DNL Settings] aponte para a propriedade AEM Publish host/port.

>[!VIDEO](https://video.tv.adobe.com/v/28344/?quality=12&learn=on)

## Execução do aplicativo Mobile localmente

1. Baixe e instale o [Android Studio](https://developer.android.com/studio/install) para instalar o Emulador do Android.
1. **Baixar** o Android [!DNL APK] arquivo [GitHub > Ativos > wknd-mobile.x.x.xapk](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
1. Abrir **Android Studio**
   * Na primeira inicialização do Android Studio, um prompt para instalar o [!DNL Android SDK] estará presente. Aceite os padrões e conclua a instalação.
1. Abra o Android Studio e selecione **Perfil ou Depuração APK**
1. Selecione o arquivo APK (**wknd-mobile.x.x.apk**) baixado na Etapa 2 e clique em **OK**
   * Se solicitado a **Criar uma nova pasta** ou **Usar existente**, selecione **Usar existente**.
1. Na primeira inicialização do Android Studio, clique com o botão direito do mouse no **wknd-mobile.x.x** na lista Projetos e selecione **Abrir configurações do módulo**.
   * Em **Módulos > wknd-mobile.x.x > guia Dependências**, selecione **Plataforma Android API 29**. Toque em OK para fechar e salvar as alterações.
   * Se não fizer isso, você verá um erro &quot;Selecione Android SDK&quot; ao tentar iniciar o emulador.
1. Abra o **Gerenciador de AVD** selecionando **Ferramentas > Gerenciador de AVD** ou tocar no **Gerenciador de AVD** na barra superior.
1. No **Gerenciador de AVD** , clique em **+ Criar Dispositivo Virtual...** se você ainda não tiver o dispositivo registrado.
   1. À esquerda, selecione o **Telefone** categoria .
   1. Selecione um **Pixel 2**.
   1. Clique no botão **Próximo** botão.
   1. Selecionar **Q** com **Nível de API 29**.
      * Na primeira inicialização do AVD Manager, você será solicitado a Baixar a API com versão. Clique no link Download ao lado da versão &quot;Q&quot; e conclua o download e a instalação.
   1. Clique no botão **Próximo** botão.
   1. Clique no botão **Concluir** botão.
1. Feche o **Gerenciador de AVD** janela.
1. Na barra do menu superior, selecione **wknd-mobile.x.x** do **Executar/editar configurações** menu suspenso.
1. Toque no **Executar** ao lado do botão selecionado **Executar/Editar configuração**
1. Na janela pop-up , selecione o arquivo recém-criado **[!DNL Pixel 2 API 29]** dispositivo virtual e toque **OK**
1. Se a variável [!DNL WKND Mobile] o aplicativo não carrega, encontra e toca imediatamente no **[!DNL WKND]** ícone na tela inicial do Android no emulador.
   * Se o emulador for iniciado, mas a tela do emulador permanecer preta, toque no **potência** na janela de ferramentas do emulador, ao lado da janela do emulador.
   * Para rolar dentro do dispositivo virtual, clique e segure e arraste.
   * Para atualizar o conteúdo do AEM, puxe para baixo a partir da parte superior até que o ícone Atualizar seja exibido e solte.

>[!VIDEO](https://video.tv.adobe.com/v/28341/?quality=12&learn=on)

## O código do aplicativo Mobile

Esta seção destaca o código de aplicativo Mobile do Android que mais interage e depende AEM serviços de conteúdo e da saída JSON.

Após o carregamento, o aplicativo Mobile faz `HTTP GET` para `/content/wknd-mobile/en/api/events.model.json` que é o ponto final AEM Content Services configurado para fornecer o conteúdo para direcionar o aplicativo Mobile.

Como o modelo editável da API de eventos (`/content/wknd-mobile/en/api/events.model.json`) estiver bloqueado, o aplicativo Mobile pode ser codificado para procurar informações específicas em locais específicos na resposta JSON.

### Fluxo de código de alto nível

1. Abrir o [!DNL WKND Mobile] O aplicativo chama um `HTTP GET` para publicar no AEM em `/content/wknd-mobile/en/api/events.model.json` para coletar o conteúdo para preencher a interface do usuário do aplicativo Mobile.
2. Ao receber o conteúdo do AEM, cada um dos três elementos de visualização do aplicativo Mobile, a variável **logotipo, linha de tag e lista de eventos**, são inicializadas com o conteúdo do AEM.
   * Para se vincular ao conteúdo AEM ao elemento de exibição do aplicativo Mobile, o JSON que representa cada componente AEM é um objeto mapeado para um Java POJO, que, por sua vez, é vinculado ao elemento de Exibição Android.
      * Componente de imagem JSON → POJO de logotipo → Imagem do logotipo
      * Componente de texto JSON → TagLine POJO → ImageView de texto
      * Lista de fragmentos de conteúdo JSON → Eventos POJO >Visualização de eventos
   * *O código de aplicativo do Mobile é capaz de mapear o JSON para os POJOs devido aos locais conhecidos na maior resposta JSON. Lembre-se, as chaves JSON de &quot;imagem&quot;, &quot;texto&quot; e &quot;contentfragmentlist&quot; são ditadas pelo backup AEM nomes de nó dos Componentes. Se esses nomes de nó forem alterados, o aplicativo do Mobile será interrompido, pois não saberá como extrair o conteúdo necessário dos dados JSON.*

#### Chamar o ponto final dos serviços de conteúdo AEM

Veja a seguir uma destilação do código no aplicativo do Mobile `MainActivity` responsável por chamar AEM Content Services para coletar o conteúdo que direciona a experiência do aplicativo Mobile.

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

`onCreate(..)` é o gancho de inicialização do aplicativo Mobile e registra o 3 personalizado `ViewBinders` responsável por analisar o JSON e vincular os valores ao `View` elementos.

`initApp(...)` é chamada de , que faz com que a solicitação HTTP GET para o ponto final AEM Content Services na AEM Publish colete o conteúdo. Ao receber uma resposta JSON válida, a resposta JSON é passada para cada `ViewBinder` responsável por analisar o JSON e vinculá-lo ao dispositivo móvel `View` elementos.

#### Análise da resposta JSON

Em seguida, observaremos o `LogoViewBinder`, que é simples, mas destaca várias considerações importantes.

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

A primeira linha de `bind(...)` navega para baixo na Resposta JSON por meio das chaves **:itens → raiz → itens** que representa o Contêiner de layout de AEM ao qual os componentes foram adicionados.

Aqui é feita uma verificação para uma chave chamada **imagem**, que representa o componente Imagem (novamente, é importante esse nome de nó → A chave JSON é estável). Se este objeto existir, ele será lido e mapeado para a variável [POJO de imagem personalizada](#image-pojo) via Jackson `ObjectMapper` biblioteca. O Image POJO é explorado abaixo.

Finalmente, o logotipo `src` é carregada no Android ImageView usando o [!DNL Glide] biblioteca auxiliar.

Observe que devemos fornecer o esquema de AEM, o host e a porta (via `aemHost`) para a instância de publicação do AEM, pois AEM Content Services fornecerá apenas o caminho JCR (ou seja, `/content/dam/wknd-mobile/images/wknd-logo.png`) ao conteúdo referenciado.

#### A imagem POJO{#image-pojo}

Embora seja opcional, o uso da variável [Jackson ObjectMapper](https://fasterxml.github.io/jackson-databind/javadoc/2.9/com/fasterxml/jackson/databind/ObjectMapper.html) ou recursos semelhantes fornecidos por outras bibliotecas, como o Gson, ajudam a mapear estruturas JSON complexas para POJOs Java sem o tédio de lidar diretamente com os próprios objetos JSON nativos. Nesse caso simples, mapeamos a variável `src` chave do `image` objeto JSON, para a variável `src` atributo no POJO da imagem diretamente através do `@JSONProperty` anotação.

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

O POJO do evento, que requer selecionar muito mais pontos de dados do objeto JSON, se beneficia dessa técnica mais do que a Imagem simples, que só queremos é a `src`.

## Explore a experiência do aplicativo Mobile

Agora que você tem uma compreensão de como os Serviços de conteúdo AEM podem impulsionar a experiência nativa do Mobile, use o que aprendeu para executar as etapas a seguir e ver suas alterações refletidas no aplicativo Mobile.

Após cada etapa, atualize o aplicativo Mobile e verifique a atualização da experiência móvel.

1. Criar e publicar **novo [!DNL Event] Fragmento de conteúdo**
1. Cancelar a publicação de um **existente [!DNL Event] Fragmento de conteúdo**
1. Publicar uma atualização no **Linha de tag**

## Parabéns

**Você concluiu com o tutorial AEM Headless!**

Para saber mais sobre os Serviços de conteúdo e o AEM como um CMS sem cabeçalho, visite a documentação do Adobe e os materiais de ativação:

* [Uso de fragmentos de conteúdo](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/understand-content-fragments-and-experience-fragments.html)
* [Guia do usuário dos Componentes principais do WCM AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=pt-BR)
* [Biblioteca de componentes principais do WCM AEM](https://opensource.adobe.com/aem-core-wcm-components/library.html)
* [AEM Projeto GitHub dos Componentes principais do WCM](https://github.com/adobe/aem-core-wcm-components)
* [Amostra de código do exportador de componentes](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleComponentExporter.java)
