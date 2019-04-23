---
title: Messaggi WSUS e suggerimenti per la risoluzione dei problemi
description: Argomento di Windows Server Update Service (WSUS) - risoluzione dei problemi mediante i messaggi di Windows Server Update Services
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9f6317f7-bfe0-42d9-87ce-d8f038c728ca
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cc24893b1a227501959002ea2d81c62813855d4a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883632"
---
# <a name="wsus-messages-and-troubleshooting-tips"></a>Messaggi WSUS e suggerimenti per la risoluzione dei problemi

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

In questo argomento contiene informazioni sui messaggi WSUS seguenti:

-   "Computer non ha segnalato lo stato"

-   "ID messaggio 6703 - sincronizzazione WSUS non riuscita"

-   "Errore 0x80070643: Errore irreversibile durante l'installazione"

-   "Alcuni servizi non vengono eseguiti. Controllare i servizi seguenti [...]"

## <a name="computer-has-not-reported-status"></a>Computer non ha segnalato lo stato
Quando un computer client WSUS non invia informazioni al server WSUS per indicare lo stato di aggiornamento corrente, viene generato questo messaggio nella console di WSUS. Questo problema è causato in genere dal computer client WSUS, non il server WSUS.

I motivi più comuni sono:

-   Il computer ha perso la connettività alla rete:
    -   Il cavo di rete è scollegato.
    -   Un cavo di rete corrispondente sia difettoso.
    -   Il computer ha una scheda di rete difettosi.
    -   La porta di rete a cui si connette il computer è stata disabilitata.
    -   La scheda di rete wireless è in grado di associare e connettersi al punto di accesso wireless aziendale.
-   Il computer è spento. (È stato arrestato oppure si trova in modalità sospensione o ibernazione.)

## <a name="message-id-6703---wsus-synchronization-failed"></a>Message ID 6703 - WSUS Synchronization Failed
> Messaggio: La richiesta non è riuscita con codice di stato HTTP 503: Servizio non disponibile.

> Origine: Microsoft.UpdateServices.Administration.AdminProxy.createUpdateServer.

Quando si tenta di aprire i servizi di aggiornamento nel server WSUS viene visualizzato l'errore seguente:

> Errore: Errore di connessione

> Errore durante il tentativo di connettersi al server WSUS. Questo errore può verificarsi per diversi motivi. Se il problema persiste, contattare l'amministratore di rete. Fare clic su Reimposta il nodo del Server per connettersi al server di nuovo.

Inoltre, tenta di accedere all'URL per il sito Web di amministrazione di WSUS (ad esempio, http://CM12CAS:8530) ha esito negativo con errore:

> Errore HTTP 503. Il servizio è disponibile

In questo caso, la causa più probabile è che il Pool di applicazioni WsusPool in IIS è in uno stato arrestato.

Inoltre, il limite memoria privata (KB) per il Pool di applicazioni è probabilmente impostata sul valore predefinito di 1843200 KB. Se si verifica questo problema, aumentare il limite di memoria privata a 4GB (4000000 KB) e riavviare il Pool di applicazioni. Per aumentare il limite di memoria privata, selezionare il Pool di applicazioni WsusPool e fare clic su impostazioni avanzate in modifica Pool di applicazioni. Quindi impostare il limite di memoria privata a 4GB (4000000 KB). Dopo il riavvio del Pool di applicazioni, monitorare lo stato attuale SMS_WSUS_SYNC_MANAGER del componente, wcm e Wsyncmgr. log per gli errori. Si noti che potrebbe essere necessario aumentare il limite di memoria privata a 8GB (8000000 KB) o versione successiva a seconda dell'ambiente.

Per altre informazioni, vedere: [WSUS-sincronizzazione in Configuration Manager 2012 ha esito negativo con errori HTTP 503](http://blogs.technet.com/b/sus/archive/2015/03/23/configmgr-2012-support-tip-wsus-sync-fails-with-http-503-errors.aspx)

## <a name="error-0x80070643-fatal-error-during-installation"></a>Errore 0x80070643: Errore irreversibile durante l'installazione
Il programma di installazione di WSUS utilizza Microsoft SQL Server per eseguire l'installazione. Questo problema si verifica perché l'utente che esegue l'installazione di Windows Server Update Services non dispone delle autorizzazioni di amministratore di sistema in SQL Server.

Per risolvere questo problema, concedere le autorizzazioni di amministratore di sistema a un account utente o a un account di gruppo in SQL Server e quindi eseguire nuovamente l'installazione di WSUS.

## <a name="some-services-are-not-running-check-the-following-services"></a>Alcuni servizi non sono in esecuzione. Controllare i servizi seguenti:

- **SelfUpdate:** Visualizzare [gli aggiornamenti automatici devono essere aggiornate](https://technet.microsoft.com/library/cc708554(v=ws.10).aspx) per informazioni sulla risoluzione dei problemi del servizio Selfupdate.

- **WSSUService.exe:** Questo servizio semplifica la sincronizzazione. Se hai problemi con la sincronizzazione, accedere a WSUSService.exe facendo **avviare**, scegliendo **strumenti di amministrazione**, fare clic su **Services**e individuando **Windows Server Update Services** nell'elenco dei servizi. Eseguire le operazioni seguenti:
    
    -   Verificare che il servizio sia in esecuzione. Fare clic su **avviare** se è stato arrestato o **riavviare** per aggiornare il servizio.
    
    -   Utilizzare il Visualizzatore eventi per controllare la **Application**, **Securit**y, e **sistema** i registri eventi per vedere se sono presenti eventi che potrebbero indicare un problema.
    
    -   È anche possibile controllare SoftwareDistribution per verificare se sono presenti eventi che potrebbero indicare un problema.

- **ServicesSQL Web del servizio:** Servizi Web sono ospitati in IIS. Se non sono in esecuzione, assicurarsi che IIS sia in esecuzione (avviato). È anche possibile reimpostare il servizio Web digitando **iisreset** un prompt dei comandi.

- **Servizio SQL:** Ogni servizio, ad eccezione del servizio selfupdate richiede che il servizio SQL è in esecuzione. Se uno qualsiasi dei file di log indicano problemi di connessione SQL, verificare innanzitutto il servizio SQL. Per accedere al servizio SQL, fare clic su **avviare**, scegliere **strumenti di amministrazione**, fare clic su **Services**, quindi cercare i possibili valori sono i seguenti:
    
    -   **MSSQLSERver** (se si usa WMSDE o MSDE, o se si usa SQL Server e Usa il nome dell'istanza predefinito per il nome dell'istanza)
    
    -   **MSSQL$ WSUS** (se si utilizza un database di SQL Server e ha assegnato un nome dell'istanza del database "Windows Server Update Services")
    
    Il pulsante destro del servizio e quindi fare clic su **avviare** se il servizio non è in esecuzione, o **riavviare** per aggiornare il servizio se è in esecuzione.
