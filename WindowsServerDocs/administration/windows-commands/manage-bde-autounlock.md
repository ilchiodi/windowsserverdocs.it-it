---
title: sblocco automatico gestione-BDE
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 063528bf-d235-4b44-887a-52a7d983e01a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3786700a809a672c00ee77c444c133b04e71e863
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80840254"
---
# <a name="manage-bde-autounlock"></a>Manage-bde: sblocco automatico



Gestisce il sblocco automatico delle unità dati protette da BitLocker. Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
manage-bde -autounlock [{-enable|-disable|-clearallkeys}] <Drive> [-computername <Name>] [{-?|/?}] [{-help|-h}]

```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|-enable|Abilita lo sblocco automatico per un'unità di dati.|
|-disable|Disabilita lo sblocco automatico per un'unità di dati.|
|-clearallkeys|Rimuove tutte le stored chiavi esterne nelle unità del sistema operativo.|
|Unità \<>|Rappresenta una lettera di unità seguita da due punti.|
|-computername|Specifica che verrà utilizzato Gestione bde.exe per modificare la protezione BitLocker su un computer diverso. È inoltre possibile utilizzare **- cn** come una versione abbreviata di questo comando.|
|Nome \<>|Rappresenta il nome del computer in cui si desidera modificare la protezione BitLocker. I valori accettati includono nome NetBIOS del computer e l'indirizzo IP del computer.|
|-? o /?|Visualizza una breve guida al prompt dei comandi.|
|-Help o-h|Visualizza la Guida completa al prompt dei comandi.|

## <a name="examples"></a><a name=BKMK_Examples></a>Esempi

Nell'esempio seguente viene illustrato l'utilizzo di **lo sblocco automatico -** comando per abilitare lo sblocco automatico dell'unità di dati E.
```
manage-bde –autounlock -enable E:
```

## <a name="additional-references"></a>Altre informazioni di riferimento

-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
-   [Manage-bde](manage-bde.md)