---
title: whoami
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 6e3f4d5c-f1f5-4429-b602-afad2b3488bf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6844ba001c2ebd7407b77f97204069a48a1b595b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840152"
---
# <a name="whoami"></a>whoami



Visualizza le informazioni utente, gruppo e dei privilegi per l'utente attualmente connesso al sistema locale. Se utilizzata senza parametri, **whoami** Visualizza il nome di dominio e l'utente corrente.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
whoami [/upn | /fqdn | /logonid]
whoami {[/user] [/groups] [/priv]} [/fo <Format>] [/nh]
whoami /all [/fo <Format>] [/nh]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/UPN|Visualizza il nome utente nel formato user principal name (UPN).|
|/FQDN|Visualizza il nome utente nel formato di nome (FQDN) di dominio completo.|
|/LOGONID|Visualizza l'ID di accesso dell'utente corrente.|
|/User|Visualizza il nome di dominio e l'utente corrente e l'ID di sicurezza (SID).|
|/Groups|Visualizza i gruppi di utenti a cui appartiene l'utente corrente.|
|/PRIV|Visualizza i privilegi di sicurezza dell'utente corrente.|
|/Fo \<formato >|Specifica il formato di output. I valori validi includono:</br>**tabella** viene visualizzato l'output in una tabella. Rappresenta il valore predefinito.</br>**elenco** viene visualizzato l'output in un elenco.</br>**CSV** viene visualizzato l'output in formato con valori delimitati da virgole (CSV).|
|/all|Visualizza tutte le informazioni nel token di accesso corrente, inclusi il nome dell'utente corrente, gli identificatori di protezione (SID), privilegi e gruppi a cui appartiene l'utente corrente.|
|/NH|Specifica che l'intestazione di colonna non deve essere visualizzato nell'output. Questo è valido solo per i formati CSV e tabella.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="BKMK_examples"></a>Esempi

Per visualizzare il nome di dominio e utente della persona attualmente connessa al computer, digitare:
```
whoami
```
Viene visualizzato l'output simile al seguente:
```
DOMAIN1\administrator
```
Per visualizzare tutte le informazioni nel token di accesso corrente, digitare:
```
whoami /all
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)