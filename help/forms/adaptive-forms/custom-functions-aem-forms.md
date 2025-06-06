---
title: Funções personalizadas no AEM Forms
description: Criar e usar funções personalizadas no Formulário adaptável
feature: Adaptive Forms
version: Experience Manager 6.5
topic: Development
role: User
level: Beginner
jira: KT-9685
exl-id: 07fed661-0995-41ab-90c4-abde35a14a4c
last-substantial-update: 2021-06-09T00:00:00Z
duration: 286
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '319'
ht-degree: 0%

---

# Funções personalizadas

O AEM Forms 6.5 apresentou a capacidade de definir funções do JavaScript que podem ser usadas na definição de regras de negócios complexas usando o editor de regras.
O AEM Forms fornece várias dessas funções personalizadas prontas para uso, mas você terá a necessidade de definir suas próprias funções personalizadas e usá-las em vários formulários.

Para definir sua primeira função personalizada, siga as seguintes etapas:
* [Fazer logon no crx](http://localhost:4502/crx/de/index.jsp#/apps/experience-league/clientlibs)
* Crie uma nova pasta em aplicativos chamada experience-league (esse nome de pasta pode ser o nome de sua escolha)
* Salve as alterações.
* Na pasta experience-league, crie um novo nó do tipo cq:ClientLibraryFolder chamado clientlibs.
* Selecione a pasta recém-criada clientlibs e adicione as propriedades allowProxy e categories como mostrado na captura de tela e salve as alterações.

![client-lib](assets/custom-functions.png)
* Crie uma pasta chamada **js** na pasta **clientlibs**
* Crie um arquivo chamado **functions.js** na pasta **js**
* Crie um arquivo chamado **js.txt** na pasta **clientlibs**. Salve as alterações.
* A estrutura de pastas deve parecer com a captura de tela abaixo.

![Editor de regras](assets/folder-structure.png)

* Clique duas vezes em functions.js para abrir o editor.
Copie o código a seguir em functions.js e salve as alterações.

```javascript
/**
* Get List of County names
* @name getCountyNamesList Get list of county names
* @returns {string[]} An array of county names
 */
function getCountyNamesList()
{
    return ["Santa Clara", "Alameda", "Buxor", "Contra Costa", "Merced"];

}
/**
* Covert UTC to Local Time
* @name convertUTC Convert UTC Time to Local Time
* @param {string} strUTCString in Stringformat
* @return {string}
*/
function convertUTC(strUTCString)
{
    var dt = new Date(strUTCString);
    console.log(dt.toLocaleString());
    return dt.toLocaleString();
}
```

[Consulte jsdoc](https://jsdoc.app/index.html)para obter mais detalhes sobre como anotar funções javascript.
O código acima tem duas funções:
**getCountyNamesList** - retorna uma matriz de cadeia de caracteres
**convertUTC** - Converte o carimbo de data e hora UTC para a data e a hora locais

Abra o js.txt, cole o código a seguir e salve as alterações.

```javascript
#base=js
functions.js
```

A linha #base=js especifica em qual diretório os arquivos JavaScript estão localizados.
As linhas abaixo indicam o local do arquivo JavaScript relativo ao local base.

Se tiver problemas para criar as funções personalizadas, sinta-se à vontade para [baixar e instalar este pacote](assets/custom-functions.zip) na sua instância do AEM.

## Uso das funções personalizadas

O vídeo a seguir mostra as etapas envolvidas no uso da função personalizada no editor de regras de um formulário adaptável
>[!VIDEO](https://video.tv.adobe.com/v/3445846?quality=12&learn=on&captions=por_br)
