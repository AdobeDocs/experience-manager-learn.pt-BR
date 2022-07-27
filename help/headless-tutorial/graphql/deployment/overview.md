---
source-git-commit: 10032375a23ba99f674fb59a6f6e48bf93908421
workflow-type: tm+mt
source-wordcount: '174'
ht-degree: 0%

---



# AEM implantações headless

AEM as implantações de clientes headless assumem várias formas; SPA hospedados pela AEM, SPA externos ou site da Web, aplicativo móvel ou até mesmo processo servidor a servidor.

Dependendo do cliente e de como ele é implantado, AEM as implantações headless têm diferentes considerações.

## Arquitetura de serviço AEM

Antes de explorar as considerações sobre implantação, é imperativo entender AEM arquitetura lógica e a separação e as funções dos níveis de serviço AEM as a Cloud Service. AEM as a Cloud Service é composto por dois serviços lógicos:

+ __Autor do AEM__ é o serviço onde as equipes criam, colaboram e publicam Fragmentos de conteúdo (e outros ativos)
+ __Publicação do AEM__ é o serviço no qual os Fragmentos de conteúdo publicados (e outros ativos) são replicados para consumo geral

AEM clientes headless operando em uma capacidade de produção normalmente interagem com o AEM Publish, que contém o conteúdo aprovado e publicado. O cliente que interage com o autor do AEM precisa ter uma consideração especial, pois o autor do AEM é seguro por padrão, exigindo autorização para todas as solicitações, bem como pode conter conteúdo incompleto ou não aprovado.

## Implantações de clientes headless

+ [Aplicativo de página única](./spa.md)
+ [Componente da Web/JS](./web-component.md)
+ [Aplicativo móvel](./mobile.md)
+ [Servidor para servidor](./server-to-server.md)

