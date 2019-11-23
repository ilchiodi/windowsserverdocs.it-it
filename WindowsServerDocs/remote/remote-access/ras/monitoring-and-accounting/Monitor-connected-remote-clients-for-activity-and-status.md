---
title: Monitorare i client remoti connessi relativamente ad attività e a stato
description: Questo argomento fa parte della Guida per il monitoraggio e l'accounting di accesso remoto in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: beb94475-b21f-46a9-ac51-bf2bb28ca94e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 03d87fb086a9f2797af8399be3d833b11bed79a5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71367262"
---
# <a name="monitor-connected-remote-clients-for-activity-and-status"></a>Monitorare i client remoti connessi relativamente ad attività e a stato

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

**Nota:** Windows Server 2012 combina DirectAccess e il servizio di accesso remoto (RAS) in un singolo ruolo accesso remoto.  
  
È possibile utilizzare la console di gestione nel server di accesso remoto per monitorare l'attività e lo stato dei client remoti.  
  
> [!NOTE]  
> È necessario essere connessi come membro del gruppo Domain Admins o un membro del gruppo Administrators su ciascun computer per completare le attività descritte in questo argomento. Se è possibile completare un'attività mentre si è connessi con un account membro del gruppo Administrators, provare a eseguire l'attività mentre si è connessi con un account membro del gruppo Domain Admins.  
  
#### <a name="to-monitor-remote-client-activity-and-status"></a>Per monitorare l'attività e lo stato del client remoto  
  
1.  In **Server Manager**, fare clic su **strumenti**, quindi fare clic su **Gestione accesso remoto**.  
  
2.  Fare clic su **REPORTING** per passare a **Remote Access Reporting** nel **Console Gestione accesso remoto**.  
  
3.  Fare clic su **stato client remoto** per passare all'interfaccia utente dell'attività e dello stato del client remoto nella **console di gestione accesso remoto**.  
  
4.  Verrà visualizzato l'elenco degli utenti connessi al server di accesso remoto e le relative statistiche dettagliate. Fare clic sulla prima riga nell'elenco corrispondente a un client. Quando si seleziona una riga, nel riquadro di anteprima viene visualizzata l'attività dell'utente remoto.  
  
![](../../../media/Monitor-connected-remote-clients-for-activity-and-status/PowerShellLogoSmall.gif)***<em>comandi equivalenti di Windows PowerShell</em> per Windows PowerShell***  
  
Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.  
  
```  
PS> Get-RemoteAccessConnectionStatistics  
```  
  
Le statistiche utente possono essere filtrate, in base alle selezioni dei criteri, usando i campi indicati nella tabella seguente.  
  
|Nome campo|Valore|  
|-------|-----|  
|Nome utente|Nome utente o alias dell'utente remoto. I caratteri jolly possono essere usati per selezionare un gruppo di utenti, ad esempio contoso\\* o \*\Administrator.|  
|Hostname|Nome dell'account computer del computer remoto. È possibile specificare anche un indirizzo IPv4 o IPv6.|  
|Tipo|DirectAccess o VPN. Se si seleziona DirectAccess, vengono elencati tutti gli utenti remoti connessi tramite DirectAccess. Se si seleziona VPN, vengono elencati tutti gli utenti remoti connessi tramite VPN.|  
|indirizzo ISP|Indirizzo IPv4 o IPv6 dell'utente remoto.|  
|Indirizzo IPv4|Indirizzo IPv4 interno del tunnel che connette l'utente remoto alla rete aziendale.|  
|Indirizzo IPv6|Indirizzo IPv6 interno del tunnel che connette l'utente remoto alla rete aziendale.|  
|Protocollo/Tunnel|Tecnologia di transizione usata dal client remoto. Si tratta di Teredo, 6to4 o IP-HTTPS per gli utenti DirectAccess e è PPTP, L2TP, SSTP o IKEv2 per gli utenti VPN.|  
|Risorsa a cui si effettua l'accesso|Tutti gli utenti che accedono a una determinata risorsa aziendale o a un endpoint. Il valore corrispondente a questo campo è il nome host/indirizzo IP del server.|  
|Server|Server di Accesso remoto a cui sono connessi i client. È rilevante soltanto per distribuzioni cluster e multisito.|  
  
  
  


