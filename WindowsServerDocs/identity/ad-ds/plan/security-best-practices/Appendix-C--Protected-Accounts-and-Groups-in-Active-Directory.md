---
ms.assetid: 5b2876ac-fe7d-4054-bfba-b692e57bc0d2
title: 'Appendice C: account protetti e gruppi in Active Directory'
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 70e29ad42b57cf315c7179d6eea8220ad2bfab48
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59834702"
---
# <a name="appendix-c-protected-accounts-and-groups-in-active-directory"></a>Appendice C: Gli account protetti e gruppi in Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

## <a name="appendix-c-protected-accounts-and-groups-in-active-directory"></a>Appendice C: Gli account protetti e gruppi in Active Directory

All'interno di Active Directory, un set predefinito di account con privilegi elevati e i gruppi sono considerati gli account protetti e gruppi. Con la maggior parte degli oggetti in Active Directory, gli amministratori delegati (gli utenti che sono state delegate le autorizzazioni per gestire oggetti Active Directory) possono modificare le autorizzazioni per gli oggetti, tra cui la modifica delle autorizzazioni per consentire loro di modificare le appartenenze ai i gruppi, ad esempio.  

Tuttavia, con gli account protetti e gruppi, le autorizzazioni degli oggetti vengono impostate e applicate tramite un processo automatico che garantisce le autorizzazioni per il rimane oggetti coerente anche se gli oggetti vengono spostati la directory. Anche se un utente modifica manualmente le autorizzazioni dell'oggetto protetto, questo processo assicura che le autorizzazioni vengono restituite rapidamente le impostazioni predefinite.  

### <a name="protected-groups"></a>Gruppi protetti

Nella tabella seguente contiene i gruppi protetti in Active Directory elencati in base al sistema operativo del controller di dominio.  

#### <a name="protected-accounts-and-groups-in-active-directory-by-operating-system"></a>Gli account protetti e gruppi in Active Directory dal sistema operativo

| Windows Server 2003 RTM | Windows Server 2003 SP1+ | Windows Server 2012, <br> Windows Server 2008 R2, <br> Windows Server 2008 | Windows Server 2016 |
| --- | --- | --- | --- |
|Account Operators|Account Operators|Account Operators|Account Operators|
|Administrator|Administrator|Administrator|Administrator|
|Administrators|Administrators|Administrators|Administrators|
|Backup Operators|Backup Operators|Backup Operators|Backup Operators|
|Cert Publishers|||
|Domain Admins|Domain Admins|Domain Admins|Domain Admins|
|Controller di dominio|Controller di dominio|Controller di dominio|Controller di dominio|
|Enterprise Admins|Enterprise Admins|Enterprise Admins|Enterprise Admins|
||||Enterprise Admins chiave|
||||Amministratori principali|
|Krbtgt|Krbtgt|Krbtgt|Krbtgt|
|Print Operators|Print Operators|Print Operators|Print Operators|
|||Controller di dominio di sola lettura|Controller di dominio di sola lettura|
|Replicator|Replicator|Replicator|Replicator|
|Schema Admins|Schema Admins|Schema Admins|Schema Admins|
|Server Operators|Server Operators|Server Operators|Server Operators|

#### <a name="adminsdholder"></a>AdminSDHolder

Lo scopo dell'oggetto AdminSDHolder è fornire le autorizzazioni "modello" per gli account protetti e i gruppi nel dominio. AdminSDHolder viene creato automaticamente come oggetto nel contenitore di sistema di ogni dominio di Active Directory. Il percorso è: **CN=AdminSDHolder,CN=System,DC=<domain_component>,DC=<domain_component>?.**  

A differenza di maggior parte degli oggetti nel dominio Active Directory, che appartengono al gruppo Administrators, AdminSDHolder appartiene al gruppo Domain Admins. Per impostazione predefinita, EAs può apportare modifiche all'oggetto AdminSDHolder un dominio, nonché da gruppi Domain Admins e amministratori del dominio. Inoltre, anche se il proprietario predefinito di AdminSDHolder gruppo Domain Admins del dominio, i membri del gruppo Administrators o Enterprise Admins possono assumere la proprietà dell'oggetto.  

