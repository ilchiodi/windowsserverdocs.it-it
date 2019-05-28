---
title: Con ridondanza geografica data center di servizi desktop remoto in Azure
description: Informazioni su come creare una distribuzione di servizi desktop remoto che usa più data Center per garantire una disponibilità elevata tra aree geografiche.
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
ms.sourcegitcommit: 8ba2c4de3bafa487a46c13c40e4a488bf95b6c33
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/25/2019
ms.locfileid: "66222789"
---
# <a name="create-a-geo-redundant-multi-data-center-rds-deployment-for-disaster-recovery"></a>Creare un geograficamente ridondante, più data center la distribuzione di servizi desktop remoto per il ripristino di emergenza

>Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016

È possibile abilitare il ripristino di emergenza per la distribuzione di Servizi Desktop remoto usando più data center in Azure. A differenza di una distribuzione di servizi desktop remoto a disponibilità elevata standard (come descritto nel [architettura di Servizi Desktop remoto](desktop-hosting-logical-architecture.md)), che usa i Data Center in una singola area di Azure (ad esempio, Europa occidentale), una distribuzione a più data center Usa i dati Data Center in più aree geografiche, aumentare la disponibilità della distribuzione - un data center di Azure potrebbe non essere disponibile, ma è improbabile che le aree più andrebbe verso il basso nello stesso momento. Tramite la distribuzione di un'architettura di servizi desktop remoto con ridondanza geografica, è possibile abilitare il failover nel caso di errore irreversibile di un'intera area.

