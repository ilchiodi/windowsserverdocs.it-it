---
title: gestire-bde off
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0a27c119-d385-45e5-89fe-e311d4429876
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d8b2fb5abb739c2905c29336d543888d4d0d50ef
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837762"
---
# <a name="manage-bde-off"></a>manage-bde: off



Consente di decrittografare l'unità e consente di disattivare BitLocker. Una volta completata la decrittografia, vengono rimosse tutte le protezioni con chiave. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
manage-bde -off [<Volume>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<Volume >|Una lettera di unità seguita da due punti, un percorso GUID volume o un volume montato.|
|-computername|Specifica che verrà utilizzato Gestione bde.exe per modificare la protezione BitLocker su un computer diverso. È inoltre possibile utilizzare **- cn** come una versione abbreviata di questo comando.|
|\<Nome >|Rappresenta il nome del computer in cui si desidera modificare la protezione BitLocker. I valori accettati includono nome NetBIOS del computer e l'indirizzo IP del computer.|
|-? o /?|Consente di visualizzare breve guida al prompt dei comandi.|
|-help o -h|Visualizza la Guida completa al prompt dei comandi.|

## <a name="BKMK_Examples"></a>Esempi

Nell'esempio seguente viene illustrato l'utilizzo di **-off** comando per disattivare BitLocker nell'unità C.
```
manage-bde –off C:
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)
-   [Gestire-bde](manage-bde.md)