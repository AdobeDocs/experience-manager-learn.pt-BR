---
title: Integrar o AEM Forms com o SendGrid
description: Aproveite a plataforma de entrega de email baseada na nuvem do SengGrid usando o AEM Forms.
feature: Adaptive Forms
version: Experience Manager 6.4, Experience Manager 6.5
jira: KT-13605
topic: Development
role: Developer
level: Intermediate
last-substantial-update: 2023-07-14T00:00:00Z
exl-id: 62b73f4b-69d8-4ede-9d57-3d6472d25d5a
duration: 118
source-git-commit: 03b68057748892c757e0b5315d3a41d0a2e4fc79
workflow-type: tm+mt
source-wordcount: '534'
ht-degree: 0%

---

# Integrar o AEM Forms com o SendGrid

Bem-vindo a este guia técnico, onde exploraremos o processo de envio de emails com modelos dinâmicos do SendGrid do AEM Forms. Este guia tem como objetivo fornecer uma compreensão clara de como aproveitar modelos dinâmicos para personalizar seu conteúdo de email de maneira eficaz.

Os modelos dinâmicos permitem criar modelos de email que podem exibir conteúdo diferente para os recipients com base nos dados capturados no formulário adaptável. Ao utilizar variáveis de personalização, você pode fornecer experiências de email direcionadas e personalizadas que repercutem com seu público-alvo.

Além disso, analisaremos o uso do arquivo Swagger, que permite personalizar ainda mais seus emails, incluindo o nome e o endereço de email do cliente, bem como selecionando o modelo de email dinâmico apropriado.

Siga as instruções passo a passo deste documento para aproveitar o potencial dos modelos dinâmicos do SendGrid e do AEM Forms, além de elevar suas comunicações por email a novos níveis de engajamento e relevância. Vamos começar!

## Pré-requisitos

Antes de continuar com o envio de emails usando os modelos dinâmicos do SendGrid do AEM Forms, verifique se você atendeu aos seguintes pré-requisitos:

1. **Conta do SendGrid**: cadastre-se em uma conta do SendGrid em [https://sendgrid.com](https://sendgrid.com) para acessar os serviços de entrega de email. Você precisará das credenciais da conta para integrar o SendGrid ao AEM Forms.
1. **Familiaridade com a Criação de Fontes de Dados**: tenha conhecimento prático sobre como criar fontes de dados no AEM Forms. Se necessário, consulte a documentação sobre [criação de fontes de dados](https://experienceleague.adobe.com/docs/experience-manager-learn/forms/ic-web-channel-tutorial/parttwo.html) para obter instruções detalhadas.
1. **Familiaridade com o modelo de dados de formulário**: entenda o conceito do modelo de dados de formulário no AEM Forms. Se necessário, revise a documentação sobre [criação de modelos de dados de formulário](https://experienceleague.adobe.com/docs/experience-manager-65/forms/form-data-model/create-form-data-models.html) para garantir que você tenha a compreensão necessária.

Ao atender a esses pré-requisitos, você estará equipado com o conhecimento e os recursos essenciais para enviar emails com eficiência usando os modelos dinâmicos do SendGrid do AEM Forms.

## Ativos de amostra

Os ativos de amostra fornecidos com este artigo incluem:

* **[Arquivo Swagger](assets/SendGridWithDynamicTemplate.yaml)**: esse arquivo permite enviar emails usando um modelo de email dinâmico. Ele fornece as especificações e configurações necessárias para integração com o SendGrid e o AEM Forms, para uma entrega de email sem interrupções.

Sinta-se à vontade para utilizar o arquivo Swagger fornecido como referência ou ponto de partida para implementar a funcionalidade de email com modelos dinâmicos.

## Instruções de teste

Para testar a funcionalidade descrita neste guia, siga estas etapas:

1. Baixe o [arquivo swagger](assets/SendGridWithDynamicTemplate.yaml) fornecido na pasta de ativos.
2. Crie uma fonte de dados Restful usando o arquivo Swagger baixado e suas credenciais do SendGrid.
3. Crie um Modelo de dados de formulário com base na fonte de dados Restful.
4. Invoque a operação POST `mail/send` do modelo de dados de formulário de acordo com suas necessidades. Por exemplo, você pode acionar o email ao clicar no botão ou incluí-lo como parte do fluxo de trabalho do AEM Forms.

A amostra de carga para o serviço é a seguinte: Substitua os valores de espaço reservado pelos seus próprios dados:

```json
{
    "sendgridpayload": {
        "from": {
            "email": "gs@xyz.com"
        },
        "personalizations": [{
            "to": [{
                "email": "johndoe@xyz.com"
            }],
            "dynamic_template_data": {
                "customerName": "John Doe"
            }
        }],
        "template_id": "d-72aau292a3bd60b5300c"
    }
}
```

Verifique se `template_id` corresponde à ID do seu modelo de email dinâmico do SendGrid e se os endereços de email são válidos e verificados pelo SendGrid. Os valores na seção `personalizations` permitem personalizar o email usando os dados inseridos pelo usuário a partir do formulário adaptável.

Seguindo essas etapas e personalizando a carga útil fornecida, você pode testar efetivamente a integração dos modelos dinâmicos do SendGrid com o AEM Forms.
