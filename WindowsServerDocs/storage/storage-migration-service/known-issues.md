---
title: Servizio di migrazione archiviazione problemi noti
description: Problemi noti e risoluzione dei problemi il supporto per il servizio di migrazione di archiviazione, ad esempio come raccogliere i log per il supporto tecnico Microsoft.
author: nedpyle
ms.author: nedpyle
manager: siroy
ms.date: 02/27/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage
ms.openlocfilehash: f5fefab2c1b7ba8b62ffd6734217eab9a13ae95e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888872"
---
# <a name="storage-migration-service-known-issues"></a>Servizio di migrazione archiviazione problemi noti

In questo argomento è fornite le risposte ai problemi noti quando si usa [servizio di migrazione archiviazione](overview.md) la migrazione di server.

## <a name="collecting-logs"></a> Come raccogliere i file di log quando si lavora con il supporto tecnico Microsoft

Il servizio di migrazione di archiviazione contiene i registri eventi per il servizio Orchestrator e il servizio Proxy. Il server urchestrator include sempre sia i registri eventi e i server di destinazione con installato il servizio proxy contengono i log di proxy. Questi log si trovano in:

- Registri applicazioni e servizi \ Microsoft \ Windows \ StorageMigrationService
- Registri applicazioni e servizi \ Microsoft \ Windows \ Proxy StorageMigrationService

