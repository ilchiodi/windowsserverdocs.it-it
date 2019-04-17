---
title: Aggiornare controller di dominio a Windows Server 2016
description: Questo documento descrive come eseguire l'aggiornamento da Windows Server 2012 R2 a Windows Server 2016
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 187972c2f44a4d7f91b1b3ac1c905529564cfa6d
ms.sourcegitcommit: f748c6c4ce700b0787ffdd1fca620c21c4331fd2
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/21/2017
---
# <a name="upgrade-domain-controllers-to-windows-server-2016"></a>Aggiornare controller di dominio a Windows Server 2016

Si applica a: Windows Server 2016

In questo argomento vengono fornite informazioni su servizi di dominio Active Directory in Windows Server 2016 e viene illustrato il processo di aggiornamento dei controller di dominio da Windows Server 2012 o Windows Server 2012 R2. 

## <a name="pre-requisites"></a>Prerequisiti
Il metodo consigliato per aggiornare un dominio è alzare di livello controller di dominio che eseguono versioni più recenti di Windows Server e abbassare di livello il controller di dominio precedenti in base alle esigenze. Questo metodo è preferibile per l'aggiornamento del sistema operativo di un controller di dominio esistente. Questo elenco enumera i passaggi generali da seguire prima di promuovere un controller di dominio che esegue una versione più recente di Windows Server: 

1.  Verificare che il server di destinazione soddisfi i requisiti di sistema. 
2.  Verificare la compatibilità delle applicazioni. 
3.  Esaminare i suggerimenti per lo spostamento in Windows Server 2016 
4.  Verificare le impostazioni di sicurezza. Per ulteriori informazioni, vedere [caratteristiche deprecate e modifiche del comportamento relative a servizi di dominio Active Directory in Windows Server 2016](../../../get-started\deprecated-features.md). 
5.  Verificare la connettività al server di destinazione dal computer in cui si prevede di eseguire l'installazione. 
6.  Controllare la disponibilità dei ruoli master operazione necessari: 
    - Per installare il primo controller di dominio che esegue Windows Server 2016 in un dominio esistente e della foresta, il computer in cui si esegue l'installazione disponga della connettività al **master schema** per poter eseguire adprep /forestprep e al master infrastrutture per poter eseguire adprep /domainprep. 
    - Per installare il primo controller di dominio in un dominio in cui è già esteso lo schema della foresta, è necessario solo connettività al master infrastrutture. 
    - Per installare o rimuovere un dominio in una foresta esistente, è necessaria la connettività per la **master denominazione domini**. 
    - Qualsiasi installazione di controller di dominio richiede inoltre la connettività per la **master RID.** 
    - Se si installa il primo controller di dominio di sola lettura in una foresta esistente, è necessaria la connettività al master infrastrutture per ogni partizione di directory applicativa, noto anche come un contesto dei nomi non di dominio o detta. 

### <a name="installation-steps-and-required-administrative-levels"></a>I passaggi dell'installazione e necessari livelli dell'amministrazione
La tabella seguente fornisce un riepilogo delle operazioni di aggiornamento e i requisiti di autorizzazione per eseguire questi passaggi

|Azione di installazione|Credenziali necessarie|
| ----- | ----- |
|Installare una nuova foresta|Amministratore locale sul server di destinazione|
|Installare un nuovo dominio in una foresta esistente|Enterprise Admins|
|Installare un controller di dominio aggiuntivo in un dominio esistente|Domain Admins|
|Eseguire adprep /forestprep|Schema Admins, Enterprise Admins e Domain Admins|
|Eseguire adprep /domainprep.|Domain Admins|
|Eseguire adprep /domainprep /gpprep|Domain Admins|
|Eseguire adprep /rodcprep|Enterprise Admins|

