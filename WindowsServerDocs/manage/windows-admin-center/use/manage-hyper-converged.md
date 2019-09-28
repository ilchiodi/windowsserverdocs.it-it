---
title: Gestire l'infrastruttura iperconvergente con l'interfaccia di amministrazione di Windows
description: Gestire l'infrastruttura iperconvergente con l'interfaccia di amministrazione di Windows (Project Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 03/01/2019
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: d692251e1ba0fef43e4eeee6f259f26f4347f3c0
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71356878"
---
# <a name="manage-hyper-converged-infrastructure-with-windows-admin-center"></a>Gestire l'infrastruttura iperconvergente con l'interfaccia di amministrazione di Windows

>Si applica a: Windows Admin Center, Windows Admin Center Preview

## <a name="what-is-hyper-converged-infrastructure"></a>Che cos'è l'infrastruttura iperconvergente

L'infrastruttura iperconvergente consolida le risorse di calcolo, archiviazione e rete definite dal software in un unico cluster per offrire una virtualizzazione ad alte prestazioni, economica e facilmente scalabile. Questa funzionalità è stata introdotta in Windows Server 2016 con [spazi di archiviazione diretta](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview), [Software Defined Networking](https://docs.microsoft.com/windows-server/networking/sdn/software-defined-networking) e [Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-on-windows-server).

> [!Tip]
> Si vuole acquisire un'infrastruttura iperconvergente? Microsoft consiglia le soluzioni [software-defined di Windows Server](https://microsoft.com/wssd) dei partner. Sono progettati, assemblati e convalidati in base all'architettura di riferimento per garantire la compatibilità e l'affidabilità, per poter essere operativi rapidamente.

> [!IMPORTANT]
> Alcune delle funzionalità descritte in questo articolo sono disponibili solo nell'anteprima dell'interfaccia di amministrazione di Windows. [Ricerca per categorie ottenere questa versione?](http://aka.ms/windowsadmincenter)

## <a name="what-is-windows-admin-center"></a>Che cos'è Windows Admin Center

L'interfaccia di [amministrazione di Windows](../understand/windows-admin-center.md) è lo strumento di gestione di prossima generazione per Windows Server, il successore per gli strumenti "in-box" tradizionali come server Manager. È gratuito e può essere installato e usato senza una connessione Internet. È possibile usare l'interfaccia di amministrazione di Windows per gestire e monitorare l'infrastruttura iperconvergente che esegue Windows Server 2016 o Windows Server 2019.

![Dashboard del cluster iperconvergente](../media/manage-hyper-converged/hci-dashboard-v1809.png)

## <a name="key-features"></a>Funzionalità principali

Le caratteristiche principali dell'interfaccia di amministrazione di Windows per l'infrastruttura iperconvergente includono:

- **Singolo riquadro unificato per il calcolo, l'archiviazione e la rete presto disponibili.** Visualizza le tue macchine virtuali, i server host, i volumi, le unità e altro ancora all'interno di un'esperienza integrata, coerente e interconnessa.
- **Creare e gestire spazi di archiviazione e macchine virtuali Hyper-V.** Flussi di lavoro radicalmente semplici per la creazione, l'apertura, il ridimensionamento e l'eliminazione di volumi; e creare, avviare, connettersi e spostare le macchine virtuali; e molto altro ancora.
- **Potente monitoraggio a livello di cluster.** Il dashboard rappresenta l'utilizzo della memoria e della CPU, la capacità di archiviazione, gli IOPS, la velocità effettiva e la latenza in tempo reale, in ogni server del cluster, con avvisi chiari quando qualcosa non è adatto.
- **Supporto di SDN (Software Defined Networking).** Gestisci e monitora reti virtuali, subnet, Connetti macchine virtuali alle reti virtuali e monitora l'infrastruttura SDN.

Il centro di amministrazione di Windows per l'infrastruttura iperconvergente viene attivamente sviluppato da Microsoft. Riceve aggiornamenti frequenti che consentono di migliorare le funzionalità esistenti e aggiungere nuove funzionalità.

## <a name="before-you-start"></a>Prima di iniziare

Per gestire il cluster come infrastruttura iperconvergente nell'interfaccia di amministrazione di Windows, è necessario che esegua Windows Server 2016 o Windows Server 2019 e che Hyper-V e Spazi di archiviazione diretta siano abilitati. Facoltativamente, è anche possibile abilitare e gestire il Software Defined Networking tramite l'interfaccia di amministrazione di Windows.

> [!Tip]
> L'interfaccia di amministrazione di Windows offre anche un'esperienza di gestione generica per tutti i cluster che supportano qualsiasi carico di lavoro, disponibile per Windows Server 2012 e versioni successive. Se questo aspetto è più adatto, quando si aggiunge il cluster all'interfaccia di amministrazione di Windows, selezionare [**cluster di failover**](manage-failover-clusters.md) anziché **cluster iperconvergente**.

### <a name="prepare-your-windows-server-2016-cluster-for-windows-admin-center"></a>Preparare il cluster Windows Server 2016 per l'interfaccia di amministrazione di Windows

L'interfaccia di amministrazione di Windows per l'infrastruttura iperconvergente dipende dalle API di Gestione aggiunte dopo il rilascio di Windows Server 2016. Prima di poter gestire il cluster Windows Server 2016 con l'interfaccia di amministrazione di Windows, è necessario eseguire i due passaggi seguenti:

1. Verificare che in ogni server del cluster sia installato l' [aggiornamento cumulativo 2018-05 per Windows server 2016 (KB4103723)](https://support.microsoft.com/help/4103723/windows-10-update-kb4103723) o versione successiva. Per scaricare e installare questo aggiornamento, passare a **Impostazioni** > **Aggiorna & sicurezza** > **Windows Update** e selezionare **Controlla aggiornamenti online da Microsoft Update**.
2. Eseguire il cmdlet di PowerShell seguente come amministratore nel cluster:

```powershell
    Add-ClusterResourceType -Name "SDDC Management" -dll "$env:SystemRoot\Cluster\sddcres.dll" -DisplayName "SDDC Management"
```

> [!Tip]
> È sufficiente eseguire il cmdlet una sola volta, in tutti i server del cluster. È possibile eseguirlo localmente in Windows PowerShell o usare Credential Security Service Provider (CredSSP) per eseguirlo in modalità remota. A seconda della configurazione, potrebbe non essere possibile eseguire questo cmdlet dall'interfaccia di amministrazione di Windows.

### <a name="prepare-your-windows-server-2019-cluster-for-windows-admin-center"></a>Preparare il cluster Windows Server 2019 per l'interfaccia di amministrazione di Windows

Se il cluster esegue Windows Server 2019, i passaggi precedenti non sono necessari. È sufficiente aggiungere il cluster all'interfaccia di amministrazione di Windows, come descritto nella sezione successiva, per iniziare.

### <a name="configure-software-defined-networking-optional"></a>Configurare Software Defined Networking (facoltativo) ###

È possibile configurare l'infrastruttura iperconvergente che esegue Windows Server 2016 o 2019 per l'uso di SDN (Software Defined Networking) con i passaggi seguenti:

1. Preparare il disco rigido virtuale del sistema operativo che è lo stesso sistema operativo installato negli host di infrastruttura iperconvergenti. Questo VHD verrà usato per tutte le macchine virtuali NC/SLB/GW.
2. Scaricare tutte le cartelle e i file in SDN Express [https://github.com/Microsoft/SDN/tree/master/SDNExpress](https://github.com/Microsoft/SDN/tree/master/SDNExpress)da.
3. Preparare una macchina virtuale diversa usando la console di distribuzione. Questa macchina virtuale dovrebbe essere in grado di accedere agli host SDN. Inoltre, è necessario che nella macchina virtuale sia installato lo strumento Hyper-V di strumenti di amministrazione remota del server.
4. Copiare tutti gli elementi scaricati per SDN Express nella VM della console di distribuzione. E condividono la cartella **SDNExpress** . Verificare che ogni host possa accedere alla cartella condivisa **SDNExpress** , come definito nel file di configurazione riga 8:
   ```
    \\$env:Computername\SDNExpress
   ```
5. Copiare il disco rigido virtuale del sistema operativo nella cartella **Immagini** nella cartella **SDNExpress** della VM della console di distribuzione.
6. Modificare la configurazione SDN Express con la configurazione dell'ambiente. Completare i due passaggi seguenti dopo aver modificato la configurazione SDN Express in base alle informazioni sull'ambiente.
7. Eseguire PowerShell con privilegi di amministratore per distribuire SDN:

```powershell
    .\SDNExpress.ps1 -ConfigurationDataFile .\your_fabricconfig.PSD1 -verbose 
```

La distribuzione può richiedere circa 30 – 45 minuti.

## <a name="get-started"></a>Attività iniziali

Una volta distribuita l'infrastruttura iperconvergente, è possibile gestirla usando l'interfaccia di amministrazione di Windows.

### <a name="install-windows-admin-center"></a>Installare Windows Admin Center

Se non è già stato fatto, scaricare e installare l'interfaccia di amministrazione di Windows. Il modo più rapido per iniziare a eseguire è installarlo nel computer Windows 10 e gestire i server in modalità remota. Questa operazione richiede meno di cinque minuti. [Scarica adesso](https://aka.ms/windowsadmincenter) o [Scopri di più sulle altre opzioni di installazione](../deploy/install.md).

### <a name="add-hyper-converged-cluster"></a>Aggiungi cluster iperconvergente

Per aggiungere il cluster al centro di amministrazione di Windows:

1. Fare clic su **+ Aggiungi** in tutte le connessioni.
2. Scegliere di aggiungere una **connessione cluster iperconvergente**.
3. Digitare il nome del cluster e, se richiesto, le credenziali da usare.
4. Fare clic su **Aggiungi** per terminare.

Il cluster verrà aggiunto all'elenco di connessioni. Fare clic su di esso per avviare il dashboard.

![Aggiungi connessione cluster iperconvergente](../media/manage-hyper-converged/add-hyper-converged-cluster-connection.gif)

### <a name="add-sdn-enabled-hyper-converged-cluster-windows-admin-center-preview"></a>Aggiungere un cluster iperconvergente abilitato per SDN (anteprima dell'interfaccia di amministrazione di Windows)

La versione più recente dell'anteprima dell'interfaccia di amministrazione di Windows supporta la gestione di Software Defined Networking per l'infrastruttura iperconvergente. Aggiungendo un URI REST del controller di rete alla connessione cluster iperconvergente, è possibile usare Gestione cluster iperconvergente per gestire le risorse SDN e monitorare l'infrastruttura SDN.

1. Fare clic su **+ Aggiungi** in tutte le connessioni.
2. Scegliere di aggiungere una **connessione cluster iperconvergente**.
3. Digitare il nome del cluster e, se richiesto, le credenziali da usare.
4. Selezionare **Configura il controller di rete** per continuare.
5. Immettere l' **URI del controller di rete** e fare clic su **convalida**.
6. Fare clic su **Aggiungi** per terminare.

Il cluster verrà aggiunto all'elenco di connessioni. Fare clic su di esso per avviare il dashboard.

![Aggiungere una connessione cluster iperconvergente abilitata per SDN](../media/manage-hyper-converged/add-snd-enabled-hci-connection.png)

> [!Important]
> Gli ambienti SDN con autenticazione Kerberos per la comunicazione verso il Nord non sono attualmente supportati.

## <a name="frequently-asked-questions"></a>Domande frequenti

### <a name="are-there-differences-between-managing-windows-server-2016-and-windows-server-2019"></a>Esistono differenze tra la gestione di Windows Server 2016 e Windows Server 2019?

Sì. L'interfaccia di amministrazione di Windows per l'infrastruttura iperconvergente riceve aggiornamenti frequenti che consentono di migliorare l'esperienza per Windows Server 2016 e Windows Server 2019. Tuttavia, alcune nuove funzionalità sono disponibili solo per Windows Server 2019, ad esempio l'interruttore per la deduplicazione e la compressione.

### <a name="can-i-use-windows-admin-center-to-manage-storage-spaces-direct-for-other-use-cases-not-hyper-converged-such-as-converged-scale-out-file-server-sofs-or-microsoft-sql-server"></a>È possibile usare l'interfaccia di amministrazione di Windows per gestire Spazi di archiviazione diretta per altri casi d'uso (non iperconvergenti), ad esempio File server di scalabilità orizzontale convergenti (SoFS) o Microsoft SQL Server?

L'interfaccia di amministrazione di Windows per l'infrastruttura iperconvergente non fornisce opzioni di gestione o monitoraggio specifiche per altri casi d'uso di Spazi di archiviazione diretta, ad esempio non è possibile creare condivisioni file. Tuttavia, il dashboard e le funzionalità di base, ad esempio la creazione di volumi o la sostituzione di unità, funzionano per qualsiasi cluster Spazi di archiviazione diretta.

### <a name="whats-the-difference-between-a-failover-cluster-and-a-hyper-converged-cluster"></a>Qual è la differenza tra un cluster di failover e un cluster iperconvergente?

In generale, il termine "iperconvergente" si riferisce all'esecuzione di Hyper-V e Spazi di archiviazione diretta negli stessi server del cluster per la virtualizzazione delle risorse di calcolo e di archiviazione. Nel contesto dell'interfaccia di amministrazione di Windows, quando si fa clic su **+ Aggiungi** nell'elenco connessioni, è possibile scegliere tra aggiungere una **connessione cluster di failover** o una **connessione cluster iperconvergente**:

- La **connessione del cluster di failover** è il successore dell'app desktop gestione cluster di failover. Offre un'esperienza di gestione familiare e per utilizzo generico per tutti i cluster che supportano qualsiasi carico di lavoro, inclusi Microsoft SQL Server. È disponibile per Windows Server 2012 e versioni successive.

- La **connessione cluster iperconvergente** è una nuova esperienza basata su spazi di archiviazione diretta e Hyper-V. Include il dashboard e offre grafici e avvisi per il monitoraggio. È disponibile per Windows Server 2016 e Windows Server 2019.

### <a name="why-do-i-need-the-latest-cumulative-update-for-windows-server-2016"></a>Perché è necessario l'aggiornamento cumulativo più recente per Windows Server 2016?

L'interfaccia di amministrazione di Windows per l'infrastruttura iperconvergente dipende dalle API di gestione sviluppate dopo il rilascio di Windows Server 2016. Queste API vengono aggiunte nell' [aggiornamento cumulativo 2018-05 per Windows Server 2016 (KB4103723)](https://support.microsoft.com/help/4103723/windows-10-update-kb4103723), disponibile a partire dall'8 maggio 2018.

### <a name="how-much-does-it-cost-to-use-windows-admin-center"></a>Quanto costa utilizzare Windows Admin Center?

Windows Admin Center non comporta costi aggiuntivi oltre a Windows.

Puoi usare Windows Admin Center (disponibile come download separato) con licenze valide di Windows Server o Windows 10 senza alcun costo aggiuntivo: viene concesso in licenza nel quadro di condizioni di licenza integrative di Windows.

### <a name="does-windows-admin-center-require-system-center"></a>Windows Admin Center richiede System Center?

No.

### <a name="does-it-require-an-internet-connection"></a>È necessaria una connessione Internet?

No.

Anche se l'interfaccia di amministrazione di Windows offre un'integrazione potente e comoda con la Microsoft Azure cloud, l'esperienza di gestione e monitoraggio di base per l'infrastruttura iperconvergente è completamente in locale. Può essere installato e usato senza una connessione Internet.

## <a name="things-to-try"></a>Elementi da provare

Se si è appena iniziato, di seguito sono riportate alcune esercitazioni rapide che consentono di apprendere come organizzare e funzionare il centro di amministrazione di Windows per l'infrastruttura iperconvergente. Si consiglia di valutare con attenzione gli ambienti di produzione. Questi video sono stati registrati con l'interfaccia di amministrazione di Windows versione 1804 e una build di anteprima Insider di Windows Server 2019.

### <a name="manage-storage-spaces-direct-volumes"></a>Gestire volumi Spazi di archiviazione diretta

<ul>
               <li>(0:37) <a href="https://youtu.be/o66etKq70N8">come creare un volume mirror a tre vie</a></li>
               <li>(1:17) <a href="https://youtu.be/R72QHudqWpE">come creare un volume di parità con accelerazione con mirroring</a></li>
               <li>(1:02) <a href="https://youtu.be/j59z7ulohs4">come aprire un volume e aggiungere file</a></li>
               <li>(0:51) <a href="https://youtu.be/PRibTacyKko">come attivare la deduplicazione e la compressione</a></li>
               <li>(0:47) <a href="https://youtu.be/hqyBzipBoTI">come espandere un volume</a></li>
               <li>(0:26) <a href="https://youtu.be/DbjF8r2F6Jo">come eliminare un volume</a></li>
</ul>

<table>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>Crea volume, mirroring a tre vie</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/o66etKq70N8" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>Crea volume, parità con accelerazione con mirroring</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/R72QHudqWpE" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>Apri volume e Aggiungi file</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/j59z7ulohs4" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>Attivare la deduplicazione e la compressione</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/PRibTacyKko" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>Espandi volume</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/hqyBzipBoTI" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>Elimina volume</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/DbjF8r2F6Jo" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
</table>

### <a name="create-a-new-virtual-machine"></a>Creare una nuova macchina virtuale

1. Fare clic sullo strumento **macchine virtuali** nel riquadro di spostamento a sinistra.
2. Nella parte superiore dello strumento macchine virtuali scegliere la scheda **inventario** , quindi fare clic su **nuova** per creare una nuova macchina virtuale.
3. Immettere il nome della macchina virtuale e scegliere tra le macchine virtuali di prima e seconda generazione.
4. È può quindi scegliere quale host creare inizialmente la macchina virtuale o usare l'host consigliato.
5. Scegliere un percorso per i file della macchina virtuale. Scegliere un volume dall'elenco a discesa o fare clic su **Sfoglia** per scegliere una cartella tramite la selezione cartelle. I file di configurazione della macchina virtuale e il file del disco rigido virtuale verranno salvati in una singola `\Hyper-V\[virtual machine name]` cartella nel percorso del volume o del percorso selezionato.
6. Scegliere il numero di processori virtuali, se si desidera abilitare la virtualizzazione annidata, configurare le impostazioni della memoria, le schede di rete, i dischi rigidi virtuali e scegliere se installare un sistema operativo da un file di immagine ISO o dalla rete.
7. Fare clic su **Crea** per creare la macchina virtuale.
8. Dopo aver creato la macchina virtuale e averla visualizzata nell'elenco delle macchine virtuali, è possibile avviare la macchina virtuale.
9. Una volta avviata la macchina virtuale, è possibile connettersi alla console della macchina virtuale tramite VMConnect per installare il sistema operativo. Selezionare la macchina virtuale dall'elenco e fare clic su **altro** > **Connetti** per scaricare il file con estensione RDP. Aprire il file con estensione RDP nell'app Connessione Desktop remoto. Poiché si sta effettuando la connessione alla console della macchina virtuale, sarà necessario immettere le credenziali di amministratore dell'host Hyper-V.

[Altre informazioni sulla gestione delle macchine virtuali con l'interfaccia di amministrazione di Windows](manage-virtual-machines.md).

### <a name="pause-and-safely-restart-a-server"></a>Sospendere e riavviare in modo sicuro un server

1. Dal **Dashboard**selezionare **Server** dalla finestra di spostamento sul lato sinistro o facendo clic sul collegamento **Visualizza server >** nel riquadro nell'angolo inferiore destro del dashboard.
2. Nella parte superiore passare da **Riepilogo** alla scheda **inventario** .
3. Selezionare un server facendo clic sul relativo nome per aprire la pagina dei dettagli del **Server** .
4. Fare clic su **Sospendi server per manutenzione**. Se è possibile procedere in modo sicuro, le macchine virtuali vengono spostate in altri server del cluster. Il server avrà lo stato di svuotamento quando si verifica questo problema. Se lo si desidera, è possibile osservare lo spostamento delle macchine virtuali nella pagina delle **macchine virtuali > inventario** , in cui il server host viene visualizzato chiaramente nella griglia. Quando tutte le macchine virtuali sono state spostate, lo stato del server viene **sospeso**.
5. Fare clic su **Gestisci server** per accedere a tutti gli strumenti di gestione per server nell'interfaccia di amministrazione di Windows.
6. Fare clic su **Riavvia**, quindi su **Sì**. Verrà eseguito nuovamente l'elenco delle connessioni.
7. Tornando al **Dashboard**, il server è colorato in rosso mentre è inattivo.
8. Una volta eseguito il backup, tornare alla pagina del **Server** e fare clic su **Riprendi server da manutenzione** per impostare lo stato del server su semplicemente. Nel tempo, le macchine virtuali si sposteranno indietro. non è richiesto alcun intervento da parte dell'utente.

### <a name="replace-a-failed-drive"></a>Sostituire un'unità non riuscita

1. Quando un'unità non riesce, viene visualizzato un avviso nell'area **avvisi** in alto a sinistra del **Dashboard**.
2. È anche possibile selezionare le **unità** dall'esplorazione sul lato sinistro oppure fare clic sul collegamento **Visualizza unità >** nel riquadro nell'angolo inferiore destro per visualizzare le unità e visualizzarne lo stato. Nella scheda **inventario** la griglia supporta l'ordinamento, il raggruppamento e la ricerca di parole chiave.
3. Dal **Dashboard**fare clic sull'avviso per visualizzare i dettagli, ad esempio la posizione fisica dell'unità.
4. Per ulteriori informazioni, fare clic sul collegamento **Vai a unità** alla pagina dei dettagli dell' **unità** .
5. Se l'hardware lo supporta, è possibile fare clic **su Attiva/disattiva luce** per controllare la luce dell'indicatore dell'unità.
6. Spazi di archiviazione diretta ritira automaticamente e evacua le unità non riuscite. Quando si è verificato questo problema, lo stato dell'unità viene ritirato e la relativa barra di capacità di archiviazione è vuota.
7. Rimuovere l'unità non riuscita e inserire la relativa sostituzione.
8. In **unità > inventario**verrà visualizzata la nuova unità. Nell'intervallo di tempo l'avviso verrà cancellato, i volumi ripristineranno lo stato OK e lo spazio di archiviazione ribilanciarà la nuova unità. non è richiesta alcuna azione da parte dell'utente.

### <a name="manage-virtual-networks-sdn-enabled-hci-clusters-using-windows-admin-center-preview"></a>Gestire le reti virtuali (cluster HCI abilitati per SDN con l'anteprima dell'interfaccia di amministrazione di Windows)

1. Selezionare **reti virtuali** dalla finestra di spostamento sul lato sinistro.
2. Fare clic su **nuovo** per creare una nuova rete virtuale e subnet oppure scegliere una rete virtuale esistente e fare clic su **Impostazioni** per modificarne la configurazione.
3. Fare clic su una rete virtuale esistente per visualizzare le connessioni VM alle subnet della rete virtuale e agli elenchi di controllo di accesso applicati alle subnet della rete virtuale.

![Gestire le reti virtuali](../media/manage-hyper-converged/manage-virtual-networks.png)

### <a name="connect-a-virtual-machine-to-a-virtual-network-sdn-enabled-hci-clusters-using-windows-admin-center-preview"></a>Connettere una macchina virtuale a una rete virtuale (cluster HCI abilitati per SDN con l'anteprima dell'interfaccia di amministrazione di Windows)

1. Selezionare **macchine virtuali** dalla finestra di spostamento sul lato sinistro.
2. Scegliere una macchina virtuale esistente > fare clic su **impostazioni** > aprire la scheda **reti** in **Impostazioni**.
3. Configurare la **rete virtuale** e i campi della **subnet virtuale** per connettere la macchina virtuale a una rete virtuale.

È anche possibile configurare la rete virtuale quando si crea una macchina virtuale.

![Connettere una macchina virtuale a una rete virtuale](../media/manage-hyper-converged/connect-vm-to-virtual-network.png)

### <a name="monitor-software-defined-networking-infrastructure-sdn-enabled-hci-clusters-using-windows-admin-center-preview"></a>Monitorare l'infrastruttura software defined Networking (i cluster HCI abilitati per SDN con l'anteprima dell'interfaccia di amministrazione di Windows)

1. Selezionare **Sdn Monitoring (monitoraggio Sdn** ) dallo spostamento sul lato sinistro.
2. Visualizzare informazioni dettagliate sull'integrità del controller di rete, Load Balancer software, gateway virtuale e monitorare il pool di gateway virtuali, l'utilizzo del pool IP pubblico e privato e lo stato dell'host SDN.

![Monitorare l'infrastruttura SDN](../media/manage-hyper-converged/sdn-monitoring.png)

## <a name="feedback"></a>Feedback

Sono tutti i tuoi commenti e suggerimenti! Il vantaggio più importante degli aggiornamenti frequenti è quello di sapere cosa funziona e cosa è necessario migliorare. Ecco alcuni modi per farci sapere cosa si sta pensando:

- [Inviare e votare le richieste di funzionalità in UserVoice](https://windowsserver.uservoice.com/forums/295071/category/319162?query=%5Bhci%5D)
- [Partecipa al forum di Windows Admin Center su Microsoft Tech community](https://techcommunity.microsoft.com/t5/Windows-Server-Management/bd-p/WindowsServerManagement)
- Invia un tweet a`@servermgmt`

### <a name="see-also"></a>Vedere anche

- [Windows Admin Center](../understand/windows-admin-center.md)
- [Spazi di archiviazione diretta](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview)
- [Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-on-windows-server)
- [SDN (Software Defined Networking)](https://docs.microsoft.com/windows-server/networking/sdn/software-defined-networking)
