---
title: Gestire l'infrastruttura Iperconvergente con Windows Admin Center
description: Gestire l'infrastruttura Iperconvergente con Windows Admin Center (progetto Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 03/01/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: fe00072932d9c7f283ebd887a5292ac9a9d0e37f
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66446036"
---
# <a name="manage-hyper-converged-infrastructure-with-windows-admin-center"></a>Gestire l'infrastruttura Iperconvergente con Windows Admin Center

>Si applica a: Windows Admin Center, Windows Admin Center anteprima

## <a name="what-is-hyper-converged-infrastructure"></a>Che cos'è l'infrastruttura Hyper-Converged

Infrastruttura Iperconvergente consolida definita dal software di calcolo, archiviazione e rete in un unico cluster per offrire prestazioni elevate, economica e la virtualizzazione facilmente scalabile. Questa funzionalità è stata introdotta in Windows Server 2016 con [spazi di archiviazione diretta](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview), [Software Defined Networking](https://docs.microsoft.com/en-us/windows-server/networking/sdn/software-defined-networking) e [Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-on-windows-server).

> [!Tip]
> Se si desidera per acquisire Hyper-Converged infrastruttura, Microsoft consiglia queste [Windows Server Software-Defined](https://microsoft.com/wssd) soluzioni dei partner. Sono progettate, assemblati e convalidare l'architettura di riferimento per garantire rapidità e affidabilità, in modo da essere operativi e compatibilità.

> [!IMPORTANT]
> Alcune delle funzionalità descritte in questo articolo sono disponibili solo in fase di anteprima di Windows Admin Center. [Come si ottiene questa versione?](http://aka.ms/windowsadmincenter)

## <a name="what-is-windows-admin-center"></a>Che cos'è Windows Admin Center

[Windows Admin Center](../understand/windows-admin-center.md) è lo strumento di gestione di prossima generazione per Windows Server, il successore di strumenti tradizionali "in-box", ad esempio Server Manager. È gratuito e può essere installato e utilizzato senza una connessione Internet. È possibile usare Windows Admin Center per gestire e monitorare l'infrastruttura Hyper-Converged che eseguono Windows Server 2016 o Windows Server 2019.

![Dashboard del cluster iperconvergente](../media/manage-hyper-converged/hci-dashboard-v1809.png)

## <a name="key-features"></a>Funzionalità principali

Novità di Windows Admin Center per l'infrastruttura Hyper-Converged includono:

- **Unificata singolo-pannello-di-controllo di calcolo, archiviazione e rete a breve.** Visualizzare le macchine virtuali, i server host, i volumi, unità e altro ancora all'interno di un fatto su misura, coerente e interconnesse.
- **Creare e gestire le macchine virtuali Hyper-V e spazi di archiviazione.** I flussi di lavoro radicalmente semplice per creare, aprire, ridimensionare ed eliminare volumi. e creare, avviare, connettersi a e spostare le macchine virtuali; e molto altro ancora.
- **Il monitoraggio avanzato a livello di cluster.** Memoria e utilizzo della CPU, capacità di archiviazione, IOPS, velocità effettiva e latenza nel creare un grafico del dashboard in tempo reale, in ogni server del cluster, con gli avvisi non crittografati quando qualcosa non è corretto.
- **Supporto di software Defined Networking (SDN).** Gestire e monitorare le reti virtuali, subnet, connettono le macchine virtuali a reti virtuali e monitorare l'infrastruttura SDN.

Windows Admin Center per l'infrastruttura Hyper-Converged viene sviluppato attivamente da Microsoft. Riceve gli aggiornamenti frequenti di migliorare le funzionalità esistenti e aggiungono nuove funzionalità.

## <a name="before-you-start"></a>Prima di iniziare

Per gestire il cluster come infrastruttura Hyper-Converged in Windows Admin Center, deve essere in esecuzione Windows Server 2016 o Windows Server 2019 e Hyper-V e spazi di archiviazione diretta abilitata. Facoltativamente, può avere anche Software Defined Networking, abilitato e gestiti tramite Windows Admin Center.

> [!Tip]
> Windows Admin Center offre anche una gestione generalizzata una migliore esperienza per tutti i cluster che supportano qualsiasi carico di lavoro disponibile per Windows Server 2012 e versioni successive. Se ciò possa sembrare una soluzione migliore, quando si aggiunge il cluster a Windows Admin Center, selezionare [ **Cluster di Failover** ](manage-failover-clusters.md) anziché **Cluster Hyper-Converged**.

### <a name="prepare-your-windows-server-2016-cluster-for-windows-admin-center"></a>Preparare il cluster di Windows Server 2016 per Windows Admin Center

Windows Admin Center Hyper-Converged infrastruttura dipende da gestione che API aggiunte dopo il rilascio di Windows Server 2016. Prima di poter gestire il cluster di Windows Server 2016 con Windows Admin Center, è necessario eseguire questi due passaggi:

1. Verificare che tutti i server del cluster sia installato il [aggiornamento cumulativo per Windows Server 2016 (KB4103723) / 05 2018](https://support.microsoft.com/help/4103723/windows-10-update-kb4103723) o versione successiva. Per scaricare e installare questo aggiornamento, passare a **le impostazioni** > **aggiornamento e sicurezza** > **aggiornare Windows** selezionare  **Controlla online per gli aggiornamenti da Microsoft Update**.
2. Eseguire il seguente cmdlet di PowerShell come amministratore nel cluster:

```powershell
    Add-ClusterResourceType -Name "SDDC Management" -dll "$env:SystemRoot\Cluster\sddcres.dll" -DisplayName "SDDC Management"
```

> [!Tip]
> È sufficiente eseguire il cmdlet una volta, in qualsiasi server nel cluster. È possibile eseguirlo in locale in Windows PowerShell o usare Credential Security Service Provider (CredSSP) per l'esecuzione in modalità remota. A seconda della configurazione, potrebbe non essere in grado di eseguire questo cmdlet all'interno di Windows Admin Center.

### <a name="prepare-your-windows-server-2019-cluster-for-windows-admin-center"></a>Preparare il cluster di Windows Server 2019 per Windows Admin Center

Se il cluster esegua Windows Server 2019, i passaggi precedenti non sono più necessari. Aggiungere semplicemente il cluster in Windows Admin Center, come descritto nella sezione successiva e sei pronto per iniziare!

### <a name="configure-software-defined-networking-optional"></a>Configurare Software definito Networking (facoltativo) ###

È possibile configurare l'infrastruttura Hyper-Converged che esegue Windows Server 2016 o 2019 per usare funzionalità di rete SDN (Software Defined) con i passaggi seguenti:

1. Preparare il disco rigido virtuale del sistema operativo che è lo stesso sistema operativo è installato negli host infrastruttura iperconvergente. Questo disco rigido virtuale verrà utilizzato per tutte le macchine virtuali NC SLB/rete/gateway.
2. Scarica tutte le cartelle e file nella rete SDN Express dal [ https://github.com/Microsoft/SDN/tree/master/SDNExpress ](https://github.com/Microsoft/SDN/tree/master/SDNExpress).
3. Preparare una macchina virtuale diversa utilizzando la console di distribuzione. Questa macchina virtuale deve essere in grado di accedere gli host di rete SDN. Inoltre, la macchina virtuale deve essere disponibile lo strumento di amministrazione remota del server Hyper-V installato.
4. Copiare tutti gli elementi scaricati per SDN Express nella console di distribuzione della macchina virtuale. E condividere ciò **SDNExpress** cartella. Assicurarsi che tutti gli host possano accedere i **SDNExpress** condiviso cartella, come definito nella riga del file di configurazione 8:
   ```
    \\$env:Computername\SDNExpress
   ```
5. Copiare il file VHD del sistema operativo per il **immagini** cartella sotto il **SDNExpress** cartella nella console di distribuzione della macchina virtuale.
6. Modificare la configurazione di SDN Express con la configurazione dell'ambiente. Dopo aver modificato la configurazione di SDN Express sulla base di informazioni di ambiente, completare i due passaggi seguenti.
7. Eseguire PowerShell con privilegi di amministratore per distribuire SDN:

```powershell
    .\SDNExpress.ps1 -ConfigurationDataFile .\your_fabricconfig.PSD1 -verbose 
```

La distribuzione richiederà circa 30 a 45 minuti.

## <a name="get-started"></a>Informazioni di base

Dopo aver distribuito l'infrastruttura Hyper-Converged, è possibile gestire tramite Windows Admin Center.

### <a name="install-windows-admin-center"></a>Installare Windows Admin Center

Se già stato fatto, scaricare e installare Windows Admin Center. Il più veloce consentono di iniziare subito e in esecuzione è a installarlo nel computer Windows 10 e gestire i server in modalità remota. Ciò richiede meno di cinque minuti. [Scarica adesso](https://aka.ms/windowsadmincenter) oppure [ulteriori informazioni sulle altre opzioni di installazione](../deploy/install.md).

### <a name="add-hyper-converged-cluster"></a>Aggiungi Cluster Iperconvergente

Per aggiungere il cluster a Windows Admin Center:

1. Fare clic su **+ Aggiungi** in tutte le connessioni.
2. Scegliere di aggiungere un **connessione al Cluster Hyper-Converged**.
3. Digitare il nome del cluster e, se richiesto, le credenziali da utilizzare.
4. Fare clic su **Add** alla fine.

Il cluster verrà aggiunto all'elenco di connessioni. Fare clic per avviare il Dashboard.

![Aggiungi connessione cluster iperconvergente](../media/manage-hyper-converged/add-hyper-converged-cluster-connection.gif)

### <a name="add-sdn-enabled-hyper-converged-cluster-windows-admin-center-preview"></a>Aggiungere SDN Iperconvergente Cluster abilitato (anteprima Windows Admin Center)

L'anteprima più recente Windows Admin Center supporta la gestione di Software Defined Networking per Hyper-Converged infrastruttura. Tramite l'aggiunta di un URI REST di Controller di rete per la connessione cluster Iperconvergente, è possibile utilizzare Gestione Cluster iperconvergente per gestire le risorse SDN e monitorare l'infrastruttura SDN.

1. Fare clic su **+ Aggiungi** in tutte le connessioni.
2. Scegliere di aggiungere un **connessione al Cluster Hyper-Converged**.
3. Digitare il nome del cluster e, se richiesto, le credenziali da utilizzare.
4. Controllare **configurare il Controller di rete** per continuare.
5. Immettere il **URI di Controller di rete** e fare clic su **Validate**.
6. Fare clic su **Add** alla fine.

Il cluster verrà aggiunto all'elenco di connessioni. Fare clic per avviare il Dashboard.

![Aggiungi connessione cluster iperconvergente abilitato per SDN](../media/manage-hyper-converged/add-snd-enabled-hci-connection.png)

> [!Important]
> Gli ambienti di rete SDN con l'autenticazione Kerberos per la comunicazione di Northbound non sono attualmente supportati.

## <a name="frequently-asked-questions"></a>Domande frequenti

### <a name="are-there-differences-between-managing-windows-server-2016-and-windows-server-2019"></a>Esistono differenze tra la gestione di Windows Server 2016 e Windows Server 2019?

Sì. Windows Admin Center per l'infrastruttura Hyper-Converged riceve aggiornamenti frequenti che consentono di migliorare l'esperienza di Windows Server 2016 e Windows Server 2019. Tuttavia, alcune nuove funzionalità sono disponibili solo per Windows Server 2019 – ad esempio, l'opzione di attivazione/disattivazione per la deduplicazione e compressione.

### <a name="can-i-use-windows-admin-center-to-manage-storage-spaces-direct-for-other-use-cases-not-hyper-converged-such-as-converged-scale-out-file-server-sofs-or-microsoft-sql-server"></a>È possibile usare Windows Admin Center per gestire spazi di archiviazione diretta per gli altri casi d'uso (non iperconvergente), ad esempio convergente Scale-Out File Server di (scalabilità orizzontale SoFS) o Microsoft SQL Server?

Windows Admin Center Hyper-Converged infrastruttura non è incluso gestione o le opzioni di monitoraggio in modo specifico per gli altri casi d'uso di spazi di archiviazione diretta, ad esempio, è possibile creare condivisioni file. Tuttavia, le funzionalità di Dashboard e i core, ad esempio la creazione di volumi o sostituendo le unità, funzionano per qualsiasi cluster di spazi di archiviazione diretta.

### <a name="whats-the-difference-between-a-failover-cluster-and-a-hyper-converged-cluster"></a>Che cos'è la differenza tra un Cluster di Failover e un Cluster Hyper-Converged?

In generale, il termine "iperconvergente" si riferisce all'esecuzione di Hyper-V e spazi di archiviazione diretta nello stesso cluster i server di virtualizzazione delle risorse di calcolo e archiviazione. Nel contesto di Windows Admin Center, quando si fa clic **+ Aggiungi** dall'elenco delle connessioni, è possibile scegliere tra l'aggiunta di un **connessione Cluster di Failover** o un **Hyper-Converged Cluster connessione**:

- Il **connessione del Cluster di Failover** è il successore di app desktop gestione Cluster di Failover. Fornisce un'esperienza di gestione familiari, per utilizzo generico per tutti i cluster che supportano qualsiasi carico di lavoro, tra cui Microsoft SQL Server. È disponibile per Windows Server 2012 e versioni successive.

- Il **connessione Cluster Hyper-Converged** è un'esperienza completamente nuovo progettato appositamente per spazi di archiviazione diretta e Hyper-V. Include il dashboard e offre grafici e avvisi per il monitoraggio. È disponibile per Windows Server 2016 e Windows Server 2019.

### <a name="why-do-i-need-the-latest-cumulative-update-for-windows-server-2016"></a>Il motivo per cui è necessario l'aggiornamento cumulativo più recente per Windows Server 2016?

Windows Admin Center Hyper-Converged infrastruttura dipende da gestione che API ha sviluppate dopo il rilascio di Windows Server 2016. Queste API vengono aggiunti i [2018-05 aggiornamento cumulativo per Windows Server 2016 (KB4103723)](https://support.microsoft.com/help/4103723/windows-10-update-kb4103723), disponibile a partire da all'8 maggio 2018.

### <a name="how-much-does-it-cost-to-use-windows-admin-center"></a>Quanto costa utilizzare Windows Admin Center?

Windows Admin Center non comporta costi aggiuntivi oltre a Windows.

È possibile usare Windows Admin Center (disponibile come download separato) con licenze valide di Windows Server o Windows 10 senza costi aggiuntivi: si è concesso in licenza con un contratto di licenza Windows supplementare.

### <a name="does-windows-admin-center-require-system-center"></a>Windows Admin Center richiede System Center?

No.

### <a name="does-it-require-an-internet-connection"></a>Necessaria una connessione Internet?

No.

Anche se Windows Admin Center offre potenti e le caratteristiche di integrazione con cloud di Microsoft Azure, la base della gestione e l'esperienza di monitoraggio per l'infrastruttura Hyper-Converged è completamente in locale. Può essere installato e utilizzato senza una connessione Internet.

## <a name="things-to-try"></a>Possibili soluzioni

Se sta appena iniziando a usare, ecco alcune lezioni veloci per scoprire come Windows Admin Center per l'infrastruttura Hyper-Converged è organizzata e funziona. . Verificare l'uso del buon senso e prestare attenzione con gli ambienti di produzione. Questi video sono stati registrati con Windows Admin Center versione 1804 e una build Insider Preview di Windows Server 2019.

### <a name="manage-storage-spaces-direct-volumes"></a>Gestire i volumi di spazi di archiviazione diretta

<ul>
               <li>(0:37) <a href="https://youtu.be/o66etKq70N8">come creare un volume con mirroring a tre vie</a></li>
               <li>(1:17) <a href="https://youtu.be/R72QHudqWpE">come creare un volume con parità con accelerazione mirror</a></li>
               <li>(1:02) <a href="https://youtu.be/j59z7ulohs4">come aprire un volume e aggiungere i file</a></li>
               <li>(0:51) <a href="https://youtu.be/PRibTacyKko">come attivare la compressione e deduplicazione</a></li>
               <li>(0:47) <a href="https://youtu.be/hqyBzipBoTI">come espandere un volume</a></li>
               <li>(0:26) <a href="https://youtu.be/DbjF8r2F6Jo">come eliminare un volume</a></li>
</ul>

<table>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>Creare volumi, tre vie</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/o66etKq70N8" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>Creare la parità con accelerazione mirror volume</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/R72QHudqWpE" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>Aprire il volume e aggiungere i file</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/j59z7ulohs4" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>Attivare la deduplicazione e compressione</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/PRibTacyKko" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>Espandere i volumi</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/hqyBzipBoTI" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>Eliminazione del volume</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/DbjF8r2F6Jo" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
</table>

### <a name="create-a-new-virtual-machine"></a>Creare una nuova macchina virtuale

1. Fare clic sui **macchine virtuali** strumento dal riquadro di spostamento sinistra.
2. Nella parte superiore dello strumento di macchine virtuali, scegliere il **inventario** tab, quindi fare clic su **New** per creare una nuova macchina virtuale.
3. Immettere il nome della macchina virtuale e scegliere tra le macchine virtuali di generazione 1 e 2.
4. È quindi possibile scegliere quali host in cui creare la macchina virtuale in inizialmente oppure usare l'host consigliato.
5. Scegliere un percorso per i file della macchina virtuale. Scegliere un volume nell'elenco a discesa oppure fare clic su **esplorare** per scegliere una cartella utilizzando il selettore di cartella. Il file di configurazione macchina virtuale e il file di disco rigido virtuale verrà salvati in una singola cartella sotto la `\Hyper-V\[virtual machine name]` percorso del volume selezionato o del percorso.
6. Scegliere il numero di processori virtuali, se si desidera che la virtualizzazione annidata abilitata, configurare le impostazioni della memoria, schede di rete, dischi rigidi virtuali e scegliere se si desidera installare un sistema operativo da un file di immagine ISO o dalla rete.
7. Fare clic su **Crea** per creare la macchina virtuale.
8. Una volta che la macchina virtuale viene creata e visualizzato nell'elenco di macchine virtuali, è possibile avviare la macchina virtuale.
9. Una volta che viene avviata la macchina virtuale, è possibile connettersi alla console della macchina virtuale tramite VMConnect per installare il sistema operativo. Selezionare la macchina virtuale dall'elenco, fare clic su **altre** > **Connect** per scaricare il file con estensione rdp. Nell'app connessione Desktop remoto, aprire il file con estensione rdp. Poiché questo si connette alla console della macchina virtuale, sarà necessario immettere le credenziali di amministratore dell'host Hyper-V.

[Altre informazioni sulla gestione delle macchine virtuali con Windows Admin Center](manage-virtual-machines.md).

### <a name="pause-and-safely-restart-a-server"></a>Sospendere e riavviare in modo sicuro un server

1. Dal **Dashboard**, selezionare **server** dalla barra di spostamento a sinistra o facendo clic la **Visualizza server >** collegamento nel riquadro in basso a destra del Dashboard .
2. Nella parte superiore, passare dalla **Summary** per il **inventario** scheda.
3. Selezionare un server facendo clic sul relativo nome per aprire la **Server** pagina dei dettagli.
4. Fare clic su **sospendere server per la manutenzione**. Se è sicuro procedere, si sposterà macchine virtuali in altri server nel cluster. Il server avranno lo stato allo svuotamento mentre accade ciò. Se si desidera, è possibile guardare le macchine virtuali passare il **macchine virtuali > inventario** pagina, in cui i server host viene illustrato chiaramente nella griglia. Quando si sono spostati tutte le macchine virtuali, lo stato del server saranno **Paused**.
5. Fare clic su **Gestisci server** per accedere a tutti gli strumenti di gestione per server Windows Admin Center.
6. Fare clic su **riavviare**, quindi **Yes**. È possibile essere avviata nuovamente all'elenco delle connessioni.
7. Nella **Dashboard**, il server viene visualizzato in rosso quando esso è inattivo.
8. Una volta eseguire il backup, passare di nuovo la **Server** e fare clic su **server Resume dalla manutenzione** per impostare lo stato del server a semplicemente aggiornato. Nel tempo, le macchine virtuali verranno riportare – non è richiesto alcun intervento dell'utente.

### <a name="replace-a-failed-drive"></a>Sostituire un'unità non riuscita

1. Quando un'unità ha esito negativo, viene visualizzato un avviso in alto a sinistra **Alerts** area della **Dashboard**.
2. È anche possibile selezionare **unità** dalla barra di spostamento sul lato sinistro o fare clic sui **unità di visualizzazione >** collegamento nel riquadro nell'angolo inferiore destro per esplorare le unità e visualizzarne lo stato per se stessi. Nel **inventario** scheda griglia supporta l'ordinamento, raggruppamento e ricerca per parole chiave.
3. Dal **Dashboard**, fare clic sull'avviso per visualizzare i dettagli, quali la posizione fisica dell'unità.
4. Per altre informazioni, fare clic sui **passare all'unità** scelta rapida per il **unità** pagina dei dettagli.
5. Se l'hardware supporta, è possibile fare clic su **light di attivare/disattivare** per controllare la spia dell'unità.
6. Spazi di archiviazione diretta automaticamente ritira ed evacuates unità non riuscita. Quando ciò si è verificato, lo stato dell'unità è stato ritirato e la barra di capacità di archiviazione è vuota.
7. Rimuovere il disco guasto e inserire relativa sostituzione.
8. Nelle **unità > inventario**, verrà visualizzata la nuova unità. Nel tempo, l'avviso verrà cancellato, tornare allo stato OK, in grado di ripristinare i volumi e archiviazione bilancerà nuovamente nel nuovo disco, è necessaria alcuna azione utente.

### <a name="manage-virtual-networks-sdn-enabled-hci-clusters-using-windows-admin-center-preview"></a>Gestire le reti virtuali (cluster uomo abilitati SDN usando Windows Admin Center anteprima)

1. Selezionare **reti virtuali** dalla barra di spostamento a sinistra.
2. Fare clic su **New** per creare una nuova rete virtuale e subnet, o scegliere una rete virtuale esistente e fare clic su **impostazioni** per modificarne la configurazione.
3. Fare clic su una rete virtuale esistente per visualizzare le connessioni della macchina virtuale per la subnet della rete virtuale e accedere a elenchi di controllo applicati alla subnet della rete virtuale.

![Gestire le reti virtuali](../media/manage-hyper-converged/manage-virtual-networks.png)

### <a name="connect-a-virtual-machine-to-a-virtual-network-sdn-enabled-hci-clusters-using-windows-admin-center-preview"></a>Connettere una macchina virtuale a una rete virtuale (cluster uomo abilitati SDN usando Windows Admin Center anteprima)

1. Selezionare **macchine virtuali** dalla barra di spostamento a sinistra.
2. Scegliere una macchina virtuale esistente > fare clic su **le impostazioni** > aprire le **reti** scheda **impostazioni**.
3. Configurare il **rete virtuale** e **Subnet virtuale** campi per la connessione alla macchina virtuale a una rete virtuale.

È anche possibile configurare la rete virtuale durante la creazione di una macchina virtuale.

![Connettere una macchina virtuale a una rete virtuale](../media/manage-hyper-converged/connect-vm-to-virtual-network.png)

### <a name="monitor-software-defined-networking-infrastructure-sdn-enabled-hci-clusters-using-windows-admin-center-preview"></a>Infrastruttura Software Defined Networking monitoraggio (cluster uomo abilitati SDN usando Windows Admin Center anteprima)

1. Selezionare **SDN monitoraggio** dalla barra di spostamento a sinistra.
2. Visualizzare informazioni dettagliate sull'integrità del Gateway virtuale Controller di rete, bilanciamento del carico Software e il monitoraggio dell'utilizzo di Pool di Gateway virtuale, pubblico e il Pool di indirizzi IP privati e stato dell'host SDN.

![Monitorare l'infrastruttura SDN](../media/manage-hyper-converged/sdn-monitoring.png)

## <a name="feedback"></a>Feedback

Si tratta di tutti i tuoi commenti! Il vantaggio più importante di aggiornamenti frequenti è ciò che funziona e cosa deve essere migliorato. Ecco alcuni modi per segnalarci cosa stai pensando:

- [Inviare e votare le richieste di funzionalità su UserVoice](https://windowsserver.uservoice.com/forums/295071/category/319162?query=%5Bhci%5D)
- [Aggiungere i forum di Windows Admin Center su Microsoft Tech Community](https://techcommunity.microsoft.com/t5/Windows-Server-Management/bd-p/WindowsServerManagement)
- TWEET a `@servermgmt`

### <a name="see-also"></a>Vedere anche

- [Windows Admin Center](../understand/windows-admin-center.md)
- [Spazi di archiviazione diretta](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview)
- [Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-on-windows-server)
- [SDN (Software Defined Networking)](https://docs.microsoft.com/windows-server/networking/sdn/software-defined-networking)
