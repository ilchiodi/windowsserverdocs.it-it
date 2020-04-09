---
title: Usare il monitoraggio e l'accounting di accesso remoto
description: Questo argomento fa parte della Guida per il monitoraggio e l'accounting di accesso remoto in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 92519b49-0df4-43c1-9717-f13570644212
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 03d367f2f981ca3ed649f1ca4d5eca23967c9cfc
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860514"
---
# <a name="use-remote-access-monitoring-and-accounting"></a>Usare il monitoraggio e l'accounting di accesso remoto

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Il monitoraggio di Accesso remoto segnala l'attività dell'utente remoto e lo stato delle connessioni VPN e DirectAccess. Registra il numero e la durata delle connessioni client (tra le altre statistiche) e monitora lo stato delle operazioni del server. Si tratta di una console facile da usare che fornisce una visualizzazione dell'intera infrastruttura di Accesso remoto. Le visualizzazioni di monitoraggio sono disponibili per le configurazioni dei singoli server, dei cluster e multisito.  
  
**Nota:** Windows Server 2012 combina DirectAccess e Routing e accesso remoto (RRAS) in un unico ruolo Accesso remoto.  
  
> [!NOTE]  
> Oltre a questo argomento, sono disponibili i seguenti argomenti sul monitoraggio di Accesso remoto.  
>   
> -   [Monitorare il carico esistente nel server di accesso remoto](Monitor-the-existing-load-on-the-Remote-Access-server.md)  
> -   [Monitorare lo stato di distribuzione della configurazione del server di accesso remoto](Monitor-the-configuration-distribution-status-of-the-Remote-Access-server.md)  
> -   [Monitorare lo stato delle operazioni del server di accesso remoto e dei relativi componenti](Monitor-the-operations-status-of-the-Remote-Access-server-and-its-components.md)  
> -   [Identificare e risolvere i problemi di operazioni del server di accesso remoto](Identify-and-resolve-Remote-Access-server-operations-problems.md)  
> -   [Monitorare i client remoti connessi relativamente all'attività e allo stato](Monitor-connected-remote-clients-for-activity-and-status.md)  
> -   [Generare un report di utilizzo per i client remoti usando i dati cronologici](Generate-a-usage-report-for-remote-clients-using-historical-data.md)  

## <a name="in-this-guide"></a>In questa guida  
Questo documento contiene le istruzioni su come usare le capacità di monitoraggio di Accesso remoto con la console di gestione DirectAccess e il corrispondente cmdlet di Windows PowerShell, forniti insieme al ruolo server di Accesso remoto.  
  
Vengono illustrati i seguenti scenari di monitoraggio e accounting:  
  
1.  Monitorare il carico esistente nel server di accesso remoto  
  
2.  Monitorare lo stato di distribuzione della configurazione del server di accesso remoto  
  
3.  Monitorare lo stato delle operazioni del server di accesso remoto e dei relativi componenti  
  
4.  Identificare e risolvere i problemi operativi del server di Accesso remoto  
  
5.  Monitorare i client remoti connessi relativamente ad attività e a stato  
  
6.  Generare un rapporto sull'utilizzo per i client remoti usando i dati cronologici  
  
### <a name="understand-monitoring-and-accounting"></a>Informazioni su monitoraggio e accounting  
Prima di iniziare le attività di monitoraggio e accounting per i client remoti, è necessario comprendere la differenza tra le due operazioni.  
  
-   Il **monitoraggio** visualizza gli utenti attivi connessi in un determinato momento.  
  
-   L'**accounting** mantiene una cronologia degli utenti connessi alla rete aziendale e i dettagli sull'uso (per operazioni di conformità e controllo).  
  
Il monitoraggio dei client remoti si basa sulle connessioni. Esistono due tipi di connessioni tunnel stabilite dai client DirectAccess:  
  
-   **Connessioni al traffico del tunneling**del computer: questo tunnel viene stabilito dal computer nel contesto di sistema per accedere ai server necessari per la risoluzione dei nomi, l'autenticazione, l'aggiornamento della correzione e così via.  
  
-   **Connessioni al traffico del tunnel utente**: questo tunnel viene stabilito dall'account utente nel computer, in un contesto utente, quando l'utente tenta di accedere a una risorsa nella rete aziendale. A seconda dei requisiti di distribuzione, possono essere richieste credenziali complesse (ad esempio, una smart card o una One-Time Password, OTP) per l'accesso alle risorse della rete aziendale.  
  
Per DirectAccess, una connessione viene identificata in modo univoco dall'indirizzo IP del client remoto. Ad esempio, se un tunnel del computer è aperto per un computer client e un utente è connesso da tale computer, utilizzerà la stessa connessione. In una situazione in cui l'utente si disconnette e si connette nuovamente mentre il tunnel del computer è ancora attivo, si tratta di una sola connessione.  
  


