---
title: Obter parâmetro de solicitação
description: Acessar o parâmetro de solicitação do serviço de preenchimento do modelo de dados de formulário
feature: Adaptive Forms
version: 6.4,6.5
jira: KT-5815
thumbnail: kt-5815.jpg
topic: Development
role: Developer
level: Beginner
exl-id: a640539d-c67f-4224-ad81-dd0b62e18c79
duration: 40
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '176'
ht-degree: 1%

---

# Obter parâmetro de solicitação

## Obter parâmetro empID

A próxima etapa é acessar o parâmetro empID no url. O valor do parâmetro de solicitação empID é passado para o **_obter_** operação de serviço do modelo de dados de formulário.
Para o propósito deste curso criamos e fornecemos o seguinte

* Modelo de formulário adaptável chamado **_FDMDemo_**
* Componente de Página chamado **_fdmdemo_**
* Foi incluído nosso jsp personalizado no componente Página
* Modelo de formulário adaptável associado ao componente Página

Ao fazer isso, nosso código no jsp personalizado só será executado quando o formulário adaptável baseado nesse modelo personalizado for renderizado

* [Importar o pacote](assets/template-page-component.zip) usar [gerenciador de pacotes](http://localhost:4502/crx/packmgr/index.jsp)
* [Abrir fdmrequest.jsp](http://localhost:4502/crx/de/index.jsp#/apps/fdmdemo/component/page/fdmdemo/fdmrequest.jsp)
* Remova o comentário das linhas comentadas.
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
>A chave empID deve corresponder ao valor de ligação das novas entidades obterem serviço

## Próximas etapas

[Criar um formulário adaptável com base no modelo de dados de formulário](./create-adaptive-form.md)
