---
title: Creare un cluster di failover
description: Come creare un cluster di failover per Windows Server 2012 R2, Windows Server 2012, Windows Server 2016 e Windows Server 2019.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.openlocfilehash: 496ae061d438a429ba13ee49d9da94127f308f37
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369804"
---
# <a name="create-a-failover-cluster"></a>Creare un cluster di failover

>Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012

In questo argomento viene descritto come creare un cluster di failover usando lo snap-in Gestione cluster di failover o Windows PowerShell. Verrà illustrata una distribuzione tipica, in cui vengono creati oggetti computer per il cluster, insieme ai relativi ruoli del cluster, in Servizi di dominio Active Directory. Se si sta distribuendo un cluster di Spazi di archiviazione diretta, vedere invece [Deploy spazi di archiviazione diretta](../storage/storage-spaces/deploy-storage-spaces-direct.md).

È anche possibile distribuire un cluster scollegato da Active Directory. Con questo tipo di distribuzione è possibile creare un cluster di failover senza le autorizzazioni necessarie per creare oggetti computer in Servizi di dominio Active Directory o senza dover richiedere la pre-installazione degli oggetti computer in Servizi di dominio Active Directory. Questa opzione è disponibile solo tramite Windows PowerShell ed è consigliata solo per scenari specifici. Per ulteriori informazioni, vedere [Distribuire un cluster scollegato da Active Directory](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11)).

#### <a name="checklist-create-a-failover-cluster"></a>Elenco di controllo: Creare un cluster di failover

