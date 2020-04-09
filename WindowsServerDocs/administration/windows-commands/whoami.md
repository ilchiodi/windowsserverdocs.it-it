---
title: whoami
description: Windows Commands Topic for whoami, che visualizza le informazioni su utenti, gruppi e privilegi per l'utente attualmente connesso al sistema locale.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 6e3f4d5c-f1f5-4429-b602-afad2b3488bf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9ff45ed95b35215859f2f83aec75b33570ef46d2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80829274"
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

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/UPN|Visualizza il nome utente nel formato user principal name (UPN).|
|/FQDN|Visualizza il nome utente nel formato di nome (FQDN) di dominio completo.|
|/LOGONID|Visualizza l'ID di accesso dell'utente corrente.|
|/User|Visualizza il nome di dominio e l'utente corrente e l'ID di sicurezza (SID).|
|/groups|Visualizza i gruppi di utenti a cui appartiene l'utente corrente.|
|/PRIV|Visualizza i privilegi di sicurezza dell'utente corrente.|
|Formato \</fo >|Specifica il formato di output. I valori validi includono:</br>**tabella** viene visualizzato l'output in una tabella. Questo è il valore predefinito.</br>**elenco** viene visualizzato l'output in un elenco.</br>**CSV** viene visualizzato l'output in formato con valori delimitati da virgole (CSV).|
|/all|Visualizza tutte le informazioni nel token di accesso corrente, inclusi il nome dell'utente corrente, gli identificatori di protezione (SID), privilegi e gruppi a cui appartiene l'utente corrente.|
|/NH|Specifica che l'intestazione di colonna non deve essere visualizzato nell'output. Questo è valido solo per i formati CSV e tabella.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

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

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)