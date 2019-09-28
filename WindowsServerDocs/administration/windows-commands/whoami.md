---
title: whoami
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 9731ba3be3983eb53ade88fceaee863800229084
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71362137"
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
|/fo \<Format >|Specifica il formato di output. I valori validi includono:</br>**tabella** viene visualizzato l'output in una tabella. Rappresenta il valore predefinito.</br>**elenco** viene visualizzato l'output in un elenco.</br>**CSV** viene visualizzato l'output in formato con valori delimitati da virgole (CSV).|
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

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)