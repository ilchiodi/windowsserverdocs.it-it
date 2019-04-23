---
title: gestire-bde KeyPackage
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c631ef10-2a2f-4541-8578-292f2d4e9e80
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8fb49ce8fbe4be076151b203560e62f44a78c9d4
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886902"
---
# <a name="manage-bde-keypackage"></a>manage-bde: KeyPackage



Genera un pacchetto di chiavi per un'unità. Il pacchetto della chiave è utilizzabile in combinazione con lo strumento di ripristino per ripristinare l'unità danneggiata. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
manage-bde -KeyPackage [<Drive>] [-ID <KeyProtectoryID>] [-path <PathToExternalKeyDirectory>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<Drive>|Rappresenta una lettera di unità seguita da due punti.|
|-ID|Creare un pacchetto di chiavi utilizzando la protezione con chiave con l'identificatore specificato dal valore ID.|
|-percorso|Posizione in cui salvare il pacchetto della chiave creato.|
|-computername|Specifica che verrà utilizzato Gestione bde.exe per modificare la protezione BitLocker su un computer diverso. È inoltre possibile utilizzare **- cn** come una versione abbreviata di questo comando.|
|\<Nome >|Rappresenta il nome del computer in cui si desidera modificare la protezione BitLocker. I valori accettati includono nome NetBIOS del computer e l'indirizzo IP del computer.|
|-? o /?|Consente di visualizzare breve guida al prompt dei comandi.|
|-help o -h|Visualizza la Guida completa al prompt dei comandi.|

## <a name="BKMK_Examples"></a>Esempi

Nell'esempio seguente viene illustrato l'utilizzo di **- KeyPackage** comando per creare un pacchetto di chiavi per l'unità C, disattivare la protezione con chiave identificata dal GUID basato e per salvare il pacchetto della chiave F:\Folder.
```
manage-bde -KeyPackage C: -id {84E151C1...7A62067A512} -path "f:\Folder"
```

> [!TIP]
> Utilizzare **gestire-bde – protezione – ottenere** con la lettera di unità che si desidera creare un pacchetto di chiavi per ottenere un elenco di GUID disponibili da utilizzare come valore dell'ID.

#### <a name="additional-references"></a>Altri riferimenti

-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)
-   [Gestire-bde](manage-bde.md)