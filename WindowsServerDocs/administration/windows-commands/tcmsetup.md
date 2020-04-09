---
title: tcmsetup
description: Informazioni su come configurare e disabilitare il client TAPI.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 15e0c10f-996f-4301-92e5-943f7ee8212d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: bbfc9a0238d258f11233b0e48a30048c1d62cef4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833414"
---
# <a name="tcmsetup"></a>tcmsetup



Imposta o Disabilita il client TAPI.

## <a name="syntax"></a>Sintassi

```
tcmsetup [/q] [/x] /c <Server1> [<Server2> …] 
tcmsetup  [/q] /c /d
```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/q|Impedisce la visualizzazione delle finestre di messaggio.|
|/x|Specifica che i callback orientati alla connessione verranno usati per reti con traffico elevato in cui la perdita di pacchetti è elevata. Se questo parametro viene omesso, verranno utilizzati callback senza connessione.|
|/c|Obbligatoria. Specifica la configurazione client.|
|\<server1 >|Obbligatoria. Specifica il nome del server remoto che contiene i provider di servizi TAPI che il client utilizzerà. Il client userà le linee e i telefoni dei provider di servizi. Il client deve trovarsi nello stesso dominio del server o in un dominio che ha una relazione di trust bidirezionale con il dominio che contiene il server.|
|\<Server2 >...|Specifica eventuali server o server aggiuntivi che saranno disponibili per questo client. Se si specifica un elenco di server, utilizzare uno spazio per separare i nomi dei server.|
|/d|Cancella l'elenco dei server remoti. Disabilita il client TAPI evitando che usi i provider di servizi TAPI presenti nei server remoti.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Note

-   Per eseguire questa procedura, è necessario essere membro del gruppo Administrators sul computer locale o aver ricevuto in delega l'autorizzazione appropriata. Se il computer appartiene a un dominio, i membri del gruppo Domain Admins potrebbero essere in grado di eseguire questa procedura. Per garantire un livello di protezione ottimale, è consigliabile utilizzare la funzione **RunAs** per eseguire questa procedura.
-   Per il corretto funzionamento di TAPI, è necessario eseguire **tcmsetup** per specificare i server remoti che verranno usati dai client TAPI.
-   Prima che un utente client possa utilizzare un telefono o una linea in un server TAPI, l'amministratore del server di telefonia deve assegnare l'utente al telefono o alla riga.
-   L'elenco dei server di telefonia creati da questo comando sostituisce qualsiasi elenco esistente di server di telefonia disponibili per il client. Non è possibile usare questo comando per aggiungere all'elenco esistente.

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

[Panoramica della shell comandi](https://technet.microsoft.com/library/cc737438(v=ws.10).aspx)

[Specificare i server di telefonia in un computer client](https://technet.microsoft.com/library/cc759226(v=ws.10).aspx)

[Assegnare un utente di telefonia a una riga o a un telefono](https://technet.microsoft.com/library/cc736875(v=ws.10).aspx)