Se è necessario raccogliere questi log per la visualizzazione non in linea o per l'invio al supporto Microsoft, è presente un open source script di PowerShell disponibile in GitHub:

 [Helper di servizio di migrazione archiviazione](https://aka.ms/smslogs) 

Esaminare il file Leggimi per informazioni sull'utilizzo.

## <a name="storage-migration-service-doesnt-show-up-in-windows-admin-center-unless-managing-windows-server-2019"></a>Il servizio di migrazione di archiviazione non viene visualizzata in Windows Admin Center, a meno che la gestione di Windows Server 2019

Quando si usa la versione 1809 di Windows Admin Center per gestire un agente di orchestrazione di Windows Server 2019, opzione non è presente la strumento per il servizio di migrazione di archiviazione. 

L'estensione del servizio migrazione di archiviazione di Windows Admin Center è associato a versione per gestire solo versione di Windows Server 2019 1809 o sistemi operativi successivi. Se si usarlo per gestire i sistemi operativi Windows Server meno recenti o anteprime insider, lo strumento non verrà visualizzata. Questo comportamento è da progettazione. 

Per risolvere, usare o eseguire l'aggiornamento a Windows Server 2019 build 1809 o versione successiva.

## <a name="storage-migration-service-doesnt-let-you-choose-static-ip-on-cutover"></a>Il servizio di migrazione di archiviazione non è possibile scegliere l'indirizzo IP statico in cutover

Quando si usa la versione 0,57 la migrazione del servizio di archiviazione estensione Windows Admin Center e si raggiunge la fase di migrazione completa, è possibile selezionare un indirizzo IP statico per un indirizzo. È necessario usare il protocollo DHCP.

Per risolvere questo problema, in Windows Admin Center, esaminare **le impostazioni** > **estensioni** per un avviso indicante che tale versione aggiornata del servizio di migrazione archiviazione 0.57.2 è disponibile per l'installazione. Si potrebbe essere necessario riavviare la scheda del browser per interfaccia di amministrazione di Windows.

## <a name="storage-migration-service-cutover-validation-fails-with-error-access-is-denied-for-the-token-filter-policy-on-destination-computer"></a>Archiviazione del servizio di migrazione cutover convalida ha esito negativo con errore "Accesso negato per i criteri di filtro di token nel computer di destinazione"

Quando si esegue la convalida cutover, viene visualizzato l'errore "errore: Accesso negato per i criteri di filtro di token nel computer di destinazione". Ciò si verifica anche se sono fornite le credenziali di amministratore locale corretto per i computer di origine e di destinazione.

Questo problema è causato da un difetto del codice in Windows Server 2019. Il problema si verifica quando si usa il computer di destinazione come agente di orchestrazione del servizio migrazione archiviazione.

Per risolvere questo problema, installare il servizio di migrazione di archiviazione in un computer Windows Server 2019 che non è la destinazione di migrazione desiderato, quindi connettersi al server con Windows Admin Center ed eseguire la migrazione.

Si prevede di risolvere il problema in una versione successiva di Windows Server. Aprire un caso di supporto tramite [supporto tecnico Microsoft](https://support.microsoft.com) per richiedere un esegue il backporting di questa soluzione venga creato.

## <a name="storage-migration-service-isnt-included-in-windows-server-2019-evaluation-edition"></a>Il servizio di migrazione di archiviazione non è incluso nell'edizione di valutazione di Windows Server 2019

Quando si usa Windows Admin Center per la connessione a un [versione di valutazione di Windows Server 2019](https://www.microsoft.com/evalcenter/evaluate-windows-server-2019) non c'è un'opzione per gestire il servizio di migrazione di archiviazione. Il servizio di migrazione di archiviazione non è incluso anche in ruoli e funzionalità.

Questo problema è causato da un problema di manutenzione nel supporto di valutazione di Windows Server 2019. 

Per risolvere questo problema, installare delle vendite al dettaglio, la versione MSDN, OEM o Volume License di Windows Server 2019 e non l'attivo. Senza attivazione, tutte le edizioni di Windows Server operano in modalità di valutazione di 180 giorni. 

Questo problema è stato risolto in una versione successiva di Windows Server 2019.  

## <a name="storage-migration-service-times-out-downloading-the-transfer-error-csv"></a>Il servizio di migrazione archiviazione timeout scaricando l'errore di trasferimento CSV

Quando si usa Windows Admin Center o PowerShell per scaricare il trasferimento operazioni solo errori CSV log dettagliato, viene visualizzato l'errore:

 >   Registro trasferimento: verificare la condivisione file è consentita nel firewall. : L'operazione di richiesta inviato a NET.TCP://localhost:8001/NetTcp: 28940/sms/service/1/trasferimento non ha ricevuto una risposta entro il timeout configurato (00: 01:00). Il tempo allocato a questa operazione fosse una parte di un timeout più lungo. Può trattarsi di perché il servizio sta ancora elaborando l'operazione o il servizio è riuscito a inviare un messaggio di risposta. . Provare ad aumentare il timeout dell'operazione (eseguendo il cast del canale/proxy su IContextChannel e impostando la proprietà OperationTimeout) e verificare che il servizio sia in grado di connettersi al client.

Questo problema è causato da un numero estremamente elevato di file trasferiti non è possibile filtrare il timeout di un minuto predefinito consentita dal servizio di migrazione di archiviazione. 

Per risolvere questo problema:

1. Nel computer dell'agente di orchestrazione, modificare il *%SYSTEMROOT%\SMS\Microsoft.StorageMigration.Service.exe.config* file utilizzando Notepad.exe modificare "sendTimeout" rispetto all'impostazione predefinita di 1 minuto a 10 minuti

   ```
     <bindings>
      <netTcpBinding>
        <binding name="NetTcpBindingSms"
                 sendTimeout="00:01:00"
   ```

2. Riavviare il servizio "Servizio di migrazione di archiviazione" sul computer dell'agente di orchestrazione. 
3. Nel computer dell'agente di orchestrazione, avviare Regedit.exe
4. Individuare e fare clic sulla seguente sottochiave del Registro di sistema: 

   `HKEY_LOCAL_MACHINE\\Software\\Microsoft\\SMSPowershell`

5. Dal menu Modifica, scegliere Nuovo e poi fare clic su Valore DWORD. 
6. Digitare "WcfOperationTimeoutInMinutes" per il nome del valore DWORD e quindi premere INVIO.
7. Fare doppio clic su "WcfOperationTimeoutInMinutes" e quindi fare clic su Modifica. 
8. Nella finestra di dati di Base, fare clic su "Decimal"
9. Nella casella dati valore, digitare "10" e quindi fare clic su OK.
10. Uscire dall'editor del Registro di sistema.
11. Tentativo di scaricare nuovamente il file CSV solo gli errori. 

Si prevede di modificare questo comportamento nelle versioni successive di Windows Server 2019.  

## <a name="cutover-fails-when-migrating-between-networks"></a>Cutover si verifica un errore durante la migrazione tra le reti

Durante la migrazione a una macchina virtuale di destinazione in esecuzione in una rete diversa rispetto all'origine, ad esempio un'istanza IaaS di Azure, migrazione completa non viene completata quando l'origine usava un indirizzo IP statico. 

Questo comportamento è per impostazione predefinita, per evitare problemi di connettività dopo la migrazione da utenti, applicazioni e gli script che si connettono tramite indirizzo IP. Quando l'indirizzo IP viene spostato dal computer di origine precedente alla nuova destinazione di destinazione, non corrisponderà le nuove informazioni di subnet di rete, ad esempio DNS e WINS.

Per risolvere questo problema, eseguire la migrazione in un computer nella stessa rete. Quindi passare tale computer a una nuova rete e riassegnare le relative informazioni IP. Ad esempio, se la migrazione ad Azure IaaS, prima di tutto la migrazione a una macchina virtuale locale, quindi usare Azure Migrate per spostare la macchina virtuale in Azure.  

Questo problema è stato risolto in una versione successiva di Windows Server 2019. A questo punto si sarà consentono di specificare le migrazioni che non modificano le impostazioni di rete del server di destinazione. È possibile rilasciare un aggiornamento alla versione esistente di Windows Server 2019 come parte del ciclo di aggiornamento mensile normale. 


## <a name="see-also"></a>Vedere anche

- [Panoramica del servizio di migrazione archiviazione](overview.md)