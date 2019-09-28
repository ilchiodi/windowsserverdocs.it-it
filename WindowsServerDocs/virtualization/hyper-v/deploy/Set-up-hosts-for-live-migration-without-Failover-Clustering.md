---
title: Configurare gli host per live migration senza Clustering di Failover
description: Vengono fornite istruzioni per la configurazione della migrazione in tempo reale in un ambiente non cluster
ms.prod: windows-server
ms.service: na
manager: dongill
ms.technology: compute-hyper-v
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: b5e3c405-cb76-4ff2-8042-c2284448c435
author: KBDAzure
ms.author: kathydav
ms.date: 9/30/2016
ms.openlocfilehash: ad5c3da632df3afc4c7b22c4e1c79b3b92ea7508
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71364287"
---
# <a name="set-up-hosts-for-live-migration-without-failover-clustering"></a>Configurare gli host per live migration senza Clustering di Failover

>Si applica a: Windows Server 2016, Microsoft Hyper-V Server 2016, Windows Server 2019 Microsoft Hyper-V Server 2019

In questo articolo viene descritto come impostare l'host non in cluster in modo è possibile eseguire live migrazioni tra di essi. Utilizzare queste istruzioni se è stata impostata migrazione in tempo reale durante l'installazione di Hyper-V o se si desidera modificare le impostazioni. Per configurare il cluster host, utilizzare gli strumenti per Clustering di Failover.  
  
## <a name="requirements-for-setting-up-live-migration"></a>Requisiti per configurare la funzionalità live migration  
  
Per configurare gli host non cluster per live migration, è necessario:   
  
-  Un account utente con autorizzazione per eseguire i vari passaggi. L'appartenenza al gruppo amministratori Hyper-V locale o il gruppo Administrators nel computer di origine e di destinazione soddisfa questo requisito, a meno che non si configura la delega vincolata. L'appartenenza al gruppo di amministratori di dominio è necessario configurare la delega vincolata.  
  
- Il ruolo Hyper-V in Windows Server 2016 o Windows Server 2012 R2 installato nel server di origine e in quello di destinazione. È possibile eseguire una migrazione in tempo reale tra host che eseguono Windows Server 2016 e Windows Server 2012 R2 se la macchina virtuale è almeno la versione 5. <br>Per istruzioni sull'aggiornamento della versione, vedere [aggiornare la versione della macchina virtuale in Hyper-V in Windows 10 o Windows Server 2016](Upgrade-virtual-machine-version-in-Hyper-V-on-Windows-or-Windows-Server.md). Per le istruzioni di installazione, vedere [installare il ruolo Hyper-V in Windows Server](../get-started/Install-the-Hyper-V-role-on-Windows-Server.md).  
  
- Computer di origine e di destinazione che appartengono allo stesso dominio Active Directory oppure appartengono a domini di trust reciproca.    
- Gli strumenti di gestione di Hyper-V installati in un computer che eseguono Windows Server 2016 o Windows 10, a meno che gli strumenti vengono installati in server di origine o destinazione ed è necessario eseguire gli strumenti dal server.  
  
## <a name="consider-options-for-authentication-and-networking"></a>Prendere in considerazione le opzioni per l'autenticazione e di rete  
  
Considerare come si desidera impostare le seguenti opzioni:  
  
-  **Autenticazione**: Quale protocollo verrà usato per autenticare il traffico di migrazione in tempo reale tra i server di origine e di destinazione? La scelta determina se è necessario accedere al server di origine prima di avviare una migrazione in tempo reale:   
   - Kerberos consente di evitare di dover effettuare l'accesso al server, ma richiede la delega vincolata da impostare. Per istruzioni, vedere di seguito.  
   - CredSSP consente di evitare di configurare la delega vincolata, ma è necessario che accedere al server di origine. È possibile farlo tramite una sessione di console locale, una sessione Desktop remoto o una sessione remota di Windows PowerShell.  
  
      CredSPP richiede una firma situazioni in cui potrebbe non essere evidente. Ad esempio, se si accede a TestServer01 per spostare una macchina virtuale in TestServer02 e quindi si desidera riportare la macchina virtuale in TestServer01, è necessario accedere a TestServer02 prima di tentare di riportare la macchina virtuale in TestServer01. Se non si esegue questa operazione, il tentativo di autenticazione ha esito negativo, si verifica un errore, e viene visualizzato il messaggio seguente:  
    
      "Operazione di migrazione di macchina virtuale non riuscita all'origine della migrazione.  
      Non è stato possibile stabilire una connessione con il *nome del computer*host: Nessuna credenziale disponibile nel pacchetto di sicurezza 0x8009030E. "
  
