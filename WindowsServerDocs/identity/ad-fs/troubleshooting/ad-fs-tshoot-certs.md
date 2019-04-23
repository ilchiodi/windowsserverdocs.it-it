---
title: Risoluzione dei problemi di AD FS - certificati
description: Questo documento vengono descritti i problemi di certificato tipico.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 02/21/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 42fdce936bb4a6f25b456c4a96c7b99f650e6033
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59860582"
---
# <a name="ad-fs-troubleshooting---certificates"></a>Risoluzione dei problemi di AD FS - certificati
ADFS richiede che i certificati seguenti per poter funzionare correttamente.  Se uno di questi non sono state il programma di installazione o configurati correttamente possono verificarsi problemi.  

## <a name="required-certificates"></a>Certificati richiesti
ADFS richiede che i certificati seguenti:



- **Relazione di trust federativa** – ciò richiede che sia presente nell'archivio radice attendibile del provider di attestazioni sia relying party, i server federativi, un certificato concatenato a una radice di Internet dell'autorità di certificazione (CA) reciprocamente attendibile un progettazione di certificazione incrociata è stata implementata in cui ogni lato include lo scambio di un certificato CA radice con il proprio partner o autofirmato certificati che sono stati importati su ciascun lato laddove appropriato.
- **Firma di token** – servizio federativo ogni computer richiede un certificato di firma di token.  Il certificato di firma di token del provider di attestazioni deve essere considerato attendibile dalla relying party Server federativo. Il certificato di firma di token di relying party deve essere considerato attendibile da tutte le applicazioni che ricevono i token dal Server federativo della relying Party.
- **Secure Sockets Layer (SSL)** : il certificato SSL per il servizio federativo deve essere presente in un archivio attendibile nel computer proxy Server federativo e ha una catena di certificati valida a un archivio di autorità di certificazione (CA) attendibile.
- **Elenco di revoche di certificati (CRL) del certificato** – in caso di un certificato con un elenco CRL pubblicato, il CRL deve essere accessibile a tutti i client e server che necessitano di accedere al certificato.

Se uno qualsiasi dei valori precedenti non sono configurato correttamente, ADFS non funzionerà.

## <a name="common-things-to-check-with-certificates"></a>Elementi comuni da controllare con certificati
Di seguito è riportato un elenco di elementi che possono verificarsi e deve essere verificato quando provano a risolvere un problema di certificato.

- Assicurarsi che il certificato è attendibile
    - Certificati SSL devono essere considerato attendibile dai client
    - Certificati firma del token devono essere considerato attendibile dalla relying party
- Verificare la catena di attendibilità - ogni certificato nel catena deve essere valido.
- Verificare la data di scadenza certificato
- Verifica accessibilità Strumentazione gestione Windows (CRL, Certificate Revocation List)
    - Assicurarsi che il campo punti di distribuzione viene popolato
    - Esplorare manualmente i punti di distribuzione
- Assicurarsi che il certificato non sia stato revocato

## <a name="common-certificate-errors"></a>Errori comuni del certificato
Nella tabella seguente è un elenco di errori comuni e le possibili cause.

