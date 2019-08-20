---
title: Eseguire la migrazione di un file server usando il servizio migrazione archiviazione
description: Breve descrizione dell'argomento per i risultati del motore di ricerca
author: jasongerend
ms.author: jgerend
manager: elizapo
ms.date: 02/13/2019
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: storage
ms.openlocfilehash: 7cec2a9c805208baceff8a8afe22a20fd2859edd
ms.sourcegitcommit: e2b565ce85a97c0c51f6dfe7041f875a265b35dd
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/19/2019
ms.locfileid: "69584846"
---
# <a name="use-storage-migration-service-to-migrate-a-server"></a>Usare il servizio migrazione archiviazione per eseguire la migrazione di un server

Questo argomento illustra come eseguire la migrazione di un server, inclusi i relativi file e configurazione, a un altro server usando il [servizio migrazione archiviazione](overview.md) e l'interfaccia di amministrazione di Windows. La migrazione esegue tre passaggi dopo l'installazione del servizio e l'apertura delle porte del firewall necessarie, ovvero l'inventario dei server, il trasferimento dei dati e l'abbreviazione nei nuovi server.

## <a name="step-0-install-storage-migration-service-and-check-firewall-ports"></a>Passaggio 0: Installare il servizio migrazione archiviazione e controllare le porte del firewall

Prima di iniziare, installare il servizio migrazione archiviazione e assicurarsi che siano aperte le porte del firewall necessarie.

1. Se non è già stato fatto, verificare i [requisiti del servizio migrazione archiviazione](overview.md#requirements) e installare l'interfaccia di [amministrazione di Windows](../../manage/windows-admin-center/understand/windows-admin-center.md) nel PC o in un server di gestione.
2. Nell'interfaccia di amministrazione di Windows connettersi al server dell'agente di orchestrazione che esegue Windows Server 2019. <br>Si tratta del server in cui verrà installato il servizio migrazione archiviazione e viene utilizzato per gestire la migrazione. Se si esegue la migrazione di un solo server, è possibile usare il server di destinazione purché sia in esecuzione Windows Server 2019. Si consiglia di utilizzare un server di orchestrazione separato per tutte le migrazioni multiserver.
1. Passare a **Server Manager** (nell'interfaccia di amministrazione di Windows) > **servizio migrazione archiviazione** e selezionare **Installa** per installare il servizio migrazione archiviazione e i relativi componenti necessari, come illustrato nella figura 1.
    ![Screenshot della pagina del servizio migrazione archiviazione che mostra il pulsante](media/migrate/install.png) **di installazione Figura 1: Installazione del servizio migrazione archiviazione**
1. Installare il proxy del servizio migrazione archiviazione in tutti i server di destinazione che eseguono Windows Server 2019. Questa operazione raddoppia la velocità di trasferimento quando viene installata nei server di destinazione. <br>A tale scopo, connettersi al server di destinazione nell'interfaccia di amministrazione di Windows, quindi passare a **Server Manager** (nell'interfaccia di amministrazione di windows) > **ruoli e funzionalità**, selezionare **proxy servizio migrazione archiviazione**e quindi selezionare **Installa**.
1. In tutti i server di origine e in tutti i server di destinazione che eseguono Windows Server 2012 R2 o Windows Server 2016, nell'interfaccia di amministrazione di Windows, connettersi a ogni server, passare a **Server Manager** (nell'interfaccia di amministrazione di windows) > **Firewall**  >   **Regole in ingresso**, quindi verificare che siano abilitate le seguenti regole:
    - Condivisione di file e stampanti (SMB-In)
    - Servizio Netlogon (NP-in)
    - Strumentazione gestione Windows (DCOM-in)
    - Strumentazione gestione Windows (WMI-In)

   > [!NOTE]
   > Se si usano firewall di terze parti, gli intervalli di porte in ingresso da aprire sono TCP/445 (SMB), TCP/135 (mapper di endpoint RPC/DCOM) e TCP 1025-65535 (porte temporanee RPC/DCOM). Le porte del servizio migrazione archiviazione sono TCP/28940 (agente di orchestrazione) e TCP/28941 (proxy).

