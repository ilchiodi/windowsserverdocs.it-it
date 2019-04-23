---
title: Monitorare i client remoti connessi relativamente ad attività e a stato
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
ms.assetid: beb94475-b21f-46a9-ac51-bf2bb28ca94e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 468854d6e03bbc2abee3b9fce7f7960c7c25cfd3
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879712"
---
# <a name="monitor-connected-remote-clients-for-activity-and-status"></a>Monitorare i client remoti connessi relativamente ad attività e a stato

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

**Nota:** Windows Server 2012 combina DirectAccess e servizio di accesso remoto (RAS) in un unico ruolo Accesso remoto.  
  
È possibile utilizzare la console di gestione nel server di accesso remoto per monitorare lo stato e attività del client remoto.  
  
> [!NOTE]  
> È necessario essere connessi come membro del gruppo Domain Admins o un membro del gruppo Administrators su ciascun computer per completare le attività descritte in questo argomento. Se è possibile completare un'attività mentre si è connessi con un account membro del gruppo Administrators, provare a eseguire l'attività mentre si è connessi con un account membro del gruppo Domain Admins.  
  
#### <a name="to-monitor-remote-client-activity-and-status"></a>Per monitorare lo stato e attività del client remoto  
  
1.  In **Server Manager**, fare clic su **strumenti**, quindi fare clic su **Gestione accesso remoto**.  
  
2.  Fare clic su **REPORTING** per passare a **Remote Access Reporting** nel **Console Gestione accesso remoto**.  
  
3.  Fare clic su **stato Client remoto** per passare al client remoto attività e stato di interfaccia utente nel **Console Gestione accesso remoto**.  
  
4.  Verrà visualizzato l'elenco degli utenti vengono connessi al server di accesso remoto e dettagliate le statistiche. Fare clic sulla prima riga nell'elenco che corrisponde a un client. Quando si seleziona una riga, nel riquadro di anteprima viene visualizzata l'attività dell'utente remoto.  
  
![Windows PowerShell](../../../media/Monitor-connected-remote-clients-for-activity-and-status/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *  
  
Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.  
  
```  
PS> Get-RemoteAccessConnectionStatistics  
```  
  
Le statistiche di utente possono essere filtrate in base a criteri selezionati, usando i campi nella tabella seguente.  
  
|Nome campo|Value|  
|-------|-----|  
|Nome utente|Nome utente o alias dell'utente remoto. Caratteri jolly possono essere utilizzati per selezionare un gruppo di utenti, ad esempio contoso\\* o \*\administrator.|  
|Hostname|Nome dell'account computer del computer remoto. Un indirizzo IPv4 o IPv6 inoltre può essere specificato.|  
|Tipo|DirectAccess o VPN. Se si seleziona DirectAccess, vengono elencati tutti gli utenti remoti che sono connessi tramite DirectAccess. Se si seleziona VPN, vengono elencati tutti gli utenti remoti che sono connessi tramite VPN.|  
|indirizzo ISP|Indirizzo IPv4 o IPv6 dell'utente remoto.|  
|Indirizzo IPv4|Indirizzo IPv4 interno del tunnel che connettono l'utente remoto alla rete aziendale.|  
|Indirizzo IPv6|Indirizzo IPv6 interno del tunnel che connette l'utente remoto alla rete aziendale.|  
|Protocollo/Tunnel|Tecnologia di transizione utilizzata dal client remoto. Ciò è Teredo, 6to4 o IP-HTTPS per gli utenti di DirectAccess e PPTP, L2TP, SSTP o IKEv2 è per gli utenti VPN.|  
|Risorsa a cui si effettua l'accesso|Tutti gli utenti che accedono a una determinata risorsa aziendale o a un endpoint. Il valore corrispondente a questo campo è il nome host/indirizzo IP del server.|  
|Server|Server di Accesso remoto a cui sono connessi i client. È rilevante soltanto per distribuzioni cluster e multisito.|  
  
  
  


