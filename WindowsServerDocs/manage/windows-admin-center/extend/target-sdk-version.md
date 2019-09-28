---
title: Specificare come destinazione una versione diversa di Windows Admin Center SDK
description: Specificare come destinazione una versione diversa di Windows Admin Center SDK (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 0d3b7af5229f7b8487aa9f04eaf0d1756d8c02f4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356974"
---
# <a name="target-a-different-version-of-the-windows-admin-center-sdk"></a>Specificare come destinazione una versione diversa di Windows Admin Center SDK

>Si applica a: Windows Admin Center, Windows Admin Center Preview

L'aggiornamento dell'estensione con le modifiche apportate all'SDK e le modifiche apportate alla piattaforma è facile.  Si usano i [tag NPM](https://www.npmjs.com/package/@microsoft/windows-admin-center-sdk) per organizzare il rilascio di nuove funzionalità nelle versioni di SDK.

Sono disponibili tre versioni dell'SDK tra cui è possibile scegliere:

* ```latest```: questo pacchetto SDK è allineato alla versione GA corrente dell'interfaccia di amministrazione di Windows
* ```insider```: questo pacchetto SDK è allineato alla versione di anteprima corrente di Windows Admin Center (disponibile in Windows Server Insider Preview)
* ```next```: questo pacchetto SDK contiene le funzionalità più recenti

> [!NOTE]
> Scopri di più sulle diverse [versioni](https://aka.ms/WACDownloadPage) dell'interfaccia di amministrazione di Windows disponibili per il download.

## <a name="targeting-sdk-version-on-a-new-project"></a>Destinazione della versione SDK in un nuovo progetto

Quando si crea una nuova estensione, è possibile includere il parametro ```--version``` per fare riferimento a una versione diversa dell'SDK:

```
wac create --company "{!Company Name}" --tool "{!Tool Name}" --version {!version}
```

| Value | Spiegazione | Esempio |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | Nome della società (con spazi) | ```Contoso Inc``` |
| ```{!Tool Name}``` | Nome dello strumento (con spazi) | ```Manage Foo Works``` |
| ```{!version}``` | Versione SDK | ```latest``` |

Di seguito è riportato un esempio di creazione di una nuova estensione destinata a ```insider```:

```
wac create --company "Contoso Inc" --tool "Manage Foo Works" --version insider
```

## <a name="targeting-sdk-version-on-an-existing-project"></a>Destinazione della versione SDK in un progetto esistente

Per modificare un progetto esistente in modo che abbia come destinazione una versione diversa di SDK, modificare la riga seguente in ```package.json```:

```
"@microsoft/windows-admin-center-sdk": "latest",
```
In questo esempio, sostituire ```latest``` con la versione dell'SDK desiderata, ad esempio ```insider```:

```
"@microsoft/windows-admin-center-sdk": "insider",
```

Eseguire quindi ```npm install``` per aggiornare i riferimenti nell'intero progetto.