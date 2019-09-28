---
title: Aggiungere un iFrame a un'estensione dello strumento
description: Sviluppare un'estensione dello strumento Windows Admin Center SDK (Project Honolulu)-aggiungere un iFrame a un'estensione dello strumento
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 0833b2fd92f2bf4b512120783bb71295a3112745
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406896"
---
# <a name="add-an-iframe-to-a-tool-extension"></a>Aggiungere un iFrame a un'estensione dello strumento

>Si applica a: Windows Admin Center, Windows Admin Center Preview

In questo articolo verrà aggiunto un iFrame a una nuova estensione dello strumento vuota creata con l'interfaccia della riga di comando di Windows Admin Center.

## <a name="prepare-your-environment"></a>Preparazione dell'ambiente ##

Se non è già stato fatto, seguire le istruzioni riportate in [sviluppare un'estensione dello strumento](../develop-tool.md) per preparare l'ambiente e creare una nuova estensione dello strumento vuota.

## <a name="add-a-module-to-your-project"></a>Aggiungere un modulo al progetto ##

Aggiungere al progetto un nuovo [modulo vuoto](add-module.md) , a cui verrà aggiunto un iframe nel passaggio successivo.  

## <a name="add-an-iframe-to-your-module"></a>Aggiungere un iFrame al modulo ##

A questo punto si aggiungerà un iFrame al nuovo modulo vuoto appena creato.

In \src\app @ no__t-0 passare alla cartella del modulo, quindi aprire il file ```{!module-name}.component.html```, disponibile con la convenzione di denominazione seguente:

| Value | Spiegazione | Nome del file di esempio |
| ----- | ----------- | ------- |
| ```{!module-name}``` | Nome del modulo (lettere minuscole, spazi sostituiti da trattini) | ```manage-foo-works-portal.component.html``` |
    
Aggiungere il contenuto seguente al file HTML:

``` html
<div>
  <iframe  style="height: 850px;" src="https://www.bing.com"></iframe>
</div>
```

A questo proposito, è stato aggiunto un iFrame all'estensione.  Successivamente, è possibile [compilare e caricare](../develop-tool.md#build-and-side-load-your-extension) il proprio interno nell'interfaccia di amministrazione di Windows per visualizzare i risultati.

> [!Note]
> Le impostazioni dei criteri di sicurezza del contenuto (CSP) possono impedire il rendering di alcuni siti in un iFrame all'interno dell'interfaccia di amministrazione di Windows. Per altre informazioni, vedere [qui](https://content-security-policy.com/). 
