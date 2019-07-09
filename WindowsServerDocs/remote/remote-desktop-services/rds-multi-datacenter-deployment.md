---
title: Data center di Servizi Desktop remoto con ridondanza geografica in Azure
description: Informazioni su come creare una distribuzione di Servizi Desktop remoto che usa più data center per fornire disponibilità elevata tra aree geografiche.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 61c36528-cf47-4af0-83c1-a883f79a73a5
author: haley-rowland
ms.author: elizapo
ms.date: 06/14/2017
manager: dongill
ms.openlocfilehash: 2d12062f302c28a8124e0aa49af7f441e77ffe33
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/17/2019
ms.locfileid: "66222789"
---
# <a name="create-a-geo-redundant-multi-data-center-rds-deployment-for-disaster-recovery"></a>Creare una distribuzione di Servizi Desktop remoto con ridondanza geografica e in più data center per il ripristino di emergenza

>Si applica a: Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016

È possibile abilitare il ripristino di emergenza per la distribuzione di Servizi Desktop remoto usando più data center in Azure. A differenza di una distribuzione di Servizi Desktop remoto a disponibilità elevata standard (come descritto in [Architettura di Servizi Desktop remoto](desktop-hosting-logical-architecture.md)), che usa i data center in una singola area di Azure (ad esempio Europa occidentale), una distribuzione in più data center usa data center in più aree geografiche per aumentare la disponibilità della distribuzione: un data center di Azure potrebbe non essere disponibile, ma è improbabile che più aree diventino non disponibili nello stesso momento. Distribuendo un'architettura di Servizi Desktop remoto con ridondanza geografica, puoi abilitare il failover in caso di errore irreversibile di un'intera area.

