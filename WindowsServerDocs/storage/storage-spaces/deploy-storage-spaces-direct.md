---
title: Distribuire spazi di archiviazione diretta
ms.prod: windows-server-threshold
manager: eldenc
ms.author: stevenek
ms.technology: storage-spaces
ms.topic: get-started-article
ms.assetid: 20fee213-8ba5-4cd3-87a6-e77359e82bc0
author: stevenek
ms.date: 8/16/2018
description: Istruzioni dettagliate per la distribuzione di archiviazione software-defined con spazi di archiviazione diretta in Windows Server come un'infrastruttura iperconvergente o infrastruttura convergente (detto anche disaggregata).
ms.localizationpriority: medium
ms.openlocfilehash: 55cfa0e066506d7174f9e5b1e61cc0aa290706d7
ms.sourcegitcommit: 65e4f760be73104e67847f77e834e7b5e065211b
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/23/2018
ms.locfileid: "5429043"
---
# Distribuire spazi di archiviazione diretta

> Si applica a: Windows Server 2019, Windows Server 2016

Questo argomento fornisce istruzioni dettagliate su come distribuire [Spazi di archiviazione diretta](storage-spaces-direct-overview.md).

> [!Tip]
> Stai cercando di acquisire l'infrastruttura iperconvergente? Microsoft consiglia queste soluzioni [Windows Server Software-Defined](https://microsoft.com/wssd) dai nostri partner. Sono progettate, assemblati e convalidate nostra architettura di riferimento per garantire la compatibilità e affidabilità, in modo che ottenere fino ed eseguire rapidamente.

