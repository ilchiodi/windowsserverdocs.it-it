---
title: Panoramica di Replica DFS
ms.date: 03/08/2019
ms.prod: windows-server
ms.technology: storage
author: JasonGerend
manager: elizapo
ms.author: jgerend
ms.openlocfilehash: c17fd78a2cf726ab156d3eda09b9c0e2d4ed6a75
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "77520357"
---
# <a name="dfs-replication-overview"></a>Panoramica di Replica DFS

> Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008, Windows Server (Canale semestrale)

Replica DFS è un servizio ruolo di Windows Server che consente di replicare in modo efficiente le cartelle (incluse quelle a cui viene fatto riferimento da un percorso di spazio dei nomi DFS) in più server e siti. Replica DFS è un motore di replica multimaster efficace utilizzabile per mantenere sincronizzate le cartelle tra più server attraverso connessioni di rete con larghezza di banda limitata. Sostituisce il servizio Replica file (FRS) come motore di replica per gli spazi dei nomi DFS, nonché per la replica della cartella SYSVOL di Active Directory Domain Services in domini che usano il livello funzionale di dominio di Windows Server 2008 o versioni successive.

Replica DFS utilizza un algoritmo di compressione noto come RDC (Remote Differential Compression). La tecnologia RDC è in grado di rilevare le modifiche apportate ai dati di un file e consente a Replica DFS di replicare solo i blocchi di file modificati anziché l'intero file.

Per altre informazioni sulla replica di SYSVOL con Replica DFS, vedi [Eseguire la migrazione della replica di SYSVOL in Replica DFS](migrate-sysvol-to-dfsr.md).

Per usare Replica DFS, devi creare gruppi di replica e aggiungere le cartelle replicate ai gruppi. I gruppi di replica, le cartelle replicate e i membri sono illustrati nella figura seguente.

![Gruppo di replica contenente una connessione tra due membri, ognuno con un paio di cartelle replicate](media/dfsr-overview.gif)

Questa figura mostra che un gruppo di replica è un set di server, noti come membri, che partecipa alla replica di una o più cartelle replicate. Una cartella replicata è una cartella sempre sincronizzata in tutti i membri. Nella figura sono presenti due cartelle replicate: per i progetti e per le proposte. Man mano che i dati cambiano in ciascuna cartella replicata, le modifiche vengono replicate tramite le connessioni tra i membri del gruppo di replica. Le connessioni tra tutti i membri formano la topologia di replica.
La creazione di più cartelle replicate in un singolo gruppo di replica semplifica il processo di distribuzione delle cartelle replicate, in quanto la topologia, la pianificazione e la limitazione larghezza di banda per il gruppo di replica vengono applicate a ogni cartella replicata. Per distribuire altre cartelle replicate, puoi usare Dfsradmin.exe o seguire le istruzioni di una procedura guidata per definire il percorso locale e le autorizzazioni per la nuova cartella replicata.

Ciascuna cartella replicata dispone di impostazioni univoche, ad esempio filtri di file e sottocartelle, in modo che sia possibile filtrare file e sottocartelle diversi per ogni cartella replicata.

Le cartelle replicate archiviate in ogni membro possono trovarsi in volumi diversi del membro e le cartelle replicate non devono necessariamente essere cartelle condivise o parte di uno spazio dei nomi. Lo snap-in Gestione DFS, tuttavia, semplifica la condivisione delle cartelle replicate e, facoltativamente, la relativa pubblicazione in uno spazio dei nomi esistente.

Puoi amministrare Replica DFS tramite Gestione DFS, i comandi DfsrAdmin e Dfsrdiag o gli script che chiamano WMI.

## <a name="requirements"></a>Requisiti

Per poter distribuire Replica DFS, è prima necessario configurare i server nel modo seguente:

- Aggiornare lo schema di Active Directory Domain Services in modo da includere le aggiunte per lo schema di Windows Server 2003 R2 o versioni successive. Non è possibile usare cartelle replicate di sola lettura con le aggiunte per lo schema di Windows Server 2003 R2 o versioni precedenti.
- Verificare che tutti i server inclusi in un gruppo di replica si trovino nella stessa foresta. Non è possibile abilitare la replica tra server in foreste diverse.
- Installare Replica DFS in tutti i server che funzioneranno come membri di un gruppo di replica.
- Contattare il fornitore del software antivirus per verificare la compatibilità del software antivirus con Replica DFS.
- Individuare eventuali cartelle di cui si desidera eseguire la replica in volumi formattati con il file system NTFS. Replica DFS non supporta Resilient File System (ReFS) o il file system FAT. Replica DFS non supporta inoltre la replica di contenuto archiviato in volumi condivisi del cluster.

## <a name="interoperability-with-azure-virtual-machines"></a>Interoperabilità con macchine virtuali di Azure

L'uso di Replica DFS in una macchina virtuale in Azure è stato testato con Windows Server, ma è necessario rispettare alcuni limiti e requisiti.

- Se si usano snapshot o stati salvati per ripristinare un server che esegue Replica DFS per la replica di qualsiasi elemento diverso dalla cartella SYSVOL, Replica DFS avrà esito negativo e saranno necessari passaggi speciali di ripristino del database. Allo stesso modo, non esportare, clonare o copiare le macchine virtuali. Per altre informazioni, vedere l'articolo [2517913](https://support.microsoft.com/kb/2517913) della Microsoft Knowledge Base e [Virtualizzazione sicura di DFSR](https://blogs.technet.microsoft.com/filecab/2013/04/05/safely-virtualizing-dfsr/).
- Durante il backup dei dati in una cartella replicata ospitata in una macchina virtuale, devi usare il software di backup dall'interno della macchina virtuale guest.
- Replica DFS richiede l'accesso ai controller di dominio fisici o virtualizzati perché non può comunicare direttamente con Azure AD.
- Replica DFS richiede una connessione VPN tra i membri del gruppo di replica locale e i membri ospitati nelle macchine virtuali di Azure. Devi anche configurare il router locale, ad esempio Forefront Threat Management Gateway, per consentire all'Agente mapping endpoint RPC (porta 135) e a una porta assegnata casualmente compresa tra 49152 e 65535 di saltare la connessione VPN. Puoi usare il cmdlet Set-DfsrMachineConfiguration o lo strumento da riga di comando Dfsrdiag per specificare una porta statica invece della porta casuale. Per altre informazioni su come specificare una porta statica per Replica DFS, vedere [Set-DfsrServiceConfiguration](https://docs.microsoft.com/powershell/module/dfsr/set-dfsrserviceconfiguration). Per informazioni sulle porte correlate da aprire per gestire Windows Server, vedere l'articolo [832017](https://support.microsoft.com/kb/832017) della Microsoft Knowledge Base.

Per altre informazioni introduttive sulle macchine virtuali di Azure, visitare il [sito Web di Microsoft Azure](https://docs.microsoft.com/azure/virtual-machines/).

## <a name="installing-dfs-replication"></a>Installazione di Replica DFS

Replica DFS fa parte del ruolo Servizi file e archiviazione. Gli strumenti di gestione per il file system DFS (Gestione DFS, il modulo Replica DFS per Windows PowerShell e gli strumenti da riga di comando) vengono installati separatamente come parte degli Strumenti di amministrazione remota del server.

Installa Replica DFS tramite [Windows Admin Center](../../manage/windows-admin-center/understand/windows-admin-center.md), Server Manager o PowerShell, come descritto nelle sezioni seguenti.

### <a name="to-install-dfs-by-using-server-manager"></a>Per installare il file system DFS tramite Server Manager

1. Aprire Server Manager, fare clic su **Gestione**e quindi su **Aggiungi ruoli e funzionalità**. Verrà visualizzata l'Aggiunta guidata ruoli e funzionalità.

2. Nella pagina **Selezione dei server** selezionare il server o il disco rigido virtuale (VHD) di una macchina virtuale offline in cui si desidera installare il file system DFS.

3. Selezionare i servizi ruolo e le funzionalità che si desidera installare.

    - Per installare il servizio Replica DFS, nella pagina **Ruoli server** seleziona **Replica DFS**.

    - Per installare solo gli Strumenti di gestione DFS, nella pagina **Funzionalità** espandere **Strumenti di amministrazione remota del server**, **Strumenti di amministrazione ruoli**, **Strumenti per Servizi file** e quindi selezionare **Strumenti di gestione DFS**.

         Tramite **Strumenti di gestione DFS** vengono installati lo snap-in Gestione DFS, i moduli Replica DFS e Spazi dei nomi DFS per Windows PowerShell e gli strumenti da riga di comando, ma non viene installato alcun servizio DFS nel server.

### <a name="to-install-dfs-replication-by-using-windows-powershell"></a>Per installare Replica DFS tramite Windows PowerShell

Apri una sessione di Windows PowerShell con diritti utente elevati e quindi digita il comando seguente, in cui <name\> è il servizio ruolo o la funzionalità che vuoi installare (vedi la tabella seguente per un elenco di nomi di servizi ruolo o funzionalità rilevanti):

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

Per installare Replica DFS e i componenti Strumenti per File system distribuito (DFS) della funzionalità Strumenti di amministrazione remota del server, digita:

```PowerShell
Install-WindowsFeature "FS-DFS-Replication", "RSAT-DFS-Mgmt-Con"
```

## <a name="see-also"></a>Vedi anche

- [Panoramica di Spazi dei nomi DFS e Replica DFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj127250(v%3dws.11))
- [Elenco di controllo: Distribuire Replica DFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc772201(v%3dws.11))
- [Elenco di controllo: Gestire Replica DFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc755035(v%3dws.11))
- [Distribuzione di Replica DFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770925(v%3dws.11))
- [Gestione di Replica DFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc770925(v%3dws.11))
- [Risoluzione dei problemi di Replica DFS](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/cc732802(v%3dws.11))
