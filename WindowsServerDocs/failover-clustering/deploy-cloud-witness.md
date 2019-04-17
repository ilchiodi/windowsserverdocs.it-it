---
ms.assetid: 0cd1ac70-532c-416d-9de6-6f920a300a45
title: Distribuire un cloud di controllo per un Cluster di Failover
ms.prod: windows-server-threshold
manager: eldenc
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.topic: article
author: JasonGerend
ms.date: 10/2/2017
description: Come usare Microsoft Azure per ospitare il controllo per un Cluster di Failover Windows Server nel cloud - aka come distribuire un Cloud di controllo.
ms.openlocfilehash: 564c6668fcc80a8bd1531c05c142996689de8154
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="deploy-a-cloud-witness-for-a-failover-cluster"></a>Distribuire un Cloud di controllo per un Cluster di Failover

> Si applica a: si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

Cloud di controllo è un nuovo tipo di quorum di controllo del Cluster di Failover introdotte in Windows Server 2016. In questo argomento viene fornita una panoramica della funzionalità Cloud di controllo, gli scenari che supporta e istruzioni su come configurare un cloud di controllo per un Cluster di Failover che esegue Windows Server 2016.

## <a name="CloudWitnessOverview"></a>Panoramica di controllo cloud

Figura 1 è illustrata una configurazione di quorum del Cluster di Failover estesa multisita con Windows Server 2016. In questo esempio di configurazione (figura 1), esistono 2 nodi in centri 2 dati (detto siti). Tieni presente, è possibile che un cluster si estendono su più di 2 centri dati. Inoltre, ogni Data Center può avere più di 2 nodi. Una configurazione di quorum del cluster tipico in questa configurazione (il failover automatico contratto di servizio) offre ogni nodo di un voto. Uno aggiuntivo voto viene assegnato il quorum di controllo per consentire al cluster mantenere in esecuzione anche se uno dei Data Center interruzione dell'alimentazione. I calcoli sono semplici: esistono 5 voti totale ed è necessario 3 voti per il cluster di mantenerla in esecuzione.  

![Controllo di condivisione file in un terzo separato del sito con 2 nodi 2 altri siti](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_1.png "Witness di condivisione File")  
**Figura 1: Uso di un controllo di condivisione File come un quorum di controllo**  

In caso di interruzione dell'alimentazione in un centro dati, per consentire a uguale per il cluster in altri Data Center di mantenerla in esecuzione, è consigliabile per ospitare il quorum di controllo in un percorso diverso da due Data Center. Questo in genere significa che richiedono una terza separa datacenter (sito) per ospitare un File Server che esegue il backup la condivisione di File che viene utilizzato come il quorum di controllo (controllo di condivisione File).  

La maggior parte delle organizzazioni non è necessario un terzo separare datacenter che ospiterà il controllo di condivisione File di backup di File Server. Questo significa che le organizzazioni ospitano principalmente il File Server in uno dei due Data Center, rendendo, di conseguenza, tale data center del Data Center primario. In uno scenario in cui è presente un'interruzione dell'alimentazione del Data Center primario, il cluster verrebbero verso il basso come altri Data Center avrebbe solo 2 voti sotto la maggioranza del quorum di 3 voti necessari. Per i clienti che hanno terzo datacenter separato per ospitare il File Server, è un sovraccarico di mantenere il File Server a disponibilità elevata il controllo di condivisione File di backup. Hosting di macchine virtuali nel cloud pubblico con il File Server per il controllo di condivisione File in esecuzione nel sistema operativo Guest è un sovraccarico significativo in termini sia di configurazione e manutenzione.  

Cloud di controllo è un nuovo tipo di Cluster di Failover quorum di controllo che si basa su Microsoft Azure come punto di arbitraggio (figura 2). Usa l'archiviazione Blob di Azure per un file blob che viene utilizzato come un punto di arbitraggio in caso di risoluzione "split Brain" lettura/scrittura.  

