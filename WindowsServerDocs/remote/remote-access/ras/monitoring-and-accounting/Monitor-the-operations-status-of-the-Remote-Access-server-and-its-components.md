---
title: Monitorare lo stato delle operazioni del server di accesso remoto e dei relativi componenti
description: Questo argomento fa parte della Guida per il monitoraggio e l'accounting di accesso remoto in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 077a3a64-2fa3-4994-9711-ec1fbdc081ba
ms.author: pashort
author: shortpatti
ms.openlocfilehash: d0ad63ec88a428239a174a0217db94c44ab799bc
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404546"
---
# <a name="monitor-the-operations-status-of-the-remote-access-server-and-its-components"></a>Monitorare lo stato delle operazioni del server di accesso remoto e dei relativi componenti

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

**Nota:** Windows Server 2012 riunisce DirectAccess e il servizio Routing e Accesso remoto (RRAS) in un singolo ruolo Accesso remoto.  
  
È possibile utilizzare la console di gestione nel server di accesso remoto per monitorare lo stato delle operazioni.  
  
> [!NOTE]  
> Per completare l'attività descritta in questo argomento, è necessario aver eseguito l'accesso come membro del gruppo Domain Admins o di un membro del gruppo Administrators in ogni computer. Se è possibile completare un'attività mentre si è connessi con un account membro del gruppo Administrators, provare a eseguire l'attività mentre si è connessi con un account membro del gruppo Domain Admins.  
  
#### <a name="to-monitor-the-remote-access-server-operations-status"></a>Per monitorare lo stato delle operazioni del server di accesso remoto  
  
1.  In **Server Manager**, fare clic su **strumenti**, quindi fare clic su **Gestione accesso remoto**.  
  
2.  Fare clic su **Dashboard** per passare a **report di accesso remoto** nella **console di gestione accesso remoto**.  
  
3.  Nel dashboard di monitoraggio, si noti il riquadro **stato operazioni** nel riquadro **stato server** . In questo riquadro sono elencati lo stato delle operazioni del server e lo stato di tutti i componenti del server.  
  
4.  Fare clic su **Aggiorna** in **attività** nel riquadro destro per ricaricare lo stato delle operazioni. Lo stato delle operazioni viene aggiornato automaticamente ogni cinque minuti, ovvero l'intervallo di aggiornamento predefinito. Per modificare l'intervallo di aggiornamento predefinito, fare clic su **Configura intervallo di aggiornamento**.  
  
](../../../media/Monitor-the-operations-status-of-the-Remote-Access-server-and-its-components/PowerShellLogoSmall.gif)***<em>comandi equivalenti</em> di PowerShell per Windows PowerShell @no__t 0Windows***  
  
Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.  
  
> [!NOTE]  
> Il comando per lo stato delle operazioni di un cluster è incluso come riferimento.  
  
```  
PS> Get-RemoteAccessHealth  
PS> Get-RemoteAccessHealth -Cluster  
```  
  


