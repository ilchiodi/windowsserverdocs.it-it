---
title: Distribuire spazi di archiviazione diretta
ms.prod: windows-server
manager: eldenc
ms.author: stevenek
ms.technology: storage-spaces
ms.topic: get-started-article
ms.assetid: 20fee213-8ba5-4cd3-87a6-e77359e82bc0
author: stevenek
ms.date: 06/07/2019
description: Istruzioni dettagliate per la distribuzione di una risorsa di archiviazione definita dal software con Spazi di archiviazione diretta in Windows Server come infrastruttura iperconvergente o infrastruttura convergente (anche nota come disaggregata).
ms.localizationpriority: medium
ms.openlocfilehash: 60b29cbebb19cd8f1ce364d1eb7e920759375285
ms.sourcegitcommit: 06ae7c34c648538e15c4d9fe330668e7df32fbba
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/05/2020
ms.locfileid: "78371773"
---
# <a name="deploy-storage-spaces-direct"></a>Distribuire spazi di archiviazione diretta

> Si applica a: Windows Server 2019, Windows Server 2016

In questo argomento vengono fornite istruzioni dettagliate per la distribuzione di [spazi di archiviazione diretta](storage-spaces-direct-overview.md).

> [!Tip]
> Si vuole acquisire un'infrastruttura iperconvergente? Microsoft consiglia di acquistare una soluzione hardware/software convalidata dai partner, che include strumenti e procedure di distribuzione. Queste soluzioni sono progettate, assemblate e convalidate in base all'architettura di riferimento per garantire la compatibilità e l'affidabilità, per poter iniziare subito a funzionare. Per le soluzioni Windows Server 2019, visitare il [sito Web relativo alle soluzioni Azure stack HCI](https://azure.microsoft.com/overview/azure-stack/hci). Per le soluzioni Windows Server 2016, ulteriori informazioni sono disponibili in [Windows Server software-defined](https://microsoft.com/wssd).

