---
title: Distribuire spazi di archiviazione diretta
ms.prod: windows-server-threshold
manager: eldenc
ms.author: stevenek
ms.technology: storage-spaces
ms.topic: get-started-article
ms.assetid: 20fee213-8ba5-4cd3-87a6-e77359e82bc0
author: stevenek
ms.date: 06/07/2019
description: Istruzioni dettagliate per distribuire archiviazione software-defined con spazi di archiviazione diretta in Windows Server come infrastruttura iperconvergente o infrastruttura convergente (noto anche come disaggregata).
ms.localizationpriority: medium
ms.openlocfilehash: a4159c85be23025ef57084b47dcc77d4f749888f
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812360"
---
# <a name="deploy-storage-spaces-direct"></a>Distribuire spazi di archiviazione diretta

> Si applica a: Windows Server 2019, Windows Server 2016

In questo argomento vengono fornite istruzioni dettagliate per distribuire [spazi di archiviazione diretta](storage-spaces-direct-overview.md).

> [!Tip]
> Se si desidera per acquisire Hyper-Converged infrastruttura, Microsoft consiglia di acquistare una soluzione di hardware e software convalidati dai nostri partner, tra cui procedure e strumenti di distribuzione. Queste soluzioni sono progettate, assemblate e convalidate in base a nostra architettura di riferimento per garantire rapidità e affidabilità, in modo da essere operativi e compatibilità. Per le soluzioni Windows Server 2019, visitare il [sito Web di soluzioni di Azure Stack uomo](https://azure.microsoft.com/overview/azure-stack/hci). Per le soluzioni Windows Server 2016, altre informazioni, vedi [Windows Server Software-Defined](https://microsoft.com/wssd).

> [!Tip]
> È possibile usare macchine virtuali Hyper-V, incluso in Microsoft Azure, al [valutare spazi di archiviazione diretta senza hardware](storage-spaces-direct-in-vm.md). È anche possibile esaminare il comodo [script di distribuzione di Windows Server lab rapido](https://aka.ms/wslab), che viene usato per il training.

## <a name="before-you-start"></a>Prima di iniziare

Rivedere le [requisiti hardware di spazi di archiviazione diretta](Storage-Spaces-Direct-Hardware-Requirements.md) e limita a scorrere questo documento per acquisire familiarità con l'approccio complessivo e note importanti associate alcuni passaggi.

Raccogliere le informazioni seguenti:

- **Opzione di distribuzione.** Supporta la funzionalità spazi di archiviazione diretta [due opzioni di distribuzione: iperconvergente e convergente](storage-spaces-direct-overview.md#deployment-options), noto anche come disaggregata. Acquisire familiarità con i vantaggi della ognuno a decidere quale sia adatta a te. I passaggi 1 e 3 riportato di seguito si applicano a entrambe le opzioni di distribuzione. Passaggio 4 è necessario solo per la distribuzione convergente.

- **Nomi dei server.** Acquisire familiarità con i criteri di denominazione dell'organizzazione per i computer, file, percorsi e altre risorse. È necessario eseguire il provisioning di server diversi, ognuno con nomi univoci.

- **Nome di dominio.** Acquisire familiarità con i criteri dell'organizzazione per la denominazione dei domini e aggiunta al dominio.  È il server di partecipare al dominio, ed è necessario specificare il nome di dominio. 

- **Rete RDMA.** Esistono due tipi di protocolli RDMA: iWarp e RoCE. Si noti che quello di usare le schede di rete, e se RoCE, si noti inoltre la versione (v1 o v2). Per RoCE, si noti inoltre il modello del commutatore top-of-rack.

- **VLAN ID.** Prendere nota dell'ID di VLAN da usare per schede di rete di gestione del sistema operativo nei server, se presente. È possibile ottenere queste informazioni dall'amministratore di rete.

## <a name="step-1-deploy-windows-server"></a>Passaggio 1: Distribuzione di Windows Server

### <a name="step-11-install-the-operating-system"></a>Passaggio 1.1: Installare il sistema operativo

Il primo passaggio è installare Windows Server in ogni server che saranno nel cluster. Spazi di archiviazione diretta richiede Windows Server 2016 Datacenter Edition. È possibile usare l'opzione di installazione Server Core o Server con esperienza Desktop.

Quando si installa Windows Server usando l'installazione guidata, è possibile scegliere tra *Windows Server* (riferimento a Server Core) e *Windows Server (Server con esperienza Desktop)* , che è l'equivalente del *completo* opzione di installazione disponibile in Windows Server 2012 R2. Se non si sceglie, si otterrà l'opzione di installazione Server Core. Per altre informazioni, vedere [opzioni di installazione per Windows Server 2016](../../get-started/Windows-Server-2016.md).

### <a name="step-12-connect-to-the-servers"></a>Passaggio 1.2: Connettersi ai server

Questa guida descrive l'opzione di installazione Server Core e la distribuzione/gestione in remoto da un sistema di gestione separato che deve avere:

- Windows Server 2016 con gli stessi aggiornamenti dei server gestiti
- Connettività di rete ai server gestiti
- Aggiunto al dominio stesso o a un dominio completamente attendibile
- Ha gli strumenti di amministrazione remota del server (RSAT, Remote Server Administration Tools) e i moduli PowerShell per Hyper-V e per il clustering di failover. Strumenti di amministrazione remota del server e i moduli di PowerShell sono disponibili in Windows Server e possono essere installati senza installare altre funzionalità. È anche possibile installare il [strumenti di amministrazione remota del Server](https://www.microsoft.com/download/details.aspx?id=45520) su un computer di gestione di Windows 10.

Nel sistema di gestione installare il cluster di failover e gli strumenti di gestione di Hyper-V. Questa operazione può essere eseguita tramite Server Manager usando l'**Aggiunta guidata ruoli e funzionalità**. Nella pagina **Funzionalità** selezionare **Strumenti di amministrazione remota del Server**, quindi selezionare gli strumenti da installare.

Immettere la sessione PS e usare il nome del server o l'indirizzo IP del nodo a cui connettersi. Sarà richiesto di immettere una password dopo aver eseguito questo comando, immettere la password amministratore specificata durante la configurazione di Windows.

   ```PowerShell
   Enter-PSSession -ComputerName <myComputerName> -Credential LocalHost\Administrator
   ```

   Di seguito è riportato un esempio di eseguire la stessa operazione in modo che sia più utile per gli script, nel caso in cui è necessario eseguire questa operazione più volte:

   ```PowerShell
   $myServer1 = "myServer-1"
   $user = "$myServer1\Administrator"

   Enter-PSSession -ComputerName $myServer1 -Credential $user
   ```

> [!TIP]
> Se si distribuisce in modalità remota da un sistema di gestione, si potrebbe ottenere un errore, ad esempio *WinRM non è in grado di elaborare la richiesta.* Per risolvere questo problema, usare Windows PowerShell per aggiungere ogni server all'elenco di host attendibili nel computer di gestione:  
>   
> `Set-Item WSMAN:\Localhost\Client\TrustedHosts -Value Server01 -Force`
>  
> Nota: l'elenco di host attendibili supporta i caratteri jolly, ad esempio `Server*`.
>
> Per visualizzare l'elenco di host attendibili, digitare `Get-Item WSMAN:\Localhost\Client\TrustedHosts`.  
>   
> Per svuotare l'elenco, digitare `Clear-Item WSMAN:\Localhost\Client\TrustedHost`.  

### <a name="step-13-join-the-domain-and-add-domain-accounts"></a>Passaggio 1.3: Aggiunta al dominio e aggiungere gli account di dominio

Finora è stato configurato singoli server con l'account amministratore locale, `<ComputerName>\Administrator`.

Per gestire spazi di archiviazione diretta, è necessario aggiungere i server a un dominio e usare un account di dominio Active Directory Domain Services presente nel gruppo Administrators in tutti i server.

Da sistema di gestione aprire una console di PowerShell con privilegi di amministratore. Usare `Enter-PSSession` per connettersi a ogni server ed eseguire il cmdlet seguente, sostituendo il proprio nome del computer, nome di dominio e le credenziali di dominio:

```PowerShell  
Add-Computer -NewName "Server01" -DomainName "contoso.com" -Credential "CONTOSO\User" -Restart -Force  
```

Se l'account di amministratore di archiviazione non è un membro del gruppo Domain Admins, aggiungere l'account di amministratore di archiviazione al gruppo Administrators locale in ogni nodo, o meglio ancora, aggiungere il gruppo usato per gli amministratori di archiviazione. È possibile usare il comando seguente (o scrivere una funzione di Windows PowerShell per eseguire questa operazione, vedere la [usare PowerShell per aggiungere utenti del dominio a un gruppo locale](http://blogs.technet.com/b/heyscriptingguy/archive/2010/08/19/use-powershell-to-add-domain-users-to-a-local-group.aspx) per altre informazioni):

```
Net localgroup Administrators <Domain\Account> /add
```

### <a name="step-14-install-roles-and-features"></a>Passaggio 1.4: Installare ruoli e funzionalità

Il passaggio successivo è installare ruoli del server in ogni server. È possibile farlo usando [Windows Admin Center](../../manage/windows-admin-center/use/manage-servers.md), [Server Manager](../../administration/server-manager/install-or-uninstall-roles-role-services-or-features.md)), o PowerShell. Di seguito sono i ruoli da installare:

- Clustering di failover
- Hyper-V
- File Server (se si vuole ospitare le condivisioni file, ad esempio per una distribuzione convergente)
- Data-Center-Bridging (se si utilizza RoCEv2 anziché schede di rete iWARP)
- RSAT-Clustering-PowerShell
- Hyper-V-PowerShell

Per installare tramite PowerShell, usare il [Install-WindowsFeature](https://docs.microsoft.com/powershell/module/microsoft.windows.servermanager.migration/install-windowsfeature) cmdlet. È possibile usarla in un singolo server simile al seguente:

```PowerShell
Install-WindowsFeature -Name "Hyper-V", "Failover-Clustering", "Data-Center-Bridging", "RSAT-Clustering-PowerShell", "Hyper-V-PowerShell", "FS-FileServer"
```

Per eseguire il comando in tutti i server del cluster contemporaneamente, usare questo piccolo bit dello script, la modifica dell'elenco di variabili all'inizio dello script per adattare l'ambiente.

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

Se si distribuisce spazi di archiviazione diretta all'interno delle macchine virtuali, ignorare questa sezione.

Spazi di archiviazione diretta richiede larghezza di banda elevata, bassa latenza di rete tra i server del cluster. Funzionalità di rete costituita da almeno 10 GbE è necessario e accesso diretto a memoria remota (RDMA) è consigliato. È possibile usare iWARP o RoCE fino a quando è presente il logo di Windows Server 2016, ma iWARP viene in genere più semplice da configurare.

> [!Important]
> A seconda le apparecchiature di rete e in particolare con RoCE v2, alcune operazioni di configurazione del commutatore top-of-rack potrebbe essere necessario. Configurazione del commutatore corretta è importante per garantire affidabilità e prestazioni di spazi di archiviazione diretta.

Windows Server 2016 introduce switch embedded teaming (SET) nel commutatore virtuale Hyper-V. In questo modo le stesse porte di interfaccia di rete fisiche da utilizzare per tutto il traffico di rete durante l'uso di RDMA, riducendo il numero di porte NIC fisiche necessarie. Opzione incorporato teaming è consigliato per spazi di archiviazione diretta.

Per istruzioni su come configurare la rete per spazi di archiviazione diretta, vedere [convergente NIC di Windows Server 2016 e Guida alla distribuzione di RDMA Guest](https://github.com/Microsoft/SDN/blob/master/Diagnostics/S2D%20WS2016_ConvergedNIC_Configuration.docx).

## <a name="step-3-configure-storage-spaces-direct"></a>Passaggio 3: Configurare Spazi di archiviazione diretta

I passaggi seguenti vengono eseguiti in un sistema di gestione con la stessa versione dei server da configurare. I passaggi seguenti dovrebbero non essere eseguiti in modalità remota tramite una sessione di PowerShell, ma eseguiti in una sessione di PowerShell locale nel sistema di gestione, con autorizzazioni amministrative.

### <a name="step-31-clean-drives"></a>Passaggio 3.1: Pulitura unità

Prima di abilitare spazi di archiviazione diretta, assicurarsi che le unità siano vuote: nessun partizioni precedenti o altri dati. Eseguire lo script seguente, sostituendo i nomi di computer, per rimuovere tutte le eventuali partizioni precedenti o altri dati.

> [!Warning]
> Questo script rimuoverà definitivamente tutti i dati su qualsiasi unità diverse unità di avvio del sistema operativo.

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

L'output avrà un aspetto simile al seguente, dove **conteggio** è il numero di unità di ogni modello in ogni server:

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

### <a name="step-32-validate-the-cluster"></a>Passaggio 3.2: Convalida del cluster

In questo passaggio si eseguirà lo strumento di convalida del cluster per garantire che i nodi del server siano configurati correttamente per creare un cluster con spazi di archiviazione diretta. Convalida del cluster quando (`Test-Cluster`) viene eseguito prima della creazione del cluster, viene eseguito il test per verificare che la configurazione viene visualizzata funzionerà correttamente come cluster di failover. Nell'esempio seguente viene usato il `-Include` vengono specificati parametri, quindi selezionare le categorie specifiche di test. Ciò assicura che i test specifici di Spazi di archiviazione diretta siano inclusi nella convalida.

Usare il comando seguente di PowerShell per convalidare un set di server per l'uso come cluster di Spazi di archiviazione diretta.

```PowerShell
Test-Cluster –Node <MachineName1, MachineName2, MachineName3, MachineName4> –Include "Storage Spaces Direct", "Inventory", "Network", "System Configuration"
```

### <a name="step-33-create-the-cluster"></a>Passaggio 3.3: creare il cluster

In questo passaggio si creerà un cluster con i nodi convalidati per la creazione del cluster nel passaggio precedente usando il cmdlet di PowerShell seguente.

Quando si crea il cluster, si riceverà un avviso che indica: "si sono verificati problemi durante la creazione del ruolo del cluster che potrebbe impedire l'avvio. Per altre informazioni, visualizzare il file di report seguente". È possibile ignorare questo avviso. È causato dalla non disponibilità di dischi per il quorum del cluster. Si consiglia di configurare un controllo di condivisione file o un controllo cloud dopo la creazione del cluster.

> [!Note]
> Se i server usano indirizzi IP statici, modificare il comando seguente in modo da riflettere l'indirizzo IP statico aggiungendo il parametro seguente e specificando l'indirizzo IP: -StaticAddress &lt;X.X.X.X&gt;.
> Nel comando seguente il segnaposto ClusterName deve essere sostituito con un nome NetBIOS univoco che includa massimo 15 caratteri.
> ```PowerShell
> New-Cluster –Name <ClusterName> –Node <MachineName1,MachineName2,MachineName3,MachineName4> –NoStorage
> ```

Dopo aver creato il cluster, la replica della voce DNS per il nome del cluster può richiedere tempo. Il tempo richiesto dipende dall'ambiente e dalla configurazione della replica DNS. Se la risoluzione del cluster non ha esito positivo, nella maggior parte dei casi è possibile usare il nome del computer di un nodo che è membro attivo del cluster al posto del nome del cluster.

### <a name="step-34-configure-a-cluster-witness"></a>Passaggio 3.4: Configurare un controllo di cluster

È consigliabile configurare un controllo per il cluster, in modo che i cluster con tre o più server possono persistere se due server non funzionano o sono offline. Una distribuzione a due server richiede un controllo di cluster, in caso contrario, uno dei due server passerà offline fa sì che l'altro risulterà non disponibile. Con questi sistemi, come controllo è possibile usare una condivisione file o un cloud di controllo. 

Per altre info, vedi i seguenti argomenti:

- [Configurare e gestire il quorum](../../failover-clustering/manage-cluster-quorum.md)
- [Distribuire un Cloud di controllo per un Cluster di Failover](../../failover-clustering/deploy-cloud-witness.md)

### <a name="step-35-enable-storage-spaces-direct"></a>Passaggio 3.5: Abilitare Spazi di archiviazione diretta

Dopo aver creato il cluster, usare il `Enable-ClusterStorageSpacesDirect` cmdlet di PowerShell, che verrà inserito il sistema di archiviazione in modalità spazi di archiviazione diretta e automaticamente le operazioni seguenti:

-   **Creare un pool:** Crea un singolo pool di grandi dimensioni con un nome simile a "S2D in Cluster1".

-   **Configura le cache di spazi di archiviazione diretta:** Se è presente più di un supporto tipo (unità) disponibile per l'uso di spazi di archiviazione diretta, consente più velocemente come dispositivo di cache (lettura e scrittura nella maggior parte dei casi)

-   **Livelli:** Crea due livelli come livelli predefiniti. Uno è denominato "Capacità" e l'altro "Prestazioni". Il cmdlet analizza i dispositivi e configura ogni livello con una combinazione di tipi di dispositivi e resilienza.

Eseguire il comando seguente dal sistema di gestione in una finestra di comandi di PowerShell aperta con privilegi di amministratore. Il nome del cluster corrisponde al cluster creato nei passaggi precedenti. Se questo comando viene eseguito localmente in uno dei nodi, il parametro -CimSession non è necessario.

```PowerShell
Enable-ClusterStorageSpacesDirect –CimSession <ClusterName>
```

Per abilitare Spazi di archiviazione diretta tramite il comando precedente, è anche possibile usare il nome del nodo anziché il nome del cluster. L'uso del nome del nodo potrebbe essere più affidabile a causa di ritardi di replica DNS che possono verificarsi con il nome del cluster appena creato.

Al termine dell'esecuzione di questo comando, che può richiedere alcuni minuti, il sistema è pronto per la creazione dei volumi.

### <a name="step-36-create-volumes"></a>Passaggio 3.6: Creare volumi

È consigliabile usare il `New-Volume` cmdlet perché offre un'esperienza più semplice e rapida. Questo cmdlet singolo crea automaticamente il disco virtuale, lo divide in partizioni e lo formatta, crea il volume con il nome corrispondente e lo aggiunge ai volumi condivisi del cluster, tutto in un unico, semplice passaggio.

Per ulteriori informazioni, consulta [Creazione di volumi in Spazi di archiviazione diretta](create-volumes.md).

### <a name="step-37-optionally-enable-the-csv-cache"></a>Passaggio 3.7: Facoltativamente, abilitare la cache CSV

È possibile abilitare facoltativamente la cache del volume (CSV) condivisi del cluster per l'utilizzo della memoria di sistema (RAM) come una cache write-through a livello di blocco delle operazioni di lettura che non sono già memorizzati nella cache per la gestione della cache di Windows. Ciò può migliorare le prestazioni per applicazioni quali Hyper-V. La cache CSV può migliorare le prestazioni delle richieste di lettura ed è anche utile per gli scenari di tipo Scale-Out File Server.

Abilitare la cache CSV consente di ridurre la quantità di memoria disponibile per l'esecuzione di macchine virtuali in un cluster iperconvergente, pertanto è necessario bilanciare le prestazioni di archiviazione con memoria disponibile per i dischi rigidi virtuali.

Per impostare le dimensioni della cache CSV, aprire una sessione di PowerShell nel sistema di gestione con un account che dispone delle autorizzazioni di amministratore nel cluster di archiviazione e quindi usare questo script, modificando la `$ClusterName` e `$CSVCacheSize` variabili appropriate (in questo esempio si imposta una cache CSV di 2 GB per ogni server):

```PowerShell
$ClusterName = "StorageSpacesDirect1"
$CSVCacheSize = 2048 #Size in MB

Write-Output "Setting the CSV cache..."
(Get-Cluster $ClusterName).BlockCacheSize = $CSVCacheSize

$CSVCurrentCacheSize = (Get-Cluster $ClusterName).BlockCacheSize
Write-Output "$ClusterName CSV cache size: $CSVCurrentCacheSize MB"
```

Per altre informazioni, vedi [uso di CSV in memoria cache di lettura](csv-cache.md).

### <a name="step-38-deploy-virtual-machines-for-hyper-converged-deployments"></a>Passaggio 3.8: Distribuire macchine virtuali per le distribuzioni iperconvergenti

Se si distribuisce un cluster iperconvergente, l'ultimo passaggio è eseguire il provisioning di macchine virtuali nel cluster di spazi di archiviazione diretta.

File della macchina virtuale devono essere archiviati nello spazio dei nomi CSV sistemi (esempio: c:\\ClusterStorage\\Volume1) come macchine virtuali in cluster nei cluster di failover.

È possibile utilizzare strumenti integrati o altri strumenti per gestire l'archiviazione e macchine virtuali, ad esempio System Center Virtual Machine Manager.

## <a name="step-4-deploy-scale-out-file-server-for-converged-solutions"></a>Passaggio 4: Distribuire File Server di scalabilità orizzontale per le soluzioni per la convergenza

Se si distribuisce una soluzione convergente, il passaggio successivo è creare un'istanza di Scale-Out File Server e alcune condivisioni file di installazione. Se si distribuisce un cluster iperconvergente - è terminata ed è non necessario di questa sezione.

### <a name="step-41-create-the-scale-out-file-server-role"></a>Passaggio 4.1: Creare il ruolo File Server di scalabilità orizzontale

Il passaggio successivo nella configurazione di servizi cluster per il file server consiste nel creare il ruolo server di cluster di file, ovvero quando si crea l'istanza di Scale-Out File Server in cui sono ospitate le condivisioni file sempre disponibili.

#### <a name="to-create-a-scale-out-file-server-role-by-using-server-manager"></a>Per creare un ruolo di tipo Scale-Out File Server tramite Server Manager

1. In Gestione Cluster di Failover, selezionare il cluster, passare a **ruoli**, quindi fare clic su **Configura ruolo...** .<br>Viene visualizzata la configurazione guidata disponibilità elevata.
2. Nel **selezionare il ruolo** pagina, fare clic su **File Server**.
3. Nel **tipo di File Server** pagina, fare clic su **Scale-Out File Server per i dati dell'applicazione**.
4. Nel **punto di accesso Client** , digitare un nome per il File Server di scalabilità orizzontale.
5. Verificare che il ruolo è stato configurato correttamente passando a **ruoli** e conferma che il **stato** colonna Mostra **esecuzione** accanto al ruolo cluster file server è stato creato, come illustrato nella figura 1.

   ![Screenshot di gestione Cluster di Failover che mostra il File Server di scalabilità orizzontale](media/Hyper-converged-solution-using-Storage-Spaces-Direct-in-Windows-Server-2016/SOFS_in_FCM.png "gestione Cluster di Failover che mostra il File Server di scalabilità orizzontale")

    **Figura 1** gestione Cluster di Failover con Scale-Out File Server con lo stato di esecuzione

> [!NOTE]
>  Dopo aver creato il ruolo del cluster, potrebbe essere presenti alcuni rete ritardi di propagazione che potrebbero impedire la creazione di condivisioni file su di esso per alcuni minuti, o potenzialmente lunga.  
  
#### <a name="to-create-a-scale-out-file-server-role-by-using-windows-powershell"></a>Per creare un ruolo di tipo Scale-Out File Server con Windows PowerShell

 In una sessione di Windows PowerShell che è connesso al cluster di file server, immettere i comandi seguenti per creare il ruolo File Server di scalabilità orizzontale, modificando *FSCLUSTER* corrisponda al nome del cluster, e *SOFS* corrisponda al nome da assegnare il ruolo File Server di scalabilità orizzontale:

```PowerShell
Add-ClusterScaleOutFileServerRole -Name SOFS -Cluster FSCLUSTER
```

> [!NOTE]
>  Dopo aver creato il ruolo del cluster, potrebbe essere presenti alcuni rete ritardi di propagazione che potrebbero impedire la creazione di condivisioni file su di esso per alcuni minuti, o potenzialmente lunga. Se il ruolo SOFS ha immediatamente esito negativo e non si avvia, è possibile che l'oggetto computer del cluster non è autorizzato a creare un account computer per il ruolo del SOFS. Per assistenza, argomento, vedere questo post di blog: [Scale-Out File Server ruolo non è possibile iniziare con gli ID evento 1205, 1069 e 1194](http://www.aidanfinn.com/?p=14142).

### <a name="step-42-create-file-shares"></a>Passaggio 4.2: Creare condivisioni file

Dopo aver creato i dischi virtuali e averle aggiunte a volumi condivisi cluster, è possibile creare condivisioni file su di essi - una condivisione file per ogni CSV per ogni disco virtuale. System Center Virtual Machine Manager (VMM) è probabilmente il modo handiest per eseguire questa operazione perché gestisce le autorizzazioni per l'utente, ma se non è nell'ambiente in uso, è possibile usare Windows PowerShell per automatizzare parzialmente la distribuzione.

Usare gli script inclusi nel [configurazione della condivisione SMB per i carichi di lavoro di Hyper-V](http://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a) uno script che parzialmente automatizza il processo di creazione di gruppi e le condivisioni. È scritta per carichi di lavoro Hyper-V, in modo che se si distribuisce altri carichi di lavoro, potrebbe essere necessario modificare le impostazioni o eseguire passaggi aggiuntivi dopo aver creato le condivisioni. Ad esempio, se si usa Microsoft SQL Server, l'account del servizio SQL Server deve concedere il controllo completo su condivisione e file system.

> [!NOTE]
>  È possibile aggiornare l'appartenenza al gruppo quando si aggiungono i nodi del cluster se non si usa System Center Virtual Machine Manager per creare le condivisioni.

Per creare condivisioni file tramite gli script di PowerShell, eseguire le operazioni seguenti:

1. Scaricare gli script inclusi in [configurazione della condivisione SMB per i carichi di lavoro di Hyper-V](http://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a) a uno dei nodi del cluster di file server.
2. Aprire una sessione di Windows PowerShell con le credenziali di amministratore di dominio nel sistema di gestione e quindi usare lo script seguente per creare un gruppo di Active Directory per gli oggetti computer Hyper-V, modificando i valori per le variabili nel modo appropriato per il ambiente:

    ```PowerShell
    # Replace the values of these variables
    $HyperVClusterName = "Compute01"
    $HyperVObjectADGroupSamName = "Hyper-VServerComputerAccounts" <#No spaces#>
    $ScriptFolder = "C:\Scripts\SetupSMBSharesWithHyperV"

    # Start of script itself
    CD $ScriptFolder
    .\ADGroupSetup.ps1 -HyperVObjectADGroupSamName $HyperVObjectADGroupSamName -HyperVClusterName $HyperVClusterName
    ```
3. Aprire una sessione di Windows PowerShell con le credenziali di amministratore su uno dei nodi di archiviazione e quindi usare lo script seguente per creare le condivisioni per ogni volume condiviso cluster e concedere le autorizzazioni amministrative per le condivisioni al gruppo Domain Admins e il cluster di calcolo.

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

### <a name="step-43-enable-kerberos-constrained-delegation"></a>Passaggio 4.3 abilitare Kerberos per la delega vincolata

Per configurare la delega vincolata Kerberos per uno scenario remoto gestione e migliorare la protezione di Live Migration, da uno dei nodi del cluster di archiviazione, usare lo script KCDSetup.ps1 incluso [configurazione della condivisione SMB per carichi di lavoro di Hyper-V](http://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a). Ecco un piccolo wrapper per lo script:

```PowerShell
$HyperVClusterName = "Compute01"
$ScaleOutFSName = "SOFS"
$ScriptFolder = "C:\Scripts\SetupSMBSharesWithHyperV"

CD $ScriptFolder
.\KCDSetup.ps1 -HyperVClusterName $HyperVClusterName -ScaleOutFSName $ScaleOutFSName -EnableLM
```

## <a name="next-steps"></a>Passaggi successivi

Dopo aver distribuito il cluster file server, è consigliabile testare le prestazioni della soluzione usando carichi di lavoro sintetici prima di disconnettere tutti i carichi di lavoro reali. Ciò consente di verificare che la soluzione funzioni correttamente e risolvere gli eventuali problemi residui prima di aggiungere la complessità dei carichi di lavoro. Per altre informazioni, vedi [Test archiviazione spazi delle prestazioni con carichi di lavoro sintetici](https://technet.microsoft.com/library/dn894707.aspx).

## <a name="see-also"></a>Vedere anche

-   [Spazi di archiviazione diretta in Windows Server 2016](storage-spaces-direct-overview.md)
-   [Comprendere la cache in spazi di archiviazione diretta](understand-the-cache.md)
-   [Pianificazione di volumi in spazi di archiviazione diretta](plan-volumes.md)
-   [Tolleranza di errore di spazi di archiviazione](storage-spaces-fault-tolerance.md)
-   [Spazi di archiviazione diretta i requisiti Hardware](Storage-Spaces-Direct-Hardware-Requirements.md)
-   [Utilizzare o meno RDMA](https://blogs.technet.microsoft.com/filecab/2017/03/27/to-rdma-or-not-to-rdma-that-is-the-question/) (TechNet blog)
