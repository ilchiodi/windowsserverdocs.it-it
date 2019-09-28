---
title: Aggiornare i controller di dominio a Windows Server 2016
description: Questo documento descrive come eseguire l'aggiornamento da Windows Server 2012 R2 a Windows Server 2016
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: cbb947c17219d4fe2f6694f0e44e379fc8671e76
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71401933"
---
# <a name="upgrade-domain-controllers-to-windows-server-2016"></a>Aggiornare i controller di dominio a Windows Server 2016

Si applica a: Windows Server

In questo argomento vengono fornite informazioni complementari sulle Active Directory Domain Services in Windows Server 2016 e viene illustrato il processo di aggiornamento dei controller di dominio da Windows Server 2012 o Windows Server 2012 R2.

## <a name="pre-requisites"></a>Prerequisiti

Il modo consigliato per aggiornare un dominio è promuovere i controller di dominio che eseguono versioni più recenti di Windows Server e abbassare di livello i controller di dominio precedenti in base alle esigenze. Questo metodo è preferibile all'aggiornamento del sistema operativo di un controller di dominio esistente. Questo elenco include i passaggi generali da seguire prima di alzare di livello un controller di dominio che esegue una versione più recente di Windows Server:

1. Verificare che il server di destinazione soddisfi i requisiti di sistema.
1. Verificare la compatibilità delle applicazioni.
1. Esaminare i consigli per il passaggio a Windows Server 2016
1. Verificare le impostazioni di sicurezza Per ulteriori informazioni, vedere [funzionalità deprecate e modifiche del comportamento relative a servizi di dominio Active Directory in Windows Server 2016](../../../get-started/deprecated-features.md).
1. Controllare la connettività al server di destinazione dal computer in cui si prevede di eseguire l'installazione.
1. Controllare la disponibilità dei ruoli master operazione necessari:
   - Per installare il primo controller di dominio che esegue Windows Server 2016 in un dominio e una foresta esistenti, il computer in cui si esegue l'installazione necessita della connettività al **master schema** per eseguire adprep/forestprep e il master infrastrutture per eseguire adprep/ DomainPrep.
   - Per installare il primo controller di dominio in un dominio in cui lo schema della foresta è già esteso, è necessaria solo la connettività al **master infrastrutture**.
   - Per installare o rimuovere un dominio in una foresta esistente, è necessaria la connettività al **Master**per la denominazione dei domini.
   - Qualsiasi installazione di controller di dominio richiede anche la connettività al **master RID.**
   - Se si installa il primo controller di dominio di sola lettura in una foresta esistente, è necessaria la connettività al **master infrastrutture** per ogni partizione di directory applicativa, nota anche come contesto dei nomi non di dominio o detta.

### <a name="installation-steps-and-required-administrative-levels"></a>Passaggi di installazione e livelli amministrativi richiesti

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

Per ulteriori informazioni sulle nuove funzionalità di Windows Server 2016, vedere Novità di [Windows server 2016](../../../get-started/what-s-new-in-windows-server-2016.md).

## <a name="supported-in-place-upgrade-paths"></a>Percorsi di aggiornamento sul posto supportati

I controller di dominio che eseguono versioni a 64 bit di Windows Server 2012 o Windows Server 2012 R2 possono essere aggiornati a Windows Server 2016. Sono supportati solo gli aggiornamenti della versione a 64 bit perché Windows Server 2016 è incluso solo in una versione a 64 bit.

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

Per ulteriori informazioni sui percorsi di aggiornamento supportati, vedere [percorsi di aggiornamento supportati](../../../get-started/supported-upgrade-paths.md) .

## <a name="adprep-and-domainprep"></a>Adprep e DomainPrep

Se si esegue un aggiornamento sul posto di un controller di dominio esistente al sistema operativo Windows Server 2016, sarà necessario eseguire adprep/forestprep e adprep/domainprep manualmente.  Adprep/forestprep deve essere eseguito solo una volta nella foresta.  È necessario eseguire adprep/domainprep una volta in ogni dominio in cui sono presenti controller di dominio che si sta aggiornando a Windows Server 2016.