Fai riferimento alle istruzioni seguenti per usare i servizi di infrastruttura di Microsoft Azure e Servizi Desktop remoto per offrire licenze SAL (Subscriber Access License) e servizi di hosting desktop con ridondanza geografica a più tenant tramite il [programma Microsoft SPLA (Service Provider License Agreement)](https://www.microsoft.com/hosting/licensing/splabenefits.aspx). Puoi anche usare i passaggi seguenti per creare un servizio di hosting con ridondanza geografica per i dipendenti usando i [diritti estesi delle licenze CAL Per Utente di Servizi Desktop remoto tramite Software Assurance](https://download.microsoft.com/download/6/B/A/6BA3215A-C8B5-4AD1-AA8E-6C93606A4CFB/Windows_Server_2012_R2_Remote_Desktop_Services_Licensing_Datasheet.pdf).

## <a name="logical-architecture-for-high-availability---single-and-multiple-regions"></a>Architettura logica per la disponibilità elevata - area singola e più aree
L'immagine seguente mostra l'architettura per una distribuzione a disponibilità elevata in una singola area di Azure:

![Distribuzione a disponibilità elevata in una singola area di Azure](media/rds-ha-single-region.png)

La distribuzione è costituita da tre livelli:

- Servizi di Azure - Interfacce di gestione di Azure, inclusi il portale e le API di Azure, e servizi di rete pubblici, ad esempio DNS e indirizzi IP pubblici.
- Servizio di hosting desktop - Macchine virtuali, reti, risorse di archiviazione, servizi di Azure e servizi ruolo Windows Server
- Infrastruttura di Azure - Sistemi operativi Windows Server che eseguono il ruolo Hyper-V, usati per virtualizzare server fisici, unità di archiviazione, commutatori di rete e router. L'infrastruttura di Azure consente di creare macchine virtuali, reti, risorse di archiviazione e applicazioni indipendenti dall'hardware sottostante.


A confronto, ecco l'architettura per una distribuzione che usa più data center di Azure:

![Distribuzione di Servizi Desktop remoto che usa più aree di Azure](media/rds-ha-multi-region.png)

L'intera distribuzione di Servizi Desktop remoto viene replicata in una seconda area di Azure per creare una distribuzione con ridondanza geografica. Questa architettura usa un modello attivo/passivo, in cui viene eseguita una sola distribuzione di Servizi Desktop remoto alla volta. Una connessione da rete virtuale a rete virtuale consente la comunicazione tra i due ambienti. Le distribuzioni di Servizi Desktop remoto si basano su un singolo dominio/foresta di Active Directory e i server AD eseguono la replica tra le due distribuzioni, quindi gli utenti possono accedere a una qualsiasi delle due distribuzioni usando le stesse credenziali. Le impostazioni utente e i dati archiviati nei dischi dei profili utente (UPD) sono memorizzati in una soluzione File server di scalabilità orizzontale di Spazi di archiviazione diretta in un cluster a due nodi. Un secondo cluster di Spazi di archiviazione diretta identico viene distribuito nella seconda area (passiva) e viene usata la replica di archiviazione per replicare i profili utente dalla distribuzione attiva a quella passiva. Il servizio Gestione traffico di Azure viene usato per indirizzare automaticamente gli utenti finali alla distribuzione attualmente attiva. Gli utenti finali accedono alla distribuzione tramite un singolo URL senza essere a conoscenza dell'area che stanno usando.


Sarebbe *possibile* creare una distribuzione di Servizi Desktop remoto non a disponibilità elevata in ogni area, ma si verificherebbe un failover anche in caso di riavvio di una singola macchina virtuale in un'area, aumentando così le probabilità di failover con un conseguente impatto sulle prestazioni.

## <a name="deployment-steps"></a>Fasi di distribuzione
Crea le risorse seguenti in Azure per creare una distribuzione di Servizi Desktop remoto con ridondanza geografica e in più data center:

1. Due gruppi di risorse in due aree di Azure distinte. Ad esempio RG A (la distribuzione attiva, dove RG sta per "resource group" ovvero gruppo di risorse) e RG B (la distribuzione passiva).
2. Una distribuzione di Active Directory a disponibilità elevata in RG A. Per creare la distribuzione, puoi usare il [modello per un nuovo dominio di Active Directory con 2 controller di dominio](https://azure.microsoft.com/resources/templates/active-directory-new-domain-ha-2-dc/).
3. Una distribuzione di Servizi Desktop remoto a disponibilità elevata in RG A. Usa il [modello di distribuzione di una farm di Servizi Desktop remoto tramite Active Directory](https://azure.microsoft.com/resources/templates/rds-deployment-existing-ad/) per creare la distribuzione di Servizi Desktop remoto di base e quindi segui le informazioni in [Servizi Desktop remoto - Disponibilità elevata](rds-plan-high-availability.md) per configurare gli altri componenti di Servizi Desktop remoto per la disponibilità elevata.
4. Una rete virtuale in RG B. Assicurati di usare uno spazio indirizzi che non si sovrapponga alla distribuzione in RG A.
5. Una [connessione da rete virtuale a rete virtuale](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-vnet-vnet-rm-ps) tra i due gruppi di risorse.
6. Due macchine virtuali di Active Directory in un set di disponibilità in RG B. Assicurati che i nomi delle macchine virtuali siano diversi da quelli delle macchine virtuali AD in RG A. Distribuisci due macchine virtuali Windows Server 2016 in un singolo set di disponibilità, installa il ruolo Active Directory Domain Services e quindi alza di livello le macchine nel controller di dominio creato nel passaggio 1.
7. Una seconda distribuzione di Servizi Desktop remoto a disponibilità elevata in RG B. 
   1. Usa di nuovo il [modello di distribuzione di una farm di Servizi Desktop remoto tramite Active Directory](https://azure.microsoft.com/resources/templates/rds-deployment-existing-ad/), ma questa volta apporta le modifiche seguenti. Per personalizzare il modello, selezionalo nella raccolta, fai clic su **Distribuisci in Azure** e quindi su **Modifica modello**.
      1. Modifica lo spazio indirizzi dell'indirizzo IP privato del server DNS in modo che corrisponda alla rete virtuale in RG B. 
      
         Cerca "dnsServerPrivateIp" nelle variabili. Modifica l'indirizzo IP predefinito (10.0.0.4) in modo che corrisponda allo spazio indirizzi definito nella rete virtuale in RG B.
   
      2. Modifica i nomi dei computer in modo che non siano in conflitto con quelli della distribuzione in RG A.
      
         Individua le macchine virtuali nella sezione **Risorse** del modello. Modifica il campo **computerName** in **osProfile**. Ad esempio, "gateway" può diventare "gateway **-b**", "[concat('rdsh-', copyIndex())]" può diventare "[concat('rdsh-b-', copyIndex())]" e "broker" può diventare "broker **-b**".
      
         Puoi anche modificare i nomi delle macchine virtuali manualmente dopo l'esecuzione del modello.
   2. Come nel passaggio 3 precedente usa le informazioni in [Servizi Desktop remoto - Disponibilità elevata](rds-plan-high-availability.md) per configurare gli altri componenti di Servizi Desktop remoto per la disponibilità elevata.
8. File server di scalabilità orizzontale di Spazi di archiviazione diretta con replica di archiviazione nelle due distribuzioni. Usa lo [script di PowerShell](https://github.com/robotechredmond/301-s2d-sr-dr-md/tree/master/scripts) per distribuire il [modello](https://github.com/robotechredmond/301-s2d-sr-dr-md) nei gruppi di risorse.

   > [!NOTE]
   > Puoi effettuare il provisioning delle risorse di archiviazione manualmente, invece di usare il modello e lo script di PowerShell: 
   >1. Distribuisci una soluzione [File server di scalabilità orizzontale di Spazi di archiviazione diretta a due nodi](rds-storage-spaces-direct-deployment.md) in RG A per archiviare i dischi dei profili utente.
   >2. Distribuisci una seconda soluzione File server di scalabilità orizzontale di Spazi di archiviazione diretta identica in RG B, assicurandoti di usare lo stesso spazio di archiviazione in ogni cluster.
   >3. Configura la [replica di archiviazione con replica asincrona](../../storage/storage-replica/cluster-to-cluster-storage-replication.md) tra le due soluzioni.

### <a name="enable-upds"></a>Abilitare i dischi dei profili utente
La replica di archiviazione replica i dati da un volume di origine (associato alla distribuzione primaria/attiva) a un volume di destinazione (associato alla distribuzione secondaria/passiva). Per impostazione predefinita, il cluster di destinazione viene indicato come **Online (No Access)** (Online - Nessun accesso). La replica di archiviazione smonta i volumi di destinazione e le relative lettere di unità o punti di montaggio. Ciò significa che l'abilitazione dei dischi dei profili utente per la distribuzione secondaria fornendo il percorso della condivisione file non riesce, perché il volume non è montato. 

Vuoi altre informazioni sulla gestione della replica? Vedi [Replica di archiviazione da cluster a cluster](../../storage/storage-replica/cluster-to-cluster-storage-replication.md).

Per abilitare i dischi dei profili utente in entrambe le distribuzioni, esegui questa procedura:

1. Esegui il [cmdlet Set-RDSessionCollectionConfiguration](https://docs.microsoft.com/powershell/module/remotedesktop/set-rdsessioncollectionconfiguration) per abilitare i dischi dei profili utente per la distribuzione primaria (attiva) fornendo un percorso alla condivisione file nel volume di origine (di cui è stata eseguita la creazione nel passaggio di distribuzione 7).
2. Inverti la direzione della replica di archiviazione in modo che il volume di destinazione diventi il volume di origine (in questo modo il volume viene montato e diventa accessibile per la distribuzione secondaria). A tale scopo, puoi eseguire il cmdlet **Set-SRPartnership**. Ad esempio:

   ```powershell
   Set-SRPartnership -NewSourceComputerName "cluster-b-s2d-c" -SourceRGName "cluster-b-s2d-c" -DestinationComputerName "cluster-a-s2d-c" -DestinationRGName "cluster-a-s2d-c"
   ```
3. Abilita i dischi dei profili utente nella distribuzione secondaria (passiva). Usa gli stessi passaggi seguiti per la distribuzione primaria, nel passaggio 1.
4. Inverti di nuovo la direzione della replica di archiviazione, in modo che il volume di origine originale torni a essere il volume di origine nella relazione di replica di archiviazione, così che la distribuzione primaria possa accedere alla condivisione file. Ad esempio:

   ```powershell
   Set-SRPartnership -NewSourceComputerName "cluster-a-s2d-c" -SourceRGName "cluster-a-s2d-c" -DestinationComputerName "cluster-b-s2d-c" -DestinationRGName "cluster-b-s2d-c"
   ```


### <a name="azure-traffic-manager"></a>Gestione traffico di Azure 

Crear un profilo di [Gestione traffico di Azure](/azure/traffic-manager/traffic-manager-overview) e seleziona il metodo di routing **Priorità**. Imposta i due endpoint sugli indirizzi IP pubblici di ogni distribuzione. In **Configurazione** modifica il protocollo in HTTPS (al posto di HTTP) e la porta in 443 (al posto di 80). Prendi nota del valore di **Durata DNS** e impostalo in modo appropriato in base alle esigenze di failover. 

Gestione traffico richiede che gli endpoint restituiscano 200 OK in risposta a una richiesta GET per poter essere contrassegnati come "integri". L'oggetto indirizzo IP pubblico creato dai modelli di Servizi Desktop remoto funzionerà, ma non aggiungerà informazioni sul percorso. In alternativa, puoi fornire agli utenti finali l'URL di Gestione traffico aggiungendo "/RDWeb", ad esempio: ```http://deployment.trafficmanager.net/RDWeb```

Distribuendo Gestione traffico di Azure con il metodo di routing Priorità impedisci agli utenti finali l'accesso alla distribuzione passiva quando la distribuzione attiva funziona correttamente. Se gli utenti finali accedono alla distribuzione passiva e la direzione della replica di archiviazione non è stata cambiata per il failover, l'accesso dell'utente si blocca perché la distribuzione non riesce ad accedere alla condivisione file nel cluster di Spazi di archiviazione diretta passivo, fino a quando la distribuzione smette di eseguire tentativi e all'utente viene assegnato un profilo temporaneo.  

### <a name="deallocate-vms-to-save-resources"></a>Deallocare le macchine virtuali per risparmiare risorse 
Dopo aver configurato entrambe le distribuzioni, puoi arrestare e deallocare l'infrastruttura di Servizi Desktop remoto e le macchine virtuali di Host sessione Desktop remoto secondarie per risparmiare sui costi delle macchine. File server di scalabilità orizzontale di Spazi di archiviazione diretta e le macchine virtuali server Active Directory devono sempre rimanere in esecuzione nella distribuzione secondaria/passiva per consentire la sincronizzazione di account utente e profili.  

Quando si verifica un failover, è necessario avviare le macchine virtuali deallocate. Questa configurazione della distribuzione ha il vantaggio di avere un costo inferiore, ma a discapito del tempo di failover. Se si verifica un errore irreversibile nella distribuzione attiva, dovrai avviare manualmente la distribuzione passiva oppure usare uno script di automazione per rilevare l'errore e avviare automaticamente la distribuzione passiva. In entrambi i casi, potrebbero essere necessari alcuni minuti per avviare la distribuzione passiva e renderla disponibile per l'accesso degli utenti, con un conseguente tempo di inattività del servizio. Questo tempo di inattività dipende dalla quantità di tempo che serve per avviare l'infrastruttura di Servizi Desktop remoto e le macchine virtuali di Host sessione Desktop remoto (in genere da 2 a 4 minuti se le macchine virtuali vengono avviate in parallelo invece che in modo seriale) e dal tempo necessario per portare online il cluster passivo (che dipende dalle dimensioni del cluster, in genere da 2 a 4 minuti per un cluster a 2 nodi con 2 dischi per nodo). 

### <a name="active-directory"></a>Active Directory 
I server Active Directory in ogni distribuzione sono repliche nello stesso dominio/foresta. Active Directory ha un protocollo di sincronizzazione predefinito per mantenere sincronizzati i quattro controller di dominio. Tuttavia, potrebbe verificarsi un ritardo e quindi, se viene aggiunto un nuovo utente a un server AD, potrebbero essere necessari alcuni minuti per la replica in tutti i server AD nelle due distribuzioni. Di conseguenza, assicurati di avvisare gli utenti di non provare a eseguire l'accesso immediatamente dopo essere stati aggiunti al dominio. 

### <a name="rd-license-server"></a>Server licenze Desktop remoto 
Fornisci una [licenza CAL Per Utente di Desktop remoto](rds-client-access-license.md) per ogni utente non anonimo autorizzato ad accedere alla distribuzione con ridondanza geografica. Distribuisci le licenze CAL Per Utente in modo uniforme nei due server licenze Desktop remoto nella distribuzione attiva. Duplica quindi le licenze CAL Per Utente nei due server licenze Desktop remoto nella distribuzione passiva. Poiché le licenze CAL sono duplicate tra la distribuzione attiva e quella passiva, in qualsiasi momento può essere attiva una sola distribuzione con utenti che si connettono. In caso contrario, verrebbe violato il contratto di licenza.  

### <a name="image-management"></a>Gestione delle immagini 
Quando aggiorni le immagini di Host sessione Desktop remoto per fornire aggiornamenti software o nuove applicazioni, devi aggiornare separatamente gli insiemi di Host sessione Desktop remoto in ogni distribuzione per mantenere un'esperienza utente comune in entrambe le distribuzioni. Puoi usare il [modello di aggiornamento di un insieme di Host sessione Desktop remoto](https://azure.microsoft.com/resources/templates/rds-update-rdsh-collection/), ma tieni presente che l'infrastruttura di Servizi Desktop remoto della distribuzione passiva e le macchine virtuali di Host sessione Desktop remoto devono essere in esecuzione per poter eseguire il modello. 

## <a name="failover"></a>Failover

Nel caso della distribuzione attiva/passiva, il failover richiede l'avvio delle macchine virtuali della distribuzione secondaria. Puoi eseguire questa operazione manualmente o con uno script di automazione. Nel caso di failover irreversibile di File server di scalabilità orizzontale di Spazi di archiviazione diretta, modifica la direzione della relazione di replica di archiviazione in modo che il volume di destinazione diventi il volume di origine. Ad esempio:

   ```powershell
   Set-SRPartnership -NewSourceComputerName "cluster-b-s2d-c" -SourceRGName "cluster-b-s2d-c" -DestinationComputerName "cluster-a-s2d-c" -DestinationRGName "cluster-a-s2d-c"
   ```

Per altre informazioni, vedi [Replica di archiviazione da cluster a cluster](../../storage/storage-replica/cluster-to-cluster-storage-replication.md).

Gestione traffico di Azure riconosce automaticamente che la distribuzione primaria non funziona e che la distribuzione secondaria è integra (le macchine virtuali in Gateway Desktop remoto sono state avviate in RG B) e indirizza il traffico utente alla distribuzione secondaria. Gli utenti possono usare lo stesso URL di Gestione traffico per continuare a lavorare con le risorse remote, per un'esperienza coerente. Tieni presente che la cache DNS client non aggiorna il record per la durata (TTL) impostata nella configurazione di Gestione traffico di Azure.

### <a name="test-failover"></a>Failover di test
In una relazione di replica di archiviazione, può essere attivo un solo volume (origine) per volta. Ciò significa che quando cambi la direzione della relazione di replica di archiviazione, il volume nella distribuzione primaria (RG A) diventa la destinazione della replica e di conseguenza viene nascosto. Gli utenti che si connettono a RG A non potranno quindi più accedere ai relativi dischi dei profili utente archiviati in File server di scalabilità orizzontale in RG A. 

Per testare il failover consentendo agli utenti di continuare ad accedere:
1. Avvia le macchine virtuali dell'infrastruttura e le macchine virtuali di Host sessione Desktop remoto in RG B.
2. Modifica la direzione della relazione di replica di archiviazione (cluster-b-s2d-c diventa il volume di origine).
3. [Disabilita l'endpoint](/azure/traffic-manager/traffic-manager-manage-endpoints#to-disable-an-endpoint) di RG A nel profilo di Gestione traffico di Azure per fare in modo che il servizio indirizzi il traffico a RG B. In alternativa, puoi usare uno script di PowerShell:

   ```powershell
   Disable-AzureRmTrafficManagerEndpoint -Name publicIpA -Type AzureEndpoints -ProfileName MyTrafficManagerProfile -ResourceGroupName RGA -Force
   ```

RG B è ora la distribuzione primaria attiva. Per reimpostare RG A come distribuzione primaria:

1. Modifica la direzione della relazione di replica di archiviazione (cluster-a-s2d-c diventa il volume di origine):

   ```powershell
   Set-SRPartnership -NewSourceComputerName "cluster-a-s2d-c" -SourceRGName "cluster-a-s2d-c" -DestinationComputerName "cluster-b-s2d-c" -DestinationRGName "cluster-b-s2d-c"
   ```
2. Riabilita l'endpoint di RG A nel profilo di Gestione traffico di Azure:

   ```powershell
   Enable-AzureRmTrafficManagerEndpoint -Name publicIpA -Type AzureEndpoints -ProfileName MyTrafficManagerProfile -ResourceGroupName RGA 
   ```

## <a name="considerations-for-on-premises-deployments"></a>Considerazioni per le distribuzioni locali

Anche se una distribuzione locale non può usare i modelli di avvio rapido di Azure indicati in questo articolo, puoi implementare tutti i ruoli di infrastruttura manualmente. In una distribuzione locale in cui i costi non sono determinati dal consumo di risorse di Azure, è consigliabile usare un modello attivo/attivo per un failover più veloce.

Puoi usare Gestione traffico di Azure con endpoint locali, ma è necessaria una sottoscrizione di Azure. In alternativa, per il DNS fornito agli utenti finali, fornisci un record CNAME che indirizza semplicemente gli utenti alla distribuzione primaria. In caso di failover, modifica il record CNAME DNS per eseguire il reindirizzamento alla distribuzione secondaria. In questo modo, l'utente finale usa un singolo URL, proprio come con Gestione traffico di Azure, che indirizza l'utente alla distribuzione appropriata. 

Se vuoi creare un modello di sito da locale ad Azure, puoi usare [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview).