-   **Prestazioni**: È opportuno configurare le opzioni relative alle prestazioni? Queste opzioni possono ridurre l'utilizzo della CPU e rete, nonché eseguire migrazioni in tempo reale più velocemente. Prendere in considerazione i requisiti e l'infrastruttura e configurazioni diverse per decidere di test. Le opzioni sono descritte alla fine del passaggio 2.  
  
-  **Preferenza di rete**: Si intende consentire il traffico delle migrazioni in tempo reale a tutte le reti disponibili o isolarlo a reti specifiche? Come procedura consigliata di sicurezza, è preferibile isolare il traffico delle migrazioni in tempo reale a reti privati e attendibili perché non viene crittografato durante l'invio in rete. L'isolamento di rete può essere ottenuto con una rete isolata fisicamente o con un'altra tecnologia di rete attendibile come le VLAN.  
  
## <a name="BKMK_Step1"></a>Passaggio 1: Configurare la delega vincolata (facoltativo)  
Se si è deciso di utilizzare Kerberos per autenticare il traffico di migrazione in tempo reale, configurare la delega vincolata utilizzando un account membro del gruppo Domain Administrators.  
  
### <a name="use-the-users-and-computers-snap-in-to-configure-constrained-delegation"></a>Utilizzare lo snap-in utenti e computer per configurare la delega vincolata  
  
1.  Aprire lo snap-in Utenti e computer di Active Directory. (Da Server Manager, selezionare il server se non è selezionata, fare clic su **Strumenti** >> **Active Directory Users and Computers**).  
  
2.  Nel riquadro di spostamento in **Active Directory Users and Computers**, selezionare il dominio e fare doppio clic sui **computer** cartella.  
  
3.  Dal **computer** cartella, fare doppio clic sull'account computer del server di origine e quindi fare clic su **proprietà**.  
  
4.  Da **proprietà**, fare clic sui **delega** scheda.  
  
5.  Nella scheda delega selezionare **computer attendibile per la delega solo ai servizi specificati** e quindi selezionare **Usa un qualsiasi protocollo di autenticazione**.  
  
6.  Fai clic su **Aggiungi**.  
  
7.  Da **aggiungere servizi**, fare clic su **utenti o computer**.  
  
8.  Da **Seleziona utenti o computer**, digitare il nome del server di destinazione. Fare clic su **Controlla nomi** per verificarlo e quindi fare clic su **OK**.  
  
9. Da **aggiungere servizi**, nell'elenco dei servizi disponibili, eseguire le operazioni seguenti e quindi fare clic su **OK**:  
  
    -   Per spostare lo spazio di archiviazione della macchina virtuale, selezionare **cifs**. Questo è necessario se si desidera spostare l'archiviazione insieme alla macchina virtuale, nonché come se si desidera spostare solo l'archiviazione di una macchina virtuale. Se il server è configurato per l'utilizzo dello spazio di archiviazione SMB per Hyper-V, questa opzione è già selezionata.  
  
    -   Per spostare macchine virtuali, selezionare **Servizio di migrazione sistema virtuale**.  
  
10. Nella scheda **Delega** della finestra di dialogo Proprietà verificare che i servizi selezionati nel passaggio precedente siano elencati tra quelli ai quali il computer di destinazione può presentare le credenziali delegate. Fare clic su **OK**.  
  
11. Nella cartella **Computer** selezionare l'account del computer del server di destinazione e ripetere il processo. Nella finestra di dialogo **Seleziona Utenti o Computer** assicurarsi di specificare il nome del server di origine.  
  
Le modifiche di configurazione applicate dopo verificarsi entrambe le operazioni seguenti:  
  
  -  Le modifiche vengono replicate nei controller di dominio che si è connessi i server che esegue Hyper-V.  
  -  Il controller di dominio emette un nuovo ticket Kerberos.  
  
## <a name="BKMK_Step2"></a>Passaggio 2: Configurare i computer di origine e di destinazione per la migrazione in tempo reale  
Questo passaggio include la scelta di opzioni per l'autenticazione e di rete. Per una protezione ottimale, è consigliabile selezionare reti specifiche da utilizzare per il traffico di migrazione in tempo reale, come illustrato in precedenza. Questo passaggio illustra anche come scegliere l'opzione relativa alle prestazioni.   
  
### <a name="use-hyper-v-manager-to-set-up-the-source-and-destination-computers-for-live-migration"></a>Gestione di Hyper-V consente di configurare i computer di origine e destinazione per live migration  
  
1.  Aprire la console di gestione di Hyper-V. (Da Server Manager, fare clic su **Strumenti** >>**Hyper-V Manager**.)  
  
