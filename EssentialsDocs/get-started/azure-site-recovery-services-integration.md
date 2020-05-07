---
title: Integrazione dei servizi Azure Site Recovery
description: Viene descritto come usare Windows Server Essentials
ms.date: 10/01/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 262701a6-8a97-4c4e-bfbf-9f8007c308d6
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: b947ca49a82c18fd7a6c1da71b1e4b43ea741b41
ms.sourcegitcommit: f247065941508b913c31828944978d3e721e2110
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/07/2020
ms.locfileid: "82876416"
---
# <a name="azure-site-recovery-services-integration"></a>Integrazione dei servizi Azure Site Recovery 

>Si applica a: Windows Server 2016 Essentials

[Azure Site Recovery Services](https://docs.microsoft.com/azure/site-recovery/) è un servizio offerto da Microsoft Azure abilitando la replica in tempo reale delle macchine virtuali (VM) in un insieme di credenziali per il backup in Azure. Se il server o il sito è inattivo a causa di un errore hardware o di altro genere, è possibile eseguire il failover in Azure in cui l'immagine di macchina virtuale archiviata nell'insieme di credenziali di backup verrà sottoposta a provisioning come macchina virtuale in esecuzione in Azure. In combinazione con una rete virtuale di Azure, in caso di failover in Azure, i computer client che in precedenza erano connessi al server locale si connetteranno in modo trasparente al server in esecuzione in Azure.

L'integrazione di Azure Site Recovery Services con Windows Server Essentials viene avviata allo stesso modo della configurazione della [rete virtuale di Azure](azure-virtual-network-integration.md). Nella pagina di **integrazione di Microsoft Cloud Services** del dashboard fare clic su **integra con Azure Site Recovery Services** a destra del dashboard:

![Screenshot che mostra la scheda attività iniziali nella Home page del dashboard di Windows Server Essentials. Nella scheda attività iniziali, è stata selezionata la sezione servizi e il dashboard indica in integrazione Microsoft Cloud Services che il ripristino di Azure è attualmente disabilitato.](media/azure-site-recovery-1.PNG)

Come per l'integrazione della rete virtuale di Azure e l'integrazione di backup di Azure, è necessario accedere ad Azure con l'account Azure esistente o creare un nuovo account:

![Screenshot che mostra la pagina Accedi a Microsoft Azure della procedura guidata Abilita la replica in Azure. Il pulsante Accedi viene visualizzato perché l'utente non ha ancora effettuato l'accesso Microsoft Azure.](media/azure-site-recovery-2.PNG)

Dopo aver eseguito l'accesso ad Azure, verrà visualizzata una schermata in cui verrà chiesto quale sottoscrizione si desidera associare al servizio Azure Site Recovery, nonché l'area di Azure in cui verrà archiviata e ospitata la macchina virtuale:

![Screenshot che mostra la pagina Accedi a Microsoft Azure della procedura guidata Abilita la replica in Azure. Poiché l'utente ha effettuato l'accesso Microsoft Azure, in questa pagina sono disponibili opzioni per la selezione di una sottoscrizione, di un account di archiviazione e di un'area.](media/azure-site-recovery-3.PNG)

Dopo la selezione di sottoscrizione e area, viene visualizzata una nuova scheda nel **dashboard di Windows Server Essentials** denominata **ripristino di Azure**. L'analisi della rete viene eseguita per identificare ed enumerare i server host supportati (che eseguono Windows Server Hyper-V 2012 R2 e versioni successive), nonché le macchine virtuali (Guest) nei singoli host:

![Screenshot che mostra la pagina di ripristino di Azure del dashboard di Windows Server Essentials. Vengono visualizzati due host Hyper-V insieme alle macchine virtuali in esecuzione in questi host. È stata selezionata una macchina virtuale denominata ramh157v01 nell'host RAM-H1-7 e la replica in Azure è attualmente disabilitata per questa macchina virtuale.](media/azure-site-recovery-4.PNG)

### <a name="enabling-guest-virtual-machines-for-protection"></a>Abilitazione di macchine virtuali guest per la protezione

Quando si seleziona una macchina virtuale presente nella finestra di ripristino di Azure, è possibile fare clic su **Abilita replica in Azure** sul lato destro del dashboard per preparare e copiare l'immagine &trade;della macchina virtuale in Azure:

![Screenshot che mostra la finestra di dialogo Abilita replica in Azure. Viene visualizzato un indicatore di stato durante l'aggiunta di un host.](media/azure-site-recovery-5.PNG)

Durante questo processo, il Azure Site Recovery agente del servizio è installato nel server host, viene creato un insieme di credenziali di backup in cui verrà archiviata l'immagine della VM guest e viene avviata la replica dell'immagine in Azure. A seconda delle dimensioni della macchina virtuale replicata, il completamento del processo di replica potrebbe richiedere ore o giorni. Fino a quando l'intera immagine di macchina virtuale e i Delta più recenti non vengono replicati in Azure, le attività di failover non sono disponibili e la macchina virtuale non è protetta. Una volta completata la replica, la colonna stato protezione nella finestra ripristino di Azure passerà dalla **replica** a **abilitata**:

![Screenshot che mostra la pagina di ripristino di Azure del dashboard di Windows Server Essentials. Vengono visualizzati due host Hyper-V insieme alle macchine virtuali in esecuzione in questi host. È stata selezionata una macchina virtuale denominata ramh12v02 nell'host RAM-H1-2 e la replica in Azure è attualmente abilitata per questa macchina virtuale.](media/azure-site-recovery-6.PNG)

### <a name="failover-of-a-guest-vm-to-azure"></a>Failover di una macchina virtuale guest in Azure

![Screenshot che mostra la macchina virtuale di failover in Azure.](media/azure-site-recovery-7.PNG)

Quando una macchina virtuale protetta non riesce o il server host in cui viene eseguita la macchina virtuale protetta non riesce, è possibile eseguire il failover in Azure per mantenere la continuità aziendale fino a quando la macchina virtuale o il server host locale non viene ripristinato e disponibile. Come illustrato nella figura precedente, sono disponibili tre tipi di failover supportati con Azure Site Recovery Services:

-   Il piano di ripristino di emergenza ƒA del **failover di test** include la possibilità di simulare un'emergenza per garantire un tempo di inattività minimo in caso di emergenza reale. Un failover di test prende l'immagine di macchina virtuale replicata nell'insieme di credenziali di ripristino, la esegue il provisioning come macchina virtuale in esecuzione in Azure e consente di connettersi al server per testare gli scenari che si applicano all'azienda. Durante un failover di test, la macchina virtuale locale continua a funzionare senza interruzioni per evitare interruzioni aziendali durante il test di ripristino di emergenza. Al termine del failover di test, il test verrà interrotto tramite il portale di Azure, che esegue il deprovisioning della macchina virtuale ed Elimina il disco rigido virtuale. Durante l'intero failover di test, l'immagine di macchina virtuale nell'insieme di credenziali di ripristino continua a essere replicata dalla VM sul sito come se non fosse mai avvenuta.

-   Failover non **pianificato** ƒAn il failover non pianificato viene eseguito quando si verifica un errore effettivo con il server host protetto o la macchina virtuale in esecuzione nel server host. Il failover viene attivato manualmente dal dashboard di Windows Server Essentials o se il server in cui è in esecuzione Windows Server Essentials non è in esecuzione, può essere attivato direttamente dal portale di Azure. In questo caso, il failover non pianificato acquisisce l'immagine di macchina virtuale che è stata replicata nell'insieme di credenziali di ripristino, ne esegue il provisioning come macchina virtuale in esecuzione in Azure e consente di connettersi al server per testare gli scenari applicabili all'azienda. Quando il server viene ripristinato in locale, un failback manuale può essere attivato dal portale di Azure, che quindi copia l'immagine della macchina virtuale nel server locale.

-   Il failover **pianificato** ƒA il failover pianificato è un'azione che può essere eseguita nel caso in cui sia necessario eseguire attività pianificate, ad esempio la manutenzione HW, che potrebbero portare il server inattivo. In caso di failover pianificato, lo stesso processo si verifica per quanto riguarda il provisioning dell'immagine di macchina virtuale replicata in Azure. Prima di procedere, tuttavia, il server locale viene arrestato in modo ordinato per assicurarsi che tutte le modifiche vengano replicate in Azure prima dell'arresto. Una volta completata la manutenzione pianificata, è possibile attivare manualmente un failback dal dashboard di Windows Server Essentials o dalla portale di Azure, che consente di portare il server locale e quindi di eseguire il deprovisioning della macchina virtuale in Azure ed eliminare il. File VHD. La replica dalla macchina virtuale locale ad Azure continuerà a ripetersi normalmente.

In uno dei tre casi precedenti, quando viene eseguito il failover di una macchina virtuale in Azure, il dashboard di Windows Server Essentials visualizzerà la nuova macchina virtuale in Azure, come illustrato nella figura seguente.

![Screenshot che mostra la pagina di ripristino di Azure del dashboard di Windows Server Essentials. La replica in Azure è stata abilitata per un host denominato Essentials e una macchina virtuale denominata Essentials-test in esecuzione in Azure indica che l'host ha eseguito il failover in Azure.](media/azure-site-recovery-8.PNG)

<a name="see-also"></a>Vedere anche
--------
[Introduzione a Windows Server Essentials](get-started.md)
