---
title: Fare riferimento a una versione diversa di Windows Admin Center SDK
description: Fare riferimento a una versione diversa di Windows Admin Center SDK (Project Honolulu)
ms.technology: manage
ms.topic: article
author: nwashburn-ms
ms.author: niwashbu
ms.date: 09/18/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 47ae669e517f963762ee6267594e18f3413a72ff
ms.sourcegitcommit: be0144eb59daf3269bebea93cb1c467d67e2d2f1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/20/2018
ms.locfileid: "4081038"
---
# Fare riferimento a una versione diversa di Windows Admin Center SDK

>Si applica a: Windows Admin Center, Windows Admin Center Preview

Mantenere l'estensione aggiornato con le modifiche SDK e platform è facile.  Usiamo [i tag NPM](https://www.npmjs.com/package/@microsoft/windows-admin-center-sdk) per organizzare il rilascio di nuove funzionalità in versioni degli SDK.

Esistono tre versioni degli SDK che puoi scegliere tra:

* ```latest``` : questo pacchetto SDK allineato con la versione GA corrente di Windows Admin Center
* ```insider``` : questo pacchetto SDK allineato con la versione di anteprima corrente di Windows Admin Center (disponibile in Windows Server Insider Preview)
* ```next``` : questo pacchetto SDK contiene le funzionalità più recenti

> [!NOTE]
> Trovare altre informazioni sulle diverse [versioni](https://aka.ms/WACDownloadPage) di Windows Admin Center disponibili per il download.

## Versione del SDK in un nuovo progetto di destinazione

Quando si crea una nuova estensione, puoi includere il ```--version``` parametro per fare riferimento a una versione diversa del SDK:

```
wac create --company "{!Company Name}" --tool "{!Tool Name}" --version {!version}
```

| Valore | Spiegazione | Esempio |
| ----- | ----------- | ------- |
| ```{!Company Name}``` | Il nome della società (con spazi) | ```Contoso Inc``` |
| ```{!Tool Name}``` | Il nome dello strumento (con spazi) | ```Manage Foo Works``` |
| ```{!version}``` | Versione del SDK | ```latest``` |

Ecco un esempio di creazione di una nuova destinazione di estensione ```insider```:

```
wac create --company "Contoso Inc" --tool "Manage Foo Works" --version insider
```

## Versione del SDK in un progetto esistente di destinazione

Per modificare un progetto esistente per una versione diversa di SDK di destinazione, modificare la riga seguente in ```package.json```:

```
"@microsoft/windows-admin-center-sdk": "latest",
```
In questo esempio, Sostituisci ```latest``` con la versione del SDK desiderata, ad esempio ```insider```:

```
"@microsoft/windows-admin-center-sdk": "insider",
```

Quindi Esegui ```npm install``` aggiornare i riferimenti in tutto il progetto.