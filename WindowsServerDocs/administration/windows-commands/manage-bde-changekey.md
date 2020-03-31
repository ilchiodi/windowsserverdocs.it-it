---
title: Manage-bde ChangeKey
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 69463db9-7e03-47ff-b233-a95d5055725f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0fc273bc84e0bc25a7409941af6dca02b6042640
ms.sourcegitcommit: 479ad84a0d6c7c7b8308122b8bac8308cb36fe9b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/30/2020
ms.locfileid: "80391696"
---
# <a name="manage-bde-changekey"></a>Manage-bde: ChangeKey



Modifica la chiave di avvio per un'unità del sistema operativo. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
manage-bde -changekey [<Drive>] [<PathToExternalKeyDirectory>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Unità \<>|Rappresenta una lettera di unità seguita da due punti.|
|\<PathToExternalKeyDirectory >|Rappresenta il percorso della directory per salvare il file di chiave esterna di avvio che può essere utilizzato per sbloccare l'unità.|
|-computername|Specifica che verrà utilizzato Gestione bde.exe per modificare la protezione BitLocker su un computer diverso. È inoltre possibile utilizzare **- cn** come una versione abbreviata di questo comando.|
|Nome \<>|Rappresenta il nome del computer in cui si desidera modificare la protezione BitLocker. I valori accettati includono nome NetBIOS del computer e l'indirizzo IP del computer.|
|-? o /?|Visualizza una breve guida al prompt dei comandi.|
|-Help o-h|Visualizza la Guida completa al prompt dei comandi.|

## <a name="examples"></a><a name="BKMK_Examples"></a>Esempi

Nell'esempio seguente viene illustrato l'utilizzo di **- changekey** comando per creare una nuova chiave di avvio sull'unità E da utilizzare con la crittografia BitLocker nell'unità C.
```
manage-bde -changekey C: E:\
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
-   [Manage-bde](manage-bde.md)
