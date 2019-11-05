---
title: Distribuire un'infrastruttura iperconvergente con l'interfaccia di amministrazione di Windows
ms.topic: article
author: cosmosdarwin
ms.author: cosdar
ms.prod: windows-server
ms.technology: manage
ms.date: 11/04/2019
ms.openlocfilehash: 62bf21dd0afcb99aa77cff8a733e80fc4cffe2fb
ms.sourcegitcommit: 1da993bbb7d578a542e224dde07f93adfcd2f489
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/04/2019
ms.locfileid: "73587231"
---
# <a name="deploy-hyperconverged-infrastructure-with-windows-admin-center"></a>Distribuire un'infrastruttura iperconvergente con l'interfaccia di amministrazione di Windows

> Si applica a: Windows Admin Center, Windows Admin Center Preview

È possibile usare l'interfaccia di amministrazione di Windows [versione 1910](https://docs.microsoft.com/windows-server/manage/windows-admin-center/understand/windows-admin-center) o successiva per distribuire l'infrastruttura iperconvergente usando due o più server Windows appropriati. Questa nuova funzionalità ha il formato di un flusso di lavoro in più fasi che guida l'utente durante l'installazione delle funzionalità, la configurazione della rete, la creazione del cluster e la distribuzione di Spazi di archiviazione diretta e/o SDN (Software Defined Networking) se selezionato.

![Screenshot del flusso di lavoro di creazione del cluster](../media/deploy-hyperconverged-infrastructure/deployment-workflow-screenshot.png)

  > [!Important]
  > Questa funzionalità è in fase di sviluppo attivo. È disponibile in anteprima, quindi è possibile provarlo tempestivamente e condividere i propri commenti.

## <a name="preview-the-workflow"></a>Visualizzare in anteprima il flusso di lavoro

### <a name="1-prerequisites"></a>1. prerequisiti

Il flusso di lavoro di creazione del cluster nell'interfaccia di amministrazione di Windows non esegue l'installazione del sistema operativo bare metal, quindi è necessario installare prima Windows Server in ogni server. Le versioni supportate sono Windows Server 2016, Windows Server 2019 e Windows Server Insider Preview. È anche necessario aggiungere ogni server allo stesso Active Directory dominio in cui è in esecuzione il centro di amministrazione di Windows prima di avviare il flusso di lavoro.

### <a name="2-install-windows-admin-center"></a>2. installare l'interfaccia di amministrazione di Windows
 
