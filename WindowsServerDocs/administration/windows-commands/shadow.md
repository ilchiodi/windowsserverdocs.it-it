---
title: shadow
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f81d9717-6883-4e14-9508-4b2a87e48ea7 Lizap
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 125b2971d5d4783ea0b974c45b988cbd1c58a2bf
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877112"
---
# <a name="shadow"></a>shadow

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consente di controllare in remoto una sessione attiva di un altro utente in un server Host sessione Desktop remoto (Host sessione rd).
Per esempi di come usare questo comando, vedere [esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi
```
shadow {<SessionName> | <SessionID>} [/server:<ServerName>] [/v]
```

### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|\<SessionName >|Specifica il nome della sessione che si vuole controllare in remoto.|
|\<SessionID >|Specifica l'ID della sessione che si vuole controllare in remoto. Uso **query utente** per visualizzare l'elenco di sessioni e i relativi ID di sessione.|
|/server:\<ServerName>|Specifica il server Host sessione Desktop remoto che contiene la sessione che si desidera controllare in remoto. Per impostazione predefinita, viene utilizzato il server di desktop remoto Host4 sessione corrente.|
|/v|Consente di visualizzare informazioni sulle azioni eseguibili in esecuzione.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note
-   È possibile visualizzare o controllare attivamente la sessione. Se si sceglie di controllare attivamente una sessione utente, sarà in grado di input da tastiera e mouse azioni per la sessione.
-   È possibile controllare sempre in modalità remota le proprie sessioni (tranne la sessione corrente), ma è necessario disporre dell'autorizzazione controllo completo o l'autorizzazione di accesso speciali controllo remoto per il controllo remoto di un'altra sessione.
-   È anche possibile avviare il controllo remoto tramite Gestione Servizi Desktop remoto.
-   Prima di iniziare il monitoraggio, il server avvisa l'utente che la sessione sta per essere controllato da remoto, a meno che questo avviso è disabilitato. La sessione potrebbe sembrare bloccata per pochi secondi mentre si attende una risposta da parte dell'utente. Per configurare il controllo remoto per utenti e sessioni, utilizzare lo strumento di configurazione di Servizi Desktop remoto o le estensioni di Servizi Desktop remoto per gli utenti locali e i gruppi e active directory gli utenti e computer.
-   La sessione deve essere in grado di supportare la risoluzione video usata durante la sessione che sta controllando in modalità remota o l'operazione ha esito negativo.
-   La sessione della console può non controllare in remoto un'altra sessione né può essere controllato in modalità remota da un'altra sessione.
-   Quando si desidera terminare il controllo remoto (shadowing), premere CTRL + * (tramite \* solo del tastierino numerico).

## <a name="BKMK_examples"></a>Esempi
-   Alla sessione shadow 93, digitare:
    ```
    shadow 93
    ```
-   Per lo shadowing della sessione ACCTG01, digitare:
    ```
    shadow ACCTG01
    ```

#### <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave sintassi della riga di comando](command-line-syntax-key.md)
[Servizi Desktop remoto &#40;servizi Terminal&#41; Guida comandi](remote-desktop-services-terminal-services-command-reference.md)
