---
title: Exibir mensagem de agradecimento no envio do formulário
description: Use o manipulador onSubmitSuccess para exibir a mensagem de agradecimento configurada no aplicativo react
feature: Adaptive Forms
version: 6.5
jira: KT-13490
topic: Development
role: User
level: Intermediate
exl-id: 489970a6-1b05-4616-84e8-52b8c87edcda
duration: 60
source-git-commit: f23c2ab86d42531113690df2e342c65060b5c7cd
workflow-type: tm+mt
source-wordcount: '171'
ht-degree: 0%

---

# Exibir a mensagem de agradecimento configurada

Uma mensagem de agradecimento no envio do formulário é uma maneira bem cuidada de reconhecer e expressar gratidão ao usuário por preencher e enviar um formulário. Serve como confirmação de que o seu envio foi recebido e apreciado. A mensagem de agradecimento é configurada usando a guia envio do container do guia do formulário adaptável

![mensagem de agradecimento](assets/thank-you-message.png)

A mensagem de agradecimento configurada pode ser acessada no manipulador de eventos onSuccess do supercomponente AdaptiveForm.
O código para associar o evento onSuccess e o código para o manipulador de eventos onSuccess estão listados abaixo

```javascript
<AdaptiveForm mappings={extendMappings} onSubmitSuccess={onSuccess} formJson={selectedForm}/>
```

```javascript
const onSuccess=(action) =>{
        let body = action.payload?.body;
        debugger;
        setThankYouMessage(body.thankYouMessage.replace(/<(.|\n)*?>/g, ''));
        console.log("Thank you message "+body.thankYouMessage.replace(/<(.|\n)*?>/g, ''));

      }
```

O código completo do componente da função Contato é fornecido abaixo

```javascript
import Form from './components/Form';
import PlainText from './components/plainText';
import TextField from './components/TextField';
import Button from './components/Button';
import { useState,useEffect } from "react";
import { AdaptiveForm } from "@aemforms/af-react-renderer";
export default function Contact(){
  
    const [selectedForm, setForm] = useState("");
    const [thankYouMessage, setThankYouMessage] = useState("");
    const [formSubmitted, setFormSubmitted] = useState(false);
  
    const extendMappings = {
        'plain-text' : PlainText,
        'text-input' : TextField,
        'button' : Button,
        'form': Form
      };
     const onSuccess=(action) =>{
        let body = action.payload?.body;
        debugger;
        setFormSubmitted(true);
        setThankYouMessage(body.thankYouMessage.replace(/<(.|\n)*?>/g, ''));
        // Remove any html tags in the thank you message
        console.log("Thank you message "+body.thankYouMessage.replace(/<(.|\n)*?>/g, ''));

      }
      
      const getForm = async () => {
        
        const resp = await fetch('/adobe/forms/af/L2NvbnRlbnQvZm9ybXMvYWYvY29udGFjdHVz');
        // Get the form id manually using the listform api
        let formJSON = await resp.json();
        setForm(formJSON.afModelDefinition)
      }
      useEffect( ()=>{
        getForm()
        

    },[]);
    
    return(
        
        <div>
           {!formSubmitted ?
            (
                <div>
                    <h1>Get in touch with us!!!!</h1>
                    <AdaptiveForm mappings={extendMappings} onSubmitSuccess={onSuccess} formJson={selectedForm}/>
                </div>
            ) :
            (
                <div>
                    <div>{thankYouMessage}</div>
                </div>
            )}
        </div>
      
          
        
    )
}
```

O código acima usa componentes html nativos que são mapeados para os componentes usados no formulário adaptável. Por exemplo, estamos mapeando o componente de formulário adaptável de entrada de texto para o componente TextField. Os componentes nativos usados no artigo [pode ser baixado aqui](./assets/native-components.zip)
