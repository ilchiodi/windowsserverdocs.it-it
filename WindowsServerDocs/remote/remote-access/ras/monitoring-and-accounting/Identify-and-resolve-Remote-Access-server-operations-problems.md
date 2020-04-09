---
title: Identificare e risolvere i problemi di operazioni del server di accesso remoto
description: Questo argomento fa parte della Guida per il monitoraggio e l'accounting di accesso remoto in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 7ce84c9f-fd1f-4463-8fc7-d2f33344a2c9
ms.author: lizross
author: eross-msft
ms.openlocfilehash: ae48f9f59bcc297c9bcc2b65a4d094f6d0279359
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80860554"
---
# <a name="identify-and-resolve-remote-access-server-operations-problems"></a>Identificare e risolvere i problemi di operazioni del server di accesso remoto

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

**Nota:** Windows Server 2012 combina DirectAccess e Routing e accesso remoto (RRAS) in un unico ruolo Accesso remoto.  
  
È possibile utilizzare le procedure seguenti per identificare i problemi operativi del server Accesso remoto, le relative cause e la risoluzione necessaria per correggere i problemi.  
  
> [!NOTE]  
> È necessario essere connessi come membro del gruppo Domain Admins o un membro del gruppo Administrators su ciascun computer per completare le attività descritte in questo argomento. Se è possibile completare un'attività mentre si è connessi con un account membro del gruppo Administrators, provare a eseguire l'attività mentre si è connessi con un account membro del gruppo Domain Admins.  
  
In questo argomento sono incluse informazioni sull'esecuzione delle operazioni seguenti:  
  
- Simulare un problema di operazioni  
  
- Identificare il problema di operazioni e intraprendere l'azione correttiva  
  
- Ripristinare il servizio Helper IP  
  
### <a name="simulate-an-operations-issue"></a><a name="BKMK_Simulate"></a>Simulare un problema di operazioni  
  
> [!CAUTION]  
> Poiché l'accesso remoto server è configurato correttamente e non si sono verificati eventuali problemi, è possibile utilizzare la procedura seguente per simulare un problema di operazioni. Se il server è attualmente utilizzato da client in un ambiente di produzione, è possibile evitare di eseguire le azioni in questo momento. Piuttosto, è possibile leggere i passaggi per informazioni su come risolvere i problemi che potrebbero verificarsi nel server di accesso remoto in futuro.  
  
L'Helper IP (IPHlpSvc) host IPv6 transizione tecnologie del servizio (ad esempio, IP-HTTPS, 6to4 o Teredo) ed è necessario per il server DirectAccess funzionare correttamente. Per illustrare un problema di operazioni simulato sul server di accesso remoto, è necessario arrestare il servizio di rete (IPHlpSvc).  
  
##### <a name="to-stop-the-ip-helper-service"></a>Per arrestare il servizio Helper IP  
  
1.  Nel **avviare** schermata del server di accesso remoto, fare clic su **Strumenti di amministrazione**, quindi fare doppio clic su **servizi**.  
  
2.  Nell'elenco dei **servizi**, scorrere verso il basso e fare doppio clic su **Helper IP**, quindi fare clic su **arrestare**.  
  
### <a name="identify-the-operations-issue-and-take-corrective-action"></a><a name="BKMK_Identify"></a>Identificare il problema delle operazioni e intraprendere un'azione correttiva  
La disattivazione del servizio Helper IP causerà un errore grave nel server di accesso remoto. Il dashboard di monitoraggio viene visualizzato lo stato di operazioni del server e i dettagli del problema.  
  
##### <a name="to-identify-the-details-and-take-corrective-action"></a>Per identificare i dettagli e azioni correttive  
  
1.  In **Server Manager** fare clic su **Strumenti** e quindi su **Gestione Accesso remoto**.  
  
2.  Fare clic su **DASHBOARD** per passare a **Dashboard di Accesso remoto** nella **console di gestione di Accesso remoto**.  
  
3.  Assicurarsi che il server di accesso remoto sia selezionato nel riquadro a sinistra e quindi nel riquadro centrale, fare clic su **stato operazioni**.  
  
4.  Verrà visualizzato l'elenco dei componenti con icone di colore rosse o verde, che indicano lo stato operativo. Fare clic sui **IP-HTTPS** riga nell'elenco. Quando si seleziona una riga, in vengono visualizzati i dettagli per l'operazione di **Dettagli** riquadro come indicato di seguito:  
  
    **Errore**  
  
    Il servizio Helper IP (IPHlpSvc) è stato arrestato. DirectAccess potrebbe non funzionare come previsto. Il servizio Helper IP fornisce connettività tunnel utilizzando la piattaforma di connettività, tecnologie di transizione IPv6 e IP-HTTPS.  
  
    **Cause**  
  
    1.  Il servizio Helper IP è stato arrestato.  
  
    2.  Il servizio Helper IP non risponde.  
  
    **Soluzione**  
  
    1.  Per assicurarsi che il servizio sia in esecuzione, digitare **Get-Service iphlpsvc** al prompt di Windows PowerShell.  
  
    2.  Per abilitare il servizio, digitare **Start-Service iphlpsvc** da un prompt di Windows PowerShell con privilegi elevato.  
  
    3.  Per riavviare il servizio, digitare **Restart-Service iphlpsvc** da un prompt di Windows PowerShell con privilegi elevato.  
  
### <a name="restore-the-ip-helper-service"></a><a name="BKMK_Restart"></a>Ripristinare il servizio helper IP  
Per ripristinare il servizio Helper IP sul server di accesso remoto, è possibile seguire la procedura di risoluzione precedente per avviare o riavviare il servizio, oppure è possibile utilizzare la procedura seguente per annullare la procedura utilizzata per simulare l'errore del servizio Helper IP.  
  
##### <a name="to-restart-the-ip-helper-service-on-the-remote-access-server"></a>Per riavviare il servizio Helper IP nel server di accesso remoto  
  
1.  Nel **avviare** schermata, fare clic su **Strumenti di amministrazione**, e quindi fare doppio clic su **servizi**.  
  
2.  Nell'elenco dei **servizi**, scorrere verso il basso e fare doppio clic su **Helper IP**, quindi fare clic su **avviare**.  
  
![](../../../media/Identify-and-resolve-Remote-Access-server-operations-problems/PowerShellLogoSmall.gif)***<em>comandi equivalenti di Windows PowerShell</em> per Windows PowerShell***  
  
Tramite i cmdlet di Windows PowerShell seguenti viene eseguita la stessa funzione della procedura precedente. Immettere ogni cmdlet in una singola riga, anche se è possibile il ritorno a capo automatico in diverse righe a causa di limiti di formattazione.  
  
```PowerShell
PS> Get-RemoteAccessHealth | Where-Object {$_.Component -eq "IP-HTTPS"} | Format-List -Property *  
```
