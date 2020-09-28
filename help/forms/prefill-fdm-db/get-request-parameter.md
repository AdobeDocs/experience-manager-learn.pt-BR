---
title: Obter parâmetro de solicitação
description: Acessar o parâmetro de solicitação em um serviço de preenchimento prévio do modelo de dados de formulário
feature: adaptive-forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5815
thumbnail: kt-5815.jpg
translation-type: tm+mt
source-git-commit: a0e5a99408237c367ea075762ffeb3b9e9a5d8eb
workflow-type: tm+mt
source-wordcount: '179'
ht-degree: 0%

---

# Obter parâmetro de solicitação

## Obter parâmetro empID

A próxima etapa é acessar o parâmetro empID a partir do url. O valor do parâmetro de solicitação empID é então passado para a operação de **_obtenção_** do serviço do modelo de dados de formulário.
Para a finalidade deste curso, criamos e fornecemos o seguinte

* Modelo de formulário adaptável chamado **_FDMDemo_**
* Componente de página chamado **_fdmdemo_**
* Incluído nosso jsp personalizado com o componente de página
* Associado o modelo de formulário adaptativo ao componente de página

Fazendo isso, nosso código no jsp personalizado só será executado quando o formulário adaptável baseado nesse modelo personalizado for renderizado

* [Importar o pacote](assets/template-page-component.zip) usando o gerenciador de [pacotes](http://localhost:4502/crx/packmgr/index.jsp)
* [Abra fdmrequest.jsp](http://localhost:4502/crx/de/index.jsp#/apps/fdmdemo/component/page/fdmdemo/fdmrequest.jsp)
* Exclua as barras de comentário.
* Salvar suas alterações

```java
if(request.getParameter("empID")!=null)
    {
      //System.out.println("Adobe !!!There is a empID parameter in the request "+request.getParameter("empID"));
      //java.util.Map paraMap = new java.util.HashMap();
      //paraMap.put("empID",request.getParameter("empID"));
      //slingRequest.setAttribute("paramMap",paraMap);
    }
```

O valor de empID está associado à chave chamada empID no paraMap. Este mapa é então passado para slingRequest

>[!NOTE]
>
>A empID da chave deve corresponder ao valor de vínculo das novas entidades para obter o serviço
