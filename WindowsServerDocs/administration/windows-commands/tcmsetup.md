---
title: tcmsetup
description: Informazioni su come configurare e disabilitare il client TAPI.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 0c646acef51f06c57f16ec7e5310e3319a11383f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370676"
---
# <a name="tcmsetup"></a>tcmsetup



Imposta o Disabilita il client TAPI.

## <a name="syntax"></a>Sintassi

```
tcmsetup [/q] [/x] /c <Server1> [<Server2> …] 
tcmsetup  [/q] /c /d
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/q|Impedisce la visualizzazione delle finestre di messaggio.|
|/x|Specifica che i callback orientati alla connessione verranno usati per reti con traffico elevato in cui la perdita di pacchetti è elevata. Se questo parametro viene omesso, verranno utilizzati callback senza connessione.|
|/c|Obbligatorio. Specifica la configurazione client.|
|\<Server1 >|Obbligatorio. Specifica il nome del server remoto che contiene i provider di servizi TAPI che il client utilizzerà. Il client userà le linee e i telefoni dei provider di servizi. Il client deve trovarsi nello stesso dominio del server o in un dominio che ha una relazione di trust bidirezionale con il dominio che contiene il server.|
|\<Server2 >...|Specifica eventuali server o server aggiuntivi che saranno disponibili per questo client. Se si specifica un elenco di server, utilizzare uno spazio per separare i nomi dei server.|
|/d|Cancella l'elenco dei server remoti. Disabilita il client TAPI evitando che usi i provider di servizi TAPI presenti nei server remoti.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Per eseguire questa procedura è necessario essere membri del gruppo Administrators nel computer locale oppure disporre della delega per l'autorità appropriata. Se il computer fa parte di un dominio, i membri del gruppo Domain Admins potrebbero essere in grado di eseguire questa procedura. Per garantire un livello di protezione ottimale, è consigliabile utilizzare la funzione **RunAs** per eseguire questa procedura.
-   Per il corretto funzionamento di TAPI, è necessario eseguire **tcmsetup** per specificare i server remoti che verranno usati dai client TAPI.
-   Prima che un utente client possa utilizzare un telefono o una linea in un server TAPI, l'amministratore del server di telefonia deve assegnare l'utente al telefono o alla riga.
-   L'elenco dei server di telefonia creati da questo comando sostituisce qualsiasi elenco esistente di server di telefonia disponibili per il client. Non è possibile usare questo comando per aggiungere all'elenco esistente.

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

[Panoramica della shell comandi](https://technet.microsoft.com/library/cc737438(v=ws.10).aspx)

[Specificare i server di telefonia in un computer client](https://technet.microsoft.com/library/cc759226(v=ws.10).aspx)

[Assegnare un utente di telefonia a una riga o a un telefono](https://technet.microsoft.com/library/cc736875(v=ws.10).aspx)

