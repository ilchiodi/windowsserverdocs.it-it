---
ms.assetid: 0cd1ac70-532c-416d-9de6-6f920a300a45
title: Distribuire un cloud di controllo per un Cluster di Failover
ms.prod: windows-server-threshold
manager: eldenc
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.topic: article
author: JasonGerend
ms.date: 01/18/2019
description: Come usare Microsoft Azure per ospitare il controllo del mirroring per un Cluster di Failover Windows Server nel cloud, noto anche come come distribuire un Cloud di controllo.
ms.openlocfilehash: f7e1c84e54f08044a772f06e591588c1add33026
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857982"
---
# <a name="deploy-a-cloud-witness-for-a-failover-cluster"></a>Distribuire un cloud di controllo per un cluster di failover

> Si applica a: Windows Server 2019, Windows Server 2016, Windows Server (canale semestrale)

Cloud di controllo è un tipo di Cluster di Failover quorum di controllo che usa Microsoft Azure per fornire un voto sul quorum del cluster. In questo argomento viene fornita una panoramica della funzionalità Cloud di controllo, gli scenari supportati e le istruzioni su come configurare un cloud di controllo per un Cluster di Failover.

## <a name="CloudWitnessOverview"></a>Panoramica del cloud di controllo

Figura 1 illustra una configurazione del quorum del Cluster di Failover con estensione su più siti con Windows Server 2016. In questo esempio di configurazione (figura 1), esistono 2 nodi nei data 2 Center (detto siti). Si noti che è possibile che un cluster per estendersi su più di 2 Data Center. Inoltre, ogni Data Center può avere più di 2 nodi. Una configurazione di quorum del cluster tipico in questa configurazione (il failover automatico Service Level AGREEMENT) offre ogni nodo di un voto. Voto aggiuntiva è specificato per il quorum di controllo per consentire di mantenere in esecuzione anche se entrambi quello del Data Center del cluster si verifichi un'interruzione dell'alimentazione. La matematica è semplice: sono disponibili 5 voti totali ed è necessario 3 voti per il cluster garantire l'esecuzione.  

![Controllo di condivisione file in una terza il sito con 2 nodi 2 altri siti](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_1.png "Witness di condivisione File")  
**Figura 1: Utilizzo di un controllo di condivisione File come un quorum di controllo**  

In caso di interruzione dell'alimentazione in un Data Center, per consentire a uguale per il cluster in altri Data Center per garantire l'esecuzione, è consigliabile ospitare il quorum di controllo in una posizione diversa da due Data Center. Ciò significa che richiedono una terza separano datacenter (sito) per ospitare un File Server che esegue il backup di condivisione di File che viene usato come controllo del quorum (testimone di condivisione File).  

La maggior parte delle organizzazioni non è un terzo separare i Data Center che ospiterà il controllo di condivisione File di backup di File Server. Ciò significa che le organizzazioni ospitano principalmente il File Server in uno dei due Data Center, rendendo di conseguenza, Data Center in questione nel Data Center primario. In uno scenario in cui è presente un'interruzione dell'alimentazione nel Data Center primario, il cluster andrebbe verso il basso come altri Data Center sono solo 2 voti collocata sotto la maggioranza del quorum di 3 voti necessari. Per i clienti che hanno terzo separate del Data Center per ospitare il File Server, è un sovraccarico per mantenere il File Server a disponibilità elevata di controllo di condivisione File di backup. Hosting di macchine virtuali nel cloud pubblico con il File Server per la condivisione File di controllo in esecuzione nel sistema operativo Guest è un significativo sovraccarico in termini di configurazione e manutenzione.  

Cloud di controllo è un nuovo tipo di Cluster di Failover quorum di controllo che si avvale di Microsoft Azure come punto di arbitraggio (figura 2). Usa archiviazione Blob di Azure per leggere e scrivere un file di blob che viene quindi usato come punto di arbitraggio in caso di risoluzione "split Brain".  