#### <a name="sdprop"></a>SDProp

SDProp è un processo che esegue ogni 60 minuti (impostazione predefinita) nel controller di dominio che contiene l'emulatore PDC (PDCE del dominio). SDProp mette a confronto le autorizzazioni sull'oggetto AdminSDHolder del dominio con le autorizzazioni per gli account protetti e i gruppi nel dominio. Se le autorizzazioni per gli account protetti e i gruppi di non corrispondono le autorizzazioni per l'oggetto AdminSDHolder, le autorizzazioni per gli account protetti e i gruppi vengono reimpostate corrispondano a quelli dell'oggetto AdminSDHolder del dominio.  

Inoltre, ereditarietà delle autorizzazioni è disabilitato in gruppi protetti e gli account, il che significa che anche se gli account e gruppi vengono spostati in percorsi diversi nella directory, non ereditano le autorizzazioni dai relativi oggetti padre nuovo. Ereditarietà è disabilitata per l'oggetto AdminSDHolder in modo che le modifiche alle autorizzazioni per gli oggetti padre non modificare le autorizzazioni di AdminSDHolder.  

##### <a name="changing-sdprop-interval"></a>Modifica intervallo SDProp

In genere, non è necessario modificare l'intervallo in cui viene eseguito il SDProp, fatta eccezione per scopi di test. Se è necessario modificare l'intervallo di SDProp, nell'emulatore PDC per il dominio, usare regedit per aggiungere o modificare il valore DWORD AdminSDProtectFrequency HKLM\System\CurrentControlSet\Services\NTDS\Parameters.  

L'intervallo di valori è espresso in secondi da 60 a 7200 (un minuto a due ore). Per invertire le modifiche, eliminare AdminSDProtectFrequency chiave, che causerà SDProp ripristinare l'intervallo di 60 minuti. È in genere necessario non ridurre questo intervallo in domini di produzione come è possibile aumentare LSASS overhead di elaborazione nel controller di dominio. L'impatto dell'aumento dipende dal numero di oggetti protetti nel dominio.  

##### <a name="running-sdprop-manually"></a>Esecuzione manuale SDProp

Un approccio migliore al testing delle modifiche AdminSDHolder consiste nell'eseguire SDProp manualmente, che fa sì che l'attività venga eseguita immediatamente, ma non influisce sul esecuzioni pianificate. Esecuzione manuale SDProp viene eseguita in modo leggermente diverso nei controller di dominio che esegue Windows Server 2008 e versioni precedenti non sul controller di dominio che eseguono Windows Server 2012 o Windows Server 2008 R2.  

Le procedure per l'esecuzione SDProp manualmente nei sistemi operativi precedenti sono disponibili nel [articolo del supporto Microsoft 251343](https://support.microsoft.com/kb/251343), e di seguito sono fornite istruzioni dettagliate per i sistemi operativi precedenti e più recenti. In entrambi i casi, è necessario connettersi all'oggetto rootDSE in Active Directory ed eseguire un'operazione di modifica con un nome di dominio null per l'oggetto rootDSE, specificando il nome dell'operazione come l'attributo da modificare. Per altre informazioni sulle operazioni modificabile sull'oggetto rootDSE, vedere [rootDSE modificare operazioni](https://msdn.microsoft.com/library/cc223297.aspx) sul sito Web MSDN.  

###### <a name="running-sdprop-manually-in-windows-server-2008-or-earlier"></a>Esecuzione manuale SDProp in Windows Server 2008 o versioni precedenti

È possibile forzare SDProp da eseguire usando Ldp.exe oppure eseguendo uno script di modifica LDAP. Per eseguire SDProp usando Ldp.exe, procedere come segue dopo avere apportato modifiche all'oggetto AdminSDHolder in un dominio:  

1. Avvio veloce **Ldp.exe**.  
2. Fare clic su **Connection** sulla finestra di dialogo Ldp, fare clic **Connect**.  

   ![account protetti e gruppi](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_9.gif)  

3. Nel **Connect** finestra di dialogo digitare il nome del controller di dominio per il dominio che detiene il ruolo emulatore PDC (emulatore PDC), fare clic su **OK**.  

   ![account protetti e gruppi](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_10.png)  

