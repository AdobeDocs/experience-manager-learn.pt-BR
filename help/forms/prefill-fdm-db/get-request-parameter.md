---
title: Obter parâmetro de solicitação
description: Acesse o parâmetro de solicitação em um serviço de preenchimento prévio do modelo de dados de formulário
feature: Adaptive Forms
topics: development
audience: developer
doc-type: article
activity: implement
version: 6.4,6.5
kt: 5815
thumbnail: kt-5815.jpg
topic: Development
role: Developer
level: Beginner
translation-type: tm+mt
source-git-commit: d9714b9a291ec3ee5f3dba9723de72bb120d2149
workflow-type: tm+mt
source-wordcount: '184'
ht-degree: 4%

---

# Obter parâmetro de solicitação

## Obter parâmetro empID

A próxima etapa é acessar o parâmetro empID a partir do url. O valor do parâmetro de solicitação empID é então passado para a operação de serviço **_get_** do modelo de dados de formulário.
Para o objetivo deste curso, criamos e fornecemos o seguinte

* Modelo de formulário adaptável chamado **_FDMDemo_**
* Componente de página chamado **_fdmdemo_**
* Incluído nosso jsp personalizado com o componente página
* Associado o modelo de formulário adaptável ao componente de página

Ao fazer isso, nosso código no jsp personalizado só será executado quando o formulário adaptável baseado nesse modelo personalizado for renderizado

* [Importe o ](assets/template-page-component.zip) pacote usando o gerenciador  [de pacotes](http://localhost:4502/crx/packmgr/index.jsp)
* [Abra fdmrequest.jsp](http://localhost:4502/crx/de/index.jsp#/apps/fdmdemo/component/page/fdmdemo/fdmrequest.jsp)
* Exclua as linhas comentadas.
* Salve as alterações

```java
if(request.getParameter("empID")!=null)
    {
      //System.out.println("Adobe !!!There is a empID parameter in the request "+request.getParameter("empID"));
      //java.util.Map paraMap = new java.util.HashMap();
      //paraMap.put("empID",request.getParameter("empID"));
      //slingRequest.setAttribute("paramMap",paraMap);
    }
```

O valor de empID está associado à chave chamada empID no paraMap. Este mapa é então passado para o slingRequest

>[!NOTE]
>
>A chave empID deve corresponder ao valor de vínculo das novas entidades que obtêm o serviço
