---
title: Creare un cluster di failover
description: Come creare un cluster di failover per Windows Server 2012 R2, Windows Server 2012, Windows Server 2016 e Windows Server 2019.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.openlocfilehash: b36f707edc08a4a8b2bc55a87c9db168e19a5487
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66810975"
---
# <a name="create-a-failover-cluster"></a>Creare un cluster di failover

>Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012

In questo argomento viene descritto come creare un cluster di failover usando lo snap-in Gestione cluster di failover o Windows PowerShell. Verrà illustrata una distribuzione tipica, in cui vengono creati oggetti computer per il cluster, insieme ai relativi ruoli del cluster, in Servizi di dominio Active Directory. Se si distribuisce un cluster di spazi di archiviazione diretta, vedere invece [distribuire spazi di archiviazione diretta](../storage/storage-spaces/deploy-storage-spaces-direct.md).

È anche possibile distribuire un cluster scollegato da Active Directory. Con questo tipo di distribuzione è possibile creare un cluster di failover senza le autorizzazioni necessarie per creare oggetti computer in Servizi di dominio Active Directory o senza dover richiedere la pre-installazione degli oggetti computer in Servizi di dominio Active Directory. Questa opzione è disponibile solo tramite Windows PowerShell ed è consigliata solo per scenari specifici. Per ulteriori informazioni, vedere [Distribuire un cluster scollegato da Active Directory](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11)).

#### <a name="checklist-create-a-failover-cluster"></a>Elenco di controllo: Creare un cluster di failover

