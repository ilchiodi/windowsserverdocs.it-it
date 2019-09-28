---
title: AD FS risoluzione dei problemi-certificati
description: In questo documento vengono descritti i problemi tipici dei certificati.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: fdadbefc138562246c72f7707b303d966bff0989
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407195"
---
# <a name="ad-fs-troubleshooting---certificates"></a>AD FS risoluzione dei problemi-certificati
Per il corretto funzionamento di AD FS sono necessari i certificati seguenti.  Se una di queste impostazioni non è stata configurata o configurata correttamente, possono verificarsi problemi.  

## <a name="required-certificates"></a>Certificati richiesti
AD FS richiede i certificati seguenti:



- **Trust federativo** : è necessario che un certificato concatenato a un'autorità di certificazione radice Internet (CA) attendibile a vicenda sia presente nell'archivio radice attendibile del provider di attestazioni e di relying party server federativi, la progettazione della certificazione incrociata è stata implementata in cui ogni lato ha scambiato la propria autorità di certificazione radice con il partner o certificati autofirmati che sono stati importati in ogni lato, laddove appropriato.
- **Firma di token** : ogni computer servizio federativo richiede un certificato per la firma di token.  Il certificato per la firma di token del provider di attestazioni deve essere considerato attendibile dal relying party server federativo. Il certificato per la firma di token relying party deve essere considerato attendibile da tutte le applicazioni che ricevono token dal server federativo di relying party.
- **Secure Sockets Layer (SSL)** : il certificato SSL per il servizio federativo deve essere presente in un archivio attendibile nel computer proxy server federativo e presenta una catena valida a un archivio di autorità di certificazione (CA) attendibile.
- **Elenco di revoche di certificati (CRL)** : per tutti i certificati con un CRL pubblicato, il CRL deve essere accessibile a tutti i client e i server che devono accedere al certificato.

Se uno dei precedenti non è configurato correttamente, AD FS non funzionerà.

## <a name="common-things-to-check-with-certificates"></a>Elementi comuni da verificare con i certificati
Di seguito è riportato un elenco di elementi che possono verificarsi e che devono essere verificati quando si tenta di risolvere un problema di certificato.

- Verificare che il certificato sia attendibile
    - I certificati SSL devono essere considerati attendibili dai client
    - I certificati per la firma di token devono essere considerati attendibili dai relying party
- Controllare la catena di certificati. ogni certificato nella catena deve essere valido.
- Verificare la data di scadenza del certificato
- Controllare l'accessibilità dell'elenco di revoche di certificati (CRL)
    - Verificare che il campo CDP sia popolato
    - Passare manualmente al CDP
- Verificare che il certificato non sia stato revocato

## <a name="common-certificate-errors"></a>Errori comuni relativi ai certificati
La tabella seguente elenca gli errori comuni e le possibili cause.

