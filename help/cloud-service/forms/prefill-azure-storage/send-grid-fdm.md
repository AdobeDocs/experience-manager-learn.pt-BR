---
title: Enviar email usando SendGrid
description: Acionar um email com um link para o formulário salvo
feature: Adaptive Forms
type: Documentation
role: Developer
level: Beginner
version: Experience Manager as a Cloud Service
topic: Integrations
jira: KT-13717
exl-id: 4b2d1e50-9fa1-4934-820b-7dae984cee00
duration: 37
source-git-commit: 48433a5367c281cf5a1c106b08a1306f1b0e8ef4
workflow-type: tm+mt
source-wordcount: '183'
ht-degree: 1%

---

# Integração com o SendGrid

A Integração de dados do AEM Forms permite configurar e conectar fontes de dados diferentes ao AEM Forms. Ele fornece uma interface de usuário intuitiva para criar um esquema de representação de dados unificada de entidades e serviços comerciais em fontes de dados conectadas.

Usamos a API SendGrid para enviar emails usando um modelo dinâmico. Presume-se que você esteja familiarizado com a API SendGrid para enviar emails usando modelos dinâmicos. Um arquivo swagger para descrever para a API foi fornecido a você como parte deste tutorial.

## Criar a integração

Siga as etapas a seguir para criar a integração entre o AEM Forms e o SendGrid

* Crie uma fonte de dados RESTful usando o [arquivo swagger](./assets/SendGridWithDynamicTemplate.yaml). [Siga este vídeo para obter instruções detalhadas](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) sobre como criar fontes de dados no AEM Forms
* Criar modelo de dados de formulário com base na fonte de dados criada na etapa anterior.[Siga a documentação detalhada](https://experienceleague.adobe.com/docs/experience-manager-cloud-service/content/forms/integrate/use-form-data-model/create-form-data-models.html) sobre como criar o modelo de dados de formulário.

O modelo de dados de formulário criado para este tutorial é incluído como parte dos ativos do artigo.

### Próximas etapas

[Criar integração de armazenamento do Azure](./create-fdm.md)