| Stato | Attività | Riferimenti |
| ---    | ---  | ---       |
| ☐    | Verificare i prerequisiti | [Verificare i prerequisiti](#verify-the-prerequisites) |
| ☐    | Installare la funzionalità Clustering di failover in tutti i server da aggiungere come nodi del cluster | [Installare la funzionalità Clustering di Failover](#install-the-failover-clustering-feature) |
| ☐    | Eseguire la convalida guidata del cluster per la verifica della configurazione | [Convalidare la configurazione](#validate-the-configuration) |
| ☐ | Eseguire la Creazione guidata cluster per creare il cluster di failover | [Creare il cluster di failover](#create-the-failover-cluster) |
| ☐ | Creare ruoli del cluster per ospitare i carichi di lavoro del cluster | [Creare ruoli del cluster](#create-clustered-roles) |

## <a name="verify-the-prerequisites"></a>Verificare i prerequisiti

Prima di iniziare, verificare i prerequisiti seguenti:

- Assicurarsi che tutti i server da aggiungere come nodi del cluster eseguano la stessa versione di Windows Server.
- Esaminare i requisiti hardware per assicurarsi che la configurazione in uso sia supportata. Per altre informazioni, vedere [Requisiti hardware per il clustering di failover e opzioni di archiviazione](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj612869(v%3dws.11)). Se si crea un cluster di spazi di archiviazione diretta, vedere [requisiti hardware di spazi di archiviazione diretta](../storage/storage-spaces/storage-spaces-direct-hardware-requirements.md).
- Per aggiungere archiviazione in cluster durante la creazione del cluster, assicurarsi che tutti i server possano accedere all'archiviazione. L'archiviazione in cluster può essere aggiunta anche dopo il completamento della creazione del cluster.
- Assicurarsi che tutti i server da aggiungere come nodi del cluster siano aggiunti allo stesso dominio Active Directory.
- (Facoltativo). Creare un'unità organizzativa e spostarvi all'interno gli account computer per i server da aggiungere come nodi del cluster. La procedura consigliata prevede il posizionamento dei cluster di failover in una propria unità organizzativa all'interno di Servizi di dominio Active Directory. Questo può aiutare a controllare meglio quali impostazioni di Criteri di gruppo o di modelli di sicurezza hanno effetto sui nodi del cluster. L'isolamento del cluster in una propria unità organizzativa aiuta inoltre a prevenire l'eliminazione accidentale di oggetti computer del cluster.

Verificare anche i requisiti di account seguenti:

- Assicurarsi che l'account da usare per la creazione del cluster sia un utente di dominio che dispone di diritti di amministratore su tutti i server da aggiungere come nodi del cluster.
- Assicurarsi che una delle condizioni seguenti sia vera:
    - l'utente che crea il cluster dispone dell'autorizzazione **Crea oggetti computer** per l'unità organizzativa o il contenitore in cui risiedono i server che costituiranno il cluster.
    - Se l'utente non dispone dell'autorizzazione **Crea oggetti computer**, rivolgersi a un amministratore di dominio richiedendo la pre-installazione di un oggetto computer del cluster per il nuovo cluster. Per altre informazioni, vedere [Pre-installare oggetti computer del cluster in Servizi di dominio Active Directory](prestage-cluster-adds.md).

> [!NOTE]
> Questo requisito non si applica se si desidera creare un cluster scollegato da Active Directory in Windows Server 2012 R2. Per ulteriori informazioni, vedere [Distribuire un cluster scollegato da Active Directory](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11)).

## <a name="install-the-failover-clustering-feature"></a>Installare la funzionalità Clustering di failover

È necessario installare la funzionalità Clustering di failover in tutti i server da aggiungere come nodi del cluster.

### <a name="install-the-failover-clustering-feature"></a>Installare la funzionalità Clustering di failover

1. Avviare Server Manager.
2. Nel **Manage** dal menu **Aggiungi ruoli e funzionalità**.
3. Nel **prima di iniziare** pagina, selezionare **successivo**.
4. Nel **Seleziona tipo di installazione** pagina, selezionare **installazione basata su ruoli o basata su funzionalità**e quindi selezionare **Avanti**.
5. Nel **Selezione server di destinazione** pagina, selezionare il server in cui si desidera installare la funzionalità e quindi selezionare **successivo**.
6. Nel **Selezione ruoli server** pagina, selezionare **successivo**.
7. Nella pagina **Selezione funzionalità** selezionare la casella di controllo **Clustering di failover**.
8. Per installare gli strumenti di Gestione cluster di failover, selezionare **Aggiungi funzionalità**, quindi selezionare **successivo**.
9. Nel **Conferma selezioni per l'installazione** pagina, selezionare **installare**.
<br>L'installazione della funzionalità Clustering di failover non richiede alcun riavvio del server.

10. Al termine dell'installazione, selezionare **Chiudi**.
11. Ripetere la procedura su tutti i server da aggiungere come nodi del cluster.

> [!NOTE]
> Dopo l'installazione della funzionalità Clustering di failover, è consigliabile applicare gli aggiornamenti più recenti da Windows Update. Inoltre, per un cluster di failover basato su Windows Server 2012, vedere la [hotfix e aggiornamenti consigliati per i cluster di failover Windows Server 2012](https://support.microsoft.com/help/2784261/recommended-hotfixes-and-updates-for-windows-server-2012-based-failove) il supporto tecnico Microsoft articolo e installare gli aggiornamenti applicabili.

## <a name="validate-the-configuration"></a>Convalidare la configurazione

Prima di creare il cluster di failover è consigliabile eseguire la convalida della configurazione per assicurarsi che l'hardware e le relative impostazioni siano compatibili con il clustering di failover. Microsoft supporta una soluzione di cluster solo quando l'intera configurazione supera tutti i test di convalida e tutto l'hardware è certificato per la versione di Windows Server eseguita nei nodi del cluster.

> [!NOTE]
> Per l'esecuzione di tutti i test sono necessari almeno due nodi. Se si dispone di un solo nodo, molti test fondamentali relativi all'archiviazione non verranno eseguiti.

### <a name="run-cluster-validation-tests"></a>Eseguire test di convalida cluster

1. In un computer in cui sono installati gli strumenti di gestione del cluster di failover da Strumenti di amministrazione remota del server, oppure in un server in cui è installata la funzionalità Clustering di failover, avviare Gestione cluster di failover. A tale scopo in un server avviare Server Manager e quindi scegliere il **Tools** dal menu **gestione Cluster di Failover**.
2. Nel **gestione Cluster di Failover** riquadro, di sotto **Management**, selezionare **convalida configurazione**.
3. Nel **prima di iniziare** pagina, selezionare **successivo**.
4. Nel **selezione dei server o un Cluster** nella pagina il **immettere il nome** casella, immettere il nome NetBIOS o il nome di dominio completo di un server che si intende aggiungere come nodo del cluster di failover e quindi selezionare **Aggiungere**. Ripetere questo passaggio per ogni server da aggiungere. Per aggiungere più server contemporaneamente, separare i nomi con una virgola o un punto e virgola. Ad esempio, immettere i nomi nel formato `server1.contoso.com, server2.contoso.com`. Al termine, selezionare **successivo**.
5. Nel **opzioni di Testing** pagina, selezionare **Esegui tutti i test (scelta consigliati)** e quindi selezionare **Avanti**.
6. Nel **conferma** pagina, selezionare **successivo**.

    Nella pagina Convalida in corso verrà visualizzato lo stato dei test in esecuzione.
7. Nella pagina **Riepilogo** eseguire una delle operazioni seguenti:
    
      - Se i risultati indicano che i test sono stati completati correttamente e la configurazione è idonea per il clustering, e si vuole creare il cluster immediatamente, assicurarsi che il **creare il cluster ora utilizzando i nodi convalidati** controllare casella è selezionata, quindi fare clic **fine**. Procedere quindi al passaggio 4 della procedura [Creare il cluster di failover](#create-the-failover-cluster).
      - Se i risultati indicano che si sono verificati errori o avvisi, selezionare **visualizzazione Report** per visualizzare i dettagli e determinare quali problemi devono essere risolti. Tenere presente che un avviso per un test di convalida specifico indica che quell'aspetto del cluster di failover può essere supportato, ma che potrebbe non rispettare le procedure consigliate.
        
        > [!NOTE]
        > Se viene visualizzato un avviso per il test Convalida prenotazione permanente degli spazi di archiviazione, vedere il post di blog sulla [visualizzazione di un avviso relativo alla convalida del cluster di failover di Windows che indica che i dischi in uso non supportano le prenotazioni permanenti per gli spazi di archiviazione](https://blogs.msdn.microsoft.com/clustering/2013/05/24/validate-storage-spaces-persistent-reservation-test-results-with-warning/) per ottenere altre informazioni.

Per altre informazioni sui test di convalida dell'hardware, vedere [Validate Hardware for a Failover Cluster](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v%3dws.11)>).

## <a name="create-the-failover-cluster"></a>Creare il cluster di failover

Per completare questo passaggio, assicurarsi che l'account utente usato per l'accesso soddisfi i requisiti descritti nella sezione [Verificare i prerequisiti](#verify-the-prerequisites) di questo argomento.

1. Avviare Server Manager.
2. Nel **degli strumenti** dal menu **gestione Cluster di Failover**.
3. Nel **gestione Cluster di Failover** riquadro, di sotto **Management**, selezionare **crea Cluster**.
    
    Verrà visualizzata la Creazione guidata cluster.
4. Nel **prima di iniziare** pagina, selezionare **successivo**.
5. Se il **selezione dei server** verrà visualizzata la pagina, nelle **immettere il nome** casella, immettere il nome NetBIOS o il nome di dominio completo di un server che si intende aggiungere come nodo del cluster di failover e quindi selezionare **Aggiungere**. Ripetere questo passaggio per ogni server da aggiungere. Per aggiungere più server contemporaneamente, separare i nomi con una virgola o un punto e virgola. Immettere ad esempio i nomi nel formato *server1.contoso.com; server2.contoso.com*. Al termine, selezionare **successivo**.
    
    > [!NOTE]
    > Se si sceglie di creare il cluster immediatamente dopo aver eseguito la convalida nella [configurazione procedure di convalida](#validate-the-configuration), non sarà visibile il **selezione dei server** pagina. I nodi che hanno superato la convalida vengono aggiunti automaticamente alla Creazione guidata cluster e non sarà più necessario immetterli successivamente.
6. Se si è precedentemente ignorata la convalida, verrà visualizzata la pagina **Avviso di convalida** . È consigliabile che la convalida del cluster venga eseguita. Solo i cluster che superano tutti i test di convalida sono supportati da Microsoft. Per eseguire i test di convalida, selezionare **Yes**, quindi selezionare **successivo**. Completare la convalida guidata configurazione come descritto in [convalidare la configurazione](#validate-the-configuration).
7. Nella pagina **Punto di accesso per l'amministrazione del cluster** eseguire le operazioni seguenti:
    
    1. Nella casella **Nome cluster** immettere il nome che si vuole usare per amministrare il cluster. Prima di procedere, tuttavia, prendere nota delle informazioni seguenti:
        
          - Durante la creazione del cluster, questo nome verrà registrato come oggetto computer del cluster (anche noto come *oggetto nome cluster* o *CNO*) in Servizi di dominio Active Directory. Se si specifica un nome NetBIOS per il cluster, l'oggetto CNO viene creato nello stesso percorso in cui risiedono gli altri oggetti computer per i nodi del cluster. Questo può essere il contenitore Computer predefinito o un'unità organizzativa.
          - Per specificare un percorso diverso per l'oggetto CNO, è possibile immettere il nome distinto di un'unità organizzativa nella casella **Nome cluster**. Ad esempio: *CN=NomeCluster, OU=Cluster, DC=Contoso, DC=com*.
          - Se un amministratore di dominio ha pre-installato l'oggetto CNO in un'unità organizzativa differente rispetto a quella in cui risiedono i nodi del cluster, specificare il nome distinto fornito dall'amministratore.
    2. Se il server non dispone di un adattatore di rete configurato per usare DHCP, è necessario configurare uno o più indirizzi IP statici per il cluster di failover. Selezionare la casella di controllo accanto a ogni rete da usare per la gestione del cluster. Selezionare il **indirizzo** campo accanto a una rete selezionata e quindi immettere l'indirizzo IP che si desidera assegnare al cluster. Questo indirizzo (o questi indirizzi) IP verranno associati al nome del cluster in ambito DNS (Domain Name System).
    3. Al termine, selezionare **successivo**.
8. Verificare le impostazioni nella finestra di dialogo **Conferma**. Per impostazione predefinita, la casella di controllo **Aggiungi tutte le risorse di archiviazione idonee al cluster** è selezionata. Deselezionarla se si intende procedere in uno dei modi seguenti:
    
      - Si vuole configurare l'archiviazione in un secondo tempo.
      - Si prevede di creare spazi di archiviazione in cluster mediante Gestione cluster di failover o mediante i cmdlet di Windows PowerShell per il clustering di failover e non si sono ancora creati spazi di archiviazione in Servizi file e archiviazione. Per altre informazioni, vedere [Deploy Clustered Storage Spaces](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11)>).
9. Selezionare **successivo** per creare il cluster di failover.
10. Nella pagina **Riepilogo** confermare che il cluster di failover è stato creato correttamente. Se si sono verificati eventuali avvisi o errori, visualizzare l'output di riepilogo o select **visualizzazione Report** per visualizzare il report completo. Selezionare **fine**.
11. Per confermare che il cluster sia stato creato, verificare che il nome sia elencato nell'albero di spostamento di **Gestione cluster di failover**. È possibile espandere il nome del cluster e quindi selezionare gli elementi sotto **i nodi**, **archiviazione** oppure **reti** per visualizzare le risorse associate.
    
    Tenere presente che il completamento della replica del nome del cluster nel DNS potrebbe richiedere del tempo. Dopo la corretta registrazione e replica DNS, se si seleziona **tutti i server** in Server Manager, il nome del cluster dovrebbe essere elencato come un server con un **gestibilità** stato di **Online** .

Dopo la creazione del cluster, è possibile eseguire operazioni quali la verifica della configurazione del quorum del cluster e, facoltativamente, la creazione di volumi condivisi cluster. Per altre informazioni, vedere [Quorum comprensione in spazi di archiviazione diretta](../storage/storage-spaces/understand-quorum.md) e [usare volumi condivisi Cluster in un cluster di failover](failover-cluster-csvs.md).

## <a name="create-clustered-roles"></a>Creare ruoli del cluster

Dopo la creazione del cluster di failover, è possibile creare ruoli del cluster per ospitarne i carichi di lavoro.

>[!NOTE]
>Per i ruoli del cluster che richiedono un punto di accesso client, viene creato un oggetto computer virtuale in Servizi di dominio Active Directory. Per impostazione predefinita, tutti gli oggetti computer virtuale per il cluster vengono creati nello stesso contenitore o unità organizzativa in cui risiede l'oggetto nome cluster. Tenere presente che in seguito alla creazione di un cluster è possibile spostare l'oggetto nome cluster in qualsiasi unità organizzativa.

Di seguito viene illustrato come creare un ruolo del cluster:

1. Usare Server Manager o Windows PowerShell per installare il ruolo o la funzionalità richiesti per un ruolo del cluster in ogni nodo del cluster stesso. Per creare un file server del cluster, ad esempio, installare il ruolo File server in tutti i nodi del cluster.
    
    Nella tabella seguente vengono indicati i ruoli del cluster che è possibile configurare nella Configurazione guidata disponibilità elevata e il ruolo server o funzionalità associata che è necessario installare come prerequisito.

   | Ruolo del cluster  | Funzionalità o ruolo prerequisito  |
   | ---------       | ---------                    |
   | Server Namespace     |   Spazi dei nomi (parte del ruolo File Server)       |
   | Server dello spazio dei nomi DFS     |  Ruolo Server DHCP       |
   | Distributed Transaction Coordinator (DTC)     | Nessuno        |
   | File Server     |  Ruolo File server       |
   | Applicazione generica     |  Non applicabile       |
   | Script generico     |   Non applicabile      |
   | Servizio generico     |   Non applicabile      |
   | Gestore di replica Hyper-V     |   Ruolo Hyper-V      |
   | Server di destinazione iSCSI     |    Server di destinazione iSCSI (parte del ruolo File server)     |
   | Server iSNS     |  Funzionalità servizio server iSNS       |
   | Accodamento messaggi     |  Funzionalità servizio di Accodamento messaggi       |
   | Altro server     |  Nessuno       |
   | Macchina virtuale     |  Ruolo Hyper-V       |
   | Server WINS     |   Funzionalità Server WINS      |

2. In Gestione Cluster di Failover espandere il nome del cluster, fare doppio clic su **ruoli**, quindi selezionare **Configura ruolo**.
3. Seguire i passaggi nella Configurazione guidata disponibilità elevata per creare il ruolo del cluster.
4. Per verificare che il ruolo del cluster sia stato creato, nel riquadro **Ruoli** assicurarsi che il ruolo presenti lo stato **In esecuzione**. Nel riquadro Ruoli è inoltre indicato il ruolo proprietario. Per testare il failover, fare clic sul ruolo, scegliere **spostare**, quindi selezionare **Seleziona nodo**. Nel **Sposta ruolo cluster** finestra di dialogo, selezionare il nodo del cluster desiderato e quindi selezionare **OK**. Nella colonna **Nodo proprietario** verificare che il ruolo proprietario sia stato modificato.

## <a name="create-a-failover-cluster-by-using-windows-powershell"></a>Creare un cluster di failover usando Windows PowerShell

I seguenti cmdlet di Windows PowerShell eseguire le stesse funzioni come le procedure descritte in precedenza in questo argomento. Immettere ogni cmdlet su una singola riga, anche se le parole potrebbero andare automaticamente a capo e quindi apparire su più righe a causa dei limiti di formattazione.

> [!NOTE]
> È necessario usare Windows PowerShell per creare un cluster scollegato da Active Directory in Windows Server 2012 R2. Per informazioni sulla sintassi, vedere [Deploy an Active Directory-Detached Cluster](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11)).

Nell'esempio seguente viene installata la funzionalità Clustering di failover.

```PowerShell
Install-WindowsFeature –Name Failover-Clustering –IncludeManagementTools
```

L'esempio seguente permette di eseguire tutti i test di convalida del cluster nei computer denominati *Server1* e *Server2*.

```PowerShell
Test-Cluster –Node Server1, Server2
```

> [!NOTE]
> Il **Test-Cluster** cmdlet restituisce i risultati in un file di log nella directory di lavoro corrente. Ad esempio:  C:\Users\<username > \AppData\Local\Temp.

Nell'esempio seguente viene creato un cluster di failover denominato *MyCluster* con i nodi *Server1* e *Server2*, viene assegnato l'indirizzo IP statico *192.168.1.12*e viene infine aggiunta tutta l'archiviazione idonea al cluster di failover.

```PowerShell
New-Cluster –Name MyCluster –Node Server1, Server2 –StaticAddress 192.168.1.12
```

Nell'esempio seguente viene creato lo stesso cluster di failover dell'esempio precedente, ma non viene aggiunta l'archiviazione idonea al cluster di failover.

```PowerShell
New-Cluster –Name MyCluster –Node Server1, Server2 –StaticAddress 192.168.1.12 -NoStorage
```

Nell'esempio seguente viene creato un cluster denominato *MyCluster* nell'unità organizzativa *Cluster* del dominio *Contoso.com*.

```PowerShell
New-Cluster -Name CN=MyCluster,OU=Cluster,DC=Contoso,DC=com -Node Server1, Server2
```

Per esempi su come aggiungere i ruoli del cluster, vedere argomenti quali [Add-ClusterFileServerRole](https://docs.microsoft.com/powershell/module/failoverclusters/add-clusterfileserverrole?view=win10-ps) e [Add-ClusterGenericApplicationRole](https://docs.microsoft.com/powershell/module/failoverclusters/add-clustergenericapplicationrole?view=win10-ps).

## <a name="more-information"></a>Altre informazioni

  - [Clustering di failover](failover-clustering.md)
  - [Distribuire un Cluster Hyper-V](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj863389(v%3dws.11)>)
  - [Scale-Out File Server per i dati dell'applicazione](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831349(v%3dws.11)>)
  - [Distribuire un Cluster scollegato da Directory attivo](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11))
  - [Uso di cluster Guest per la disponibilità elevata](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn440540(v%3dws.11)>)
  - [Aggiornamento compatibile con cluster](cluster-aware-updating.md)
  - [Nuovo Cluster](https://docs.microsoft.com/powershell/module/failoverclusters/new-cluster?view=win10-ps)
  - [Test-Cluster](https://docs.microsoft.com/powershell/module/failoverclusters/test-cluster?view=win10-ps)
