---
title: Reg query
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0e6a0d7c-ed9b-4318-833d-33f265a81f39
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1e239184cc5d118a858d012528fd8135f0b834e5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59827752"
---
# <a name="reg-query"></a>Reg query



Restituisce un elenco di livello successivo di sottochiavi e le voci che si trovano in una sottochiave specificata nel Registro di sistema.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
reg query <KeyName> [{/v <ValueName> | /ve}] [/s] [/se <Separator>] [/f <Data>] [{/k | /d}] [/c] [/e] [/t <Type>] [/z]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<Nome chiave >|Specifica il percorso completo della sottochiave. Per specificare un computer remoto, includere il nome del computer (nel formato \\ \\nomecomputer\) come parte del *KeyName*. L'omissione \\ \\ComputerName\ fa sì che l'operazione per impostazione predefinita nel computer locale. Il *KeyName* deve includere una chiave radice valido. Le chiavi principali valide per il computer locale sono: HKLM, HKCU, HKCR, HKU e HKCC. Se viene specificato un computer remoto, le chiavi principali valide sono: HKLM e HKU.|
|/v \<ValueName>|Specifica il nome del valore del Registro di sistema che deve essere sottoposto a query. Se omesso, i nomi di tutti i valori per *KeyName* vengono restituiti. *ValueName* per questo parametro è facoltativo se il **/f** opzione viene inoltre utilizzata.|
|/ve|Esegue una query per i nomi di valore non vuoto.|
|/s|Specifica per eseguire query su tutte le sottochiavi e in modo ricorsivo i nomi di valore.|
|/Se \<separator >|Specifica il separatore singolo valore per la ricerca nel tipo di nome valore REG_MULTI_SZ. Se *separatore* non è specificato, **\0** viene utilizzato.|
|/f \<Data>|Specifica i dati o i criteri di ricerca. Se una stringa contiene spazi, utilizzare le virgolette doppie. Se non specificato, un carattere jolly (**&#42;**) viene usato come criterio di ricerca.|
|/k|Specifica la ricerca nei nomi delle chiavi.|
|/d|Specifica la ricerca solo nei dati.|
|/c|Specifica che la query viene fatta distinzione tra maiuscole e minuscole. Per impostazione predefinita, le query non sono tra maiuscole e minuscole.|
|/e|Specifica che verranno restituite solo corrispondenze esatte. Per impostazione predefinita, vengono restituite tutte le corrispondenze.|
|/t \<tipo >|Specifica i tipi di registro di sistema per la ricerca. I tipi validi sono: REG_SZ, REG_MULTI_SZ, REG_EXPAND_SZ, REG_DWORD, REG_BINARY, REG_NONE. Se non specificato, vengono ricercati tutti i tipi.|
|/z|Specifica in modo da includere il valore numerico equivalente per il tipo del Registro di sistema nei risultati della ricerca.|
|/?|Visualizza la Guida per **reg query** al prompt dei comandi.|

## <a name="remarks-optional-section"></a>La sezione Osservazioni \<sezione facoltativa >

Nella tabella seguente sono elencati i valori restituiti per il **reg query** operazione.

|Value|Descrizione|
|-----|-----------|
|0|Riuscito|
|1|Operazione non riuscita|

## <a name="BKMK_examples"></a>Esempi

Per visualizzare il valore del valore del nome, versione nella chiave HKLM\Software\Microsoft\ResKit, digitare:
```
REG QUERY HKLM\Software\Microsoft\ResKit /v Version
```
Per visualizzare tutte le sottochiavi e valori nella chiave HKLM\Software\Microsoft\ResKit\Nt\Setup in un computer remoto denominato ABC, digitare:
```
REG QUERY \\ABC\HKLM\Software\Microsoft\ResKit\Nt\Setup /s
```
Per visualizzare tutte le sottochiavi e valori del tipo REG_MULTI_SZ usando **#** come separatore, digitare:
```
REG QUERY HKLM\Software\Microsoft\ResKit\Nt\Setup /se #
```
Per visualizzare la chiave, valore e i dati per l'esatta e tra maiuscole e minuscole corrispondente del SISTEMA sotto la radice HKLM del tipo di dati REG_SZ, digitare:
```
REG QUERY HKLM /f SYSTEM /t REG_SZ /c /e
```
Per visualizzare la chiave, valore e corrisponde a quella **0F** nei dati sotto la chiave radice HKCU dei dati di tipo REG_BINARY.
```
REG QUERY HKCU /f 0F /d /t REG_BINARY
```
Per visualizzare il valore e i dati per i nomi dei valori null (predefinito) in HKLM\SOFTWARE, digitare:
```
REG QUERY HKLM\SOFTWARE /ve
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)