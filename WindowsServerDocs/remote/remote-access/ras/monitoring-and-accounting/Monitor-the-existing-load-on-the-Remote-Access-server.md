---
title: Monitorare il carico esistente nel server di accesso remoto
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
ms.assetid: 62fa2895-62ae-42cf-817c-53e06ac2a26c
ms.author: pashort
author: shortpatti
ms.openlocfilehash: a1f47273ab3be6faa762df2fb90d6486bc0ed2d5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59849022"
---
# <a name="monitor-the-existing-load-on-the-remote-access-server"></a>Monitorare il carico esistente nel server di accesso remoto

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

**Nota:** Windows Server 2012 riunisce DirectAccess e il servizio Routing e Accesso remoto (RRAS) in un singolo ruolo Accesso remoto.  
  
Il termine **carico** fa riferimento alle statistiche che riguardano il numero di connessioni nel server di accesso remoto. Di seguito sono i passaggi necessari per rilevare il carico sul server di accesso remoto.  
  
È possibile usare il dashboard di monitoraggio che è disponibile nella console di gestione nel server di accesso remoto per visualizzare le statistiche di carico per il server oppure è possibile usare contatori di Performance Monitor per tenere traccia delle statistiche.  
  
> [!NOTE]  
> È necessario essere connessi come membro del gruppo Domain Admins o un membro del gruppo Administrators su ciascun computer per completare le attività descritte in questo argomento. Se è possibile completare un'attività mentre si è connessi con un account membro del gruppo Administrators, provare a eseguire l'attività mentre si è connessi con un account membro del gruppo Domain Admins.  
  
#### <a name="to-use-the-monitoring-dashboard-to-monitor-the-remote-access-server-load"></a>Usare il dashboard di monitoraggio per monitorare il carico del server Accesso remoto  
  
1.  In **Server Manager**, fare clic su **strumenti**, quindi fare clic su **Gestione accesso remoto**.  
  
2.  Fare clic su **DASHBOARD** per passare a **Dashboard di Accesso remoto** nella **console di gestione di Accesso remoto**.  
  
3.  Nel dashboard di monitoraggio, si noti che il **stato Client remoto** riquadro all'interno di **lo stato del Server** riquadro. Questo riquadro sono elencate le statistiche, ad esempio il numero totale di client remoti connessi, il numero totale di client DirectAccess connessi e il numero massimo di utenti connessi nelle ultime 24 ore.  
  
4.  È possibile fare clic su **Refresh** sotto **attività** nel riquadro di destra per ricaricare lo stato di integrità. Per modificare l'intervallo di aggiornamento, fare clic su **configurare l'intervallo di aggiornamento** sotto **attività**.  
  
#### <a name="to-use-the-performance-monitor-tool-to-monitor-performance-counters-on-the-remote-access-server"></a>Usare lo strumento Performance Monitor per monitorare i contatori delle prestazioni nel server di accesso remoto  
  
1.  Fare clic su **avviare**, fare clic su **strumenti di amministrazione**, quindi fare doppio clic su **Performance Monitor**.  
  
2.  Sotto **Performance**, fare clic su **Performance Monitor**.  
  
3.  Fare clic sui **Add** pulsante (indicato da un'icona verde a forma) nella **Performance Monitor** sulla barra degli strumenti.  
  
4.  Nell'elenco delle **contatori disponibili**, selezionare tutti i contatori nel **RAS** e **RAmgmtsvc** categorie e quindi fare clic su **Aggiungi >>**.  
  
5.  Anche in questo caso dall'elenco degli **contatori disponibili**, selezionare tutti i contatori nel **connessioni IPsec** categoria e quindi fare clic su **Aggiungi >>.**  
  
6.  Fare clic su **OK** per aggiungere i contatori selezionati nel **Performance Monitor** console per il rilevamento.  
  
**Monitoraggio prestazioni** graficamente mostrerà ora le statistiche di carico del server selezionato.  
  
![Windows PowerShell](../../../media/Monitor-the-existing-load-on-the-Remote-Access-server/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *  
  
Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.  
  
```  
PS> Get-RemoteAccessConnectionStatisticsSummary  
```  
  


