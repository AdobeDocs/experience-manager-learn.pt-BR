---
title: Capítulo 7 - Consumir serviços de conteúdo AEM de um aplicativo móvel - Serviços de conteúdo
description: O capítulo 7 do tutorial executa o aplicativo móvel Android para consumir conteúdo criado pelo AEM Content Services.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: d6b6d425-842a-43a9-9041-edf78e51d962
duration: 512
source-git-commit: 9fef4b77a2c70c8cf525d42686f4120e481945ee
workflow-type: tm+mt
source-wordcount: '1350'
ht-degree: 0%

---

# Capítulo 7 - Consumir serviços de conteúdo de AEM de um aplicativo móvel

O capítulo 7 do tutorial usa um aplicativo móvel Android nativo para consumir conteúdo do AEM Content Services.

## O aplicativo móvel Android

Este tutorial usa um **aplicativo móvel Android nativo simples** para consumir e exibir o conteúdo do Evento exposto pelo AEM Content Services.

A utilização de [Android](https://developer.android.com/) O não é importante, e o aplicativo móvel de consumo pode ser escrito em qualquer estrutura para qualquer plataforma móvel, por exemplo, iOS.

O Android é usado como tutorial devido à capacidade de executar um emulador Android no Windows, macOs e Linux, sua popularidade e que pode ser escrito como Java, uma linguagem bem compreendida pelos desenvolvedores do AEM.

*O aplicativo móvel Android do tutorial é&#x200B;**não**O objetivo era instruir como criar aplicativos móveis Android ou transmitir práticas recomendadas de desenvolvimento do Android, mas sim ilustrar como os Serviços de conteúdo AEM podem ser consumidos de um aplicativo móvel.*

### Como o AEM Content Services orienta a experiência do aplicativo móvel

![Aplicativo móvel para mapeamento de serviços de conteúdo](assets/chapter-7/content-services-mapping.png)

1. A variável **logotipo** conforme definido pela [!DNL Events API] da página **Componente de imagem**.
1. A variável **linha da tag** conforme definido no [!DNL Events API] da página **Componente de texto**.
1. Este **Lista de eventos** é derivado da serialização dos Fragmentos de conteúdo do evento, exposto por meio do **Componente da lista de fragmentos do conteúdo**.

## Demonstração do aplicativo móvel

>[!VIDEO](https://video.tv.adobe.com/v/28345?quality=12&learn=on)

### Configuração do aplicativo móvel para uso fora do host local

Se a publicação do AEM não estiver sendo executada no **http://localhost:4503** o host e a porta podem ser atualizados no campo [!DNL Settings] para apontar para a propriedade AEM Publish host/port.

>[!VIDEO](https://video.tv.adobe.com/v/28344?quality=12&learn=on)

## Execução local do aplicativo móvel

1. Baixe e instale o [Android Studio](https://developer.android.com/studio/install) para instalar o emulador de Android.
1. **Baixar** o Android [!DNL APK] arquivo [GitHub > Ativos > wknd-mobile.x.x.xapk](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
1. Abertura **Android Studio**
   * Na primeira inicialização do Android Studio, um prompt para instalar o [!DNL Android SDK] apresentará. Aceite os padrões e finalize a instalação.
1. Abra o Android Studio e selecione **Perfil ou depurar APK**
1. Selecione o arquivo APK (**wknd-mobile.x.x.x.apk**) baixado na Etapa 2 e clique em **OK**
   * Se solicitado a **Criar uma nova pasta** ou **Usar existente**, selecione **Usar existente**.
1. Na primeira inicialização do Android Studio, clique com o botão direito do mouse no **wknd-mobile.x.x.x** na lista Projetos e selecione **Abrir configurações do módulo**.
   * Em **Módulos > wknd-mobile.x.x.x > guia Dependências**, selecione **Plataforma Android API 29**. Toque em OK para fechar e salvar as alterações.
   * Se você não fizer isso, verá um erro &quot;Selecione Android SDK&quot; ao tentar iniciar o emulador.
1. Abra o **Gerenciador AVD** selecionando **Ferramentas > Gerenciador AVD** ou tocando no **Gerenciador AVD** ícone na barra superior.
1. No **Gerenciador AVD** clique em **+ Criar Dispositivo Virtual...** se você ainda não tiver um dispositivo registrado.
   1. À esquerda, selecione o **Telefone** categoria.
   1. Selecione um **Pixel 2**.
   1. Clique em **Próxima** botão.
   1. Selecionar **Q** com **Nível de API 29**.
      * Após a primeira inicialização do AVD Manager, você será solicitado a Baixar a API versionada. Clique no link Download ao lado da versão &quot;Q&quot; e conclua o download e a instalação.
   1. Clique em **Próxima** botão.
   1. Clique em **Concluir** botão.
1. Feche o **Gerenciador AVD** janela.
1. Na barra de menu superior, selecione **wknd-mobile.x.x.x** do **Executar/Editar configurações** lista suspensa.
1. Toque no **Executar** ao lado do botão selecionado **Executar/Editar configuração**
1. Na janela pop-up, selecione o recém-criado **[!DNL Pixel 2 API 29]** dispositivo virtual e toque **OK**
1. Se a variável [!DNL WKND Mobile] O aplicativo não carrega imediatamente, localiza e toque no **[!DNL WKND]** ícone na tela inicial do Android no emulador.
   * Se o emulador for iniciado, mas a tela do emulador permanecer preta, toque no **alimentação** botão na janela de ferramentas do emulador próximo à janela do emulador.
   * Para rolar dentro do dispositivo virtual, clique e segure e arraste.
   * Para atualizar o conteúdo do AEM, puxe-o para baixo desde a parte superior até que o ícone Atualizar seja exibido e solte-o.

>[!VIDEO](https://video.tv.adobe.com/v/28341?quality=12&learn=on)

## O código do aplicativo móvel

Esta seção destaca o código de aplicativo móvel do Android que mais interage e depende do AEM Content Services e da saída JSON.

Após o carregamento, o aplicativo móvel faz `HTTP GET` para `/content/wknd-mobile/en/api/events.model.json` que é o ponto de acesso do AEM Content Services configurado para fornecer o conteúdo que direciona o aplicativo móvel.

Como o modelo editável da API de eventos (`/content/wknd-mobile/en/api/events.model.json`) estiver bloqueado, o aplicativo móvel poderá ser codificado para procurar informações específicas em locais específicos na resposta JSON.

### Fluxo de código de alto nível

1. Abrir o [!DNL WKND Mobile] O aplicativo chama um `HTTP GET` solicitação para o AEM Publicar em `/content/wknd-mobile/en/api/events.model.json` para coletar o conteúdo e preencher a interface do usuário do aplicativo móvel.
2. Ao receber o conteúdo do AEM, cada um dos três elementos de visualização do aplicativo móvel, o **logotipo, linha de tag e lista de eventos**, são inicializados com o conteúdo do AEM.
   * Para vincular ao conteúdo AEM ao elemento de visualização do aplicativo móvel, o JSON que representa cada componente AEM é o objeto mapeado para um POJO Java, que por sua vez é vinculado ao elemento de visualização do Android.
      * Componente de Imagem JSON → POJO de logotipo → Visualização de imagem de logotipo
      * Componente de Texto JSON → POJO de tag → ImageView de texto
      * Lista de fragmentos de conteúdo JSON → Eventos POJO → Eventos RecyclerView
   * *O código do aplicativo móvel pode mapear o JSON para os POJOs devido aos locais bem conhecidos na maior resposta JSON. Lembre-se de que as chaves JSON de &quot;imagem&quot;, &quot;texto&quot; e &quot;contentfragmentlist&quot; são ditadas pelos nomes de nó dos componentes AEM de apoio. Se esses nomes de nó forem alterados, o aplicativo móvel será interrompido, pois não saberá como obter o conteúdo necessário dos dados JSON.*

#### Chamar o ponto de acesso dos serviços de conteúdo do AEM

Veja a seguir uma destilação do código no do aplicativo móvel `MainActivity` responsável por chamar os Serviços de conteúdo AEM para coletar o conteúdo que impulsiona a experiência do aplicativo móvel.

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

`onCreate(..)` é o gancho de inicialização do Aplicativo móvel e registra o 3 personalizado `ViewBinders` responsável por analisar o JSON e vincular os valores ao `View` elementos.

`initApp(...)` em seguida, é chamado de, o que faz a solicitação HTTP GET para o ponto de acesso AEM Content Services no AEM Publish para coletar o conteúdo. Ao receber uma Resposta JSON válida, a resposta JSON é transmitida a cada `ViewBinder` responsável por analisar o JSON e vinculá-lo ao dispositivo móvel `View` elementos.

#### Análise da resposta JSON

Em seguida, examinaremos o `LogoViewBinder`, que é simples, mas destaca várias considerações importantes.

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

A primeira linha de `bind(...)` navega para baixo na Resposta JSON por meio das chaves **:itens → raiz → :itens** que representa o Contêiner de layout AEM ao qual os componentes foram adicionados.

Aqui, é feita uma verificação para uma chave chamada **imagem**, que representa o componente de Imagem (novamente, é importante que esse nome de nó → Chave JSON seja estável). Se esse objeto existir, ele será lido e mapeado para a variável [POJO de imagem personalizado](#image-pojo) através do Jackson `ObjectMapper` biblioteca. O POJO de imagem é explorado abaixo.

Por último, o logótipo `src` é carregado no ImageView do Android usando o [!DNL Glide] biblioteca auxiliar.

Observe que devemos fornecer o esquema, o host e a porta do AEM (via `aemHost`) para a instância de publicação do AEM, pois o AEM Content Services fornecerá somente o caminho JCR (ou seja, `/content/dam/wknd-mobile/images/wknd-logo.png`) ao conteúdo referenciado.

#### O POJO de imagem{#image-pojo}

Embora opcional, a utilização do [Mapeador de Objetos Jackson](https://fasterxml.github.io/jackson-databind/javadoc/2.9/com/fasterxml/jackson/databind/ObjectMapper.html) Para recursos semelhantes fornecidos por outras bibliotecas, como Gson, ajuda a mapear estruturas JSON complexas em POJOs Java sem o tédio de lidar diretamente com os próprios objetos JSON nativos. Neste caso simples, mapeamos o `src` chave do `image` objeto JSON, para o `src` no POJO de imagens diretamente através da variável `@JSONProperty` anotação.

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

O POJO de evento, que requer a seleção de muito mais pontos de dados do objeto JSON, se beneficia dessa técnica mais do que a Imagem simples, que tudo o que queremos é a `src`.

## Explore a experiência do aplicativo móvel

Agora que você entende como os Serviços de conteúdo para AEM podem impulsionar a experiência nativa em dispositivos móveis, use o que você aprendeu para executar as etapas a seguir e ver suas alterações refletidas no aplicativo móvel.

Após cada etapa, puxe para atualizar o aplicativo móvel e verifique a atualização da experiência móvel.

1. Criar e publicar **novo [!DNL Event] Fragmento do conteúdo**
1. Desfazer a publicação de um **existente [!DNL Event] Fragmento do conteúdo**
1. Publicar uma atualização na **Linha de tag**

## Parabéns

**Você concluiu com o tutorial AEM Headless!**

Para saber mais sobre o AEM Content Services e o AEM as a Headless CMS, visite Adobe outra documentação e materiais de ativação:

* [Uso de fragmentos de conteúdo](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/understand-content-fragments-and-experience-fragments.html)
* [Guia do usuário dos componentes principais do WCM no AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=pt-BR)
* [Biblioteca de componentes principais WCM do AEM](https://opensource.adobe.com/aem-core-wcm-components/library.html)
* [Projeto GitHub dos Componentes principais do WCM no AEM](https://github.com/adobe/aem-core-wcm-components)
* [Amostra de código do exportador de componente](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleComponentExporter.java)
