---
title: reg delete
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cee05071-1607-4ab1-b8ab-65caebeb85c3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7156bf58b27da1602931f0dc1903de71d86764e7
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71384756"
---
# <a name="reg-delete"></a>reg delete



Elimina una sottochiave o le voci dal Registro di sistema.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
Reg delete <KeyName> [{/v ValueName | /ve | /va}] [/f]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<KeyName >|Specifica il percorso completo della sottochiave o voce da eliminare. Per specificare un computer remoto, includere il nome del computer (nel formato \\ @ no__t-1ComputerName @ no__t-2 come parte del *nome*della pagina. Se si omette \\ @ no__t-1ComputerName \, l'operazione viene impostata sul computer locale per impostazione predefinita. Il *KeyName* deve includere una chiave radice valido. Le chiavi radice valide per il computer locale sono: HKLM, HKCU, HKCR, HKU e HKCC. Se viene specificato un computer remoto, le chiavi radice valide sono: HKLM e HKU.|
|/v \<ValueName >|Elimina una voce specifica della sottochiave. Se non viene specificato alcun valore, verranno eliminate tutti i movimenti e le sottochiavi della sottochiave.|
|/ve|Specifica che verranno eliminate solo le voci che non hanno alcun valore.|
|/va|Elimina tutte le voci nella sottochiave specificata. Sottochiave sotto la sottochiave specificata non vengono eliminati.|
|/f|Elimina la sottochiave del Registro di sistema esistente o una voce senza chiedere conferma.|
|/?|Visualizza la Guida per **reg delete** al prompt dei comandi.|

## <a name="remarks"></a>Note

Nella tabella seguente sono elencati i valori restituiti per il **reg delete** operazione.

|Value|Descrizione|
|-----|-----------|
|0|Riuscito|
|1|Errore|

## <a name="BKMK_examples"></a>Esempi

Per eliminare la chiave del Registro di sistema Timeout e il relativo tutte le sottochiavi e valori, digitare:
```
REG DELETE HKLM\Software\MyCo\MyApp\Timeout
```
Per eliminare il valore del Registro di sistema MTU HKLM\Software\MiaSoc nel computer denominato ZODIAC, digitare:
```
REG DELETE \\ZODIAC\HKLM\Software\MyCo /v MTU
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)