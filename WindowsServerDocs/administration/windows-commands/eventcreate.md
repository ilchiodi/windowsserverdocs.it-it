---
title: eventcreate
description: Argomento di riferimento per il comando eventcreate, che consente a un amministratore di creare un evento personalizzato in un registro eventi specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f2b1b26d-a70e-49a6-832b-91eb5a1a159a
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 8348e61f6cd94c9b660d0ad9cac4cb1f96920cad
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/16/2020
ms.locfileid: "83436876"
---
# <a name="eventcreate"></a>eventcreate

Consente agli amministratori di creare un evento personalizzato in un registro eventi specificato.

> [!IMPORTANT]
> Gli eventi personalizzati non possono essere scritti nel registro di sicurezza.

## <a name="syntax"></a>Sintassi

```
eventcreate [/s <computer> [/u <domain\user> [/p <password>]] {[/l {APPLICATION|SYSTEM}]|[/so <srcname>]} /t {ERROR|WARNING|INFORMATION|SUCCESSAUDIT|FAILUREAUDIT} /id <eventID> /d <description>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- |------------ |
| /s`<computer>` | Specifica il nome o l'indirizzo IP di un computer remoto (non utilizzare barre rovesciate). Il valore predefinito è il computer locale. |
| /u`<domain\user>` | Esegue il comando con le autorizzazioni dell'account dell'utente specificato da `<user>` o `<domain\user>`. Il valore predefinito è le autorizzazioni dell'oggetto utente nel computer eseguendo il comando connesso. |
| /p`<password>` | Specifica la password dell'account utente specificato nella **/u** parametro. |
| /l`{APPLICATION | SYSTEM}` | Specifica il nome del registro eventi in cui verrà creato l'evento. I nomi di log validi sono **applicazione** o **sistema**. |
| /so`<srcname>` | Specifica l'origine da utilizzare per l'evento. Un'origine valida può essere qualsiasi stringa e deve rappresentare l'applicazione o componente che genera l'evento. |
| /t`{ERROR | WARNING | INFORMATION | SUCCESSAUDIT | FAILUREAUDIT}` | Specifica il tipo di evento da creare. I tipi validi sono **Error**, **warning**, **Information**, **SuccessAudit**e **FailureAudit**. |
| /ID`<eventID>` | Specifica l'ID evento per l'evento. Un ID valido è qualsiasi numero compreso tra 1 e 1000. |
| /d`<description>` | Specifica la descrizione da utilizzare per l'evento appena creato. |
| /? | Visualizza la guida al prompt dei comandi. |

### <a name="examples"></a>Esempi

Gli esempi seguenti illustrano come è possibile usare il comando **eventcreate** :

```
eventcreate /t error /id 100 /l application /d Create event in application log
eventcreate /t information /id 1000 /so winmgmt /d Create event in WinMgmt source
eventcreate /t error /id 2001 /so winword /l application /d new src Winword in application log
eventcreate /s server /t error /id 100 /l application /d Remote machine without user credentials
eventcreate /s server /u user /p password /id 100 /t error /l application /d Remote machine with user credentials
eventcreate /s server1 /s server2 /u user /p password /id 100 /t error /so winmgmt /d Creating events on Multiple remote machines
eventcreate /s server /u user /id 100 /t warning /so winmgmt /d Remote machine with partial user credentials
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
