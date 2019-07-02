---
title: REG aggiungere
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d9ad143e-dc10-4e2e-a229-408393c40079
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d46fc2df23391a1dbb782014addc68d9522d603a
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66441910"
---
# <a name="reg-add"></a>REG aggiungere


Aggiunge una nuova sottochiave o voce nel Registro di sistema.

## <a name="syntax"></a>Sintassi

```
reg add <KeyName> [{/v ValueName | /ve}] [/t DataType] [/s Separator] [/d Data] [/f]
```
Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="parameters"></a>Parametri

|      Parametro      |                                                                                                                                                                                                                                                                   Descrizione                                                                                                                                                                                                                                                                   |
|---------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| \<KeyName<em>></em> | Specifica il percorso completo della sottochiave o voce da aggiungere. Per specificare un computer remoto, includere il nome del computer (nel formato \\ \\ \<nomecomputer >\) come parte del *KeyName*. L'omissione \\ \\ComputerName\ fa sì che l'operazione per impostazione predefinita nel computer locale. Il *KeyName* deve includere una chiave radice valido. Le chiavi principali valide per il computer locale sono: HKLM, HKCU, HKCR, HKU e HKCC. Se viene specificato un computer remoto, le chiavi principali valide sono: HKLM e HKU. Se il nome della chiave del Registro di sistema contiene uno spazio, racchiudere il nome della chiave tra virgolette. |
|   /v \<ValueName>   |                                                                                                                                                                                                                                Specifica il nome della voce del Registro di sistema da aggiungere nella sottochiave specificata.                                                                                                                                                                                                                                 |
|         /ve         |                                                                                                                                                                                                                                Specifica che la voce del Registro di sistema che viene aggiunto al Registro di sistema ha un valore null.                                                                                                                                                                                                                                |
|     /t \<tipo >      |                                                                                                                                          Specifica il tipo per la voce del Registro di sistema. *Tipo* deve essere uno dei seguenti:</br>REG_SZ</br>REG_MULTI_SZ</br>REG_DWORD_BIG_ENDIAN</br>REG_DWORD</br>REG_BINARY</br>REG_DWORD_LITTLE_ENDIAN</br>REG_LINK</br>REG_FULL_RESOURCE_DESCRIPTOR</br>REG_EXPAND_SZ                                                                                                                                          |
|   /s \<separator >   |                                                                                                                                                              Specifica il carattere da utilizzare per separare più istanze di dati quando viene specificato il tipo di dati REG_MULTI_SZ e deve essere elencato più di una voce. Se non specificato, il separatore predefinito è **\0**.                                                                                                                                                              |
|     /d \<Data>      |                                                                                                                                                                                                                                                 Specifica i dati per la nuova voce del Registro di sistema.                                                                                                                                                                                                                                                  |
|         /f          |                                                                                                                                                                                                                                           Aggiunge la voce del Registro di sistema senza chiedere conferma.                                                                                                                                                                                                                                           |
|         /?          |                                                                                                                                                                                                                                              Visualizza la Guida per **reg aggiungere** al prompt dei comandi.                                                                                                                                                                                                                                               |

## <a name="remarks"></a>Note

-   Impossibile aggiungere sottostrutture con questa operazione. Questa versione di **reg** chiedere conferma quando si aggiunge una sottochiave.
-   Nella tabella seguente sono elencati i valori restituiti per il **reg aggiungere** operazione.

| Value | Descrizione |
|-------|-------------|
|   0   |   Riuscito   |
|   1   |   Operazione non riuscita   |

-   Per il tipo di chiave REG_EXPAND_SZ, utilizzare il simbolo di accento circonflesso ( **^** ) con **%** "all'interno del parametro /d

## <a name="BKMK_examples"></a>Esempi

Per aggiungere la chiave HKLM\Software\MyCo sul computer remoto ABC, digitare:
```
REG ADD \\ABC\HKLM\Software\MyCo
```
Per aggiungere una voce del Registro di sistema a HKLM\Software\MiaSoc con un valore denominato **dati** di tipo REG_BINARY e i dati di **fe340ead**, tipo:
```
REG ADD HKLM\Software\MyCo /v Data /t REG_BINARY /d fe340ead
```
Per aggiungere una voce del Registro di sistema multivalore a HKLM\Software\MiaSoc con il nome di un valore di **MRU** di tipo REG_MULTI_SZ e i dati di **fax\0mail\0\0**, tipo:
```
REG ADD HKLM\Software\MyCo /v MRU /t REG_MULTI_SZ /d fax\0mail\0\0
```
Per aggiungere una voce del Registro di sistema espansa a HKLM\Software\MiaSoc con il nome di un valore di **percorso** di tipo REG_EXPAND_SZ e i dati di **% systemroot %** , tipo:
```
REG ADD HKLM\Software\MyCo /v Path /t REG_EXPAND_SZ /d ^%systemroot^%
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)