|Evento|Causa|Risoluzione
|-----|-----|-----|
|Evento 249: Impossibile trovare un certificato nell'archivio certificati. Negli scenari di rollover dei certificati questo può causare un errore quando il Servizio federativo esegue la firma o la decrittografia utilizzando questo certificato.|Il certificato in questione non è presente nell'archivio certificati locale o l'account del servizio non dispone dell'autorizzazione per la chiave privata del certificato.|Verificare che il certificato sia installato nell'archivio LocalMAchine\My nel server AD FS. Verificare che l'account del servizio AD FS disponga dell'accesso in lettura alla chiave privata del certificato.|
|Evento 315: si è verificato un errore durante un tentativo di compilazione della catena di certificati per il certificato di firma del trust del provider di attestazioni.|Il certificato è stato revocato.</br></br>Impossibile verificare la catena di certificati.</br></br>Il certificato è scaduto o non è ancora valido.|Verificare che il certificato sia valido e che non sia stato revocato.</br></br>Verificare che l'elenco CRL sia accessibile.|
|Evento 316: si è verificato un errore durante un tentativo di compilazione della catena di certificati per il certificato di firma di attendibilità relying party.|Il certificato è stato revocato.</br></br>Impossibile verificare la catena di certificati.</br></br>Il certificato è scaduto o non è ancora valido.|Verificare che il certificato sia valido e che non sia stato revocato.</br></br>Verificare che l'elenco CRL sia accessibile.|
|Evento 317: si è verificato un errore durante un tentativo di compilazione della catena di certificati per il certificato di crittografia di attendibilità relying party.|Il certificato è stato revocato.</br></br>Impossibile verificare la catena di certificati.</br></br>Il certificato è scaduto o non è ancora valido.|Verificare che il certificato sia valido e che non sia stato revocato.</br></br>Verificare che l'elenco CRL sia accessibile.|
|Evento 319: si è verificato un errore durante la compilazione della catena di certificati per il certificato client.|Il certificato è stato revocato.</br></br>Impossibile verificare la catena di certificati.</br></br>Il certificato è scaduto o non è ancora valido.|Verificare che il certificato sia valido e che non sia stato revocato.</br></br>Verificare che l'elenco CRL sia accessibile.|
|Evento 360: è stata effettuata una richiesta a un endpoint di trasporto del certificato, ma la richiesta non include un certificato client.|La CA radice che ha emesso il certificato client non è attendibile.</br></br>Il certificato client è scaduto.</br></br>Il certificato client è autofirmato e non è attendibile.|Verificare che l'autorità di certificazione radice che ha emesso il certificato client sia presente nell'archivio radice attendibile.</br></br>Verificare che il certificato client non sia scaduto.</br></br>Se il certificato client è autofirmato, assicurarsi che sia stato aggiunto all'elenco di certificati attendibili oppure sostituire il certificato autofirmato con un certificato attendibile.|
|Evento 374: si è verificato un errore durante la compilazione della catena di certificati per il certificato di crittografia del trust del provider di attestazioni.|Il certificato è stato revocato.</br></br>Impossibile verificare la catena di certificati.</br></br>Il certificato è scaduto o non è ancora valido.|Verificare che il certificato sia valido e che non sia stato revocato.</br></br>Verificare che l'elenco CRL sia accessibile.|
|Evento 381: si è verificato un errore durante un tentativo di compilazione della catena di certificati per il certificato di configurazione.|Uno dei certificati configurati per l'utilizzo nel server AD FS è scaduto o è stato revocato.|Verificare che tutti i certificati configurati non siano stati revocati e non siano scaduti.|
|Evento 385-AD FS rilevato che uno o più certificati nel database di configurazione AD FS devono essere aggiornati manualmente.|Uno dei certificati configurati per l'utilizzo nel server AD FS è scaduto o è prossimo alla data di scadenza.|Aggiornare il certificato scaduto o presto in scadenza con una sostituzione. Se si usano certificati autofirmati e il rollover automatico dei certificati è abilitato, questo errore può essere ignorato perché verrà risolto automaticamente.|
|Evento 387-AD FS rilevato che uno o più certificati specificati nel Servizio federativo non sono accessibili all'account di servizio utilizzato dal servizio AD FS Windows.|L'account del servizio AD FS non dispone delle autorizzazioni di lettura per la chiave privata di uno o più certificati configurati.|Verificare che l'account del servizio AD FS disponga delle autorizzazioni di lettura per la chiave privata di tutti i certificati configurati.|
|Evento 389-AD FS rilevato che uno o più Trust richiedono che i certificati vengano aggiornati manualmente perché sono scaduti o scadranno a breve.|Uno dei certificati del partner configurato è scaduto o sta per scadere. Questa operazione può essere applicata a un trust del provider di attestazioni o a un trust relying party.|Se la relazione di trust è stata creata manualmente, aggiornare manualmente la configurazione del certificato. Se sono stati usati metadati federativi per creare il trust, il certificato verrà aggiornato automaticamente non appena il partner aggiornerà il certificato.|




## <a name="next-steps"></a>Passaggi successivi

- [Risoluzione dei problemi relativi ad AD FS](ad-fs-tshoot-overview.md)
 