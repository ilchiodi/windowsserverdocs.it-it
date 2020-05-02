---
title: change logon
description: Argomento di riferimento per il comando change logon, che Abilita o Disabilita gli accessi da sessioni client o Visualizza lo stato di accesso corrente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 41466260-aee9-4333-bcb6-178112c22afd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4a2ebe75f6efa8c3bcfc0018d1f4e6051bb9ebb7
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82716126"
---
# <a name="change-logon"></a>change logon

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Abilita o Disabilita gli accessi da sessioni client o Visualizza lo stato di accesso corrente. Questa utilità è utile per la manutenzione del sistema. Per eseguire questo comando, è necessario essere un amministratore.

> [!NOTE]
> In Windows Server 2008 R2, Servizi terminal si chiama ora Servizi Desktop remoto. Per informazioni sulle novità della versione più recente, vedere Novità di [Servizi Desktop remoto in Windows Server](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/dn283323(v=ws.11)).

## <a name="syntax"></a>Sintassi

```
change logon {/query | /enable | /disable | /drain | /drainuntilrestart}
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| /query | Visualizza lo stato di accesso corrente, se abilitato o disabilitato. |
| /Enable | Consente l'accesso da sessioni client, ma non dalla console di. |
| /Disable | Disabilita gli accessi successivi dalle sessioni client, ma non dalla console. Non influisce sugli utenti attualmente connessi. |
| /drain | Disabilita gli accessi dalle nuove sessioni client, ma consente la riconnessione a sessioni esistenti. |
| /drainuntilrestart | Disabilita gli accessi dalle nuove sessioni client finché il computer non viene riavviato, ma consente la riconnessione a sessioni esistenti. |
| /? | Visualizza la guida al prompt dei comandi. |

#### <a name="remarks"></a>Osservazioni

- Gli accessi vengono abilitati di nuovo al riavvio del sistema.

- Se si è connessi al server host sessione Desktop remoto da una sessione client e si disabilitano gli accessi e si disconnettono prima di riattivare gli accessi, non sarà possibile riconnettersi alla sessione. Per riabilitare gli accessi da sessioni client, accedere alla console di.

### <a name="examples"></a>Esempi

- Per visualizzare lo stato di accesso corrente, digitare:
  
  ```
  change logon /query
  ```

- Per abilitare gli accessi da sessioni client, digitare:

  ```
  change logon /enable
  ```

- Per disabilitare gli accessi client, digitare:

  ```
  change logon /disable
  ```
  
## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Change](change.md)

- [Informazioni di riferimento sui comandi di Servizi Desktop remoto (Servizi terminal)](remote-desktop-services-terminal-services-command-reference.md)
