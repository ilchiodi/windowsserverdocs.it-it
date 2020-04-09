---
title: shadow
description: Argomento Windows Commands per Shadow, che consente di controllare in modalità remota una sessione attiva di un altro utente in un host sessione Desktop remoto server.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: f81d9717-6883-4e14-9508-4b2a87e48ea7 Lizap
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 90c3202d810257cc94c73b88c5c1627901f54af0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834334"
---
# <a name="shadow"></a>shadow

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Consente di controllare in modalità remota una sessione attiva di un altro utente in un server host sessione Desktop remoto.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi
```
shadow {<SessionName> | <SessionID>} [/server:<ServerName>] [/v]
```

#### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|\<sessionname >|Specifica il nome della sessione che si desidera controllare in remoto.|
|\<SessionID >|Specifica l'ID della sessione che si desidera controllare in remoto. Usare **query user** per visualizzare l'elenco delle sessioni e i relativi ID di sessione.|
|/Server:\<ServerName >|Specifica il server Host sessione Desktop remoto contenente la sessione che si desidera controllare in remoto. Per impostazione predefinita, viene usato il server Host4 sessione Desktop remoto corrente.|
|/v|Visualizza le informazioni sulle azioni eseguite.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note
-   È possibile visualizzare o controllare attivamente la sessione. Se si sceglie di controllare attivamente la sessione di un utente, sarà possibile immettere le azioni della tastiera e del mouse per la sessione.
-   È sempre possibile controllare in remoto le proprie sessioni, ad eccezione della sessione corrente, ma è necessario disporre dell'autorizzazione controllo completo o del controllo remoto per l'accesso in remoto a un'altra sessione.
-   È anche possibile avviare il controllo remoto tramite Servizi Desktop remoto Manager.
-   Prima dell'avvio del monitoraggio, il server avvisa l'utente che la sessione sta per essere controllata in remoto, a meno che questo avviso non sia disabilitato. La sessione potrebbe sembrare bloccata per alcuni secondi durante l'attesa di una risposta da parte dell'utente. Per configurare il controllo remoto per utenti e sessioni, usare lo strumento di configurazione Servizi Desktop remoto o le estensioni Servizi Desktop remoto per utenti e gruppi locali e utenti e computer di Active Directory.
-   La sessione deve essere in grado di supportare la risoluzione video utilizzata nella sessione controllata in remoto oppure l'operazione non riesce.
-   La sessione della console non può controllare in modalità remota un'altra sessione né può essere controllata in remoto da un'altra sessione.
-   Quando si vuole terminare il controllo remoto (ombreggiatura), premere CTRL +\* (usando \* solo dal tastierino numerico).

## <a name="examples"></a><a name=BKMK_examples></a>Esempi
-   Per eseguire l'ombreggiatura della sessione 93, digitare:
    ```
    shadow 93
    ```
-   Per nascondere la sessione ACCTG01, digitare:
    ```
    shadow ACCTG01
    ```

## <a name="additional-references"></a>Altre informazioni di riferimento
- Guida di riferimento alla [chiave della sintassi della riga di comando](command-line-syntax-key.md)
[Servizi Desktop remoto (Servizi terminal)](remote-desktop-services-terminal-services-command-reference.md)
