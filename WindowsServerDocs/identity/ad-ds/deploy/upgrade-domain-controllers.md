---
title: Aggiornare i controller di dominio a Windows Server 2016
description: Questo documento descrive come eseguire l'aggiornamento da Windows Server 2012 R2 a Windows Server 2016
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: fe6fb196c996d4d95c6b58d1ab77591602e143d9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59868422"
---
# <a name="upgrade-domain-controllers-to-windows-server-2016"></a>Aggiornare i controller di dominio a Windows Server 2016

Si applica a: Windows Server 2016

Questo argomento fornisce informazioni complementari su servizi di dominio Active Directory in Windows Server 2016 e viene illustrato il processo di aggiornamento dei controller di dominio da Windows Server 2012 o Windows Server 2012 R2. 

## <a name="pre-requisites"></a>Prerequisiti
Il metodo consigliato per aggiornare un dominio è alzare di livello controller di dominio che eseguono versioni più recenti di Windows Server e abbassare di livello i controller di dominio precedenti in base alle esigenze. Questo metodo è preferibile all'aggiornamento del sistema operativo di un controller di dominio esistente. Questo elenco illustra i passaggi generali da seguire prima che si Alza di livello un controller di dominio che esegue una versione più recente di Windows Server: 

1.  Verificare che il server di destinazione soddisfi i requisiti di sistema. 
2.  Verificare la compatibilità delle applicazioni. 
3.  Esaminare i suggerimenti per lo spostamento in Windows Server 2016 
4.  Verificare le impostazioni di sicurezza Per altre informazioni, vedere [caratteristiche deprecate e modifiche del comportamento relative a Active Directory Domain Services in Windows Server 2016](../../../get-started\deprecated-features.md). 
5.  Controllare la connettività al server di destinazione dal computer in cui si prevede di eseguire l'installazione. 
6.  Controllare la disponibilità dei ruoli master operazione necessari: 
    - Per installare il primo controller di dominio che esegue Windows Server 2016 in un dominio esistente e una foresta, il computer in cui si esegue l'installazione richiede la connettività per il **master schema** per poter eseguire adprep /forestprep e al master infrastrutture Per poter eseguire adprep /domainprep. 
    - Per installare il primo controller di dominio in un dominio in cui lo schema della foresta è già esteso è necessaria solo la connettività al master infrastrutture. 
    - Per installare o rimuovere un dominio in una foresta esistente, è necessaria la connettività per il **master denominazione domini**. 
    - Qualsiasi installazione di controller di dominio richiesta anche la connettività di **master RID.** 
    - Se si sta installando il primo controller di dominio di sola lettura in una foresta esistente è necessaria la connettività al master infrastrutture per ogni partizione di directory applicativa, altrimenti detta contesto dei nomi non di dominio. 

### <a name="installation-steps-and-required-administrative-levels"></a>Passaggi di installazione e livelli dell'amministrazione obbligatori
La tabella seguente fornisce un riepilogo dei passaggi di aggiornamento e i requisiti di autorizzazione per eseguire questi passaggi

|Operazione di installazione|Requisiti relativi alle credenziali|
| ----- | ----- |
|Installare una nuova foresta|Amministratore locale nel server di destinazione|
|Installare un nuovo dominio in una foresta esistente|Enterprise Admins|
|Installare un controller di dominio aggiuntivo in un dominio esistente|Domain Admins|
|Eseguire adprep /forestprep|Schema Admins, Enterprise Admins e Domain Admins|
|Eseguire adprep /domainprep|Domain Admins|
|Eseguire adprep /domainprep /gpprep|Domain Admins|
|Eseguire adprep /rodcprep|Enterprise Admins|

Per altre informazioni sulle nuove funzionalità di Windows Server 2016, vedere [novità in Windows Server 2016](../../../get-started/what-s-new-in-windows-server-2016.md).



## <a name="supported-in-place-upgrade-paths"></a>Percorsi di aggiornamento sul posto supportati
I controller di dominio che eseguono versioni a 64 bit di Windows Server 2012 o Windows Server 2012 R2 possono essere aggiornati a Windows Server 2016. Solo gli aggiornamenti di versione a 64 bit sono supportati perché Windows Server 2016 è disponibile solo in una versione a 64 bit.