1. Se si usa un server dell'agente di orchestrazione per gestire la migrazione e si vuole scaricare gli eventi o un log dei dati trasferiti, verificare che la regola del firewall condivisione file e stampanti (SMB-in) sia abilitata anche in tale server.

## <a name="step-1-create-a-job-and-inventory-your-servers-to-figure-out-what-to-migrate"></a>Passaggio 1: Creazione di un processo e inventario dei server per determinare gli elementi da migrare

In questo passaggio si specificano i server di cui eseguire la migrazione e quindi li si analizza per raccogliere informazioni sui file e le configurazioni.

1. Selezionare **nuovo processo**, denominare il processo e quindi fare clic su **OK**.
1. Nella pagina **immissione credenziali** Digitare le credenziali di amministratore che funzionano nei server da cui si desidera eseguire la migrazione e quindi fare clic su **Avanti**.
1. Selezionare **Aggiungi un dispositivo**, digitare il nome del server di origine e quindi fare clic su **OK**. <br>Ripetere questa operazione per tutti gli altri server che si desidera includere nell'inventario.
1. Selezionare **Avvia analisi**.<br>La pagina viene aggiornata per visualizzare il completamento dell'analisi.
    ![Screenshot che mostra un server pronto per essere](media/migrate/inventory.png) analizzato **figura 2: Inventario di server**