| Stato | Attività | Riferimenti |
| ---    | ---  | ---       |
| ☐    | Verificare i prerequisiti | [Verificare i prerequisiti](#verify-the-prerequisites) |
| ☐    | Installare la funzionalità Clustering di failover in tutti i server da aggiungere come nodi del cluster | [Installare la funzionalità clustering di failover](#install-the-failover-clustering-feature) |
| ☐    | Eseguire la convalida guidata del cluster per la verifica della configurazione | [Convalidare la configurazione](#validate-the-configuration) |
| ☐ | Eseguire la Creazione guidata cluster per creare il cluster di failover | [Creare il cluster di failover](#create-the-failover-cluster) |
| ☐ | Creare ruoli del cluster per ospitare i carichi di lavoro del cluster | [Creare ruoli del cluster](#create-clustered-roles) |

## <a name="verify-the-prerequisites"></a>Verificare i prerequisiti

Prima di iniziare, verificare i prerequisiti seguenti:

- Assicurarsi che tutti i server da aggiungere come nodi del cluster eseguano la stessa versione di Windows Server.
- Esaminare i requisiti hardware per assicurarsi che la configurazione in uso sia supportata. Per altre informazioni, vedere [Requisiti hardware per il clustering di failover e opzioni di archiviazione](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj612869(v%3dws.11)). Se si sta creando un cluster Spazi di archiviazione diretta, vedere [spazi di archiviazione diretta requisiti hardware](../storage/storage-spaces/storage-spaces-direct-hardware-requirements.md).
- Per aggiungere spazio di archiviazione in cluster durante la creazione del cluster, assicurarsi che tutti i server possano accedere all'archiviazione. L'archiviazione in cluster può essere aggiunta anche dopo il completamento della creazione del cluster.
- Assicurarsi che tutti i server da aggiungere come nodi del cluster siano aggiunti allo stesso dominio Active Directory.
- (Facoltativo). Creare un'unità organizzativa e spostarvi all'interno gli account computer per i server da aggiungere come nodi del cluster. La procedura consigliata prevede il posizionamento dei cluster di failover in una propria unità organizzativa all'interno di Servizi di dominio Active Directory. Questo può aiutare a controllare meglio quali impostazioni di Criteri di gruppo o di modelli di sicurezza hanno effetto sui nodi del cluster. L'isolamento del cluster in una propria unità organizzativa aiuta inoltre a prevenire l'eliminazione accidentale di oggetti computer del cluster.

Verificare anche i requisiti di account seguenti:

- Assicurarsi che l'account da usare per la creazione del cluster sia un utente di dominio che dispone di diritti di amministratore su tutti i server da aggiungere come nodi del cluster.
- Assicurarsi che una delle condizioni seguenti sia vera:
    - l'utente che crea il cluster dispone dell'autorizzazione **Crea oggetti computer** per l'unità organizzativa o il contenitore in cui risiedono i server che costituiranno il cluster.
    - Se l'utente non dispone dell'autorizzazione **Crea oggetti computer**, rivolgersi a un amministratore di dominio richiedendo la pre-installazione di un oggetto computer del cluster per il nuovo cluster. Per altre informazioni, vedere [Pre-installare oggetti computer del cluster in Servizi di dominio Active Directory](prestage-cluster-adds.md).

> [!NOTE]
> Questo requisito non si applica se si vuole creare un cluster scollegato da Active Directory in Windows Server 2012 R2. Per ulteriori informazioni, vedere [Distribuire un cluster scollegato da Active Directory](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11)).

## <a name="install-the-failover-clustering-feature"></a>Installare la funzionalità Clustering di failover

È necessario installare la funzionalità Clustering di failover in tutti i server da aggiungere come nodi del cluster.

### <a name="install-the-failover-clustering-feature"></a>Installare la funzionalità Clustering di failover

1. Avviare Server Manager.
2. Scegliere **Aggiungi ruoli e funzionalità**dal menu **Gestisci** .
3. Nella pagina **prima di iniziare** selezionare **Avanti**.
4. Nella pagina **Selezione tipo di installazione** selezionare **installazione basata su ruoli o basata su funzionalità**e quindi fare clic su **Avanti**.
5. Nella pagina **Selezione server di destinazione** selezionare il server in cui si vuole installare la funzionalità, quindi fare clic su **Avanti**.
6. Nella pagina **Selezione ruoli server** selezionare **Avanti**.
7. Nella pagina **Selezione funzionalità** selezionare la casella di controllo **Clustering di failover**.
8. Per installare gli strumenti di gestione del cluster di failover, selezionare **Aggiungi funzionalità**e quindi fare clic su **Avanti**.
9. Nella pagina **Conferma selezioni** per l'installazione selezionare **Installa**.
<br>L'installazione della funzionalità Clustering di failover non richiede alcun riavvio del server.

10. Al termine dell'installazione, fare clic su **Chiudi**.
11. Ripetere la procedura su tutti i server da aggiungere come nodi del cluster.

> [!NOTE]
> Dopo l'installazione della funzionalità Clustering di failover, è consigliabile applicare gli aggiornamenti più recenti da Windows Update. Inoltre, per un cluster di failover basato su Windows Server 2012, esaminare gli [aggiornamenti rapidi e gli aggiornamenti consigliati per i cluster di failover basati su Windows server 2012](https://support.microsoft.com/help/2784261/recommended-hotfixes-and-updates-for-windows-server-2012-based-failove) supporto tecnico Microsoft articolo e installare gli eventuali aggiornamenti applicabili.

## <a name="validate-the-configuration"></a>Convalidare la configurazione

Prima di creare il cluster di failover è consigliabile eseguire la convalida della configurazione per assicurarsi che l'hardware e le relative impostazioni siano compatibili con il clustering di failover. Microsoft supporta una soluzione di cluster solo quando l'intera configurazione supera tutti i test di convalida e tutto l'hardware è certificato per la versione di Windows Server eseguita nei nodi del cluster.

> [!NOTE]
> Per l'esecuzione di tutti i test sono necessari almeno due nodi. Se si dispone di un solo nodo, molti test fondamentali relativi all'archiviazione non verranno eseguiti.

### <a name="run-cluster-validation-tests"></a>Eseguire i test di convalida del cluster

1. In un computer in cui sono installati gli strumenti di gestione del cluster di failover da Strumenti di amministrazione remota del server, oppure in un server in cui è installata la funzionalità Clustering di failover, avviare Gestione cluster di failover. Per eseguire questa operazione in un server, avviare Server Manager, quindi scegliere **Gestione cluster di failover**dal menu **strumenti** .
2. Nel riquadro **Gestione cluster di failover** fare clic su **Convalida configurazione**in **gestione**.
3. Nella pagina **prima di iniziare** selezionare **Avanti**.
4. Nella pagina **selezione di server o di un cluster** , nella casella **immettere** il nome, immettere il nome NetBIOS o il nome di dominio completo di un server che si prevede di aggiungere come nodo del cluster di failover e quindi selezionare **Aggiungi**. Ripetere questo passaggio per ogni server da aggiungere. Per aggiungere più server contemporaneamente, separare i nomi con una virgola o un punto e virgola. Ad esempio, immettere i nomi nel formato `server1.contoso.com, server2.contoso.com`. Al termine, fare clic su **Avanti**.
5. Nella pagina **Opzioni di testing** selezionare **Esegui tutti i test (scelta consigliata)** e quindi fare clic su **Avanti**.
6. Nella pagina **conferma** selezionare **Avanti**.

    Nella pagina Convalida in corso verrà visualizzato lo stato dei test in esecuzione.
7. Nella pagina **Riepilogo** eseguire una delle operazioni seguenti:
    
      - Se i risultati indicano che i test sono stati completati correttamente e la configurazione è adatta per il clustering e si desidera creare immediatamente il cluster, assicurarsi che la casella di controllo **Crea il cluster ora utilizzando i nodi convalidati** sia selezionata, quindi Selezionare **fine**. Procedere quindi al passaggio 4 della procedura [Creare il cluster di failover](#create-the-failover-cluster).
      - Se i risultati indicano la presenza di avvisi o errori, selezionare **Visualizza report** per visualizzare i dettagli e determinare quali problemi devono essere corretti. Tenere presente che un avviso per un test di convalida specifico indica che quell'aspetto del cluster di failover può essere supportato, ma che potrebbe non rispettare le procedure consigliate.
        
        > [!NOTE]
        > Se viene visualizzato un avviso per il test Convalida prenotazione permanente degli spazi di archiviazione, vedere il post di blog sulla [visualizzazione di un avviso relativo alla convalida del cluster di failover di Windows che indica che i dischi in uso non supportano le prenotazioni permanenti per gli spazi di archiviazione](https://blogs.msdn.microsoft.com/clustering/2013/05/24/validate-storage-spaces-persistent-reservation-test-results-with-warning/) per ottenere altre informazioni.

Per altre informazioni sui test di convalida dell'hardware, vedere [Validate Hardware for a Failover Cluster](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v%3dws.11)>).

## <a name="create-the-failover-cluster"></a>Creare il cluster di failover

Per completare questo passaggio, assicurarsi che l'account utente usato per l'accesso soddisfi i requisiti descritti nella sezione [Verificare i prerequisiti](#verify-the-prerequisites) di questo argomento.

1. Avviare Server Manager.
2. Scegliere **Gestione cluster di failover**dal menu **strumenti** .
3. Nel riquadro **Gestione cluster di failover** fare clic su **Crea cluster**in **gestione**.
    
    Verrà visualizzata la Creazione guidata cluster.
4. Nella pagina **prima di iniziare** selezionare **Avanti**.
5. Se viene visualizzata la pagina **Selezione server** , nella casella **immettere** il nome immettere il nome NetBIOS o il nome di dominio completo di un server che si prevede di aggiungere come nodo del cluster di failover e quindi selezionare **Aggiungi**. Ripetere questo passaggio per ogni server da aggiungere. Per aggiungere più server contemporaneamente, separare i nomi con una virgola o un punto e virgola. Immettere ad esempio i nomi nel formato *server1.contoso.com; server2.contoso.com*. Al termine, fare clic su **Avanti**.
    
    > [!NOTE]
    > Se si sceglie di creare il cluster immediatamente dopo aver eseguito la convalida nella [procedura di convalida della configurazione](#validate-the-configuration), la pagina **Selezione server** non sarà visualizzata. I nodi che hanno superato la convalida vengono aggiunti automaticamente alla Creazione guidata cluster e non sarà più necessario immetterli successivamente.
6. Se si è precedentemente ignorata la convalida, verrà visualizzata la pagina **Avviso di convalida** . È consigliabile che la convalida del cluster venga eseguita. Solo i cluster che superano tutti i test di convalida sono supportati da Microsoft. Per eseguire i test di convalida, selezionare **Sì**e quindi fare clic su **Avanti**. Completare la convalida guidata configurazione come descritto in [convalidare la configurazione](#validate-the-configuration).
7. Nella pagina **Punto di accesso per l'amministrazione del cluster** eseguire le operazioni seguenti:
    
    1. Nella casella **Nome cluster** immettere il nome che si vuole usare per amministrare il cluster. Prima di procedere, tuttavia, prendere nota delle informazioni seguenti:
        
          - Durante la creazione del cluster, questo nome verrà registrato come oggetto computer del cluster (anche noto come *oggetto nome cluster* o *CNO*) in Servizi di dominio Active Directory. Se si specifica un nome NetBIOS per il cluster, l'oggetto CNO viene creato nello stesso percorso in cui risiedono gli altri oggetti computer per i nodi del cluster. Questo può essere il contenitore Computer predefinito o un'unità organizzativa.
          - Per specificare un percorso diverso per l'oggetto CNO, è possibile immettere il nome distinto di un'unità organizzativa nella casella **Nome cluster**. Esempio: *CN=NomeCluster, OU=Cluster, DC=Contoso, DC=com*.
          - Se un amministratore di dominio ha pre-installato l'oggetto CNO in un'unità organizzativa differente rispetto a quella in cui risiedono i nodi del cluster, specificare il nome distinto fornito dall'amministratore.
    2. Se il server non dispone di un adattatore di rete configurato per usare DHCP, è necessario configurare uno o più indirizzi IP statici per il cluster di failover. Selezionare la casella di controllo accanto a ogni rete da usare per la gestione del cluster. Selezionare il campo **Indirizzo** accanto a una rete selezionata, quindi immettere l'indirizzo IP che si desidera assegnare al cluster. Questo indirizzo (o questi indirizzi) IP verranno associati al nome del cluster in ambito DNS (Domain Name System).
    3. Al termine, fare clic su **Avanti**.
8. Verificare le impostazioni nella finestra di dialogo **Conferma**. Per impostazione predefinita, la casella di controllo **Aggiungi tutte le risorse di archiviazione idonee al cluster** è selezionata. Deselezionarla se si intende procedere in uno dei modi seguenti:
    
      - Si vuole configurare l'archiviazione in un secondo tempo.
      - Si prevede di creare spazi di archiviazione in cluster mediante Gestione cluster di failover o mediante i cmdlet di Windows PowerShell per il clustering di failover e non si sono ancora creati spazi di archiviazione in Servizi file e archiviazione. Per altre informazioni, vedere [Deploy Clustered Storage Spaces](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11)>).
9. Selezionare **Avanti** per creare il cluster di failover.
10. Nella pagina **Riepilogo** confermare che il cluster di failover è stato creato correttamente. Se sono presenti avvisi o errori, visualizzare l'output di riepilogo o selezionare **Visualizza report** per visualizzare il report completo. Selezionare **Fine**.
11. Per confermare che il cluster sia stato creato, verificare che il nome sia elencato nell'albero di spostamento di **Gestione cluster di failover**. È possibile espandere il nome del cluster e quindi selezionare gli elementi in **nodi**, **archiviazione** o **reti** per visualizzare le risorse associate.
    
    Tenere presente che il completamento della replica del nome del cluster nel DNS potrebbe richiedere del tempo. Dopo la corretta registrazione e replica DNS, se si selezionano **tutti i server** in Server Manager, il nome del cluster dovrebbe essere elencato come server con lo stato di **gestibilità** **online**.

Dopo la creazione del cluster, è possibile eseguire operazioni quali la verifica della configurazione del quorum del cluster e, facoltativamente, la creazione di volumi condivisi cluster. Per ulteriori informazioni, vedere informazioni sul [quorum in spazi di archiviazione diretta](../storage/storage-spaces/understand-quorum.md) e [utilizzo dei volumi condivisi del cluster in un cluster di failover](failover-cluster-csvs.md).

## <a name="create-clustered-roles"></a>Creare ruoli del cluster

Dopo la creazione del cluster di failover, è possibile creare ruoli del cluster per ospitarne i carichi di lavoro.

>[!NOTE]
>Per i ruoli del cluster che richiedono un punto di accesso client, viene creato un oggetto computer virtuale in Servizi di dominio Active Directory. Per impostazione predefinita, tutti gli oggetti computer virtuale per il cluster vengono creati nello stesso contenitore o unità organizzativa in cui risiede l'oggetto nome cluster. Tenere presente che in seguito alla creazione di un cluster è possibile spostare l'oggetto nome cluster in qualsiasi unità organizzativa.

Di seguito viene illustrato come creare un ruolo del cluster:

1. Usare Server Manager o Windows PowerShell per installare il ruolo o la funzionalità richiesti per un ruolo del cluster in ogni nodo del cluster stesso. Per creare un file server del cluster, ad esempio, installare il ruolo File server in tutti i nodi del cluster.
    
    Nella tabella seguente vengono indicati i ruoli del cluster che è possibile configurare nella Configurazione guidata disponibilità elevata e il ruolo server o funzionalità associata che è necessario installare come prerequisito.

   | Ruolo del cluster  | Funzionalità o ruolo prerequisito  |
   | ---------       | ---------                    |
   | Server dello spazio dei nomi     |   Spazi dei nomi (parte del ruolo file server)       |
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

2. In Gestione cluster di failover espandere il nome del cluster, fare clic con il pulsante destro del mouse su **ruoli**e quindi scegliere **Configura ruolo**.
3. Seguire i passaggi nella Configurazione guidata disponibilità elevata per creare il ruolo del cluster.
4. Per verificare che il ruolo del cluster sia stato creato, nel riquadro **Ruoli** assicurarsi che il ruolo presenti lo stato **In esecuzione**. Nel riquadro Ruoli è inoltre indicato il ruolo proprietario. Per testare il failover, fare clic con il pulsante destro del mouse sul ruolo, scegliere **Sposta**, quindi selezionare **Seleziona nodo**. Nella finestra di dialogo **Sposta ruolo cluster** selezionare il nodo del cluster desiderato e quindi fare clic su **OK**. Nella colonna **Nodo proprietario** verificare che il ruolo proprietario sia stato modificato.

## <a name="create-a-failover-cluster-by-using-windows-powershell"></a>Creare un cluster di failover usando Windows PowerShell

I cmdlet di Windows PowerShell seguenti eseguono le stesse funzioni delle procedure precedenti di questo argomento. Immettere ogni cmdlet su una singola riga, anche se le parole potrebbero andare automaticamente a capo e quindi apparire su più righe a causa dei limiti di formattazione.

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
> Il cmdlet **test-cluster** restituisce i risultati in un file di log nella directory di lavoro corrente. Esempio: C:\Users @ no__t-0username > \AppData\Local\Temp.

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
  - [Distribuire un cluster Hyper-V](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj863389(v%3dws.11)>)
  - [File server di scalabilità orizzontale per i dati delle applicazioni](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831349(v%3dws.11)>)
  - [Distribuire un cluster scollegato da Active Directory](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11))
  - [Uso del clustering guest per la disponibilità elevata](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn440540(v%3dws.11)>)
  - [Aggiornamento compatibile con cluster](cluster-aware-updating.md)
  - [Nuovo-cluster](https://docs.microsoft.com/powershell/module/failoverclusters/new-cluster?view=win10-ps)
  - [Test-cluster](https://docs.microsoft.com/powershell/module/failoverclusters/test-cluster?view=win10-ps)
