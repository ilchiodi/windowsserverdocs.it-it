---
title: Utilizzare live migration senza Clustering di Failover per spostare una macchina virtuale
description: Fornisce i prerequisiti e istruzioni per eseguire una migrazione in tempo reale in un ambiente autonomo.
ms.prod: windows-server-threshold
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 75c32e42-97f7-48df-aac9-1d82d34825e1
author: KBDAzure
ms.author: kathydav
ms.date: 01/17/2017
ms.openlocfilehash: 9be61fbc860e9d8c5cbc020d6dd4082722e32509
ms.sourcegitcommit: 6ef4986391607bb28593852d06cc6645e548a4b3
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/07/2019
ms.locfileid: "66812093"
---
# <a name="use-live-migration-without-failover-clustering-to-move-a-virtual-machine"></a>Utilizzare live migration senza Clustering di Failover per spostare una macchina virtuale

>Si applica a: Windows Server 2016

In questo articolo viene descritto come spostare una macchina virtuale eseguendo una migrazione in tempo reale senza utilizzare il Clustering di Failover. Live migration sposta le macchine virtuali in esecuzione tra gli host Hyper-V senza tempi di inattività evidenti.   
  
Per poter eseguire questa operazione, è necessario:   

- Un account utente che è un membro del gruppo amministratori Hyper-V locale o il gruppo Administrators nel computer di origine e di destinazione. 
  
- Il ruolo Hyper-V in Windows Server 2016 o Windows Server 2012 R2 installato nel server di origine e destinazione e configurazione per migrazioni in tempo reale. È possibile eseguire una migrazione in tempo reale tra host che eseguono Windows Server 2016 e Windows Server 2012 R2, se la macchina virtuale sia almeno la versione 5.

    Per istruzioni sull'aggiornamento di versione, vedere [versione aggiornamento macchina virtuale in Hyper-V in Windows 10 o Windows Server 2016](../deploy/Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md). Per istruzioni sull'installazione, vedere [configurare gli host per live migration](../deploy/Set-up-hosts-for-live-migration-without-Failover-Clustering.md).

- Gli strumenti di gestione di Hyper-V installati in un computer che eseguono Windows Server 2016 o Windows 10, a meno che gli strumenti vengono installati in server di origine o destinazione e si eseguirà li da lì.  
   
## <a name="use-hyper-v-manager-to-move-a-running-virtual-machine"></a>Gestione di Hyper-V consente di spostare una macchina virtuale in esecuzione  
  
1.  Aprire la console di gestione di Hyper-V. (Da Server Manager, fare clic su **Strumenti** >>**Hyper-V Manager**.)  
  
2.  Nel riquadro di spostamento, selezionare uno dei server. (Se non è elencato, fare doppio clic su **Gestione di Hyper-V**, fare clic su **Connetti al Server**, digitare il nome del server e fare clic su **OK**. Ripetere il passaggio per aggiungere più server.)  
  
3.  Dal **macchine virtuali** riquadro, fare clic sulla macchina virtuale e quindi fare clic su **spostare**. Verrà visualizzata la procedura guidata sposta. 
  
4.  Utilizzare le pagine della procedura guidata per scegliere il tipo di spostamento, server di destinazione e le opzioni.
  
5.  Nella pagina **Riepilogo** controllare le opzioni selezionate e quindi fare clic su **Fine**.  

## <a name="use-windows-powershell-to-move-a-running-virtual-machine"></a>Utilizzare Windows PowerShell per spostare una macchina virtuale in esecuzione
  
Nell'esempio seguente utilizza il cmdlet Move-VM per spostare una macchina virtuale denominata *LMTest* in un server di destinazione denominato *TestServer02* e consente di spostare i dischi rigidi virtuali e altri file, tali checkpoint e i file di Paging intelligente, per il *D:\LMTest* directory nel server di destinazione.  
  
```  
PS C:\> Move-VM LMTest TestServer02 -IncludeStorage -DestinationStoragePath D:\LMTest  
```  
  
## <a name="troubleshooting"></a>Risoluzione dei problemi

### <a name="failed-to-establish-a-connection"></a>Impossibile stabilire una connessione 

Se è stata impostata una delega vincolata, è necessario accedere al server di origine prima di poter spostare una macchina virtuale. Se non si esegue questa operazione, il tentativo di autenticazione ha esito negativo, si verifica un errore, e viene visualizzato questo messaggio:  
  
"Operazione di migrazione di macchina virtuale non riuscita all'origine della migrazione.  
Non è riuscito a stabilire una connessione all'host *nome computer*: Le credenziali non sono disponibili nel pacchetto di protezione 0x8009030E."
  
 Per risolvere questo problema, accedere al server di origine e lo spostamento di provare nuovamente. Per evitare la necessità di accedere a un server di origine prima di eseguire una migrazione in tempo reale, configurare la delega vincolata. È necessario che le credenziali di amministratore di dominio per configurare la delega vincolata. Per istruzioni, vedere [configurare gli host per live migration](../deploy/Set-up-hosts-for-live-migration-without-Failover-Clustering.md). 
 
 ### <a name="failed-because-the-host-hardware-isnt-compatible"></a>Non è riuscita perché l'hardware host non è compatibile
 
 Se una macchina virtuale non dispone di compatibilità processore accesa e presenta uno o più snapshot, il passaggio ha esito negativo se l'host dispone di versioni diverse di processore. Si verifica un errore e viene visualizzato questo messaggio:
 
**Impossibile spostare la macchina virtuale nel computer di destinazione. L'hardware nel computer di destinazione non è compatibile con i requisiti hardware della macchina virtuale.**
 
 Per risolvere questo problema, arrestare la macchina virtuale e attivare l'impostazione di compatibilità del processore.
 
1. Di gestione di Hyper-V nel **macchine virtuali** riquadro, fare clic sulla macchina virtuale e fare clic su impostazioni.
2. Nel riquadro di spostamento, espandere **processori** e fare clic su **compatibilità**.
3. Controllare **migrazione a un computer con una versione diversa del processore**.
4. Fare clic su **OK**.
 
   Per utilizzare Windows PowerShell, utilizzare il [Set-VMProcessor](https://technet.microsoft.com/library/hh848533.aspx) cmdlet:
 
   ```
   PS C:\> Set-VMProcessor TestVM -CompatibilityForMigrationEnabled $true
   ```