|Evento|Causa|Risoluzione
|-----|-----|-----|
|Evento da 249 - un certificato non è stato trovato nell'archivio certificati. In scenari di rollover dei certificati, questo può causare un errore quando il servizio federativo è firmare o decrittografare usando questo certificato.|Il certificato in questione non è presente nell'archivio certificati locale o l'account del servizio non dispone dell'autorizzazione alla chiave privata del certificato.|Assicurarsi che sia installato il certificato nell'archivio LocalMAchine\My nel server AD FS. Assicurarsi che l'account del servizio AD FS abbia accesso in lettura alla chiave privata del certificato.|
|Evento 315 - si è verificato un errore durante il tentativo di compilare la catena di certificati per i trust del provider di attestazioni certificato di firma.|Il certificato è stato revocato.</br></br>Non è possibile verificare la catena di certificati.</br></br>Il certificato è scaduto o non è ancora valido.|Assicurarsi che il certificato sia valido e non sia stato revocato.</br></br>Assicurarsi che il CRL non sia accessibile.|
|Evento 316 - si è verificato un errore durante il tentativo di compilare la catena di certificati per il trust della relying party, il certificato di firma.|Il certificato è stato revocato.</br></br>Non è possibile verificare la catena di certificati.</br></br>Il certificato è scaduto o non è ancora valido.|Assicurarsi che il certificato sia valido e non sia stato revocato.</br></br>Assicurarsi che il CRL non sia accessibile.|
|Evento 317 - si è verificato un errore durante il tentativo di compilare la catena di certificati per il certificato di crittografia di relying party trust.|Il certificato è stato revocato.</br></br>Non è possibile verificare la catena di certificati.</br></br>Il certificato è scaduto o non è ancora valido.|Assicurarsi che il certificato sia valido e non sia stato revocato.</br></br>Assicurarsi che il CRL non sia accessibile.|
|Evento 319 - errore durante la catena di certificati per il certificato client è stata generata.|Il certificato è stato revocato.</br></br>Non è possibile verificare la catena di certificati.</br></br>Il certificato è scaduto o non è ancora valido.|Assicurarsi che il certificato sia valido e non sia stato revocato.</br></br>Assicurarsi che il CRL non sia accessibile.|
|Evento 360 - è stata effettuata una richiesta per un endpoint di trasporto di certificato, ma la richiesta non include un certificato client.|La radice della CA che ha emesso il certificato client non è attendibile.</br></br>Il certificato client è scaduto.</br></br>Il certificato client è autofirmato e non è attendibile.|Assicurarsi che la radice della CA che ha emesso il certificato client sia presente nell'archivio radice attendibile.</br></br>Assicurarsi che il certificato client non sia scaduto.</br></br>Se il certificato client autofirmato, assicurarsi che sia stato aggiunto all'elenco di certificati attendibili o sostituire il certificato autofirmato con un certificato attendibile.|
|Evento 374 - si è verificato un errore durante la compilazione della catena di certificati per il provider di attestazioni di certificato di crittografia di trust.|Il certificato è stato revocato.</br></br>Non è possibile verificare la catena di certificati.</br></br>Il certificato è scaduto o non è ancora valido.|Assicurarsi che il certificato sia valido e non sia stato revocato.</br></br>Assicurarsi che il CRL non sia accessibile.|
|Evento 381 - si è verificato un errore durante un tentativo di compilare la catena di certificati per certificati di configurazione.|Uno dei certificati configurati per l'uso nel server AD FS è scaduto o è stato revocato.|Assicurarsi che tutti i certificati configurati non sono stati revocati e non sono scaduti.|
|Evento 385 - ADFS ha rilevato che uno o più certificati nel database di configurazione di ADFS deve essere aggiornato manualmente.|Uno dei certificati configurati per l'uso nel server AD FS è scaduto o sta per raggiungere la data di scadenza.|Aggiornare il certificato scaduto o prossimo-a-alla scadenza con una sostituzione. (Se si utilizzano certificati autofirmati e rollover automatico dei certificati è abilitato, questo errore può essere ignorato perché che verrà risolto automaticamente.)|
|Evento 387 - ADFS ha rilevato che uno o più dei certificati specificati nel servizio federativo non erano accessibili all'account del servizio utilizzato dal servizio di Windows di AD FS.|L'account del servizio AD FS non dispone di autorizzazioni di lettura per la chiave privata di uno o più certificati configurati.|Assicurarsi che l'account del servizio ADFS dispone di autorizzazione di lettura per la chiave privata di tutti i certificati configurati.|
|Evento 389 - ADFS ha rilevato che uno o più i trust di richiedere i certificati per aggiornare manualmente perché è scaduti o scadrà a breve.|Uno dei certificati del partner configurato è scaduto o sta per scadere. Ciò può applicare per entrambi un trust del provider di attestazioni o a un trust della relying party.|Se è stato creato manualmente questa relazione di trust, è possibile aggiornare manualmente la configurazione dei certificati. Se i metadati della federazione è stato usato per creare la relazione di trust, il certificato verrà aggiornato automaticamente non appena il partner aggiorna il certificato.|




## <a name="next-steps"></a>Passaggi successivi

- [Risoluzione dei problemi di AD FS](ad-fs-tshoot-overview.md)
 