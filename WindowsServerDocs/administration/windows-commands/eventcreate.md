---
title: eventcreate
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f2b1b26d-a70e-49a6-832b-91eb5a1a159a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 80d575364adbeba9d9ea4da75a0a866bcc02acea
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818712"
---
# <a name="eventcreate"></a>eventcreate



Consente agli amministratori di creare un evento personalizzato in un registro eventi specificato. Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
eventcreate [/s <Computer> [/u <Domain\User> [/p <Password>]] {[/l {APPLICATION|SYSTEM}]|[/so <SrcName>]} /t {ERROR|WARNING|INFORMATION|SUCCESSAUDIT|FAILUREAUDIT} /id <EventID> /d <Description>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/s \<computer >|Specifica il nome o indirizzo IP di un computer remoto (non utilizzare le barre rovesciate). Il valore predefinito è il computer locale.|
|/u \<Domain\User>|Esegue il comando con le autorizzazioni dell'utente specificato dall'account \<utente > o < dominio\utente >. Il valore predefinito è le autorizzazioni dell'oggetto utente nel computer eseguendo il comando connesso.|
|/p \<Password>|Specifica la password dell'account utente specificato nella **/u** parametro.|
|/l {applicazione\|sistema}|Specifica il nome del registro eventi in cui verrà creato l'evento. I nomi di log valido sono APPLICAZIONI e SISTEMA.|
|/SO \<NomeOrigine >|Specifica l'origine da utilizzare per l'evento. Un'origine valida può essere qualsiasi stringa e deve rappresentare l'applicazione o componente che genera l'evento.|
|/t {errore\|avviso\|informazioni\|</br>SUCCESSAUDIT\|FAILUREAUDIT}|Specifica il tipo di evento da creare. I tipi validi sono ERRORE, AVVISO, INFORMAZIONI, SUCCESSAUDIT e FAILUREAUDIT.|
|/id \<EventID>|Specifica l'ID evento per l'evento. Un ID valido è qualsiasi numero compreso tra 1 e 1000.|
|/d \<description >|Specifica la descrizione da utilizzare per l'evento appena creato.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Eventi personalizzati non possono essere scritti nel Registro di protezione.

## <a name="BKMK_examples"></a>Esempi

Negli esempi seguenti viene illustrano come è possibile utilizzare il comando eventcreate:
```
eventcreate /t error /id 100 /l application /d "Create event in application log"
eventcreate /t information /id 1000 /so winmgmt /d "Create event in WinMgmt source"
eventcreate /t error /id 2001 /so winword /l application /d "new src Winword in application log"
eventcreate /s server /t error /id 100 /l application /d "Remote machine without user credentials"
eventcreate /s server /u user /p password /id 100 /t error /l application /d "Remote machine with user credentials"
eventcreate /s server1 /s server2 /u user /p password /id 100 /t error /so winmgmt /d "Creating events on Multiple remote machines"
eventcreate /s server /u user /id 100 /t warning /so winmgmt /d "Remote machine with partial user credentials"
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)
