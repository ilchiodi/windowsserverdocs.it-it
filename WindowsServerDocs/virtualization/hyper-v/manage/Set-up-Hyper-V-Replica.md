---
title: Configurare la replica Hyper-V
ms.technology: compute-hyper-v
description: Vengono fornite istruzioni per la configurazione della replica, il test del failover e la prima replica.
ms.prod: windows-server
manager: dongill
ms.topic: article
ms.assetid: eea9e996-bfec-4065-b70b-d8f66e7134ac
author: kbdazure
ms.author: kathydav
ms.date: 10/10/2016
ms.openlocfilehash: b2730c04e4a4cba4a8c79e600e18bfdba1cfd0c4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80859034"
---
# <a name="set-up-hyper-v-replica"></a>Configurare la replica Hyper-V

>Si applica a: Windows Server 2016

La replica Hyper-V è parte integrante del ruolo Hyper-V Favorisce la strategia di ripristino di emergenza replicando le macchine virtuali da un server host Hyper-V a altro per mantenere i carichi di lavoro disponibili.  Replica Hyper-V crea una copia di una macchina virtuale a una macchina virtuale non in linea di replica. Tenere presente quanto segue:  

-   **Host Hyper-V**: possono essere collocati fisicamente insieme o in posizioni geografiche diverse con la replica in una rete WAN collegare i server host primario e secondario. Host Hyper-V possono essere autonomi, cluster, o una combinazione di entrambi. Non esiste alcuna dipendenza di Active Directory tra i server e non devono essere membri del dominio.  

-   **La replica e il rilevamento delle modifiche**: quando si abilita la Replica Hyper-V per una macchina virtuale specifica, la replica iniziale Crea una macchina virtuale di replica identico in un server host secondario. A questo punto, il rilevamento delle modifiche di Replica Hyper-V crea e gestisce un file di log che acquisiscono le modifiche in un VHD della macchina virtuale. Il file di log viene riprodotto in ordine inverso rispetto alla replica di che disco rigido Virtuale in base alle impostazioni di frequenza di replica. Ciò significa che le modifiche più recenti vengono archiviate e replicate in modo asincrono. La replica può essere tramite HTTP o HTTPS.  

-   **Estesa (concatenata) replica**: consente di replicare una macchina virtuale da un host primario a uno secondario e quindi replicare l'host secondario a un host di terzo. Si noti che non è possibile replicare dall'host principale direttamente al secondo e terzo.  

    Questa funzionalità rende più affidabile per il ripristino di emergenza Replica Hyper-V perché se si verifica un'interruzione è possibile ripristinare dalla replica primaria e quella estesa.  È possibile eseguire il failover alla replica estesa se si disattivano i percorsi primari e secondari. Si noti che la replica estesa non supporta la replica coerente con l'applicazione deve utilizzare i dischi rigidi virtuali stesso che utilizza la replica secondaria.  

-   **Failover**: se si verifica un'interruzione nel server principale (o secondario in caso di estesi) è possibile avviare manualmente un test, pianificato, percorso o il failover non pianificato.  

    ||Test|Pianificato|Non pianificato|  
    |-|--------|-----------|-------------|  
    |Quando è necessario eseguire?|Verificare che una macchina virtuale può eseguire il failover e avviare il sito secondario<p>Utile per il testing e training|Durante le interruzioni e tempi di inattività pianificati|Durante gli eventi imprevisti|  
    |Viene creata una macchina virtuale duplicata?|Sì|No|No|  
    |In cui è iniziata?|Nella macchina virtuale di replica|Avviata nel computer primario e completata nel secondario|Nella macchina virtuale di replica|  
    |Quanto spesso è consigliabile eseguire?|Si consiglia una volta al mese per i test|Una volta ogni sei mesi o in base ai requisiti di conformità|Solo in caso di emergenza quando la macchina virtuale primaria non è disponibile|  
    |La macchina virtuale primaria continuare a replicare?|Sì|Sì. Quando l'interruzione del servizio viene risolto replica inversa replica che le modifiche al sito primario in modo che siano sincronizzati primario e secondario.|No|  
    |È disponibile alcuna perdita di dati?|None|Nessuno Dopo il failover Replica Hyper-V replica l'ultimo set di modifiche rilevate nuovamente al database primario per garantire una perdita di dati.|Dipende i punti di ripristino e di evento|  
    |Si verificano tempi di inattività?|Nessuno Non inciderà nell'ambiente di produzione. Crea una macchina virtuale di test duplicati durante il failover. Termine del failover si seleziona **Failover** sulla replica macchina virtuale e ha automaticamente eliminate ed eliminati.|La durata di un'interruzione pianificata|La durata di inattività non pianificato|  

