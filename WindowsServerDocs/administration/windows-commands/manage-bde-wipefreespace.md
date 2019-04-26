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
ms.openlocfilehash: 8750094e7357a3aefa307d24abd1470fbf8d2a71
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867172"
---
# <a name="manage-bde-wipefreespace"></a>manage-bde: WipeFreeSpace



Cancella lo spazio libero nel volume di rimozione di tutti i frammenti di dati presenti nello spazio. Esecuzione del comando in un volume che è stato crittografato utilizzando "solo lo spazio utilizzato? metodo di crittografia fornisce lo stesso livello di protezione di "Full Volume Encryption? metodo di crittografia. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
manage-bde –WipeFreeSpace|-w [<Drive>] [-Cancel] [-computername <Name>] [{-?|/?}] [{-help|-h}]
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
Nell'esempio seguente viene illustrato l'utilizzo di **-w** comando con il **– Annulla** parametro per annullare la cancellazione di spazio libero nell'unità C.
```
manage-bde -w -Cancel C:
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)
-   [Gestire-bde](manage-bde.md)