> [!Tip]
> È possibile utilizzare macchine virtuali Hyper-V, incluso in Microsoft Azure, per [valutare spazi di archiviazione diretta senza hardware](storage-spaces-direct-in-vm.md). Puoi anche esaminare gli [script di distribuzione di Windows Server lab rapidi](https://aka.ms/wslab), che usiamo per scopi di formazione.

## Prima di iniziare

Esaminare i [requisiti hardware di spazi di archiviazione diretta](Storage-Spaces-Direct-Hardware-Requirements.md) e scorrere questo documento per acquisire familiarità con l'approccio generale e note importanti associate ad alcuni passaggi.

Raccogliere le informazioni seguenti:

- **Opzione di distribuzione.** Spazi di archiviazione diretta supporta [due opzioni di distribuzione: iperconvergente e convergenti](storage-spaces-direct-overview.md#deployment-options), detto anche disaggregata. Acquisire familiarità con i vantaggi di ogni decidere che fa per te. I passaggi 1-3 seguente si applicano a entrambe le opzioni di distribuzione. Passaggio 4 è necessaria solo per la distribuzione convergente.

- **Nomi dei server.** Acquisisci familiarità con i criteri di denominazione dell'organizzazione per i computer, file, percorsi e altre risorse. È necessario effettuare il provisioning dei server diversi, ognuno con nomi univoci.

- **Nome di dominio.** Acquisisci familiarità con i criteri dell'organizzazione per la denominazione dei domini e aggiunta ai domini.  Dovrai aggiungere i server al dominio e dovrai specificare il nome di dominio. 

- **Reti RDMA.** Esistono due tipi di protocolli RDMA: iWarp e RoCE. Nota capire quale usano le schede di rete, e se RoCE, nota anche la versione (v1 o v2). Per RoCE, nota anche il modello del commutatore top-of-rack.

- **ID VLAN.** Prendere nota dell'ID VLAN per essere utilizzato per le schede di rete di gestione del sistema operativo nei server, se presente. È possibile ottenere queste informazioni dall'amministratore di rete.

## Passaggio 1: Distribuire Windows Server

### Passaggio 1.1: Installazione del sistema operativo

Il primo passaggio consiste nell'installare Windows Server in ogni server che saranno del cluster. Spazi di archiviazione diretta richiede Windows Server 2016 Datacenter Edition. È possibile utilizzare l'opzione di installazione Server Core o Server con esperienza Desktop.

Quando si installa Windows Server tramite l'installazione guidata, è possibile scegliere tra *Windows Server* (che fa riferimento a Server Core) e *Windows Server (Server con esperienza Desktop)*, che è l'equivalente dell'opzione installazione *completa* disponibile in Windows Server 2012 R2. Se non si sceglie, riceverai l'opzione di installazione Server Core. Per altre informazioni, vedi [le opzioni di installazione per Windows Server 2016](../../get-started/Windows-Server-2016.md).

### Passaggio 1.2: Connettersi ai server

Questa guida è incentrata l'opzione di installazione Server Core e la distribuzione o la gestione in modalità remota da un sistema di gestione separata, che deve avere:

- Windows Server 2016 con gli stessi aggiornamenti dei server gestiti
- Connettività di rete ai server gestiti
- Parte dello stesso dominio o un dominio completamente attendibile
- Ha gli strumenti di amministrazione remota del server (RSAT, Remote Server Administration Tools) e i moduli PowerShell per Hyper-V e per il clustering di failover. Gli strumenti RSAT e i moduli di PowerShell sono disponibili in Windows Server e possono essere installati senza installare altre funzionalità. È anche possibile installare gli [Strumenti di amministrazione remota del Server](https://www.microsoft.com/download/details.aspx?id=45520) in un PC di gestione di Windows 10.

Nel sistema di gestione installare il cluster di failover e gli strumenti di gestione di Hyper-V. Questa operazione può essere eseguita tramite Server Manager usando l'**Aggiunta guidata ruoli e funzionalità**. Nella pagina **Funzionalità** selezionare **Strumenti di amministrazione remota del Server**, quindi selezionare gli strumenti da installare.

Immettere la sessione PS e usare il nome del server o l'indirizzo IP del nodo a cui connettersi. Sarai richiesta una password dopo aver eseguito questo comando. immettere la password amministratore specificata durante la configurazione di Windows.

   ```PowerShell
   Enter-PSSession -ComputerName <myComputerName> -Credential LocalHost\Administrator
   ```

   Ecco un esempio di eseguire la stessa operazione tramite in modo che sia più utile per gli script, nel caso in cui è necessario eseguire questa operazione più volte:

   ```PowerShell
   $myServer1 = "myServer-1"
   $user = "$myServer1\Administrator"

   Enter-PSSession -ComputerName $myServer1 -Credential $user
   ```

> [!TIP]
> Se stai distribuendo in modalità remota da un sistema di gestione, potresti ricevere un errore come *WinRM non è in grado di elaborare la richiesta.* Per risolvere questo problema, utilizzare Windows PowerShell per aggiungere ogni server all'elenco di host attendibili nel computer di gestione:  
>   
> `Set-Item WSMAN:\Localhost\Client\TrustedHosts -Value Server01 -Force`
>  
> Nota: l'elenco di host attendibili supporta i caratteri jolly, ad esempio `Server*`.
>
> Per visualizzare l'elenco di host attendibili, digita `Get-Item WSMAN:\Localhost\Client\TrustedHosts`.  
>   
> Per svuotare l'elenco, digita `Clear-Item WSMAN:\Localhost\Client\TrustedHost`.  

### Passaggio 1.3: Aggiunta al dominio e aggiungere gli account di dominio

Finora hai configurato il server individuali con l'account amministratore locale, `<ComputerName>\Administrator`.

Per gestire spazi di archiviazione diretta, dovrai aggiungere i server a un dominio e usare un account di dominio Active Directory Domain Services che sia nel gruppo di amministratori in ogni server.

Dal sistema di gestione aprire una console di PowerShell con privilegi di amministratore. Usa `Enter-PSSession` per connettersi a ogni server ed eseguire il cmdlet seguente, sostituendo il tuo nome del computer, nome di dominio e le credenziali di dominio:

```PowerShell  
Add-Computer -NewName "Server01" -DomainName "contoso.com" -Credential "CONTOSO\User" -Restart -Force  
```

Se l'account amministratore di archiviazione non è un membro del gruppo Domain Admins, aggiungere l'account amministratore di archiviazione al gruppo Administrators locale su ciascun nodo - o meglio ancora, Aggiungi il gruppo che usare per gli amministratori di archiviazione. Puoi usare il comando seguente (o scrivere una funzione di Windows PowerShell per eseguire questa operazione, vedere [Utilizzare PowerShell per aggiungere gli utenti del dominio a un gruppo locale](http://blogs.technet.com/b/heyscriptingguy/archive/2010/08/19/use-powershell-to-add-domain-users-to-a-local-group.aspx) per altre info):

```
Net localgroup Administrators <Domain\Account> /add
```

### Passaggio 1.4: Installare ruoli e funzionalità

Il passaggio successivo consiste nell'installare ruoli server in ogni server. È possibile eseguire questa operazione con [Windows Admin Center](../../manage/windows-admin-center/use/manage-servers.md), [Server Manager](../../administration/server-manager/install-or-uninstall-roles-role-services-or-features.md)), o PowerShell. Ecco i ruoli per l'installazione:

- Clustering di failover
- Hyper-V
- File Server (se si desidera per ospitare le condivisioni di file, ad esempio le previsioni una distribuzione convergente)
- Data-Center-Bridging (se si utilizza RoCEv2 anziché schede di rete iWARP)
- RSAT-Clustering-PowerShell
- Hyper-V-PowerShell

Per eseguire l'installazione tramite PowerShell, Usa il cmdlet [Install-WindowsFeature](https://docs.microsoft.com/powershell/module/microsoft.windows.servermanager.migration/install-windowsfeature) . È possibile utilizzarlo in un server singolo come segue:

```PowerShell
Install-WindowsFeature -Name "Hyper-V", "Failover-Clustering", "Data-Center-Bridging", "RSAT-Clustering-PowerShell", "Hyper-V-PowerShell", "FS-FileServer"
```

Per eseguire il comando su tutti i server del cluster come contemporaneamente, usare questo minimo di script, la modifica dell'elenco di variabili all'inizio dello script in base alle proprio ambiente.

```PowerShell
# Fill in these variables with your values
$ServerList = "Server01", "Server02", "Server03", "Server04"
$FeatureList = "Hyper-V", "Failover-Clustering", "Data-Center-Bridging", "RSAT-Clustering-PowerShell", "Hyper-V-PowerShell", "FS-FileServer"

# This part runs the Install-WindowsFeature cmdlet on all servers in $ServerList, passing the list of features into the scriptblock with the "Using" scope modifier so you don't have to hard-code them here.
Invoke-Command ($ServerList) {
    Install-WindowsFeature -Name $Using:Featurelist
}
```

## Passaggio 2: Configurare la rete

Se stai distribuendo spazi di archiviazione diretta all'interno di macchine virtuali, ignora questa sezione.

Spazi di archiviazione diretta richiede larghezza di banda elevata, bassa latenza di rete tra i server del cluster. Funzionalità di rete di almeno 10 GbE è obbligatorio ed è consigliata l'accesso diretto a memoria remota (RDMA). È possibile utilizzare iWARP o RoCE, purché abbiano il logo di Windows Server 2016, ma è in genere più semplice configurare iWARP.

> [!Important]
> A seconda della tua apparecchiature di rete e in particolare con RoCE v2, possono essere necessarie alcune configurazioni del commutatore top-of-rack. La configurazione corretta commutatore è importante per garantire affidabilità e prestazioni di spazi di archiviazione diretta.

Windows Server 2016 introduce switch embedded teaming (SET) all'interno del commutatore virtuale Hyper-V. In questo modo le stesse porte NIC fisiche essere utilizzato per tutto il traffico di rete durante l'uso di RDMA, riducendo il numero di porte NIC fisiche richiesto. Switch embedded teaming è consigliato per spazi di archiviazione diretta.

Per istruzioni su come configurare la rete per spazi di archiviazione diretta, vedere [Windows Server 2016 con convergenza NIC e Guida alla distribuzione RDMA Guest](https://github.com/Microsoft/SDN/blob/master/Diagnostics/S2D%20WS2016_ConvergedNIC_Configuration.docx).

## Passaggio 3: Configurare Spazi di archiviazione diretta

I passaggi seguenti vengono eseguiti in un sistema di gestione con la stessa versione dei server da configurare. La procedura seguente non deve essere eseguita in modalità remota tramite una sessione di PowerShell ma eseguita in una sessione di PowerShell locale nel sistema di gestione con autorizzazioni amministrative.

### Passaggio 3.1: Unità pulita

Prima di abilitare spazi di archiviazione diretta, verificare le unità siano vuote: nessun vecchi partizioni o altri dati. Esegui lo script seguente, sostituendo i nomi di computer, per rimuovere tutte le partizioni precedente o altri dati.

> [!Warning]
> Questo script in modo permanente rimuoverà tutti i dati su qualsiasi unità diverse dall'unità di avvio del sistema operativo!

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

L'output sarà simile, in cui **conteggio** è il numero di unità di ogni modello in ogni server:

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

### Passaggio 3.2: Convalida del cluster

In questo passaggio, dovrai eseguire lo strumento di convalida dei cluster per garantire che i nodi del server siano configurati correttamente per creare un cluster tramite spazi di archiviazione diretta. Quando la convalida del cluster (`Test-Cluster`) viene eseguita prima della creazione del cluster, viene eseguito il test per verificare che la configurazione funzionerà correttamente come cluster di failover. L'esempio seguente viene usato il `-Include` parametro, quindi le categorie dei test vengono specificati. Ciò assicura che i test specifici di Spazi di archiviazione diretta siano inclusi nella convalida.

Usare il comando seguente di PowerShell per convalidare un set di server per l'uso come cluster di Spazi di archiviazione diretta.

```PowerShell
Test-Cluster –Node <MachineName1, MachineName2, MachineName3, MachineName4> –Include "Storage Spaces Direct", "Inventory", "Network", "System Configuration"
```

### Passaggio 3.3: Creare il cluster

In questo passaggio, creerai un cluster con i nodi convalidati per la creazione di cluster nel passaggio precedente tramite il cmdlet PowerShell seguente.

Quando si crea il cluster, riceverai un avviso indicante: "si sono verificati problemi durante la creazione del ruolo del cluster che potrebbero impedire l'avvio. Per ulteriori informazioni, visualizzare il file del rapporto seguente". È possibile ignorare questo avviso. È causato dalla non disponibilità di dischi per il quorum del cluster. Si consiglia di configurare un controllo di condivisione file o un controllo cloud dopo la creazione del cluster.

> [!Note]
> Se i server usano indirizzi IP statici, modificare il comando seguente in modo da riflettere l'indirizzo IP statico aggiungendo il parametro seguente e specificando l'indirizzo IP: -StaticAddress &lt;X.X.X.X&gt;.
> Nel comando seguente il segnaposto ClusterName deve essere sostituito con un nome NetBIOS univoco che includa massimo 15 caratteri.
> ```PowerShell
> New-Cluster –Name <ClusterName> –Node <MachineName1,MachineName2,MachineName3,MachineName4> –NoStorage
> ```

Dopo aver creato il cluster, la replica della voce DNS per il nome del cluster può richiedere tempo. Il tempo richiesto dipende dall'ambiente e dalla configurazione della replica DNS. Se la risoluzione del cluster non ha esito positivo, nella maggior parte dei casi è possibile usare il nome del computer di un nodo che è membro attivo del cluster al posto del nome del cluster.

### Passaggio 3.4: Configurare un controllo di cluster

Ti consigliamo di configurare un controllo di cluster, in modo cluster con tre o più server in grado di resistere due server mancata o sono offline. Una distribuzione con due server richiede un controllo di cluster, in caso contrario, server di disconnessione fa sì che l'altro risulterà non disponibile. Con questi sistemi, come controllo è possibile usare una condivisione file o un cloud di controllo. 

Per altre info, vedi i seguenti argomenti:

- [Configurare e gestire il quorum](../../failover-clustering/manage-cluster-quorum.md)
- [Distribuire un cloud di controllo per un cluster di failover](../../failover-clustering/deploy-cloud-witness.md)

### Passaggio 3.5: Abilitare Spazi di archiviazione diretta

Dopo aver creato il cluster, usare il `Enable-ClusterStorageSpacesDirect` cmdlet di PowerShell che Inserisci il sistema di archiviazione in modalità spazi di archiviazione diretta ed eseguire automaticamente le operazioni seguenti:

-   **Crea un pool:** crea un singolo pool di grandi dimensioni con un nome simile a "S2D on Cluster1".

-   **Configura la cache di Spazi di archiviazione diretta:** se sono disponibili più tipi di supporto (unità) per l'uso di Spazi di archiviazione diretta, abilita il dispositivo di cache più veloce (nella maggior parte dei casi, in lettura e in scrittura).

-   **Livelli:** Crea due livelli come livelli predefiniti. Uno è denominato "Capacità" e l'altro "Prestazioni". Il cmdlet analizza i dispositivi e configura ogni livello con una combinazione di tipi di dispositivi e resilienza.

Eseguire il comando seguente dal sistema di gestione in una finestra di comandi di PowerShell aperta con privilegi di amministratore. Il nome del cluster corrisponde al cluster creato nei passaggi precedenti. Se questo comando viene eseguito localmente in uno dei nodi, il parametro -CimSession non è necessario.

```PowerShell
Enable-ClusterStorageSpacesDirect –CimSession <ClusterName>
```

Per abilitare Spazi di archiviazione diretta tramite il comando precedente, è anche possibile usare il nome del nodo anziché il nome del cluster. L'uso del nome del nodo potrebbe essere più affidabile a causa di ritardi di replica DNS che possono verificarsi con il nome del cluster appena creato.

Al termine dell'esecuzione di questo comando, che può richiedere alcuni minuti, il sistema è pronto per la creazione dei volumi.

### Passaggio 3.6: Creare volumi

Ti consigliamo di usare il `New-Volume` cmdlet come si fornisce l'esperienza più semplice e rapida. Questo cmdlet singolo crea automaticamente il disco virtuale, lo divide in partizioni e lo formatta, crea il volume con il nome corrispondente e lo aggiunge ai volumi condivisi del cluster, tutto in un unico, semplice passaggio.

Per ulteriori informazioni, consulta [Creazione di volumi in Spazi di archiviazione diretta](create-volumes.md).

### Passaggio 3.7: Eventualmente abilitare la cache CSV

Puoi eventualmente abilitare la cache del volume (CSV) condivisi del cluster da utilizzare la memoria di sistema (RAM) come una cache write-through a livello di blocco delle operazioni di lettura che non sono già memorizzato nella cache per la gestione della cache di Windows. Ciò può migliorare le prestazioni delle applicazioni, ad esempio Hyper-V. La cache CSV può migliorare le prestazioni delle richieste di lettura ed è utile anche per gli scenari di File Server di scalabilità orizzontale.

L'abilitazione della cache CSV riduce la quantità di memoria disponibile per l'esecuzione di macchine virtuali in un cluster iperconvergente, pertanto lo devi di bilanciare le prestazioni di archiviazione con memoria disponibile per i dischi rigidi virtuali.

Per impostare le dimensioni della cache CSV, aprire una sessione di PowerShell nel sistema di gestione con un account con autorizzazioni di amministratore nel cluster di archiviazione e quindi utilizzare questo script, la modifica di `$ClusterName` e `$CSVCacheSize` le variabili appropriato (in questo esempio imposta un 2 GB CSV cache per ogni server):

```PowerShell
$ClusterName = "StorageSpacesDirect1"
$CSVCacheSize = 2048 #Size in MB

Write-Output "Setting the CSV cache..."
(Get-Cluster $ClusterName).BlockCacheSize = $CSVCacheSize

$CSVCurrentCacheSize = (Get-Cluster $ClusterName).BlockCacheSize
Write-Output "$ClusterName CSV cache size: $CSVCurrentCacheSize MB"
```

Per altre info, vedi [uso CSV in memoria cache di lettura](csv-cache.md).

### Passaggio 3.8: Distribuire macchine virtuali per le distribuzioni iperconvergenti

Se stai distribuendo un cluster iperconvergente, effettuare il provisioning di macchine virtuali nel cluster di spazi di archiviazione diretta è l'ultimo passaggio.

I file della macchina virtuale devono essere archiviati nello spazio dei nomi CSV dei sistemi (esempio: c:\\ClusterStorage\\Volume1) allo stesso modo delle macchine virtuali in cluster di failover.

È possibile utilizzare strumenti integrati o altri strumenti per gestire l'archiviazione e le macchine virtuali, ad esempio System Center Virtual Machine Manager.

## Passaggio 4: Distribuire i File Server di scalabilità orizzontale per le soluzioni per la convergenza

Se stai distribuendo una soluzione convergente, il passaggio successivo consiste per creare un'istanza di File Server di scalabilità orizzontale e alcuni condivisioni di file di installazione. Se stai distribuendo un cluster iperconvergente, finito e non è necessario questa sezione.

### Passaggio 4.1: Creare il ruolo File Server di scalabilità orizzontale

Configurare i servizi di cluster per il file server il passaggio successivo consiste nel creare il ruolo server di cluster di file, ovvero quando si crea l'istanza di File Server di scalabilità orizzontale in cui sono ospitate le condivisioni di file continuamente disponibili.

#### Per creare un ruolo File Server di scalabilità orizzontale tramite Server Manager

1.  In Gestione Cluster di Failover, seleziona il cluster, Vai a **ruoli**e quindi fai clic su **Configura ruolo**.<br>Viene visualizzata la configurazione guidata disponibilità elevata.
2.  Nella pagina **Seleziona ruolo** , fai clic sul **File Server**.
3.  Nella pagina **Tipo di File Server** , fai clic sul **File Server di scalabilità orizzontale per i dati dell'applicazione**.
4.  Nella pagina **Punto di accesso Client** , digita un nome per il File Server di scalabilità orizzontale.
5.  Verifica che il ruolo è stato configurato correttamente accedendo a **ruoli** e conferma che la colonna **stato** indica **che eseguono** accanto il ruolo del server in cluster di file è stato creato, come illustrato nella figura 1.

    ![Screenshot del Cluster di failover che mostra il File Server di scalabilità orizzontale](media\Hyper-converged-solution-using-Storage-Spaces-Direct-in-Windows-Server-2016\SOFS_in_FCM.png "Failover Cluster Manager showing the Scale-Out File Server")

     **Figura 1** Gestione Cluster di failover che mostra il File Server di scalabilità orizzontale con lo stato di esecuzione

> [!NOTE]
>  Dopo aver creato il ruolo del cluster, potrebbe essere presenti alcuni rete ritardi che potrebbero impedire la creazione di condivisioni di file su di esso per alcuni minuti, o potenzialmente più lungo.  
  
#### Per creare un ruolo File Server di scalabilità orizzontale tramite Windows PowerShell

 In una sessione di Windows PowerShell che è connesso al cluster del file server, Immetti i comandi seguenti per creare il ruolo File Server di scalabilità orizzontale, la modifica *FSCLUSTER* necessariamente corrispondere al nome del cluster e *SOFS* in modo che corrisponda al nome che si desidera assegnare il Ruolo File Server di scalabilità orizzontale:

```PowerShell
Add-ClusterScaleOutFileServerRole -Name SOFS -Cluster FSCLUSTER
```

> [!NOTE]
>  Dopo aver creato il ruolo del cluster, potrebbe essere presenti alcuni rete ritardi che potrebbero impedire la creazione di condivisioni di file su di esso per alcuni minuti, o potenzialmente più lungo. Se il ruolo SOFS ha esito negativo immediatamente e non si avvia, potrebbe essere perché l'oggetto computer del cluster non dispone dell'autorizzazione per creare un account computer per il ruolo SOFS. Per contribuire con tale, vedere questo post di blog: [Scale-Out File Server ruolo ha esito negativo a Start con evento ID 1205, 1069 e 1194](http://www.aidanfinn.com/?p=14142).

### Passaggio 4.2: Creare condivisioni di file

Dopo aver creato i dischi virtuali e aggiunto ai volumi condivisi cluster, è il momento di creare condivisioni di file su di essi - una condivisione per ogni CSV per ogni disco virtuale. System Center Virtual Machine Manager (VMM) è probabilmente il modo per eseguire questa operazione perché gestisce le autorizzazioni per te, ma se non hai si nell'ambiente in uso, è possibile utilizzare Windows PowerShell per automatizzare la distribuzione parzialmente handiest.

Usare gli script inclusi nello script di [Configurazione della condivisione SMB per carichi di lavoro Hyper-V](http://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a) , che parzialmente consente di automatizzare il processo di creazione di gruppi e le condivisioni. Viene scritta per carichi di lavoro Hyper-V, in modo che se stai distribuendo altri carichi di lavoro, è necessario modificare le impostazioni o eseguire ulteriori passaggi dopo aver creato le condivisioni. Ad esempio, se usi Microsoft SQL Server, l'account del servizio SQL Server deve essere concessi controllo completo sul file system e la condivisione.

> [!NOTE]
>  È necessario aggiornare l'appartenenza al gruppo quando si aggiungono i nodi del cluster, a meno che non utilizzare System Center Virtual Machine Manager per creare le condivisioni.

Per creare condivisioni di file tramite script di PowerShell, eseguire le operazioni seguenti:

1. Scarica gli script inclusi nella [Configurazione della condivisione SMB per carichi di lavoro Hyper-V](http://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a) a uno dei nodi del cluster del file server.
2. Aprire una sessione di Windows PowerShell con le credenziali di amministratore di dominio nel sistema di gestione e quindi usare lo script seguente per creare un gruppo Active Directory per gli oggetti computer Hyper-V, la modifica dei valori per le variabili appropriato per il tuo ambiente:

    ```PowerShell
    # Replace the values of these variables
    $HyperVClusterName = "Compute01"
    $HyperVObjectADGroupSamName = "Hyper-VServerComputerAccounts" <#No spaces#>
    $ScriptFolder = "C:\Scripts\SetupSMBSharesWithHyperV"

    # Start of script itself
    CD $ScriptFolder
    .\ADGroupSetup.ps1 -HyperVObjectADGroupSamName $HyperVObjectADGroupSamName -HyperVClusterName $HyperVClusterName
    ```
3. Aprire una sessione di Windows PowerShell con le credenziali di amministratore su uno dei nodi di archiviazione e quindi usare lo script seguente per creare condivisioni per ogni CSV e concedere le autorizzazioni amministrative per le condivisioni al gruppo Domain Admins e il cluster di calcolo.

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

### Delega vincolata Kerberos abilitare passaggio 4.3

Per configurare la delega vincolata Kerberos per la gestione remota scenario e aumentare la protezione di Live Migration, da uno dei nodi del cluster di archiviazione, usare lo script KCDSetup.ps1 incluso nella [Configurazione di condivisione SMB per carichi di lavoro Hyper-V](http://gallery.technet.microsoft.com/SMB-Share-Configuration-4a36272a). Ecco un wrapper poco per lo script:

```PowerShell
$HyperVClusterName = "Compute01"
$ScaleOutFSName = "SOFS"
$ScriptFolder = "C:\Scripts\SetupSMBSharesWithHyperV"

CD $ScriptFolder
.\KCDSetup.ps1 -HyperVClusterName $HyperVClusterName -ScaleOutFSName $ScaleOutFSName -EnableLM
```

## Passaggi successivi

Dopo aver distribuito il cluster file server, ti consigliamo di testare le prestazioni della tua soluzione con carichi di lavoro sintetici prima della visualizzazione di tutti i carichi di lavoro reale. Ciò consente di verificare che la soluzione esegue correttamente e scoprire eventuali problemi residui prima di aggiungere la complessità di carichi di lavoro. Per altre info, vedi [Test archiviazione spazi prestazioni usando sintetico carichi di lavoro](https://technet.microsoft.com/library/dn894707.aspx).

## Vedi anche

-   [Spazi di archiviazione diretta in Windows Server 2016](storage-spaces-direct-overview.md)
-   [Informazioni sulla cache in Spazi di archiviazione diretta](understand-the-cache.md)
-   [Pianificazione dei volumi in Spazi di archiviazione diretta](plan-volumes.md)
-   [Tolleranza di errore di Spazi di archiviazione](storage-spaces-fault-tolerance.md)
-   [Requisiti hardware di Spazi di archiviazione diretta](Storage-Spaces-Direct-Hardware-Requirements.md)
-   [Utilizzare o meno RDMA](https://blogs.technet.microsoft.com/filecab/2017/03/27/to-rdma-or-not-to-rdma-that-is-the-question/) (TechNet blog)
