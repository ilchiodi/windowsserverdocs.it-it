---
title: Generare un report di utilizzo per i client remoti usando i dati cronologici
description: Questo argomento fa parte della Guida di monitoraggio di accesso remoto e l'Accounting in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology:
- networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0305467b-ce39-4532-a05a-2cc5ff946f55
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bb122990c1baf1db8a2edbbbecba8b8cdf8a264d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59833762"
---
# <a name="generate-a-usage-report-for-remote-clients-using-historical-data"></a>Generare un report di utilizzo per i client remoti usando i dati cronologici

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

**Nota:** Windows Server 2012 riunisce DirectAccess e il servizio Routing e Accesso remoto (RRAS) in un singolo ruolo Accesso remoto.  
  
La console di gestione nel server di accesso remoto consente di generare un report di utilizzo per i client remoti che accede al server. Per generare un report di utilizzo per i client remoti, è innanzitutto necessario abilitare accounting sul server di accesso remoto. Dopo aver generato il report, è possibile utilizzare il dashboard di monitoraggio che è disponibile nella console di gestione nel server di accesso remoto per visualizzare le statistiche di carico sul server.  
  
> [!NOTE]  
> È necessario essere connessi come membro del gruppo Domain Admins o un membro del gruppo Administrators su ciascun computer per completare le attività descritte in questo argomento. Se è possibile completare un'attività mentre si è connessi con un account membro del gruppo Administrators, provare a eseguire l'attività mentre si è connessi con un account membro del gruppo Domain Admins.  
  
#### <a name="to-enable-accounting-on-the-remote-access-server"></a>Per attivare l'accounting sul Server di accesso remoto  
  
1.  In **Server Manager**, fare clic su **strumenti**, quindi fare clic su **Gestione accesso remoto**.  
  
2.  Fare clic su **REPORTING** per passare a **Remote Access Reporting** nel **Console Gestione accesso remoto**.  
  
3.  Fare clic su **configurare Accounting** nel **Remote Access Reporting** riquadro attività.  
  
4.  Selezionare il **utilizzare l'accounting della posta in arrivo** casella di controllo per attivare l'accounting sul server di accesso remoto.  
  
5.  Fare clic su **Applica** per abilitare la configurazione di accounting nel server e quindi fare clic su **Chiudi** dopo che il server è applicata la configurazione corretta.  
  
#### <a name="to-generate-the-usage-report"></a>Per generare il report di utilizzo  
  
1.  In **Server Manager**, fare clic su **strumenti**, quindi fare clic su **Gestione accesso remoto**.  
  
2.  Fare clic su **REPORTING** per passare a **Remote Access Reporting** nel **Console Gestione accesso remoto**.  
  
3.  Nel riquadro centrale, fare clic su date nel calendario per selezionare il periodo di tempo **Data di inizio:** e **Data di fine:**, quindi fare clic su **Genera Report**.  
  
4.  Verrà visualizzato l'elenco di utenti connessi al server di accesso remoto all'interno di tempo selezionato e le statistiche dettagliate. Fare clic sulla prima riga nell'elenco. Quando si seleziona una riga, nel riquadro di anteprima viene visualizzata l'attività dell'utente remoto. Selezionare il **le statistiche di carico del Server** scheda nel riquadro di anteprima per visualizzare il carico cronologico nel server.  
  
    Fare clic su di **le statistiche di carico del Server** scheda nel riquadro di anteprima per visualizzare il carico cronologico nel server.  
  
> [!NOTE]  
> **Informazioni sulle sessioni**  
>   
> Accounting di accesso remoto si basa sul concetto di **sessioni**. A differenza di un **connessione**,  **sessione** è identificata da una combinazione di nome utente e all'indirizzo IP del client remoto. Ad esempio, se un tunnel del computer è costituito dal client remoto, denominato Client1, una sessione verrà creata e archiviata nel database di accounting. Quando si passa a un utente denominato User1 si connette da tale client dopo un certo tempo, ma il tunnel del computer è ancora attivo, la sessione viene registrata come una sessione separata. La distinzione di sessioni consiste nel mantenere la distinzione tra tunnel del computer e utente tunnel.  
  
![Windows PowerShell](../../../media/Generate-a-usage-report-for-remote-clients-using-historical-data/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *  
  
Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.  
  
Nello script seguente, modificare l'intervallo di date per cui si desidera un report nel **- StartDateTime** e **- EndDateTime** parametri.  
  
```  
PS> Get-RemoteAccessConnectionStatisticsSummary -StartDateTime "1 October 2010 00:00:00" -EndDateTime "14 October 2010 00:00:00"  
Shows server load statistics.  
PS> Get-RemoteAccessUserActivity -HostIPAddress 10.0.0.1 -StartDateTime "1 October 2010 00:00:00" -EndDateTime "14 October 2010 00:00:00"  
```  
  


