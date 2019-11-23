---
title: shadow
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: cb2fad4b0a553e736755f2dc56e5d88297a1fef5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71383963"
---
# <a name="shadow"></a>shadow

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consente di controllare in modalità remota una sessione attiva di un altro utente in un server di host sessione Desktop remoto (host sessione Desktop remoto).
per esempi relativi all'uso di questo comando, vedere [esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi
```
shadow {<SessionName> | <SessionID>} [/server:<ServerName>] [/v]
```

### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|\<sessionname >|Specifica il nome della sessione che si desidera controllare in remoto.|
|\<SessionID >|Specifica l'ID della sessione che si desidera controllare in remoto. Usare **query user** per visualizzare l'elenco delle sessioni e i relativi ID di sessione.|
|/Server:\<ServerName >|Specifica il server Host sessione Desktop remoto contenente la sessione che si desidera controllare in remoto. Per impostazione predefinita, viene usato il server Host4 sessione Desktop remoto corrente.|
|/v|Visualizza le informazioni sulle azioni eseguite.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni
-   È possibile visualizzare o controllare attivamente la sessione. Se si sceglie di controllare attivamente la sessione di un utente, sarà possibile immettere le azioni della tastiera e del mouse per la sessione.
-   È sempre possibile controllare in remoto le proprie sessioni, ad eccezione della sessione corrente, ma è necessario disporre dell'autorizzazione controllo completo o del controllo remoto per l'accesso in remoto a un'altra sessione.
-   È anche possibile avviare il controllo remoto tramite Servizi Desktop remoto Manager.
-   Prima dell'avvio del monitoraggio, il server avvisa l'utente che la sessione sta per essere controllata in remoto, a meno che questo avviso non sia disabilitato. La sessione potrebbe sembrare bloccata per alcuni secondi durante l'attesa di una risposta da parte dell'utente. Per configurare il controllo remoto per utenti e sessioni, usare lo strumento di configurazione Servizi Desktop remoto o le estensioni Servizi Desktop remoto per utenti e gruppi locali e utenti e computer di Active Directory.
-   La sessione deve essere in grado di supportare la risoluzione video utilizzata nella sessione controllata in remoto oppure l'operazione non riesce.
-   La sessione della console non può controllare in modalità remota un'altra sessione né può essere controllata in remoto da un'altra sessione.
-   Quando si vuole terminare il controllo remoto (ombreggiatura), premere CTRL +\* (usando \* solo dal tastierino numerico).

## <a name="BKMK_examples"></a>Esempi
-   Per eseguire l'ombreggiatura della sessione 93, digitare:
    ```
    shadow 93
    ```
-   Per nascondere la sessione ACCTG01, digitare:
    ```
    shadow ACCTG01
    ```

#### <a name="additional-references"></a>riferimenti aggiuntivi
[Chiave di sintassi della riga di comando](command-line-syntax-key.md)
[riferimento ai comandi di &#40;Servizi Desktop remoto Servizi terminal&#41; ](remote-desktop-services-terminal-services-command-reference.md)
