---
Title: Panoramica della replica DFS
ms.date: 03/08/2019
ms.prod: windows-server-threshold
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: 17fa97e28d099806c9280e42dd900e8d6c708641
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59850242"
---
# <a name="dfs-replication-overview"></a>Panoramica della replica DFS

> Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008, Windows Server (canale semestrale)

Replica DFS è un servizio ruolo in Windows Server che consente di replicare con efficienza le cartelle (incluse quelle a cui fa riferimento un percorso di spazio dei nomi DFS) in più server e siti. Replica DFS è un motore di replica multimaster efficace utilizzabile per mantenere sincronizzate le cartelle tra più server attraverso connessioni di rete con larghezza di banda limitata. Sostituisce il servizio Replica File (FRS) come il motore di replica per gli spazi dei nomi DFS, nonché per la replica della cartella SYSVOL di servizi di dominio Active Directory (AD DS) in domini che utilizzano il Windows Server 2008 o versioni successive livello funzionale del dominio.

Replica DFS utilizza un algoritmo di compressione noto come RDC (Remote Differential Compression). La tecnologia RDC è in grado di rilevare le modifiche apportate ai dati di un file e consente a Replica DFS di replicare solo i blocchi di file modificati anziché l'intero file.

Per altre informazioni sulla replica di SYSVOL con replica DFS, vedere [eseguire la migrazione della replica di SYSVOL per replica DFS](migrate-sysvol-to-dfsr.md).

Per usare la replica DFS, è necessario creare gruppi di replica e aggiungere le cartelle replicate ai gruppi. I gruppi di replica, le cartelle replicate e membri sono illustrati nella figura seguente.

![Un gruppo di replica che contiene una connessione tra due membri, ognuno dei quali con un paio di cartelle replicate](media\dfsr-overview.gif)

La figura seguente mostra che un gruppo di replica è un set di server, noti come membri, che partecipa alla replica di uno o più cartelle replicate. Una cartella replicata è una cartella che resta sincronizzata in ogni membro. Nella figura, sono presenti due cartelle replicate: I progetti e proposte. Se i dati viene modificato in tutte le cartelle replicate, le modifiche vengono replicate nelle connessioni tra i membri del gruppo di replica. Le connessioni tra tutti i membri formano la topologia di replica.
Creazione di più cartelle replicate in un singolo gruppo di replica semplifica il processo di distribuzione di cartelle replicate poiché la topologia, la pianificazione e limitazione per il gruppo di replica larghezza di banda vengono applicate a ogni cartella replicata. Per distribuire altre cartelle replicate, è possibile utilizzare Dfsradmin.exe o un, seguire le istruzioni in una procedura guidata per definire il percorso locale e le autorizzazioni per la nuova cartella replicata.

Ogni cartella replicata ha impostazioni univoche, come i filtri di file e sottocartella, in modo che è possibile filtrare i diversi file e sottocartelle per ogni cartella replicata.

Le cartelle replicate archiviate in ogni membro possono essere posizionate in volumi diversi nel membro e le cartelle replicate non sono necessario essere cartelle condivise o parte di uno spazio dei nomi. Tuttavia, lo snap-in Gestione DFS semplifica condividere le cartelle replicate e facoltativamente li pubblicano in uno spazio dei nomi esistente.

È possibile amministrare la replica DFS mediante Gestione DFS, DfsrAdmin e Dfsrdiag comandi o script che chiamano WMI.

## <a name="requirements"></a>Requisiti

Per poter distribuire Replica DFS, è prima necessario configurare i server nel modo seguente:

- Aggiornare lo schema di Active Directory Domain Services (AD DS) per includere Windows Server 2003 R2 o versioni successive aggiunte di schema. È possibile usare le cartelle replicate di sola lettura con il Windows Server 2003 R2 o aggiunte di schema obsolete.
- Verificare che tutti i server inclusi in un gruppo di replica si trovino nella stessa foresta. Non è possibile abilitare la replica tra server in foreste diverse.
- Installare Replica DFS in tutti i server che funzioneranno come membri di un gruppo di replica.
- Contattare il fornitore del software antivirus per verificare la compatibilità del software antivirus con Replica DFS.
- Individuare eventuali cartelle di cui si desidera eseguire la replica in volumi formattati con il file system NTFS. Replica DFS non supporta Resilient File System (ReFS) o il file system FAT. Replica DFS non supporta inoltre la replica di contenuto archiviato in volumi condivisi del cluster.

## <a name="interoperability-with-azure-virtual-machines"></a>Interoperabilità con macchine virtuali di Azure

Tramite la replica DFS su una macchina virtuale in Azure è stato testato con Windows Server. Tuttavia, esistono alcune limitazioni e requisiti da seguire.