1. Selezionare ogni server per esaminare le condivisioni, la configurazione, le schede di rete e i volumi di cui è stato incluso l'inventario. <br><br>Il servizio migrazione archiviazione non trasferirà i file o le cartelle che sappiamo potrebbero interferire con l'operazione di Windows. in questa versione, quindi, verranno visualizzati avvisi per le condivisioni presenti nella cartella di sistema di Windows. È necessario ignorare queste condivisioni durante la fase di trasferimento. Per altre informazioni, vedere [quali file e cartelle sono esclusi dai trasferimenti](faq.md#what-files-and-folders-are-excluded-from-transfers).
1. Selezionare **Avanti** per passare al trasferimento dei dati.

## <a name="step-2-transfer-data-from-your-old-servers-to-the-destination-servers"></a>Passaggio 2: Trasferire i dati dai server obsoleti ai server di destinazione

In questo passaggio si trasferiscono i dati dopo aver specificato dove inserirli nei server di destinazione.

1. Nella pagina **trasferimento dati** > **immissione credenziali** digitare le credenziali di amministratore che funzionano nei server di destinazione in cui si desidera eseguire la migrazione e quindi fare clic su **Avanti**.
2. Nella pagina **Aggiungi un dispositivo di destinazione e mapping** , viene elencato il primo server di origine. Digitare il nome del server in cui si desidera eseguire la migrazione e quindi selezionare **analizza dispositivo**.
3. Eseguire il mapping dei volumi di origine ai volumi di destinazione, deselezionare la casella di controllo Includi per le condivisioni che non si desidera trasferire (incluse le condivisioni amministrative presenti nella cartella di sistema di Windows) e quindi selezionare **Avanti**.
   ![Screenshot che mostra un server di origine e i relativi volumi e condivisioni e la posizione in cui verranno](media/migrate/transfer.png) trasferiti nella destinazione **figura 3: Un server di origine e il percorso di archiviazione in cui verranno trasferiti**
4. Aggiungere un server di destinazione e i mapping per tutti i server di origine e quindi fare clic su **Avanti**.
5. Facoltativamente, modificare le impostazioni di trasferimento, quindi selezionare **Avanti**.
6. Selezionare **convalida** , quindi fare clic su **Avanti**.
7. Selezionare **Avvia trasferimento** per avviare il trasferimento dei dati.<br>La prima volta che si trasferisce, si sposteranno tutti i file esistenti in una destinazione in una cartella di backup. Ai trasferimenti successivi, per impostazione predefinita la destinazione verrà aggiornata senza prima eseguirne il backup. <br>Inoltre, il servizio migrazione archiviazione è sufficientemente intelligente da gestire le condivisioni sovrapposte. non verranno copiate due volte le stesse cartelle nello stesso processo.
8. Al termine del trasferimento, controllare il server di destinazione per verificare che tutto sia stato trasferito correttamente. Selezionare **log degli errori solo** se si desidera scaricare un log di tutti i file che non sono stati trasferiti.

   > [!NOTE]
   > Se si desidera conservare un audit trail di trasferimenti o pianificare l'esecuzione di più di un trasferimento in un processo, fare clic su **trasferimento log** per salvare una copia CSV. Ogni trasferimento successivo sovrascrive le informazioni del database di un'esecuzione precedente. 

A questo punto, sono disponibili tre opzioni:

- **Andare al passaggio successivo**, in modo che i server di destinazione adotti le identità dei server di origine.
- **Tenere presente che la migrazione** è stata completata senza assumere le identità dei server di origine.
- **Trasferire nuovamente**, copiando solo i file aggiornati dopo l'ultimo trasferimento.

Se l'obiettivo consiste nel sincronizzare i file con Azure, è possibile configurare i server di destinazione con Sincronizzazione file di Azure dopo il trasferimento dei file o dopo il trasferimento nei server di destinazione. vedere [pianificazione per una distribuzione di sincronizzazione file di Azure](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning).

## <a name="step-3-optionally-cut-over-to-the-new-servers"></a>Passaggio 3: Facoltativamente, è possibile passare ai nuovi server

In questo passaggio si passa dai server di origine ai server di destinazione e si trasferiscono gli indirizzi IP e i nomi dei computer ai server di destinazione. Al termine di questo passaggio, le app e gli utenti accedono ai nuovi server tramite i nomi e gli indirizzi dei server da cui è stata eseguita la migrazione.

 1. Se si è passati dal processo di migrazione, nell'interfaccia di amministrazione di Windows passare a **Server Manager** > **servizio migrazione archiviazione** e quindi selezionare il processo che si desidera completare. 
 1. Nella pagina passaggio **alla nuova immissione dei server** > **immettere le credenziali** selezionare **Avanti** per usare le credenziali digitate in precedenza.
 1. Nella pagina **Configura cutover** specificare le schede di rete da assumere per le impostazioni del dispositivo di origine. In questo modo, l'indirizzo IP viene spostato dall'origine alla destinazione come parte del cutover.
 1. Specificare l'indirizzo IP da utilizzare per il server di origine dopo che cutover sposta il proprio indirizzo nella destinazione. È possibile usare DHCP o un indirizzo statico. Se si usa un indirizzo statico, la nuova subnet deve essere identica a quella precedente oppure cutover avrà esito negativo.
    ![Screenshot che mostra un server di origine e i relativi indirizzi IP e i nomi di computer e gli elementi in cui](media/migrate/cutover.png)
    verranno sostituiti dopo cutover**figura 4: Un server di origine e il modo in cui la configurazione di rete viene spostata nella destinazione**
 1. Specificare come rinominare il server di origine dopo che il server di destinazione acquisisce il nome. È possibile usare un nome generato in modo casuale o digitarne uno. Quindi selezionare **Avanti**.
 1. Nella pagina **Modifica impostazioni cutover** selezionare **Avanti** .
 1. Selezionare **convalida** nella pagina **convalida dispositivo di origine e destinazione** e quindi fare clic su **Avanti**.
 1. Quando si è pronti per eseguire il cutover, selezionare **Avvia cutover**. <br>Gli utenti e le app potrebbero riscontrare un'interruzione mentre l'indirizzo e i nomi vengono spostati e i server vengono riavviati più volte, ma in caso contrario non saranno interessati dalla migrazione. Il tempo impiegato da cutover dipende dalla velocità con cui i server vengono riavviati, nonché da Active Directory e tempi di replica DNS.

## <a name="see-also"></a>Vedere anche

- [Panoramica di servizio migrazione archiviazione](overview.md)
- [Domande frequenti sui servizi di migrazione archiviazione](faq.md)
- [Pianificazione di una distribuzione di Sincronizzazione file di Azure](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning)
