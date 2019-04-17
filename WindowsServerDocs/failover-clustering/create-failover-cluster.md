---
title: Creare un cluster di failover
description: Come creare un cluster di failover di Windows Server 2012 R2, Windows Server 2012 e Windows Server 2016.
ms.prod: windows-server-threshold
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.date: 11/05/2018
ms.localizationpriority: medium
ms.openlocfilehash: f919e69488c4f2272ddd07e535ba4e2248ddf79c
ms.sourcegitcommit: 5cbaca9685720f11d896c4ca167c86e74c032feb
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/06/2018
ms.locfileid: "6068979"
---
# Creare un cluster di failover

>Si applica a: Windows Server 2012 R2, Windows Server 2012, Windows Server 2016

In questo argomento viene illustrato come creare un cluster di failover tramite snap-in Gestione Cluster di Failover o Windows PowerShell. Questo argomento illustra un'installazione tipica, in cui vengono creati oggetti computer per il cluster e i relativi ruoli cluster associati in Active Directory Domain Services (AD DS). Se stai distribuendo un cluster di spazi di archiviazione diretta, vedere invece [Distribuire spazi di archiviazione diretta](../storage/storage-spaces/deploy-storage-spaces-direct.md).

Puoi anche distribuire un cluster scollegato Active Directory. Questo metodo di distribuzione consente di creare un cluster di failover senza le autorizzazioni per creare oggetti di computer in AD DS o la necessità di richiedere tale computer sono stati assegnati preventivamente gli oggetti in AD DS. Questa opzione è disponibile solo tramite Windows PowerShell ed è consigliata solo per scenari specifici. Per altre informazioni, vedere [distribuire un Cluster Active Directory-Detached](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11)).

#### Elenco di controllo: Creare un cluster di failover

