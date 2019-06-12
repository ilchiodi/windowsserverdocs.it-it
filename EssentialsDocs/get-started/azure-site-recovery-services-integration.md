---
title: Integrazione dei servizi Azure Site Recovery
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/01/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 262701a6-8a97-4c4e-bfbf-9f8007c308d6
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: e9dabc1b986fd86b5ee3efc6022023b331046250
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433957"
---
# <a name="azure-site-recovery-services-integration"></a>Integrazione dei servizi Azure Site Recovery

>Si applica a: Windows Server 2016 Essentials

[Servizi di Azure Site Recovery](https://docs.microsoft.com/azure/site-recovery/) è un servizio offerto da Microsoft Azure per consentire la replica in tempo reale delle macchine virtuali (VM) a un insieme di credenziali di backup in Azure. Nel caso in cui il server o il sito è inattivo a causa di un errore hardware o altri, è possibile effettuare il failover in Azure in cui l'immagine di macchina virtuale archiviata nell'insieme di credenziali di backup provisioning verrà eseguito come macchina virtuale in esecuzione in Azure. Combinata con una rete virtuale di Azure, in caso di failover in Azure, client PC che precedentemente connesso al server locale in modo trasparente si connetterà al server in esecuzione in Azure.

L'integrazione di servizi di Azure Site Recovery con Windows Server Essentials viene avviato nello stesso modo come configurazione [rete virtuale di Azure](azure-virtual-network-integration.md). Dal **integrazione di servizi Cloud di Microsoft** nel dashboard fare clic su **integra con servizi di Azure Site Recovery** a destra del dashboard:

![Screenshot che mostra la scheda iniziare nella Home page del dashboard di Windows Server Essentials. Nella scheda, iniziare a usare la sezione servizi è stata selezionata e il dashboard indica in integrazione con servizi Cloud Microsoft che Azure Recovery è attualmente disabilitata.](media/azure-site-recovery-1.PNG)

Come con integrazione rete virtuale di Azure e l'integrazione di Backup di Azure, è necessario accedere ad Azure con è account Azure esistente o creare un nuovo account:

![Screenshot che mostra la pagina di accesso a Microsoft Azure della procedura guidata abilita la replica da Azure ad. Il pulsante di accesso viene visualizzato perché l'utente non ha ancora effettuata in Microsoft Azure.](media/azure-site-recovery-2.PNG)

Dopo aver completato la registrazione in Azure, si noterà una schermata di cui è richiesta la sottoscrizione a cui si desidera associare il servizio Azure Site Recovery, nonché l'area di Azure in cui verrà archiviata e ospitata la macchina virtuale:

![Screenshot che mostra la pagina di accesso a Microsoft Azure della procedura guidata abilita la replica da Azure ad. Poiché l'utente ha effettuato l'accesso a Microsoft Azure, questa pagina fornisce opzioni per selezionare una sottoscrizione, un account di archiviazione e area.](media/azure-site-recovery-3.PNG)

Dopo la sottoscrizione e la selezione dell'area, verrà visualizzata una nuova scheda nel **dashboard di Windows Server Essentials** chiamato **Azure Recovery**. L'analisi di rete viene eseguita per identificare ed enumerare i server host supportate (che esegue Windows Server Hyper-V 2012 R2 e versioni successive), nonché le macchine virtuali (guest) sotto i singoli host:

![Screenshot che mostra la pagina di ripristino di Azure del dashboard di Windows Server Essentials. Due host Hyper-V vengono visualizzate insieme alle macchine virtuali in esecuzione su tali host. Una macchina virtuale denominata ramh157v01 nell'host di RAM-H1-7 sia selezionata e la replica in Azure è attualmente disabilitato per questa macchina virtuale.](media/azure-site-recovery-4.PNG)

### <a name="enabling-guest-virtual-machines-for-protection"></a>Abilitare le macchine virtuali guest per la protezione

Seleziona una macchina virtuale presente nella finestra di ripristino di Azure, è possibile fare clic su **abilitare la replica in Azure** a destra del dashboard per preparare e copiare la macchina virtuale™ immagine s in Azure:

![Screenshot che mostra la finestra di dialogo di abilitare la replica da Azure ad. Viene visualizzata una barra di stato di avanzamento durante l'aggiunta di un host.](media/azure-site-recovery-5.PNG)

Durante questo processo, l'agente del servizio Azure Site Recovery viene installato nel server host, un archivio di backup in cui viene creata l'immagine del guest che macchina virtuale verrà archiviata e viene avviata la replica dell'immagine da Azure. A seconda delle dimensioni della macchina virtuale in corso la replica, il completamento del processo di replica potrebbe richiedere ore o giorni. Fino a quando l'intera immagine di macchina virtuale e i delta più recenti vengono replicati in Azure, le attività di failover non sono disponibili e la macchina virtuale non è protetta. Una volta completata la replica, la colonna stato di protezione nella finestra di ripristino di Azure verrà modificato da **replicare** al **abilitato**:

![Screenshot che mostra la pagina di ripristino di Azure del dashboard di Windows Server Essentials. Due host Hyper-V vengono visualizzate insieme alle macchine virtuali in esecuzione su tali host. Una macchina virtuale denominata ramh12v02 nell'host di RAM-H1-2 è selezionata e la replica in Azure è attualmente abilitata per questa macchina virtuale.](media/azure-site-recovery-6.PNG)

### <a name="failover-of-a-guest-vm-to-azure"></a>Failover di un guest macchina virtuale in Azure

![Screenshot che mostra di Failover di VM alla pagina di Azure.](media/azure-site-recovery-7.PNG)

Quando una macchina virtuale protetto si verifica un errore o il server host che la macchina virtuale protetta in esecuzione sul ha esito negativo, eseguire il failover in Azure possono essere eseguita per mantenere la continuità fino a on premises macchina o un host server virtuale è corretto e disponibile. Come riporta la figura precedente, esistono tre tipi di failover supportate, i servizi di Azure Site Recovery:

-   **Failover di test** piano di ripristino di emergenza valida ƒA incorpora la possibilità di simulare una situazione di emergenza per garantire tempi di inattività minimi in caso di emergenza reale. Un Failover di Test accetta l'immagine di macchina virtuale che è stata replicata nell'insieme di credenziali di ripristino, viene eseguito il provisioning come una macchina virtuale in esecuzione in Azure e consente di connettersi al server per testare gli scenari che si applicano all'azienda. Durante un failover di test, la macchina virtuale locale continua a eseguire con delle interruzioni tale da non interrompano le attività aziendali durante il test di ripristino di emergenza. Una volta completato il Failover di Test, sarà più possibile il test tramite il portale di Azure, che effettua il deprovisioning della macchina virtuale ed elimina il disco rigido virtuale. Durante il failover di test intera immagine di macchina virtuale nell'insieme di credenziali di ripristino continua per la replica dalla macchina virtuale sul posto come se dovesse sia successo niente.

-   **Failover non pianificato** ƒAn failover non pianificato si verifica quando si verifica un errore effettivo con il server protetto host o macchina virtuale in esecuzione nel server host. Il failover viene attivato manualmente dal dashboard di Windows Server Essentials, o se il server stesso è il server che Windows Server Essentials è in esecuzione, può essere attivato direttamente dal portale di Azure. In questo caso, il Failover non pianificato richiede l'immagine di macchina virtuale che è stata replicata nell'insieme di credenziali di ripristino, viene eseguito il provisioning come una macchina virtuale in esecuzione in Azure e consente di connettersi al server per testare gli scenari che si applicano all'azienda. Quando viene ripristinato il server in locale, il failback manuale può essere attivato dal portale di Azure che è possibile copiare l'immagine di macchina virtuale tornando a server locale.

-   **Failover pianificato** ƒA pianificato failover è un'azione che può essere eseguita nel caso in cui le attività pianificate, ad esempio la manutenzione hardware, devono essere eseguita che richiederebbe server verso il basso. In caso di un failover pianificato, lo stesso processo viene eseguito per quanto riguarda il provisioning dell'immagine di macchina virtuale replicata in Azure. Tuttavia, prima in questo modo, il server locale viene arrestato in modo ordinato per assicurarsi che tutte le modifiche vengono replicate in Azure prima dell'arresto. Al termine della manutenzione pianificata, è possibile attivare manualmente un failback dal dashboard di Windows Server Essentials, o il portale di Azure, che verrebbe portare verticalmente il server locale e quindi eseguire il deprovisioning di provisioning della macchina virtuale in Azure e delete di. File di disco rigido virtuale. La replica dalla macchina virtuale da sito locale ad Azure quindi continuerà a verificarsi di nuovo come di consueto.

In uno qualsiasi dei tre casi precedenti, quando una macchina virtuale viene eseguita il failover in Azure, il dashboard di Windows Server Essentials mostrerà la nuova macchina virtuale in Azure in esecuzione come illustrato nella figura seguente.

![Screenshot che mostra la pagina di ripristino di Azure del dashboard di Windows Server Essentials. Replica in Azure è stata abilitata per un host denominato Essentials e una macchina virtuale denominato Essentials e Test in esecuzione in Azure indica che l'host ha eseguito il failover in Azure.](media/azure-site-recovery-8.PNG)

<a name="see-also"></a>Vedere anche
--------
[Introduzione a Windows Server Essentials](get-started.md)