Per ulteriori informazioni sulle nuove funzionalità in Windows Server 2016, vedere [What's new in Windows Server 2016](../../../get-started/what-s-new-in-windows-server-2016.md).



## <a name="supported-in-place-upgrade-paths"></a>Percorsi di aggiornamento sul posto supportati
I controller di dominio che eseguono versioni a 64 bit di Windows Server 2012 o Windows Server 2012 R2 possono essere aggiornati a Windows Server 2016. Poiché Windows Server 2016 è disponibile solo in una versione a 64 bit, sono supportati solo gli aggiornamenti di versione a 64 bit.

|Se si esegue questa versione:|È possibile eseguire l'aggiornamento a queste edizioni:|
| ----- | ----- |   
|Windows Server 2012 Standard|Windows Server 2016 Standard o Datacenter|   
|Windows Server 2012 Datacenter|Windows Server 2016 Datacenter| 
|Windows Server 2012 R2 Standard|Windows Server 2016 Standard o Datacenter|    
|Windows Server 2012 R2 Datacenter|Windows Server 2016 Datacenter|  
|Windows Server 2012 R2 Essentials|Windows Server 2016 Essentials|  
|Windows Storage Server 2012 Standard|Windows Storage Server 2016 Standard| 
|Gruppo di lavoro di Windows Storage Server 2012|Gruppo di lavoro di Windows Storage Server 2016|   
|Windows Storage Server 2012 R2 Standard|Windows Storage Server 2016 Standard|  
|Gruppo di lavoro di Windows Storage Server 2012 R2|Gruppo di lavoro di Windows Storage Server 2016|    

Per ulteriori informazioni sui percorsi di aggiornamento supportati, vedere [percorsi di aggiornamento supportati](../../../get-started/supported-upgrade-paths.md)

## <a name="adprep-and-domainprep"></a>Adprep e Domainprep
Se si sta eseguendo un aggiornamento sul posto di un controller di dominio esistente per il sistema operativo Windows Server 2016, è necessario eseguire adprep /forestprep e adprep /domainprep manualmente.  Adprep /forestprep deve essere eseguita una sola volta nella foresta.  Adprep /domainprep deve essere eseguito una volta in ogni dominio in cui si dispone di controller di dominio che si esegue l'aggiornamento a Windows Server 2016.

Se si alzano di livello un nuovo server Windows Server 2016 che non è necessario per eseguire queste manualmente.  Questi criteri sono integrati in PowerShell ed esperienze di Server Manager.

Per ulteriori informazioni sull'esecuzione di adprep vedere [esecuzione di Adprep](https://technet.microsoft.com/library/dd464018.aspx) 


## <a name="functional-level-features-and-requirements"></a>Requisiti e le funzionalità a livello funzionale
Windows Server 2016 richiede un livello di funzionalità foresta Windows Server 2003. Vale a dire, prima di poter aggiungere un controller di dominio che esegue Windows Server 2016 a una foresta di Active Directory esistente, il livello di funzionalità foresta deve essere Windows Server 2003 o versione successiva. Se la foresta contiene controller di dominio che esegue Windows Server 2003 o versione successiva ma le funzionalità della foresta di livello è ancora Windows 2000, l'installazione viene ugualmente bloccata. 

I controller di dominio Windows 2000 devono essere rimossa prima di aggiungere i controller di dominio Windows Server 2016 all'insieme di strutture. In questo caso, prendere in considerazione il flusso di lavoro seguente: 


1. Installare controller di dominio che eseguono Windows Server 2003 o versione successiva. Questi controller di dominio possono essere distribuiti in una versione di valutazione di Windows Server. Questo passaggio richiede anche l'esecuzione di adprep.exe per tale versione del sistema operativo come prerequisito. 
2.  Rimuovere i controller di dominio Windows 2000. In particolare, normalmente abbassare di livello o forzare la rimozione di controller di dominio Windows Server 2000 del dominio utilizzato Active Directory Users e computer per rimuovere gli account di controller di dominio per tutti i controller di dominio rimossi. 
3.  Aumentare il livello di funzionalità della foresta a Windows Server 2003 o versione successiva. 
4.  Installare controller di dominio che eseguono Windows Server 2016. 
5.  Rimuovere i controller di dominio che eseguono versioni precedenti di Windows Server. 

### <a name="rolling-back-functional-levels"></a>Eseguire il rollback di livelli di funzionalità

Dopo aver impostato il livello di funzionalità foresta (FFL) su un determinato valore, è possibile eseguire il rollback né abbassare il livello di funzionalità foresta, con le eccezioni seguenti: 

- Se si esegue l'aggiornamento da Windows Server 2012 R2 FFL, è possibile diminuirlo a Windows Server 2012 R2. 
- Se si esegue l'aggiornamento da Windows Server 2008 R2 FFL, è possibile diminuirlo a Windows Server 2008 R2.

Dopo aver impostato il livello di funzionalità del dominio su un determinato valore, è possibile eseguire il rollback né abbassare il livello funzionale di dominio, con le eccezioni seguenti: 

- Quando si aumenta il livello di funzionalità del dominio a Windows Server 2016 e se il livello funzionale della foresta è Windows Server 2012 o inferiore, è necessario eseguire il backup l'opzione di ripristino dello stato il livello di funzionalità del dominio a Windows Server 2012 o Windows Server 2012 R2 

Per ulteriori informazioni sulle funzionalità disponibili a livelli di funzionalità inferiori, vedere [livelli di funzionalità di servizi di dominio Active Directory informazioni (AD DS)](../active-directory-functional-levels.md). 
 
## <a name="ad-ds-interoperability-with-other-server-roles-and-windows-operating-systems"></a>Interoperabilità di Active Directory con altri ruoli server e sistemi operativi Windows
Servizi di dominio Active Directory non è supportato nei sistemi operativi Windows seguenti: 


- Windows MultiPoint Server 
- Windows Server 2016 Essentials 

Servizi di dominio Active Directory non è possibile installare in un server che esegue anche i seguenti ruoli del server o dei servizi ruolo: 

- Microsoft Hyper-V Server 2016
- Gestore connessione Desktop remoto 

## <a name="administration-of-windows-server-2016-servers"></a>Amministrazione dei server Windows Server 2016
Utilizzare Remote Server Administration Tools per Windows 10 per gestire i controller di dominio e altri server che eseguono Windows Server 2016. È possibile eseguire Windows Server 2016 Server strumenti di amministrazione remota in un computer che esegue Windows 10. 

## <a name="step-by-step-for-upgrading-to-windows-server-2016"></a>Istruzioni dettagliate per l'aggiornamento a Windows Server 2016
Di seguito è un semplice esempio di aggiornamento della foresta di Contoso da Windows Server 2012 R2 a Windows Server 2016.

![Eseguire l'aggiornamento](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade1.png)

1.  Aggiungere il nuovo Windows Server 2016 all'insieme di strutture. Riavviare quando richiesto. 
![Eseguire l'aggiornamento](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade2.png)
2.  Accedi al nuovo Windows Server 2016 con un account di amministratore di dominio.
3.  In **Server Manager**, in **Aggiungi ruoli e funzionalità**, installare **servizi di dominio Active Directory** nel nuovo Windows Server 2016. Adprep verrà eseguito automaticamente in 2012 R2 foresta e nel dominio.
![Eseguire l'aggiornamento](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade3.png) 
4.  In **Server Manager**il triangolo giallo e dall'elenco a discesa scegliere **promuovere il server a un controller di dominio**. 
![Eseguire l'aggiornamento](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade4.png)
5.  Nel **configurazione distribuzione** schermo, seleziona **aggiungere un controller di dominio a una foresta esistente** e fare clic su Avanti. 
![Eseguire l'aggiornamento](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade5.png)
6.  Nel **opzioni Controller di dominio** schermata, immettere il **modalità ripristino servizi Directory (DSRM)** password e fare clic su Avanti. 
7.  Per scegliere il resto delle schermate **Avanti**. 
8.  Nel **prerequisiti** schermata, fare clic su **installare**. Una volta completato il riavvio è possano accedere di nuovo.
9.  Nel server di Windows Server 2012 R2, in **Server Manager**, selezionare strumenti, **modulo Active Directory per Windows PowerShell**. 
![Eseguire l'aggiornamento](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade6.png)
10. In windows PowerShell Usa il movimento-ADDirectoryServerOperationMasterRole per spostare i ruoli FSMO. È possibile digitare il nome di ogni OperationMasterRole - o utilizzare i numeri per specificare i ruoli. I numeri. Per ulteriori informazioni vedere [Move-ADDirectoryServerOperationMasterRole](https://technet.microsoft.com/library/hh852302.aspx)

``` powershell
Move-ADDirectoryServerOperationMasterRole -Identity "DC-W2K16" -OperationMasterRole 0,1,2,3,4
```

![Eseguire l'aggiornamento](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade7.png)</br>
11. Verificare i ruoli sono stati spostati accedendo al server di Windows Server 2016, in **Server Manager**, in **strumenti**selezionare **modulo Active Directory per Windows PowerShell**. Utilizzare il `Get-ADDomain` e `Get-ADForest` cmdlet per visualizzare i titolari del ruolo FSMO.
![Eseguire l'aggiornamento] (media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade8.png)
! [L'aggiornamento](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade9.png)
12. Abbassare di livello e rimuovere il controller di dominio Windows Server 2012 R2. Per informazioni su abbassamento di livello un controller di dominio, vedere [abbassamento di livello controller di dominio e domini](../../ad-ds/deploy/Demoting-Domain-Controllers-and-Domains--Level-200-.md)
13. Una volta abbassato di livello e rimosso il server è possibile aumentare la funzionalità della foresta e livelli di funzionalità del dominio a Windows Server 2016.


## <a name="next-steps"></a>Passaggi successivi
-   [What's New in dominio Active Directory di installazione e rimozione di servizi](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md)  
-   [Installare servizi di dominio Active Directory & #40; Livello 100 & #41;](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md)     
-   [Livelli di funzionalità di Windows Server 2016](../../ad-ds/Windows-Server-2016-Functional-Levels.md)  