2.  Nel riquadro di spostamento, selezionare uno dei server. (Se non è elencato, fare doppio clic su **Gestione di Hyper-V**, fare clic su **Connetti al Server**, digitare il nome del server e fare clic su **OK**. Ripetere il passaggio per aggiungere più server.)  
  
3.  Nel **azione** riquadro, fare clic su **Impostazioni Hyper-V** >>**Live Migration**.  
  
4.  Nel riquadro **Migrazioni Live Migration** selezionare **Abilita migrazioni Live Migration in entrata e in uscita**.  
  
5.  In **migrazioni in tempo reale simultanee**, specificare un numero diverso se non si desidera utilizzare il valore predefinito è 2.  
  
6.  In **Migrazioni Live Migration in entrata**, fare clic su **Aggiungi** per digitare l'indirizzo IP se si desidera utilizzare connessioni di rete specifiche che accettino il traffico delle migrazioni in tempo reale. In caso contrario, fare clic su **Usa qualsiasi rete disponibile per Live Migration**. Fare clic su **OK**.  
  
7.  Per scegliere le opzioni di autenticazione Kerberos e prestazioni, espandere **Live Migration** e quindi selezionare **funzionalità avanzate**.  
  
    - Se è stata configurata la delega vincolata, in **protocollo di autenticazione**, selezionare **Kerberos**.  
    - In **Opzioni relative alle prestazioni**, esaminare i dettagli e scegliere un'opzione diversa se è appropriato per l'ambiente.   
  
8. Fare clic su **OK**.  
  
9. Selezionare l'altro server di gestione di Hyper-V e ripetere i passaggi.  
  
### <a name="use-windows-powershell-to-set-up-the-source-and-destination-computers-for-live-migration"></a>Utilizzare Windows PowerShell per configurare il computer di origine e destinazione per live migration  
  
Sono disponibili tre cmdlet per la configurazione della migrazione in tempo reale in host non in cluster: [Enable-VMMigration](https://technet.microsoft.com/library/hh848544.aspx), [set-VMMigrationNetwork](https://technet.microsoft.com/library/hh848467.aspx)e [set-VMHost](https://technet.microsoft.com/library/hh848524.aspx). In questo esempio utilizza tutti e tre ed esegue le operazioni seguenti:   
  - Configura migrazione in tempo reale nell'host locale  
  - Consente il traffico in ingresso migrazione solo su una rete specifica  
  - Sceglie Kerberos come protocollo di autenticazione   
  
Ogni riga rappresenta un comando distinto.  
  
```  
PS C:\> Enable-VMMigration  
  
PS C:\> Set-VMMigrationNetwork 192.168.10.1  
  
PS C:\> Set-VMHost -VirtualMachineMigrationAuthenticationType Kerberos  
  
```  
Set-VMHost consente inoltre di scegliere un'opzione relativa alle prestazioni (e molte altre impostazioni host). Ad esempio, per scegliere SMB, lasciando il protocollo di autenticazione impostato sul valore predefinito di CredSSP, digitare:  
  
```  
PS C:\> Set-VMHost -VirtualMachineMigrationPerformanceOption SMBTransport  
  
```  
  
La tabella seguente descrive come utilizzare le opzioni di prestazioni.  
  
|Opzione|Descrizione|  
|----------|---------------|  
    |TCP/IP|Copia la memoria della macchina virtuale nel server di destinazione tramite una connessione TCP/IP.|  
    |Compressione|Comprime il contenuto della memoria della macchina virtuale prima della copia nel server di destinazione tramite una connessione TCP/IP. **Nota:** Si tratta dell'impostazione **predefinita** .|  
    |SMB|Copia la memoria della macchina virtuale nel server di destinazione tramite una connessione SMB 3.0.<br /><br />-SMB diretto viene utilizzato quando le schede di rete nel server di origine e destinazione hanno funzionalità Direct accesso memoria remota (RDMA).<br />-SMB multicanale rileva e utilizza automaticamente più connessioni quando viene identificata una configurazione SMB multicanale appropriata.<br /><br />Per altre informazioni, vedere [Improve Performance of a File Server with SMB Direct](https://technet.microsoft.com/library/jj134210(WS.11).aspx).|  
      
 ## <a name="next-steps"></a>Passaggi successivi
 
 Dopo aver configurato gli host, si è pronti per eseguire una migrazione in tempo reale. Per istruzioni, vedere [utilizzare live migration senza Clustering di Failover per spostare una macchina virtuale](../manage/Use-live-migration-without-Failover-Clustering-to-move-a-virtual-machine.md). 
    