Esistono significativi vantaggi questo approccio:
1. Si avvale di Microsoft Azure (non è necessario per Data Center terzo separati).  
2. Usa standard archiviazione di Blob di Azure (non overhead aggiuntivo manutenzione delle macchine virtuali ospitate nel cloud pubblico) disponibili.  
3. Stesso Account di archiviazione di Azure può essere utilizzato per più cluster (file di un blob per ogni cluster; id univoco del cluster utilizzato come nome del file blob).  
4. Molto basso $cost in corso per l'Account di archiviazione (dati di dimensioni molto ridotte scritti per ogni file blob blob file aggiornato solo una volta quando viene modificato lo stato dei nodi del cluster).  
5. Tipo di risorsa Cloud di controllo predefinito.  

![Diagramma che illustra un cluster esteso multisito con Cloud di controllo come un quorum di controllo](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_2.png)  
**Figura 2: Multisito allungata cluster con Cloud di controllo come un quorum di controllo**  

Come illustrato nella figura 2, non esiste alcun sito separato terzo che è necessario. Cloud di controllo, come altri quorum di controllo, ottiene un voto e può partecipare nei calcoli del quorum.  

## <a name="CloudWitnessSupportedScenarios"></a>Cloud di controllo: Scenari supportati per il tipo di controllo singola
Se si dispone di una distribuzione di Cluster di Failover, in cui tutti i nodi possono accedere a internet (dall'estensione di Azure), è consigliabile configurare un controllo Cloud come risorsa quorum di controllo.  

Alcuni degli scenari supportati utilizzare del Cloud di controllo come un quorum di controllo sono i seguenti:  
- Il ripristino di emergenza estesa cluster multisito (vedi figura 2).  
- Cluster di failover senza archiviazione condivisa (SQL sempre su ecc.).  
- Cluster di failover in esecuzione all'interno del sistema operativo Guest ospitati nel ruolo macchina virtuale di Microsoft Azure (o qualsiasi altro cloud pubblico).  
- Cluster di failover in esecuzione all'interno del sistema operativo di macchine virtuali Guest ospitati in cloud privati.
- Archiviazione cluster con o senza archiviazione condivisa, ad esempio Scale-out File Server cluster.  
- Cluster di piccole succursali (anche 2 nodi cluster)  

A partire da Windows Server 2012 R2, è consigliabile configurare sempre un controllo, perché il cluster gestisce automaticamente il voto di controllo e i nodi voto con dinamica del Quorum.  

## <a name="CloudWitnessSetUp"></a>Configurare un controllo Cloud per un cluster
Per configurare un controllo Cloud come un quorum di controllo per il cluster, completare i passaggi seguenti:
1. Creare un Account di archiviazione di Azure da utilizzare come un controllo Cloud
2. Configurare il controllo Cloud come un quorum di controllo per il cluster.

## <a name="create-an-azure-storage-account-to-use-as-a-cloud-witness"></a>Creare un Account di archiviazione di Azure da utilizzare come un controllo Cloud

In questa sezione viene descritto come creare un endpoint di account e visualizzare e copiare archiviazione URL e chiavi per l'account di accesso.

Per configurare Cloud di controllo, è necessario disporre di un Account di archiviazione Azure valido che può essere utilizzato per archiviare il file blob (usato per l'arbitraggio). Cloud di controllo crea un contenitore noto **msft cloud di controllo** l'account di archiviazione di Microsoft. Cloud di controllo scritture cluster a un file singolo blob con corrispondente dell'ID univoco utilizzato come nome file del file blob in virtù del presente **msft cloud di controllo** contenitore. Ciò significa che è possibile utilizzare lo stesso Account di archiviazione Microsoft Azure per configurare un controllo Cloud per più cluster diversi.

Quando si utilizza lo stesso Account di archiviazione di Azure per la configurazione Cloud di controllo per più diverso, i cluster di una singola **msft cloud di controllo** contenitore viene creato automaticamente. Questo contenitore conterrà i file di un blob per ogni cluster.

### <a name="to-create-an-azure-storage-account"></a>Per creare un account di archiviazione di Azure

1. Accedere al [portale Azure](http://portal.azure.com).
2. Nel menu Hub, scegli Nuovo -> dati + archiviazione -> account di archiviazione.
3. Nella creazione di una pagina account di archiviazione, eseguire le operazioni seguenti:
    1. Immettere un nome per il tuo account di archiviazione.
    <br>I nomi degli account di archiviazione deve essere compreso tra 3 e 24 caratteri e può contenere numeri e lettere minuscole solo. Inoltre, il nome di account di archiviazione deve essere univoco all'interno di Azure.
        
    2. Per **Account kind**selezionare **generali**.
    <br>È possibile utilizzare un account di archiviazione Blob per un controllo Cloud.
    3. Per **prestazioni**selezionare **Standard**.
    <br>È possibile utilizzare l'archiviazione Premium di Azure per un controllo Cloud.
    2. Per **replica**selezionare **archiviazione ridondanti in locale (LRS)**.
    <br>Clustering di failover, viene utilizzato il file blob come punto di arbitraggio, che richiede alcune garanzie di coerenza durante la lettura dei dati. È necessario selezionare **archiviazione ridondanti localmente** per **replica** tipo.

### <a name="view-and-copy-storage-access-keys-for-your-azure-storage-account"></a>Visualizzare e copiare i tasti di archiviazione per il tuo Account di archiviazione di Azure

Quando si crea un Account di archiviazione di Microsoft Azure, questo viene associato con due chiavi di accesso che vengono generati automaticamente - tasto primario e secondario tasto di scelta. Per la creazione di una prima del Cloud di controllo, utilizzare il **tasto primario**. Non esiste alcuna restrizione riguardanti la chiave da utilizzare per Cloud di controllo.  

#### <a name="to-view-and-copy-storage-access-keys"></a>Per visualizzare e copiare i tasti di archiviazione

Nel portale di Azure, passare al tuo account di archiviazione, fare clic su **tutte le impostazioni** e quindi fare clic su **i tasti di scelta** per visualizzare, copiare e rigenerare le chiavi di accesso account. Le blade tasti include anche le stringhe di connessione preconfigurata con le chiavi principali e secondarie che è possibile copiare per l'utilizzo delle applicazioni (vedere la figura 4).

![Snapshot della finestra di dialogo di gestire i tasti di scelta in Microsoft Azure](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_4.png)  
**Figura 4: Archiviazione i tasti di scelta**

### <a name="view-and-copy-endpoint-url-links"></a>Visualizzare e copiare i link URL endpoint  
Quando si crea un Account di archiviazione, gli URL seguenti vengono generati utilizzando il formato: `https://<Storage Account Name>.<Storage Type>.<Endpoint>`  

Cloud di controllo Usa sempre **Blob** come tipo di archiviazione. Usa Azure **. core.windows.net** come Endpoint. Quando si configurano Cloud di controllo, è possibile che si configurarla con un altro endpoint in base al proprio scenario (ad esempio il Data Center di Microsoft Azure in Cina ha un altro endpoint).  

> [!NOTE]  
> Viene generato automaticamente l'URL dell'endpoint dalla risorsa Cloud di controllo e non esiste alcun passaggio aggiuntivo di configurazione necessarie per l'URL.  

#### <a name="to-view-and-copy-endpoint-url-links"></a>Per visualizzare e copiare l'URL dell'endpoint collegamenti
Nel portale di Azure, passare al tuo account di archiviazione, fare clic su **tutte le impostazioni** e quindi fare clic su **proprietà** per visualizzare e copiare l'URL dell'endpoint (vedere la figura 5).  

![Snapshot dei collegamenti endpoint Cloud di controllo](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_5.png)  
**Figura 5: Collegamenti a URL endpoint Cloud di controllo**

Per ulteriori informazioni sulla creazione e gestione degli account di archiviazione di Azure, vedere [informazioni sugli account di archiviazione di Azure](https://azure.microsoft.com/documentation/articles/storage-create-storage-account/)

## <a name="configure-cloud-witness-as-a-quorum-witness-for-your-cluster"></a>Configurare Cloud di controllo come un quorum di controllo per il cluster
Configurazione del controllo cloud è ben integrata all'interno della configurazione Quorum esistente guidata integrata in Gestione Cluster di Failover.  

### <a name="to-configure-cloud-witness-as-a-quorum-witness"></a>Per configurare Cloud di controllo come un Quorum di controllo
1. Avviare Gestione Cluster di Failover.
2. Pulsante destro del mouse il cluster -> **altre azioni** -> **Configura impostazioni quorum del Cluster** (vedere la figura 6). Verrà avviata la procedura guidata configurazione quorum del Cluster.  
    ![Snapshot del percorso menu impostazioni del Quorum del Cluster Configue in UI la gestione Cluster di Failover](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_7.png)**figura 6. Impostazioni quorum del cluster**

3. Nel **Selezionare configurazioni Quorum** selezionare **selezionare il quorum di controllo** (vedere la figura 7).  

    ![Pulsante della procedura guidata quorum del Cluster di snapshot di 'selezionare il controllo quotrum' opzione](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_8.png)  
    **Figura 7. Selezionare la configurazione Quorum**

4. Nel **selezione Quorum di controllo** selezionare **configurare un controllo cloud** (vedi figura 8).  

    ![Snapshot del pulsante di opzione appropriata per selezionare un controllo cloud](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_9.png)  
    **Figura 8. Selezionare il Quorum di controllo**  

5. Nel **configurare Cloud di controllo** pagina, immettere le informazioni seguenti:  
    1. (Parametro obbligatorio) Nome Account di archiviazione di Azure.  
    2. (Parametro obbligatorio) Tasto corrispondente all'Account di archiviazione.  
        1. Quando si crea per la prima volta, Usa tasto primario (vedere la figura 5)  
        2. Quando la rotazione il tasto primario, utilizzare tasto secondario (vedere la figura 5)  
    3. (Parametro facoltativo) Se si prevede di utilizzare un endpoint servizio Azure diverso (ad esempio il servizio Microsoft Azure in Cina), quindi aggiornare il nome del server endpoint.  

    ![Snapshot del riquadro di configurazione Cloud di controllo della procedura guidata quorum del Cluster](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_10.png)  
    **Figura 9: Configurare il Cloud di controllo**

6. Dopo la corretta configurazione di Cloud di controllo, è possibile visualizzare la risorsa di riferimento appena creato in Gestione Cluster di Failover snap-in (vedi figura 10).

    ![La corretta configurazione di Cloud di controllo](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_11.png)  
    **Figura 10: La corretta configurazione di Cloud di controllo**

### <a name="configuring-cloud-witness-using-powershell"></a>Configurazione di controllo Cloud tramite PowerShell  
Il comando di PowerShell Set-ClusterQuorum esistente ha nuovi parametri aggiuntivi corrispondenti ai Cloud di controllo.  

È possibile configurare Cloud di controllo mediante il [`Set-ClusterQuorum`](https://technet.microsoft.com/library/ee461013.aspx)comando PowerShell seguente:  

```PowerShell
Set-ClusterQuorum -CloudWitness -AccountName <StorageAccountName> -AccessKey <StorageAccountAccessKey>
```

Nel caso in cui è necessario utilizzare un altro endpoint (raro):  

```PowerShell
Set-ClusterQuorum -CloudWitness -AccountName <StorageAccountName> -AccessKey <StorageAccountAccessKey> -Endpoint <servername>  
```

### <a name="azure-storage-account-considerations-with-cloud-witness"></a>Azure considerazioni sull'Account di archiviazione con Cloud di controllo  
Quando si configura un controllo Cloud come un quorum di controllo per il Cluster di Failover, considerare quanto segue:
* Non archiviare la chiave di accesso, il Cluster di Failover genererà e archiviare in modo sicuro un token di sicurezza di accesso condiviso (SAS).  
* Il token di associazioni di sicurezza generato è valido, purché il tasto di scelta rimane valido. Quando la rotazione il tasto primario, è importante aggiornare prima il Cloud di controllo (su tutti i cluster che utilizzano Account di archiviazione) con la chiave di accesso secondario prima di rigenerare il tasto primario.  
* Cloud di controllo Usa interfaccia REST HTTPS del servizio di Account di archiviazione di Azure. Ciò significa che richiede la porta HTTPS sia aperta sul tutti i nodi del cluster.

### <a name="proxy-considerations-with-cloud-witness"></a>Considerazioni sui proxy con Cloud di controllo  
Cloud di controllo Usa HTTPS (porta predefinita 443) per stabilire la comunicazione con il servizio blob di Azure. Assicurarsi che la porta HTTPS sia accessibile tramite rete Proxy.

## <a name="see-also"></a>Vedere anche
- [What's New in Failover Clustering in Windows Server](whats-new-in-failover-clustering.md)
