---
title: Destinazione una versione diversa di Windows Admin Center SDK
description: Destinazione una versione diversa di Windows Admin Center SDK (progetto Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 47ae669e517f963762ee6267594e18f3413a72ff
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833622"
---
# <a name="target-a-different-version-of-the-windows-admin-center-sdk"></a>Destinazione una versione diversa di Windows Admin Center SDK

>Si applica a: Windows Admin Center, Windows Admin Center anteprima

L'estensione di stare al passo con le modifiche a SDK e piattaforma è facile.  Usiamo [NPM tag](https://www.npmjs.com/package/@microsoft/windows-admin-center-sdk) per organizzare il rilascio delle nuove funzionalità in versioni precedenti del SDK.

Esistono tre versioni del SDK che è possibile scegliere tra:

* ```latest``` : questo pacchetto SDK in linea con la versione di disponibilità generale corrente di Windows Admin Center
* ```insider``` : questo pacchetto SDK in linea con la versione di anteprima corrente di Windows Admin Center (disponibile in Windows Server Insider Preview)
* ```next``` : questo pacchetto SDK contiene le funzionalità più recenti

> [!NOTE]
> Altre informazioni sulle diverse [versioni](https://aka.ms/WACDownloadPage) di Windows Admin Center che è possibile scaricare.

## <a name="targeting-sdk-version-on-a-new-project"></a>Per la versione SDK a un nuovo progetto

Quando si crea una nuova estensione, è possibile includere il ```--version``` parametro come destinazione una versione diversa del SDK:

```
wac create --company "{!Company Name}" --tool "{!Tool Name}" --version {!version}
```

| Value | Spiegazione | Esempio |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | Il nome della società (spazi inclusi) | ```Contoso Inc``` |
| ```{!Tool Name}``` | Il nome dello strumento (spazi inclusi) | ```Manage Foo Works``` |
| ```{!version}``` | Versione del SDK | ```latest``` |

Di seguito è riportato un esempio di creazione di una nuova estensione targeting ```insider```:

```
wac create --company "Contoso Inc" --tool "Manage Foo Works" --version insider
```

## <a name="targeting-sdk-version-on-an-existing-project"></a>Per la versione SDK su un progetto esistente

Per modificare un progetto esistente come destinazione una versione SDK diversa, modificare la riga seguente nel ```package.json```:

```
"@microsoft/windows-admin-center-sdk": "latest",
```
In questo esempio, sostituire ```latest``` con la versione SDK desiderata, ad esempio ```insider```:

```
"@microsoft/windows-admin-center-sdk": "insider",
```

Eseguire quindi ```npm install``` per aggiornare i riferimenti in tutto il progetto.