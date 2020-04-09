---
title: Il numero di esecuzione o macchine virtuali configurate devono essere all'interno di limiti supportati
description: Vengono fornite istruzioni per risolvere il problema segnalato da questa regola di Best Practices Analyzer.
ms.prod: windows-server
manager: dongill
ms.technology: compute-hyper-v
ms.author: kathydav
ms.topic: article
ms.assetid: 9d3c4aa3-8416-46ec-a253-26dc98088d7b
author: kbdazure
ms.date: 8/16/2016
ms.openlocfilehash: 49013d6a4c9dda6e79d6a803bae0f5641d826817
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854624"
---
# <a name="the-number-of-running-or-configured-virtual-machines-must-be-within-supported-limits"></a>Il numero di esecuzione o macchine virtuali configurate devono essere all'interno di limiti supportati

>Si applica a: Windows Server 2016

Per altre informazioni sulle procedure consigliate e sulle analisi, vedere [Eseguire analisi di Best Practice Analyzer e gestire i risultati delle analisi](https://go.microsoft.com/fwlink/p/?LinkID=223177).  
  
|Proprietà|Dettagli|  
|-|-|  
|**Sistema operativo**|Windows Server 2016|  
|**Prodotto/funzionalità**|Hyper-V|  
|**Gravità**|Errore  
|**Categoria**|Configurazione|  
  
Nelle sezioni seguenti, corsivo indica il testo visualizzato nello strumento Analizzatore procedure consigliate per questo problema.  
  
## <a name="issue"></a>Problema  
*Più macchine virtuali sono in esecuzione o configurate rispetto a quelle supportate.*  
  
## <a name="impact"></a>Impatto  
*Microsoft non supporta il numero corrente di macchine virtuali in esecuzione o configurate in questo server.*  
  
## <a name="resolution"></a>Risoluzione  
*Spostare una o più macchine virtuali in un altro server.*  
  
Per informazioni dettagliate sulle configurazioni supportate massime per Hyper-V, ad esempio il numero di macchine virtuali in esecuzione, vedere [pianificare la scalabilità di Hyper-V in Windows Server 2016](../plan/Plan-for-Hyper-V-scalability-in-Windows-Server-2016.md).  
  
Per spostare una macchina virtuale in un altro server, è possibile:  
  
- Esportare la macchina virtuale dal server corrente e quindi importarlo in un nuovo server come descritto di seguito.   
- Eseguire una migrazione in tempo reale:   
    - Se il server appartiene a un cluster di failover, utilizzare gli strumenti forniti con la funzionalità Clustering di Failover. Per istruzioni, vedere [Live Migration, migrazione rapida o spostare una macchina virtuale dal nodo a nodo](https://go.microsoft.com/fwlink/?LinkID=181519).  
    - Se si tratta di un server autonomo, vedere le istruzioni in [configurare Live Migration e migrazione delle macchine virtuali senza Clustering di Failover](https://technet.microsoft.com//library/jj134199(v=ws.11).aspx)  
  
### <a name="to-export-a-virtual-machine"></a>Per esportare una macchina virtuale  
  
   > [!IMPORTANT]  
   > Se l'host Hyper-V per l'esportazione da appartiene a un dominio e si desidera archiviare i file esportati in una posizione remota, l'host Hyper-V deve essere configurato per la delega vincolata. Una posizione remota potrebbe essere una cartella di rete condivisa o una cartella nell'host in che si sta importando. La delega vincolata consente l'account computer dell'host Hyper-V di fornire le credenziali delegate per il servizio Common Internet File System (CIFS) al computer remoto. Per istruzioni sulla configurazione della delega vincolata, vedere la sezione dopo l'esportazione e importazione istruzioni, di seguito.  
  
1.  Aprire la console di gestione di Hyper-V. Fare clic sul pulsante **Start**, scegliere **Strumenti di amministrazione** e quindi **Console di gestione di Hyper-V**.  
  
2.  Nel riquadro dei risultati, sotto **macchine virtuali**, fare doppio clic su una macchina virtuale e quindi fare clic su **esportare**.  
  
3.  Nel **esportare una macchina virtuale** la finestra di dialogo, digitare o cercare un percorso con spazio sufficiente per archiviare tutte le risorse della macchina virtuale. Quando si esporta una macchina virtuale, tutti i dischi rigidi virtuali (file con estensione vhd o vhdx), i checkpoint (file con estensione avhd) e i file di stato salvati associati alla macchina virtuale vengono copiati nella cartella specificata.  
  
4.  Fare clic su **Esporta**.  
  
Dopo avere esportato le macchine virtuali, importare le macchine virtuali in altro server.  
  
### <a name="to-import-a-virtual-machine-to-another-server"></a>Per importare una macchina virtuale in un altro server  
  
1.  Connettersi al server che esegue Hyper-V e aprire Gestione Hyper-V.  
  
2.  Nel **azione** riquadro, fare clic su **Importa macchina virtuale**.  
  
3.  Nel **Importa macchina virtuale** finestra di dialogo specificare il percorso in cui è stato esportato la macchina virtuale. A meno che non si desidera reimportare la macchina virtuale, lascia invariate le impostazioni di importazione.  
  
4.  Fare clic su **Importa**.  
  
### <a name="to-configure-constrained-delegation"></a>Per configurare la delega vincolata  
  
L'appartenenza di **gli amministratori di dominio** gruppo è necessario per completare questa procedura.  
  
1.  In un computer con la funzionalità Strumenti di servizi di dominio di Active Directory installata, in **Strumenti di amministrazione**, aprire **Active Directory Users and Computers**, quindi passare all'account del computer per il computer che esegue Hyper-V.  
  
    > [!NOTE]  
    > Se l'opzione **Utenti e computer di Active Directory** non è elencata, installare la funzione Strumenti per Servizi di dominio Active Directory. Per istruzioni, vedere [installazione di strumenti di amministrazione remota del server per servizi di dominio Active Directory](https://go.microsoft.com/fwlink/?LinkId=140463) (https://go.microsoft.com/fwlink/?LinkId=140463).  
  
2.  Fare doppio clic su account computer per il computer che esegue Hyper-V e quindi fare clic su **proprietà**.  
  
3.  Nella scheda **Delega** fare clic su **Computer attendibile per la delega solo ai servizi specificati**, quindi su **Utilizza un qualsiasi protocollo di autenticazione**.  
  
4.  Per consentire all'account di computer Hyper-V presentare credenziali delegate per il computer remoto:  
  
    1.  Fare clic su **Add**.  
  
    2.  Nel **aggiungere servizi** la finestra di dialogo, fare clic su **utenti o computer**, selezionare il computer remoto e quindi fare clic su **OK**.  
  
    3.  Nel **servizi disponibili** elenco, selezionare il **cifs** protocol (anche noto come il protocollo Server Message Block (SMB)) e quindi fare clic su **Aggiungi**.  
  
  
  