|Stato|Attività|Informazioni di riferimento|
|:---:|---|---|
|☐|Verificare i prerequisiti|[Verificare i prerequisiti](#verify-the-prerequisites)|
|☐|Installa la funzionalità Clustering di Failover in ogni server che si desidera aggiungere come un nodo del cluster|[Installare la funzionalità Clustering di Failover](#install-the-failover-clustering-feature)|
|☐|Esegui la procedura guidata convalida dei Cluster per convalidare la configurazione|[Convalidare la configurazione](#validate-the-configuration)|
|☐|Eseguire la creazione guidata Cluster per creare il cluster di failover|[Creare il cluster di failover](#create-the-failover-cluster)|
|☐|Creare ruoli cluster per i carichi di lavoro di host del cluster|[Creare ruoli cluster](#create-clustered-roles)|

## Verificare i prerequisiti

Prima di iniziare, verifica i prerequisiti seguenti:

- Assicurati che tutti i server che si desiderano aggiungere come nodi del cluster eseguano la stessa versione di Windows Server.
- Esaminare i requisiti hardware per assicurarsi che la configurazione è supportata. Per altre informazioni, vedi [i requisiti Hardware di Clustering di Failover e opzioni di archiviazione](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj612869(v%3dws.11)). Se stai creando un cluster di spazi di archiviazione diretta, vedere [i requisiti hardware di spazi di archiviazione diretta](../storage/storage-spaces/storage-spaces-direct-hardware-requirements.md).
- Per aggiungere i cluster di archiviazione durante la creazione di cluster, assicurati che lo spazio di archiviazione possono accedere a tutti i server. (Puoi anche aggiungere cluster di archiviazione dopo aver creato il cluster.)
- Assicurati che tutti i server che si desiderano aggiungere come nodi del cluster vengono aggiunti al dominio di Active Directory stesso.
- (Facoltativo) Creare un'unità organizzativa (UO) e sposta gli account computer per i server che si desiderano aggiungere come nodi del cluster nell'unità Organizzativa. Come procedura consigliata, ti consigliamo di posizionare i cluster di failover nella propria unità Organizzativa in AD DS. Questo può aiutarti meglio controllare quali impostazioni di criteri di gruppo o impostazioni dei modelli di sicurezza influiscono sui nodi del cluster. Isolando cluster nella propria unità Organizzativa, consente inoltre di impedire la cancellazione accidentale degli oggetti computer cluster.

Inoltre, verificare i requisiti di account seguenti:

- Assicurati che l'account che vuoi usare per creare il cluster sia un utente di dominio con diritti di amministratore su tutti i server che si desiderano aggiungere come nodi del cluster.
- Verificare se una delle seguenti è true:
    - L'utente che crea il cluster dispone dell'autorizzazione di **Computer di creare oggetti** per l'unità Organizzativa o il contenitore in cui si trovano i server che andrà a formare il cluster.
    - Se l'utente non dispone dell'autorizzazione, **creare Computer objects** chiedere a un amministratore di dominio per prestage un oggetto computer cluster per il cluster. Per altre informazioni, vedi [Prestage Cluster Computer Objects in Active Directory Domain Services](prestage-cluster-adds.md).

>[!NOTE]
>Questo requisito non si applica se si desidera creare un cluster scollegato Active Directory in Windows Server 2012 R2. Per altre informazioni, vedere [distribuire un Cluster Active Directory-Detached](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11)).

## Installare la funzionalità Clustering di Failover

È necessario installare la funzionalità Clustering di Failover in ogni server che si desidera aggiungere come un nodo del cluster di failover.

### Installare la funzionalità Clustering di Failover

1. Avvia Server Manager.
2. Dal menu **Manage** seleziona **Aggiungi ruoli e funzionalità**.
3. Nella pagina **prima di iniziare** , seleziona **successivo**.
4. Nella pagina **Selezione tipo di installazione** , seleziona **installazione basato sui ruoli o basata su funzionalità**e quindi seleziona **successivo**.
5. Nella pagina **Seleziona i server di destinazione** , seleziona il server in cui si desidera installare la funzionalità e quindi seleziona **successivo**.
6. Nella pagina **Selezione ruoli server** , seleziona **successivo**.
7. Nella pagina **Select features** , seleziona la casella di controllo **Clustering di Failover** .
8. Per installare gli strumenti di Gestione cluster di failover, seleziona **Aggiungi funzionalità**e quindi seleziona **successivo**.
9. Nella pagina **Conferma selezioni per l'installazione** , selezionare **Installa**.
<br>Un riavvio del server non è necessario per la funzionalità Clustering di Failover.

10. Al termine dell'installazione, selezionare **Chiudi**.
11. Ripeti questa procedura in ogni server che si desidera aggiungere come un nodo del cluster di failover.

>[!NOTE]
>Dopo aver installato la funzionalità Clustering di Failover, consigliamo che applicare gli aggiornamenti più recenti di Windows Update. Inoltre, per un cluster di failover basato su Windows Server 2012, consulta l'articolo di supporto Microsoft [consigliata aggiornamenti rapidi e basato su Windows Server 2012 failover cluster](https://support.microsoft.com/help/2784261/recommended-hotfixes-and-updates-for-windows-server-2012-based-failove) e installare eventuali aggiornamenti che si applicano.

## Convalidare la configurazione

Prima di creare il cluster di failover, è consigliabile convalidare la configurazione per assicurarti che l'hardware e le impostazioni di hardware sono compatibili con il clustering di failover. Microsoft supporta una soluzione cluster solo se la configurazione completa supera la convalida tutti i test e se tutti i componenti hardware è ottenere la certificazione per la versione di Windows Server che eseguono i nodi del cluster.

>[!NOTE]
>Devi disporre di almeno due nodi per l'esecuzione di tutti i test. Se si dispone solo un nodo, molti dei test archiviazione critici non eseguire.

### Eseguire il test di convalida del cluster

1. In un computer che include gli strumenti di gestione Cluster di Failover installati dagli strumenti di amministrazione remota del Server o in un server in cui è installata la funzionalità Clustering di Failover, avviare Gestione Cluster di Failover. A tale scopo in un server, Avvia Server Manager e quindi dal menu **Strumenti** , seleziona **Gestione Cluster di Failover**.
2. Nel riquadro di **Gestione Cluster di Failover** , in **Gestione**selezionare **Convalidare la configurazione**.
3. Nella pagina **Prima di iniziare** , seleziona **successivo**.
4. Nella pagina **Seleziona server o un Cluster** , nella casella di **Immettere il nome** , Immetti il nome NetBIOS o il nome di dominio completo di un server che si prevede di aggiungere come un nodo del cluster di failover e quindi scegli **Aggiungi**. Ripeti questo passaggio per ogni server che si desidera aggiungere. Per aggiungere più server contemporaneamente, separare i nomi da una virgola o da un punto e virgola. Ad esempio, immettere i nomi nel formato *Server1. contoso.com, server2.contoso.com*. Al termine, seleziona **successivo**.
5. Nella pagina **Opzioni di test** , seleziona **eseguire tutti i test (scelta consigliati)** e quindi seleziona **successivo**.
6. Nella pagina di **Conferma** , seleziona **successivo**.

    La pagina di convalida visualizza lo stato dei test in esecuzione.
7. Nella pagina di **Riepilogo** , eseguire una delle operazioni seguenti:
    
      - Se i risultati indicano che i test sia stata completata correttamente e la configurazione è adatta per il clustering e si desidera creare il cluster immediatamente, assicurati che sia selezionata la casella di controllo **Crea il cluster ora utilizzando i nodi convalidati** e quindi Selezionare **Fine**. Quindi, procedere al passaggio 4 della procedura di [creazione del cluster di failover](#create-the-failover-cluster) .
      - Se i risultati indicano che si sono verificati errori o avvisi, seleziona **I Report di visualizzazione** per visualizzare i dettagli e determinare quali problemi devono essere corretti. Tenere presente che un avviso per il test di convalida particolare indica che questo aspetto del cluster di failover può essere supportato, ma potrebbe non soddisfare le procedure consigliate.
        
        >[!NOTE]
        >Se ricevi un messaggio di avviso per il test di convalida archiviazione spazi prenotazione permanente, vedi il blog post [avviso di convalida dei Cluster di Failover di Windows indica che i dischi non supportano le prenotazioni permanenti per spazi di archiviazione](https://blogs.msdn.microsoft.com/clustering/2013/05/24/validate-storage-spaces-persistent-reservation-test-results-with-warning/) per altre informazioni.

Per altre informazioni sull'hardware test di convalida, vedi [Convalidare l'Hardware per un Cluster di Failover](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj134244(v%3dws.11)>).

## Creare il cluster di failover

Per completare questo passaggio, assicurati che l'account utente che si accede come soddisfi i requisiti sono descritte nella sezione [verificare i prerequisiti](#verify-the-prerequisites) di questo argomento.

1. Avvia Server Manager.
2. Dal menu **Strumenti** , seleziona **Gestione Cluster di Failover**.
3. Nel riquadro di **Gestione Cluster di Failover** , in **Gestione**selezionare **Creazione di Cluster**.
    
    Si apre la creazione guidata Cluster.
4. Nella pagina **Prima di iniziare** , seleziona **successivo**.
5. Se viene visualizzata la pagina **Selezione server** , nella casella di **Immettere il nome** , Immetti il nome NetBIOS o il nome di dominio completo di un server che si prevede di aggiungere come un nodo del cluster di failover e quindi scegli **Aggiungi**. Ripeti questo passaggio per ogni server che si desidera aggiungere. Per aggiungere più server contemporaneamente, separare i nomi con una virgola o un punto e virgola. Ad esempio, immettere i nomi nel formato *Server1. contoso.com; server2.contoso.com*. Al termine, seleziona **successivo**.
    
    >[!NOTE]
    >Se hai scelto di creare il cluster immediatamente dopo l'esecuzione di convalida nella [procedura di convalida di configurazione](#validate-the-configuration), non verrà visualizzata la pagina **Selezione server** . I nodi che sono stati convalidati vengono aggiunti automaticamente con la creazione guidata Cluster in modo che non è necessario immettere nuovamente.
6. Se non hai eseguito la convalida in precedenza, viene visualizzata la pagina di **Avviso di convalida** . Consigliamo vivamente di eseguire la convalida del cluster. Sono supportati solo i cluster che superano tutti i test di convalida da Microsoft. Per eseguire i test di convalida, seleziona **Sì**e quindi seleziona **successivo**. Completare la convalida configurazione guidata, come descritto in [convalidare la configurazione](#validate-the-configuration).
7. Nella pagina **Punto di accesso per l'amministrazione del Cluster** , eseguire le operazioni seguenti:
    
    1. Nella casella **Nome del Cluster** , Immetti il nome che vuoi usare per amministrare il cluster. Prima di, esaminare le informazioni seguenti:
        
          - Durante la creazione del cluster, questo nome viene registrato come oggetto di computer cluster (detti anche *l'oggetto nome del cluster* o *CNO*) in Active Directory Domain Services. Se si specifica un nome NetBIOS per il cluster, viene creata la CNO nello stesso percorso in cui si trovano gli oggetti computer per i nodi del cluster. Ciò può essere il contenitore predefinito computer o un'unità Organizzativa.
          - Per specificare una posizione diversa per il CNO, puoi immettere il nome distinto di un'unità Organizzativa nella casella **Nome del Cluster** . Ad esempio: *CN = ClusterName, OU = cluster, DC = Contoso, DC = com*.
          - Se un amministratore di dominio ha preventivamente il CNO in un'unità Organizzativa diversa rispetto a dove si trovano i nodi del cluster, specificare il nome distinto che fornisce l'amministratore di dominio.
    2. Se il server non dispone di una scheda di rete che è configurata per usare il protocollo DHCP, è necessario configurare uno o più indirizzi IP statici per il cluster di failover. Seleziona la casella di controllo accanto a ogni rete che si desidera utilizzare per la gestione di cluster. Seleziona il campo **indirizzo** accanto a una rete selezionata e quindi immettere l'indirizzo IP che si desidera assegnare al cluster. Questo indirizzo IP (o gli indirizzi) sarà associati al nome del cluster nel sistema DNS (Domain Name).
    3. Al termine, seleziona **successivo**.
8. Nella pagina di **Conferma** , esamina le impostazioni. Per impostazione predefinita, viene selezionata la casella di controllo **aggiungere tutta l'archiviazione idoneo al cluster** . Deseleziona questa casella di controllo se si desidera effettuare una delle operazioni seguenti:
    
      - Vuoi configurare l'archiviazione in un secondo momento.
      - Prevedi di creare spazi di archiviazione del cluster tramite Gestione Cluster di Failover o tramite i cmdlet di Windows PowerShell di Clustering di Failover e non hai ancora creato spazi di archiviazione in servizi File e archiviazione. Per altre informazioni, vedi [Distribuire spazi di archiviazione in cluster](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj822937(v%3dws.11)>).
9. Selezionare **Avanti** per creare il cluster di failover.
10. Nella pagina di **Riepilogo** , verifica che il cluster di failover è stato creato correttamente. Se si sono verificati eventuali avvisi o errori, visualizzare l'output di riepilogo o selezionare i **Report di visualizzazione** per visualizzare il report completo. Selezionare **Fine**.
11. Per verificare che il cluster sia stato creato, verificare che il nome del cluster è elencato in **Gestione Cluster di Failover** nella struttura di spostamento. Puoi espandere il nome del cluster e quindi selezionare gli elementi sotto **i nodi**, **archiviazione** o **le reti** per visualizzare le risorse associate.
    
    Tenere presente che potrebbe richiedere alcuni minuti per il nome del cluster replicare correttamente in DNS. Dopo la registrazione DNS ha esito positiva e la replica, se selezioni **Tutti i server** in Server Manager, il nome del cluster dovrebbe essere elencato come server con stato **gestibilità** **Online**.

Dopo aver creato il cluster, è possibile eseguire operazioni come verificare la configurazione quorum del cluster e facoltativamente, creare volumi condivisi Cluster (CSV). Per altre informazioni, vedi [Conoscenza Quorum in spazi di archiviazione diretta](../storage/storage-spaces/understand-quorum.md) e [Usare volumi condivisi del Cluster in un cluster di failover](failover-cluster-csvs.md).

## Creare ruoli cluster

Dopo aver creato il cluster di failover, è possibile creare ruoli cluster per i carichi di lavoro di host del cluster.

>[!NOTE]
>Per i ruoli del cluster che richiedono un punto di accesso client, viene creato un oggetto computer virtuale (VCO) in Active Directory Domain Services. Per impostazione predefinita, tutti i VCOs per il cluster vengono create nello stesso contenitore o unità Organizzativa come il CNO. Tenere presente che, dopo aver creato un cluster, è possibile spostare il CNO in qualsiasi unità Organizzativa.

Ecco come creare un ruolo del cluster:

1. Utilizzare Server Manager o Windows PowerShell per installare il ruolo o funzionalità che è necessaria per un ruolo del cluster in ogni nodo del cluster di failover. Ad esempio, se si desidera creare un file server cluster, installare il ruolo File Server in tutti i nodi del cluster.
    
    La tabella seguente mostra i ruoli del cluster che è possibile configurare la configurazione guidata disponibilità elevata e il ruolo server associato o una funzionalità che è necessario installare come prerequisito.
    
    <table>
    <thead>
    <tr class="header">
    <th>Ruolo del cluster</th>
    <th>Ruolo o funzionalità prerequisito</th>
    </tr>
    </thead>
    <tbody>
    <tr class="odd">
    <td>Server DFS Namespace</td>
    <td>Spazi dei nomi DFS (parte di ruolo File Server)</td>
    </tr>
    <tr class="even">
    <td>Server DHCP</td>
    <td>Ruolo Server DHCP</td>
    </tr>
    <tr class="odd">
    <td>Distributed Transaction Coordinator (DTC)</td>
    <td>Nessuno</td>
    </tr>
    <tr class="even">
    <td>File Server</td>
    <td>Ruolo File server</td>
    </tr>
    <tr class="odd">
    <td>Applicazione generica</td>
    <td>Non applicabile</td>
    </tr>
    <tr class="even">
    <td>Script generico</td>
    <td>Non applicabile</td>
    </tr>
    <tr class="odd">
    <td>Servizio generico</td>
    <td>Non applicabile</td>
    </tr>
    <tr class="even">
    <td>Gestore di Replica Hyper-V</td>
    <td>Ruolo Hyper-V</td>
    </tr>
    <tr class="odd">
    <td>Server di destinazione iSCSI</td>
    <td>Server di destinazione (parte di ruolo File Server) iSCSI</td>
    </tr>
    <tr class="even">
    <td>Server iSNS</td>
    <td>funzionalità di servizio Server iSNS</td>
    </tr>
    <tr class="odd">
    <td>Accodamento messaggi</td>
    <td>Funzione Servizi Accodamento messaggi</td>
    </tr>
    <tr class="even">
    <td>Altro Server</td>
    <td>Nessuno</td>
    </tr>
    <tr class="odd">
    <td>Macchina virtuale</td>
    <td>Ruolo Hyper-V</td>
    </tr>
    <tr class="even">
    <td>Server WINS</td>
    <td>Funzionalità di Server WINS</td>
    </tr>
    </tbody>
    </table>
2. In Gestione Cluster di Failover, Espandi il nome del cluster, destro **ruoli**e quindi seleziona **Configura ruolo**.
3. Segui i passaggi della procedura guidata disponibilità elevata per creare il ruolo del cluster.
4. Per verificare che il ruolo del cluster è stato creato, nel riquadro di **ruoli** , assicurati che il ruolo ha lo stato di **esecuzione**. Il riquadro ruoli indica anche il nodo del proprietario. Per failover di test, fai il ruolo, **spostare**e quindi seleziona **Seleziona nodo**. Nella finestra di dialogo **Spostare ruolo in cluster** , seleziona il nodo del cluster desiderato e quindi seleziona **OK**. Nella colonna del **Proprietario del nodo** , verifica che il nodo del proprietario è stato modificato.

## Creare un cluster di failover tramite Windows PowerShell

Il seguente cmdlet di Windows PowerShell eseguire le funzioni come le procedure descritte in precedenza in questo argomento. Immetti ogni cmdlet su una singola riga, anche se possano essere visualizzati a capo su più righe diversi a causa di vincoli di formattazione.

>[!NOTE]
>È necessario utilizzare Windows PowerShell per creare un cluster scollegato Active Directory in Windows Server 2012 R2. Per informazioni sulla sintassi, vedere [distribuire un Cluster Active Directory-Detached](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11)).

L'esempio seguente installa la funzionalità Clustering di Failover.

```PowerShell
Install-WindowsFeature –Name Failover-Clustering –IncludeManagementTools
```

L'esempio seguente viene eseguito tutti i test di convalida dei cluster nei computer che vengono denominati *Server1* e *Server2*.

```PowerShell
Test-Cluster –Node Server1, Server2
```

>[!NOTE]
>Il cmdlet **Test-Cluster** restituisce i risultati in un file di log nella directory di lavoro corrente. Ad esempio: < username > C:\Users\ \AppData\Local\Temp..

L'esempio seguente crea un cluster di failover che è denominato *miocluster* con nodi *Server1* e *Server2*, assegna il statiche di indirizzo IP *192.168.1.12*e aggiunge tutta l'archiviazione idoneo al cluster di failover.

```PowerShell
New-Cluster –Name MyCluster –Node Server1, Server2 –StaticAddress 192.168.1.12
```

L'esempio seguente crea lo stesso cluster di failover come nell'esempio precedente, ma non aggiunge archiviazione idoneo al cluster di failover.

```PowerShell
New-Cluster –Name MyCluster –Node Server1, Server2 –StaticAddress 192.168.1.12 -NoStorage
```

L'esempio seguente crea un cluster denominato *miocluster* del *Cluster* unità Organizzativa del dominio *Contoso.com dell'azienda*.

```PowerShell
New-Cluster -Name CN=MyCluster,OU=Cluster,DC=Contoso,DC=com -Node Server1, Server2
```

Per esempi su come aggiungere ruoli del cluster, Vedi gli argomenti, ad esempio [Add-ClusterFileServerRole](https://docs.microsoft.com/powershell/module/failoverclusters/add-clusterfileserverrole?view=win10-ps) e [Add-ClusterGenericApplicationRole](https://docs.microsoft.com/powershell/module/failoverclusters/add-clustergenericapplicationrole?view=win10-ps).

## Ulteriori informazioni

  - [Clustering di failover](failover-clustering.md)
  - [Distribuire un Cluster Hyper-V](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj863389(v%3dws.11)>)
  - [Scale-Out File Server per i dati dell'applicazione](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831349(v%3dws.11)>)
  - [Distribuire un Cluster di Active Directory-scollegata](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn265970(v=ws.11))
  - [Utilizzo del clustering guest per la disponibilità elevata](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/dn440540(v%3dws.11)>)
  - [Aggiornamento compatibile con cluster](cluster-aware-updating.md)
  - [Nuovo Cluster](https://docs.microsoft.com/powershell/module/failoverclusters/new-cluster?view=win10-ps)
  - [Test-Cluster](https://docs.microsoft.com/powershell/module/failoverclusters/test-cluster?view=win10-ps)
