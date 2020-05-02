---
title: eventcreate
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f2b1b26d-a70e-49a6-832b-91eb5a1a159a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 797298622ba1021caef3d04e2f2f06f016ef6a70
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725766"
---
# <a name="eventcreate"></a>eventcreate



Consente agli amministratori di creare un evento personalizzato in un registro eventi specificato. 

## <a name="syntax"></a>Sintassi

```
eventcreate [/s <Computer> [/u <Domain\User> [/p <Password>]] {[/l {APPLICATION|SYSTEM}]|[/so <SrcName>]} /t {ERROR|WARNING|INFORMATION|SUCCESSAUDIT|FAILUREAUDIT} /id <EventID> /d <Description>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/s \<> computer|Specifica il nome o l'indirizzo IP di un computer remoto (non utilizzare barre rovesciate). Il valore predefinito è il computer locale.|
|/u \<dominio\utente>|Esegue il comando con le autorizzazioni dell'account dell'utente specificato dall' \<utente> o <dominio\utente>. Il valore predefinito è le autorizzazioni dell'oggetto utente nel computer eseguendo il comando connesso.|
|/p \<password>|Specifica la password dell'account utente specificato nella **/u** parametro.|
|/l {APPLICATION\|System}|Specifica il nome del registro eventi in cui verrà creato l'evento. I nomi di log valido sono APPLICAZIONI e SISTEMA.|
|/so \<NomeOrigine>|Specifica l'origine da utilizzare per l'evento. Un'origine valida può essere qualsiasi stringa e deve rappresentare l'applicazione o componente che genera l'evento.|
|/t {informazioni\|sugli\|errori di avviso\|</br>SUCCESSAUDIT\|FailureAudit}|Specifica il tipo di evento da creare. I tipi validi sono ERRORE, AVVISO, INFORMAZIONI, SUCCESSAUDIT e FAILUREAUDIT.|
|/ID \<eventid>|Specifica l'ID evento per l'evento. Un ID valido è qualsiasi numero compreso tra 1 e 1000.|
|/d \<Descrizione>|Specifica la descrizione da utilizzare per l'evento appena creato.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni

-   Eventi personalizzati non possono essere scritti nel Registro di protezione.

## <a name="examples"></a>Esempi

Negli esempi seguenti viene illustrano come è possibile utilizzare il comando eventcreate:
```
eventcreate /t error /id 100 /l application /d Create event in application log
eventcreate /t information /id 1000 /so winmgmt /d Create event in WinMgmt source
eventcreate /t error /id 2001 /so winword /l application /d new src Winword in application log
eventcreate /s server /t error /id 100 /l application /d Remote machine without user credentials
eventcreate /s server /u user /p password /id 100 /t error /l application /d Remote machine with user credentials
eventcreate /s server1 /s server2 /u user /p password /id 100 /t error /so winmgmt /d Creating events on Multiple remote machines
eventcreate /s server /u user /id 100 /t warning /so winmgmt /d Remote machine with partial user credentials
```

#### <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