> [!Tip]
> È possibile usare macchine virtuali Hyper-V, ad esempio in Microsoft Azure, per [valutare spazi di archiviazione diretta senza hardware](storage-spaces-direct-in-vm.md). È anche possibile esaminare gli utili script di [distribuzione di Windows Server Rapid Lab](https://aka.ms/wslab), che vengono usati a scopo di training.

## <a name="before-you-start"></a>Prima di iniziare

Esaminare i [requisiti hardware spazi di archiviazione diretta](Storage-Spaces-Direct-Hardware-Requirements.md) e scorrere questo documento per acquisire familiarità con l'approccio generale e le note importanti associate ad alcuni passaggi.

Raccogliere le informazioni seguenti:

- **Opzione di distribuzione.** Spazi di archiviazione diretta supporta [due opzioni di distribuzione: iperconvergente e convergente](storage-spaces-direct-overview.md#deployment-options), nota anche come disaggregata. Familiarizzare con i vantaggi di ognuno di essi per decidere quale sia la scelta più adatta. I passaggi 1-3 seguenti si applicano a entrambe le opzioni di distribuzione. Il passaggio 4 è necessario solo per la distribuzione convergente.

- **Nomi dei server.** Acquisire familiarità con i criteri di denominazione dell'organizzazione per computer, file, percorsi e altre risorse. È necessario eseguire il provisioning di più server, ognuno con nomi univoci.

- **Nome di dominio.** Acquisire familiarità con i criteri dell'organizzazione per la denominazione dei domini e l'aggiunta al dominio.  Si aggiungeranno i server al dominio e sarà necessario specificare il nome di dominio. 

- **Rete RDMA.** Esistono due tipi di protocolli RDMA: iWarp e RoCE. Si noti che le schede di rete usano e, se RoCE, si noti anche la versione (V1 o V2). Per RoCE, prendere nota anche del modello dell'opzione Top-of-rack.

- **ID VLAN.** Prendere nota dell'ID VLAN da usare per le schede di rete del sistema operativo di gestione sui server, se presenti. È possibile ottenere queste informazioni dall'amministratore di rete.

## <a name="step-1-deploy-windows-server"></a>Passaggio 1: Distribuire Windows Server

### <a name="step-11-install-the-operating-system"></a>Passaggio 1,1: installare il sistema operativo

Il primo passaggio consiste nell'installare Windows Server in tutti i server che saranno presenti nel cluster. Spazi di archiviazione diretta richiede Windows Server 2016 Datacenter Edition. È possibile usare l'opzione di installazione dei componenti di base del server o server con esperienza desktop.

Quando si installa Windows Server utilizzando l'installazione guidata, è possibile scegliere tra *Windows Server* (che fa riferimento a Server Core) e *Windows Server (server con esperienza desktop)* , che equivale all'opzione di installazione *completa* disponibile in Windows Server 2012 R2. Se non si sceglie, si otterrà l'opzione di installazione dei componenti di base del server. Per ulteriori informazioni, vedere [Opzioni di installazione per Windows Server 2016](../../get-started/Windows-Server-2016.md).

### <a name="step-12-connect-to-the-servers"></a>Passaggio 1,2: connettersi ai server

Questa guida è incentrata sull'opzione di installazione dei componenti di base del server e sulla distribuzione/gestione remota da un sistema di gestione separato, che deve disporre di:

- Windows Server 2016 con gli stessi aggiornamenti dei server gestiti
- Connettività di rete ai server gestiti
- Aggiunto allo stesso dominio o a un dominio completamente attendibile
- Ha gli strumenti di amministrazione remota del server (RSAT, Remote Server Administration Tools) e i moduli PowerShell per Hyper-V e per il clustering di failover. Gli strumenti di amministrazione remota e i moduli di PowerShell sono disponibili in Windows Server e possono essere installati senza installare altre funzionalità. È anche possibile installare il [strumenti di amministrazione remota del server](https://www.microsoft.com/download/details.aspx?id=45520) in un computer di gestione di Windows 10.

Nel sistema di gestione installare il cluster di failover e gli strumenti di gestione di Hyper-V. Questa operazione può essere eseguita tramite Server Manager usando l'**Aggiunta guidata ruoli e funzionalità**. Nella pagina **Funzionalità** selezionare **Strumenti di amministrazione remota del Server**, quindi selezionare gli strumenti da installare.

Immettere la sessione PS e usare il nome del server o l'indirizzo IP del nodo a cui connettersi. Dopo aver eseguito questo comando verrà richiesta una password, immettere la password di amministratore specificata durante la configurazione di Windows.

   ```PowerShell
   Enter-PSSession -ComputerName <myComputerName> -Credential LocalHost\Administrator
   ```

   Ecco un esempio di come eseguire la stessa operazione in modo più utile negli script, nel caso in cui sia necessario eseguire questa operazione più volte:

   ```PowerShell
   $myServer1 = "myServer-1"
   $user = "$myServer1\Administrator"

   Enter-PSSession -ComputerName $myServer1 -Credential $user
   ```

> [!TIP]
> Se si esegue la distribuzione in modalità remota da un sistema di gestione, potrebbe essere presente un errore, ad esempio *WinRM non è in grado di elaborare la richiesta.* Per risolvere il problema, utilizzare Windows PowerShell per aggiungere ogni server all'elenco di host attendibili nel computer di gestione:  
>   
> `Set-Item WSMAN:\Localhost\Client\TrustedHosts -Value Server01 -Force`
>  
> Nota: l'elenco di host attendibili supporta i caratteri jolly, ad esempio `Server*`.
>
> Per visualizzare l'elenco di host attendibili, digitare `Get-Item WSMAN:\Localhost\Client\TrustedHosts`.  
>   
> Per svuotare l'elenco, digitare `Clear-Item WSMAN:\Localhost\Client\TrustedHost`.  

### <a name="step-13-join-the-domain-and-add-domain-accounts"></a>Passaggio 1,3: aggiungere il dominio e aggiungere gli account di dominio

Fino a questo momento sono stati configurati i singoli server con l'account Administrator locale `<ComputerName>\Administrator`.

Per gestire Spazi di archiviazione diretta, è necessario aggiungere i server a un dominio e usare un account di dominio Active Directory Domain Services appartenente al gruppo Administrators in ogni server.

Dal sistema di gestione aprire una console di PowerShell con privilegi di amministratore. Usare `Enter-PSSession` per connettersi a ogni server ed eseguire il cmdlet seguente, sostituendo il nome del computer, il nome di dominio e le credenziali di dominio:

```PowerShell  
Add-Computer -NewName "Server01" -DomainName "contoso.com" -Credential "CONTOSO\User" -Restart -Force  
```

Se l'account amministratore di archiviazione non è un membro del gruppo Domain Admins, aggiungere l'account amministratore di archiviazione al gruppo Administrators locale in ogni nodo o ancora meglio, aggiungere il gruppo usato per gli amministratori di archiviazione. È possibile usare il comando seguente (oppure scrivere una funzione di Windows PowerShell a tale scopo. vedere [usare PowerShell per aggiungere utenti di dominio a un gruppo locale](https://blogs.technet.com/b/heyscriptingguy/archive/2010/08/19/use-powershell-to-add-domain-users-to-a-local-group.aspx) per altre informazioni):

```
Net localgroup Administrators <Domain\Account> /add
```

### <a name="step-14-install-roles-and-features"></a>Passaggio 1,4: installare ruoli e funzionalità

Il passaggio successivo consiste nell'installare i ruoli del server in ogni server. A tale scopo, è possibile usare l'interfaccia di [amministrazione di Windows](../../manage/windows-admin-center/use/manage-servers.md), [Server Manager](../../administration/server-manager/install-or-uninstall-roles-role-services-or-features.md)) o PowerShell. Ecco i ruoli da installare:

- Clustering di failover
- Hyper-V
- File server (se si desidera ospitare le condivisioni file, ad esempio per una distribuzione convergente)
- Data-Center-Bridging (se si utilizza RoCEv2 anziché schede di rete iWARP)
- RSAT-Clustering-PowerShell
- Hyper-V-PowerShell

Per eseguire l'installazione tramite PowerShell, usare il cmdlet [Install-WindowsFeature](https://docs.microsoft.com/powershell/module/microsoft.windows.servermanager.migration/install-windowsfeature) . È possibile usarlo in un unico server come il seguente:

```PowerShell
Install-WindowsFeature -Name "Hyper-V", "Failover-Clustering", "Data-Center-Bridging", "RSAT-Clustering-PowerShell", "Hyper-V-PowerShell", "FS-FileServer"
```

Per eseguire il comando su tutti i server nel cluster nello stesso momento, usare questo piccolo frammento di script, modificando l'elenco di variabili all'inizio dello script in base all'ambiente.

```PowerShell
# Fill in these variables with your values
$ServerList = "Server01", "Server02", "Server03", "Server04"
$FeatureList = "Hyper-V", "Failover-Clustering", "Data-Center-Bridging", "RSAT-Clustering-PowerShell", "Hyper-V-PowerShell", "FS-FileServer"

# This part runs the Install-WindowsFeature cmdlet on all servers in $ServerList, passing the list of features into the scriptblock with the "Using" scope modifier so you don't have to hard-code them here.
Invoke-Command ($ServerList) {
    Install-WindowsFeature -Name $Using:Featurelist
}
```

## <a name="step-2-configure-the-network"></a>Passaggio 2: Configurare la rete

Se si sta distribuendo Spazi di archiviazione diretta all'interno di macchine virtuali, ignorare questa sezione.

Spazi di archiviazione diretta richiede una rete a larghezza di banda elevata e a bassa latenza tra i server del cluster. Sono necessarie almeno 10 GbE per la rete e l'accesso diretto a memoria remota (RDMA) è consigliato. È possibile usare iWARP o RoCE, purché disponga del logo Windows Server 2016, ma iWARP è in genere più facile da configurare.

> [!Important]
> A seconda delle apparecchiature di rete e, soprattutto con RoCE V2, potrebbe essere necessaria una configurazione dell'opzione Top-of-rack. La configurazione del Commuter corretta è importante per garantire l'affidabilità e le prestazioni dei Spazi di archiviazione diretta.

Windows Server 2016 introduce il SET (switch-Embedded Teaming) nel Commuter virtuale Hyper-V. Ciò consente di usare le stesse porte NIC fisiche per tutto il traffico di rete durante l'uso di RDMA, riducendo il numero di porte NIC fisiche necessarie. Il gruppo switch-Embedded è consigliato per Spazi di archiviazione diretta.

Interconnessioni del nodo Switched o Switched
- Commutazione: i commutatori di rete devono essere configurati correttamente per gestire la larghezza di banda e il tipo di rete. Se si usa RDMA che implementa il protocollo RoCE, la configurazione del dispositivo di rete e del Commuter è ancora più importante.
- Switching: i nodi possono essere interconnessi usando connessioni dirette, evitando l'uso di un'opzione. È necessario che ogni nodo disponga di una connessione diretta a tutti gli altri nodi del cluster.

Per istruzioni su come configurare la rete per Spazi di archiviazione diretta, vedere la guida alla distribuzione di una scheda di interfaccia di rete con [convergenza e RDMA Guest per Windows Server 2016](https://github.com/Microsoft/SDN/blob/master/Diagnostics/S2D%20WS2016_ConvergedNIC_Configuration.docx).

## <a name="step-3-configure-storage-spaces-direct"></a>Passaggio 3: Configurare Spazi di archiviazione diretta

I passaggi seguenti vengono eseguiti in un sistema di gestione con la stessa versione dei server da configurare. I passaggi seguenti non devono essere eseguiti in modalità remota tramite una sessione di PowerShell, ma vengono invece eseguiti in una sessione di PowerShell locale nel sistema di gestione, con autorizzazioni amministrative.

### <a name="step-31-clean-drives"></a>Passaggio 3,1: pulire le unità

Prima di abilitare Spazi di archiviazione diretta, assicurarsi che le unità siano vuote: nessuna partizione precedente o altri dati. Eseguire lo script seguente, sostituendo i nomi dei computer, per rimuovere tutte le partizioni obsolete o altri dati.

> [!Warning]
> Questo script rimuoverà definitivamente i dati in qualsiasi unità diversa dall'unità di avvio del sistema operativo.

```PowerShell
# Fill in these variables with your values
$ServerList = "Server01", "Server02", "Server03", "Server04"

Invoke-Command ($ServerList) {
    Update-StorageProviderCache
    Get-StoragePool | ? IsPrimordial -eq $false | Set-StoragePool -IsReadOnly:$false -ErrorAction SilentlyContinue
    Get-StoragePool | ? IsPrimordial -eq $false | Get-VirtualDisk | Remove-VirtualDisk -Confirm:$false -ErrorAction SilentlyContinue
    Get-StoragePool | ? IsPrimordial -eq $false | Remove-StoragePool -Confirm:$false -ErrorAction SilentlyContinue
    Get-PhysicalDisk | Reset-PhysicalDisk -ErrorAction SilentlyContinue
    Get-Disk | ? Number -ne $null | ? IsBoot -ne $true | ? IsSystem -ne $true | ? PartitionStyle -ne RAW | % {
        $_ | Set-Disk -isoffline:$false
        $_ | Set-Disk -isreadonly:$false
        $_ | Clear-Disk -RemoveData -RemoveOEM -Confirm:$false
        $_ | Set-Disk -isreadonly:$true
        $_ | Set-Disk -isoffline:$true
    }
    Get-Disk | Where Number -Ne $Null | Where IsBoot -Ne $True | Where IsSystem -Ne $True | Where PartitionStyle -Eq RAW | Group -NoElement -Property FriendlyName
} | Sort -Property PsComputerName, Count
```

L'output sarà simile al seguente, dove **count** è il numero di unità di ogni modello in ogni server:

```
Count Name                          PSComputerName
----- ----                          --------------
4     ATA SSDSC2BA800G4n            Server01
10    ATA ST4000NM0033              Server01
4     ATA SSDSC2BA800G4n            Server02
10    ATA ST4000NM0033              Server02
4     ATA SSDSC2BA800G4n            Server03
10    ATA ST4000NM0033              Server03
4     ATA SSDSC2BA800G4n            Server04
10    ATA ST4000NM0033              Server04
```

### <a name="step-32-validate-the-cluster"></a>Passaggio 3,2: convalidare il cluster

In questo passaggio si eseguirà lo strumento di convalida del cluster per assicurarsi che i nodi del server siano configurati correttamente per la creazione di un cluster con Spazi di archiviazione diretta. Quando la convalida del cluster (`Test-Cluster`) viene eseguita prima della creazione del cluster, esegue i test che verificano che la configurazione appaia adatta per funzionare correttamente come cluster di failover. Nell'esempio riportato di seguito viene utilizzato il parametro `-Include`, quindi vengono specificate le categorie specifiche dei test. Ciò assicura che i test specifici di Spazi di archiviazione diretta siano inclusi nella convalida.

Usare il comando seguente di PowerShell per convalidare un set di server per l'uso come cluster di Spazi di archiviazione diretta.

```PowerShell
Test-Cluster –Node <MachineName1, MachineName2, MachineName3, MachineName4> –Include "Storage Spaces Direct", "Inventory", "Network", "System Configuration"
```

### <a name="step-33-create-the-cluster"></a>Passaggio 3,3: creare il cluster

In questo passaggio verrà creato un cluster con i nodi convalidati per la creazione del cluster nel passaggio precedente tramite il cmdlet di PowerShell seguente.

Quando si crea il cluster, viene visualizzato un avviso indicante che si sono verificati problemi durante la creazione del ruolo del cluster che ne può impedire l'avvio. Per altre informazioni, visualizzare il file di report seguente". È possibile ignorare tale avviso. È causato dalla non disponibilità di dischi per il quorum del cluster. Si consiglia di configurare un controllo di condivisione file o un controllo cloud dopo la creazione del cluster.

> [!Note]
> Se i server usano indirizzi IP statici, modificare il comando seguente in modo da riflettere l'indirizzo IP statico aggiungendo il parametro seguente e specificando l'indirizzo IP: -StaticAddress &lt;X.X.X.X&gt;.
> Nel comando seguente il segnaposto ClusterName deve essere sostituito con un nome NetBIOS univoco che includa massimo 15 caratteri.
> ```PowerShell
> New-Cluster –Name <ClusterName> –Node <MachineName1,MachineName2,MachineName3,MachineName4> –NoStorage
> ```

Dopo aver creato il cluster, la replica della voce DNS per il nome del cluster può richiedere tempo. Il tempo richiesto dipende dall'ambiente e dalla configurazione della replica DNS. Se la risoluzione del cluster non ha esito positivo, nella maggior parte dei casi è possibile usare il nome del computer di un nodo che è membro attivo del cluster al posto del nome del cluster.

### <a name="step-34-configure-a-cluster-witness"></a>Passaggio 3,4: configurare un server di controllo del mirroring

Si consiglia di configurare un server di controllo del mirroring per il cluster, in modo che i cluster con tre o più server siano in grado di resistere a due server in errore o offline. Per una distribuzione a due server è necessario un server di controllo del mirroring. in caso contrario, il server offline causa anche l'indisponibilità dell'altro. Con questi sistemi, come controllo è possibile usare una condivisione file o un cloud di controllo. 

Per altre info, vedi i seguenti argomenti:

- [Configurare e gestire il quorum](../../failover-clustering/manage-cluster-quorum.md)
- [Distribuire un cloud di controllo per un cluster di failover](../../failover-clustering/deploy-cloud-witness.md)

### <a name="step-35-enable-storage-spaces-direct"></a>Passaggio 3.5: Abilitare Spazi di archiviazione diretta

Dopo aver creato il cluster, usare il cmdlet `Enable-ClusterStorageSpacesDirect` PowerShell, che consente di impostare il sistema di archiviazione in modalità di Spazi di archiviazione diretta ed eseguire automaticamente le operazioni seguenti:

-   **Crea un pool:** crea un singolo pool di grandi dimensioni con un nome simile a "S2D on Cluster1".

-   **Configura la cache di Spazi di archiviazione diretta:** se sono disponibili più tipi di supporto (unità) per l'uso di Spazi di archiviazione diretta, abilita il dispositivo di cache più veloce (nella maggior parte dei casi, in lettura e in scrittura).

-   **Livelli:** Crea due livelli come livelli predefiniti. Uno è denominato "Capacità" e l'altro "Prestazioni". Il cmdlet analizza i dispositivi e configura ogni livello con una combinazione di tipi di dispositivi e resilienza.

Eseguire il comando seguente dal sistema di gestione in una finestra di comandi di PowerShell aperta con privilegi di amministratore. Il nome del cluster corrisponde al cluster creato nei passaggi precedenti. Se questo comando viene eseguito localmente in uno dei nodi, il parametro -CimSession non è necessario.

```PowerShell
Enable-ClusterStorageSpacesDirect –CimSession <ClusterName>
```

Per abilitare Spazi di archiviazione diretta tramite il comando precedente, è anche possibile usare il nome del nodo anziché il nome del cluster. L'uso del nome del nodo potrebbe essere più affidabile a causa di ritardi di replica DNS che possono verificarsi con il nome del cluster appena creato.

Al termine dell'esecuzione di questo comando, che può richiedere alcuni minuti, il sistema è pronto per la creazione dei volumi.

### <a name="step-36-create-volumes"></a>Passaggio 3.6: Creare volumi

È consigliabile usare il cmdlet `New-Volume` perché fornisce un'esperienza più veloce e intuitiva. Questo cmdlet singolo crea automaticamente il disco virtuale, lo divide in partizioni e lo formatta, crea il volume con il nome corrispondente e lo aggiunge ai volumi condivisi del cluster, tutto in un unico, semplice passaggio.

Per ulteriori informazioni, consulta [Creazione di volumi in Spazi di archiviazione diretta](create-volumes.md).

### <a name="step-37-optionally-enable-the-csv-cache"></a>Passaggio 3,7: Abilitare facoltativamente la cache CSV

Facoltativamente, è possibile abilitare la cache del volume condiviso cluster (CSV) per usare la memoria di sistema (RAM) come cache a livello di blocco write-through delle operazioni di lettura che non sono già memorizzate nella cache da Gestione cache di Windows. Questo può migliorare le prestazioni per le applicazioni, ad esempio Hyper-V. La cache CSV può migliorare le prestazioni delle richieste di lettura ed è utile anche per scenari File server di scalabilità orizzontale.

L'abilitazione della cache CSV riduce la quantità di memoria disponibile per l'esecuzione di macchine virtuali in un cluster iperconvergente, quindi è necessario bilanciare le prestazioni di archiviazione con la memoria disponibile per i dischi rigidi virtuali.

Per impostare le dimensioni della cache CSV, aprire una sessione di PowerShell nel sistema di gestione con un account che disponga di autorizzazioni di amministratore per il cluster di archiviazione, quindi utilizzare questo script, modificando le variabili `$ClusterName` e `$CSVCacheSize` in base alle esigenze (in questo esempio viene impostata una cache CSV da 2 GB per Server):

```PowerShell
$ClusterName = "StorageSpacesDirect1"
$CSVCacheSize = 2048 #Size in MB

Write-Output "Setting the CSV cache..."
(Get-Cluster $ClusterName).BlockCacheSize = $CSVCacheSize

$CSVCurrentCacheSize = (Get-Cluster $ClusterName).BlockCacheSize
Write-Output "$ClusterName CSV cache size: $CSVCurrentCacheSize MB"
```

Per altre informazioni, vedere [uso della cache di lettura CSV in memoria](csv-cache.md).

### <a name="step-38-deploy-virtual-machines-for-hyper-converged-deployments"></a>Passaggio 3,8: distribuire macchine virtuali per distribuzioni iperconvergenti

Se si distribuisce un cluster iperconvergente, l'ultimo passaggio consiste nel eseguire il provisioning di macchine virtuali nel cluster Spazi di archiviazione diretta.

I file della macchina virtuale devono essere archiviati nello spazio dei nomi CSV dei sistemi (esempio: c:\\ClusterStorage\\volume1) proprio come le macchine virtuali in cluster nei cluster di failover.

È possibile usare strumenti predefiniti o altri strumenti per gestire l'archiviazione e le macchine virtuali, ad esempio System Center Virtual Machine Manager.

## <a name="step-4-deploy-scale-out-file-server-for-converged-solutions"></a>Passaggio 4: distribuire File server di scalabilità orizzontale per le soluzioni convergenti

Se si sta distribuendo una soluzione convergente, il passaggio successivo consiste nel creare un'istanza di File server di scalabilità orizzontale e configurare alcune condivisioni file. Se si distribuisce un cluster iperconvergente, si è pronti e non è necessaria questa sezione.

### <a name="step-41-create-the-scale-out-file-server-role"></a>Passaggio 4,1: creare il ruolo File server di scalabilità orizzontale

Il passaggio successivo per la configurazione dei servizi cluster per la file server consiste nel creare il ruolo di file server cluster, ovvero quando si crea l'istanza di File server di scalabilità orizzontale in cui sono ospitate le condivisioni file continuamente disponibili.

#### <a name="to-create-a-scale-out-file-server-role-by-using-server-manager"></a>Per creare un ruolo di File server di scalabilità orizzontale utilizzando Server Manager

1. In Gestione cluster di failover selezionare il cluster, passare a **ruoli**, quindi fare clic su **Configura ruolo...** .<br>Verrà visualizzata la configurazione guidata disponibilità elevata.
2. Nella pagina **Selezione ruolo** fare clic su **file server**.
3. Nella pagina **tipo di file server** fare clic su **file server di scalabilità orizzontale per dati applicazione**.
4. Nella pagina **punto di accesso client** Digitare un nome per il file server di scalabilità orizzontale.
5. Verificare che il ruolo sia stato configurato correttamente passando a **ruoli** e confermando che nella colonna **stato** sia indicato in **esecuzione** accanto al ruolo file server cluster creato, come illustrato nella figura 1.

   ![Screenshot del Gestione cluster di failover che mostra la File server di scalabilità orizzontale](media/Hyper-converged-solution-using-Storage-Spaces-Direct-in-Windows-Server-2016/SOFS_in_FCM.png "Gestione cluster di failover che mostra la File server di scalabilità orizzontale")

    **Figura 1** Gestione cluster di failover che mostra la File server di scalabilità orizzontale con lo stato in esecuzione

> [!NOTE]
>  Dopo aver creato il ruolo del cluster, potrebbero verificarsi ritardi di propagazione della rete che potrebbero impedire la creazione di condivisioni file su di esso per alcuni minuti o potenzialmente più lunghi.  
  
#### <a name="to-create-a-scale-out-file-server-role-by-using-windows-powershell"></a>Per creare un ruolo di File server di scalabilità orizzontale usando Windows PowerShell

 In una sessione di Windows PowerShell connessa al cluster di file server, immettere i comandi seguenti per creare il ruolo File server di scalabilità orizzontale, modificare *FSCLUSTER* in modo che corrisponda al nome del cluster e *SOFS* in modo che corrisponda al nome che si vuole assegnare al ruolo file server di scalabilità orizzontale:

```PowerShell
Add-ClusterScaleOutFileServerRole -Name SOFS -Cluster FSCLUSTER
```

> [!NOTE]
>  Dopo aver creato il ruolo del cluster, potrebbero verificarsi ritardi di propagazione della rete che potrebbero impedire la creazione di condivisioni file su di esso per alcuni minuti o potenzialmente più lunghi. Se il ruolo SOFS ha esito negativo immediatamente e non si avvia, è possibile che l'oggetto computer del cluster non disponga delle autorizzazioni necessarie per creare un account computer per il ruolo SOFS. Per informazioni, vedere questo post di Blog: il [ruolo file server di scalabilità orizzontale non viene avviato con gli ID evento 1205, 1069 e 1194](http://www.aidanfinn.com/?p=14142).

### <a name="step-42-create-file-shares"></a>Passaggio 4,2: creare condivisioni file

Dopo aver creato i dischi virtuali e averli aggiunti a CSVs, è possibile creare condivisioni file su di essi, una condivisione file per ogni volume condiviso cluster per ogni disco virtuale. System Center Virtual Machine Manager (VMM) è probabilmente il modo più pratico per eseguire questa operazione perché gestisce automaticamente le autorizzazioni, ma se non è presente nell'ambiente, è possibile utilizzare Windows PowerShell per automatizzare parzialmente la distribuzione.

Usare gli script inclusi nello script [Configurazione condivisione SMB per carichi di lavoro Hyper-V](https://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a) , che consente di automatizzare parzialmente il processo di creazione di gruppi e condivisioni. Viene scritto per i carichi di lavoro di Hyper-V, pertanto se si distribuiscono altri carichi di lavoro, potrebbe essere necessario modificare le impostazioni o eseguire ulteriori passaggi dopo aver creato le condivisioni. Se ad esempio si utilizza Microsoft SQL Server, è necessario concedere all'account del servizio SQL Server il controllo completo sulla condivisione e sul file system.

> [!NOTE]
>  È necessario aggiornare l'appartenenza al gruppo quando si aggiungono nodi del cluster, a meno che non si usi System Center Virtual Machine Manager per creare le condivisioni.

Per creare condivisioni file usando gli script di PowerShell, eseguire le operazioni seguenti:

1. Scaricare gli script inclusi nella [configurazione della condivisione SMB per i carichi di lavoro di Hyper-V](https://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a) in uno dei nodi del cluster file server.
2. Aprire una sessione di Windows PowerShell con credenziali di amministratore di dominio nel sistema di gestione, quindi usare lo script seguente per creare un gruppo di Active Directory per gli oggetti computer Hyper-V, modificando i valori per le variabili in base alle esigenze ambiente

    ```PowerShell
    # Replace the values of these variables
    $HyperVClusterName = "Compute01"
    $HyperVObjectADGroupSamName = "Hyper-VServerComputerAccounts" <#No spaces#>
    $ScriptFolder = "C:\Scripts\SetupSMBSharesWithHyperV"

    # Start of script itself
    CD $ScriptFolder
    .\ADGroupSetup.ps1 -HyperVObjectADGroupSamName $HyperVObjectADGroupSamName -HyperVClusterName $HyperVClusterName
    ```
3. Aprire una sessione di Windows PowerShell con credenziali di amministratore in uno dei nodi di archiviazione e quindi usare lo script seguente per creare condivisioni per ogni volume condiviso cluster e concedere autorizzazioni amministrative per le condivisioni al gruppo Domain Admins e al cluster di calcolo.

    ```PowerShell
    # Replace the values of these variables
    $StorageClusterName = "StorageSpacesDirect1"
    $HyperVObjectADGroupSamName = "Hyper-VServerComputerAccounts" <#No spaces#>
    $SOFSName = "SOFS"
    $SharePrefix = "Share"
    $ScriptFolder = "C:\Scripts\SetupSMBSharesWithHyperV"

    # Start of the script itself
    CD $ScriptFolder
    Get-ClusterSharedVolume -Cluster $StorageClusterName | ForEach-Object
    {
        $ShareName = $SharePrefix + $_.SharedVolumeInfo.friendlyvolumename.trimstart("C:\ClusterStorage\Volume")
        Write-host "Creating share $ShareName on "$_.name "on Volume: " $_.SharedVolumeInfo.friendlyvolumename
        .\FileShareSetup.ps1 -HyperVClusterName $StorageClusterName -CSVVolumeNumber $_.SharedVolumeInfo.friendlyvolumename.trimstart("C:\ClusterStorage\Volume") -ScaleOutFSName $SOFSName -ShareName $ShareName -HyperVObjectADGroupSamName $HyperVObjectADGroupSamName
    }
    ```

### <a name="step-43-enable-kerberos-constrained-delegation"></a>Passaggio 4,3 abilitare la delega vincolata Kerberos

Per configurare la delega vincolata Kerberos per la gestione di scenari remoti e aumentare Live Migration sicurezza, da uno dei nodi del cluster di archiviazione, usare lo script KCDSetup. ps1 incluso nella [configurazione della condivisione SMB per i carichi di lavoro di Hyper-V](https://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a). Ecco un piccolo wrapper per lo script:

```PowerShell
$HyperVClusterName = "Compute01"
$ScaleOutFSName = "SOFS"
$ScriptFolder = "C:\Scripts\SetupSMBSharesWithHyperV"

CD $ScriptFolder
.\KCDSetup.ps1 -HyperVClusterName $HyperVClusterName -ScaleOutFSName $ScaleOutFSName -EnableLM
```

## <a name="next-steps"></a>Passaggi successivi

Dopo aver distribuito il file server in cluster, è consigliabile testare le prestazioni della soluzione usando carichi di lavoro sintetici prima di attivare i carichi di lavoro reali. Ciò consente di verificare che la soluzione venga eseguita correttamente e di risolvere eventuali problemi persistenti prima di aggiungere la complessità dei carichi di lavoro. Per altre informazioni, vedere [testare le prestazioni di spazi di archiviazione usando carichi di lavoro sintetici](https://technet.microsoft.com/library/dn894707.aspx).

## <a name="see-also"></a>Vedere anche

-   [Spazi di archiviazione diretta in Windows Server 2016](storage-spaces-direct-overview.md)
-   [Comprendere la cache in Spazi di archiviazione diretta](understand-the-cache.md)
-   [Pianificazione di volumi in Spazi di archiviazione diretta](plan-volumes.md)
-   [Tolleranza di errore di spazi di archiviazione](storage-spaces-fault-tolerance.md)
-   [Requisiti hardware Spazi di archiviazione diretta](Storage-Spaces-Direct-Hardware-Requirements.md)
-   [Utilizzare o meno RDMA](https://blogs.technet.microsoft.com/filecab/2017/03/27/to-rdma-or-not-to-rdma-that-is-the-question/) (TechNet blog)
