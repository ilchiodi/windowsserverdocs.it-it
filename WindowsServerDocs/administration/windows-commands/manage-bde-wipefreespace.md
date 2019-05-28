---
title: manage-bde WipeFreeSpace
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b8d83a2a-c5c8-4019-9041-23d1d6abf282
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7cf99a9124f78189de223018608d9864e51d7897
ms.sourcegitcommit: 08eba714d3ceb5f2dfb5486d6b990da1aa4dcbdd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/14/2019
ms.locfileid: "65564685"
---
# <a name="manage-bde-wipefreespace"></a>manage-bde: WipeFreeSpace



Cancella lo spazio libero nel volume di rimozione di tutti i frammenti di dati presenti nello spazio. Questo comando in un volume che era stato crittografato mediante il metodo di crittografia "Solo spazio utilizzato" fornisce lo stesso livello di protezione come metodo di crittografia "Full Volume Encryption". Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
manage-bde -WipeFreeSpace|-w [<Drive>] [-Cancel] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<Drive>|Rappresenta una lettera di unità seguita da due punti, un percorso GUID volume o un volume montato.|
|-Annulla|Annulla la cancellazione di spazio che è in corso.|
|-computername|Specifica che verrà utilizzato Gestione bde.exe per modificare la protezione BitLocker su un computer diverso. È inoltre possibile utilizzare **- cn** come una versione abbreviata di questo comando.|
|\<Nome >|Rappresenta il nome del computer in cui si desidera modificare la protezione BitLocker. I valori accettati includono nome NetBIOS del computer e l'indirizzo IP del computer.|
|-? o /?|Consente di visualizzare breve guida al prompt dei comandi.|
|-help o -h|Visualizza la Guida completa al prompt dei comandi.|

## <a name="BKMK_Examples"></a>Esempi

Nell'esempio seguente viene illustrato l'utilizzo di **-w** cancellazione di comando per creare lo spazio disponibile sull'unità C.
```
manage-bde -w C:
```
Nell'esempio seguente viene illustrato l'utilizzo di **-w** con il **-Annulla** parametro per annullare la cancellazione di spazio libero nell'unità C.
```
manage-bde -w -Cancel C:
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
-   [Gestire-bde](manage-bde.md)