Seguire le istruzioni per [scaricare e installare](https://docs.microsoft.com/windows-server/manage/windows-admin-center/understand/windows-admin-center) la versione più recente dell'interfaccia di amministrazione di Windows.

### <a name="3-install-the-cluster-creation-extension"></a>3. installare l'estensione per la creazione del cluster

Il flusso di lavoro di creazione del cluster viene fornito e aggiornato tramite il feed dell'estensione dell'interfaccia di amministrazione di Windows. In questo modo è possibile risolvere i commenti e apportare miglioramenti più rapidamente durante la fase di anteprima.

Per installare l'estensione, avviare l'interfaccia di amministrazione di Windows e quindi:

1.  Nell'angolo in alto a destra selezionare l'icona delle impostazioni.
2.  Passa a estensioni
3.  Trovare l'estensione per la creazione del cluster
4.  Selezionare Installa

![Screenshot dei quattro passaggi per l'installazione dell'estensione per la creazione del cluster](../media/deploy-hyperconverged-infrastructure/how-to-install.png)

Una volta installato, selezionare l'estensione per la creazione del cluster nell'elenco a discesa in alto a sinistra:

![Screenshot dell'avvio dell'estensione per la creazione del cluster](../media/deploy-hyperconverged-infrastructure/how-to-launch.png)

## <a name="limitations-of-the-preview-release"></a>Limitazioni della versione di anteprima

Il flusso di lavoro di creazione del cluster è in fase di sviluppo attivo, ovvero non è terminato.

Di seguito sono riportate alcune limitazioni della versione di anteprima corrente:

1.  **Requisiti di dominio.** Il flusso di lavoro richiede attualmente che tutti i server aggiunti debbano già essere aggiunti allo stesso dominio Active Directory in cui è in esecuzione il centro di amministrazione di Windows.
2.  **Mostra script.** Con la versione di anteprima corrente, non è possibile visualizzare gli script eseguiti dal flusso di lavoro usando la funzionalità Mostra script nell'interfaccia di amministrazione di Windows, né scaricare un report completo su tutto ciò che si è verificato alla fine. Questo è importante per molti utenti. Nel frattempo, usare i cmdlet riportati di seguito per [vedere cosa accade](#see-whats-happening).
3.  **Riprendere o ricominciare.** Sebbene sia possibile uscire da legato tramite il flusso di lavoro, la versione di anteprima corrente non gestisce la ripresa del punto in cui è stata interrotta, né può essere completamente pulita prima di ricominciare. Se si desidera distribuire ripetutamente negli stessi server per il test, utilizzare i cmdlet indicati di seguito per [annullare e ricominciare](#undo-and-start-over).
4.  **Accesso diretto a memoria remota (RDMA).** La versione di anteprima corrente non può abilitare la rete RDMA, comunemente usata con l'infrastruttura iperconvergente. Per il momento, completare il flusso di lavoro senza abilitare RDMA e quindi usare altri strumenti per abilitare RDMA come si farebbe altrimenti.

Oltre ad affrontare queste limitazioni, esistono innumerevoli funzionalità potenziali che potrebbero essere aggiunte nelle versioni future: applicazione degli aggiornamenti di Windows, configurazione di un server di controllo del mirroring per il quorum del cluster, importazione di macchine virtuali e così via. Per aiutarci a classificare in ordine di priorità le funzionalità desiderate, [Condividi commenti e suggerimenti](#feedback) come descritto di seguito.

## <a name="see-whats-happening"></a>Scopri cosa succede

Usare questi cmdlet di Windows PowerShell per proseguire e vedere le attività del flusso di lavoro.

Per visualizzare le funzionalità di Windows installate, usare il cmdlet `Get-WindowsFeature`. Ad esempio:

```PowerShell
Get-WindowsFeature "Hyper-V", "Failover-Clustering", "Data-Center-Bridging", "BitLocker"
```

![Screenshot dell'output di PowerShell](../media/deploy-hyperconverged-infrastructure/script-out-1.png)

  > [!Note]
  > Le funzionalità installate variano in base al tipo di cluster selezionato.

Per visualizzare le schede di rete e le relative proprietà, ad esempio nome, indirizzi IPv4 e ID VLAN:

```PowerShell
Get-NetAdapter | Where Status -Eq "Up" | Sort InterfaceAlias | Format-Table Name, InterfaceDescription, Status, LinkSpeed, VLANID, MacAddress
Get-NetAdapter | Where Status -Eq "Up" | Get-NetIPAddress -AddressFamily IPv4 -ErrorAction SilentlyContinue | Sort InterfaceAlias | Format-Table InterfaceAlias, IPAddress, PrefixLength
```

![Screenshot dell'output di PowerShell](../media/deploy-hyperconverged-infrastructure/script-out-2.png)

Per visualizzare i commutatori virtuali Hyper-V e il modo in cui vengono raggruppate le schede di rete fisiche:

```PowerShell
Get-VMSwitch
Get-VMSwitchTeam
```

![Screenshot dell'output di PowerShell](../media/deploy-hyperconverged-infrastructure/script-out-3.png)

Per visualizzare le schede di rete virtuali dell'host, se presenti, eseguire:

```PowerShell
Get-VMNetworkAdapter -ManagementOS | Format-Table Name, IsManagementOS, SwitchName
Get-VMNetworkAdapterTeamMapping -ManagementOS | Format-Table NetAdapterName, ParentAdapter
```

![Screenshot dell'output di PowerShell](../media/deploy-hyperconverged-infrastructure/script-out-4.png)

Per visualizzare il cluster di failover corrente e i relativi nodi membro, eseguire:

```PowerShell
Get-Cluster
Get-ClusterNode
```

![Screenshot dell'output di PowerShell](../media/deploy-hyperconverged-infrastructure/script-out-5.png)

Per verificare se Spazi di archiviazione diretta è abilitato e il pool di archiviazione predefinito creato automaticamente:

```PowerShell
Get-ClusterStorageSpacesDirect
Get-StoragePool -IsPrimordial $False
```

![Screenshot dell'output di PowerShell](../media/deploy-hyperconverged-infrastructure/script-out-6.png)

## <a name="undo-and-start-over"></a>Annulla e ricomincia

Usare questi cmdlet di Windows PowerShell per annullare le modifiche apportate dal flusso di lavoro e ricominciare.

### <a name="remove-virtual-machines-or-other-clustered-resources"></a>Rimuovere le macchine virtuali o altre risorse cluster

Se sono state create macchine virtuali o altre risorse cluster, ad esempio i controller di rete per la rete definita dal software, rimuoverle prima di tutto.

Per rimuovere le risorse in base al nome, ad esempio, usare:

```PowerShell
Get-ClusterResource -Name "<NAME>" | Remove-ClusterResource
```

### <a name="undo-the-storage-steps"></a>Annulla i passaggi di archiviazione

Se è stata abilitata Spazi di archiviazione diretta, disabilitarla con lo script seguente:

> [!Warning]
> Questi cmdlet eliminano definitivamente i dati nei volumi Spazi di archiviazione diretta. Questa operazione non può essere annullata.

```PowerShell
Get-VirtualDisk | Remove-VirtualDisk
Get-StoragePool -IsPrimordial $False | Remove-StoragePool
Disable-ClusterS2D
```

### <a name="undo-the-clustering-steps"></a>Annulla la procedura di clustering

Se è stato creato un cluster, rimuoverlo con il cmdlet seguente:

```PowerShell
Remove-Cluster -CleanUpAD
```

Per rimuovere anche i report di convalida del cluster, eseguire questo cmdlet in tutti i server che facevano parte del cluster:

```PowerShell
Get-ChildItem C:\Windows\cluster\Reports\ | Remove-Item
```

### <a name="undo-the-networking-steps"></a>Annulla i passaggi di rete

Eseguire questi cmdlet in tutti i server che facevano parte del cluster.

Se è stato creato un Commuter virtuale Hyper-V:

```PowerShell
Get-VMSwitch | Remove-VMSwitch
```

> [!Note]
> Il cmdlet `Remove-VMSwitch` rimuove automaticamente tutte le schede virtuali e Annulla il gruppo incorporato di schede fisiche.

Se sono state modificate le proprietà della scheda di rete, ad esempio nome, indirizzo IPv4 e ID VLAN:

> [!Warning]
> Questi cmdlet rimuovono i nomi e gli indirizzi IP delle schede di rete. Assicurarsi di disporre delle informazioni necessarie per connettersi successivamente, ad esempio un adapter per la gestione esclusa dallo script seguente. Assicurarsi inoltre di avere la certezza che i server siano connessi in termini di proprietà fisiche, ad esempio l'indirizzo MAC, non solo il nome dell'adapter in Windows.

```PowerShell
Get-NetAdapter | Where Name -Ne "Management" | Rename-NetAdapter -NewName $(Get-Random)
Get-NetAdapter | Where Name -Ne "Management" | Get-NetIPAddress -ErrorAction SilentlyContinue | Where AddressFamily -Eq IPv4 | Remove-NetIPAddress
Get-NetAdapter | Where Name -Ne "Management" | Set-NetAdapter -VlanID 0
```

A questo punto si è pronti per avviare il flusso di lavoro.

## <a name="feedback"></a>Feedback

Questa versione di anteprima riguarda tutti i commenti e suggerimenti degli utenti. Ecco alcuni modi in cui è possibile raggiungere il team:

- [Inviare e votare le richieste di funzionalità in UserVoice](https://windowsserver.uservoice.com/forums/295071/category/319162?query=%5Bhci%5D)
- [Partecipa al forum di Windows Admin Center su Microsoft Tech community](https://techcommunity.microsoft.com/t5/Windows-Server-Management/bd-p/WindowsServerManagement)
- Posta elettronica HCI-distribuzione [at] microsoft.com
- Da Tweet a [@servermgmt](https://twitter.com/servermgmt)

## <a name="report-an-issue"></a>Segnala un problema

Usare i canali elencati sopra per segnalare un problema con il flusso di lavoro di creazione del cluster.

Se possibile, includere le informazioni riportate di seguito per contribuire a riprodurre rapidamente e risolvere il problema:

- Tipo di cluster selezionato (esempio: *"iperconvergente"* )
- Passaggio in cui si è verificato il problema (ad esempio: *"3,2 crea cluster"* )
- Versione dell'estensione per la creazione del cluster. Passare a **impostazioni** > **estensioni** > **estensioni installate** e vedere la colonna **versione** (ad esempio: *"1.0.30"* ).
- Messaggi di errore, sia sullo schermo che sulla console del browser, che è possibile aprire premendo **F12**.
- Tutte le altre informazioni rilevanti sull'ambiente 

## <a name="see-also"></a>Vedi anche

- [Hello, interfaccia di amministrazione di Windows](https://docs.microsoft.com/windows-server/manage/windows-admin-center/understand/windows-admin-center)
- [Distribuire Spazi di archiviazione diretta](https://docs.microsoft.com/windows-server/storage/storage-spaces/deploy-storage-spaces-direct)
