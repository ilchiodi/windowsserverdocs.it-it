---
title: sblocco automatico gestione-BDE
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 063528bf-d235-4b44-887a-52a7d983e01a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1cc467c4afcfa2df344e9190a341a9aad086c1ea
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82724212"
---
# <a name="manage-bde-autounlock"></a>Manage-bde: sblocco automatico



Gestisce il sblocco automatico delle unità dati protette da BitLocker.

## <a name="syntax"></a>Sintassi

```
manage-bde -autounlock [{-enable|-disable|-clearallkeys}] <Drive> [-computername <Name>] [{-?|/?}] [{-help|-h}]

```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|-abilitare|Abilita lo sblocco automatico per un'unità di dati.|
|-Disabilita|Disabilita lo sblocco automatico per un'unità di dati.|
|-clearallkeys|Rimuove tutte le stored chiavi esterne nelle unità del sistema operativo.|
|\<> unità|Rappresenta una lettera di unità seguita da due punti.|
|-computername|Specifica che verrà utilizzato Gestione bde.exe per modificare la protezione BitLocker su un computer diverso. È inoltre possibile utilizzare **- cn** come una versione abbreviata di questo comando.|
|\<Name>|Rappresenta il nome del computer in cui si desidera modificare la protezione BitLocker. I valori accettati includono nome NetBIOS del computer e l'indirizzo IP del computer.|
|-? o /?|Visualizza una breve guida al prompt dei comandi.|
|-Help o-h|Visualizza la Guida completa al prompt dei comandi.|

## <a name="examples"></a>Esempi

Per illustrare l'uso del comando **-autounlock** per abilitare lo sblocco automatico dell'unità dati E.
```
manage-bde –autounlock -enable E:
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
-   [Manage-bde](manage-bde.md)