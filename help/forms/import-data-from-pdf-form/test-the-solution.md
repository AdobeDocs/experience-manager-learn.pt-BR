---
title: Importar dados de um arquivo PDF para o Formulário adaptável
description: Tutorial para preencher um formulário adaptável importando um arquivo PDF
feature: Adaptive Forms
version: 6.5
topic: Development
role: Developer
level: Beginner
exl-id: f21753b2-f065-4556-add4-b1983fb57031
duration: 21
source-git-commit: f4c621f3a9caa8c2c64b8323312343fe421a5aee
workflow-type: tm+mt
source-wordcount: '117'
ht-degree: 0%

---

# Implantar os ativos de amostra

É possível implantar os ativos de amostra para que esta solução funcione em sua instância local do AEM Forms

* [Importe a biblioteca do cliente e o componente personalizado para fazer upload do formulário pdf pelo Gerenciador de pacotes](./assets/client-libs-custom-component.zip)
* Baixe e implante o pacote usando o console da Web OSGi[Pacote de serviços de documento personalizados](/help/forms/assets/common-osgi-bundles/AEMFormsDocumentServices.core-1.0-SNAPSHOT.jar)
* Baixe e implante o pacote usando o console da Web OSGi [Desenvolvimento com o Conjunto de Usuários de Serviço](/help/forms/assets/common-osgi-bundles/DevelopingWithServiceUser.jar)
* Baixe e implante o pacote usando o console OSGi da Web[importe dados do arquivo pdf](./assets/onlineToOffline.core-1.0.0-SNAPSHOT.jar)
* Adicione a entrada _**DevelopingWithServiceUser.core:getresourceresolver=data**_ no console de configuração OSGi do _**Serviço Mapeador do Usuário do Apache Sling Service**_
