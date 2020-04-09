---
title: Messaggi WSUS e suggerimenti per la risoluzione dei problemi
description: Argomento di Windows Server Update Service (WSUS)-risolvere i problemi usando i messaggi WSUS
ms.prod: windows-server
ms.technology: manage-wsus
ms.topic: article
ms.assetid: 9f6317f7-bfe0-42d9-87ce-d8f038c728ca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: e4fe14eeaba3fc82e125288f8c47fb445f6e00b0
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80828314"
---
# <a name="wsus-messages-and-troubleshooting-tips"></a>Messaggi WSUS e suggerimenti per la risoluzione dei problemi

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento vengono fornite informazioni sui messaggi di WSUS seguenti:

-   Stato del computer non segnalato

-   ID messaggio 6703-sincronizzazione WSUS non riuscita

-   Errore 0x80070643: errore irreversibile durante l'installazione

-   Alcuni servizi non sono in esecuzione. Controllare i seguenti servizi [...]

## <a name="computer-has-not-reported-status"></a>Stato del computer non segnalato
Questo messaggio viene generato nella console di WSUS quando un computer client WSUS non invia informazioni al server WSUS per indicare lo stato di aggiornamento corrente. Questo problema è in genere causato dal computer client WSUS, non dal server WSUS.

I motivi più comuni sono:

-   Il computer ha perso la connettività alla rete:
    -   Il cavo di rete è scollegato.
    -   Un cavo di rete corrispondente è difettoso.
    -   Il computer dispone di una scheda di rete difettosa.
    -   La porta di rete a cui si connette il computer è stata disabilitata.
    -   Non è possibile associare la scheda wireless a e connettersi al punto di accesso wireless aziendale.
-   Il computer è disattivato. (È stato arrestato o è in modalità di sospensione o ibernazione).

## <a name="message-id-6703---wsus-synchronization-failed"></a>ID messaggio 6703-sincronizzazione WSUS non riuscita
> Messaggio: la richiesta non è riuscita con stato HTTP 503: servizio non disponibile.
> 
> Origine: Microsoft. UpdateServices. Administration. AdminProxy. createUpdateServer.

Quando si tenta di aprire Update Services nel server WSUS, viene visualizzato l'errore seguente:

> Errore: errore di connessione
> 
> Si è verificato un errore durante il tentativo di connessione al server WSUS. Questo errore può verificarsi per diversi motivi. Se il problema persiste, contattare l'amministratore di rete. Fare clic sul nodo Reimposta server per connettersi di nuovo al server.

Oltre al precedente, i tentativi di accesso all'URL per il sito Web di amministrazione di WSUS, ad esempio `http://CM12CAS:8530`, non riescono con l'errore:

> Errore HTTP 503. Il servizio non è disponibile

In questa situazione, la causa più probabile è che il pool di applicazioni WsusPool in IIS si trovi nello stato interrotto.

Inoltre, il limite di memoria privata (KB) per il pool di applicazioni è probabilmente impostato sul valore predefinito di 1843200 KB. Se si verifica questo problema, aumentare il limite di memoria privata a 4GB (4 milioni KB) e riavviare il pool di applicazioni. Per aumentare il limite di memoria privata, selezionare il pool di applicazioni WsusPool e fare clic su Impostazioni avanzate in Modifica pool di applicazioni. Impostare quindi il limite di memoria privata su 4 GB (4 milioni KB). Una volta riavviato il pool di applicazioni, monitorare lo stato del componente SMS_WSUS_SYNC_MANAGER, file WCM. log e wsyncmgr. log per individuare eventuali errori. Si noti che potrebbe essere necessario aumentare il limite di memoria privata a 8 GB (8 milioni KB) o superiore, a seconda dell'ambiente.

Per ulteriori informazioni, vedere la pagina relativa alla [sincronizzazione di WSUS in ConfigMgr 2012 ha esito negativo con errori HTTP 503](https://blogs.technet.com/b/sus/archive/2015/03/23/configmgr-2012-support-tip-wsus-sync-fails-with-http-503-errors.aspx) .

## <a name="error-0x80070643-fatal-error-during-installation"></a>Errore 0x80070643: errore irreversibile durante l'installazione
Il programma di installazione di WSUS USA Microsoft SQL Server per eseguire l'installazione. Questo problema si verifica perché l'utente che esegue l'installazione di WSUS non dispone delle autorizzazioni di amministratore di sistema in SQL Server.

Per risolvere il problema, concedere le autorizzazioni di amministratore di sistema a un account utente o a un account di gruppo in SQL Server, quindi eseguire di nuovo l'installazione di WSUS.

## <a name="some-services-are-not-running-check-the-following-services"></a>Alcuni servizi non sono in esecuzione. Controllare i servizi seguenti:

- **SelfUpdate:** Vedere [aggiornamenti automatici necessario aggiornare](https://technet.microsoft.com/library/cc708554(v=ws.10).aspx) per informazioni sulla risoluzione dei problemi del servizio SelfUpdate.

- **WSSUService. exe:** Questo servizio semplifica la sincronizzazione. In caso di problemi di sincronizzazione, accedere a WSUSService. exe facendo clic sul pulsante **Start**, scegliendo **strumenti di amministrazione**, facendo clic su **Servizi**e quindi individuando **Windows Server Update Service** nell'elenco dei servizi. eseguire le operazioni descritte di seguito.
    
    -   Verificare che il servizio sia in esecuzione. Fare clic su **Avvia** se è stato arrestato o **Riavvia** per aggiornare il servizio.
    
    -   Utilizzare Visualizzatore eventi per verificare l' **applicazione**, **securit**y e i registri eventi di **sistema** per verificare se sono presenti eventi che potrebbero indicare un problema.
    
    -   È anche possibile controllare SoftwareDistribution. log per verificare se sono presenti eventi che potrebbero indicare un problema.

- **Servizio ServicesSQL Web:** I servizi Web sono ospitati in IIS. Se non sono in esecuzione, assicurarsi che IIS sia in esecuzione (o avviato). È anche possibile provare a reimpostare il servizio Web digitando **iisreset** al prompt dei comandi.

- **Servizio SQL:** Ogni servizio, ad eccezione del servizio SelfUpdate, richiede che il servizio SQL sia in esecuzione. Se uno dei file di log indica problemi di connessione SQL, controllare innanzitutto il servizio SQL. Per accedere al servizio SQL, fare clic sul pulsante **Start**, scegliere **strumenti di amministrazione**, fare clic su **Servizi**, quindi cercare uno dei seguenti elementi:
    
  - **MSSQLSERver** (se si utilizza WMSDE o MSDE o se si utilizza SQL Server e si utilizza il nome dell'istanza predefinita per il nome dell'istanza)
    
  - **MSSQL $ WSUS** (se si usa un database di SQL Server e l'istanza di database è stata denominata WSUS)
    
    Fare clic con il pulsante destro del mouse sul servizio, quindi scegliere **Avvia** se il servizio non è in esecuzione oppure **Riavvia** per aggiornare il servizio se è in esecuzione.
