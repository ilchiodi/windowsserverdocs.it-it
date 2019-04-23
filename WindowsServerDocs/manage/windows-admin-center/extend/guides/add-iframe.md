---
title: Aggiungere un iFrame a un'estensione dello strumento
description: Sviluppare un'estensione per strumento Windows Admin Center SDK (progetto Honolulu) - aggiungere un elemento iFrame per un'estensione degli strumenti
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 7cf1dcec1bc8e187b6db789c5402ca8119ca8b6c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850762"
---
# <a name="add-an-iframe-to-a-tool-extension"></a>Aggiungere un iFrame a un'estensione dello strumento

>Si applica a: Windows Admin Center, Windows Admin Center anteprima

In questo articolo, si aggiungerà un iFrame per un'estensione per strumento nuovo e vuoto che sono stati creati con la CLI di Windows Admin Center.

## <a name="prepare-your-environment"></a>Preparazione dell'ambiente ##

Se hai già fatto, seguire le istruzioni disponibili nel [sviluppare un'estensione per strumento](..\develop-tool.md) per preparare l'ambiente e creare un nuovo, vuoto estensione degli strumenti.

## <a name="add-a-module-to-your-project"></a>Aggiungere un modulo al progetto ##

Aggiungere un nuovo [modulo vuoto](add-module.md) al progetto, a cui si aggiungerà un iFrame nel passaggio successivo.  

## <a name="add-an-iframe-to-your-module"></a>Aggiungere un elemento iFrame al modulo ##

Ora si aggiungerà un iFrame per il modulo nuovo e vuoto appena creato.

In \src\app\, passare alla cartella del modulo, quindi aprire il file ```{!module-name}.component.html```, disponibile con la convenzione di denominazione seguente:

| Value | Spiegazione | Nome del file di esempio |
| ----- | ----------- | ------- |
| ```{!module-name}``` | Nome del modulo (lettere minuscole, spazi sostituiti da trattini) | ```manage-foo-works-portal.component.html``` |
    
Il file html, aggiungere il contenuto seguente:

``` html
<div>
  <iframe  style="height: 850px;" src="https://www.bing.com"></iframe>
</div>
```

A questo punto, aver aggiunto un iFrame all'estensione.  Successivamente, è possibile [compilazione e sul lato carico](..\develop-tool.md#build-and-side-load-your-extension) dell'estensione in Windows Admin Center per visualizzare i risultati.

> [!Note]
> Le impostazioni di Content Security Policy (CSP) potrebbe impedire il rendering in un iFrame all'interno di Windows Admin Center di alcuni siti. Altre informazioni su questo [qui](https://content-security-policy.com/). 
