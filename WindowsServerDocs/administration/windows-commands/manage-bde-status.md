---
title: stato gestione-BDE
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1444a360-fabf-4dd3-b67f-188e6ea3fa5b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d1bf42da356d8326f459066fc168bbd38b7765b0
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724083"
---
# <a name="manage-bde-status"></a>Manage-bde: stato



Fornisce le seguenti informazioni su tutte le unità del computer. Se sono protette con BitLocker:
-   Dimensione
-   Versione di BitLocker
-   Stato di conversione
-   Percentuale crittografata
-   Metodo di crittografia
-   Stato protezione
-   Stato di blocco
-   Campo di identificazione
-   Protezioni con chiave



## <a name="syntax"></a>Sintassi

```
manage-bde -status [<Drive>] [-protectionaserrorlevel] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<> unità|Rappresenta una lettera di unità seguita da due punti.|
|-protectionaserrorlevel|Fa sì che lo strumento da riga di comando Manage-bde inviare il codice restituito pari a 0 quando il volume è protetto e 1 quando il volume è protetto; più comunemente utilizzata per gli script batch per determinare se un'unità protetta da BitLocker. È inoltre possibile utilizzare **-p** come una versione abbreviata di questo comando.|
|-computername|Specifica che verrà utilizzato Gestione bde.exe per modificare la protezione BitLocker su un computer diverso. È inoltre possibile utilizzare **- cn** come una versione abbreviata di questo comando.|
|\<Name>|Rappresenta il nome del computer in cui si desidera modificare la protezione BitLocker. I valori accettati includono nome NetBIOS del computer e l'indirizzo IP del computer.|
|-? o /?|Visualizza una breve guida al prompt dei comandi.|
|-Help o-h|Visualizza la Guida completa al prompt dei comandi.|

## <a name="examples"></a>Esempi

Per illustrare l'uso del comando **-status** per visualizzare lo stato dell'unità C.
```
manage-bde –status C:
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
-   [Manage-bde](manage-bde.md)