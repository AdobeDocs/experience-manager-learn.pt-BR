---
title: Funções personalizadas no AEM Forms
description: Criar e usar funções personalizadas no Formulário adaptável
feature: Adaptive Forms
version: 6.5
topic: Development
role: User
level: Beginner
kt: 9685
source-git-commit: 15b57ec6792bc47d0041946014863b13867adf22
workflow-type: tm+mt
source-wordcount: '327'
ht-degree: 0%

---

# Funções personalizadas

O AEM Forms 6.5 apresentou a capacidade de definir funções JavaScript que podem ser usadas na definição de regras comerciais complexas usando o editor de regras.
O AEM Forms fornece várias funções personalizadas prontas para uso, mas você terá que definir suas próprias funções personalizadas e usá-las em vários formulários.

Para definir sua primeira função personalizada, siga as seguintes etapas:
* [Faça logon no crx](http://localhost:4502/crx/de/index.jsp#/apps/experience-league/clientlibs)
* Crie uma nova pasta em aplicativos chamados experience-league (esse nome de pasta pode ser o nome de sua escolha)
* Salve as alterações.
* Em uma pasta da experience-league, crie um novo nó do tipo cq:ClientLibraryFolder chamado clientlibs.
* Selecione a pasta clientlibs recém-criada e adicione as propriedades allowProxy e categories conforme mostrado na captura de tela e salve as alterações.

![client-lib](assets/custom-functions.png)
* Crie uma pasta chamada **js** nos termos do **clientlibs** pasta
* Crie um arquivo chamado **functions.js** nos termos do **js** pasta
* Crie um arquivo chamado **js.txt** nos termos do **clientlibs** pasta. Salve as alterações.
* A estrutura da pasta deve ser parecida com a captura de tela abaixo.

![Editor de regras](assets/folder-structure.png)

* Clique duas vezes em functions.js para abrir o editor.
Copie o código a seguir em functions.js e salve as alterações.

```javascript
/**
* Get List of County names
* @name getCountyNamesList Get list of county names
* @return {OPTIONS} drop down options 
 */
function getCountyNamesList()
{
    var countyNames= [];
	countyNames[0] = "Santa Clara";
	countyNames[1] = "Alameda";
	countyNames[2] = "Buxor";
    countyNames[3] = "Contra Costa";
    countyNames[4] = "Merced";

	return countyNames;

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

Por favor [consulte jsdoc ](https://jsdoc.app/index.html)para obter mais detalhes sobre a anotação de funções javascript.
O código acima tem duas funções:
**getCountyNamesList** - retorna uma matriz de string
**conversionUTC** - Converte o carimbo de data/hora UTC em fuso horário local

Abra o js.txt e cole o código a seguir e salve as alterações.

```javascript
#base=js
functions.js
```

A linha #base=js especifica em qual diretório os arquivos JavaScript estão localizados.
As linhas abaixo indicam o local do arquivo JavaScript em relação ao local base.

Se tiver problemas para criar funções personalizadas, sinta-se à vontade para [baixar e instalar este pacote](assets/custom-functions.zip) na sua instância de AEM.

## Uso das funções personalizadas

O vídeo a seguir o orienta pelas etapas envolvidas no uso de funções personalizadas no editor de regras de um formulário adaptável
>[!VIDEO](https://video.tv.adobe.com/v/340305?quality=9&learn=on)
