---
title: Monitorare lo stato di distribuzione della configurazione del server di accesso remoto
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
ms.assetid: de285d13-9e54-4c46-88f0-607182e5e3dc
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3171eb9e9c90d0688fa413b80d9dbbf162e77fe8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59818422"
---
# <a name="monitor-the-configuration-distribution-status-of-the-remote-access-server"></a>Monitorare lo stato di distribuzione della configurazione del server di accesso remoto

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

**Nota:** Windows Server 2012 combina DirectAccess e servizio di accesso remoto (RAS) in un unico ruolo Accesso remoto.  
  
La console di gestione di Accesso remoto confronta le versioni di configurazione di tutti i server monitorati per verificare che corrispondano e usino la versione di configurazione più recente. In tal modo è possibile verificare se la versione di configurazione più recente (specificata negli oggetti Criteri di gruppo) è stata distribuita e applicata correttamente a tutti i server.  
  
### <a name="to-use-the-monitoring-dashboard-to-monitor-the-configuration-distribution"></a>Per usare il dashboard di monitoraggio per monitorare la distribuzione della configurazione  
  
1.  In **Server Manager**, fare clic su **strumenti**, quindi fare clic su **Gestione accesso remoto**.  
  
2.  Fare clic su **DASHBOARD** per passare a **Dashboard di Accesso remoto** nella **console di gestione di Accesso remoto**.  
  
3.  Sul dashboard di monitoraggio osservare il riquadro **Stato configurazione** al centro in alto, in cui è visualizzato lo stato corrente della distribuzione della configurazione.  
  
La tabella seguente contiene i messaggi visualizzati nel riquadro **Stato configurazione**, i relativi significati e le eventuali operazioni amministrative necessarie.  
  
|||||  
|-|-|-|-|  
|Gravità|Messaggio|Significato|Azioni necessarie|  
|Riuscito|Distribuzione della configurazione completata.|La configurazione nell'oggetto Criteri di gruppo è stata applicata correttamente al server.|Nessuna azione necessaria.|  
|Avviso|Configurazione per il server [*nome server*] non recuperata dal controller di dominio. L'oggetto Criteri di gruppo non è collegato.|La configurazione nell'oggetto Criteri di gruppo non ha ancora raggiunto il server. Il motivo potrebbe essere il mancato collegamento dell'oggetto Criteri di gruppo al server.|Collegare l'oggetto Criteri di gruppo a un ambito di gestione applicato al server oppure, in uno scenario in cui l'oggetto Criteri di gruppo è in gestione temporanea, esportare manualmente le impostazioni da quest'ultimo e importarle nell'oggetto Criteri di gruppo di produzione. Per altre informazioni su oggetti Criteri di gruppo di gestione temporanea, vedere **la gestione di GPO di accesso remoto con autorizzazioni limitate** nelle [Step-1-Plan-the-DirectAccess-Infrastructure](../../directaccess/single-server-advanced/Step-1-Plan-the-DirectAccess-Infrastructure.md). Per i passaggi di gestione temporanea oggetto Criteri di gruppo, vedere **configurazione di GPO di accesso remoto con autorizzazioni limitate** in [passaggio 1: Configurare l'infrastruttura DirectAccess](../../directaccess/single-server-advanced/Step-1-Configuring-DirectAccess-Infrastructure.md).|  
|Avviso|Configurazione per il server [*nome server*] non ancora recuperata dal controller di dominio.|La configurazione nell'oggetto Criteri di gruppo non ha ancora raggiunto il server.<br /><br />La propagazione di una nuova configurazione potrebbe richiedere fino a 10 minuti.|Per l'aggiornamento dei criteri sul server prevedere un intervallo più lungo.|  
|Errore|Non è possibile recuperare la configurazione per il server [*nome server*] dal controller di dominio.|La configurazione nell'oggetto Criteri di gruppo non ha raggiunto il server e sono trascorsi oltre 10 minuti dalla modifica della configurazione.|Ciò potrebbe verificarsi in uno degli scenari seguenti:<br /><br />-Il server non è connesso al dominio di aggiornamento dei criteri. È possibile eseguire "gpupdate /force" sul server per forzare un aggiornamento di criteri.<br />-Replica oggetto Criteri di gruppo potrebbe essere necessario recuperare la configurazione aggiornata.<br />-Non vi è alcun controller di dominio scrivibile nel sito di Active Directory del server di accesso remoto.<br /><br />Attendere la replica degli oggetti Criteri di gruppo su tutti i controller di dominio e quindi usare il cmdlet **Set-DAEntryPointDC** di Windows PowerShell per associare il punto di ingresso a un controller di dominio scrivibile in Active Directory nel server di Accesso remoto.|  
|Avviso|Configurazione per il server [*nome server*] recuperata dal controller di dominio, ma non ancora applicata.|La configurazione nell'oggetto Criteri di gruppo ha raggiunto il server, ma non è stata ancora applicata.<br /><br />L'applicazione della configurazione potrebbe richiedere fino a 15 minuti.|Prevedere maggior tempo per la completa applicazione della configurazione al server.|  
|Errore|Non è possibile applicare la configurazione per il server [*nome server*] recuperata dal controller di dominio.|La configurazione nell'oggetto Criteri di gruppo ha raggiunto il server, ma non è stata applicata correttamente e sono trascorsi oltre 15 minuti dalla modifica della configurazione.|Ciò potrebbe verificarsi in uno degli scenari seguenti:<br /><br />1.  La configurazione è attualmente in corso di applicazione. L'errore visualizzato può essere dovuto al lungo periodo di tempo impiegato per recuperare la configurazione dall'oggetto Criteri di gruppo.<br />    Per verificare se questo è il motivo, usare l'**Utilità di pianificazione** e passare a Microsoft\Windows\RemoteAccess per accertarsi che **RAConfigTask** sia attualmente in esecuzione.<br />2.  Se **RAConfigTask** non è in esecuzione, la configurazione potrebbe non essere stata applicata al server.<br />    Controllare gli errori nel **Visualizzatore eventi** nel canale delle operazioni del server di Accesso remoto, che si trova al percorso \Registri applicazioni e servizi\Microsoft\Windows\RemoteAccess-RemoteAccessServer.<br />    Verificare l'eventuale presenza di errori in **STATO OPERAZIONI** nella console di gestione Accesso remoto. Per altre informazioni, vedere [Monitorare lo stato operazioni del server di Accesso remoto e dei relativi componenti](Monitor-the-operations-status-of-the-Remote-Access-server-and-its-components.md).|  
|Errore|Configurazione per server multisito recuperata dal controller di dominio. La configurazione non corrisponde su tutti i server.|Vi è un'incoerenza tra le versioni di configurazione degli oggetti Criteri di gruppo del server nella distribuzione multisito.<br /><br />Tutti gli oggetti Criteri di gruppo del server per tutti i punti di ingresso dovrebbero avere la stessa configurazione globale, ma per qualche motivo non sono sincronizzati.|Ciò può accadere quando la modifica della configurazione ha esito negativo e il rollback non viene eseguito correttamente.<br /><br />Si consiglia di ripristinare gli oggetti Criteri di gruppo dallo stato di backup in cui sono stati sincronizzati tutti gli oggetti Criteri di gruppo del server. Per informazioni su uno script che è possibile usare, vedere [backup e ripristino configurazione di accesso remoto](https://gallery.technet.microsoft.com/Back-up-and-Restore-Remote-e157e6a6).|  
  


