---
title: Eseguire la migrazione di un file server usando il servizio di migrazione di archiviazione
description: Breve descrizione dell'argomento per risultati dei motori di ricerca
author: jasongerend
ms.author: jgerend
manager: elizapo
ms.date: 02/13/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage
ms.openlocfilehash: 966f25eb0bd43513b3c544fb3dc97115ed668b28
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59872752"
---
# <a name="use-storage-migration-service-to-migrate-a-server"></a>Usa il servizio di migrazione di archiviazione per eseguire la migrazione di un server

In questo argomento viene descritto come migrare un server, incluso il file e la configurazione, a un altro server tramite [servizio di migrazione archiviazione](overview.md) e Windows Admin Center. Eseguire la migrazione richiede tre passaggi dopo aver installato il servizio e aprire le porte del firewall necessarie: creare un inventario dei server, il trasferimento dei dati e trasferimento ai nuovi server.

## <a name="step-0-install-storage-migration-service-and-check-firewall-ports"></a>Passaggio 0: Installare il servizio di migrazione di archiviazione e controllare le porte del firewall

Prima di iniziare, installare il servizio di migrazione di archiviazione e assicurarsi che le porte del firewall necessarie siano aperte.

1. Verificare i [requisiti del servizio di migrazione archiviazione](overview.md#requirements) e installare [Windows Admin Center](../../manage/windows-admin-center/understand/windows-admin-center.md) nel PC o un server di gestione se già stato fatto.
2. In Windows Admin Center, connettersi al server dell'agente di orchestrazione che esegue Windows Server 2019. <br>Si tratta del server sarà installarvi il servizio di migrazione di archiviazione e usare per gestire la migrazione. Se si esegue la migrazione di un solo server, è possibile usare il server di destinazione, purché sia in esecuzione Windows Server 2019. È consigliabile che usare un server di orchestrazione separata per le migrazioni a più server.
1. Passare a **Server Manager** (in Windows Admin Center) > **servizio di migrazione di archiviazione** e selezionare **installare** per installare il servizio di migrazione di archiviazione e i relativi componenti necessari (illustrato nella figura 1).
    ![Screenshot della pagina del servizio di migrazione di archiviazione che mostra il pulsante Install](media/migrate/install.png) **figura 1: Installazione del servizio di migrazione di archiviazione**
1. Installare il proxy del servizio di migrazione di archiviazione in tutti i server di destinazione che esegue Windows Server 2019. Ciò raddoppia la velocità di trasferimento durante l'installazione nei server di destinazione. <br>A tale scopo, connettersi al server di destinazione in Windows Admin Center e quindi andare al **Server Manager** (in Windows Admin Center) > **ruoli e funzionalità**, selezionare **il servizio di migrazione di archiviazione Proxy**, quindi selezionare **installare**.
1. In tutti i server di origine e in qualsiasi server di destinazione che esegue Windows Server 2012 R2 o Windows Server 2016, Windows Admin Center, connettersi a ogni server, passare a **Server Manager** (in Windows Admin Center) > **Firewall**   >  **Regole in ingresso**e quindi verificare che siano abilitate le regole seguenti:
    - Condivisione di file e stampanti (SMB-In)
    - Servizio Accesso rete (NP-In)
    - Strumentazione gestione Windows (DCOM-In)
    - Strumentazione gestione Windows (WMI-In)

   > [!NOTE]
   > Se si usano firewall di terze parti, gli intervalli di porte in ingresso per aprire sono TCP/445 (SMB) TCP/135 (agente mapping endpoint RPC/DCOM) e TCP tra 1025 e 65535 (porte temporanee RPC/DCOM).

1. Se si usa un server dell'agente di orchestrazione per gestire la migrazione e si vuole scaricare un log dei dati si trasferiscono o eventi, verificare che la condivisione File e stampanti (SMB-In) regola del firewall sia abilitata tale anche nel server.

## <a name="step-1-create-a-job-and-inventory-your-servers-to-figure-out-what-to-migrate"></a>Passaggio 1: Creare un processo e l'inventario dei server per capire cosa eseguire la migrazione

In questo passaggio, specificare quali server per eseguire la migrazione e quindi eseguirne la scansione per raccogliere informazioni sul file e le configurazioni.

1. Selezionare **nuovo processo**, denominare il processo e quindi selezionare **OK**.
1. Nel **immetterle** , digitare le credenziali di amministratore che funzionano nei server di cui si vuole eseguire la migrazione da e quindi selezionare **successivo**.
1. Selezionare **aggiungere un dispositivo**, digitare un nome di server di origine e quindi selezionare **OK**. <br>Ripetere questo passaggio per tutti gli altri server da aggiungere all'inventario.
1. Selezionare **Avvia analisi**.<br>Gli aggiornamenti di pagina per viene illustrato quando l'analisi è stata completata.
    ![Screenshot che illustra un server pronto per essere analizzati](media/migrate/inventory.png) **figura 2: Inventario dei server**
1. Selezionare ogni server per esaminare le condivisioni, configurazione, le schede di rete e volumi di cui sono entrati in inventario. <br><br>Il servizio di migrazione di archiviazione non trasferire file o cartelle che sappiamo che potrebbero interferire con l'operazione di Windows, in questa versione noterete che sono gli avvisi per tutte le condivisioni che si trova nella cartella di sistema di Windows. È possibile ignorare queste condivisioni durante la fase di trasferimento. Per altre informazioni, vedi [quali file e cartelle vengono esclusi dal trasferimento](faq.md#excluded-files).
1. Selezionare **successivo** passare per il trasferimento dei dati.

## <a name="step-2-transfer-data-from-your-old-servers-to-the-destination-servers"></a>Passaggio 2: Trasferire i dati dai server precedenti ai server di destinazione

In questo passaggio trasferire i dati dopo aver specificato la posizione in cui inserirlo nel server di destinazione.

 1. Nel **trasferire i dati** > **immettere le credenziali** , digitare le credenziali di amministratore che funzionano nei server di destinazione si vuole eseguire la migrazione a e quindi selezionare **Next**.
 1. Nel **aggiungere un dispositivo di destinazione e i mapping** pagina, il primo server di origine è presente. Digitare il nome del server a cui si desidera eseguire la migrazione e quindi selezionare **Cerca nel dispositivo**.
 1. Mappare i volumi di origine al volume di destinazione, deselezionare il **inclusione** casella di controllo per tutte le condivisioni non si vuole trasferire (tra cui eventuali condivisioni amministrative che si trova nella cartella di sistema Windows) e quindi selezionare **successivo** .
    ![Screenshot che illustra un server di origine e i relativi volumi e condivisioni e in cui sarà essere trasferite a nella destinazione](media/migrate/transfer.png) **figura 3: Un server di origine e in cui verrà trasferito lo spazio di archiviazione**
 1. Aggiungere un server di destinazione e i mapping per eventuali altri server di origine e quindi selezionare **successivo**.
 1. Facoltativamente, modificare le impostazioni di trasferimento e quindi selezionare **successivo**.
 1. Selezionare **Validate** e quindi selezionare **successivo**.
 1. Selezionare **avviare trasferimento** per avviare il trasferimento dei dati.<br>La prima volta che si trasferiscono, abbiamo sposterai tutti i file esistenti in una destinazione in una cartella di backup. Nei successivi trasferimenti, per impostazione predefinita che verrà aggiornata la destinazione senza eseguirne il backup prima. <br>Inoltre, il servizio di migrazione di archiviazione è abbastanza intelligente da affrontare sovrapposti condivisioni, copiamo non stesse cartelle due volte nello stesso processo.
 1. Dopo aver completato il trasferimento, consultare il server di destinazione per assicurarsi che tutti gli elementi trasferito correttamente. Selezionare **solo i log degli errori** se si desidera scaricare un log di tutti i file che non sono state trasferite.

  > [!NOTE]
  > Se si vuole mantenere un audit trail di trasferimenti o si prevede di eseguire più di un trasferimento in un processo, fare clic su **registro trasferimenti** per salvare una copia CSV. Tutti i successivi trasferimenti sovrascrive le informazioni sul database di un'esecuzione precedente. 

A questo punto, sono disponibili tre opzioni:

- **Andare al passaggio successivo**, tagliando failover in modo che i server di destinazione adottano l'identità del server di origine.
- **Prendere in considerazione la migrazione completa** senza assumere il controllo delle identità dei server di origine.
- **Nuovo il trasferimento**, copiare solo i file che sono stati aggiornati dall'ultimo trasferimento.

Se l'obiettivo consiste nel sincronizzare i file con Azure, è possibile configurare i server di destinazione con sincronizzazione File di Azure dopo il trasferimento di file o il cutting ai server di destinazione (vedere [pianificazione per la distribuzione di sincronizzazione File di Azure](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning)).

## <a name="step-3-optionally-cut-over-to-the-new-servers"></a>Passaggio 3: Se lo si desidera trasferire gli ai nuovi server

In questo passaggio si taglia tramite dai server di origine per i server di destinazione, lo spostamento di indirizzi IP e nomi di computer ai server di destinazione. Una volta completato questo passaggio, le App e accedere ai nuovi server tramite i nomi e indirizzi dei server da che è eseguita la migrazione.

 1. Se è stato reindirizzato dal processo di migrazione, in Windows Admin Center, andare al **Server Manager** > **servizio migrazione del archiviazione** e quindi selezionare il processo che si desidera completare. 
 1. Nel **trasferimento ai nuovi server** > **immettere le credenziali** pagina, selezionare **Avanti** per utilizzare le credenziali immesse in precedenza.
 1. Nel **Configura cutover** pagina specificare le schede di rete per assumere il controllo delle impostazioni del dispositivo ogni origine. L'indirizzo IP viene spostato dall'origine alla destinazione durante il trasferimento.
 1. Specificare quale indirizzo IP per l'utilizzo del server di origine dopo la migrazione completa consente di passare il relativo indirizzo di destinazione. È possibile usare DHCP o un indirizzo statico. Se si usa un indirizzo statico, la nuova subnet deve essere che lo stesso come la precedente subnet o cutover avrà esito negativo.
    ![Screenshot che illustra un server di origine e relativi indirizzi IP e nomi di computer e ciò che verrà sostituiti con dopo il cutover](media/migrate/cutover.png)
    **figura 4: Un server di origine e il modo in cui si sposta la configurazione di rete nella destinazione**
 1. Specificare come rinominare il server di origine dopo che il server di destinazione subentra il relativo nome. È possibile usare un nome generato in modo casuale o digitare uno manualmente. Quindi selezionare **successivo**.
 1. Selezionare **successivo** nel **regolare le impostazioni di cutover** pagina.
 1. Selezionare **Validate** nel **dispositivo di origine e destinazione Validate** pagina e quindi selezionare **Avanti**.
 1. Quando si è pronti per eseguire la migrazione completa, selezionare **avvia cutover**. <br>Utenti e App, potrebbe verificarsi un'interruzione mentre vengono spostati i nomi e l'indirizzo e il server riavviati più volte ogni, ma in caso contrario non saranno interessate dalla migrazione. Quanto tempo cutover dipende dalla rapidità con cui riavviare i server, nonché tempi di replica di Active Directory e DNS.

## <a name="see-also"></a>Vedere anche

- [Panoramica del servizio di migrazione archiviazione](overview.md)
- [Servizi di migrazione archiviazione domande frequenti (FAQ)](faq.md)
- [Pianificazione per la distribuzione di sincronizzazione File di Azure](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning)