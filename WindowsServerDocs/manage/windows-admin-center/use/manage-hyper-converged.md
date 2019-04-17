---
title: Gestire l'infrastruttura Iperconvergente con Windows Admin Center
description: Gestire l'infrastruttura Iperconvergente con Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: daniellee-msft
ms.author: jol
ms.date: 03/01/2019
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 9ce4381735746ace6358aa2cb30f8b341c576054
ms.sourcegitcommit: e558dda2774345e9ad17ff04b759f68c59d88139
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2019
ms.locfileid: "9262844"
---
# Gestire l'infrastruttura Iperconvergente con Windows Admin Center

>Si applica a: Windows Admin Center, Windows Admin Center Preview

## Che cos'è l'infrastruttura iperconvergente

Infrastruttura Iperconvergente consolida software-defined elaborazione, archiviazione e reti in un cluster per fornire ad alte prestazioni, conveniente e la virtualizzazione facilmente scalabile. Questa funzionalità è stata introdotta in Windows Server 2016 con [Spazi di archiviazione diretta](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview), [Software Defined Networking](https://docs.microsoft.com/en-us/windows-server/networking/sdn/software-defined-networking) e [Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-on-windows-server).

> [!Tip]
> Stai cercando di acquisire l'infrastruttura iperconvergente? Microsoft consiglia di queste soluzioni [Windows Server Software-Defined](https://microsoft.com/wssd) dai nostri partner. Sono progettate, assemblati e convalidati in base al nostro architettura di riferimento per garantire la compatibilità e l'affidabilità, in modo che ottenere fino ed eseguire rapidamente.

> [!IMPORTANT]
> Alcune delle funzionalità descritte in questo articolo sono disponibili solo in Windows Admin Center Preview. [Come posso ottenere questa versione?](http://aka.ms/windowsadmincenter)

## Che cos'è Windows Admin Center

[Windows Admin Center](../understand/windows-admin-center.md) è lo strumento di gestione di prossima generazione per Windows Server, il successore di strumenti "inclusi" tradizionali, come Server Manager. È gratuita e può essere installato e usato senza una connessione Internet. È possibile utilizzare Windows Admin Center per gestire e monitorare l'infrastruttura iperconvergente che esegue Windows Server 2016 o Windows Server 2019.

![Dashboard cluster iperconvergente](../media/manage-hyper-converged/hci-dashboard-v1809.png)

## Funzionalità principali

Delle caratteristiche principali di Windows Admin Center per l'infrastruttura iperconvergente includono:

- **Unificato singolo riquadro-di-vetro per calcolo, archiviazione e rete presto.** Visualizzare le macchine virtuali, i server host, i volumi, le unità e informazioni all'interno di un' coerente esperienza specificatamente progettata, coerenza e interconnessa.
- **Creare e gestire le macchine virtuali Hyper-V e spazi di archiviazione.** Flussi di lavoro radicalmente rispetto ai semplici per creare, Apri, ridimensionare ed eliminare i volumi. creare, avviare, connettersi a e spostare le macchine virtuali; e molto altro ancora.
- **Monitoraggio potente a livello di cluster.** Il dashboard grafici della memoria e utilizzo della CPU, capacità di archiviazione, IOPS, throughput e latenza in tempo reale, tra tutti i server del cluster, con gli avvisi chiari quando un elemento non è corretto.
- **Supporto di software Defined Networking (SDN).** Gestire e monitorare le reti virtuali, subnet, connettere le macchine virtuali alle reti virtuali e monitorare l'infrastruttura SDN.

Windows Admin Center per l'infrastruttura iperconvergente è attivamente sviluppato da Microsoft. Riceve aggiornamenti frequenti che migliorare le funzionalità esistenti e aggiungono nuove funzionalità.

## Prima di iniziare

Per gestire il cluster come l'infrastruttura iperconvergente in Windows Admin Center, deve essere in esecuzione Windows Server 2016 o Windows Server 2019 e avere Hyper-V e spazi di archiviazione diretta abilitato. Facoltativamente, può anche avere Software Defined Networking abilitato e gestito tramite Windows Admin Center.

> [!Tip]
> Windows Admin Center offre anche una gestione generica esperienza per qualsiasi cluster supportare qualsiasi carico di lavoro, disponibile per Windows Server 2012 e versioni successive. Se questo sembra una soluzione migliore, quando si aggiunge il cluster a Windows Admin Center, seleziona i [**Cluster di Failover**](manage-failover-clusters.md) invece di **Cluster iperconvergente**.

### Preparare il cluster di Windows Server 2016 per Windows Admin Center

Windows Admin Center per l'infrastruttura iperconvergente dipende dalla gestione che API aggiunte dopo il rilascio di Windows Server 2016. Per poter gestire il cluster di Windows Server 2016 con Windows Admin Center, devi eseguire questi due passaggi:

1. Verificare che tutti i server del cluster sia installato l' [aggiornamento cumulativo per Windows Server 2016 (KB4103723) 2018-05](https://support.microsoft.com/help/4103723/windows-10-update-kb4103723) o versione successiva. Per scaricare e installare questo aggiornamento, Vai a **Impostazioni** > **Aggiorna & sicurezza** > **Windows Update** e seleziona **controllo online per gli aggiornamenti da Microsoft Update**.
2. Esegui il seguente cmdlet PowerShell come amministratore nel cluster:

```powershell
    Add-ClusterResourceType -Name "SDDC Management" -dll "$env:SystemRoot\Cluster\sddcres.dll" -DisplayName "SDDC Management"
```

> [!Tip]
> Devi solo eseguire una sola volta, il cmdlet su qualsiasi server del cluster. Puoi eseguire localmente in Windows PowerShell o usare provider di credenziali del servizio di sicurezza (CredSSP) per l'esecuzione in modalità remota. A seconda della configurazione, potrebbe non essere in grado di eseguire questo cmdlet all'interno di Windows Admin Center.

### Preparare il cluster di Windows Server 2019 per Windows Admin Center

Se il cluster esegue Windows Server 2019, i passaggi precedenti non sono necessari. Aggiungere semplicemente il cluster a Windows Admin Center come descritto nella sezione successiva e sei buona passare!

### Configurazione del Software definito Networking (facoltativo) ###

È possibile configurare l'infrastruttura iperconvergente che esegue Windows Server 2016 o 2019 usare Software Defined Networking (SDN) con i passaggi seguenti:

1. Preparare il disco rigido virtuale del sistema operativo che è il sistema operativo stesso è stato installato negli host dell'infrastruttura iperconvergente. Questo disco rigido virtuale verrà usato per tutte le macchine virtuali SLB/NC/GW.
2. Scarica tutte le cartelle e i file in SDN Express da [https://github.com/Microsoft/SDN/tree/master/SDNExpress](https://github.com/Microsoft/SDN/tree/master/SDNExpress).
3. Prepara una macchina virtuale diversa utilizzando la console di distribuzione. Questa macchina virtuale deve essere in grado di accedere gli host SDN. Inoltre, la macchina virtuale deve avere installato lo strumento RSAT Hyper-V.
4. Copia tutto ciò che hai scaricato per SDN Express alla console di deployment della macchina virtuale. E Condividi la cartella **SDNExpress** . Assicurati che tutti gli host possono accedere alla cartella condivisa **SDNExpress** , come definito nella riga del file di configurazione 8:
```
    \\$env:Computername\SDNExpress
```
5. Copia il disco rigido virtuale del sistema operativo nella cartella **immagini** nella cartella **SDNExpress** nella console deployment della macchina virtuale.
6. Modificare la configurazione rapida di SDN con la configurazione dell'ambiente. Completare i passaggi seguenti due dopo aver modificato la configurazione rapida di SDN in base alle tue informazioni di ambiente.
7. Esegui PowerShell con privilegi di amministratore per distribuire SDN:

```powershell
    .\SDNExpress.ps1 -ConfigurationDataFile .\your_fabricconfig.PSD1 -verbose 
```

La distribuzione richiederà circa 30-45 minuti.

## Informazioni di base

Dopo che l'infrastruttura iperconvergente viene distribuito, puoi gestire con Windows Admin Center.

### Installare Windows Admin Center

Se hai già fatto, scaricare e installare Windows Admin Center. Il modo per ottenere e rapida in esecuzione è installato nel computer di Windows 10 e gestire i server in remoto. Questa operazione richiede meno di cinque minuti. [Download](https://aka.ms/windowsadmincenter) o [altre informazioni su altre opzioni di installazione](../deploy/install.md).

### Aggiungere Cluster iper-convergenti

Per aggiungere il cluster a Windows Admin Center:

1. Fare clic su **+ Aggiungi** in tutte le connessioni.
2. Scegliere di aggiungere una **Connessione di Cluster iperconvergente**.
3. Digita il nome del cluster e, se richiesto, le credenziali da utilizzare.
4. Fai clic su **Aggiungi** alla fine.

Il cluster verrà aggiunto all'elenco di connessioni. Fai clic su esso per avviare il Dashboard.

![Aggiungi connessione cluster iper-convergenti](../media/manage-hyper-converged/add-hyper-converged-cluster-connection.gif)

### Aggiungere abilitato SDN Cluster iper-convergenti (Windows Admin Center Preview)

La versione più recente di Windows Admin Center Preview supporta la gestione di Software Defined Networking per l'infrastruttura iperconvergente. Aggiungendo un URI per il resto di Controller di rete per la connessione a Hyper-Converged cluster, è possibile utilizzare Gestione di Cluster iperconvergenti per gestire le risorse SDN e monitorare l'infrastruttura SDN.

1. Fare clic su **+ Aggiungi** in tutte le connessioni.
2. Scegliere di aggiungere una **Connessione di Cluster iperconvergente**.
3. Digita il nome del cluster e, se richiesto, le credenziali da utilizzare.
4. Controlla **Configura il Controller di rete** per continuare.
5. Immetti l' **URI di Controller di rete** e fare clic su **convalida**.
6. Fai clic su **Aggiungi** alla fine.

Il cluster verrà aggiunto all'elenco di connessioni. Fai clic su esso per avviare il Dashboard.

![Aggiungi connessione abilitato SDN cluster iper-convergenti](../media/manage-hyper-converged/add-snd-enabled-hci-connection.png)

> [!Important]
> Gli ambienti di SDN con l'autenticazione Kerberos per la comunicazione Northbound non sono attualmente supportati.

## Domande frequenti

### Ci sono differenze tra la gestione di Windows Server 2016 e Windows Server 2019?

Sì. Windows Admin Center per l'infrastruttura iperconvergente riceve aggiornamenti frequenti che migliorano l'esperienza per Windows Server 2016 e Windows Server 2019. Tuttavia, alcune nuove funzionalità sono disponibili solo per Windows Server 2019, ad esempio, l'interruttore Attiva/Disattiva per la deduplicazione e compressione.

### Posso utilizzare Windows Admin Center per gestire spazi di archiviazione diretta per altri casi di utilizzo (non iperconvergente), ad esempio convergente File Server di scalabilità orizzontale (SoFS) o Microsoft SQL Server?

Windows Admin Center per l'infrastruttura iperconvergente non fornisce gestione o le opzioni di monitoraggio in modo specifico per altri casi di utilizzo di spazi di archiviazione diretta, ad esempio, non potrà creare condivisioni di file. Tuttavia, le funzionalità del Dashboard e core, ad esempio la creazione di volumi o la sostituzione di unità, funzioneranno per qualsiasi cluster di spazi di archiviazione diretta.

### Che cos'è la differenza tra un Cluster di Failover e un Cluster iperconvergente?

In generale, il termine "iper-convergenti" si riferisce all'esecuzione di Hyper-V e spazi di archiviazione diretta nello stesso cluster di server per virtualizzare le risorse di calcolo e archiviazione. Nel contesto di Windows Admin Center, quando si fa clic **+ Aggiungi** dall'elenco delle connessioni, puoi scegliere tra l'aggiunta di una **connessione di Cluster di Failover** o una **connessione di Cluster iperconvergente**:

- La **connessione del Cluster di Failover** è la nuova versione di app desktop gestione Cluster di Failover. Fornisce un'esperienza di gestione familiare e generica per qualsiasi supporto di qualsiasi carico di lavoro, tra cui Microsoft SQL Server del cluster. È disponibile per Windows Server 2012 e versioni successive.

- La **connessione del Cluster iperconvergente** è un'esperienza completamente nuova personalizzata per spazi di archiviazione diretta e Hyper-V. Include il dashboard e offre grafici e avvisi per il monitoraggio. È disponibile per Windows Server 2016 e Windows Server 2019.

### Perché devo all'ultimo aggiornamento cumulativo per Windows Server 2016?

Windows Admin Center per l'infrastruttura iperconvergente dipende dalla gestione che API sviluppate dopo il rilascio di Windows Server 2016. Queste API vengono aggiunte nel [2018-05 aggiornamento cumulativo per Windows Server 2016 (KB4103723)](https://support.microsoft.com/help/4103723/windows-10-update-kb4103723), disponibile a partire da 8 maggio 2018.

### Quanto costa utilizzare Windows Admin Center?

Windows Admin Center non comporta costi aggiuntivi oltre a Windows.

Puoi utilizzare Windows Admin Center (disponibile come download distinto) con le licenze valide di Windows Server o Windows 10 senza alcun costo aggiuntivo: si è concesso in licenza in condizioni di licenza aggiuntive di Windows.

### Windows Admin Center richiede System Center?

No.

### E richiede una connessione Internet?

No.

Sebbene Windows Admin Center offre potenti e comodo integrazione con il cloud di Microsoft Azure, la gestione di core e monitoraggio esperienza per l'infrastruttura iperconvergente è completamente in locale. Può essere installato e usato senza una connessione Internet.

## Aspetti da provare

Se sta semplicemente attività iniziali, ecco alcune esercitazioni rapide per aiutarti a informazioni su come Windows Admin Center per l'infrastruttura iperconvergente organizzata e funziona. In alternativa, esercitare buon e prestare attenzione con gli ambienti di produzione. Questi video sono stati registrati con Windows Admin Center versione 1804 e una build di Insider Preview di Windows Server 2019.

### Gestire volumi di spazi di archiviazione diretta

<ul>
               <li>(0:37) <a href="https://youtu.be/o66etKq70N8">come creare un volume con mirroring a tre vie</a></li>
               <li>(1:17) <a href="https://youtu.be/R72QHudqWpE">come creare un volume di parità accelerata con mirroring</a></li>
               <li>(1:02) <a href="https://youtu.be/j59z7ulohs4">come aprire un volume e aggiungere i file</a></li>
               <li>(0:51) <a href="https://youtu.be/PRibTacyKko">come attivare deduplicazione e compressione</a></li>
               <li>(0:47) <a href="https://youtu.be/hqyBzipBoTI">come espandere un volume</a></li>
               <li>(0:26) <a href="https://youtu.be/DbjF8r2F6Jo">come eliminare un volume</a></li>
</ul>

<table>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>Creare volumi, mirroring a tre vie</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/o66etKq70N8" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>Creare volumi, parità accelerata con mirroring</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/R72QHudqWpE" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>Aprire il volume e aggiungere file</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/j59z7ulohs4" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>Attivare la deduplicazione e compressione</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/PRibTacyKko" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
    <tr style="border: 0;">
        <td style="padding: 5px; border: 0;">
            <strong>Espandi volume</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/hqyBzipBoTI" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
        <td style="padding: 5px; border: 0;">
            <strong>Delete volume</strong>
            <iframe width="375" height="210" src="https://www.youtube-nocookie.com/embed/DbjF8r2F6Jo" frameborder="0" allow="autoplay; encrypted-media" allowfullscreen="allowfullscreen"></iframe>
        </td>
    </tr>
</table>

### Creare una nuova macchina virtuale

1. Fai clic sullo strumento di **macchine virtuali** dal riquadro di spostamento a sinistra.
2. Nella parte superiore dello strumento di macchine virtuali, scegli la scheda di **inventario** e quindi fai clic di **Nuovo** per creare una nuova macchina virtuale.
3. Immetti il nome della macchina virtuale e scegliere tra le macchine virtuali della generazione 1 e 2.
4. Viene quindi scegliere quale host di creare la macchina virtuale in inizialmente o di usare l'host consigliata.
5. Scegliere un percorso per i file della macchina virtuale. Scegli un volume nell'elenco a discesa o fai clic su **Sfoglia** per selezionare una cartella tramite selezione cartelle. Il file di configurazione macchina virtuale e file di disco rigido virtuale verrà salvate in una singola cartella sotto la `\Hyper-V\[virtual machine name]` percorso del volume selezionato o del percorso.
6. Scegli il numero di processori virtuali, se vuoi virtualizzazione annidata abilitata, configurare le impostazioni della memoria, le schede di rete, i dischi rigidi virtuali e scegliere se si desidera installare un sistema operativo da un file di immagine ISO o dalla rete.
7. Fai clic su **Crea** per creare la macchina virtuale.
8. Una volta che la macchina virtuale viene creata e viene visualizzato nell'elenco di macchina virtuale, è possibile avviare la macchina virtuale.
9. Una volta che viene avviata la macchina virtuale, è possibile connettersi alla console della macchina virtuale tramite VMConnect per installare il sistema operativo. Selezionare la macchina virtuale dall'elenco, fai clic su **altre** > **Connect** per scaricare il file RDP. Apri il file RDP nell'app di connessione Desktop remoto. Dato che questo si connette alla console della macchina virtuale, dovrai immettere le credenziali di amministratore dell'host Hyper-V.

[Ulteriori informazioni sulla gestione delle macchine virtuali con Windows Admin Center](manage-virtual-machines.md).

### Sospendere e riavviare in modo sicuro un server

1. Nel **Dashboard**, seleziona **i server** dal riquadro di spostamento sul lato sinistro o facendo clic sul collegamento **gt _ Visualizza server** nel riquadro nell'angolo inferiore destro del Dashboard.
2. Nella parte superiore, passa da **Riepilogo** alla scheda **inventario** .
3. Selezionare un server facendo clic sul relativo nome per aprire la pagina dei dettagli di **Server** .
4. Fai clic sul **server di pausa di manutenzione**. Se è sicuro procedere, si sposterà macchine virtuali ad altri server del cluster. Il server deve quindi possono esaurire stato mentre in questo caso. Se vuoi, puoi guardare le macchine virtuali spostare nella pagina, **le macchine virtuali gt _ inventario** in cui i server host viene visualizzato chiaramente la griglia. Quando sono state spostate in tutte le macchine virtuali, lo stato del server sarà **in pausa**.
5. Fai clic su **Gestisci server** per accedere a tutti gli strumenti di gestione per ogni server in Windows Admin Center.
6. Fai clic su **Riavvia**, quindi **Sì**. Verrà essere espulso tornare all'elenco di connessioni.
7. Nuovamente nel **Dashboard**, il server viene visualizzato in rosso mentre è verso il basso.
8. Una volta è verso l'alto, passa nuovamente la pagina del **Server** e fai clic su **server ripresa da manutenzione** per impostare lo stato del server per semplicemente alto. In fase di macchine virtuali vengono spostati indietro – non è necessaria alcuna azione dell'utente.

### Sostituire un'unità non riuscita

1. Quando un'unità ha esito negativo, viene visualizzato un avviso nell'area superiore sinistra **gli avvisi** del **Dashboard**.
2. Puoi anche selezionare **unità** dal riquadro di spostamento sul lato sinistro o fare clic sul collegamento **gt _ unità di visualizzazione** nel riquadro nell'angolo inferiore destro per cercare le unità e visualizzare lo stato per te stesso. Nella scheda **inventario** , la griglia supporta l'ordinamento, il raggruppamento e ricerca parola chiave.
3. Nel **Dashboard**, fai clic sull'avviso per visualizzare i dettagli, ad esempio posizione fisica dell'unità.
4. Per ulteriori informazioni, fare clic sul collegamento **Vai all'unità** nella pagina dei dettagli di **unità** .
5. Se l'hardware la supporta, è possibile scegliere di **attivare/disattivare la luce** per controllare la luce indicatore dell'unità.
6. Spazi di archiviazione diretta viene ritirato e automaticamente evacuates unità con errori. Quando si è verificato, lo stato di unità viene ritirato e la barra di capacità di archiviazione è vuota.
7. Rimuovere l'unità non riuscito e inserire relativa sostituzione.
8. Nell' **unità gt _ inventario**, verrà visualizzata la nuova unità. Nel tempo, Cancella l'avviso, volumi ripristinerà torna allo stato OK ed archiviazione verrà ribilanciare nella nuova unità, non è necessaria alcuna azione dell'utente.

### Gestire le reti virtuali (cluster HCI abilitato SDN con Windows Admin Center Preview)

1. Seleziona **Le reti virtuali** dal riquadro di spostamento sul lato sinistro.
2. Fai clic sul **Nuovo** per creare una nuova rete virtuale e subnet, o scegliere una rete virtuale esistente e scegliere **le impostazioni** per modificare la configurazione.
3. Fai clic su una rete virtuale esistente per visualizzare le connessioni di macchina virtuale per le subnet di rete virtuale e accedere gli elenchi di controllo applicati alla subnet di rete virtuale.

![Gestire le reti virtuali](../media/manage-hyper-converged/manage-virtual-networks.png)

### Connettere una macchina virtuale a una rete virtuale (cluster HCI abilitato SDN con Windows Admin Center Preview)

1. Seleziona **le macchine virtuali** dal riquadro di spostamento sul lato sinistro.
2. Scegli un esistenti macchina virtuale gt _ Click gt _ **Impostazioni** Apri la scheda **reti** in **Impostazioni**.
3. Configurare i campi di **Rete virtuale** e **Subnet virtuale** per connettere la macchina virtuale a una rete virtuale.

È inoltre possibile configurare la rete virtuale durante la creazione di una macchina virtuale.

![Connettere una macchina virtuale a una rete virtuale](../media/manage-hyper-converged/connect-vm-to-virtual-network.png)

### Infrastruttura Software Defined Networking Monitor (cluster HCI abilitato SDN con Windows Admin Center Preview)

1. Seleziona **Il monitoraggio di SDN** dal riquadro di spostamento sul lato sinistro.
2. Visualizzare informazioni dettagliate sull'integrità di Controller di rete, bilanciamento del carico Software, Gateway virtuale e monitorare l'utilizzo di Pool di Gateway virtuali, pubblica e privata IP Pool e lo stato di host SDN.

![Infrastruttura SDN monitor](../media/manage-hyper-converged/sdn-monitoring.png)

## Feedback

È completamente incentrato sul tuo feedback! Il vantaggio più importante di aggiornamenti frequenti è ascoltare ciò che funziona e quale deve essere migliorata. Ecco alcuni modi per farci sapere cosa stai pensando:

- [Inviare e votare per le richieste di funzionalità su UserVoice](https://windowsserver.uservoice.com/forums/295071/category/319162?query=%5Bhci%5D)
- [Aggiungere il forum di Windows Admin Center nella Community tecnica di Microsoft](https://techcommunity.microsoft.com/t5/Windows-Server-Management/bd-p/WindowsServerManagement)
- TWEET a `@servermgmt`

### Vedi anche

- [Windows Admin Center](../understand/windows-admin-center.md)
- [Spazi di archiviazione diretta](https://docs.microsoft.com/windows-server/storage/storage-spaces/storage-spaces-direct-overview)
- [Hyper-V](https://docs.microsoft.com/windows-server/virtualization/hyper-v/hyper-v-on-windows-server)
- [SDN (Software Defined Networking)](https://docs.microsoft.com/windows-server/networking/sdn/software-defined-networking)
