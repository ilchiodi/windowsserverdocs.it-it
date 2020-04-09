---
title: Generare un report di utilizzo per i client remoti usando i dati cronologici
description: Questo argomento fa parte della Guida per il monitoraggio e l'accounting di accesso remoto in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 0305467b-ce39-4532-a05a-2cc5ff946f55
ms.author: lizross
author: eross-msft
ms.openlocfilehash: ae862b596ff8c3d222c8f448f9b81b3c6bea05be
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860564"
---
# <a name="generate-a-usage-report-for-remote-clients-using-historical-data"></a>Generare un report di utilizzo per i client remoti usando i dati cronologici

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

**Nota:** Windows Server 2012 combina DirectAccess e Routing e accesso remoto (RRAS) in un unico ruolo Accesso remoto.  
  
La console di gestione nel server di accesso remoto consente di generare un report di utilizzo per i client remoti che accede al server. Per generare un report di utilizzo per i client remoti, è innanzitutto necessario abilitare accounting sul server di accesso remoto. Dopo aver generato il report, è possibile utilizzare il dashboard di monitoraggio che è disponibile nella console di gestione nel server di accesso remoto per visualizzare le statistiche di carico sul server.  
  
> [!NOTE]  
> È necessario essere connessi come membro del gruppo Domain Admins o un membro del gruppo Administrators su ciascun computer per completare le attività descritte in questo argomento. Se è possibile completare un'attività mentre si è connessi con un account membro del gruppo Administrators, provare a eseguire l'attività mentre si è connessi con un account membro del gruppo Domain Admins.  
  
#### <a name="to-enable-accounting-on-the-remote-access-server"></a>Per attivare l'accounting sul Server di accesso remoto  
  
1.  In **Server Manager** fare clic su **Strumenti** e quindi su **Gestione Accesso remoto**.  
  
2.  Fare clic su **REPORTING** per passare a **Remote Access Reporting** nel **Console Gestione accesso remoto**.  
  
3.  Fare clic su **configurare Accounting** nel **Remote Access Reporting** riquadro attività.  
  
4.  Selezionare il **utilizzare l'accounting della posta in arrivo** casella di controllo per attivare l'accounting sul server di accesso remoto.  
  
5.  Fare clic su **Applica** per abilitare la configurazione di accounting nel server e quindi fare clic su **Chiudi** dopo che il server è applicata la configurazione corretta.  
  
#### <a name="to-generate-the-usage-report"></a>Per generare il report di utilizzo  
  
1.  In **Server Manager** fare clic su **Strumenti** e quindi su **Gestione Accesso remoto**.  
  
2.  Fare clic su **REPORTING** per passare a **Remote Access Reporting** nel **Console Gestione accesso remoto**.  
  
3.  Nel riquadro centrale, fare clic su date nel calendario per selezionare il periodo di tempo **Data di inizio:** e **Data di fine:** , quindi fare clic su **Genera Report**.  
  
4.  Verrà visualizzato l'elenco di utenti connessi al server di accesso remoto all'interno di tempo selezionato e le statistiche dettagliate. Fare clic sulla prima riga nell'elenco. Quando si seleziona una riga, nel riquadro di anteprima viene visualizzata l'attività dell'utente remoto. Selezionare il **le statistiche di carico del Server** scheda nel riquadro di anteprima per visualizzare il carico cronologico nel server.  
  
    Fare clic su di **le statistiche di carico del Server** scheda nel riquadro di anteprima per visualizzare il carico cronologico nel server.  
  
> [!NOTE]  
> **Informazioni sulle sessioni**  
>   
> Accounting di accesso remoto si basa sul concetto di **sessioni**. A differenza di un **connessione**,  **sessione** è identificata da una combinazione di nome utente e all'indirizzo IP del client remoto. Ad esempio, se un tunnel del computer è costituito dal client remoto, denominato Client1, una sessione verrà creata e archiviata nel database di accounting. Quando si passa a un utente denominato User1 si connette da tale client dopo un certo tempo, ma il tunnel del computer è ancora attivo, la sessione viene registrata come una sessione separata. La distinzione di sessioni consiste nel mantenere la distinzione tra tunnel del computer e utente tunnel.  
  
![](../../../media/Generate-a-usage-report-for-remote-clients-using-historical-data/PowerShellLogoSmall.gif)***<em>comandi equivalenti di Windows PowerShell</em> per Windows PowerShell***  
  
Tramite i cmdlet di Windows PowerShell seguenti viene eseguita la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se è possibile il ritorno a capo automatico in diverse righe a causa di limiti di formattazione.  
  
Nello script seguente, modificare l'intervallo di date per cui si desidera un report nel **- StartDateTime** e **- EndDateTime** parametri.  
  
```  
PS> Get-RemoteAccessConnectionStatisticsSummary -StartDateTime "1 October 2010 00:00:00" -EndDateTime "14 October 2010 00:00:00"  
Shows server load statistics.  
PS> Get-RemoteAccessUserActivity -HostIPAddress 10.0.0.1 -StartDateTime "1 October 2010 00:00:00" -EndDateTime "14 October 2010 00:00:00"  
```  
  


