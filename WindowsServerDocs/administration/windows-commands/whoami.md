---
title: whoami
description: Argomento di riferimento per whoami, che visualizza le informazioni su utenti, gruppi e privilegi per l'utente attualmente connesso al sistema locale.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6e3f4d5c-f1f5-4429-b602-afad2b3488bf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2d672b3aaa20125c5c1da10fa3a5811fb5060d11
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82725799"
---
# <a name="whoami"></a>whoami



Visualizza le informazioni utente, gruppo e dei privilegi per l'utente attualmente connesso al sistema locale. Se utilizzata senza parametri, **whoami** Visualizza il nome di dominio e l'utente corrente.



## <a name="syntax"></a>Sintassi

```
whoami [/upn | /fqdn | /logonid]
whoami {[/user] [/groups] [/priv]} [/fo <Format>] [/nh]
whoami /all [/fo <Format>] [/nh]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/UPN|Visualizza il nome utente nel formato user principal name (UPN).|
|/FQDN|Visualizza il nome utente nel formato di nome (FQDN) di dominio completo.|
|/LOGONID|Visualizza l'ID di accesso dell'utente corrente.|
|/User|Visualizza il nome di dominio e l'utente corrente e l'ID di sicurezza (SID).|
|/Groups|Visualizza i gruppi di utenti a cui appartiene l'utente corrente.|
|/PRIV|Visualizza i privilegi di sicurezza dell'utente corrente.|
|> \<formato/fo|Specifica il formato di output. I valori validi includono:</br>**tabella** di Visualizza l'output in una tabella. Questo è il valore predefinito.</br>**elenco** di Visualizza l'output in un elenco.</br>**CSV** viene visualizzato l'output in formato con valori delimitati da virgole (CSV).|
|/all|Visualizza tutte le informazioni nel token di accesso corrente, inclusi il nome dell'utente corrente, gli identificatori di protezione (SID), privilegi e gruppi a cui appartiene l'utente corrente.|
|/NH|Specifica che l'intestazione di colonna non deve essere visualizzato nell'output. Questo è valido solo per i formati CSV e tabella.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="examples"></a>Esempi

Per visualizzare il nome di dominio e utente della persona attualmente connessa al computer, digitare:
```
whoami
```
Verrà visualizzato un output simile al seguente:
```
DOMAIN1\administrator
```
Per visualizzare tutte le informazioni nel token di accesso corrente, digitare:
```
whoami /all
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)