4. Verificare che sia stato collegato correttamente, come indicato dal **Dn: (RootDSE)**  nello screenshot seguente, fare clic su **Connection** e fare clic su **associare**.  

   ![account protetti e gruppi](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_11.png)  

5. Nel **associare** finestra di dialogo, digitare le credenziali di un account utente che dispone dell'autorizzazione per modificare l'oggetto rootDSE. (Se è connessi con questo nome utente, è possibile selezionare **eseguire l'associazione come** utente attualmente connesso.) Fare clic su **OK**.  

   ![account protetti e gruppi](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_12.png)  

6. Dopo aver completato l'operazione di associazione, fare clic su **esplorare**, fare clic su **Modify**.  

   ![account protetti e gruppi](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_13.png)  

7. Nel **Modify** finestra di dialogo, lasciare il **DN** campo vuoto. Nel **Modifica attributo voce** digitare **FixUpInheritance**e il **valori** digitare **Sì**. Fare clic su **invio** per popolare le **voce elenco** come illustrato nella schermata riportata di seguito.  

   ![account protetti e gruppi](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_14.gif)  

8. Nella finestra di dialogo Modifica popolato, fare clic su Esegui e verificare che le modifiche apportate all'oggetto AdminSDHolder visualizzate su tale oggetto.  

> [!NOTE]  
> Per informazioni sulla modifica AdminSDHolder per consentire agli account senza privilegi designati modificare l'appartenenza di gruppi protetti, vedere [appendice i: Creazione di account di gestione per protette di gruppi e account in Active Directory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md).  

Se si preferisce eseguire SDProp manualmente tramite LDIFDE o uno script, è possibile creare una voce di modifica, come illustrato di seguito:  

![account protetti e gruppi](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_15.gif)  

###### <a name="running-sdprop-manually-in-windows-server-2012-or-windows-server-2008-r2"></a>Esecuzione manuale SDProp in Windows Server 2012 o Windows Server 2008 R2

È anche possibile forzare SDProp da eseguire usando Ldp.exe oppure eseguendo uno script di modifica LDAP. Per eseguire SDProp usando Ldp.exe, procedere come segue dopo avere apportato modifiche all'oggetto AdminSDHolder in un dominio:  

1. Avvio veloce **Ldp.exe**.  

2. Nel **Ldp** finestra di dialogo, fare clic su **connessione**, fare clic su **Connect**.  

   ![account protetti e gruppi](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_16.gif)  

3. Nel **Connect** finestra di dialogo digitare il nome del controller di dominio per il dominio che detiene il ruolo emulatore PDC (emulatore PDC), fare clic su **OK**.  

   ![account protetti e gruppi](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_17.gif)  

4. Verificare che sia stato collegato correttamente, come indicato dal **Dn: (RootDSE)**  nello screenshot seguente, fare clic su **Connection** e fare clic su **associare**.  

   ![account protetti e gruppi](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_18.gif)  

5. Nel **associare** finestra di dialogo, digitare le credenziali di un account utente che dispone dell'autorizzazione per modificare l'oggetto rootDSE. (Se è connessi con questo nome utente, è possibile selezionare **Bind utente attualmente connesso**.) Fare clic su **OK**.  

   ![account protetti e gruppi](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_19.gif)  

6. Dopo aver completato l'operazione di associazione, fare clic su **esplorare**, fare clic su **Modify**.  

   ![account protetti e gruppi](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_20.gif)  

7. Nel **Modify** finestra di dialogo, lasciare il **DN** campo vuoto. Nel **Modifica attributo voce** digitare **RunProtectAdminGroupsTask**e il **valori** digitare **1**. Fare clic su **invio** per popolare l'elenco di voce, come illustrato di seguito.  

   ![account protetti e gruppi](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_21.gif)  

8. In popolato **Modify** finestra di dialogo, fare clic su **eseguire**e verificare che le modifiche apportate all'oggetto AdminSDHolder visualizzate su tale oggetto.  

Se si preferisce eseguire SDProp manualmente tramite LDIFDE o uno script, è possibile creare una voce di modifica, come illustrato di seguito:  

![account protetti e gruppi](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_22.gif)  
