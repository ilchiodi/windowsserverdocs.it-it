---
title: Monitorare lo stato delle operazioni del server di accesso remoto e dei relativi componenti
description: Questo argomento fa parte della Guida di monitoraggio di accesso remoto e l'Accounting in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: networking-ras
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 077a3a64-2fa3-4994-9711-ec1fbdc081ba
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 9ccfd0cd65a3504349dcad3bd7a549ed18eb6279
ms.sourcegitcommit: afb0602767de64a76aaf9ce6a60d2f0e78efb78b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/20/2019
ms.locfileid: "67281147"
---
# <a name="monitor-the-operations-status-of-the-remote-access-server-and-its-components"></a>Monitorare lo stato delle operazioni del server di accesso remoto e dei relativi componenti

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

**Nota:** Windows Server 2012 riunisce DirectAccess e il servizio Routing e Accesso remoto (RRAS) in un singolo ruolo Accesso remoto.  
  
Console di gestione nel server di accesso remoto è utilizzabile per monitorare lo stato di operazioni.  
  
> [!NOTE]  
> È necessario essere connessi come membro del gruppo Domain Admins o un membro del gruppo Administrators in ogni computer per completare le attività descritte in questo argomento. Se è possibile completare un'attività mentre si è connessi con un account membro del gruppo Administrators, provare a eseguire l'attività mentre si è connessi con un account membro del gruppo Domain Admins.  
  
#### <a name="to-monitor-the-remote-access-server-operations-status"></a>Per monitorare lo stato di operazioni del server Accesso remoto  
  
1.  In **Server Manager**, fare clic su **strumenti**, quindi fare clic su **Gestione accesso remoto**.  
  
2.  Fare clic su **DASHBOARD** a cui passare **Remote Access Reporting** nel **Console Gestione accesso remoto**.  
  
3.  Nel dashboard di monitoraggio, si noti che il **stato operazioni** riquadro all'interno di **lo stato del Server** riquadro. Questo riquadro elenca lo stato di operazioni server e lo stato dei componenti del server.  
  
4.  Fare clic su **Refresh** sotto **attività** nel riquadro di destra per ricaricare lo stato di operazioni. Lo stato di operazioni viene automaticamente aggiornato ogni cinque minuti, ovvero l'intervallo di aggiornamento predefinito. Per modificare l'intervallo di aggiornamento, fare clic su **configurare l'intervallo di aggiornamento**.  
  
![Windows PowerShell](../../../media/Monitor-the-operations-status-of-the-Remote-Access-server-and-its-components/PowerShellLogoSmall.gif)***<em>comandi equivalenti di Windows PowerShell</em>***  
  
Il cmdlet o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se qui può sembrare che siano divisi su più righe a causa di vincoli di formattazione.  
  
> [!NOTE]  
> Il comando per lo stato di operazioni di un cluster è incluso come riferimento.  
  
```  
PS> Get-RemoteAccessHealth  
PS> Get-RemoteAccessHealth -Cluster  
```  
  


