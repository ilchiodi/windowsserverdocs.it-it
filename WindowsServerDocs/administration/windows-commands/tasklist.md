---
title: tasklist
description: Informazioni su come visualizzare un elenco di processi in esecuzione nel computer locale o remoto.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 8dbe30ee-1484-46be-917b-5ca3ff4fdc9c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: abb36c8c794836387af5547f3706e910dc06fa42
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59843002"
---
# <a name="tasklist"></a>tasklist

Visualizza un elenco dei processi attualmente in esecuzione nel computer locale o in un computer remoto. **TaskList** sostituisce il **tlist** dello strumento.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
tasklist [/s <Computer> [/u [<Domain>\]<UserName> [/p <Password>]]] [{/m <Module> | /svc | /v}] [/fo {table | list | csv}] [/nh] [/fi <Filter> [/fi <Filter> [ ... ]]]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/s \<computer >|Specifica il nome o indirizzo IP di un computer remoto (non utilizzare le barre rovesciate). Il valore predefinito è il computer locale.|
|/u [\<Domain>\\\]\<UserName>|Esegue il comando con le autorizzazioni dell'utente che ha specificato dall'account *nomeutente* oppure *Domain*\*UserName *. **/u** può essere specificato solo se **/s** è specificato. Il valore predefinito è le autorizzazioni dell'utente attualmente connesso al computer in cui viene eseguito il comando.|
|/p \<Password>|Specifica la password dell'account utente specificato nella **/u** parametro.|
|/m \<Module>|Elenca tutte le attività con DLL moduli caricati che corrispondono al nome di modello specificato. Se non viene specificato il nome del modulo, questa opzione consente di visualizzare tutti i moduli caricati da ciascuna attività.|
|/svc|Elenca tutte le informazioni del servizio per ogni processo senza troncamenti. Valido quando il **/fo** parametro è impostato su **tabella**.|
|/v|Visualizza informazioni dettagliate sull'attività nell'output. Per l'output dettagliato completo senza troncamento, usare **/v** e **/svc** tra loro.|
|/FO {tabella \| elenco \| csv}|Specifica il formato da utilizzare per l'output. I valori validi sono **tabella**, **elenco**, e **csv**. Il formato predefinito per l'output è **tabella**.|
|/NH|Elimina le intestazioni di colonna nell'output. Valido quando il **/fo** parametro è impostato su **tabella** oppure **csv**.|
|/fi \<Filter>|Specifica i tipi di processi da includere o escludere dalla query. Vedere la tabella seguente per i nomi di filtro valida, operatori e valori.|
|/?|Visualizza la guida al prompt dei comandi.|

### <a name="filter-names-operators-and-values"></a>I nomi dei filtri, gli operatori e valori

|Nome del filtro|Operatori validi|Valori validi|
|-----------|---------------|------------|
|STATO|eq, ne|IN ESECUZIONE | NON RISPONDE | SCONOSCIUTO|
|IMAGENAME|eq, ne|Nome dell'immagine|
|PID|eq, ne, gt, lt, ge, le|Valore PID|
|SESSIONE|eq, ne, gt, lt, ge, le|Numero della sessione|
|SESSIONNAME|eq, ne|Nome della sessione|
|CPUTIME|eq, ne, gt, lt, ge, le|Tempo di CPU nel formato *HH ***:*** MM ***:*** SS*, dove *MM* e *SS* sono compresi tra 0 e 59 e *HH* è qualsiasi numero di unsigned|
|MEMUSAGE|eq, ne, gt, lt, ge, le|Utilizzo della memoria in KB|
|USERNAME|eq, ne|Qualsiasi nome utente valido|
|SERVIZI|eq, ne|Nome del servizio|
|WINDOWTITLE|eq, ne|Titolo della finestra|
|MODULI|eq, ne|Nome DLL|

## <a name="remarks"></a>Note

I filtri WINDOWTITLE e lo stato non sono supportati quando viene specificato un sistema remoto.

## <a name="BKMK_examples"></a>Esempi

Per elencare tutte le attività con un ID di processo maggiore di 1000 e visualizzarli in formato CSV, digitare:
```
tasklist /v /fi "PID gt 1000" /fo csv
```
Per elencare i processi di sistema attualmente in esecuzione, digitare:
```
tasklist /fi "USERNAME ne NT AUTHORITY\SYSTEM" /fi "STATUS eq running"
```
Per elencare le informazioni dettagliate per tutti i processi attualmente in esecuzione, digitare:
```
tasklist /v /fi "STATUS eq running"
```
Per elencare tutte le informazioni del servizio per i processi nel computer remoto "Srvprinc" con un nome DLL inizia con "ntdll", digitare:
```
tasklist /s srvmain /svc /fi "MODULES eq ntdll*"
```
Per elencare i processi nel computer remoto "Srvprinc", usando le credenziali dell'account utente attualmente connesso, digitare:
```
tasklist /s srvmain 
```
Per elencare i processi nel computer remoto "Srvprinc", usando le credenziali dell'account utente Hiropln, digitare:
```
tasklist /s srvmain /u maindom\hiropln /p p@ssW23
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)