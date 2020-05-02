---
title: Copia reg
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3fe74213-39ec-4b2d-ba3d-086243eac997
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 91090faffbb925754a0d4ed610b37464872242db
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722577"
---
# <a name="reg-copy"></a>Copia reg



Copia una voce del Registro di sistema in una posizione specificata nel computer locale o remoto.



## <a name="syntax"></a>Sintassi

```
reg copy <KeyName1> <KeyName2> [/s] [/f]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<> Nomechiave1|Specifica il percorso completo della sottochiave da copiare. Per specificare un computer remoto, includere il nome del computer (nel formato \\ \\ComputerName\) come parte del *nome*della pagina. \\ \\Se si omette nomecomputer \, l'operazione viene impostata sul computer locale per impostazione predefinita. Il *nome* chiave deve includere una chiave radice valida. Le chiavi principali valide per il computer locale sono: HKLM, HKCU, HKCR, HKU e HKCC. Se viene specificato un computer remoto, le chiavi principali valide sono: HKLM e HKU.|
|\<> Nomechiave2|Specifica il percorso completo della destinazione della sottochiave. Per specificare un computer remoto, includere il nome del computer (nel formato \\ \\ComputerName\) come parte del *nome*della pagina. \\ \\Se si omette nomecomputer \, l'operazione viene impostata sul computer locale per impostazione predefinita. Il *nome* chiave deve includere una chiave radice valida. Le chiavi principali valide per il computer locale sono: HKLM, HKCU, HKCR, HKU e HKCC. Se viene specificato un computer remoto, le chiavi principali valide sono: HKLM e HKU.|
|/s|Copia tutte le sottochiavi e le voci nella sottochiave specificata.|
|/f|Copia la sottochiave senza chiedere conferma.|
|/?|Visualizza la Guida per **reg** Copia al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni

-   Reg non chiede conferma per la copia di una sottochiave.
-   Nella tabella seguente sono elencati i valori restituiti per il **Copia reg** operazione.

|valore|Descrizione|
|-----|-----------|
|0|Operazione completata|
|1|Errore|

## <a name="examples"></a>Esempi

Per copiare tutte le sottochiavi e valori della chiave MyApp chiave SalvaMiaApp, digitare:
```
REG COPY HKLM\Software\MyCo\MyApp HKLM\Software\MyCo\SaveMyApp /s
```
Per copiare tutti i valori nella chiave MyCo nel computer denominato ZODIAC nella chiave MyCo1 del computer corrente, digitare:
```
REG COPY \\ZODIAC\HKLM\Software\MyCo HKLM\Software\MyCo1
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)