Se si promuove un nuovo server Windows Server 2016, non è necessario eseguirli manualmente.  Questi sono integrati nelle esperienze di PowerShell e Server Manager.

Per ulteriori informazioni sull'esecuzione di adprep, vedere [esecuzione di adprep](https://technet.microsoft.com/library/dd464018.aspx)

## <a name="functional-level-features-and-requirements"></a>Caratteristiche e requisiti dei livelli di funzionalità

Windows Server 2016 richiede un livello di funzionalità della foresta di Windows Server 2003. Ovvero, prima di poter aggiungere un controller di dominio che esegue Windows Server 2016 a una foresta Active Directory esistente, il livello di funzionalità della foresta deve essere Windows Server 2003 o versione successiva. Se la foresta contiene controller di dominio che eseguono Windows Server 2003 o versione successiva, ma il livello funzionale della foresta è ancora Windows 2000, l'installazione viene ugualmente bloccata.

I controller di dominio Windows 2000 devono essere rimossi prima di aggiungere controller di dominio Windows Server 2016 alla foresta. In questo caso, osservare il flusso di lavoro seguente:

1. Installare controller di dominio che eseguono Windows Server 2003 o versione successiva. È possibile distribuire questi controller di dominio su una versione di valutazione di Windows Server. Questo passaggio richiede anche l'esecuzione di Adprep. exe per tale versione del sistema operativo come prerequisito.
1. Rimuovere i controller di dominio di Windows 2000. Nello specifico, provocare un abbassamento del livello dei controller di dominio di Windows Server 2000 oppure eliminarli dal dominio e utilizzare Utenti e computer di Active Directory per rimuovere gli account del controller di dominio relativi a tutti i controller di dominio rimossi.
1. Aumentare il livello di funzionalità della foresta a Windows Server 2003 o superiore.
1. Installare i controller di dominio che eseguono Windows Server 2016.
1. Rimuovere i controller di dominio che eseguono versioni precedenti di Windows Server.

### <a name="rolling-back-functional-levels"></a>Rollback dei livelli di funzionalità

Dopo aver impostato il livello di funzionalità della foresta (FFL) su un determinato valore, non è possibile eseguire il rollback né abbassare il livello di funzionalità della foresta, con le eccezioni seguenti:

- Se si esegue l'aggiornamento da Windows Server 2012 R2 FFL, è possibile riportarlo a Windows Server 2012 R2.
- Se si esegue l'aggiornamento da Windows Server 2008 R2 FFL, è possibile riportarlo a Windows Server 2008 R2.

Dopo aver impostato il livello di funzionalità del dominio su un determinato valore, non è possibile eseguire il rollback né abbassare il livello di funzionalità del dominio, con le eccezioni seguenti:

- Quando si aumenta il livello di funzionalità del dominio a Windows Server 2016 e se il livello di funzionalità della foresta è Windows Server 2012 o inferiore, è possibile eseguire il rollback del livello di funzionalità del dominio a Windows Server 2012 o Windows Server 2012 R2.

Per altre informazioni sulle caratteristiche disponibili a livelli di funzionalità inferiori, vedere [Informazioni sui livelli di funzionalità di Servizi di dominio Active Directory](../active-directory-functional-levels.md).

## <a name="ad-ds-interoperability-with-other-server-roles-and-windows-operating-systems"></a>Interoperabilità di Servizi di dominio Active Directory con altri ruoli server e sistemi operativi Windows

Servizi di dominio Active Directory non è supportato nei sistemi operativi Windows seguenti:

- Windows MultiPoint Server
- Windows Server 2016 Essentials

Non è possibile installare Servizi di dominio Active Directory su un server che esegue anche i ruoli server o servizi ruolo seguenti:

- Microsoft Hyper-V Server 2016
- Gestore connessione Desktop remoto

## <a name="administration-of-windows-server-2016-servers"></a>Amministrazione dei server Windows Server 2016

