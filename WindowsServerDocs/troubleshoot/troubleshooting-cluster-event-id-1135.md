---
title: Risoluzione dei problemi relativi al cluster con ID evento 1135
description: Viene descritto come risolvere il problema di avvio del servizio cluster con ID evento 1135.
ms.date: 05/28/2020
ms.openlocfilehash: d59f8b89e89ea7ff42aecd79670465aee8d63524
ms.sourcegitcommit: 5fac756c2c9920757e33ef0a68528cda0c85dd04
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/02/2020
ms.locfileid: "84306530"
---
# <a name="troubleshooting-cluster-issue-with-event-id-1135"></a>Risoluzione dei problemi relativi al cluster con ID evento 1135

Questo articolo consente di diagnosticare e risolvere l'ID evento 1135, che può essere registrato durante l'avvio del Servizio cluster nell'ambiente clustering di failover.

## <a name="start-page"></a>Pagina iniziale

L'ID evento 1135 indica che uno o più nodi del cluster sono stati rimossi dall'appartenenza al cluster di failover attivo. Può essere accompagnato dai sintomi seguenti:

- Failover\nodes del cluster da rimuovere dall'appartenenza al cluster di failover attivo: problemi [con i nodi rimossi dall'appartenenza al cluster di failover attivo](/problem-nodes-failover-cluster.md)
- ID evento 1069 [ID evento 1069: servizio cluster o disponibilità dell'applicazione](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc756225(v=ws.10))
- ID evento 1177 per l'evento di perdita del quorum [id 1177: quorum e connettività necessari per il quorum](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc773498(v=ws.10))
- ID evento 1006 per Servizio cluster interrotto: [ID evento 1006-avvio del servizio cluster](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc773418(v=ws.10))

È consigliabile eseguire una convalida e i test di rete come una delle procedure iniziali per la risoluzione dei problemi per verificare che non siano presenti problemi di configurazione che potrebbero causare problemi.

### <a name="check-if-installed-the-recommended-hot-fixes"></a>Controllare se sono stati installati gli aggiornamenti rapidi consigliati

Il Servizio cluster è il componente software essenziale che controlla tutti gli aspetti delle operazioni del cluster di failover e gestisce il database di configurazione del cluster. Se viene visualizzato l'ID evento 1135, Microsoft consiglia di installare le correzioni indicate negli articoli della KB seguenti e riavviare tutti i nodi del cluster, quindi osservare se il problema si verifica.

- [Hotfix per Windows Server 2012 R2](https://support.microsoft.com/help/2920151)
- [Hotfix per Windows Server 2012](https://support.microsoft.com/help/2784261)
- [Hotfix per Windows Server 2008 R2](https://support.microsoft.com/help/2545685)

### <a name="check-if-the-cluster-service-running-on-all-the-nodes"></a>Controllare se il servizio cluster è in esecuzione in tutti i nodi

Eseguire il comando seguente in base al sistema operativo Windows per verificare che il servizio cluster sia sempre in esecuzione e disponibile.

#### <a name="for-windows-server-2008-r2-cluster"></a>Per il cluster Windows Server 2008 R2

dall'esecuzione del prompt dei comandi con privilegi elevati: **nodo cluster. exe/stat**  

#### <a name="for-windows-server-2012-and-windows-server-2012-r2-cluster"></a>Per il cluster Windows Server 2012 \ e Windows Server 2012 R2

eseguire il comando PS: **nodo cluster/status**  

Il servizio cluster è sempre in esecuzione e disponibile in tutti i nodi?

## <a name="solution-for-cluster-service-is-failing"></a>La soluzione per il servizio cluster non è riuscita

Se il servizio cluster non riesce, risolvere i problemi utilizzando questo articolo: [Opzioni di avvio di Windows Server 2008 e 2008R2 failover cluster](/archive/blogs/askcore/windows-server-2008-and-2008r2-failover-cluster-startup-switches).


## <a name="several-scenarios-of-event-id-1135"></a>Diversi scenari di ID evento 1135

Si vuole esaminare più da vicino i registri eventi di sistema in tutti i nodi del cluster. Esaminare l'ID evento 1135 visualizzato nei nodi e copiare tutte le istanze di questo evento. In questo modo sarà più semplice esaminarli ed esaminarli.

```console
Event ID 1135
Cluster node ' **NODE A** ' was removed from the active failover cluster membership. The Cluster service on this node may have stopped. This could also be due to the node having lost communication with other active nodes in the failover cluster. Run the Validate a Configuration wizard to check your network configuration. If the condition persists, check for hardware or software errors related to the network adapters on this node. Also check for failures in any other network components to which the node is connected such as hubs, switches, or bridges.
```

Esistono tre scenari tipici:

### <a name="scenario-a"></a>Scenario A

Si stanno esaminando tutti gli eventi e tutti i nodi nel cluster indicano che il nodo A ha perso la comunicazione.

![Scenario uno ](media/troubleshooting-cluster-event-id-1135/18647.png)
 ![ scenario a](media/troubleshooting-cluster-event-id-1135/18648.png)

Quando si visualizzano i registri di sistema nel nodo A, è possibile che siano presenti eventi per tutti i nodi rimanenti del cluster.

#### <a name="solution"></a>Soluzione

Questo suggerisce piuttosto che al momento del problema, a causa della congestione della rete o in caso contrario, la comunicazione con il nodo A è stata persa.

Verificare e convalidare i problemi di configurazione e comunicazione di rete. Ricordarsi di cercare i problemi relativi al nodo A.

### <a name="scenario-b"></a>Scenario B

Si stanno esaminando gli eventi nei nodi e si può indicare che il cluster è distribuito in due siti. Nodo A, nodo B e nodo C nel sito 1 e nodo D & nodo E nel sito 2.

![Scenario B](media/troubleshooting-cluster-event-id-1135/18649.png)

Nei nodi A, B e C si noterà che gli eventi registrati sono per la connettività ai nodi D & E. Analogamente, quando vengono visualizzati gli eventi nei nodi D & E, gli eventi indicano che si è persa la comunicazione con A, B e C.

![Scenario B](media/troubleshooting-cluster-event-id-1135/18650.png)

#### <a name="solution"></a>Soluzione

Se viene visualizzata un'attività simile, è indicativo che si è verificato un errore di comunicazione, sul collegamento che connette questi siti. Si consiglia di esaminare la connessione tra i siti, se si tratta di una connessione WAN, si consiglia di verificare con il provider di servizi Internet la connettività.

### <a name="scenario-c"></a>Scenario C

Si stanno esaminando gli eventi nei nodi e si noterà che i nomi dei nodi non vengono conteggiati con un modello particolare. Supponiamo che il cluster sia distribuito tra due siti. NODO A, nodo B e nodo C nel sito 1 e nodo D & nodo E nel sito 2.

- Nel nodo A: vengono visualizzati gli eventi per i nodi B, D, E.
- Sul nodo B: vengono visualizzati gli eventi per i nodi C, D, E.
- Nel nodo C: vengono visualizzati gli eventi per i nodi A, B, E.
- Nel nodo D: vengono visualizzati gli eventi per i nodi A, C, E.
- Nel nodo E: vengono visualizzati gli eventi per i nodi B, C, D.
- O qualsiasi altra combinazione.

![Scenario C](media/troubleshooting-cluster-event-id-1135/18651.png)

#### <a name="solution"></a>Soluzione

Tali eventi sono possibili quando i canali di rete tra i nodi vengono soffocati e i messaggi di comunicazione del cluster non vengono raggiunti in modo tempestivo, facendo in modo che il cluster ritenga che la comunicazione tra i nodi venga persa, causando la rimozione dei nodi dall'appartenenza al cluster.

## <a name="review-cluster-networks"></a>Esaminare le reti cluster

Per continuare questa guida alla risoluzione dei problemi, è consigliabile esaminare le reti cluster selezionando una alla volta le tre opzioni seguenti.

### <a name="check-for-antivirus-exclusion"></a>Verifica l'esclusione di antivirus

Escludere i seguenti percorsi di file system dall'analisi dei virus in un server che esegue servizi cluster:

- Percorso del server di controllo del mirroring della condivisione file

- *Cartella%SystemRoot%\Cluster*

Configurare il componente di analisi in tempo reale all'interno del software antivirus in modo da escludere le directory e i file seguenti:

- Directory di configurazione della macchina virtuale predefinita (C:\ProgramData\Microsoft\Windows\Hyper-V)

- Directory di configurazione della macchina virtuale personalizzata

- Directory predefinita dell'unità disco rigido virtuale (dischi rigidi C:\utenti\public\documents\hyper-V\Virtual Hard)

- Directory di unità disco rigido virtuale personalizzate

- Directory di dati di replica personalizzata, se si usa la replica Hyper-V

- Directory snapshot

- MMS. exe

    > [!NOTE]
    > È possibile che il file debba essere configurato come esclusione del processo all'interno del software antivirus.

- Vmwp. exe

    > [!NOTE]
    > È possibile che il file debba essere configurato come esclusione del processo all'interno del software antivirus.

Inoltre, quando si usa Live Migration insieme ai volumi condivisi del cluster, escludere il percorso CSV *C:\Clusterstorage* e tutte le relative sottodirectory. Per risolvere problemi di failover o problemi generali relativi a servizi cluster e software antivirus, disinstallare temporaneamente il software antivirus o contattare il produttore del software per determinare se il software antivirus funziona con i servizi cluster. Nella maggior parte dei casi, la disabilitazione del software antivirus non è sufficiente. Anche se si disabilita il software antivirus, il driver del filtro viene ancora caricato quando si riavvia il computer.

### <a name="check-for-network-port-configuration-in-firewall"></a>Controllare la configurazione della porta di rete nel firewall

Il Servizio cluster controlla le operazioni del cluster di server e gestisce il database del cluster. Un cluster è una raccolta di computer indipendenti che fungono da singolo computer. I responsabili, i programmatori e gli utenti vedono il cluster come un unico sistema. Il software distribuisce i dati tra i nodi del cluster. In caso di errore di un nodo, gli altri nodi forniscono i servizi e i dati precedentemente forniti dal nodo mancante. Quando si aggiunge o si ripristina un nodo, il software del cluster esegue la migrazione di alcuni dati al nodo.

Nome del servizio di sistema: **clussvc**  

|Applicazione|Protocollo|Porte|
|---|---|---|
|Servizio cluster|UDP|3343|
|Servizio cluster|TCP|3343 (questa porta è obbligatoria durante un'operazione di join del nodo).|
|RPC|TCP|135|
|Amministrazione cluster|UDP|137|
|Kerberos|UDP\TCP|464 *|
|SMB|TCP|445|
|Porte UDP elevate allocate in modo casuale * *|UDP|Numero di porta casuale compreso tra 1024 e 65535<br/>Numero di porta casuale compreso tra 49152 e 65535 * * *|
||||

> [!NOTE]
> Inoltre, per la corretta convalida nei cluster di failover Windows in Windows Server 2008 e versioni successive, consentire il traffico in ingresso e in uscita per ICMP4, ICMP6.

- Per ulteriori informazioni, vedere la pagina relativa alla [creazione di un cluster di failover di Windows Server 2012 non riuscita con errore 0xC000005E](https://support.microsoft.com/help/2830510).

- Per ulteriori informazioni su come personalizzare queste porte, vedere [Panoramica dei servizi e requisiti per le porte di rete per Windows](https://support.microsoft.com/help/832017/) nella sezione "Riferimenti".

Si tratta dell'intervallo in Windows Server 2012, Windows 8, Windows Server 2008 R2, Windows 7, Windows Server 2008 e Windows Vista.

Eseguire inoltre il comando seguente per verificare la configurazione della porta di rete nel firewall. Ad esempio, questo comando consente di determinare la porta 3343 available\open usata per il cluster di failover:

```console
netsh advfirewall firewall show rule name="Failover Clusters (UDP-In)" verbose
```

### <a name="run-the-cluster-validation-report-for-any-errors-or-warnings"></a>Eseguire il report di convalida del cluster per eventuali errori o avvisi

Lo strumento di convalida cluster esegue una serie di test per verificare che l'hardware e le impostazioni siano compatibili con il clustering di failover.

Seguire queste istruzioni:

1. Eseguire il report di convalida del cluster per individuare eventuali errori o avvisi. Per ulteriori informazioni, vedere informazioni sui [test di convalida del cluster: rete](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771323(v=ws.11)?redirectedfrom=MSDN)

    ![subhatt1](media/troubleshooting-cluster-event-id-1135/18653.png)

2. Verificare la presenza di avvisi ed errori per le reti. Per ulteriori informazioni, vedere informazioni sui [test di convalida del cluster: rete](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc771323(v=ws.11)?redirectedfrom=MSDN).

    ![Risultati per rete di categoria ](media/troubleshooting-cluster-event-id-1135/18654.png) ![](media/troubleshooting-cluster-event-id-1135/18655.png)

#### <a name="check-the-list-network-binding-order"></a>Controllare l'ordine di associazione di rete dell'elenco

Questo test elenca l'ordine in cui le reti sono associate alle schede in ogni nodo.

Nella scheda adapter e binding sono elencate le connessioni in base all'ordine in cui le connessioni sono accessibili dai servizi di rete. L'ordine di queste connessioni riflette l'ordine in cui vengono inviati i pacchetti e le chiamate TCP/IP generiche alla rete.

Attenersi ai passaggi seguenti per modificare l'ordine di associazione delle schede di rete:

1. Fare clic sul pulsante **Start**, scegliere **Esegui**, digitare ncpa. cpl, quindi fare clic su **OK**. È possibile visualizzare le connessioni disponibili nella sezione **LAN e Internet ad alta velocità** della finestra **connessioni di rete** .

2. Scegliere **Impostazioni avanzate**dal menu **Avanzate** , quindi fare clic sulla scheda **Adapter e associazioni** .

3. Nell'area **connessioni** selezionare la connessione che si desidera spostare più in alto nell'elenco. Utilizzare i pulsanti freccia per spostare la connessione. Come regola generale, la scheda che comunica alla rete (connettività del dominio, routing ad altre reti e così via) deve essere la prima scheda associata (Top of the list).

I nodi del cluster sono sistemi multihomed. La priorità di rete influiscono sul client DNS per la connettività di rete in uscita. Le schede di rete utilizzate per la comunicazione client devono trovarsi nella parte superiore dell'ordine di associazione. Le reti non indirizzate possono essere posizionate a priorità più bassa. In Windows Server 2012 e Windows Server2012 R2, il driver di rete del cluster (NETFT. SYS) viene posizionata automaticamente nella parte inferiore dell'elenco degli ordini di binding.

#### <a name="check-the-validate-network-communication"></a>Controllare la comunicazione di rete di convalida

Anche la latenza sulla rete potrebbe causare questo problema. È possibile che i pacchetti non vadano perduti tra i nodi, ma potrebbero non raggiungere i nodi in modo sufficientemente veloce prima della scadenza del periodo di timeout.

Questo test verifica che i server testati siano in grado di comunicare con latenza accettabile in tutte le reti.

Ad esempio: in convalidare la comunicazione di rete, è possibile visualizzare i messaggi seguenti per i problemi di latenza di rete.

```console
Succeeded in pinging network interface node003.contoso.com IP Address 192.168.0.2 from network interface node004.contoso.com IP Address 192.168.0.3 with maximum delay 500 after 1 attempt(s).
Either address 10.0.0.96 is not reachable from 192.168.0.2 or **the ping latency is greater than the maximum allowed 2000 ms** This may be expected, since network interfaces node003.contoso.com - Heartbeat Network and node004.contoso.com - Production Network are on different cluster networks
Either address 192.168.0.2 is not reachable from 10.0.0.96 or **the ping latency is greater than the maximum allowed 2000 ms** This may be expected, since network interfaces node004.contoso.com - Production Network and node003.contoso.com - Heartbeat Network for MSCS are on different cluster networks
```

Per il cluster multisito, è possibile aumentare i valori di timeout. Per altre informazioni, vedere [configurare l'heartbeat e le impostazioni DNS in un cluster di failover multisito](/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd197562(v=ws.10)?redirectedfrom=MSDN).

Controllare con ISP eventuali problemi di connettività WAN.

Controllare se si verifica uno dei problemi seguenti.

##### <a name="network-packets-lost-between-nodes"></a>Pacchetti di rete persi tra i nodi

1. Controllare la perdita di pacchetti usando le prestazioni

    Se il pacchetto viene perso in transito in un punto qualsiasi tra i nodi, gli heartbeat avranno esito negativo. È possibile scoprire facilmente se si tratta di un problema usando Performance Monitor per esaminare il contatore "Interface\Packets di rete ricevuti rimossi". Una volta aggiunto questo contatore, esaminare i numeri media, minimo e massimo e se si tratta di un valore maggiore di zero, il buffer di ricezione deve essere regolato per l'adapter.

    ![Aggiungi contatori](media/troubleshooting-cluster-event-id-1135/18652.png)

    Se si verifica una perdita di pacchetti di rete nella piattaforma di virtualizzazione VmWare, vedere la sezione "cluster installato nella piattaforma di virtualizzazione VmWare".

2. Aggiornare i driver della scheda di interfaccia di rete

    Questo problema può verificarsi a causa di schede di interfaccia di rete drivers\Integration (IC) non aggiornate \VmTools o schede NIC difettose.
    Se si verificano perdite di pacchetti di rete tra i nodi dei computer fisici, verificare che il driver della scheda di rete sia aggiornato. Driver della scheda di rete obsoleti o obsoleti o firmware.
    In alcuni casi, una semplice configurazione errata della scheda di rete o del Commuter può causare la perdita di heartbeat.

##### <a name="cluster-installed-in-the-vmware-virtualization-platform"></a>Cluster installato nella piattaforma di virtualizzazione VmWare

Verificare i problemi dell'adapter VMware nel caso di un ambiente VMware. 

Questo problema può verificarsi se i pacchetti vengono eliminati durante i picchi di traffico elevato. Assicurarsi che non si verifichino filtri di traffico, ad esempio con un filtro di posta elettronica. Dopo aver eliminato questa possibilità, aumentare gradualmente il numero di buffer nel sistema operativo guest e verificare.

Per ridurre il traffico di riduzioni, attenersi alla procedura seguente:

1. Aprire la casella Run usando il tasto Windows + R.
2. Digitare devmgmt. msc e premere **invio**.
3. Espandi **schede di rete**  
4. Fare clic con il pulsante destro del mouse su **vmxnet3 e scegliere Proprietà.**  
5. Fare clic sulla scheda **Avanzate**.
6. Fare clic su **piccoli buffer Rx** e aumentare il valore. Il valore predefinito è 512 e il valore massimo è 8192.
7. Fare clic su **RX Ring #1** size e aumentare il valore. Il valore predefinito è 1024 e il valore massimo è 4096.

Controllare gli URL seguenti per verificare i problemi dell'adapter VMware in caso di ambiente VMware:

- [Nodi rimossi dall'appartenenza al cluster di failover in VMware ESX?](/archive/blogs/askcore/nodes-being-removed-from-failover-cluster-membership-on-vmware-esx)

- [Perdita di pacchetti di grandi dimensioni a livello di sistema operativo guest in VMXNET3 vNIC in ESXi](https://kb.vmware.com/s/article/2039495)

##### <a name="noticed-any-network-congestion"></a>Si è notato qualsiasi congestione della rete

La congestione della rete può anche causare problemi di connettività di rete.

Verificare che la rete sia configurata in base ai consigli per MS e fornitore, vedere [configurazione delle reti di cluster di failover di Windows](/archive/blogs/askcore/configuring-windows-failover-cluster-networks).

##### <a name="check-the-network-configuration"></a>controllare la configurazione della rete

Se ancora non funziona, verificare che sia stata visualizzata la rete partizionata nell'interfaccia utente grafica del cluster o che gruppo NIC sia abilitato nella scheda di interfaccia di rete di heartbeat.

Se viene visualizzata la rete partizionata in GUI del cluster, vedere l'argomento relativo alle [reti cluster partizionate](/archive/blogs/askcore/partitioned-cluster-networks) per risolvere il problema.

Se la funzionalità Gruppo NIC è abilitata nella scheda di interfaccia di rete heartbeat, verificare il raggruppamento della funzionalità del software in base alla raccomandazione del fornitore del team.

##### <a name="upgrade-the-nic-drivers"></a>Aggiornare i driver della scheda di interfaccia di rete

Questo problema può verificarsi a causa di driver NIC obsoleti o schede NIC difettose.

Se si verificano perdite di pacchetti di rete tra i nodi dei computer fisici, è necessario aggiornare il driver della scheda di rete. Driver della scheda di rete obsoleti o obsoleti o firmware.

In alcuni casi, una semplice configurazione errata della scheda di rete o del Commuter può causare la perdita di heartbeat.

##### <a name="check-the-network-configuration"></a>controllare la configurazione della rete

Se ancora non funziona, controllare se è stata visualizzata la rete partizionata nell'interfaccia utente grafica del cluster o se gruppo NIC è abilitato nella scheda di interfaccia di rete di heartbeat.
