---
title: reg delete
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: cee05071-1607-4ab1-b8ab-65caebeb85c3
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 726a3c700a9278dbc7abb1873aae7ea3c957bbb5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80836504"
---
# <a name="reg-delete"></a>reg delete



Elimina una sottochiave o le voci dal Registro di sistema.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
Reg delete <KeyName> [{/v ValueName | /ve | /va}] [/f]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<nome della >|Specifica il percorso completo della sottochiave o voce da eliminare. Per specificare un computer remoto, includere il nome del computer (nel formato \\\\nomecomputer\) come parte del *nome*della pagina. Se si omette \\\\nomecomputer \ l'operazione viene impostata sul computer locale per impostazione predefinita. Il *KeyName* deve includere una chiave radice valido. Le chiavi principali valide per il computer locale sono: HKLM, HKCU, HKCR, HKU e HKCC. Se viene specificato un computer remoto, le chiavi principali valide sono: HKLM e HKU.|
|/v \<valore >|Elimina una voce specifica della sottochiave. Se non viene specificato alcun valore, verranno eliminate tutti i movimenti e le sottochiavi della sottochiave.|
|/ve|Specifica che verranno eliminate solo le voci che non hanno alcun valore.|
|/va|Elimina tutte le voci nella sottochiave specificata. Sottochiave sotto la sottochiave specificata non vengono eliminati.|
|/f|Elimina la sottochiave del Registro di sistema esistente o una voce senza chiedere conferma.|
|/?|Visualizza la Guida per **reg delete** al prompt dei comandi.|

## <a name="remarks"></a>Note

Nella tabella seguente sono elencati i valori restituiti per il **reg delete** operazione.

|Valore|Descrizione|
|-----|-----------|
|0|Success|
|1|Operazione non riuscita|

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per eliminare la chiave del Registro di sistema Timeout e il relativo tutte le sottochiavi e valori, digitare:
```
REG DELETE HKLM\Software\MyCo\MyApp\Timeout
```
Per eliminare il valore del Registro di sistema MTU HKLM\Software\MiaSoc nel computer denominato ZODIAC, digitare:
```
REG DELETE \\ZODIAC\HKLM\Software\MyCo /v MTU
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)