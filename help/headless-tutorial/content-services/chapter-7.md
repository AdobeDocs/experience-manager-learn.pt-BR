---
title: Capítulo 7 - Consumir serviços de conteúdo AEM de um aplicativo móvel - Serviços de conteúdo
description: O capítulo 7 do tutorial executa o aplicativo móvel Android para consumir conteúdo criado a partir do AEM Content Services.
feature: Content Fragments, APIs
topic: Headless, Content Management
role: Developer
level: Beginner
doc-type: Tutorial
exl-id: d6b6d425-842a-43a9-9041-edf78e51d962
duration: 467
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '1350'
ht-degree: 0%

---

# Capítulo 7 - Consumir serviços de conteúdo de AEM de um aplicativo móvel

O Capítulo 7 do tutorial usa um aplicativo móvel nativo do Android para consumir conteúdo do AEM Content Services.

## O aplicativo móvel do Android

Este tutorial usa um **aplicativo simples nativo para dispositivos móveis do Android** a fim de consumir e exibir o conteúdo do Evento exposto pelo AEM Content Services.

O uso do [Android](https://developer.android.com/) não é muito importante, e o aplicativo para dispositivos móveis de consumo pode ser escrito em qualquer estrutura para qualquer plataforma móvel, por exemplo, iOS.

O Android é usado como tutorial devido à capacidade de executar um Android Emulator no Windows, macOs e Linux, sua popularidade e que pode ser escrito como Java, uma linguagem bem compreendida pelos desenvolvedores do AEM.

*O objetivo do tutorial para o aplicativo móvel do Android é&#x200B;**não**&#x200B;instruir como criar aplicativos do Android Mobile ou transmitir práticas recomendadas de desenvolvimento do Android AEM, mas sim ilustrar como o Content Services pode ser consumido de um aplicativo móvel.*

### Como o AEM Content Services orienta a experiência do aplicativo móvel

![Mapeamento de Aplicativo Móvel para Serviços de Conteúdo](assets/chapter-7/content-services-mapping.png)

1. O **logotipo** conforme definido pelo **componente de Imagem** da página [!DNL Events API].
1. A **linha da marca** conforme definido no **componente de Texto** da página [!DNL Events API].
1. Esta **lista de eventos** é derivada da serialização dos Fragmentos de conteúdo do evento, exposta por meio do **componente de Lista de fragmentos de conteúdo** configurado.

## Demonstração do aplicativo móvel

>[!VIDEO](https://video.tv.adobe.com/v/28345?quality=12&learn=on)

### Configuração do aplicativo móvel para uso fora do host local

Se o AEM Publish não estiver sendo executado em **http://localhost:4503**, o host e a porta poderão ser atualizados no [!DNL Settings] do Aplicativo Móvel para apontar para a propriedade AEM host/porta do Publish.

>[!VIDEO](https://video.tv.adobe.com/v/28344?quality=12&learn=on)

## Execução local do aplicativo móvel

1. Baixe e instale o [Android Studio](https://developer.android.com/studio/install) para instalar o Android Emulator.
1. **Baixar** o arquivo [!DNL APK] do Android [GitHub > Assets > wknd-mobile.x.x.xapk](https://github.com/adobe/aem-guides-wknd-mobile/releases/latest)
1. Abrir o **Android Studio**
   * Na primeira inicialização do Android Studio, será apresentado um prompt para instalar o [!DNL Android SDK]. Aceite os padrões e finalize a instalação.
1. Abra o Android Studio e selecione **Perfil ou Depurar APK**
1. Selecione o arquivo APK (**wknd-mobile.x.x.x.apk**) baixado na Etapa 2 e clique em **OK**
   * Se for solicitado para **Criar uma Nova Pasta** ou **Usar Existente**, selecione **Usar Existente**.
1. Na primeira inicialização do Android Studio, clique com o botão direito do mouse em **wknd-mobile.x.x.x** na lista Projetos e selecione **Abrir Configurações do Módulo**.
   * Em **Modules > wknd-mobile.x.x.x > Dependencies tab**, selecione **Android API 29 Platform**. Toque em OK para fechar e salvar as alterações.
   * Se você não fizer isso, verá um erro &quot;Selecione o SDK do Android&quot; ao tentar iniciar o emulador.
1. Abra o **Gerenciador AVD** selecionando **Ferramentas > Gerenciador AVD** ou tocando no ícone **Gerenciador AVD** na barra superior.
1. Na janela **Gerenciador AVD**, clique em **+ Criar Dispositivo Virtual...** se ainda não tiver um dispositivo registrado.
   1. À esquerda, selecione a categoria **Telefone**.
   1. Selecione um **Pixel 2**.
   1. Clique no botão **Avançar**.
   1. Selecione **Q** com **Nível de API 29**.
      * Após a primeira inicialização do AVD Manager, você será solicitado a Baixar a API versionada. Clique no link Download ao lado da versão &quot;Q&quot; e conclua o download e a instalação.
   1. Clique no botão **Avançar**.
   1. Clique no botão **Concluir**.
1. Feche a janela **Gerenciador AVD**.
1. Na barra de menu superior, selecione **wknd-mobile.x.x.x** no menu suspenso **Executar/Editar configurações**.
1. Toque no botão **Executar** ao lado da **Executar/Editar Configuração** selecionada
1. Na janela pop-up, selecione o dispositivo virtual **[!DNL Pixel 2 API 29]** recém-criado e toque em **OK**
1. Se o aplicativo [!DNL WKND Mobile] não carregar imediatamente, localize e toque no ícone **[!DNL WKND]** na tela inicial do Android no emulador.
   * Se o emulador for iniciado, mas a tela do emulador permanecer preta, toque no botão **ligar** na janela de ferramentas do emulador ao lado da janela do emulador.
   * Para rolar dentro do dispositivo virtual, clique e segure e arraste.
   * Para atualizar o conteúdo do AEM, puxe da parte superior até o ícone Atualizar
é exibida e lançada.

>[!VIDEO](https://video.tv.adobe.com/v/28341?quality=12&learn=on)

## O código do aplicativo móvel

Esta seção destaca o código do aplicativo móvel do Android que mais interage e depende do AEM Content Services e da saída JSON.

Após o carregamento, o Aplicativo Móvel faz `HTTP GET` para `/content/wknd-mobile/en/api/events.model.json`, que é o ponto de acesso do AEM Content Services configurado para fornecer o conteúdo para direcionar o Aplicativo Móvel.

Como o Modelo Editável da API de Eventos (`/content/wknd-mobile/en/api/events.model.json`) está bloqueado, o Aplicativo Móvel pode ser codificado para procurar informações específicas em locais específicos na resposta JSON.

### Fluxo de código de alto nível

1. Abrir o Aplicativo [!DNL WKND Mobile] invoca uma solicitação `HTTP GET` ao AEM Publish em `/content/wknd-mobile/en/api/events.model.json` para coletar o conteúdo e preencher a interface do usuário do Aplicativo Móvel.
2. Ao receber o conteúdo do AEM, cada um dos três elementos de exibição do Aplicativo Móvel, o **logotipo, a linha da tag e a lista de eventos**, é inicializado com o conteúdo do AEM.
   * Para vincular ao conteúdo AEM ao elemento de visualização do aplicativo móvel, o JSON que representa cada componente AEM é o objeto mapeado para um POJO Java, que por sua vez é vinculado ao elemento de visualização Android.
      * Componente de Imagem JSON → POJO de logotipo → Visualização de imagem de logotipo
      * Componente de Texto JSON → POJO de tag → ImageView de texto
      * Lista de fragmentos de conteúdo JSON → Eventos POJO → Eventos RecyclerView
   * *O código do Aplicativo Móvel pode mapear o JSON para os POJOs devido aos locais bem conhecidos dentro da maior resposta JSON. Lembre-se de que as chaves JSON de &quot;imagem&quot;, &quot;texto&quot; e &quot;contentfragmentlist&quot; são ditadas pelos nomes de nó dos componentes AEM de apoio. Se esses nomes de nós forem alterados, o Aplicativo Móvel será interrompido, pois não saberá como obter o conteúdo necessário dos dados JSON.*

#### Chamar o ponto de acesso dos serviços de conteúdo do AEM

Veja a seguir uma destilação do código no `MainActivity` do aplicativo móvel responsável por invocar o AEM Content Services para coletar o conteúdo que orienta a experiência do aplicativo móvel.

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

`onCreate(..)` é o gancho de inicialização do Aplicativo Móvel e registra os três `ViewBinders` personalizados responsáveis pela análise do JSON e pela associação dos valores aos elementos `View`.

`initApp(...)` é então chamado, o que faz a solicitação HTTP GET para o ponto de acesso AEM Content Services no Publish AEM para coletar o conteúdo. Ao receber uma Resposta JSON válida, a resposta JSON é passada para cada `ViewBinder` que é responsável por analisar o JSON e associá-lo aos elementos `View` móveis.

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

A primeira linha de `bind(...)` navega pela Resposta JSON por meio das chaves **:items → root → :items**, que representa o Contêiner de Layout AEM ao qual os componentes foram adicionados.

Daqui é feita uma verificação de uma chave chamada **image**, que representa o componente de Imagem (novamente, é importante que esse nome de nó → A chave JSON seja estável). Se este objeto existir, ele será lido e mapeado para o [POJO de Imagem personalizado](#image-pojo) através da biblioteca Jackson `ObjectMapper`. O POJO de imagem é explorado abaixo.

Finalmente, o `src` do logotipo é carregado no ImageView do Android usando a biblioteca auxiliar [!DNL Glide].

Observe que devemos fornecer o esquema, o host e a porta do AEM (via `aemHost`) para a instância do AEM Publish AEM, pois o Content Services fornecerá somente o caminho JCR (ou seja, `/content/dam/wknd-mobile/images/wknd-logo.png`) ao conteúdo referenciado.

#### O POJO de imagem{#image-pojo}

Embora seja opcional, o uso do [Jackson ObjectMapper](https://fasterxml.github.io/jackson-databind/javadoc/2.9/com/fasterxml/jackson/databind/ObjectMapper.html) ou de recursos semelhantes fornecidos por outras bibliotecas, como o Gson, ajuda a mapear estruturas JSON complexas em POJOs Java sem o tédio de lidar diretamente com os próprios objetos JSON nativos. Nesse caso simples, mapeamos a chave `src` do objeto JSON `image` para o atributo `src` no POJO de Imagem diretamente por meio da anotação `@JSONProperty`.

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

O POJO de Evento, que requer a seleção de muito mais pontos de dados do objeto JSON, se beneficia dessa técnica mais do que a Imagem simples, que tudo o que queremos é o `src`.

## Explore a experiência do aplicativo móvel

Agora que você entende como os Serviços de conteúdo para AEM podem impulsionar a experiência nativa em dispositivos móveis, use o que você aprendeu para executar as etapas a seguir e ver suas alterações refletidas no aplicativo móvel.

Após cada etapa, puxe para atualizar o aplicativo móvel e verifique a atualização da experiência móvel.

1. Criar e publicar **novo [!DNL Event] fragmento de conteúdo**
1. Cancelar publicação de um **fragmento de conteúdo [!DNL Event] existente**
1. O Publish pode atualizar a **Linha da Marca**

## Parabéns

**Você concluiu o tutorial sobre AEM Headless!**

Para saber mais sobre o AEM Content Services e o AEM as a Headless CMS, visite Adobe outra documentação e materiais de ativação:

* [Usando Fragmentos de Conteúdo](https://experienceleague.adobe.com/docs/experience-manager-learn/sites/content-fragments/understand-content-fragments-and-experience-fragments.html)
* [Guia do Usuário dos Componentes principais do WCM no AEM](https://experienceleague.adobe.com/docs/experience-manager-core-components/using/introduction.html?lang=pt-BR)
* [Biblioteca de componentes principais WCM do AEM](https://opensource.adobe.com/aem-core-wcm-components/library.html)
* [Projeto GitHub dos Componentes principais do WCM no AEM](https://github.com/adobe/aem-core-wcm-components)
* [Amostra de código do Exportador de componentes](https://github.com/Adobe-Consulting-Services/acs-aem-samples/blob/master/core/src/main/java/com/adobe/acs/samples/models/SampleComponentExporter.java)
