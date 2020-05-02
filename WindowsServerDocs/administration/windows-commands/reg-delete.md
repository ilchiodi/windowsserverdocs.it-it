---
title: reg delete
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cee05071-1607-4ab1-b8ab-65caebeb85c3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: a4ff643970bac021a6b7dcb731e64c412deb8df3
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722570"
---
# <a name="reg-delete"></a>reg delete



Elimina una sottochiave o le voci dal Registro di sistema.



## <a name="syntax"></a>Sintassi

```
Reg delete <KeyName> [{/v ValueName | /ve | /va}] [/f]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<Nome>|Specifica il percorso completo della sottochiave o voce da eliminare. Per specificare un computer remoto, includere il nome del computer (nel formato \\ \\ComputerName\) come parte del *nome*della pagina. \\ \\Se si omette nomecomputer \, l'operazione viene impostata sul computer locale per impostazione predefinita. Il *nome* chiave deve includere una chiave radice valida. Le chiavi principali valide per il computer locale sono: HKLM, HKCU, HKCR, HKU e HKCC. Se viene specificato un computer remoto, le chiavi principali valide sono: HKLM e HKU.|
|/v \<valorename>|Elimina una voce specifica della sottochiave. Se non viene specificato alcun valore, verranno eliminate tutti i movimenti e le sottochiavi della sottochiave.|
|/ve|Specifica che verranno eliminate solo le voci che non hanno alcun valore.|
|/va|Elimina tutte le voci nella sottochiave specificata. Sottochiave sotto la sottochiave specificata non vengono eliminati.|
|/f|Elimina la sottochiave del Registro di sistema esistente o una voce senza chiedere conferma.|
|/?|Visualizza la Guida per **reg delete** al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni

Nella tabella seguente sono elencati i valori restituiti per il **reg delete** operazione.

|valore|Descrizione|
|-----|-----------|
|0|Operazione completata|
|1|Errore|

## <a name="examples"></a>Esempi

Per eliminare la chiave del Registro di sistema Timeout e il relativo tutte le sottochiavi e valori, digitare:
```
REG DELETE HKLM\Software\MyCo\MyApp\Timeout
```
Per eliminare il valore del Registro di sistema MTU HKLM\Software\MiaSoc nel computer denominato ZODIAC, digitare:
```
REG DELETE \\ZODIAC\HKLM\Software\MyCo /v MTU
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)