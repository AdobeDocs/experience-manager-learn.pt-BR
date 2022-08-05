---
title: Gerenciamento de hosts AEM para AEM GraphQL
description: Saiba como configurar AEM hosts no aplicativo sem cabeçalho AEM.
version: Cloud Service
feature: GraphQL API
topic: Headless, Content Management
role: Developer, Architect
level: Intermediate
kt: 10831
thumbnail: KT-10831.jpg
source-git-commit: b98f567e05839db78a1a0a593c106b87af931a49
workflow-type: tm+mt
source-wordcount: '1712'
ht-degree: 0%

---


# Gerenciamento de hosts AEM

A implantação de um aplicativo AEM Headless requer atenção em como AEM URLs são construídos para garantir que o host/domínio AEM correto seja usado. Os principais tipos de URL/solicitação que devem estar cientes são:

+ Solicitações HTTP para __[AEM APIs GraphQL](#aem-graphql-api-requests)__
+ __[URLs de imagem](#aem-image-urls)__ para criar imagens de ativos referenciados nos Fragmentos de conteúdo e entregues por AEM

Normalmente, um aplicativo sem cabeçalho AEM interage com um único serviço de AEM para a API GraphQL e solicitações de imagem. O serviço de AEM é alterado com base na implantação AEM do aplicativo headless:

| Tipo de implantação AEM sem cabeçalho | Ambiente AEM | serviço AEM |
|-------------------------------|:---------------------:|:----------------:|
| Produção | Produção | Publicação |
| Visualização de criação | Produção | Visualizar |
| Desenvolvimento | Desenvolvimento | Publicação |

Para lidar com permutas de tipo de implantação, cada implantação de aplicativo é criada usando uma configuração que especifica o serviço de AEM ao qual se conectar. O host/domínio do serviço de AEM configurado é usado para criar os URLs de API GraphQL AEM e os URLs de imagem. Para determinar a abordagem correta para gerenciar configurações dependentes de criação, consulte a documentação da estrutura do aplicativo sem cabeçalho AEM (por exemplo, React, iOS, Android™ e assim por diante), já que a abordagem varia de acordo com a estrutura.

| Tipo de cliente | [Aplicativo de página única (SPA)](../spa.md) | [Componente da Web/JS](../web-component.md) | [Móvel](../mobile.md) | [Servidor para servidor](../server-to-server.md) |
|------------------------------------------:|:---------------------:|:----------------:|:---------:|:----------------:|
| Configuração de hosts AEM | ✔ | ✔ | ✔ | ✔ |

Veja a seguir exemplos de possíveis abordagens para a construção de URLs para [AEM API GraphQL](#aem-graphql-api-requests) e [solicitações de imagem](#aem-image-requests), para várias estruturas e plataformas headless populares.

## Solicitações de API GraphQL da AEM

As solicitações HTTP GET do aplicativo sem cabeçalho para AEM APIs GraphQL devem ser configuradas para interagir com o serviço de AEM correto, conforme descrito no [tabela acima](#managing-aem-hosts).

Ao usar [SDKs sem cabeçalho do AEM](../../how-to/aem-headless-sdk.md) (disponível para JavaScript baseado em navegador, JavaScript baseado em servidor e Java™), um host de AEM pode inicializar o objeto cliente sem cabeçalho AEM com o Serviço de AEM para se conectar.

Ao desenvolver um cliente sem cabeçalho de AEM personalizado, certifique-se de que o host do serviço de AEM seja parametrizável com base nos parâmetros de criação.

### Exemplos

A seguir estão exemplos de como AEM solicitações de API GraphQL podem ter o valor de host AEM tornado configurável para várias estruturas de aplicativo sem cabeçalho.

+++ Exemplo de reação

Este exemplo é baseado na variável [Aplicativo de reação sem cabeçalho do AEM](../../example-apps/react-app.md)O ilustra como AEM solicitações de API GraphQL podem ser configuradas para se conectar a diferentes Serviços AEM com base em variáveis de ambiente.

Os aplicativos React devem usar o [Cliente autônomo do AEM para JavaScript](../../how-to/aem-headless-sdk.md) para interagir com AEM APIs GraphQL. O cliente Sem Cabeçalho do AEM, fornecido pelo AEM Cliente Sem Cabeçalho para JavaScript, deve ser inicializado com o host do Serviço de AEM ao qual ele se conecta.

#### Reagir arquivo de ambiente

Reagir usos [arquivos de ambiente personalizados](https://create-react-app.dev/docs/adding-custom-environment-variables/)ou `.env` arquivos, armazenados na raiz do projeto para definir valores específicos da criação. Por exemplo, a variável `.env.development` O arquivo contém valores usados para durante o desenvolvimento, enquanto `.env.production` contém valores usados para builds de produção.

+ `.env.development`

```
# Environment variable used to specify the AEM service the React app will connect to when running under this profile
REACT_APP_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com
...
```

`.env` arquivos para outros usos [pode ser especificado](https://create-react-app.dev/docs/adding-custom-environment-variables/#what-other-env-files-can-be-used) por postfix `.env` e um descritor semântico, como `.env.stage` ou `.env.production`. Different `.env` os arquivos podem ser usados ao executar ou criar o aplicativo React, definindo a variável `REACT_APP_ENV` antes de executar uma `npm` comando.

Por exemplo, um aplicativo React `package.json` pode conter: `scripts` configuração:

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

#### AEM cliente sem periféricos

O [Cliente autônomo do AEM para JavaScript](../../how-to/aem-headless-sdk.md) contém um cliente sem cabeçalho AEM que faz solicitações HTTP para AEM APIs GraphQL. O cliente sem cabeçalho do AEM deve ser inicializado com o host AEM com o qual ele interage, usando o valor do `.env` arquivo.

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

#### React useEffect(..) gancho

Custom React useOs ganchos de efeito chamam o cliente sem cabeçalho AEM, inicializado com o host AEM, em nome do componente React que renderiza a exibição.

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

#### Reagir componente

O gancho useEffect personalizado, `useAdventureByPath` é importado e usado para obter os dados usando o cliente Sem cabeçalho do AEM e, em última análise, renderizar o conteúdo para o usuário final.

+ &#39;src/components/AdventureDetail.js&#39;

```javascript
import { useAdventureByPath } from './api/persistedQueries';
...
// Query AEM GraphQL APIs via the useEffect hook that invokes the AEM Headless client initialized with the AEM host
let { data, error } = useAdventureByPath('/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg')

...
```

+++

+++ Exemplo iOS™

Esse exemplo, com base na variável [exemplo AEM aplicativo iOS™ headless](../../example-apps/ios-swiftui-app.md), ilustra como AEM solicitações de API GraphQL podem ser configuradas para se conectar a diferentes hosts AEM com base em [variáveis de configuração específicas da build](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3).

Os aplicativos iOS™ exigem que um cliente sem cabeçalho de AEM personalizado interaja com AEM APIs GraphQL. O cliente sem cabeçalho do AEM deve ser gravado de modo que o host do serviço de AEM possa ser configurado.

#### Configuração da build

O arquivo de configuração XCode contém os detalhes de configuração padrão.

+ `Config.xcconfig`

```swift
// The http/https protocol scheme used to access the AEM_HOST
AEM_SCHEME = https

// Target hostname for AEM service, do not include the scheme: http:// or https://
AEM_HOST = publish-p123-e789.adobeaemcloud.com
...
```

#### Inicializar o cliente AEM personalizado sem periféricos

O [exemplo AEM aplicativo iOS autônomo](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/ios-app) O usa um cliente AEM sem cabeçalho personalizado inicializado com os valores de configuração para `AEM_SCHEME` e `AEM_HOST`.

```swift
...
let aemScheme: String = try Configuration.value(for: "AEM_SCHEME")  // https
let aemHost: String = try Configuration.value(for: "AEM_HOST")      // publish-p123-e456.adobeaemcloud.com

let aemHeadlessClient = Aem(scheme: aemScheme, host: aemHost);
```

O cliente AEM personalizado sem periféricos (`api/Aem.swift`) contém um método `makeRequest(..)` que prefixa AEM solicitações de APIs GraphQL com o AEM configurado `scheme` e `host`.

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

[É possível criar novos arquivos de configuração de build](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3) para se conectar a diferentes serviços de AEM. Os valores específicos da build para a variável `AEM_SCHEME` e `AEM_HOST` são usadas com base na build selecionada no XCode, resultando no cliente personalizado AEM Headless para se conectar com o serviço de AEM correto.

+++

+++ Exemplo do Android™

Esse exemplo, com base na variável [exemplo AEM aplicativo Android™ headless](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app)O ilustra como AEM solicitações de API GraphQL podem ser configuradas para se conectar a diferentes AEM Services com base em variáveis de configuração específicas de compilação (ou de sabor).

Os aplicativos Android™ (quando escritos em Java™) devem usar o [Cliente autônomo AEM para Java™](https://github.com/adobe/aem-headless-client-java) para interagir com AEM APIs GraphQL. O cliente Sem Cabeça de AEM, fornecido pelo Cliente Sem Cabeça de AEM para Java™, deve ser inicializado com o host de Serviço de AEM ao qual ele se conecta.

#### Criar arquivo de configuração

Os aplicativos Android™ definem &quot;productFlavors&quot; que são usados para criar artefatos para diferentes usos.
Este exemplo mostra como dois sabores de produtos Android™ podem ser definidos, fornecendo diferentes hosts de serviço de AEM (`AEM_HOST`) valores para desenvolvimento (`dev`) e produção (`prod`).

No `build.gradle` arquivo, um novo `flavorDimension` nomeado `env` é criada.

No `env` dimensão, dois `productFlavors` são definidas: `dev` e `prod`. Cada `productFlavor` uses `buildConfigField` para definir variáveis específicas de criação que definem o serviço de AEM ao qual se conectar.

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

Inicialize o `AEMHeadlessClient` Construtor, fornecido pelo Cliente sem periféricos AEM para Java™ com o `AEM_HOST` do `buildConfigField` campo.

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

Ao criar o aplicativo Android™ para diferentes usos, especifique o `env` sabor e o valor de host AEM correspondente é usado.

+++

## URLs de imagem AEM

As solicitações de imagem do aplicativo sem cabeçalho para AEM devem ser configuradas para interagir com o serviço de AEM correto, conforme descrito na [tabela acima](#managing-aem-hosts).

Enquanto AEM GraphQL `... on ImageRef` fornece campos `_authorUrl` e `_publishUrl` contendo URLs absolutos para os respectivos serviços de AEM, geralmente é mais direto usar o `_path` e prefixar o host do serviço de AEM usado para consultar AEM APIs GraphQL.

Usando `_path` pode ser especialmente benéfico se o aplicativo sem cabeçalho puder se conectar ao AEM Author ou AEM Publish com base no contexto de implantação.

Se o aplicativo sem cabeçalho interagir exclusivamente com o AEM Author ou Publish, `_authorUrl` ou `_publishUrl` para simplificar a implementação, e as orientações nos exemplos abaixo podem ser ignoradas.

### Exemplos

A seguir estão exemplos de como os URLs de imagem podem prefixar o valor do host AEM, tornando-o configurável para várias estruturas de aplicativo sem cabeçalho. Os exemplos pressupõem o uso de consultas GraphQL que retornam referências de imagem usando o `_path` campo.

Por exemplo:

#### Consulta GraphQL mantida

Esta consulta GraphQL retorna uma referência de imagem `_path`. Como visto no [Resposta GraphQL](#examples-react-graphql-response) que exclui um host.

```graphql
query ($path: String!) {
  adventureByPath(_path: $path) {
    item {
      title,
      primaryImage {
        ... on ImageRef {
          _path
        }
      }
    }
  }
}
```

#### Resposta GraphQL

Essa resposta GraphQL retorna a `_path` que exclui um host.

```json
{
  "data": {
    "adventureByPath": {
      "item": {
        "adventurePrimaryImage": {
          "_path": "/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg",
        }
      }
    }
  }
}
```

+++ Exemplo de reação

Esse exemplo, com base na variável [exemplo AEM aplicativo Headless React](../../example-apps/react-app.md)O ilustra como os URLs de imagem podem ser configurados para se conectar aos Serviços de AEM corretos com base nas variáveis de ambiente.

Este exemplo mostra como o prefixo faz referência à imagem `_path` , com um `REACT_APP_AEM_HOST` Variável de ambiente de reação.

#### Reagir arquivo de ambiente

Reagir usos [arquivos de ambiente personalizados](https://create-react-app.dev/docs/adding-custom-environment-variables/)ou `.env` arquivos, armazenados na raiz do projeto para definir valores específicos da criação. Por exemplo, a variável `.env.development` O arquivo contém valores usados para durante o desenvolvimento, enquanto `.env.production` contém valores usados para builds de produção.

+ `.env.development`

```
# Environment variable used to specify the AEM service the React app will connect to when running under this profile
REACT_APP_AEM_HOST=https://publish-p123-e456.adobeaemcloud.com
...
```

`.env` arquivos para outros usos [pode ser especificado](https://create-react-app.dev/docs/adding-custom-environment-variables/#what-other-env-files-can-be-used) por postfix `.env` e um descritor semântico, como `.env.stage` ou `.env.production`. Different `.env` O arquivo pode ser usado ao executar ou criar o aplicativo React, definindo a variável `REACT_APP_ENV` antes de executar uma `npm` comando.

Por exemplo, um aplicativo React `package.json` pode conter: `scripts` configuração:

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

#### Reagir componente

O componente React importa o `REACT_APP_AEM_HOST` variável de ambiente e prefixos a imagem `_path` , para fornecer um URL de imagem totalmente resolvível.

Este mesmo `REACT_APP_AEM_HOST` A variável de ambiente é usada para inicializar o cliente sem cabeçalho AEM usado por `useAdventureByPath(..)` useEffect hook personalizado usado para buscar os dados GraphQL da AEM. Usando a mesma variável para criar a solicitação de API GraphQL como o URL da imagem, verifique se o aplicativo React interage com o mesmo serviço de AEM para ambos os casos de uso.

+ &#39;src/components/AdventureDetail.js&#39;

```javascript
...
// Import the AEM origin from the app's environment configuration
const AEM_HOST = env.process.REACT_APP_AEM_HOST; // https://publish-p123-e456.adobeaemcloud.com

let { data, error } = useAdventureByPath('/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg')

return (
    // Prefix the image src URL with the AEM host
    <img src={AEM_HOST + data.adventureByPath.item.primaryImage._path }>
    {/* Resulting in: <img src="https://publish-p123-e456.adobeaemcloud.com/content/dam/wknd-shared/en/adventures/bali-surf-camp/adobestock-175749320.jpg"/>  */}
)
```

+++

+++ Exemplo iOS™

Esse exemplo, com base na variável [exemplo AEM aplicativo iOS™ headless](../../example-apps/ios-swiftui-app.md), ilustra como AEM URLs de imagem podem ser configuradas para se conectar a diferentes hosts AEM com base em [variáveis de configuração específicas da build](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3).

#### Configuração da build

O arquivo de configuração XCode contém os detalhes de configuração padrão.

+ `Config.xcconfig`

```swift
// The http/https protocol scheme used to access the AEM_HOST
AEM_SCHEME = https

// Target hostname for AEM service, do not include the scheme: http:// or https://
AEM_HOST = publish-p123-e789.adobeaemcloud.com
...
```

#### Gerador de URL da imagem

Em `Aem.swift`, a implementação personalizada AEM sem cabeçalho do cliente, uma função personalizada `imageUrl(..)` pega o caminho da imagem, como fornecido na variável `_path` na resposta GraphLQ e a prepara com AEM host. Essa função é então invocada nas exibições do iOS sempre que uma imagem é renderizada.

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
    /// Prefixes AEM image paths wit the AEM scheme/host
    func imageUrl(path: String) -> URL {
        return URL(string: "\(self.scheme)://\(self.host)\(path)")!
    }
    ...
}
```

#### Exibição do iOS

A exibição do iOS e os prefixos da imagem `_path` , para fornecer um URL de imagem totalmente resolvível.

+ `WKNDAdventures/Views/AdventureListItemView.swift`

```swift
import SDWebImageSwiftUI
...
struct AdventureListItemView: View {
    @EnvironmentObject private var aem: Aem

    var adventure: Adventure
    
    var body: some View {
        HStack {
            // Path the image path to `aem.imageUrl(..)` to prepend the AEM service host     
            AdventureRowImage(imageUrl: aem.imageUrl(path: adventure.image()))
            Text(adventure.title)
            Spacer()
        }
    }
}
...
```

[É possível criar novos arquivos de configuração de build](https://developer.apple.com/documentation/xcode/adding-a-build-configuration-file-to-your-project?changes=l_3) para se conectar a diferentes serviços de AEM. Os valores específicos da build para a variável `AEM_SCHEME` e `AEM_HOST` são usadas com base na build selecionada no XCode, resultando no cliente personalizado AEM Headless para interagir com o serviço de AEM correto.

+++

+++ Exemplo do Android™

Esse exemplo, com base na variável [exemplo AEM aplicativo Android™ headless](https://github.com/adobe/aem-guides-wknd-graphql/tree/main/android-app)O ilustra como AEM URLs de imagem podem ser configuradas para se conectar a diferentes Serviços AEM com base em variáveis de configuração específicas de criação (ou de sabor).

#### Criar arquivo de configuração

Os aplicativos Android™ definem &quot;productFlavors&quot;, que são usados para criar artefatos para diferentes usos.
Este exemplo mostra como dois sabores de produtos Android™ podem ser definidos, fornecendo diferentes hosts de serviço de AEM (`AEM_HOST`) valores para desenvolvimento (`dev`) e produção (`prod`).

No `build.gradle` arquivo, um novo `flavorDimension` nomeado `env` é criada.

No `env` dimensão, dois `productFlavors` são definidas: `dev` e `prod`. Cada `productFlavor` uses `buildConfigField` para definir variáveis específicas de criação que definem o serviço de AEM ao qual se conectar.

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

#### Carregamento da imagem de AEM

O Android™ usa um `ImageGetter` para buscar e armazenar em cache localmente os dados de imagem do AEM. Em `prepareDrawableFor(..)` o host do serviço de AEM, definido na configuração de build ativa, é usado para prefixar o caminho da imagem criando um URL resolvível para AEM.

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
    public Drawable getDrawable(String path) {
        // Get the image data from the cache using the path as the key
        Drawable drawable = drawablesByPath.get(path);
        return drawable;
    }
}
```

#### Exibição do Android™

A exibição do Android™ obtém os dados da imagem por meio da `RemoteImagesCache` usando o `_path` valor da resposta GraphQL.

+ `app/src/main/java/com/adobe/wknd/androidapp/AdventureDetailFragment.java`

```java
...
public class AdventureDetailFragment extends Fragment implements LoaderManager.LoaderCallbacks<Adventure> {
    ...
    private ImageView adventureDetailImage;
    ...

    private void updateContent() {
        ...
        adventureDetailImage.setImageDrawable(RemoteImagesCache.getInstance().getDrawable(adventure.getPrimaryImagePath()));
        ...
    }
...
}
```

Ao criar o aplicativo Android™ para diferentes usos, especifique o `env` sabor e o valor de host AEM correspondente é usado.

+++