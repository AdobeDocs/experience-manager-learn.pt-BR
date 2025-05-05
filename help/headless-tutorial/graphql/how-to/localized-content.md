---
title: Usar conteúdo localizado com o AEM Headless
description: Saiba como usar o GraphQL para consultar o conteúdo localizado no AEM.
version: Experience Manager as a Cloud Service
feature: GraphQL API
topic: Headless
role: Developer
level: Intermediate
jira: KT-10254
thumbnail: KT-10254.jpeg
exl-id: 5e3d115b-f3a1-4edc-86ab-3e0713a36d54
duration: 130
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '472'
ht-degree: 0%

---

# Conteúdo localizado com o AEM Headless

O AEM fornece uma [estrutura de integração de tradução](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/reusing-content/translation/integration-framework.html?lang=pt-BR) para conteúdo headless, permitindo que os Fragmentos de conteúdo e ativos de suporte sejam facilmente traduzidos para uso em localidades. Essa é a mesma estrutura usada para traduzir outro conteúdo do AEM, como Páginas, Fragmentos de experiência, Assets e Forms. Depois que o conteúdo [headless for traduzido](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/journeys/translation/overview.html?lang=pt-BR) e publicado, ele estará pronto para consumo por aplicativos headless.

## Estrutura de pastas do Assets{#assets-folder-structure}

Verifique se os fragmentos de conteúdo localizados no AEM seguem a [estrutura de localização recomendada](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/journeys/translation/getting-started.html?lang=pt-BR#recommended-structure).

![Pastas localizadas de ativos da AEM](./assets/localized-content/asset-folders.jpg)

As pastas de localidade devem ser irmãs e o nome da pasta, em vez do título, deve ser um [código ISO 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) válido representando a localidade do conteúdo contido na pasta.

O código de localidade também é o valor usado para filtrar os fragmentos de conteúdo retornados pela consulta do GraphQL.

| Código da localidade | Caminho do AEM | Local do conteúdo |
|--------------------------------|----------|----------|
| de | /content/dam/.../**de**/... | Conteúdo em alemão |
| en | /content/dam/.../**en**/... | Conteúdo em inglês |
| es | /content/dam/.../**es**/... | Conteúdo em espanhol |

## Consulta persistente do GraphQL

O AEM fornece um filtro GraphQL `_locale` que filtra automaticamente o conteúdo por código de localidade. Por exemplo, a consulta de todas as aventuras em inglês no [projeto do Site WKND](https://github.com/adobe/aem-guides-wknd) pode ser feita com uma nova consulta persistente `wknd-shared/adventures-by-locale` definida como:

```graphql
query($locale: String!) {
  adventureList(_locale: $locale) {
    items {      
      _path
      title
    }
  }
}
```

A variável `$locale` usada no filtro `_locale` requer o código de localidade (por exemplo `en`, `en_us` ou `de`) conforme especificado na [convenção de localização baseada em pasta de ativos da AEM](#assets-folder-structure).

## Exemplo do React

Vamos criar um aplicativo React simples que controla qual conteúdo Adventure consultar do AEM com base em um seletor de localidade usando o filtro `_locale`.

Quando __Inglês__ é selecionado no seletor de localidades, os Fragmentos de Conteúdo de Aventura em Inglês `/content/dam/wknd/en` são retornados, quando __Espanhol__ é selecionado, os Fragmentos de Conteúdo em Espanhol `/content/dam/wknd/es` e assim por diante.

![Aplicativo de exemplo localizado do React](./assets/localized-content/react-example.png)

### Criar um `LocaleContext`{#locale-context}

Primeiro, crie um [Contexto do React](https://reactjs.org/docs/context.html) para permitir que a localidade seja usada nos componentes do aplicativo React.

```javascript
// src/LocaleContext.js

import React from 'react'

const DEFAULT_LOCALE = 'en';

const LocaleContext = React.createContext({
    locale: DEFAULT_LOCALE, 
    setLocale: () => {}
});

export default LocaleContext;
```

### Criar um componente React `LocaleSwitcher`{#locale-switcher}

Em seguida, crie um componente React do alternador de local que defina o valor [LocaleContext](#locale-context) para a seleção do usuário.

Esse valor de localidade é usado para direcionar as consultas do GraphQL, garantindo que elas retornem apenas o conteúdo correspondente à localidade selecionada.

```javascript
// src/LocaleSwitcher.js

import { useContext } from "react";
import LocaleContext from "./LocaleContext";

export default function LocaleSwitcher() {
  const { locale, setLocale } = useContext(LocaleContext);

  return (
    <select value={locale}
            onChange={e => setLocale(e.target.value)}>
      <option value="de">Deutsch</option>
      <option value="en">English</option>
      <option value="es">Español</option>
    </select>
  );
}
```

### Consultar conteúdo usando o filtro `_locale`{#adventures}

O componente Aventuras consulta o AEM para todas as aventuras por localidade e lista seus títulos. Isso é feito passando o valor da localidade armazenado no contexto do React, para a consulta usando o filtro `_locale`.

Essa abordagem pode ser estendida para outras consultas em seu aplicativo, garantindo que todas as consultas incluam apenas o conteúdo especificado pela seleção de localidade de um usuário.

A consulta em relação ao AEM é executada no gancho personalizado React [getAdventuresByLocale, descrito com mais detalhes na documentação de Consulta do AEM GraphQL](./aem-headless-sdk.md).

```javascript
// src/Adventures.js

import { useContext } from "react"
import { useAdventuresByLocale } from './api/persistedQueries'
import LocaleContext from './LocaleContext'

export default function Adventures() {
    const { locale } = useContext(LocaleContext);

    // Get data from AEM using GraphQL persisted query as defined above 
    // The details of defining a React useEffect hook are explored in How to > AEM Headless SDK
    let { data, error } = useAdventuresByLocale(locale);

    return (
        <ul>
            {data?.adventureList?.items?.map((adventure, index) => { 
                return <li key={index}>{adventure.title}</li>
            })}
        </ul>
    )
}
```

### Definir o `App.js`{#app-js}

Por fim, junte tudo isso encapsulando o aplicativo React com o `LanguageContext.Provider` e definindo o valor de localidade. Isso permite que os outros componentes do React, [LocaleSwitcher](#locale-switcher) e [Adventures](#adventures) compartilhem o estado de seleção de localidade.

```javascript
// src/App.js

import { useState, useContext } from "react";
import LocaleContext from "./LocaleContext";
import LocaleSwitcher from "./LocaleSwitcher";
import Adventures from "./Adventures";

export default function App() {
  const [locale, setLocale] = useState(useContext(LocaleContext).locale);

  return (
    <LocaleContext.Provider value={{locale, setLocale}}>
      <LocaleSwitcher />
      <Adventures />
    </LocaleContext.Provider>
  );
}
```
