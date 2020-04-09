---
title: Monitorare lo stato di distribuzione della configurazione del server di accesso remoto
description: Questo argomento fa parte della Guida per il monitoraggio e l'accounting di accesso remoto in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: de285d13-9e54-4c46-88f0-607182e5e3dc
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 19c3d9c306215c92edb2bf322e722a93f9962738
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854034"
---
# <a name="monitor-the-configuration-distribution-status-of-the-remote-access-server"></a>Monitorare lo stato di distribuzione della configurazione del server di accesso remoto

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

**Nota:** Windows Server 2012 combina DirectAccess e il servizio di accesso remoto (RAS) in un singolo ruolo accesso remoto.  
  
La console di gestione di Accesso remoto confronta le versioni di configurazione di tutti i server monitorati per verificare che corrispondano e usino la versione di configurazione più recente. In tal modo è possibile verificare se la versione di configurazione più recente (specificata negli oggetti Criteri di gruppo) è stata distribuita e applicata correttamente a tutti i server.  
  
### <a name="to-use-the-monitoring-dashboard-to-monitor-the-configuration-distribution"></a>Per usare il dashboard di monitoraggio per monitorare la distribuzione della configurazione  
  
1.  In **Server Manager** fare clic su **Strumenti** e quindi su **Gestione Accesso remoto**.  
  
2.  Fare clic su **DASHBOARD** per passare a **Dashboard di Accesso remoto** nella **console di gestione di Accesso remoto**.  
  
3.  Sul dashboard di monitoraggio osservare il riquadro **Stato configurazione** al centro in alto, in cui è visualizzato lo stato corrente della distribuzione della configurazione.  
  
La tabella seguente contiene i messaggi visualizzati nel riquadro **Stato configurazione**, i relativi significati e le eventuali operazioni amministrative necessarie.  
  
|||||  
|-|-|-|-|  
|Severity|Message|Significato|Azioni necessarie|  
|Success|Distribuzione della configurazione completata.|La configurazione nell'oggetto Criteri di gruppo è stata applicata correttamente al server.|Nessuna azione necessaria.|  
|Avviso|Configurazione per il server [*nome server*] non recuperata dal controller di dominio. L'oggetto Criteri di gruppo non è collegato.|La configurazione nell'oggetto Criteri di gruppo non ha ancora raggiunto il server. Il motivo potrebbe essere il mancato collegamento dell'oggetto Criteri di gruppo al server.|Collegare l'oggetto Criteri di gruppo a un ambito di gestione applicato al server oppure, in uno scenario in cui l'oggetto Criteri di gruppo è in gestione temporanea, esportare manualmente le impostazioni da quest'ultimo e importarle nell'oggetto Criteri di gruppo di produzione. Per ulteriori informazioni sugli oggetti Criteri di gruppo di staging, vedere **gestione degli oggetti Criteri di gruppo di accesso remoto con autorizzazioni limitate** in [Step-1-Plan-the-DirectAccess-Infrastructure](../../directaccess/single-server-advanced/Step-1-Plan-the-DirectAccess-Infrastructure.md). Per i passaggi di gestione temporanea di GPO, vedere **configurazione degli oggetti Criteri di gruppo di accesso remoto con autorizzazioni limitate** in [passaggio 1: configurare l'infrastruttura DirectAccess](../../directaccess/single-server-advanced/Step-1-Configuring-DirectAccess-Infrastructure.md).|  
|Avviso|Configurazione per il server [*nome server*] non ancora recuperata dal controller di dominio.|La configurazione nell'oggetto Criteri di gruppo non ha ancora raggiunto il server.<p>La propagazione di una nuova configurazione potrebbe richiedere fino a 10 minuti.|Per l'aggiornamento dei criteri sul server prevedere un intervallo più lungo.|  
|Errore|Non è possibile recuperare la configurazione per il server [*nome server*] dal controller di dominio.|La configurazione nell'oggetto Criteri di gruppo non ha raggiunto il server e sono trascorsi oltre 10 minuti dalla modifica della configurazione.|Ciò potrebbe verificarsi in uno degli scenari seguenti:<p>-Il server non dispone di connettività al dominio per aggiornare i criteri. È possibile eseguire "gpupdate/force" sul server per forzare un aggiornamento dei criteri.<br />-La replica degli oggetti Criteri di gruppo potrebbe essere necessaria per recuperare la configurazione aggiornata.<br />-Non è presente alcun controller di dominio scrivibile nel sito Active Directory del server di accesso remoto.<p>Attendere la replica degli oggetti Criteri di gruppo su tutti i controller di dominio e quindi usare il cmdlet **Set-DAEntryPointDC** di Windows PowerShell per associare il punto di ingresso a un controller di dominio scrivibile in Active Directory nel server di Accesso remoto.|  
|Avviso|Configurazione per il server [*nome server*] recuperata dal controller di dominio, ma non ancora applicata.|La configurazione nell'oggetto Criteri di gruppo ha raggiunto il server, ma non è stata ancora applicata.<p>L'applicazione della configurazione potrebbe richiedere fino a 15 minuti.|Prevedere maggior tempo per la completa applicazione della configurazione al server.|  
|Errore|Non è possibile applicare la configurazione per il server [*nome server*] recuperata dal controller di dominio.|La configurazione nell'oggetto Criteri di gruppo ha raggiunto il server, ma non è stata applicata correttamente e sono trascorsi oltre 15 minuti dalla modifica della configurazione.|Ciò potrebbe verificarsi in uno degli scenari seguenti:<p>1. la configurazione è attualmente in fase di applicazione. L'errore visualizzato può essere dovuto al lungo periodo di tempo impiegato per recuperare la configurazione dall'oggetto Criteri di gruppo.<br />    Per verificare se questo è il motivo, usare l'**Utilità di pianificazione** e passare a Microsoft\Windows\RemoteAccess per accertarsi che **RAConfigTask** sia attualmente in esecuzione.<br />2. Se **RAConfigTask** non è in esecuzione, potrebbe non essere in grado di applicare la configurazione nel server.<br />    Controllare gli errori nel **Visualizzatore eventi** nel canale delle operazioni del server di Accesso remoto, che si trova al percorso \Registri applicazioni e servizi\Microsoft\Windows\RemoteAccess-RemoteAccessServer.<br />    Verificare l'eventuale presenza di errori in **STATO OPERAZIONI** nella console di gestione Accesso remoto. Per altre informazioni, vedere [Monitorare lo stato operazioni del server di Accesso remoto e dei relativi componenti](Monitor-the-operations-status-of-the-Remote-Access-server-and-its-components.md).|  
|Errore|Configurazione per server multisito recuperata dal controller di dominio. La configurazione non corrisponde su tutti i server.|Vi è un'incoerenza tra le versioni di configurazione degli oggetti Criteri di gruppo del server nella distribuzione multisito.<p>Tutti gli oggetti Criteri di gruppo del server per tutti i punti di ingresso dovrebbero avere la stessa configurazione globale, ma per qualche motivo non sono sincronizzati.|Ciò può accadere quando la modifica della configurazione ha esito negativo e il rollback non viene eseguito correttamente.<p>Si consiglia di ripristinare gli oggetti Criteri di gruppo dallo stato di backup in cui sono stati sincronizzati tutti gli oggetti Criteri di gruppo del server. Per informazioni su uno script che è possibile usare, vedere [backup e ripristino della configurazione di accesso remoto](https://gallery.technet.microsoft.com/Back-up-and-Restore-Remote-e157e6a6).|  
  


