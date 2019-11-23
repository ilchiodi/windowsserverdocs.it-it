---
ms.assetid: 0cd1ac70-532c-416d-9de6-6f920a300a45
title: Distribuire un cloud di controllo per un cluster di failover
ms.prod: windows-server
manager: eldenc
ms.author: jgerend
ms.technology: storage-failover-clustering
ms.topic: article
author: JasonGerend
ms.date: 01/18/2019
description: Come usare Microsoft Azure per ospitare il server di controllo del mirroring per un cluster di failover di Windows Server nel cloud, ovvero come distribuire un cloud di controllo.
ms.openlocfilehash: 1f38a1a436cfced8637b743817dc1b3d150f7fa6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369877"
---
# <a name="deploy-a-cloud-witness-for-a-failover-cluster"></a>Distribuire un cloud di controllo per un cluster di failover

> Si applica a: Windows Server 2019, Windows Server 2016

Cloud Witness è un tipo di quorum di controllo del cluster di failover che usa Microsoft Azure per fornire un voto sul quorum del cluster. Questo argomento fornisce una panoramica della funzionalità di controllo del cloud, degli scenari supportati e delle istruzioni per la configurazione di un cloud di controllo per un cluster di failover.

## <a name="CloudWitnessOverview"></a>Panoramica di cloud Witness

Nella figura 1 viene illustrata una configurazione del quorum del cluster di failover esteso multisito con Windows Server 2016. In questa configurazione di esempio (Figura 1) sono presenti 2 nodi in 2 Data Center (detti siti). Si noti che è possibile che un cluster si estenda su più di 2 Data Center. Ogni data center può inoltre avere più di due nodi. Una tipica configurazione del quorum del cluster in questa configurazione (SLA di failover automatico) assegna a ogni nodo un voto. Un ulteriore voto viene assegnato al quorum di controllo per consentire l'esecuzione del cluster anche se uno dei data center riscontra un'interruzione dell'alimentazione. La matematica è semplice: sono disponibili 5 voti totali ed è necessario disporre di 3 voti per il cluster per mantenerne l'esecuzione.  

![Condivisione file di controllo in un terzo sito separato con 2 nodi in 2 altri siti](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_1.png "condivisione file") di controllo  
**Figura 1: uso di una condivisione file di controllo come quorum di controllo**  

In caso di interruzione dell'alimentazione in un Data Center, per fornire la stessa opportunità per il cluster in un altro Data Center per mantenerla in esecuzione, è consigliabile ospitare il quorum di controllo in una posizione diversa dai due data center. Ciò significa in genere che richiede un terzo data center separato (sito) per ospitare un file server che sta eseguendo il backup della condivisione file usata come quorum di controllo (condivisione file di controllo).  

Per la maggior parte delle organizzazioni non è presente un terzo data center separato che ospiterà il file server che supporta il controllo di condivisione file. Ciò significa che le organizzazioni ospitano principalmente il file server in uno dei due Data Center, che per estensione rende il data center primario. In uno scenario in cui si verifica un'interruzione dell'alimentazione nel data center principale, il cluster diminuisce perché l'altro Data Center avrà solo 2 voti, che è inferiore alla maggioranza del quorum di 3 voti necessari. Per i clienti che dispongono di un terzo data center separato per ospitare il file server, si tratta di un sovraccarico per gestire il file server a disponibilità elevata che supporta il controllo della condivisione file. L'hosting di macchine virtuali nel cloud pubblico che dispongono del file server per il controllo di condivisione file in esecuzione nel sistema operativo guest è un sovraccarico significativo in termini di installazione & manutenzione.  

Cloud Witness è un nuovo tipo di quorum di controllo del cluster di failover che si avvale Microsoft Azure come punto di arbitraggio (Figura 2). Usa l'archivio BLOB di Azure per leggere/scrivere un file BLOB che viene quindi usato come punto di arbitraggio in caso di risoluzione di Split Brain.  

