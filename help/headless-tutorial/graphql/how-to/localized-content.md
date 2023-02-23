---
title: Uso de conteúdo localizado com AEM headless
description: Saiba como usar o GraphQL para consultar AEM de conteúdo localizado.
version: Cloud Service
feature: GraphQL API
topic: Headless
role: Developer
level: Intermediate
kt: 10254
thumbnail: KT-10254.jpeg
source-git-commit: ae49fb45db6f075a34ae67475f2fcc5658cb0413
workflow-type: tm+mt
source-wordcount: '508'
ht-degree: 3%

---


# Conteúdo localizado com AEM headless

AEM fornece uma [Estrutura de integração de tradução](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/sites/administering/reusing-content/translation/integration-framework.html) para conteúdo sem interface, permitindo que Fragmentos de conteúdo e ativos de suporte sejam facilmente traduzidos para uso em locais. Essa é a mesma estrutura usada para traduzir outro conteúdo AEM, como Páginas, Fragmentos de experiência, Ativos e Forms. Uma vez [o conteúdo sem cabeçalho foi traduzido](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/journeys/translation/overview.html?lang=pt-BR), e publicado, está pronto para o consumo por aplicações sem interface.

## Estrutura da pasta Ativos{#assets-folder-structure}

Verifique se os Fragmentos de conteúdo localizados no AEM seguem o [estrutura de localização recomendada](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/headless/journeys/translation/getting-started.html#recommended-structure).

![AEM pastas de ativos localizados](./assets/localized-content/asset-folders.jpg)

As pastas de localidade devem ser irmãos, e o nome da pasta, em vez do título, deve ser válido [Código ISO 639-1](https://en.wikipedia.org/wiki/List_of_ISO_639-1_codes) representando a localidade do conteúdo contido na pasta.

O código de local também é o valor usado para filtrar os Fragmentos de conteúdo retornados pela consulta GraphQL.

| Código de localidade | Caminho AEM | Localidade do conteúdo |
|--------------------------------|----------|----------|
| de | /content/dam/../**de**/... | Conteúdo alemão |
| en | /content/dam/../**en**/... | Conteúdo em inglês |
| es | /content/dam/../**es**/... | Conteúdo espanhol |

## Consulta persistente do GraphQL

AEM fornece uma `_locale` Filtro GraphQL que filtra automaticamente o conteúdo por código de localidade . Por exemplo, querendo todas as aventuras em inglês no [Projeto de Site WKND](https://github.com/adobe/aem-guides-wknd) pode ser feita com uma nova consulta persistente `wknd-shared/adventures-by-locale` definido como:

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

O `$locale` usada na variável `_locale` o filtro requer o código de localidade (por exemplo, `en`, `en_us`ou `de`), conforme especificado em [AEM convenção de localização baseada em pastas de ativos](#assets-folder-structure).

## Exemplo de reação

Vamos criar um aplicativo React simples que controla o conteúdo da Adventure para ser consultado AEM com base em um seletor de localidade usando a variável `_locale` filtro.

When __Inglês__ está selecionada no seletor de localidade, em seguida, em Inglês Aventurar fragmentos de conteúdo em `/content/dam/wknd/en` são retornadas, quando __Espanhol__ está selecionada, em seguida, Fragmentos de conteúdo em espanhol em `/content/dam/wknd/es`e assim por diante, e assim por diante.

![Aplicativo de exemplo React localizado](./assets/localized-content/react-example.png)

### Crie um `LocaleContext`{#locale-context}

Primeiro, crie um [Contexto de reação](https://reactjs.org/docs/context.html) para permitir que a localidade seja usada em todos os componentes do aplicativo React.

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

### Crie um `LocaleSwitcher` Reagir componente{#locale-switcher}

Em seguida, crie um componente de Reação do alternador de localidade que defina o [LocaleContext](#locale-context) para a seleção do usuário.

Esse valor de localidade é usado para direcionar as consultas do GraphQL, garantindo que elas retornem somente o conteúdo correspondente ao local selecionado.

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

### Consultar o conteúdo usando o `_locale` filter{#adventures}

As consultas do componente Aventuras AEM para todas as aventuras por localidade e listam seus títulos. Isso é feito transmitindo o valor da localidade armazenado no contexto React, ao query usando a variável `_locale` filtro.

Essa abordagem pode ser estendida a outras consultas em seu aplicativo, garantindo que todas as consultas incluam apenas o conteúdo especificado pela seleção de local de um usuário.

A consulta contra AEM é realizada no gancho React personalizado [getAdventuresByLocale, descrito em mais detalhes na documentação Consulta AEM GraphQL](./aem-headless-sdk.md).

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

### Defina as `App.js`{#app-js}

Por fim, vincule tudo isso vinculando o aplicativo React ao `LanguageContext.Provider` e definir o valor da localidade. Isso permite que os outros componentes React , [LocaleSwitcher](#locale-switcher)e [Aventuras](#adventures) para compartilhar o estado de seleção da localidade.

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