|Edizioni eseguite|Edizioni a cui è possibile eseguire l'aggiornamento|
| ----- | ----- |   
|Windows Server 2012 Standard|Windows Server 2016 Standard o Datacenter|   
|Windows Server 2012 Datacenter|Windows Server 2016 Datacenter| 
|Windows Server 2012 R2 Standard|Windows Server 2016 Standard o Datacenter|    
|Windows Server 2012 R2 Datacenter|Windows Server 2016 Datacenter|  
|Windows Server 2012 R2 Essentials|Windows Server 2016 Essentials|  
|Windows Storage Server 2012 Standard|Windows Storage Server 2016 Standard| 
|Windows Storage Server 2012 Workgroup|Windows Storage Server 2016 Workgroup|   
|Windows Storage Server 2012 R2 Standard|Windows Storage Server 2016 Standard|  
|Windows Storage Server 2012 R2 Workgroup|Windows Storage Server 2016 Workgroup|    

Per altre informazioni sui percorsi di aggiornamento supportati, vedere [percorsi di aggiornamento supportati](../../../get-started/supported-upgrade-paths.md)

## <a name="adprep-and-domainprep"></a>Adprep e Domainprep
Se si sta eseguendo un aggiornamento sul posto di un controller di dominio esistente per il sistema operativo Windows Server 2016, è necessario eseguire adprep /forestprep e adprep /domainprep manualmente.  Adprep /forestprep deve essere eseguito una sola volta nella foresta.  Adprep /domainprep deve essere eseguito una volta in ogni dominio in cui sono presenti controller di dominio che si esegue l'aggiornamento a Windows Server 2016.

Se si sta alzando di livello un nuovo server Windows Server 2016 che non è necessario eseguire questi passaggi manualmente.  Questi sono integrati in PowerShell e si verifica nel Server di gestione.

Per altre informazioni sull'esecuzione di adprep vedere [esecuzione di Adprep](https://technet.microsoft.com/library/dd464018.aspx) 


## <a name="functional-level-features-and-requirements"></a>Caratteristiche e requisiti dei livelli di funzionalità
Windows Server 2016 richiede un livello di funzionalità foresta Windows Server 2003. Vale a dire, prima di poter aggiungere un controller di dominio che esegue Windows Server 2016 a una foresta di Active Directory esistente, il livello funzionale della foresta deve essere Windows Server 2003 o versione successiva. Se la foresta contiene controller di dominio che eseguono Windows Server 2003 o versione successiva, ma il livello funzionale della foresta è ancora Windows 2000, l'installazione viene ugualmente bloccata. 

Prima di aggiungere i controller di dominio di Windows Server 2016 per la foresta, è necessario rimuovere i controller di dominio Windows 2000. In questo caso, osservare il flusso di lavoro seguente: 


1. Installare controller di dominio che eseguono Windows Server 2003 o versione successiva. È possibile distribuire questi controller di dominio su una versione di valutazione di Windows Server. Questo passaggio richiede anche che eseguono adprep.exe. per tale versione del sistema operativo come prerequisito. 
2.  Rimuovere i controller di dominio di Windows 2000. Nello specifico, provocare un abbassamento del livello dei controller di dominio di Windows Server 2000 oppure eliminarli dal dominio e utilizzare Utenti e computer di Active Directory per rimuovere gli account del controller di dominio relativi a tutti i controller di dominio rimossi. 
3.  Aumentare il livello di funzionalità della foresta a Windows Server 2003 o superiore. 
4.  Installare controller di dominio che eseguono Windows Server 2016. 
5.  Rimuovere i controller di dominio che eseguono versioni precedenti di Windows Server. 

### <a name="rolling-back-functional-levels"></a>Eseguire il rollback di livelli di funzionalità

Dopo aver impostato il livello funzionale della foresta (FFL) su un determinato valore, non è possibile eseguire il rollback o abbassare il livello funzionale di foresta, con le eccezioni seguenti: 

- Se esegue l'aggiornamento da Windows Server 2012 R2 FFL, è possibile diminuirlo a Windows Server 2012 R2. 
- Se esegue l'aggiornamento da Windows Server 2008 R2 FFL, è possibile diminuirlo a Windows Server 2008 R2.

Dopo aver impostato il livello funzionale del dominio su un determinato valore, non è possibile eseguire il rollback o abbassare il livello funzionale di dominio, con le eccezioni seguenti: 

- Quando si aumenta il livello funzionale di dominio a Windows Server 2016 e se il livello funzionale della foresta è Windows Server 2012 o versione precedente, è necessario eseguire il rollback il livello funzionale del dominio a Windows Server 2012 o Windows Server 2012 R2 

Per altre informazioni sulle caratteristiche disponibili a livelli di funzionalità inferiori, vedere [Informazioni sui livelli di funzionalità di Servizi di dominio Active Directory](../active-directory-functional-levels.md). 
 