Usare il Strumenti di amministrazione remota del server per Windows 10 per gestire i controller di dominio e altri server che eseguono Windows Server 2016. È possibile eseguire Windows Server 2016 Strumenti di amministrazione remota del server in un computer che esegue Windows 10.

## <a name="step-by-step-for-upgrading-to-windows-server-2016"></a>Procedura dettagliata per l'aggiornamento a Windows Server 2016

Di seguito è riportato un semplice esempio di aggiornamento della foresta contoso da Windows Server 2012 R2 a Windows Server 2016.

![Aggiornamento](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade1.png)

1. Aggiungere la nuova finestra di Windows Server 2016 alla foresta. Riavviare quando richiesto.

   ![Aggiornamento](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade2.png)

1. Accedere al nuovo Windows Server 2016 con un account amministratore di dominio.
1. In **Server Manager**, in **Aggiungi ruoli e funzionalità**, installare **Active Directory Domain Services** nel nuovo Windows Server 2016. Verrà eseguito automaticamente adprep nella foresta e nel dominio 2012 R2.

   ![Aggiornamento](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade3.png)

1. In **Server Manager**fare clic sul triangolo giallo, quindi, dall'elenco a discesa, **alzare di livello il server a controller di dominio**.

   ![Aggiornamento](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade4.png)

1. Nella schermata **Configurazione distribuzione** selezionare **Aggiungi un controller di dominio a una foresta esistente** e fare clic su Avanti.

   ![Aggiornamento](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade5.png)

1. Nella schermata **Opzioni controller di dominio** immettere la password per la **modalità ripristino servizi directory** e fare clic su Avanti.
1. Per il resto delle schermate, fare clic su **Avanti**.
1. Nella schermata di **controllo dei prerequisiti** fare clic su **Installa**. Una volta completato il riavvio, è possibile eseguire di nuovo l'accesso.
1. Nel server Windows Server 2012 R2, in **Server Manager**, in strumenti selezionare **Active Directory modulo per Windows PowerShell**.

   ![Aggiornamento](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade6.png)

1. Nelle finestre di PowerShell usare il Move-ADDirectoryServerOperationMasterRole per spostare i ruoli FSMO. È possibile digitare il nome di ogni OperationMasterRole o usare i numeri per specificare i ruoli. Per altre informazioni [, vedere Move-ADDirectoryServerOperationMasterRole](https://technet.microsoft.com/library/hh852302.aspx)

    ``` powershell
    Move-ADDirectoryServerOperationMasterRole -Identity "DC-W2K16" -OperationMasterRole 0,1,2,3,4
    ```

    ![Aggiornamento](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade7.png)

1. Verificare che i ruoli siano stati spostati passando al server Windows Server 2016, in **Server Manager**, in **strumenti**Selezionare **Active Directory modulo per Windows PowerShell**. Usare i `Get-ADDomain` cmdlet `Get-ADForest` e per visualizzare i titolari del ruolo FSMO.

    ![Aggiornamento](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade8.png)

    ![Aggiornamento](media/Upgrade-Domain-Controllers-to-Windows-Server-2016/upgrade9.png)

1. Abbassare di e rimuovere il controller di dominio Windows Server 2012 R2. Per informazioni su abbassamento di un controller di [dominio, vedere domini e controller di dominio abbassamento](../../ad-ds/deploy/Demoting-Domain-Controllers-and-Domains--Level-200-.md)
1. Una volta abbassato di livello e rimosso il server, è possibile aumentare i livelli di funzionalità del dominio e funzionale della foresta a Windows Server 2016.

## <a name="next-steps"></a>Passaggi successivi

- [Novità delle procedure di installazione e rimozione di Active Directory Domain Services](../../ad-ds/deploy/What-s-New-in-Active-Directory-Domain-Services-Installation-and-Removal.md)
- [Installare il &#40;livello di Active Directory Domain Services 100&#41;](../../ad-ds/deploy/Install-Active-Directory-Domain-Services--Level-100-.md)
- [Livelli di funzionalità di Windows Server 2016](../../ad-ds/Windows-Server-2016-Functional-Levels.md)  
