---
title: Uso de conteúdo localizado com AEM Headless
description: Saiba como usar o GraphQL para consultar AEM em busca de conteúdo localizado.
version: Cloud Service
feature: GraphQL API
topic: Headless
role: Developer
level: Intermediate
jira: KT-10254
thumbnail: KT-10254.jpeg
exl-id: 5e3d115b-f3a1-4edc-86ab-3e0713a36d54
duration: 130
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '472'
ht-degree: 0%

---

# Conteúdo localizado com AEM Headless

O AEM fornece uma [Estrutura de integração de tradução](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/reusing-content/translation/integration-framework.html) para conteúdo headless, permitindo que os Fragmentos de conteúdo e ativos de suporte sejam facilmente traduzidos para uso em localidades. Essa é a mesma estrutura usada para traduzir outro conteúdo AEM, como Páginas, Fragmentos de experiência, Ativos e Forms. Uma vez [o conteúdo headless foi traduzido](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/journeys/translation/overview.html?lang=pt-BR)e publicado, está pronto para ser consumido por aplicativos headless.

## Estrutura da pasta de ativos{#assets-folder-structure}

Verifique se os Fragmentos de conteúdo localizados no AEM seguem o [estrutura de localização recomendada](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/journeys/translation/getting-started.html#recommended-structure).

![Pastas localizadas de ativos do AEM](./assets/localized-content/asset-folders.jpg)

As pastas de local devem ser irmãs e o nome da pasta, em vez do título, deve ser válido [Código ISO 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) que representa o local do conteúdo contido na pasta.

O código de localidade também é o valor usado para filtrar os fragmentos de conteúdo retornados pela consulta do GraphQL.

| Código da localidade | Caminho AEM | Local do conteúdo |
|--------------------------------|----------|----------|
| de | /content/dam/.../**de**/... | Conteúdo em alemão |
| en | /content/dam/.../**en**/... | Conteúdo em inglês |
| es | /content/dam/.../**es**/... | Conteúdo em espanhol |

## Consulta persistente do GraphQL

O AEM fornece uma `_locale` Filtro do GraphQL que filtra automaticamente o conteúdo por código de localidade. Por exemplo, consultar todas as aventuras em inglês no [Projeto do site WKND](https://github.com/adobe/aem-guides-wknd) pode ser feita com uma nova consulta persistente `wknd-shared/adventures-by-locale` definido como:

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

A variável `$locale` variável usada no `_locale` O filtro exige o código de localidade (por exemplo, `en`, `en_us`ou `de`) conforme especificado no [Convenção de localização da base de pastas de ativos do AEM](#assets-folder-structure).

## Exemplo do React

Vamos criar um aplicativo React simples que controla qual conteúdo Adventure consultar do AEM com base em um seletor de localidade usando o `_locale` filtro.

Quando __Inglês__ é selecionado no seletor de local e Fragmentos de conteúdo de aventura em inglês, em `/content/dam/wknd/en` são retornados, quando __Espanhol__ estiver selecionada, depois Fragmentos de conteúdo em espanhol em `/content/dam/wknd/es`e assim por diante.

![Aplicativo de exemplo React localizado](./assets/localized-content/react-example.png)

### Criar um `LocaleContext`{#locale-context}

Primeiro, crie um [Contexto do React](https://reactjs.org/docs/context.html) para permitir que o local seja usado nos componentes do aplicativo React.

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

### Criar um `LocaleSwitcher` Componente do React{#locale-switcher}

Em seguida, crie um componente React do alternador de local que defina como [ContextoLocal](#locale-context) para a seleção do usuário.

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

### Consultar conteúdo usando o `_locale` filtro{#adventures}

O componente Aventuras consulta o AEM para todas as aventuras por localidade e lista seus títulos. Isso é feito passando o valor do local armazenado no contexto do React, para a consulta usando o `_locale` filtro.

Essa abordagem pode ser estendida para outras consultas em seu aplicativo, garantindo que todas as consultas incluam apenas o conteúdo especificado pela seleção de localidade de um usuário.

A consulta ao AEM é executada no gancho React personalizado [getAdventuresByLocale, descrito com mais detalhes na documentação do Querying AEM GraphQL](./aem-headless-sdk.md).

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

### Defina o `App.js`{#app-js}

Por último, agrupe-o embrulhando o aplicativo React com o `LanguageContext.Provider` e definindo o valor do local. Isso permite que os outros componentes do React, [LocaleSwitcher](#locale-switcher), e [Aventuras](#adventures) para compartilhar o estado de seleção de local.

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
