---
title: Manage-bde ChangeKey
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 69463db9-7e03-47ff-b233-a95d5055725f
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6d50abbe40232987045713c5eadf43607aeb6a81
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820711"
---
# <a name="manage-bde-changekey"></a>Manage-bde: ChangeKey



Modifica la chiave di avvio per un'unità del sistema operativo.

## <a name="syntax"></a>Sintassi

```
manage-bde -changekey [<Drive>] [<PathToExternalKeyDirectory>] [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<> unità|Rappresenta una lettera di unità seguita da due punti.|
|\<> PathToExternalKeyDirectory|Rappresenta il percorso della directory per salvare il file di chiave esterna di avvio che può essere utilizzato per sbloccare l'unità.|
|-computername|Specifica che verrà utilizzato Gestione bde.exe per modificare la protezione BitLocker su un computer diverso. È inoltre possibile utilizzare **- cn** come una versione abbreviata di questo comando.|
|\<Name>|Rappresenta il nome del computer in cui si desidera modificare la protezione BitLocker. I valori accettati includono nome NetBIOS del computer e l'indirizzo IP del computer.|
|-? o /?|Visualizza una breve guida al prompt dei comandi.|
|-Help o-h|Visualizza la Guida completa al prompt dei comandi.|

## <a name="examples"></a>Esempi

Per illustrare l'uso del comando **-ChangeKey** per creare una nuova chiave di avvio sull'unità E da usare con la crittografia BitLocker nell'unità C.
```
manage-bde -changekey C: E:\
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
-   [Manage-bde](manage-bde.md)
