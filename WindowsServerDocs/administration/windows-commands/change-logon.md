---
title: change logon
description: Argomento dei comandi di Windows per Change logon, che Abilita o Disabilita gli accessi da sessioni client o Visualizza lo stato di accesso corrente.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 41466260-aee9-4333-bcb6-178112c22afd
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6a101fc567981716536ecad8e754b81a43eb2c91
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80848154"
---
# <a name="change-logon"></a>change logon

> Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Abilita o Disabilita gli accessi da sessioni client o Visualizza lo stato di accesso corrente. Questa utilità è utile per la manutenzione del sistema.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

> [!NOTE]
> In Windows Server 2008 R2 Servizi terminal è stato rinomato Servizi Desktop remoto. Per informazioni sulle novità della versione più recente, vedere Novità di [Servizi Desktop remoto in Windows server 2012](https://technet.microsoft.com/library/hh831527) nella libreria TechNet di Windows Server.

## <a name="syntax"></a>Sintassi
```
change logon {/query | /enable | /disable | /drain | /drainuntilrestart}
```
### <a name="parameters"></a>Parametri

|     Parametro      |                                                       Descrizione                                                        |
|--------------------|--------------------------------------------------------------------------------------------------------------------------|
|       /query       |                             Visualizza lo stato di accesso corrente, se abilitato o disabilitato.                              |
|      /Enable       |                              Consente l'accesso da sessioni client, ma non dalla console di.                              |
|      /Disable      |  Disabilita gli accessi successivi dalle sessioni client, ma non dalla console. Non influisce sugli utenti attualmente connessi.   |
|       /drain       |                 Disabilita gli accessi dalle nuove sessioni client, ma consente la riconnessione a sessioni esistenti.                 |
| /drainuntilrestart | Disabilita gli accessi dalle nuove sessioni client finché il computer non viene riavviato, ma consente la riconnessione a sessioni esistenti. |
|         /?         |                                           Visualizza la guida al prompt dei comandi.                                           |

## <a name="remarks"></a>Note
- Solo gli amministratori possono usare il comando **Change logon** .
- Gli accessi vengono abilitati di nuovo al riavvio del sistema. Se si è connessi al server host sessione Desktop remoto (host sessione Desktop remoto) da una sessione client e si disabilitano gli accessi e quindi si disconnette prima di riabilitare gli accessi, non sarà possibile riconnettersi alla sessione. Per riabilitare gli accessi da sessioni client, accedere alla console di.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

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
  
## <a name="additional-references"></a>Altri riferimenti
- - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
- [change](change.md)
- [Informazioni di riferimento sui comandi di Servizi Desktop remoto (Servizi terminal)](remote-desktop-services-terminal-services-command-reference.md)
