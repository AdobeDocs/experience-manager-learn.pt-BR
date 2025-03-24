---
title: Gerenciamento de hosts do AEM para o AEM GraphQL
description: Saiba como configurar hosts do AEM no aplicativo AEM Headless.
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
jira: KT-10831
thumbnail: KT-10831.jpg
exl-id: a932147c-2245-4488-ba1a-99c58045ee2b
duration: 496
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '1614'
ht-degree: 0%

---

# Gerenciamento de hosts do AEM

A implantação de um aplicativo AEM Headless requer atenção a como os URLs do AEM são construídos para garantir que o host/domínio correto do AEM seja usado. Os principais tipos de URL/solicitação que devem ser considerados são:

+ Solicitações HTTP para __[APIs do AEM GraphQL](#aem-graphql-api-requests)__
+ __[URLs de imagem](#aem-image-urls)__ para ativos de imagem referenciados em Fragmentos de conteúdo e entregues pela AEM

Normalmente, um aplicativo do AEM Headless interage com um único serviço do AEM para solicitações de API e imagem do GraphQL. O serviço do AEM muda com base na implantação do aplicativo AEM Headless:

| Tipo de implantação do AEM Headless | Ambiente do AEM | serviço AEM |
|-------------------------------|:---------------------:|:----------------:|
| Produção | Produção | Publicação |
| Visualização de criação | Produção | Visualização |
| Desenvolvimento | Desenvolvimento | Publicação |

Para lidar com permutas de tipo de implantação, cada implantação de aplicativo é criada usando uma configuração que especifica o serviço do AEM ao qual se conectar. O host/domínio do serviço AEM configurado é usado para criar os URLs da API e os URLs de imagem do AEM GraphQL. Para determinar a abordagem correta para gerenciar configurações dependentes de criação, consulte a documentação da estrutura do aplicativo AEM Headless (por exemplo, React, iOS, Android™ e assim por diante), pois a abordagem varia de acordo com a estrutura.

| Tipo de cliente | [Aplicativo de página única (SPA)](../spa.md) | [Componente da Web/JS](../web-component.md) | [Móvel](../mobile.md) | [Servidor para servidor](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| Configuração de hosts do AEM | ✔ | ✔ | ✔ | ✔ |

A seguir estão exemplos de possíveis abordagens para a construção de URLs para [API do AEM GraphQL](#aem-graphql-api-requests) e [solicitações de imagem](#aem-image-requests), para várias estruturas e plataformas headless populares.

## Solicitações de API do AEM GraphQL

As solicitações HTTP do GET do aplicativo headless para as APIs GraphQL da AEM devem ser configuradas para interagir com o serviço AEM correto, conforme descrito na [tabela acima](#managing-aem-hosts).

Ao usar os [SDKs do AEM Headless](../../how-to/aem-headless-sdk.md) (disponíveis para JavaScript baseado em navegador, JavaScript baseado em servidor e Java™), um host do AEM pode inicializar o objeto de cliente do AEM Headless com o AEM Service para se conectar com o.

Ao desenvolver um cliente AEM Headless personalizado, verifique se o host do serviço AEM pode ser parametrizado com base em parâmetros de build.

### Exemplos

A seguir estão exemplos de como as solicitações de API do AEM GraphQL podem fazer com que o valor de host do AEM se torne configurável para várias estruturas de aplicativo headless.

+++ Exemplo do React

Este exemplo, vagamente baseado no [aplicativo AEM Headless React](../../example-apps/react-app.md), ilustra como as solicitações de API do AEM GraphQL podem ser configuradas para se conectar a diferentes serviços da AEM com base em variáveis de ambiente.

Os aplicativos React devem usar o [AEM Headless Client for JavaScript](../../how-to/aem-headless-sdk.md) para interagir com as APIs do AEM GraphQL. O cliente AEM Headless, fornecido pelo AEM Headless Client para JavaScript, deve ser inicializado com o host do AEM Service ao qual se conecta.

#### Arquivo de ambiente do React

O React usa [arquivos de ambiente personalizados](https://create-react-app.dev/docs/adding-custom-environment-variables/) ou `.env` arquivos, armazenados na raiz do projeto para definir valores específicos de compilação. Por exemplo, o arquivo `.env.development` contém valores usados durante o desenvolvimento, enquanto `.env.production` contém valores usados para compilações de produção.

+ `.env.development`

```
# Environment variable used to specify the AEM service the React app will connect to when running under this profile
REACT_APP_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com
...
```

`.env` arquivos para outros usos [podem ser especificados](https://create-react-app.dev/docs/adding-custom-environment-variables/#what-other-env-files-can-be-used) por postfix `.env` e um descritor semântico, como `.env.stage` ou `.env.production`. Diferentes arquivos `.env` podem ser usados ao executar ou criar o aplicativo React, configurando o `REACT_APP_ENV` antes de executar um comando `npm`.

Por exemplo, um `package.json` do aplicativo React pode conter a seguinte configuração `scripts`:

+ `package.json`

```
...
"scripts": {
  "build:development": "REACT_APP_ENV=development npm run build",
  "build:stage": "REACT_APP_ENV=stage npm run build",
  "build:production": "REACT_APP_ENV=production npm run build"
},
...
```

#### Cliente headless do AEM

O [AEM Headless Client para JavaScript](../../how-to/aem-headless-sdk.md) contém um cliente AEM Headless que faz solicitações HTTP às APIs GraphQL da AEM. O cliente AEM Headless deve ser inicializado com o host AEM com o qual interage, usando o valor do arquivo `.env` ativo.

+ `src/api/headlessClient.js`

```
const { AEMHeadless } = require('@adobe/aem-headless-client-js');
...
// Get the environment variables for configuring the headless client, 
// specifically the `REACT_APP_AEM_HOST` which contains the AEM service host.
const {
    REACT_APP_AEM_HOST,         // https://publish-p123-e456.adobeaemcloud.com
    REACT_APP_GRAPHQL_ENDPOINT,
} = process.env;
...

// Initialize the AEM Headless client with the AEM Service host, which dictates the AEM service provides the GraphQL data.
export const aemHeadlessClient = new AEMHeadless({
    serviceURL: REACT_APP_AEM_HOST,        
    endpoint: REACT_APP_GRAPHQL_ENDPOINT
});
```

#### Reagir useEffect(..) gancho

Os ganchos useEffect personalizados do React chamam o cliente AEM Headless, inicializado com o host do AEM, em nome do componente React que renderiza a exibição.

+ `src/api/persistedQueries.js`

```javascript
import { aemHeadlessClient , mapErrors} from "./headlessClient";
...
// The exported useEffect hook
export const getAdventureByPath = async function(adventurePath) {
    const queryVariables = {'adventurePath': adventurePath};
    return executePersistedQuery('wknd-shared/adventures-by-path', queryVariables);
}
...
// Private function that invokes the aemHeadlessClient
const executePersistedQuery = async function(persistedQueryPath, queryVariables) {
    let data;
    let errors;

    try {
        // Run the persisted query using using the aemHeadlessClient that's initialized with the AEM host
        const response = await aemHeadlessClient.runPersistedQuery(persistedQueryPath, queryVariables);
        // The GraphQL data is stored on the response's data field
        data = response.data;
        errors = response.errors ? mapErrors(response.errors) : undefined;
    } catch (e) {
        console.error(e.toJSON());
        errors = e;
    }

    return {data, errors}; 
}
```

#### Componente do React

O gancho personalizado useEffect, `useAdventureByPath` é importado e usado para obter os dados usando o cliente AEM Headless e, por fim, renderizar o conteúdo para o usuário final.

+ &#39;src/components/AdventureDetail.js&#39;

```javascript
import { useAdventureByPath } from './api/persistedQueries';
...
// Query AEM GraphQL APIs via the useEffect hook that invokes the AEM Headless client initialized with the AEM host
let { data, error } = useAdventureByPath('/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg')

...
```

+++

+++ iOS™ example

Este exemplo, com base no [exemplo de aplicativo AEM Headless iOS™](../../example-apps/ios-swiftui-app.md), ilustra como as solicitações de API do AEM GraphQL podem ser configuradas para se conectar a diferentes hosts do AEM com base nas [variáveis de configuração específicas da compilação](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3).

Os aplicativos iOS™ exigem um cliente AEM Headless personalizado para interagir com as APIs GraphQL da AEM. O cliente AEM Headless deve ser escrito de modo que o host de serviço do AEM possa ser configurado.

#### Configuração de build

O arquivo de configuração do XCode contém os detalhes da configuração padrão.

+ `Config.xcconfig`

```swift
// The http/https protocol scheme used to access the AEM_HOST
AEM_SCHEME = https

// Target hostname for AEM service, do not include the scheme: http:// or https://
AEM_HOST = publish-p123-e789.adobeaemcloud.com
...
```

#### Inicializar o cliente AEM headless personalizado

O [exemplo de aplicativo AEM Headless iOS](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app) usa um cliente AEM headless personalizado inicializado com os valores de configuração para `AEM_SCHEME` e `AEM_HOST`.

```swift
...
let aemScheme: String = try Configuration.value(for: "AEM_SCHEME")  // https
let aemHost: String = try Configuration.value(for: "AEM_HOST")      // publish-p123-e456.adobeaemcloud.com

let aemHeadlessClient = Aem(scheme: aemScheme, host: aemHost);
```

O cliente AEM headless personalizado (`api/Aem.swift`) contém um método `makeRequest(..)` que prefixa solicitações de APIs do AEM GraphQL com o AEM configurado `scheme` e `host`.

+ `api/Aem.swift`

```swift
/// #makeRequest(..)
/// Generic method for constructing and executing AEM GraphQL persisted queries
private func makeRequest(persistedQueryName: String, params: [String: String] = [:]) -> URLRequest {
    // Encode optional parameters as required by AEM
    let persistedQueryParams = params.map { (param) -> String in
        encode(string: ";\(param.key)=\(param.value)")
    }.joined(separator: "")
    
    // Construct the AEM GraphQL persisted query URL, including optional query params
    let url: String = "\(self.scheme)://\(self.host)/graphql/execute.json/" + persistedQueryName + persistedQueryParams;

    var request = URLRequest(url: URL(string: url)!);
    
    return request;
}
```

[Novos arquivos de configuração de compilação podem ser criados](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3) para conexão com diferentes serviços da AEM. Os valores específicos da compilação para `AEM_SCHEME` e `AEM_HOST` são usados com base na compilação selecionada no XCode, resultando no cliente AEM Headless personalizado se conectar com o serviço AEM correto.

+++

+++ Android™ example

Este exemplo, baseado no [exemplo de aplicativo AEM Headless Android™](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app), ilustra como as solicitações de API do AEM GraphQL podem ser configuradas para se conectar a diferentes Serviços da AEM com base em variáveis de configuração específicas de compilação (ou tipos).

Os aplicativos Android™ (quando escritos em Java™) devem usar o [AEM Headless Client for Java™](https://github.com/adobe/aem-headless-client-java) para interagir com as APIs GraphQL da AEM. O cliente AEM Headless, fornecido pelo AEM Headless Client para Java™, deve ser inicializado com o host do AEM Service ao qual ele se conecta.

#### Criar arquivo de configuração

Os aplicativos Android™ definem &quot;productFlavors&quot; que são usados para criar artefatos para diferentes usos.
Este exemplo mostra como duas opções de produtos da Android™ podem ser definidas, fornecendo valores de hosts de serviço da AEM (`AEM_HOST`) diferentes para usos de desenvolvimento (`dev`) e produção (`prod`).

No arquivo `build.gradle` do aplicativo, um novo `flavorDimension` chamado `env` é criado.

Na dimensão `env`, dois `productFlavors` são definidos: `dev` e `prod`. Cada `productFlavor` usa `buildConfigField` para definir variáveis específicas de compilação que definem o serviço AEM ao qual se conectar.

+ `app/build.gradle`

```gradle
android {
    ...
    flavorDimensions 'env'
    productFlavors {
        dev {
            dimension 'env'
            applicationIdSuffix '.dev'
            buildConfigField "String", "AEM_HOST", '"http://10.0.2.2:4503"'
            ...
        }
        prod {
            dimension 'env'
            buildConfigField "String", "AEM_HOST", '"https://publish-p123-e789.adobeaemcloud.com"'
            ...
        }
    }
    ...
}
```

#### Carregador Android™

Inicialize o construtor `AEMHeadlessClient`, fornecido pelo AEM Headless Client para Java™, com o valor `AEM_HOST` do campo `buildConfigField`.

+ `app/src/main/java/com/adobe/wknd/androidapp/loader/AdventuresLoader.java`

```java
public class AdventuresLoader extends AsyncTaskLoader<AdventureList> {
    ...

    @Override
    public AdventureList loadInBackground() {
        ...
        // Initialize the AEM Headless client using the AEM Host exposed via BuildConfig.AEM_HOST
        AEMHeadlessClientBuilder builder = AEMHeadlessClient.builder().endpoint(BuildConfig.AEM_HOST);
        AEMHeadlessClient client = builder.build();
        // With the AEM headless client initialized, make GraphQL persisted query calls to AEM
        ...    
    }
    ...
}
```

Ao criar o aplicativo Android™ para diferentes usos, especifique o tipo de host `env` e o valor de host AEM correspondente seja usado.

+++

## URLs de imagem do AEM

As solicitações de imagem do aplicativo headless para o AEM devem ser configuradas para interagir com o serviço AEM correto, conforme descrito na [tabela acima](#managing-aem-hosts).

A Adobe recomenda usar [imagens otimizadas](../../how-to/images.md) disponibilizadas por meio do campo `_dynamicUrl` nas APIs GraphQL da AEM. O campo `_dynamicUrl` retorna uma URL sem host que pode receber o prefixo do host de serviço do AEM usado para consultar APIs do AEM GraphQL. O campo `_dynamicUrl` na resposta do GraphQL tem a seguinte aparência:

```json
{
    ...
    "_dynamicUrl": "/adobe/dynamicmedia/deliver/dm-aid--dd42d814-88ec-4c4d-b5ef-e3dc4bc0cb42/example.jpg?preferwebp=true",
    ...
}
```

### Exemplos

A seguir estão exemplos de como URLs de imagem podem prefixar o valor de host do AEM, tornado configurável para várias estruturas de aplicativo headless. Os exemplos presumem o uso de consultas GraphQL que retornam referências de imagem usando o campo `_dynamicUrl`.

Por exemplo:

#### Consulta persistente do GraphQL

Esta consulta GraphQL retorna uma referência de imagem de `_dynamicUrl`. Como visto na [resposta do GraphQL](#examples-react-graphql-response) que exclui um host.

```graphql
query ($path: String!) {
  adventureByPath(_path: $path, _assetTransform: { format: JPG, preferWebp: true }) {
    item {
      title,
      primaryImage {
        ... on ImageRef {
          _dynamicUrl
        }
      }
    }
  }
}
```

#### Resposta do GraphQL

Esta resposta do GraphQL retorna a `_dynamicUrl` da referência da imagem que exclui um host.

```json
{
  "data": {
    "adventureByPath": {
      "item": {
        "adventurePrimaryImage": {
          "_dynamicUrl": "/adobe/dynamicmedia/deliver/dm-aid--de43411-88ec-4c4d-b5ef-e3dc4bc0cb42/adobestock-175749320.jpg?preferwebp=true",
        }
      }
    }
  }
}
```

+++ Exemplo do React

Este exemplo, com base no [exemplo do aplicativo AEM Headless React](../../example-apps/react-app.md), ilustra como URLs de imagens podem ser configuradas para se conectar aos Serviços da AEM corretos com base em variáveis de ambiente.

Este exemplo mostra como adicionar um prefixo ao campo `_dynamicUrl` da referência da imagem, com uma variável de ambiente `REACT_APP_AEM_HOST` do React configurável.

#### Arquivo de ambiente do React

O React usa [arquivos de ambiente personalizados](https://create-react-app.dev/docs/adding-custom-environment-variables/) ou `.env` arquivos, armazenados na raiz do projeto para definir valores específicos de compilação. Por exemplo, o arquivo `.env.development` contém valores usados durante o desenvolvimento, enquanto `.env.production` contém valores usados para compilações de produção.

+ `.env.development`

```
# Environment variable used to specify the AEM service the React app will connect to when running under this profile
REACT_APP_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com
...
```

`.env` arquivos para outros usos [podem ser especificados](https://create-react-app.dev/docs/adding-custom-environment-variables/#what-other-env-files-can-be-used) por postfix `.env` e um descritor semântico, como `.env.stage` ou `.env.production`. Um arquivo `.env` diferente pode ser usado ao executar ou criar o aplicativo React, configurando o `REACT_APP_ENV` antes de executar um comando `npm`.

Por exemplo, um `package.json` do aplicativo React pode conter a seguinte configuração `scripts`:

+ `package.json`

```
...
"scripts": {
  "build:development": "REACT_APP_ENV=development npm run build",
  "build:stage": "REACT_APP_ENV=stage npm run build",
  "build:production": "REACT_APP_ENV=production npm run build"
},
...
```

#### Componente do React

O componente React importa a variável de ambiente `REACT_APP_AEM_HOST` e prefixa o valor da imagem `_dynamicUrl` para fornecer uma URL de imagem totalmente resolvível.

A mesma variável de ambiente `REACT_APP_AEM_HOST` é usada para inicializar o cliente AEM Headless usado pelo gancho `useAdventureByPath(..)` custom useEffect usado para buscar os dados do GraphQL no AEM. Usando a mesma variável para criar a solicitação de API do GraphQL como o URL da imagem, verifique se o aplicativo React interage com o mesmo serviço do AEM para ambos os casos de uso.

+ &#39;src/components/AdventureDetail.js&#39;

```javascript
...
// Import the AEM origin from the app's environment configuration
const AEM_HOST = env.process.REACT_APP_AEM_HOST; // https://publish-p123-e456.adobeaemcloud.com

let { data, error } = useAdventureByPath('/content/dam/wknd-shared/en/adventures/bali-surf-camp/bali-surf-camp')

return (
    // Prefix the image src URL with the AEM host
    <img src={AEM_HOST + data.adventureByPath.item.primaryImage._dynamicUrl }>
    {/* Resulting in: <img src="https://publish-p123-e456.adobeaemcloud.com/adobe/dynamicmedia/deliver/dm-aid--de43411-88ec-4c4d-b5ef-e3dc4bc0cb42/adobestock-175749320.jpg"/>  */}
)
```

+++

+++ iOS™ example

Este exemplo, com base no [exemplo de aplicativo AEM Headless iOS™](../../example-apps/ios-swiftui-app.md), ilustra como URLs de imagens do AEM podem ser configuradas para se conectar a diferentes hosts do AEM com base em [variáveis de configuração específicas da compilação](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3).

#### Configuração de build

O arquivo de configuração do XCode contém os detalhes da configuração padrão.

+ `Config.xcconfig`

```swift
// The http/https protocol scheme used to access the AEM_HOST
AEM_SCHEME = https

// Target hostname for AEM service, do not include the scheme: http:// or https://
AEM_HOST = publish-p123-e789.adobeaemcloud.com
...
```

#### Gerador de URL de imagem

Em `Aem.swift`, a implementação de cliente headless AEM personalizado, uma função personalizada `imageUrl(..)`, toma o caminho da imagem conforme fornecido no campo `_dynamicUrl` na resposta do GraphQL e o anexa ao host da AEM. Essa função é invocada nas visualizações do iOS sempre que uma imagem é renderizada.

+ `WKNDAdventures/AEM/Aem.swift`

```swift
class Aem: ObservableObject {
    let scheme: String
    let host: String
    ...
    init(scheme: String, host: String) {
        self.scheme = scheme
        self.host = host
    }
    ...
    /// Prefixes AEM image dynamicUrl with the AEM scheme/host
    func imageUrl(dynamicUrl: String) -> URL {
        return URL(string: "\(self.scheme)://\(self.host)\(dynamicUrl)")!
    }
    ...
}
```

#### Visualização do iOS

A exibição do iOS e prefixos o valor da imagem `_dynamicUrl`, para fornecer uma URL de imagem totalmente resolvível.

+ `WKNDAdventures/Views/AdventureListItemView.swift`

```swift
import SDWebImageSwiftUI
...
struct AdventureListItemView: View {
    @EnvironmentObject private var aem: Aem

    var adventure: Adventure
    
    var body: some View {
        HStack {
            // Path the image dynamicUrl to `aem.imageUrl(..)` to prepend the AEM service host     
            AdventureRowImage(imageUrl: aem.imageUrl(dynamicUrl: adventure.image()))
            Text(adventure.title)
            Spacer()
        }
    }
}
...
```

[Novos arquivos de configuração de compilação podem ser criados](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3) para conexão com diferentes serviços da AEM. Os valores específicos da compilação para `AEM_SCHEME` e `AEM_HOST` são usados com base na compilação selecionada no XCode, resultando no cliente AEM Headless personalizado para interagir com o serviço AEM correto.

+++

+++ Android™ example

Este exemplo, baseado no [exemplo de aplicativo AEM Headless Android™](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app), ilustra como URLs de imagens do AEM podem ser configuradas para se conectar a diferentes Serviços da AEM com base em variáveis de configuração específicas de compilação (ou tipos).

#### Criar arquivo de configuração

Os aplicativos Android™ definem &quot;productFlavors&quot; que são usados para criar artefatos para diferentes usos.
Este exemplo mostra como duas opções de produtos da Android™ podem ser definidas, fornecendo valores de hosts de serviço da AEM (`AEM_HOST`) diferentes para usos de desenvolvimento (`dev`) e produção (`prod`).

No arquivo `build.gradle` do aplicativo, um novo `flavorDimension` chamado `env` é criado.

Na dimensão `env`, dois `productFlavors` são definidos: `dev` e `prod`. Cada `productFlavor` usa `buildConfigField` para definir variáveis específicas de compilação que definem o serviço AEM ao qual se conectar.

+ `app/build.gradle`

```gradle
android {
    ...
    flavorDimensions 'env'
    productFlavors {
        dev {
            dimension 'env'
            applicationIdSuffix '.dev'
            buildConfigField "String", "AEM_HOST", '"http://10.0.2.2:4503"'
            ...
        }
        prod {
            dimension 'env'
            buildConfigField "String", "AEM_HOST", '"https://publish-p123-e789.adobeaemcloud.com"'
            ...
        }
    }
    ...
}
```

#### Carregamento da imagem do AEM

O Android™ usa um `ImageGetter` para buscar e armazenar em cache localmente dados de imagem do AEM. No `prepareDrawableFor(..)`, o host de serviço do AEM, definido na configuração de compilação ativa, é usado para prefixar o caminho da imagem, criando uma URL que pode ser resolvida para o AEM.

+ `app/src/main/java/com/adobe/wknd/androidapp/loader/RemoteImagesCache.java`

```java
...
public class RemoteImagesCache implements Html.ImageGetter {
    ...
    private final Map<String, Drawable> drawablesByPath = new HashMap<>();
    ...
    public void prepareDrawableFor(String path) {
        ...

        // Prefix the image path with the build config AEM_HOST variable
        String urlStr = BuildConfig.AEM_HOST + path;

        URL url = new URL(urlStr);
        HttpURLConnection connection = (HttpURLConnection) url.openConnection();
        // Get the image data from AEM 
        Drawable drawable = Drawable.createFromStream(is, new File(path).getName());
        ...
        // Save the image data into the cache using the path as the key
        drawablesByPath.put(path, drawable);
        ...    
    }

    @Override
    public Drawable getDrawable(String dynamicUrl) {
        // Get the image data from the cache using the dynamicUrl as the key
        return drawablesByPath.get(dynamicUrl);
    }
}
```

#### Android™ view

A visualização Android™ obtém os dados da imagem por meio de `RemoteImagesCache` usando o valor `_dynamicUrl` da resposta do GraphQL.

+ `app/src/main/java/com/adobe/wknd/androidapp/AdventureDetailFragment.java`

```java
...
public class AdventureDetailFragment extends Fragment implements LoaderManager.LoaderCallbacks<Adventure> {
    ...
    private ImageView adventureDetailImage;
    ...

    private void updateContent() {
        ...
        adventureDetailImage.setImageDrawable(RemoteImagesCache.getInstance().getDrawable(adventure.getPrimaryImageDynamicUrl()));
        ...
    }
...
}
```

Ao criar o aplicativo Android™ para diferentes usos, especifique o tipo de host `env` e o valor de host AEM correspondente seja usado.

+++
