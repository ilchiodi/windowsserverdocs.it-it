---
title: Integrazione di servizi di ripristino del sito di Azure
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
ms.openlocfilehash: 85e681cfc19241941773dd94f05ba59759aaf62a
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
#<a name="azure-site-recovery-services-integration"></a>Integrazione di servizi di ripristino del sito di Azure

>Si applica a: Windows Server 2016 Essentials

[Servizi di ripristino del sito Azure](https://azure.microsoft.com/en-us/documentation/services/site-recovery/) è un servizio offerto da abilitare la replica in tempo reale delle macchine virtuali (VM) di Microsoft Azure a un insieme di credenziali di backup in Azure. Nel caso in cui il server o il sito è inattivo a causa di un errore hardware o altri, è possibile il failover in Azure in cui verrà eseguito l'immagine di macchina virtuale archiviata nell'insieme di credenziali di backup come una macchina virtuale in esecuzione in Azure. Combinato con una rete virtuale di Azure, in caso di failover in Azure, client PC che in precedenza connesso al server locale in modo trasparente si connetterà al server in esecuzione in Azure.

Integrazione servizi di ripristino del sito di Azure con Windows Server Essentials viene avviato in configurazione allo stesso modo [rete virtuale di Azure](azure-virtual-network-integration.md). Dal **integrazione di servizi Cloud Microsoft** pagina nel dashboard, fare clic su **integrazione con servizi di ripristino del sito di Azure** a destra del dashboard:

![Screenshot che mostra la scheda iniziare nella Home page del dashboard di Windows Server Essentials. Nella scheda per iniziare, è stata selezionata la sezione servizi e in servizi di integrazione con Microsoft Cloud dashboard indica che il ripristino di Azure è attualmente disabilitato.](media/azure-site-recovery-1.PNG)

Con l'integrazione di rete virtuali di Azure e l'integrazione di Azure Backup, è necessario accedere ad Azure con si Azure account esistente o creare un nuovo account:

![Screenshot che mostra la pagina di accesso In per Microsoft Azure della procedura guidata abilita la replica in Azure. Poiché l'utente non ha ancora connesso in Microsoft Azure, viene visualizzato il pulsante Accedi.](media/azure-site-recovery-2.PNG)

Dopo aver completato l'accesso in Azure, vedrai una schermata che richiederà quali sottoscrizione che vuoi associare il servizio di recupero del sito di Azure, nonché l'area di Azure in cui verrà archiviata e ospitata di una macchina virtuale:

![Screenshot che mostra la pagina di accesso In per Microsoft Azure della procedura guidata abilita la replica in Azure. Poiché l'utente ha eseguito l'accesso a Microsoft Azure, questa pagina fornisce opzioni per la selezione di una sottoscrizione, account di archiviazione e area geografica.](media/azure-site-recovery-3.PNG)

Dopo la sottoscrizione e selezione dell'area geografica, verrà visualizzata una nuova scheda nel **dashboard di Windows Server Essentials** chiamato **Azure Recovery**. Rete l'analisi viene eseguita per identificare ed enumerare i server host supportati (che esegue Windows Server Hyper-V 2012 R2 e versioni successive) nonché le macchine virtuali (guest) in singoli host:

![Screenshot che mostra la pagina di ripristino di Azure del dashboard di Windows Server Essentials. Insieme di macchine virtuali in esecuzione in questi host vengono visualizzati due host Hyper-V. Una macchina virtuale denominata ramh157v01 nell'host H1-RAM-7 è selezionata e la replica in Azure è attualmente disabilitato per questa macchina virtuale.](media/azure-site-recovery-4.PNG)

### <a name="enabling-guest-virtual-machines-for-protection"></a>Abilitazione di macchine virtuali guest per la protezione dati

Seleziona una macchina virtuale presente nella finestra di ripristino di Azure, è possibile fare clic su **Abilita replica in Azure** sul lato destro del dashboard per preparare e copiare la macchina virtuale™ immagine s in Azure:

![Screenshot che mostra la finestra di dialogo di abilitare la replica in Azure. Una barra di stato viene visualizzata come un host viene aggiunto.](media/azure-site-recovery-5.PNG)

Durante questo processo, l'agente del servizio di recupero di Azure del sito viene installato nel server host, un archivio di backup in cui viene creato l'immagine del guest che macchina virtuale verrà archiviati e inizia la replica dell'immagine in Azure. A seconda delle dimensioni della macchina virtuale in corso la replica, il completamento del processo di replica potrebbe richiedere ore o giorni. Fino a quando l'intera immagine di macchina virtuale e i delta più recenti viene replicati in Azure, attività di failover non sono disponibili e la macchina virtuale non è protetto. Una volta completata la replica, la colonna lo stato di protezione nella finestra di ripristino di Azure passerà da **replicare** a **abilitato**:

![Screenshot che mostra la pagina di ripristino di Azure del dashboard di Windows Server Essentials. Insieme di macchine virtuali in esecuzione in questi host vengono visualizzati due host Hyper-V. Una macchina virtuale denominata ramh12v02 nell'host H1-RAM-2 è selezionata e la replica in Azure è attualmente abilitata per la macchina virtuale.](media/azure-site-recovery-6.PNG)

### <a name="failover-of-a-guest-vm-to-azure"></a>Failover di una macchina virtuale in Azure guest

![Screenshot che mostra la VM Failover alla pagina di Azure.](media/azure-site-recovery-7.PNG)

Quando una macchina virtuale che viene protetto ha esito negativo o il server host che viene eseguita la macchina virtuale protetta in avrà esito negativo, eseguire il failover in Azure può essere eseguita per mantenere la continuità aziendale fino al server in locale virtuale host o macchina è riparato e disponibile. Come nella figura precedente, esistono tre tipi di failover che sono supportati con servizi di ripristino del sito di Azure:

-   **Eseguire test di Failover** piano di ripristino di emergenza buona ƒA incorpora la possibilità di simulare una situazione di emergenza per assicurare tempi di inattività minimi in caso di emergenza reale. Un Test del Failover acquisisce l'immagine di macchina virtuale che è stato replicato l'insieme di credenziali di ripristino, si esegue il provisioning come una macchina virtuale in esecuzione in Azure e consente di connettersi al server per testare gli scenari che riguardano l'azienda. Durante un failover di test, la macchina virtuale locale continua a eseguire con senza interruzioni per non causare interruzioni business durante il test di ripristino di emergenza. Una volta completato il Test del Failover, sarà più possibile il test tramite il portale di Azure, che effettua il deprovisioning della macchina virtuale ed elimina il disco rigido virtuale. Durante il failover di test intero, l'immagine di macchina virtuale nell'insieme di credenziali del ripristino continua a essere replicate dalla macchina virtuale in loco come se mai non succede nulla.

-   **Failover non pianificato** ƒAn failover non pianificato si verifica quando si verifica un errore effettivo con il server host protetto o di una macchina virtuale in esecuzione nel server host. Il failover viene attivato manualmente da uno dei due dashboard di Windows Server Essentials, o se il server è il server che Windows Server Essentials è in esecuzione, può essere attivato direttamente dal portale di Azure. In questo caso, il Failover non pianificato acquisisce l'immagine di macchina virtuale che è stato replicato l'insieme di credenziali di ripristino, si esegue il provisioning come una macchina virtuale in esecuzione in Azure e consente di connettersi al server per testare gli scenari che riguardano l'azienda. Quando il server viene ripristinato in locale, è possibile attivare un manuale failback dal portale di Azure che verrà quindi copiare l'immagine di macchina virtuale indietro verso il basso per il server locale.

-   **Failover pianificato** ƒA pianificato failover è un'azione che può essere eseguita nel caso in cui le attività pianificate, ad esempio manutenzione hardware, devono avvenire che richiederà il server verso il basso. In caso di un failover pianificato, lo stesso processo avviene per quanto riguarda il provisioning dell'immagine di macchina virtuale replicata in Azure. Tuttavia, prima in questo modo, il server locale viene arrestato in modo ordinato per garantire che tutte le modifiche vengono replicate in Azure prima dell'arresto. Una volta completata la manutenzione pianificata, è possibile attivare manualmente un failback dal dashboard di Windows Server Essentials o il portale di Azure, che potrebbe portare il server locale e quindi annullare il provisioning della macchina virtuale in Azure ed elimina il. File di disco rigido virtuale. La replica dalla macchina virtuale locale in Azure quindi continuerebbe a verificarsi di nuovo come di consueto.

In uno dei tre casi, quando una macchina virtuale viene eseguita il failover in Azure, il dashboard di Windows Server Essentials mostrerà la nuova macchina virtuale in Azure in esecuzione come nella figura seguente.

![Screenshot che mostra la pagina di ripristino di Azure del dashboard di Windows Server Essentials. La replica in Azure è stata abilitata per un host denominato Essentials e una macchina virtuale denominata Test Essentials in esecuzione in Azure indica che l'host ha eseguito il failover in Azure.](media/azure-site-recovery-8.PNG)

<a name="see-also"></a>Vedere anche
--------
[Introduzione a Windows Server Essentials](get-started.md)
