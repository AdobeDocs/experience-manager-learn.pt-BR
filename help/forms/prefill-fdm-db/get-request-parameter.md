---
title: Obter parâmetro de solicitação
description: Acesse o parâmetro de solicitação em um serviço de preenchimento prévio do modelo de dados de formulário
feature: Adaptive Forms
version: 6.4,6.5
kt: 5815
thumbnail: kt-5815.jpg
topic: Development
role: Developer
level: Beginner
exl-id: a640539d-c67f-4224-ad81-dd0b62e18c79
source-git-commit: bd41cd9d64253413e793479b5ba900c8e01c0eab
workflow-type: tm+mt
source-wordcount: '190'
ht-degree: 3%

---

# Obter parâmetro de solicitação

## Obter parâmetro empID

A próxima etapa é acessar o parâmetro empID a partir do url. O valor do parâmetro de solicitação empID é então passado para o **_get_** operação de serviço do modelo de dados de formulário.
Para o objetivo deste curso, criamos e fornecemos o seguinte

* Modelo de formulário adaptável chamado **_FDMDemo_**
* Componente de página chamado **_fdmdemo_**
* Incluído nosso jsp personalizado com o componente página
* Associado o modelo de formulário adaptável ao componente de página

Ao fazer isso, nosso código no jsp personalizado só será executado quando o formulário adaptável baseado nesse modelo personalizado for renderizado

* [Importe o pacote](assets/template-page-component.zip) usar [gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp)
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

## Próximas etapas

[Criar um formulário adaptável com base no modelo de dados de formulário](./create-adaptive-form.md)
