---
title: query reg
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 0e6a0d7c-ed9b-4318-833d-33f265a81f39
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ea66a7d96435309a3b30b67f45bc68200f30dd2f
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722534"
---
# <a name="reg-query"></a>query reg



Restituisce un elenco di livello successivo di sottochiavi e le voci che si trovano in una sottochiave specificata nel Registro di sistema.



## <a name="syntax"></a>Sintassi

```
reg query <KeyName> [{/v <ValueName> | /ve}] [/s] [/se <Separator>] [/f <Data>] [{/k | /d}] [/c] [/e] [/t <Type>] [/z]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<Nome>|Specifica il percorso completo della sottochiave. Per specificare i computer remoti, includere il nome del computer ( \\ \\nel formato\) ComputerName come parte del *nome*della finestra di. \\ \\Se si omette nomecomputer \, l'operazione viene impostata sul computer locale per impostazione predefinita. Il *nome* chiave deve includere una chiave radice valida. Le chiavi principali valide per il computer locale sono: HKLM, HKCU, HKCR, HKU e HKCC. Se viene specificato un computer remoto, le chiavi principali valide sono: HKLM e HKU.|
|/v \<valorename>|Specifica il nome del valore del Registro di sistema che deve essere sottoposto a query. Se omesso, i nomi di tutti i valori per *KeyName* vengono restituiti. *ValueName* per questo parametro è facoltativo se il **/f** opzione viene inoltre utilizzata.|
|/ve|Esegue una query per i nomi di valore non vuoto.|
|/s|Specifica per eseguire query su tutte le sottochiavi e in modo ricorsivo i nomi di valore.|
|> \<separatore/se|Specifica il separatore singolo valore per la ricerca nel tipo di nome valore REG_MULTI_SZ. Se *separatore* non è specificato, **\0** viene utilizzato.|
|/f \<> dati|Specifica i dati o i criteri di ricerca. Se una stringa contiene spazi, utilizzare le virgolette doppie. Se non specificato, come criterio di ricerca viene usato un carattere jolly (**&#42;**).|
|/k|Specifica la ricerca nei nomi delle chiavi.|
|/d|Specifica la ricerca solo nei dati.|
|/C|Specifica che la query viene fatta distinzione tra maiuscole e minuscole. Per impostazione predefinita, le query non sono tra maiuscole e minuscole.|
|/e|Specifica che verranno restituite solo corrispondenze esatte. Per impostazione predefinita, vengono restituite tutte le corrispondenze.|
|/t \<tipo>|Specifica i tipi di registro di sistema per la ricerca. I tipi validi sono: REG_SZ, REG_MULTI_SZ, REG_EXPAND_SZ, REG_DWORD, REG_BINARY, REG_NONE. Se non specificato, vengono ricercati tutti i tipi.|
|/z|Specifica in modo da includere il valore numerico equivalente per il tipo del Registro di sistema nei risultati della ricerca.|
|/?|Visualizza la Guida per **reg query** al prompt dei comandi.|

## <a name="remarks-optional-section"></a>Sezione \<facoltativa osservazioni>

Nella tabella seguente sono elencati i valori restituiti per il **reg query** operazione.

|valore|Descrizione|
|-----|-----------|
|0|Operazione completata|
|1|Errore|

## <a name="examples"></a>Esempi

Per visualizzare il valore del valore del nome, versione nella chiave HKLM\Software\Microsoft\ResKit, digitare:
```
REG QUERY HKLM\Software\Microsoft\ResKit /v Version
```
Per visualizzare tutte le sottochiavi e valori nella chiave HKLM\Software\Microsoft\ResKit\Nt\Setup in un computer remoto denominato ABC, digitare:
```
REG QUERY \\ABC\HKLM\Software\Microsoft\ResKit\Nt\Setup /s
```
Per visualizzare tutte le sottochiavi e i valori del tipo REG_MULTI_SZ utilizzando **#** come separatore, digitare:
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

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)