-   **Punti di ripristino**: quando si configurano le impostazioni di replica per una macchina virtuale, specificare i punti di ripristino che si desidera archiviare da esso. Punti di ripristino rappresentano uno snapshot temporizzato di cui è possibile ripristinare una macchina virtuale. Ovviamente meno dati vengono persi se esegue il ripristino da un punto di ripristino molto recente. È possibile accedere ai punti di ripristino fino a 24 ore.  

## <a name="deployment-prerequisites"></a>Prerequisiti per la distribuzione  
Ecco cosa è necessario verificare prima di iniziare:  

-   **Scoprire che necessitano di dischi rigidi virtuali da replicare**. In particolare, i dischi rigidi virtuali che contengono dati che cambiano rapidamente e non usati dal server di Replica dopo il failover, ad esempio dischi di file di paging, devono essere esclusi dalla replica per risparmiare larghezza di banda di rete. Prendere nota delle quali possono essere esclusi i dischi rigidi virtuali.  

-   **La frequenza con cui si decide di sincronizzare i dati**: I dati nel server di Replica sono sincronizzati aggiornato in base alla frequenza di replica configurata (30 secondi, 5 minuti o 15 minuti). La frequenza è scegliere opportuno considerare quanto segue: le macchine virtuali con un RPO ridotto di dati critici? Quali sono si considerazioni sulla larghezza di banda?  Le macchine virtuali estremamente critico sarà ovviamente necessario replica più frequente.  

-   **Decidere come ripristinare i dati**: per impostazione predefinita la Replica Hyper-V archivia solo un punto di ripristino singolo che sarà la replica più recente inviata dal database primario nel database secondario. Se si desidera che l'opzione per ripristinare i dati in un punto precedente nel tempo, tuttavia è possibile specificare che devono essere archiviati i punti di ripristino aggiuntivi (per un massimo di 24 punti oraria). Se sono necessari ulteriori punti di ripristino si noti che questa operazione richiede più risorse in overhead di elaborazione e archiviazione.  

-   **Scoprire quale è necessario replicare i carichi di lavoro**: la Replica Hyper-V Standard viene mantenuta la coerenza con lo stato del sistema operativo macchina virtuale dopo un failover, ma non lo stato delle applicazioni che l'esecuzione nella macchina virtuale. Se si desidera essere in grado di ripristino dello stato del carico di lavoro è possibile creare ripristino coerenti con l'applicazione fa riferimento. Si noti che ripristino coerenti con l'app non è disponibile sul sito di replica estesa se si utilizza la replica estesa (concatenata).  

-   **Decidere come eseguire la replica iniziale dei dati della macchina virtuale**: la replica viene avviata trasferendo le esigenze per trasferire lo stato corrente delle macchine virtuali. Questo stato iniziale può essere trasmesso direttamente tramite la rete esistente, immediatamente o in un momento successivo configurato. È inoltre possibile utilizzare una macchina virtuale ripristinata preesistente (ad esempio, se è stato ripristinato un backup precedente della macchina virtuale nel server di Replica) come copia iniziale. In alternativa, è possibile risparmiare larghezza di banda di rete copiando la copia iniziale su un supporto esterno e quindi distribuendo fisicamente il supporto nel sito di replica.  Se si desidera utilizzare una preesistente tutti gli snapshot precedenti associati eliminazione della macchina virtuale.  

## <a name="deployment-steps"></a>Fasi di distribuzione  

### <a name="step-1-set-up-the-hyper-v-hosts"></a>Passaggio 1: Configurare gli host Hyper-V  
È necessario almeno due host Hyper-V con uno o più macchine virtuali in ogni server. [Introduzione a Hyper-V](../get-started/Get-started-with-Hyper-V-on-Windows.md). Il server host che si effettua la replica per le macchine virtuali dovranno essere impostati come server di replica.  

1.  Nelle impostazioni di Hyper-V per il server è necessario replicare macchine virtuali in, in **configurazione della replica**, selezionare **Abilita questo computer come server di Replica**.  

2.  È possibile replicare tramite HTTP o HTTPS crittografato. Selezionare **Usa Kerberos (HTTP)** o **utilizzano l'autenticazione basata su certificato (HTTPS**). Per impostazione predefinita, HTTP 80 e HTTPS 443 sono abilitati come eccezioni del firewall nel server Hyper-V di replica. Se si modificano le impostazioni della porta predefinita è necessario modificare anche l'eccezione del firewall. Se si esegue la replica su HTTPS, è necessario configurare l'autenticazione del certificato è necessario selezionare un certificato.  