## <a name="ad-ds-interoperability-with-other-server-roles-and-windows-operating-systems"></a>Interoperabilità di Servizi di dominio Active Directory con altri ruoli server e sistemi operativi Windows
Servizi di dominio Active Directory non è supportato nei sistemi operativi Windows seguenti: 


- Windows MultiPoint Server 
- Windows Server 2016 Essentials 

Non è possibile installare Servizi di dominio Active Directory su un server che esegue anche i ruoli server o servizi ruolo seguenti: 

- Microsoft Hyper-V Server 2016
- Gestore connessione Desktop remoto 

## <a name="administration-of-windows-server-2016-servers"></a>Amministrazione di server Windows Server 2016
Utilizzare Remote Server Administration Tools per Windows 10 per gestire i controller di dominio e altri server che eseguono Windows Server 2016. È possibile eseguire Windows Server 2016 Server strumenti di amministrazione remota in un computer che esegue Windows 10. 

## <a name="step-by-step-for-upgrading-to-windows-server-2016"></a>Istruzioni dettagliate per l'aggiornamento a Windows Server 2016
Di seguito è riportato un esempio semplice di aggiornare la foresta di Contoso da Windows Server 2012 R2 a Windows Server 2016.

![Aggiornamento](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade1.png)

1.  Aggiungere il nuovo Windows Server 2016 all'insieme di strutture. Riavviare quando richiesto. 
![Eseguire l'aggiornamento](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade2.png)
2.  Accedere al nuovo Windows Server 2016 con un account di amministratore di dominio.
3.  Nelle **Server Manager**, in **Aggiungi ruoli e funzionalità**, installare **Active Directory Domain Services** nel nuovo Windows Server 2016. Ciò viene eseguito adprep automaticamente in 2012 R2 foresta e nel dominio.
![Eseguire l'aggiornamento](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade3.png) 
4.  Nelle **Server Manager**, fare clic sul triangolo giallo e dall'elenco a discesa fare clic su **Alza di livello il server a controller di dominio**. 
![Eseguire l'aggiornamento](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade4.png)
5.  Nel **configurazione di distribuzione** schermata, seleziona **aggiungere un controller di dominio a una foresta esistente** e fare clic su Avanti. 
![Eseguire l'aggiornamento](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade5.png)
6.  Nel **opzioni Controller di dominio** immettere il **modalità ripristino servizi Directory (DSRM)** password e fare clic su Avanti. 
7.  Per il resto delle schermate, scegliere **successivo**. 
8.  Nel **controllo prerequisiti** schermata, fare clic su **installare**. Una volta completato il riavvio è possibile eseguire nuovamente l'accesso.
9.  Nel server di Windows Server 2012 R2, in **Server Manager**, in strumenti, selezionare **modulo Active Directory per Windows PowerShell**. 
![Eseguire l'aggiornamento](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade6.png)
10. In windows PowerShell usare il ADDirectoryServerOperationMasterRole-Move per spostare i ruoli FSMO. È possibile digitare il nome di ogni OperationMasterRole - o utilizzare i numeri per i ruoli specificati. Per altre informazioni vedere [Move-ADDirectoryServerOperationMasterRole](https://technet.microsoft.com/library/hh852302.aspx)

   ``` powershell
   Move-ADDirectoryServerOperationMasterRole -Identity "DC-W2K16" -OperationMasterRole 0,1,2,3,4
   ```

   ![Aggiornamento](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade7.png)</br>
11. Verificare i ruoli sono stati spostati, passare al server di Windows Server 2016, in **Server Manager**, in **tools**, selezionare **modulo Active Directory per Windows PowerShell**. Usare la `Get-ADDomain` e `Get-ADForest` cmdlet per visualizzare i titolari del ruolo FSMO.
![Eseguire l'aggiornamento](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade8.png)
![esegue l'aggiornamento](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade9.png)
12. Abbassare di livello e rimuovere il controller di dominio di Windows Server 2012 R2. Per informazioni sull'abbassamento di livello un controller di dominio, vedere [abbassamento di livello controller di dominio e domini](../../ad-ds/deploy/Demoting-Domain-Controllers-and-Domains--Level-200-.md)
13. Una volta che il server abbassato di livello e rimosso è possibile generare le funzionalità della foresta e i livelli funzionali del dominio a Windows Server 2016.


## <a name="next-steps"></a>Passaggi successivi
-   [Novità nel dominio di Active Directory di installazione e rimozione di servizi](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md)  
-   [Installare servizi di dominio Active Directory &#40;livello 100&#41;](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md)     
-   [Livelli di funzionalità di Windows Server 2016](../../ad-ds/Windows-Server-2016-Functional-Levels.md)  
