---
title: tcmsetup
description: Informazioni su come configurare e disabilitare il client TAPI.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 15e0c10f-996f-4301-92e5-943f7ee8212d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: ac92c7b793274227bd20e6fa90a4106a32ea0446
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862002"
---
# <a name="tcmsetup"></a>tcmsetup



Imposta o disabilita il client TAPI.

## <a name="syntax"></a>Sintassi

```
tcmsetup [/q] [/x] /c <Server1> [<Server2> …] 
tcmsetup  [/q] /c /d
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/q|Impedisce la visualizzazione delle finestre di messaggio.|
|/x|Specifica che i callback orientato alla connessione verranno utilizzati per le reti di traffico elevato in cui la perdita di pacchetti è elevata. Quando questo parametro viene omesso, verranno utilizzati effettuate richiamate.|
|/c|Obbligatorio. Specifica le impostazioni client.|
|\<Server1>|Obbligatorio. Specifica il nome del server remoto con i provider di servizi TAPI che verrà utilizzata dal client. Il client utilizzerà i provider di servizi righe e telefoni. Il client deve essere nello stesso dominio del server o in un dominio che abbia una relazione di trust bidirezionale con il dominio che contiene il server.|
|\<Server2>…|Specifica gli ulteriori server che saranno disponibili per questo client. Se si specifica un elenco di server, usare uno spazio per separare i nomi dei server.|
|/d|Cancella l'elenco dei server remoti. Disabilita il client TAPI, impedendo utilizzano il provider di servizi TAPI che si trovano in server remoti.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Per eseguire questa procedura è necessario essere membri del gruppo Administrators nel computer locale oppure disporre della delega per l'autorità appropriata. Se il computer fa parte di un dominio, i membri del gruppo Domain Admins potrebbero essere in grado di eseguire questa procedura. Per garantire un livello di protezione ottimale, è consigliabile utilizzare la funzione **RunAs** per eseguire questa procedura.
-   Affinché TAPI funzionare correttamente, è necessario eseguire **tcmsetup** per specificare i server remoti che verranno usati dai client TAPI.
-   Prima di un utente del client può usare un telefono o una riga in un server TAPI, l'amministratore del server di telefonia necessario assegnare l'utente per il telefono o la riga.
-   L'elenco dei server di telefonia che viene creato tramite questo comando sostituisce qualsiasi elenco esistente di server di telefonia disponibili al client. È possibile utilizzare questo comando da aggiungere all'elenco esistente.

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)

[Cenni preliminari sulla shell comandi](https://technet.microsoft.com/library/cc737438(v=ws.10).aspx)

[Specificare i server di telefonia in un computer client](https://technet.microsoft.com/library/cc759226(v=ws.10).aspx)

[Assegnare un utente di telefonia a una riga o telefono](https://technet.microsoft.com/library/cc736875(v=ws.10).aspx)