Vi sono significativi vantaggi che questo approccio:
1. Si avvale di Microsoft Azure (senza bisogno di terzo separate del Data Center).  
2. Usa standard disponibile Azure Blob Storage (nessun overhead di manutenzione aggiuntive di macchine virtuali ospitate nel cloud pubblico).  
3. Stesso Account di archiviazione di Azure può essere utilizzato per più cluster (file di un blob per ogni cluster; cluster id univoco usato come nome del file blob).  
4. Molto bassa $cost in corso per l'Account di archiviazione (dati di dimensioni molto ridotte scritti per ogni file di blob, file di blob aggiornato solo una volta quando viene modificato lo stato dei nodi del cluster).  
5. Tipo di risorse Cloud di controllo incorporato.  

![Diagramma che illustra un cluster esteso a più siti con Cloud di controllo come un quorum di controllo](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_2.png)  
**Figura 2: Cluster estesi su più siti con Cloud di controllo come un quorum di controllo**  

Come illustrato nella figura 2, non vi è alcun terzo sito separato che è necessario. Cloud di controllo, come altri quorum di controllo, ottiene un voto e può partecipare nei calcoli del quorum.  

## <a name="CloudWitnessSupportedScenarios"></a>Cloud di controllo: Scenari supportati per il tipo di singolo server di controllo
Se si dispone di una distribuzione di Cluster di Failover, in cui tutti i nodi possono raggiungere internet (dall'estensione di Azure), è consigliabile configurare un Cloud di controllo come risorsa di controllo del quorum.  

Alcuni degli scenari supportati usare del Cloud di controllo come un quorum di controllo sono i seguenti:  
- Ripristino di emergenza esteso i cluster multisito (vedere la figura 2).  
- Cluster di failover senza archiviazione condivisa (SQL sempre su ecc.).  
- Cluster di failover in esecuzione all'interno del sistema operativo Guest ospitati nel ruolo di macchina virtuale di Microsoft Azure (o qualsiasi altro cloud pubblico).  
- Cluster di failover in esecuzione all'interno del sistema operativo di macchine virtuali Guest ospitati in cloud privati.
- Archiviazione cluster con o senza archiviazione condivisa, ad esempio Scale-out File Server cluster.  
- Cluster di piccole succursali (anche 2 nodi cluster)  

A partire da Windows Server 2012 R2, è consigliabile configurare sempre il controllo come il cluster gestisce automaticamente il voto di controllo e votano i nodi con dinamica del Quorum.  

## <a name="CloudWitnessSetUp"></a> Configurare un Cloud di controllo per un cluster
Per configurare un Cloud di controllo come un quorum di controllo per il cluster, completare i passaggi seguenti:
1. Creare un Account di archiviazione di Azure da usare come un Cloud di controllo
2. Configurare il Cloud di controllo come un quorum di controllo per il cluster.

## <a name="create-an-azure-storage-account-to-use-as-a-cloud-witness"></a>Creare un Account di archiviazione di Azure da usare come un Cloud di controllo

Questa sezione descrive come creare un endpoint di archiviazione account e visualizzare e copiare l'URL e le chiavi per l'account di accesso.

Per configurare i Cloud di controllo, è necessario disporre di un Account di archiviazione di Azure valido che può essere usato per archiviare il file blob (usato per l'arbitraggio). Cloud di controllo consente di creare un contenitore noto **msft-cloud-witness** con l'Account di archiviazione di Microsoft. Cloud Witness scritture del cluster un solo file blob con i corrispondenti dell'ID univoco usato come nome del file del file del blob sotto ciò **msft-cloud-witness** contenitore. Ciò significa che è possibile utilizzare lo stesso Account di archiviazione di Microsoft Azure per configurare un Cloud di controllo per più cluster diversi.

Quando si usa lo stesso Account di archiviazione di Azure per la configurazione Cloud di controllo per più diverse di cluster, un singolo **msft-cloud-witness** contenitore viene creato automaticamente. Questo contenitore conterrà-blob in un file per ogni cluster.

### <a name="to-create-an-azure-storage-account"></a>Per creare un account di archiviazione di Azure

1. Accedi per il [portale di Azure](http://portal.azure.com).
2. Nel menu Hub selezionare Nuovo -> Data + archiviazione -> account di archiviazione.
3. Nella creazione di una pagina di account di archiviazione, eseguire le operazioni seguenti:
    1. Immettere un nome per l'account di archiviazione.
    <br>I nomi degli account di archiviazione devono avere una lunghezza compresa tra 3 e 24 caratteri e possono contenere solo numeri e lettere minuscole. Inoltre, il nome di account di archiviazione deve essere univoco all'interno di Azure.
        
    2. Per la **tipologia Account**, selezionare **generico**.
    <br>È possibile usare un account di archiviazione Blob per un Cloud di controllo.
    3. Per la **Performance**, selezionare **Standard**.
    <br>È possibile usare archiviazione Premium di Azure per un Cloud di controllo.
    2. Per la **replica**, selezionare **l'archiviazione con ridondanza locale (LRS)** .
    <br>Clustering di failover Usa il file di blob come punto di arbitraggio, che richiede alcune garanzie di coerenza durante la lettura dei dati. È pertanto necessario selezionare **archiviazione con ridondanza locale** per **replica** tipo.

### <a name="view-and-copy-storage-access-keys-for-your-azure-storage-account"></a>Visualizzare e copiare le chiavi di accesso di archiviazione per l'Account di archiviazione di Azure

Quando si crea un Account di archiviazione di Microsoft Azure, è associato a due chiavi di accesso che vengono generati automaticamente - chiave di accesso primaria e chiave di accesso secondaria. Per la creazione di un primo del Cloud di controllo, usare il **chiave di accesso primaria**. Non vi è alcuna restrizione riguardanti la chiave da usare per Cloud di controllo.  

#### <a name="to-view-and-copy-storage-access-keys"></a>Per visualizzare e copiare le chiavi di accesso di archiviazione

Nel portale di Azure, passare all'account di archiviazione, fare clic su **tutte le impostazioni** e quindi fare clic su **chiavi di accesso** per visualizzare, copiare e rigenerare le chiavi di accesso. Il pannello chiavi di accesso include anche le stringhe di connessione preconfigurate usando le chiavi primaria e secondarie che è possibile copiare per usarle nelle applicazioni (vedere la figura 4).

![Snapshot della finestra di dialogo Gestisci chiavi di accesso di Microsoft Azure](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_4.png)  
**Figura 4: Chiavi di accesso di archiviazione**

### <a name="view-and-copy-endpoint-url-links"></a>Visualizzare e copiare i collegamenti URL endpoint  
Quando si crea un Account di archiviazione, vengono generati gli URL seguenti usando il formato: `https://<Storage Account Name>.<Storage Type>.<Endpoint>`  

Usa sempre cloud Witness **Blob** come tipo di archiviazione. Azure Usa **. core.windows.net** come Endpoint. Quando si configura Cloud di controllo, è possibile che si configurarlo con un endpoint diverso in base al proprio scenario (ad esempio Data Center di Microsoft Azure in Cina ha un endpoint diverso).  

> [!NOTE]  
> L'URL dell'endpoint viene generato automaticamente da risorsa del Cloud di controllo e non è necessaria per l'URL di alcun passaggio aggiuntivo della configurazione.  

#### <a name="to-view-and-copy-endpoint-url-links"></a>Visualizzare e copiare l'URL dell'endpoint di collegamenti
Nel portale di Azure, passare all'account di archiviazione, fare clic su **tutte le impostazioni** e quindi fare clic su **proprietà** per visualizzare e copiare l'URL dell'endpoint (vedere la figura 5).  

![Snapshot dei collegamenti dell'endpoint Cloud di controllo](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_5.png)  
**Figura 5: Cloud collegamenti URL endpoint di controllo del mirroring**

Per altre informazioni sulla creazione e la gestione degli account di archiviazione di Azure, vedere [informazioni sugli account di archiviazione di Azure](https://azure.microsoft.com/documentation/articles/storage-create-storage-account/)

## <a name="configure-cloud-witness-as-a-quorum-witness-for-your-cluster"></a>Configurare i Cloud di controllo come un quorum di controllo per il cluster
Configurazione del controllo cloud è perfettamente integrata interno della procedura guidata configurazione di Quorum esistenti compilati in Gestione Cluster di Failover.  

### <a name="to-configure-cloud-witness-as-a-quorum-witness"></a>Per configurare i Cloud di controllo come un Quorum di controllo
1. Avviare Gestione Cluster di Failover.
2. Pulsante destro del mouse nel cluster -> **altre azioni** -> **Configura impostazioni quorum del Cluster** (vedere la figura 6). Verrà avviata la procedura guidata configurare Quorum di Cluster.  
    ![Snapshot del percorso menu Configura impostazioni Quorum del Cluster nella UI di gestione Cluster di Failover](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_7.png) **figura 6. Impostazioni quorum del cluster**

3. Nel **selezionare le configurazioni del Quorum** pagina, selezionare **seleziona il quorum di controllo** (vedere la figura 7).  

    ![Snapshot di 'Seleziona il controllo quotrum' RadioButton nella procedura guidata quorum del Cluster](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_8.png)  
    **Figura 7. Selezionare la configurazione del Quorum**

4. Nel **selezione Quorum di controllo** pagina, selezionare **configurazione di un controllo cloud** (vedere la figura 8).  

    ![Snapshot del pulsante di opzione appropriato per selezionare un cloud di controllo](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_9.png)  
    **Figura 8. Seleziona il Quorum di controllo**  

5. Nel **configurazione Cloud Witness** pagina, immettere le informazioni seguenti:  
    1. (Parametro obbligatorio) Nome Account di archiviazione di Azure.  
    2. (Parametro obbligatorio) Chiave di accesso corrispondente all'Account di archiviazione.  
        1. Quando si crea per la prima volta, usare una chiave di accesso primaria (vedere la figura 5)  
        2. Ruotare la chiave di accesso primaria, usare una chiave di accesso secondaria (vedere la figura 5)  
    3. (Parametro facoltativo) Se si prevede di usare un endpoint di servizio di Azure diverso (ad esempio il servizio di Microsoft Azure in Cina), quindi aggiornare il nome dell'endpoint server.  

    ![Snapshot del riquadro di configurazione del Cloud di controllo della procedura guidata quorum del Cluster](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_10.png)  
    **Figura 9: Configurare il controllo di Cloud**

6. Dopo la corretta configurazione di Cloud di controllo, è possibile visualizzare la risorsa di controllo del mirroring appena creata in Gestione Cluster di Failover snap-in (vedere la figura 10).

    ![La corretta configurazione di Cloud di controllo](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_11.png)  
    **Figura 10: La corretta configurazione di Cloud di controllo**

### <a name="configuring-cloud-witness-using-powershell"></a>Configurazione di Cloud di controllo usando PowerShell  
Il comando di PowerShell Set-ClusterQuorum esistente con nuovi parametri aggiuntivi corrispondenti a Cloud di controllo.  

È possibile configurare Cloud di controllo utilizzando il [ `Set-ClusterQuorum` ](https://technet.microsoft.com/library/ee461013.aspx) il comando PowerShell seguente:  

```PowerShell
Set-ClusterQuorum -CloudWitness -AccountName <StorageAccountName> -AccessKey <StorageAccountAccessKey>
```

Nel caso in cui è necessario usare un endpoint diverso (raro):  

```PowerShell
Set-ClusterQuorum -CloudWitness -AccountName <StorageAccountName> -AccessKey <StorageAccountAccessKey> -Endpoint <servername>  
```

### <a name="azure-storage-account-considerations-with-cloud-witness"></a>Considerazioni di Account di archiviazione di Azure con Cloud di controllo  
Quando si configura un Cloud di controllo come un quorum di controllo per il Cluster di Failover, considerare quanto segue:
* Invece di archiviare la chiave di accesso, il Cluster di Failover viene generato e archiviato in modo sicuro un token di sicurezza di accesso condiviso (SAS).  
* Il token di firma di accesso condiviso generato è valido fino a quando la chiave di accesso rimane valida. Ruotare la chiave di accesso primaria, è importante innanzitutto aggiornare il server di controllo Cloud (in tutti i cluster che usano l'Account di archiviazione) con la chiave di accesso secondaria prima di rigenerare la chiave di accesso primaria.  
* Cloud di controllo utilizza l'interfaccia REST HTTPS del servizio di Account di archiviazione di Azure. Pertanto che è necessaria la porta HTTPS sia aperta in tutti i nodi del cluster.

### <a name="proxy-considerations-with-cloud-witness"></a>Considerazioni di proxy con il Cloud di controllo  
Controllo cloud usa HTTPS (porta predefinita 443) per stabilire la comunicazione con il servizio blob di Azure. Assicurarsi che la porta HTTPS sia accessibile tramite rete Proxy.

## <a name="see-also"></a>Vedere anche
- [What ' s New in Failover Clustering in Windows Server](whats-new-in-failover-clustering.md)
