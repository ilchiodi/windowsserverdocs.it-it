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
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 64471ba81842fb91a7f6ef765e171949294102fa
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80314185"
---
# <a name="monitor-the-operations-status-of-the-remote-access-server-and-its-components"></a>Monitorare lo stato delle operazioni del server di accesso remoto e dei relativi componenti

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

**Nota:** Windows Server 2012 combina DirectAccess e Routing e accesso remoto (RRAS) in un unico ruolo Accesso remoto.  
  
È possibile utilizzare la console di gestione nel server di accesso remoto per monitorare lo stato delle operazioni.  
  
> [!NOTE]  
> Per completare l'attività descritta in questo argomento, è necessario aver eseguito l'accesso come membro del gruppo Domain Admins o di un membro del gruppo Administrators in ogni computer. Se è possibile completare un'attività mentre si è connessi con un account membro del gruppo Administrators, provare a eseguire l'attività mentre si è connessi con un account membro del gruppo Domain Admins.  
  
#### <a name="to-monitor-the-remote-access-server-operations-status"></a>Per monitorare lo stato delle operazioni del server di accesso remoto  
  
1.  In **Server Manager** fare clic su **Strumenti** e quindi su **Gestione Accesso remoto**.  
  
2.  Fare clic su **Dashboard** per passare a **report di accesso remoto** nella **console di gestione accesso remoto**.  
  
3.  Nel dashboard di monitoraggio, si noti il riquadro **stato operazioni** nel riquadro **stato server** . In questo riquadro sono elencati lo stato delle operazioni del server e lo stato di tutti i componenti del server.  
  
4.  Fare clic su **Aggiorna** in **attività** nel riquadro destro per ricaricare lo stato delle operazioni. Lo stato delle operazioni viene aggiornato automaticamente ogni cinque minuti, ovvero l'intervallo di aggiornamento predefinito. Per modificare l'intervallo di aggiornamento predefinito, fare clic su **Configura intervallo di aggiornamento**.  
  
![](../../../media/Monitor-the-operations-status-of-the-Remote-Access-server-and-its-components/PowerShellLogoSmall.gif)***<em>comandi equivalenti di Windows PowerShell</em> per Windows PowerShell***  
  
Tramite i cmdlet di Windows PowerShell seguenti viene eseguita la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se è possibile il ritorno a capo automatico in diverse righe a causa di limiti di formattazione.  
  
> [!NOTE]  
> Il comando per lo stato delle operazioni di un cluster è incluso come riferimento.  
  
```  
PS> Get-RemoteAccessHealth  
PS> Get-RemoteAccessHealth -Cluster  
```  
  


