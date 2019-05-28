---
title: taskkill
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2b71e792-08b6-46d4-95a5-cb6336a79524
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c31098a7dc151b29def2f3615da1e969ff8c5664
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222947"
---
# <a name="taskkill"></a>taskkill

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Termina una o più attività o processi. È possibile terminare i processi in base al nome di immagine o all'ID di processo. **Taskkill** sostituisce il **kill** dello strumento.
Per esempi di come usare questo comando, vedere [esempi](#examples).

## <a name="syntax"></a>Sintassi
```
taskkill [/s <computer> [/u [<Domain>\]<UserName> [/p [<Password>]]]] {[/fi <Filter>] [...] [/pid <ProcessID> | /im <ImageName>]} [/f] [/t]
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|/s \<computer>|Specifica il nome o indirizzo IP di un computer remoto (non utilizzare le barre rovesciate). Il valore predefinito è il computer locale.|
|/u \<Domain>\\\<UserName>|Esegue il comando con le autorizzazioni dell'utente che ha specificato dall'account *nomeutente* oppure *Domain*\\*UserName*. **/u** può essere specificato solo se **/s** è specificato. Il valore predefinito è le autorizzazioni dell'utente attualmente connesso al computer in cui viene eseguito il comando.|
|/p \<Password>|Specifica la password dell'account utente specificato nella **/u** parametro.|
|/fi \<Filter>|Si applica un filtro per selezionare un set di attività. È possibile usare più di un filtro o usare il carattere jolly ( **\*** ) per specificare tutte le attività o i nomi di immagine. Vedere gli argomenti seguenti [tabella per i nomi di filtro valida](#filter-names-operators-and-values), operatori e valori.|
|/pid \<ProcessID>|Specifica l'ID del processo del processo da terminare.|
|/IM \<ImageName >|Specifica il nome dell'immagine del processo da terminare. Usare il carattere jolly ( **\*** ) per specificare tutti i nomi di immagine.|
|/f|Specifica che i processi terminati in modo forzato. Questo parametro viene ignorato per i processi remoti. tutti i processi remoti vengono interrotti.|
|/t|Termina il processo specificato e tutti i processi figlio avviati da esso.|

#### <a name="filter-names-operators-and-values"></a>I nomi dei filtri, gli operatori e valori
|Nome del filtro|Operatori validi|Valori validi|
|--------|----------|----------|
|STatUS|eq, ne|ESECUZIONE DI &AMP;#124; NON RISPONDE &AMP;#124; SCONOSCIUTA|
|IMAGENAME|eq, ne|Nome dell'immagine|
|PID|eq, ne, gt, lt, ge, le|Valore PID|
|SESSIONE|eq, ne, gt, lt, ge, le|Numero della sessione|
|CPUtime|eq, ne, gt, lt, ge, le|Tempo di CPU nel formato *HH ***:*** MM ***:*** SS*, dove *MM* e *SS* sono compresi tra 0 e 59 e *HH* è qualsiasi numero di unsigned|
|MEMUSAGE|eq, ne, gt, lt, ge, le|Utilizzo della memoria in KB|
|USERNAME|eq, ne|Qualsiasi nome utente valido (*utente* oppure *Domain*\\*utente*)|
|SERVIZI|eq, ne|Nome del servizio|
|WINDOWTITLE|eq, ne|Titolo della finestra|
|MODULI|eq, ne|Nome DLL|

## <a name="remarks"></a>Note
* I filtri WINDOWTITLE e lo stato non sono supportati quando viene specificato un sistema remoto.
* Il carattere jolly ( **\*** ) è accettato per il **/im** opzione solo quando viene applicato un filtro.
* Terminazione dei processi remoti viene sempre eseguita in modo forzato, indipendentemente dal fatto che il **/f** opzione specificata.
* Specificando un nome di computer per il filtro hostname filter causa un arresto e tutti i processi vengono arrestati.
* È possibile usare **tasklist** per determinare l'ID processo (PID) per il processo da terminare.

## <a name="examples"></a>Esempi
Per terminare i processi con 1230 gli ID di processo, 1241 e 1253, digitare:
```
taskkill /pid 1230 /pid 1241 /pid 1253
```
Per terminare forzatamente il processo "Notepad.exe" se è stata avviata dal sistema, digitare:
```
taskkill /f /fi "USERNAME eq NT AUTHORITY\SYSTEM" /im notepad.exe
```
Per terminare tutti i processi nel computer remoto "Srvprinc" con un'immagine nome che inizia con "Nota", quando si usa le credenziali per l'account utente Hiropln, digitare:
```
taskkill /s srvmain /u maindom\hiropln /p p@ssW23 /fi "IMAGENAME eq note*" /im *
```
Per terminare il processo con elementi figlio e il processo di ID 2134 elabora che avviato, ma solo se i processi avviati dall'account di amministratore, digitare:
```
taskkill /pid 2134 /t /fi "username eq administrator"
```
Per terminare tutti i processi con un ID di processo, maggiore o uguale a 1000, indipendentemente dalla loro i nomi delle immagini, digitare:
```
taskkill /f /fi "PID ge 1000" /im *
```

#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