È possibile usare le istruzioni seguenti per sfruttare servizi di infrastruttura di Microsoft Azure e servizi desktop remoto per distribuire licenze SAL (Subscriber) Access e i servizi di hosting del desktop con ridondanza geografica a più tenant tramite il [Provider di servizi di Microsoft Programma License Agreement (SPLA)](https://www.microsoft.com/hosting/licensing/splabenefits.aspx). È anche possibile usare la procedura seguente per creare un servizio di hosting con ridondanza geografica per i proprio dipendenti che usano [CAL utente i diritti tramite Software Assurance estesi](https://download.microsoft.com/download/6/B/A/6BA3215A-C8B5-4AD1-AA8E-6C93606A4CFB/Windows_Server_2012_R2_Remote_Desktop_Services_Licensing_Datasheet.pdf).

## <a name="logical-architecture-for-high-availability---single-and-multiple-regions"></a>Architettura logica per la disponibilità elevata - singola e più aree
L'immagine seguente mostra l'architettura per la distribuzione a disponibilità elevata in una singola area di Azure:

![Una distribuzione a disponibilità elevata in una singola area di Azure](media/rds-ha-single-region.png)

La distribuzione è costituita da tre livelli:

- Servizi di Azure - le interfacce di gestione di Azure, inclusi il portale di Azure e le API e servizi di rete pubblici, ad esempio DNS e indirizzi IP pubblici.
- Servizio - macchine virtuali, reti, archiviazione, servizi di Azure e servizi del ruolo Server di Windows dell'hosting del desktop
- Infrastruttura di Azure - sistemi operativi Windows Server che esegue il ruolo Hyper-V, consente di virtualizzare i server fisici, unità di archiviazione, commutatori di rete e i router. Utilizzo dell'infrastruttura di Azure consente di creare macchine virtuali, reti, archiviazione e le applicazioni indipendenti dall'hardware sottostante.


In confronto, ecco l'architettura per una distribuzione che usa più data center di Azure:

![Una distribuzione di servizi desktop remoto che usa più aree di Azure](media/rds-ha-multi-region.png)

L'intera distribuzione di servizi desktop remoto viene replicata in una seconda area di Azure per creare una distribuzione con ridondanza geografica. Questa architettura Usa un modello attivo / passivo, in cui viene eseguito solo una distribuzione di servizi desktop remoto alla volta. Una connessione di rete virtuale a rete virtuale consente due ambienti di comunicare tra loro. Le distribuzioni di servizi desktop remoto si basano su un singolo insieme di strutture di Active Directory/dominio e i server AD eseguire la replica tra le due distribuzioni, pertanto gli utenti possono accedere a una distribuzione usando le stesse credenziali. Dati archiviati in dischi profili utente (UPD) e le impostazioni utente vengono archiviati in un file server cluster a due nodi spazi di archiviazione diretta di scalabilità orizzontale (SOFS). Nella seconda area (passivo) viene distribuito un cluster di spazi di archiviazione diretta identico secondo e la Replica di archiviazione viene usata per replicare i profili utente da attivo a passiva distribuzione. Gestione traffico di Azure viene usato per indirizzare automaticamente agli utenti finali di qualsiasi distribuzione è attualmente attivo - dal punto di vista degli utenti finali, la distribuzione tramite un singolo URL di accesso e non sono a conoscenza dell'area in cui vengono emesse utilizzando.


Si *è stato possibile* creare una distribuzione di servizi desktop remoto non a disponibilità elevata in ogni area, ma se anche una singola macchina virtuale viene riavviata in un'area, si verifica un failover, aumentando la probabilità di failover che si verificano con associato l'impatto sulle prestazioni.

## <a name="deployment-steps"></a>Fasi di distribuzione
Creare le risorse seguenti in Azure per creare una distribuzione di servizi desktop remoto con ridondanza geografica a più data center:

1. Separano i due gruppi di risorse in due aree di Azure. Ad esempio A RG (la distribuzione attiva, gruppo di risorse è l'acronimo di "gruppo di risorse") e B RG (distribuzione passiva).
2. Una distribuzione di Active Directory a disponibilità elevata in RG A. È possibile usare la [nuovo dominio di Active Directory con il modello di controller di dominio 2](https://azure.microsoft.com/resources/templates/active-directory-new-domain-ha-2-dc/) per creare la distribuzione.
3. Una distribuzione di servizi desktop remoto a disponibilità elevata in RG A. Use il [Servizi Desktop remoto distribuiti tramite active directory esistente della farm](https://azure.microsoft.com/resources/templates/rds-deployment-existing-ad/) modello per creare la distribuzione di servizi desktop remoto di base e quindi seguire le informazioni contenute in [Servizi Desktop remoto - alto disponibilità](rds-plan-high-availability.md) per configurare gli altri componenti di servizi desktop remoto per la disponibilità elevata.
4. Una rete virtuale B RG - assicurarsi di usare uno spazio indirizzi che non si sovrapponga la distribuzione nel RG A.
5. Oggetto [VNet-to-VNet connessione](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-vnet-vnet-rm-ps) tra i gruppi di due risorse.
6. Due macchine virtuali di Active Directory in un set di disponibilità nel gruppo di risorse B - assicurarsi che i nomi delle VM sono diverse dalle VM AD RG A. Deploy due VM di Windows Server 2016 in una singola disponibilità impostato, installare il ruolo Servizi di dominio Active Directory e quindi promozione a segue il dominio Rullo nel dominio che è stato creato nel passaggio 1.
7. Una seconda distribuzione di servizi desktop remoto a disponibilità elevata in RG B. 
   1. Usare la [Servizi Desktop remoto distribuiti tramite active directory esistente della farm](https://azure.microsoft.com/resources/templates/rds-deployment-existing-ad/) modello anche in questo caso, ma questa volta apportare le modifiche seguenti. (Per personalizzare il modello, selezionarlo in gallery, fare clic su **Deploy to Azure** e quindi **modifica modello**.)
      1. Modificare lo spazio degli indirizzi dell'indirizzo IP privato di server DNS in modo che corrispondano alla rete virtuale in RG B. 
      
         Cercare "dnsServerPrivateIp" nelle variabili. Modificare l'indirizzo IP predefinito (10.0.0.4) in modo da corrispondere allo spazio di indirizzi definiti nella rete virtuale in RG B.
   
      2. Modificare i nomi dei computer in modo che essi non siano in conflitto con quelli della distribuzione A., RG
      
         Individuare le macchine virtuali nel **risorse** sezione del modello. Modifica il **computerName** campo **osProfile**. Ad esempio, può diventare "gateway" "gateway **-b**"; "[concat ('rdsh-', copyIndex())]" può diventare "[concat ('rdsh - b-', copyIndex())]" e "broker" possono diventare "broker **-b**".
      
         (È possibile anche modificare i nomi delle macchine virtuali manualmente dopo l'esecuzione del modello.)
   2. Come indicato nel passaggio 3 riportato sopra, usare le informazioni contenute in [Servizi Desktop remoto - disponibilità elevata](rds-plan-high-availability.md) per configurare gli altri componenti di servizi desktop remoto per la disponibilità elevata.
8. Spazi di archiviazione diretta scale-out file server con Replica archiviazione tra le due distribuzioni. Usare la [script di PowerShell](https://github.com/robotechredmond/301-s2d-sr-dr-md/tree/master/scripts) per distribuire il [modello](https://github.com/robotechredmond/301-s2d-sr-dr-md) tutti i gruppi di risorse.

   > [!NOTE]
   > È possibile eseguire il provisioning di archiviazione manualmente (invece di usare lo script di PowerShell e modello): 
   >1. Distribuire un' [SOFS diretto spazi di archiviazione di due nodi](rds-storage-spaces-direct-deployment.md) a gruppo di risorse per archiviare i dischi dei profili utente (UPD).
   >2. Distribuire un secondo, identica archiviazione spazi diretti SOFS in RG B: assicurarsi di usare la stessa quantità di spazio di archiviazione in ogni cluster.
   >3. Configurare [Replica di archiviazione con la replica asincrona](../../storage/storage-replica/cluster-to-cluster-storage-replication.md) tra i due.

### <a name="enable-upds"></a>Abilita gli UPD
Replica archiviazione replica i dati da un volume di origine (associato alla distribuzione primaria/attiva) per un volume di destinazione (associato con la distribuzione della replica secondaria/passiva). Per impostazione predefinita, il cluster di destinazione viene visualizzato come **Online (Nessun accesso)** -Replica archiviazione Smonta i volumi di destinazione e i punti di montaggio o lettere di unità. Ciò significa che l'abilitazione degli UPD per la distribuzione secondaria, fornendo il percorso della condivisione file non riuscirà, perché non è montato il volume. 

Vuoi altre informazioni sulla gestione della replica? Consulta [Cluster a cluster di replica di archiviazione](../../storage/storage-replica/cluster-to-cluster-storage-replication.md).

Per abilitare gli UPD in entrambe le distribuzioni, eseguire le operazioni seguenti:

1. Eseguire la [cmdlet Set-RDSessionCollectionConfiguration](https://docs.microsoft.com/powershell/module/remotedesktop/set-rdsessioncollectionconfiguration) per abilitare i dischi dei profili utente per la distribuzione (attivo) primario - specificare il percorso della condivisione file nel volume di origine (che è stata creata nel passaggio 7 nei passaggi di distribuzione).
2. Invertire la direzione della Replica di archiviazione in modo che il volume di destinazione diventa il volume di origine (monta il volume e lo rende accessibile per la distribuzione secondaria). È possibile eseguire **Set-SRPartnership** cmdlet per eseguire questa operazione. Ad esempio: 

   ```powershell
   Set-SRPartnership -NewSourceComputerName "cluster-b-s2d-c" -SourceRGName "cluster-b-s2d-c" -DestinationComputerName "cluster-a-s2d-c" -DestinationRGName "cluster-a-s2d-c"
   ```
3. Abilitare i dischi dei profili utente della distribuzione secondario (passivo). Usare la stessa procedura usata per la distribuzione primaria, nel passaggio 1.
4. Invertire la direzione della Replica di archiviazione anche in questo caso, in modo che il volume di origine originale viene nuovamente il volume di origine nella relazione di SR e alla distribuzione primaria può accedere alla condivisione file. Ad esempio: 

   ```powershell
   Set-SRPartnership -NewSourceComputerName "cluster-a-s2d-c" -SourceRGName "cluster-a-s2d-c" -DestinationComputerName "cluster-b-s2d-c" -DestinationRGName "cluster-b-s2d-c"
   ```


### <a name="azure-traffic-manager"></a>Gestione traffico di Azure 

Creare un [gestione traffico di Azure](/azure/traffic-manager/traffic-manager-overview) del profilo e assicurarsi di selezionare la **priorità** il metodo di routing. Impostare i due endpoint per gli indirizzi IP pubblici di ogni distribuzione. Sotto **configurazione**, modificare il protocollo HTTPS (invece di HTTP) e la porta 443 (anziché 80). Annotare il **durata DNS**e impostarlo in modo appropriato per il failover. 

Si noti che Gestione traffico richiede gli endpoint da restituire 200 OK in risposta a una richiesta GET per poter essere contrassegnato come "Integro". L'oggetto indirizzo IP pubblico creato dai modelli di servizi desktop remoto funzionerà, ma non aggiungere un supplemento di percorso. In alternativa, è possibile assegnare gli utenti finali l'URL di gestione traffico con "/ RDWeb" aggiunto, ad esempio: ```http://deployment.trafficmanager.net/RDWeb```

Tramite la distribuzione di gestione traffico di Azure con il metodo di routing priorità, impedire agli utenti finali l'accesso alla distribuzione passiva durante la distribuzione attiva è funzionale. Se gli utenti finali di accedere alla distribuzione dei passiva e non è stata passata la direzione di Replica di archiviazione per il failover, l'accesso dell'utente si blocca durante la distribuzione di prova e ha esito negativo accedere alla condivisione file nel cluster di spazi di archiviazione diretta passivo - alla fine la distribuzione verrà rinunciare e concedere all'utente un profilo temporaneo.  

### <a name="deallocate-vms-to-save-resources"></a>Deallocare la VM per risparmiare risorse 
Dopo aver configurato entrambe le distribuzioni, è facoltativamente possibile arrestare e deallocare l'infrastruttura Servizi Desktop remoto e le macchine virtuali host sessione Desktop remoto per risparmiare sui costi in queste macchine virtuali secondarie. Il SOFS diretto di spazi di archiviazione AD server e le macchine virtuali deve sempre rimanere in esecuzione nella distribuzione secondaria/passiva per abilitare la sincronizzazione profilo e account utente.  

Quando si verifica un failover, è necessario avviare le VM deallocate. Questa configurazione di distribuzione ha il vantaggio di essere costo inferiore, ma a scapito della fase di failover. Se si verifica un errore irreversibile nella distribuzione attiva, è necessario avviare manualmente la distribuzione passiva oppure è necessario uno script di automazione per rilevare l'errore e avviare automaticamente la distribuzione passiva. In entrambi i casi, potrebbe richiedere alcuni minuti per ottenere la distribuzione passiva in esecuzione e disponibili per gli utenti per l'accesso, causando periodi di inattività per il servizio. Questo periodo di inattività dipende dalla quantità di tempo viene impiegato per avviare l'infrastruttura di servizi desktop remoto e le macchine virtuali host sessione Desktop remoto (in genere 2 a 4 minuti, se le macchine virtuali vengono avviate in parallelo anziché in modo seriale) e l'ora di portare online il cluster passivo (che dipende dalle dimensioni del cluster in genere 2 a 4 minuti per un cluster a 2 nodi con 2 dischi per nodo). 

### <a name="active-directory"></a>Active Directory 
I server di Active Directory in ogni distribuzione sono repliche nello stesso dominio/foresta. Active Directory dispone di un protocollo di sincronizzazione predefinite per mantenere sincronizzati i controller di quattro dominio. Tuttavia, potrebbero esserci un ritardo in modo che se un nuovo utente viene aggiunto a un server di AD, potrebbe richiedere alcuni minuti per la replica tra tutti i server AD nelle distribuzioni dei due. Di conseguenza, assicurarsi di avvisare gli utenti non tentano di accedere immediatamente dopo essere stato aggiunto al dominio. 

### <a name="rd-license-server"></a>Server licenze Desktop remoto 
Fornire una [CAL di desktop remoto per ogni utente](rds-client-access-license.md) per ogni utente non anonimo che è autorizzato ad accedere alla distribuzione con ridondanza geografica. Distribuire il per ogni utente CAL Servizi Desktop remoto in modo uniforme tra i due server licenze Desktop remoto nella distribuzione attiva. Quindi, duplica queste licenze CAL per due server licenze Desktop remoto nella distribuzione passiva. Perché le licenze CAL sono univoci tra la distribuzione attiva e passiva, in qualsiasi momento può essere attiva solo una distribuzione con utenti che si connettono; in caso contrario, si viola il contratto di licenza.  

### <a name="image-management"></a>Gestione delle immagini 
Quando si aggiornano le immagini host sessione Desktop remoto per fornire aggiornamenti software o nuove applicazioni, è necessario aggiornare separatamente le raccolte di host sessione Desktop remoto in ogni distribuzione per mantenere un'esperienza utente comune in entrambe le distribuzioni. È possibile usare la [modello di host sessione Desktop remoto aggiornamento raccolta](https://azure.microsoft.com/resources/templates/rds-update-rdsh-collection/), ma si noti che è necessario eseguire la distribuzione passiva infrastruttura Servizi Desktop remoto e le macchine virtuali host sessione Desktop remoto per eseguire il modello. 

## <a name="failover"></a>Failover

Nel caso della distribuzione attiva-passiva, il failover è necessario avviare le macchine virtuali della distribuzione secondaria. È possibile farlo manualmente o con uno script di automazione. Nel caso di failover irreversibili del SOFS spazi di archiviazione diretta, modificare la direzione di partnership di Replica di archiviazione, in modo che il volume di destinazione diventa il volume di origine. Ad esempio: 

   ```powershell
   Set-SRPartnership -NewSourceComputerName "cluster-b-s2d-c" -SourceRGName "cluster-b-s2d-c" -DestinationComputerName "cluster-a-s2d-c" -DestinationRGName "cluster-a-s2d-c"
   ```

Altre informazioni, vedere [Cluster a cluster di replica di archiviazione](../../storage/storage-replica/cluster-to-cluster-storage-replication.md).

Gestione traffico di Azure riconosce automaticamente che quella primaria non è riuscito e che la distribuzione secondaria è integra (nelle VM del Gateway Desktop remoto sono stati avviati in RG B) e indirizza il traffico utente per la distribuzione secondaria. Gli utenti possono usare lo stesso URL di gestione traffico a continuare a utilizzare le relative risorse remote, godendo di un'esperienza coerente. Si noti che il client della cache DNS non aggiornerà i record per la durata della durata (TTL) impostata nella configurazione di gestione traffico di Azure.

### <a name="test-failover"></a>Failover di test
In una relazione di Replica di archiviazione, può essere attivo un solo volume (origine) alla volta. Questo significa che quando si passa la direzione di Partnership SR, il volume in quella primaria (A RG) diventa la destinazione della replica e di conseguenza è nascosto. In questo modo, eventuali utenti che si connettono ad A RG non avrà più accesso alle loro UPD archiviati sul SOFS RG A. 

Per testare il failover, consentendo agli utenti di continuare la connessione:
1. Avviare le macchine virtuali di infrastruttura e le macchine virtuali host sessione Desktop remoto in RG B.
2. Invertire la direzione di Partnership SR (cluster-b-s2d-c diventa il volume di origine).
3. [Disabilitare l'endpoint](/azure/traffic-manager/traffic-manager-manage-endpoints#to-disable-an-endpoint) RG nel profilo di gestione traffico di Azure per forzare il servizio di Bancomat per indirizzare il traffico al gruppo di risorse B. in alternativa, usare uno script di PowerShell:

   ```powershell
   Disable-AzureRmTrafficManagerEndpoint -Name publicIpA -Type AzureEndpoints -ProfileName MyTrafficManagerProfile -ResourceGroupName RGA -Force
   ```

RG B è ora la distribuzione primaria attiva. Per tornare alla RG A come la distribuzione primaria:

1. Invertire la direzione di Partnership SR (cluster-a-s2d-c diventa il volume di origine):

   ```powershell
   Set-SRPartnership -NewSourceComputerName "cluster-a-s2d-c" -SourceRGName "cluster-a-s2d-c" -DestinationComputerName "cluster-b-s2d-c" -DestinationRGName "cluster-b-s2d-c"
   ```
2. Riabilitare l'endpoint di RG A nel profilo di gestione traffico di Azure:

   ```powershell
   Enable-AzureRmTrafficManagerEndpoint -Name publicIpA -Type AzureEndpoints -ProfileName MyTrafficManagerProfile -ResourceGroupName RGA 
   ```

## <a name="considerations-for-on-premises-deployments"></a>Considerazioni per le distribuzioni locali

Anche se una distribuzione da sito locale non è stato possibile usare i modelli di avvio rapido di Azure citato nel presente articolo, è possibile implementare manualmente tutti i ruoli di infrastruttura. In una distribuzione da sito locale in cui costi non sono determinato dai consumo di Azure, è consigliabile usare un modello attivo-attivo per il failover più veloce.

È possibile usare Gestione traffico di Azure con endpoint locali, ma richiede una sottoscrizione di Azure. In alternativa, per il DNS fornito agli utenti finali, assegnare loro un record CNAME che semplicemente indirizza gli utenti a quella primaria. In caso di failover, modificare il record CNAME DNS per reindirizzare la distribuzione secondaria. In questo modo, l'utente finale Usa un singolo URL, proprio come con gestione traffico di Azure, che indirizza l'utente per la distribuzione appropriata. 

Se si è interessati alla creazione di un modello in locale-Azure-sito a, è consigliabile usare [Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/site-recovery-overview).