- Se si usano snapshot o stati salvati per ripristinare un server che esegue Replica DFS per la replica di qualsiasi elemento diverso dalla cartella SYSVOL, Replica DFS avrà esito negativo e saranno necessari passaggi speciali di ripristino del database. Allo stesso modo, non esportare, clonare o copiare le macchine virtuali. Per altre informazioni, vedere l'articolo [2517913](http://support.microsoft.com/kb/2517913) della Microsoft Knowledge Base e [Virtualizzazione sicura di DFSR](https://blogs.technet.microsoft.com/filecab/2013/04/05/safely-virtualizing-dfsr/).
- Quando si esegue il backup dei dati in una cartella replicata ospitata in una macchina virtuale, è necessario usare il software di backup della macchina virtuale guest.
- Replica DFS richiede l'accesso ai controller di dominio fisici o virtualizzati, che non possa comunicare direttamente con Azure AD.
- Replica DFS richiede una connessione VPN tra i membri del gruppo di replica locale e i membri ospitati nelle macchine virtuali di Azure. È anche necessario configurare il router locale (ad esempio, Forefront Threat Management Gateway) per consentire all'Agente mapping endpoint RPC (porta 135) e a una porta assegnata casualmente compresa tra 49152 e 65535 di saltare la connessione VPN. È possibile utilizzare il cmdlet Set-DfsrMachineConfiguration o lo strumento da riga di comando Dfsrdiag per specificare una porta statica invece della porta casuale. Per altre informazioni su come specificare una porta statica per Replica DFS, vedere [Set-DfsrServiceConfiguration](https://docs.microsoft.com/powershell/module/dfsr/set-dfsrserviceconfiguration). Per informazioni sulle porte correlate da aprire per gestire Windows Server, vedere l'articolo [832017](http://support.microsoft.com/kb/832017) della Microsoft Knowledge Base.

Per altre informazioni introduttive sulle macchine virtuali di Azure, visitare il [sito Web di Microsoft Azure](https://docs.microsoft.com/azure/virtual-machines/).

## <a name="installing-dfs-replication"></a>Installazione della replica DFS

Replica DFS è una parte del ruolo Servizi File e archiviazione. Strumenti di gestione per DFS (Gestione DFS, il modulo replica DFS per Windows PowerShell e strumenti da riga di comando) vengono installati separatamente come parte di strumenti di amministrazione remota del Server.

Installare Replica DFS mediante [Windows Admin Center](../../manage/windows-admin-center/understand/windows-admin-center.md), Server Manager o PowerShell, come descritto nelle sezioni successive.

### <a name="to-install-dfs-by-using-server-manager"></a>Per installare il file system DFS tramite Server Manager

1. Aprire Server Manager, fare clic su **Gestione** e quindi su **Aggiungi ruoli e funzionalità**. Verrà visualizzata l'Aggiunta guidata ruoli e funzionalità.

2. Nella pagina **Selezione dei server** selezionare il server o il disco rigido virtuale (VHD) di una macchina virtuale offline in cui si desidera installare il file system DFS.

3. Selezionare i servizi ruolo e le funzionalità che si desidera installare.

    - Per installare il servizio Replica DFS, nella **ruoli predefiniti del Server** pagina, selezionare **replica DFS**.

    - Per installare solo gli Strumenti di gestione DFS, nella pagina **Funzionalità** espandere **Strumenti di amministrazione remota del server**, **Strumenti di amministrazione ruoli**, **Strumenti per Servizi file** e quindi selezionare **Strumenti di gestione DFS**.

         **Strumenti di gestione DFS** installazioni di snap-in Gestione DFS, la replica DFS e i moduli di spazi dei nomi DFS per Windows PowerShell e strumenti da riga di comando, ma non installa alcun servizio DFS nel server.

### <a name="to-install-dfs-replication-by-using-windows-powershell"></a>Per installare la replica DFS tramite Windows PowerShell

Aprire una sessione di Windows PowerShell con diritti utente elevati e quindi digitare il comando seguente, dove < nome\> è il servizio ruolo o funzionalità che si desidera installare (vedere la tabella seguente per un elenco di nomi ruolo del servizio o funzionalità rilevanti):

```PowerShell
Install-WindowsFeature <name>
```

|Servizio ruolo o funzionalità|Nome|
|---|---|
|Replica DFS|`FS-DFS-Replication`|
|Strumenti di gestione DFS|`RSAT-DFS-Mgmt-Con`|

Per installare, ad esempio, il componente Strumenti per File system distribuito (DFS) della funzionalità Strumenti di amministrazione remota del server, digitare:

```PowerShell
Install-WindowsFeature "RSAT-DFS-Mgmt-Con"
```

Per installare la replica DFS e le parti di strumenti per Distributed File System della funzionalità Strumenti di amministrazione remota del Server, digitare:

```PowerShell
Install-WindowsFeature "FS-DFS-Replication", "RSAT-DFS-Mgmt-Con"
```

## <a name="see-also"></a>Vedere anche

- [Panoramica di spazi dei nomi DFS e replica DFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj127250(v%3dws.11))
- [Elenco di controllo: Distribuire la replica DFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc772201(v%3dws.11))
- [Elenco di controllo: Gestire la replica DFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc755035(v%3dws.11))
- [Distribuzione di replica DFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770925(v%3dws.11))
- [Gestione della replica DFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770925(v%3dws.11))
- [Risoluzione dei problemi di replica DFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732802(v%3dws.11))