3.  Per l'autorizzazione, selezionare **consentire la replica da qualsiasi server autenticato** per consentire al server di replica accettare il traffico di replica macchina virtuale da qualsiasi server primario che esegue l'autenticazione corretta. Selezionare **consentire la replica da server specificato** ad accettare il traffico solo dal server primario specificamente selezionati.  

    Per entrambe le opzioni è possibile specificare dove devono essere archiviati i dischi rigidi virtuali replicati nel server Hyper-V di replica.  

4.  Fare clic su **OK** o su **Applica**.  

### <a name="step-2-set-up-the-firewall"></a>Passaggio 2: Configurare il firewall  
Per consentire la replica tra i server primari e secondari, è necessario ottenere il traffico attraverso Windows firewall (o qualsiasi altro firewall di terze parti). Quando è installato il ruolo Hyper-V Server eccezioni predefinite per HTTP (80) e HTTPS (443) vengono creati. Se si utilizza queste porte standard, è necessario solo abilitare le regole:  

-  Per abilitare le regole in un server host autonomo:  

    1. Aprire Windows Firewall con sicurezza avanzata e fare clic su **Regole connessioni in entrata**.  

    2. Per abilitare l'autenticazione HTTP (Kerberos), fare doppio clic su **Listener HTTP di Replica Hyper-V (TCP-In)**  >**Abilita regola.** Per abilitare l'autenticazione basata su certificato HTTPS, fare doppio clic su **Listener HTTPS di Replica Hyper-V (TCP-In)** > E**Attiva regola**.  

-  Per abilitare le regole in un cluster Hyper-V, aprire una sessione di Windows PowerShell utilizzando **Esegui come amministratore**, quindi eseguire uno dei seguenti comandi:  

    -   Per HTTP: `get-clusternode | ForEach-Object  {Invoke-command -computername $_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTP Listener (TCP-In)"}}`  

    -   Per HTTPS: `get-clusternode | ForEach-Object  {Invoke-command -computername $_.name -scriptblock {Enable-Netfirewallrule -displayname "Hyper-V Replica HTTPS Listener (TCP-In)"}}`  

### <a name="enable-virtual-machine-replication"></a>Abilitare le macchine virtuali per la replica  
Eseguire le operazioni seguenti in ogni macchina virtuale da replicare:  

1.  Nel **Dettagli** riquadro di gestione di Hyper-V, selezionare una macchina virtuale facendovi clic sopra.  
    Pulsante destro del mouse sulla macchina virtuale selezionata e fare clic su **Abilita replica** per aprire l'abilitazione guidata replica.  

2.  Nella pagina **Prima di iniziare** fare clic su **Avanti**.  

3.  Nel **specificare il Server di Replica** pagina, nella casella degli strumenti Server di Replica, immettere il NetBIOS o FQDN del server di Replica. Se il server di Replica fa parte di un cluster di failover, immettere il nome del gestore di Replica Hyper-V. Fare clic su **Avanti**.  

4.  Nel **Specifica parametri di connessione** pagina Replica Hyper-V consente di recuperare automaticamente le impostazioni di autenticazione e la porta configurata per il server di replica. Se i valori non vengono recuperati verificare che il server è configurato come server di replica e che sia registrato in DNS. Se nell'impostazione del tipo richiesto manualmente.  

5.  Nel **Selezione dischi rigidi virtuali di replica** pagina, assicurarsi che i dischi rigidi virtuali che si desidera replicare siano selezionati e deselezionare le caselle di controllo per i dischi rigidi virtuali che si desidera escludere dalla replica. Fare quindi clic su **Avanti**.  

6.  Nel **Configura frequenza di replica** specificare la frequenza con cui le modifiche devono essere sincronizzate dal sito primario con secondario. Fare quindi clic su **Avanti**.  

7.  Nel **Configura i punti di ripristino aggiuntive** pagina, selezionare se si desidera mantenere solo l'ultimo punto di ripristino o per creare altri punti.    Se si desidera ripristinare in modo coerente le applicazioni e carichi di lavoro con i propri VSS Writer è consigliabile selezionare **del servizio Copia Shadow del Volume (VSS) frequenc**y e specificare la frequenza di creazione di snapshot coerenti con l'applicazione. Si noti che il servizio richiedente di Hyper-V VMM deve essere in esecuzione sul server Hyper-V primario e secondario. Fare quindi clic su **Avanti**.  