Questo approccio presenta vantaggi significativi:
1. Sfrutta Microsoft Azure (non è necessario un terzo data center separato).  
2. Usa l'archivio BLOB di Azure disponibile standard (nessun overhead di manutenzione aggiuntivo per le macchine virtuali ospitate nel cloud pubblico).  
3. Lo stesso account di archiviazione di Azure può essere usato per più cluster (un file BLOB per cluster; ID univoco del cluster usato come nome del file BLOB).  
4. $Cost in corso molto basso per l'account di archiviazione (dati molto piccoli scritti per ogni file BLOB, il file BLOB è stato aggiornato una sola volta quando viene modificato lo stato dei nodi del cluster).  
5. Tipo di risorsa del server di controllo del cloud incorporato.  

![diagramma che illustra un cluster con estensione multisito con il server di controllo del mirroring come quorum di controllo](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_2.png)  
**Figura 2: cluster con estensione multisito con il controllo del cloud come server di controllo del quorum**  

Come illustrato nella figura 2, non è disponibile un terzo sito separato obbligatorio. Il mirroring del cloud, come qualsiasi altro quorum di controllo, ottiene un voto e può partecipare ai calcoli del quorum.  

## <a name="CloudWitnessSupportedScenarios"></a>Cloud Witness: scenari supportati per il tipo di server di controllo singolo
Se si dispone di una distribuzione di cluster di failover, in cui tutti i nodi possono raggiungere Internet (per estensione di Azure), è consigliabile configurare un cloud di controllo come risorsa di controllo del quorum.  

Di seguito sono riportati alcuni degli scenari supportati dall'uso di cloud Witness come server di controllo del quorum:  
- Cluster multisito con estensione per il ripristino di emergenza (vedere la figura 2).  
- Cluster di failover senza archiviazione condivisa (SQL Always On e così via).  
- Cluster di failover in esecuzione all'interno del sistema operativo guest ospitato in Microsoft Azure ruolo macchina virtuale (o qualsiasi altro cloud pubblico).  
- Cluster di failover in esecuzione all'interno del sistema operativo guest di macchine virtuali ospitate in cloud privati.
- Cluster di archiviazione con o senza archiviazione condivisa, ad esempio cluster di file server di scalabilità orizzontale.  
- Cluster di piccole Branch-Office (anche cluster a 2 nodi)  

A partire da Windows Server 2012 R2, è consigliabile configurare sempre un server di controllo del mirroring poiché il cluster gestisce automaticamente il voto del server di controllo del mirroring e i nodi votano con il quorum dinamico.  

## <a name="CloudWitnessSetUp"></a>Configurare un cloud di controllo per un cluster
Per configurare un server di controllo del mirroring come quorum di controllo per il cluster, seguire questa procedura:
1. Creare un account di archiviazione di Azure da usare come server di controllo del cloud
2. Configurare il server di controllo del mirroring come quorum di controllo per il cluster.

## <a name="create-an-azure-storage-account-to-use-as-a-cloud-witness"></a>Creare un account di archiviazione di Azure da usare come server di controllo del cloud

Questa sezione descrive come creare un account di archiviazione e visualizzare e copiare gli URL e le chiavi di accesso dell'endpoint per tale account.

