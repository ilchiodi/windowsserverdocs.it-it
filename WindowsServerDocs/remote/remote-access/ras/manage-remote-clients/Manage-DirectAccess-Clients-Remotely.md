---
title: Gestire i client DirectAccess in remoto
description: Questo argomento fa parte della Guida di client DirectAccess di gestire in remoto in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 36255d80-a13e-4af7-a5c0-ab4c8f302622
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 736495ff5d1d5d3ba73fa86e6030606da92271de
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59832752"
---
# <a name="manage-directaccess-clients-remotely"></a>Gestire i client DirectAccess in remoto

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Il monitoraggio di Accesso remoto segnala l'attività dell'utente remoto e lo stato delle connessioni VPN e DirectAccess. Registra il numero e la durata delle connessioni client (tra le altre statistiche) e monitora lo stato delle operazioni del server. Si tratta di una console facile da usare che fornisce una visualizzazione dell'intera infrastruttura di Accesso remoto. Le visualizzazioni di monitoraggio sono disponibili per le configurazioni dei singoli server, dei cluster e multisito.  
  
**Nota:** Windows Server 2016 riunisce DirectAccess e servizio di accesso remoto (RAS) in un unico ruolo Accesso remoto.  
  
## <a name="in-this-guide"></a>Contenuto della guida  
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
  
-   **Connessioni per il traffico tunnel del computer**: Questo tunnel viene stabilito dal computer nel contesto di sistema per accedere ai server necessari per la risoluzione dei nomi, l'autenticazione, l'aggiornamento di monitoraggio e aggiornamento e così via.  
  
-   **Connessioni per il traffico tunnel dell'utente**: Questo tunnel viene stabilito dall'account utente nel computer in un contesto utente quando l'utente tenta di accedere a una risorsa nella rete aziendale. A seconda dei requisiti di distribuzione, possono essere richieste credenziali complesse (ad esempio, una smart card o una One-Time Password, OTP) per l'accesso alle risorse della rete aziendale.  
  
Per DirectAccess, una connessione viene identificata in modo univoco dall'indirizzo IP del client remoto. Ad esempio, se un tunnel del computer sia aperto per un computer client e un utente è connesso da tale computer, questi utilizzeranno la stessa connessione. In una situazione in cui l'utente si disconnette e si connette nuovamente mentre il tunnel del computer è ancora attivo, si tratta di una sola connessione.  
  