8.  Nel **scegliere la replica iniziale** pagina, selezionare il metodo di replica iniziale da utilizzare.  L'impostazione predefinita per l'invio della copia iniziale sulla rete consentirà di copiare il file di configurazione della macchina virtuale primario (VMCX) e i file del disco rigido virtuale (VHDX e VHD) selezionati sulla connessione di rete. Verificare la disponibilità di larghezza di banda di rete se prevede di utilizzare questa opzione. Se la macchina virtuale primaria è già configurata nel sito secondario come una macchina virtuale di replica può essere utile selezionare  **utilizzare una macchina virtuale esistente nel server di replica come copia iniziale**. È possibile utilizzare Hyper-V Esporta per esportare la macchina virtuale primaria e importarlo come macchina virtuale di replica nel server secondario. Per le macchine virtuali più grandi o larghezza di banda limitata è possibile scegliere per la replica iniziale sulla rete si verificano in un secondo momento e quindi configurare le fasce orarie o per inviare le informazioni di replica iniziale come supporti non in linea.  

    Se si esegue la replica offline sarà di trasporto la copia iniziale per il server secondario utilizzando un supporto di archiviazione esterna, ad esempio un disco rigido o unità USB. A tale scopo che è necessario connettersi esterno archiviazione al server primario (o nodo proprietario di un cluster) e quindi quando si seleziona Invia copia iniziale usando supporti esterni, che è possibile specificare un percorso locale o su supporti esterni, dove è possibile archiviare la copia iniziale.  Sul sito di replica viene creata una macchina virtuale di segnaposto. Una volta completata la replica iniziale di archiviazione esterna può essere spedito al sito di replica. Il supporto esterno vi si connetterà al server secondario o al nodo proprietario del cluster secondario. Verranno quindi importare la replica iniziale in una posizione specificata e unirlo alla macchina virtuale di segnaposto.  

9. Nel **completamento Abilita replica** pagina, esaminare le informazioni nel riepilogo e quindi fare clic su **Fine.** . Dati della macchina virtuale verranno trasferiti in base alle impostazioni scelte. e viene visualizzata una finestra di dialogo indicante che la replica è stata abilitata.  

10. Se si desidera configurare la replica estesa (concatenata), aprire il server di replica e fare clic sulla macchina virtuale che si desidera replicare. Fare clic su **replica** > **Estendi replica** e specificare le impostazioni di replica.  

## <a name="run-a-failover"></a>Eseguire un failover  
Dopo aver completato questi passaggi di distribuzione dell'ambiente replicato sia in esecuzione. È ora possibile eseguire i failover in base alle esigenze.  

**Failover di test**: se si desidera eseguire il pulsante destro del failover di test per la macchina virtuale primaria e selezionare **replica** > **Test Failover**. Selezionare il punto di ripristino più recente o altri se configurato. Una nuova macchina virtuale di test verrà creata e avviata nel sito secondario. Al termine del test, selezionare **Arresta failover di test** nella macchina virtuale di replica per pulirlo. Si noti che per una macchina virtuale è possibile eseguire solo un failover di test alla volta. [Ulteriori](https://blogs.technet.com/b/virtualization/archive/2012/07/26/types-of-failover-operations-in-hyper-v-replica.aspx).  

**Failover pianificato**: per eseguire una rapida failover pianificato della macchina virtuale primaria e selezionare **replica** > **Failover pianificato**. Failover pianificato esegue controlli dei prerequisiti per garantire una perdita di dati. Consente di verificare che la macchina virtuale primaria viene arrestata prima di iniziare il failover. Dopo la macchina virtuale viene eseguita il failover, avviare la replica delle modifiche al sito primario quando è disponibile. Si noti che per il corretto funzionamento del server primario devono essere configurato per la replica ricevono dal server secondario o dal gestore di Replica Hyper-V nel caso di un cluster primario. Pianificato failover invia l'ultimo set di modifiche rilevate. [Ulteriori](https://blogs.technet.com/b/virtualization/archive/2012/07/31/types-of-failover-operations-in-hyper-v-replica-part-ii-planned-failover.aspx).  

**Failover non pianificato**: per eseguire un failover non pianificato, su macchina virtuale di replica e scegliere **replica** > **Failover non pianificato** da Hyper-V Manager o Gestione cluster di Failover. Se questa opzione è abilitata, è possibile ripristinare dal punto di ripristino più recente o da punti di ripristino precedenti. Dopo il failover, verificare che funzionino come previsto in è stato eseguito il failover macchina virtuale, quindi fare clic su **Complete** nella macchina virtuale di replica. [Ulteriori](https://blogs.technet.com/b/virtualization/archive/2012/08/08/types-of-failover-operations-in-hyper-v-replica-part-iii-unplanned-failover.aspx).  
