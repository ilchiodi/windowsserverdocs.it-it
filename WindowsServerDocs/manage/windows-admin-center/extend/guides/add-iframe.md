---
title: Aggiungere un'estensione dello strumento iFrame
description: "Sviluppare un'estensione dello strumento Windows Admin Center SDK (Project Honolulu): aggiungere un iFrame a un'estensione dello strumento"
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 7b4a7b688e4b2d9f52e44395c19211b91b964578
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/20/2018
ms.locfileid: "4080928"
---
# Aggiungere un'estensione dello strumento iFrame

>Si applica a: Windows Admin Center, Windows Admin Center Preview

In questo articolo, aggiungeremo un iFrame a un'estensione dello strumento di nuovo e vuoto che abbiamo creato con l'interfaccia CLI di Windows Admin Center.

## Preparazione dell'ambiente ##

Se hai già fatto, segui le istruzioni nel [sviluppare un'estensione dello strumento](..\develop-tool.md) per preparare l'ambiente e creare un'estensione dello strumento di nuovo e vuoto.

## Aggiungere un modulo al progetto ##

Aggiungi un nuovo [modulo vuota](add-module.md) al tuo progetto, a cui aggiungeremo iFrame nel passaggio successivo.  

## Aggiungere un iFrame al modulo ##

Ora aggiungeremo iFrame a tale modulo di nuovo e vuoto che abbiamo appena creato.

In \src\app\, Sfoglia nella cartella del modulo, quindi aprire file ```{!module-name}.component.html```trovato in con la convenzione di denominazione:

| Valore | Spiegazione | Nome del file di esempio |
| ----- | ----------- | ------- |
| ```{!module-name}``` | Nome del modulo (lettere minuscole, spazi sostituiti da trattini) | ```manage-foo-works-portal.component.html``` |
    
Aggiungi il contenuto seguente al file html:

``` html
<div>
  <iframe  style="height: 850px;" src="https://www.bing.com"></iframe>
</div>
```

Ecco fatto, hai aggiunto un iFrame alla tua estensione.  Successivamente, è possibile [creare e trasferire localmente carico](..\develop-tool.md#build-and-side-load-your-extension) l'estensione in Windows Admin Center per visualizzare i risultati.
