---
title: Acionar o fluxo de trabalho de AEM no envio do formulário HTML5 - Colocando o caso de uso em funcionamento
description: Implante os ativos de amostra no sistema local
feature: Mobile Forms
doc-type: article
version: 6.4,6.5
topic: Development
role: Developer
level: Experienced
jira: kt-16133
exl-id: 79935ef0-bc73-4625-97dd-767d47a8b8bb
badgeVersions: label="AEM Forms 6.5" before-title="false"
duration: 90
source-git-commit: 9545fae5a5f5edd6f525729e648b2ca34ddbfd9f
workflow-type: tm+mt
source-wordcount: '457'
ht-degree: 0%

---

# Fazendo com que este caso de uso funcione em seu sistema

>[!NOTE]
>
>Para que os ativos de amostra funcionem no sistema, presume-se que você tenha acesso a um autor do AEM Forms e a uma instância de publicação do AEM Forms.

Para que esse caso de uso funcione no sistema local, siga estas etapas:

## Implante o seguinte na instância do autor do AEM Forms

* [Instalar o pacote MobileFormToWorkflow](assets/MobileFormToWorkflow.core-1.0.0-SNAPSHOT.jar)

* [Importe o perfil personalizado](assets/customprofile.zip) que mescla os dados do formulário HTML5 com o XDP e retorna um pdf interativo.

* [Implantar o pacote Desenvolvendo com Usuário de Serviço](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip?lang=en)
Adicione a seguinte entrada no serviço Mapeador de usuários do Apache Sling Service usando o configMgr

```
DevelopingWithServiceUser.core:getformsresourceresolver=fd-service
```

* Você pode armazenar os envios de formulários em uma pasta diferente especificando o nome da pasta na configuração de Credenciais do Servidor AEM usando o [configMgr](http://localhost:4502/system/console/configMg). Se você alterar a pasta, crie um inicializador na pasta para acionar o fluxo de trabalho **ReviewSubmittedPDF**

![config-author](assets/author-config.png)
* [Importe o xdp de exemplo e o pacote de fluxo de trabalho usando o gerenciador de pacotes](assets/xdp-form-and-workflow.zip).


## Implante os seguintes ativos na instância de publicação

* [Instalar o pacote MobileFormToWorkflow](assets/MobileFormToWorkflow.core-1.0.0-SNAPSHOT.jar)

* Especifique o nome de usuário/senha para a instância do autor e um **local existente em seu repositório AEM** para armazenar os dados enviados nas credenciais do Servidor AEM usando o [configMgr](http://localhost:4503/system/console/configMgr). Você pode deixar o URL do endpoint no servidor de fluxo de trabalho AEM como está. Esse é o endpoint que extrai e armazena os dados do envio no nó especificado.
  ![publish-config](assets/publish-config.png)

* [Implantar o pacote Desenvolvendo com Usuário de Serviço](https://experienceleague.adobe.com/docs/experience-manager-learn/assets/developingwithserviceuser.zip?lang=en)
* [Abra a configuração osgi](http://localhost:4503/system/console/configMgr).
* Procure por **Filtro referenciador Apache Sling**. Certifique-se de que a caixa de seleção Permitir vazio esteja marcada.
* [Importe o perfil personalizado](assets/customprofile.zip) que mescla os dados do formulário HTML5 com o XDP e retorna um pdf interativo.


## Testar a solução

* Faça logon na instância do autor
* [Edite as propriedades avançadas de w9.xdp](http://localhost:4502/libs/fd/fm/gui/content/forms/formmetadataeditor.html/content/dam/formsanddocuments/w9.xdp). Verifique se o url de envio e o perfil de renderização estão definidos corretamente, conforme mostrado abaixo.
  ![propriedades-avançadas-xdp](assets/mobile-form-properties.png)

* Publish, o w9.xdp
* Faça logon para publicar a instância
* [Visualizar o formulário w9](http://localhost:4503/content/dam/formsanddocuments/w9.xdp/jcr:content)
* Preencha vários campos e clique no botão na barra de ferramentas para baixar o PDF interativo.
* Preencha o PDF baixado usando o Acrobat e pressione o botão enviar.
* Você deve receber uma mensagem de sucesso
* Fazer logon na instância do autor do AEM como administrador
* [Verificar a Caixa de Entrada do AEM](http://localhost:4502/aem/inbox)
* Você deve ter um item de trabalho para revisar o PDF enviado

>[!NOTE]
>
>Em vez de enviar o PDF para o servlet em execução na instância de publicação, alguns clientes implantaram o servlet no container de servlet, como o Tomcat. Tudo depende da topologia com a qual o cliente está familiarizado.para o propósito deste tutorial, vamos usar o servlet implantado na instância de publicação para lidar com os envios de pdf.