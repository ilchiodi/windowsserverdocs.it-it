---
title: Manage-bde WipeFreeSpace
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 35d5f5fe079485d35fa412502bec745a136fbce9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71373785"
---
# <a name="manage-bde-wipefreespace"></a>Manage-bde WipeFreeSpace



Cancella lo spazio libero nel volume di rimozione di tutti i frammenti di dati presenti nello spazio. L'esecuzione di questo comando su un volume crittografato con il metodo di crittografia "solo spazio utilizzato" fornisce lo stesso livello di protezione del metodo di crittografia "Full Volume Encryption". Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
manage-bde -WipeFreeSpace|-w [<Drive>] [-Cancel] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<Drive >|Rappresenta una lettera di unità seguita da due punti, un percorso GUID volume o un volume montato.|
|-Annulla|Annulla la cancellazione di spazio che è in corso.|
|-computername|Specifica che verrà utilizzato Gestione bde.exe per modificare la protezione BitLocker su un computer diverso. È inoltre possibile utilizzare **- cn** come una versione abbreviata di questo comando.|
|\<Nome >|Rappresenta il nome del computer in cui si desidera modificare la protezione BitLocker. I valori accettati includono nome NetBIOS del computer e l'indirizzo IP del computer.|
|-? o /?|Visualizza una breve guida al prompt dei comandi.|
|-Help o-h|Visualizza la Guida completa al prompt dei comandi.|

## <a name="BKMK_Examples"></a>Esempi

Nell'esempio seguente viene illustrato l'utilizzo di **-w** cancellazione di comando per creare lo spazio disponibile sull'unità C.
```
manage-bde -w C:
```
Nell'esempio seguente viene illustrato l'utilizzo del comando **-w** con il parametro **-Cancel** per annullare la cancellazione dello spazio disponibile nell'unità C.
```
manage-bde -w -Cancel C:
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
-   [Manage-bde](manage-bde.md)