Per configurare cloud Witness, è necessario avere un account di archiviazione di Azure valido che può essere usato per archiviare il file BLOB (usato per l'arbitraggio). Cloud Witness crea un contenitore conosciuto **MSFT-cloud-Witness** con l'account di archiviazione Microsoft. Cloud Witness scrive un singolo file BLOB con l'ID univoco del cluster corrispondente usato come nome file del file BLOB in questo contenitore **MSFT-cloud-Witness** . Ciò significa che è possibile usare lo stesso account di Archiviazione di Microsoft Azure per configurare un cloud di controllo per più cluster diversi.

Quando si usa lo stesso account di archiviazione di Azure per la configurazione di cloud Witness per più cluster diversi, viene creato automaticamente un singolo contenitore **MSFT-cloud-Witness** . Questo contenitore conterrà un file BLOB per cluster.

### <a name="to-create-an-azure-storage-account"></a>Per creare un account di archiviazione di Azure

1. Accedere al portale di [Azure](http://portal.azure.com).
2. Nel menu hub selezionare Nuovo-> dati e archiviazione-> account di archiviazione.
3. Nella pagina Crea un account di archiviazione eseguire le operazioni seguenti:
    1. Immettere un nome per l'account di archiviazione.
    <br>I nomi degli account di archiviazione devono avere una lunghezza compresa tra 3 e 24 caratteri e possono contenere solo numeri e lettere minuscole. Il nome dell'account di archiviazione deve essere univoco anche in Azure.
        
    2. Per **tipologia account**selezionare **utilizzo generico**.
    <br>Non è possibile usare un account di archiviazione BLOB per un cloud di controllo.
    3. Per **prestazioni**, selezionare **standard**.
    <br>Non è possibile usare archiviazione Premium di Azure per un cloud di controllo.
    2. Per la **replica**, selezionare **archiviazione con ridondanza locale (con ridondanza locale)** .
    <br>Il clustering di failover usa il file BLOB come punto di arbitraggio, che richiede alcune garanzie di coerenza durante la lettura dei dati. Pertanto, è necessario selezionare l' **archiviazione con ridondanza locale** per il tipo di **replica** .

### <a name="view-and-copy-storage-access-keys-for-your-azure-storage-account"></a>Visualizzare e copiare le chiavi di accesso alle archiviazione per l'account di archiviazione di Azure

Quando si crea un account Archiviazione di Microsoft Azure, questo viene associato a due chiavi di accesso generate automaticamente: chiave di accesso primaria e chiave di accesso secondaria. Per la prima creazione di cloud Witness, usare la chiave di **accesso primaria**. Non esiste alcuna restrizione relativa alla chiave da usare per il cloud di controllo.  

#### <a name="to-view-and-copy-storage-access-keys"></a>Per visualizzare e copiare le chiavi di accesso alle archiviazione

Nel portale di Azure passare all'account di archiviazione, fare clic su **tutte le impostazioni** e quindi su **chiavi di accesso** per visualizzare, copiare e rigenerare le chiavi di accesso dell'account. Il pannello chiavi di accesso include anche le stringhe di connessione preconfigurate usando le chiavi primarie e secondarie che è possibile copiare per usare nelle applicazioni (vedere la figura 4).

![snapshot della finestra di dialogo Gestisci chiavi di accesso in Microsoft Azure](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_4.png)  
**Figura 4: chiavi di accesso alle archiviazione**

### <a name="view-and-copy-endpoint-url-links"></a>Visualizzare e copiare i collegamenti all'URL dell'endpoint  
Quando si crea un account di archiviazione, gli URL seguenti vengono generati usando il formato: `https://<Storage Account Name>.<Storage Type>.<Endpoint>`  

Cloud Witness usa sempre il **BLOB** come tipo di archiviazione. Azure usa **. Core.Windows.NET** come endpoint. Quando si configura il server di controllo del cloud, è possibile configurarlo con un endpoint diverso in base allo scenario (ad esempio, il Microsoft Azure Data Center in Cina ha un endpoint diverso).  

> [!NOTE]  
> L'URL dell'endpoint viene generato automaticamente dalla risorsa di controllo cloud e non è necessario alcun passaggio aggiuntivo di configurazione per l'URL.  

#### <a name="to-view-and-copy-endpoint-url-links"></a>Per visualizzare e copiare i collegamenti dell'URL dell'endpoint
Nel portale di Azure passare all'account di archiviazione, fare clic su **tutte le impostazioni** e quindi fare clic su **Proprietà** per visualizzare e copiare gli URL dell'endpoint (vedere la figura 5).  

![snapshot dei collegamenti dell'endpoint del server di controllo del cloud](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_5.png)  
**Figura 5: collegamenti all'URL dell'endpoint di controllo cloud**

Per altre informazioni sulla creazione e la gestione degli account di archiviazione di Azure, vedere [informazioni sugli account di archiviazione di Azure](https://azure.microsoft.com/documentation/articles/storage-create-storage-account/)

## <a name="configure-cloud-witness-as-a-quorum-witness-for-your-cluster"></a>Configurare il server di controllo del mirroring come quorum di controllo per il cluster
La configurazione di cloud Witness è perfettamente integrata nella configurazione guidata quorum esistente incorporata nel Gestione cluster di failover.  

### <a name="to-configure-cloud-witness-as-a-quorum-witness"></a>Per configurare il server di controllo del mirroring come quorum di controllo
1. Avviare Gestione cluster di failover.
2. Fare clic con il pulsante destro del mouse sul cluster-> **altre azioni** -> **configurare le impostazioni del quorum del cluster** (vedere la figura 6). Verrà avviata la configurazione guidata quorum del cluster.  
    ![snapshot del percorso del menu per le impostazioni del quorum del cluster configura nell'interfaccia utente di Gestione cluster di failover](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_7.png) **Figura 6. Impostazioni quorum del cluster**

3. Nella pagina **selezione configurazioni quorum** selezionare **Seleziona il quorum** di controllo (vedere la figura 7).  

    ![snapshot del pulsante di opzione "selezionare il quotrum Witness" nella procedura guidata quorum del cluster](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_8.png)  
    **Figura 7. Selezionare la configurazione quorum**

4. Nella pagina **selezione quorum** di controllo selezionare **configura un cloud di** controllo (vedere la figura 8).  

    ![snapshot del pulsante di opzione appropriato per selezionare un cloud di controllo](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_9.png)  
    **Figura 8. Selezionare il quorum di controllo**  

5. Nella pagina **Configura cloud Witness** immettere le informazioni seguenti:  
   1. (Parametro obbligatorio) Nome dell'account di archiviazione di Azure.  
   2. (Parametro obbligatorio) Chiave di accesso corrispondente all'account di archiviazione.  
       1. Quando si crea per la prima volta, usare la chiave di accesso primaria (vedere la figura 5).  
       2. Quando si ruota la chiave di accesso primaria, usare la chiave di accesso secondaria (vedere la figura 5).  
   3. (Parametro facoltativo) Se si intende usare un endpoint di servizio di Azure diverso (ad esempio il servizio Microsoft Azure in Cina), aggiornare il nome del server dell'endpoint.  

      ![snapshot del riquadro Configurazione del server di controllo del cloud nella procedura guidata quorum del cluster](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_10.png)  
      **Figura 9: configurare il server di controllo del cloud**

6. Una volta completata la configurazione di cloud Witness, è possibile visualizzare la risorsa del server di controllo appena creata nella snap-in Gestione cluster di failover (vedere la figura 10).

    ![configurazione corretta del server di controllo del cloud](media/Deploy-a-Cloud-Witness-for-a-Failover-Cluster/CloudWitness_11.png)  
    **Figura 10: configurazione corretta del server di controllo del cloud**

### <a name="configuring-cloud-witness-using-powershell"></a>Configurazione di cloud Witness con PowerShell  
Il comando di PowerShell set-ClusterQuorum esistente presenta nuovi parametri aggiuntivi corrispondenti a cloud Witness.  

È possibile configurare il cloud Witness usando il [`Set-ClusterQuorum`](https://technet.microsoft.com/library/ee461013.aspx) comando PowerShell seguente:  

```PowerShell
Set-ClusterQuorum -CloudWitness -AccountName <StorageAccountName> -AccessKey <StorageAccountAccessKey>
```

Se è necessario usare un endpoint diverso (rare):  

```PowerShell
Set-ClusterQuorum -CloudWitness -AccountName <StorageAccountName> -AccessKey <StorageAccountAccessKey> -Endpoint <servername>  
```

### <a name="azure-storage-account-considerations-with-cloud-witness"></a>Considerazioni sull'account di archiviazione di Azure con Cloud Witness  
Quando si configura un server di controllo del mirroring come quorum di controllo per il cluster di failover, tenere presente quanto segue:
* Anziché archiviare la chiave di accesso, il cluster di failover genera e archivia in modo sicuro un token di sicurezza di accesso condiviso (SAS).  
* Il token SAS generato è valido fino a quando la chiave di accesso rimane valida. Quando si ruota la chiave di accesso primaria, è importante prima di tutto aggiornare il cloud Witness (in tutti i cluster che usano tale account di archiviazione) con la chiave di accesso secondaria prima di rigenerare la chiave di accesso primaria.  
* Cloud Witness usa l'interfaccia REST HTTPS del servizio account di archiviazione di Azure. Ciò significa che è necessario che la porta HTTPS sia aperta in tutti i nodi del cluster.

### <a name="proxy-considerations-with-cloud-witness"></a>Considerazioni sul proxy con Cloud Witness  
Cloud Witness usa HTTPS (porta predefinita 443) per stabilire la comunicazione con il servizio BLOB di Azure. Verificare che la porta HTTPS sia accessibile tramite il proxy di rete.

## <a name="see-also"></a>Vedi anche
- [Novità del clustering di failover in Windows Server](whats-new-in-failover-clustering.md)
