---
title: Manage-bde (pacchetto)
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c631ef10-2a2f-4541-8578-292f2d4e9e80
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5c13502145d80693a64b284bf480fabfd03af0db
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724159"
---
# <a name="manage-bde-keypackage"></a>Manage-bde: pacchetto di pacchetti



Genera un pacchetto di chiavi per un'unità. Il pacchetto della chiave è utilizzabile in combinazione con lo strumento di ripristino per ripristinare l'unità danneggiata.

## <a name="syntax"></a>Sintassi

```
manage-bde -KeyPackage [<Drive>] [-ID <KeyProtectoryID>] [-path <PathToExternalKeyDirectory>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<> unità|Rappresenta una lettera di unità seguita da due punti.|
|-ID|Creare un pacchetto di chiavi utilizzando la protezione con chiave con l'identificatore specificato dal valore ID.|
|-percorso|Posizione in cui salvare il pacchetto della chiave creato.|
|-computername|Specifica che verrà utilizzato Gestione bde.exe per modificare la protezione BitLocker su un computer diverso. È inoltre possibile utilizzare **- cn** come una versione abbreviata di questo comando.|
|\<Name>|Rappresenta il nome del computer in cui si desidera modificare la protezione BitLocker. I valori accettati includono nome NetBIOS del computer e l'indirizzo IP del computer.|
|-? o /?|Visualizza una breve guida al prompt dei comandi.|
|-Help o-h|Visualizza la Guida completa al prompt dei comandi.|

## <a name="examples"></a>Esempi

Per illustrare l'uso del comando **-Key Package** per creare un pacchetto di chiavi per l'unità C basata sulla protezione con chiave identificata dal GUID e per salvare il pacchetto di chiavi in F:\folder.
```
manage-bde -KeyPackage C: -id {84E151C1...7A62067A512} -path f:\Folder
```

> [!TIP]
> Utilizzare **gestire-bde – protezione – ottenere** con la lettera di unità che si desidera creare un pacchetto di chiavi per ottenere un elenco di GUID disponibili da utilizzare come valore dell'ID.

## <a name="additional-references"></a>Riferimenti aggiuntivi

-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
-   [Manage-bde](manage-bde.md)