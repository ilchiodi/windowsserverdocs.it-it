---
ms.assetid: 5b2876ac-fe7d-4054-bfba-b692e57bc0d2
title: Appendice C - protetto account e gruppi in Active Directory
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c1d29b61844428115f8b2d1f5d935f225a249659
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="appendix-c-protected-accounts-and-groups-in-active-directory"></a>Appendice c: account protetti e gruppi in Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012


## <a name="appendix-c-protected-accounts-and-groups-in-active-directory"></a>Appendice c: account protetti e gruppi in Active Directory  
All'interno di Active Directory, un set predefinito di account con privilegi elevati e i gruppi sono considerati account protetti e gruppi. Con la maggior parte degli oggetti in Active Directory, gli amministratori delegati (gli utenti che sono state delegate le autorizzazioni per gestire oggetti Active Directory) possono modificare le autorizzazioni per gli oggetti, tra cui la modifica delle autorizzazioni per consentire la modifica delle appartenenze ai gruppi, ad esempio.  

Tuttavia, con gli account protetti e gruppi, le autorizzazioni degli oggetti sono impostare e applicate tramite un processo automatico che garantisce che le autorizzazioni per il rimane oggetti coerenza anche se gli oggetti vengono spostati nella directory. Anche se un utente modifica manualmente le autorizzazioni dell'oggetto protetto, questo processo assicura che le autorizzazioni vengono restituite rapidamente le impostazioni predefinite.  

### <a name="protected-groups"></a>Gruppi protetti  
Nella tabella seguente contiene i gruppi protetti in Active Directory elencati in base al sistema operativo di controller di dominio.  

**Gli account protetti e gruppi in Active Directory dal sistema operativo**  

|||||  
|-|-|-|-|  
|**Windows 2000 < SP4**|**Windows 2000 SP4, Windows Server 2003 RTM**|**Windows Server 2003 SP1 +**|**Windows Server 2012, Windows Server 2008 R2, Windows Server 2008**|  
|Amministratori|Account Operators|Account Operators|Account Operators|  
||Amministratore|Amministratore|Amministratore|  
||Amministratori|Amministratori|Amministratori|  
||Gruppo Backup Operators|Gruppo Backup Operators|Gruppo Backup Operators|  
||Cert Publishers|||  
|Domain Admins|Domain Admins|Domain Admins|Domain Admins|  
||Controller di dominio|Controller di dominio|Controller di dominio|  
|Enterprise Admins|Enterprise Admins|Enterprise Admins|Enterprise Admins|  
||Krbtgt|Krbtgt|Krbtgt|  
||Operatori di stampa|Operatori di stampa|Operatori di stampa|  
||||Controller di dominio di sola lettura|  
||Servizio di replica|Servizio di replica|Servizio di replica|  
|Schema Admins|Schema Admins|Schema Admins|Schema Admins|  
||Server Operators|Server Operators|Server Operators|  

#### <a name="adminsdholder"></a>AdminSDHolder  
Lo scopo dell'oggetto AdminSDHolder è per fornire autorizzazioni "modello" per gli account protetti e i gruppi nel dominio. AdminSDHolder viene creato automaticamente come oggetto nel contenitore di sistema di ogni dominio di Active Directory. Il percorso è: **CN = AdminSDHolder, CN = System, DC = < domain_component >, DC = < domain_component >?.**  

A differenza di maggior parte degli oggetti di dominio Active Directory, che appartengono al gruppo Administrators, AdminSDHolder appartiene al gruppo Domain Admins. Per impostazione predefinita, EAs possibile apportare modifiche all'oggetto AdminSDHolder un dominio, come i gruppi di amministratori e Domain Admins del dominio. Inoltre, anche se il proprietario predefinito di AdminSDHolder è il gruppo Domain Admins del dominio, i membri del gruppo Administrators o Enterprise Admins possono assumere la proprietà dell'oggetto.  

#### <a name="sdprop"></a>SDProp  
SDProp è un processo che esegue ogni 60 minuti (per impostazione predefinita) sul controller di dominio che contiene l'emulatore PDC (PDCE del dominio). SDProp confronta le autorizzazioni per l'oggetto AdminSDHolder del dominio con le autorizzazioni per gli account protetti e i gruppi nel dominio. Se le autorizzazioni per tutti i gruppi e gli account protetti le autorizzazioni per l'oggetto AdminSDHolder non corrispondono, vengono reimpostate le autorizzazioni per gli account protetti e gruppi corrispondenti a quelle dell'oggetto AdminSDHolder del dominio.  

Inoltre, ereditarietà delle autorizzazioni è disabilitato su protetti gruppi e account, il che significa che anche se gli account e gruppi vengono spostati in diverse posizioni nella directory, non ereditano le autorizzazioni dai relativi oggetti padre nuovo. Ereditarietà è disabilitato per l'oggetto AdminSDHolder in modo che le modifiche alle autorizzazioni per gli oggetti padre non modificare le autorizzazioni di AdminSDHolder.  

##### <a name="changing-sdprop-interval"></a>Modifica intervallo SDProp  
In genere, non è necessario modificare l'intervallo in cui viene eseguito SDProp, fatta eccezione per scopi di test. Se è necessario modificare l'intervallo SDProp, nell'emulatore PDC del dominio, usare regedit per aggiungere o modificare il valore DWORD AdminSDProtectFrequency HKLM\System\CurrentControlSet\Services\NTDS\Parameters..  

L'intervallo di valori è espresso in secondi da 60 a 7200 (un minuto a due ore). Per annullare le modifiche, eliminare la chiave AdminSDProtectFrequency, che provocherà SDProp ripristinare l'intervallo di 60 minuti. È in genere consigliabile non ridurre questo intervallo nei domini di produzione come è possibile aumentare LSASS overhead di elaborazione nel controller di dominio. L'impatto di tale aumento è dipende dal numero di oggetti protetti nel dominio.  

##### <a name="running-sdprop-manually"></a>Esecuzione manuale SDProp  
Un approccio migliore per testare le modifiche AdminSDHolder consiste nell'eseguire SDProp manualmente, che determina l'esecuzione immediata dell'attività, ma non influisce sulla esecuzione pianificata. Esecuzione manuale SDProp è eseguita in modo leggermente diverso nei controller di dominio che esegue Windows Server 2008 e versioni precedenti rispetto al controller di dominio che eseguono Windows Server 2012 o Windows Server 2008 R2.  

Procedure per l'esecuzione SDProp manualmente su sistemi operativi meno recenti sono disponibili nel [articolo di supporto Microsoft 251343](https://support.microsoft.com/kb/251343), e di seguito sono fornite istruzioni dettagliate per i sistemi operativi meno recenti e più recenti. In entrambi i casi, è necessario connettersi all'oggetto rootDSE in Active Directory ed eseguire un'operazione di modifica con un nome distinto null per l'oggetto rootDSE, specificando il nome dell'operazione come l'attributo su Modifica. Per ulteriori informazioni sulle operazioni modificabile sull'oggetto rootDSE, vedere [rootDSE modificare Operations](https://msdn.microsoft.com/library/cc223297.aspx) sul sito Web MSDN.  

###### <a name="running-sdprop-manually-in-windows-server-2008-or-earlier"></a>Esecuzione manuale SDProp in Windows Server 2008 o versioni precedenti  
È possibile forzare SDProp per l'esecuzione con Ldp.exe o eseguendo uno script di modifica LDAP. Per eseguire SDProp tramite Ldp.exe, eseguire i passaggi seguenti dopo aver apportato le modifiche apportate all'oggetto AdminSDHolder in un dominio:  

1.  Avviare **Ldp.exe**.  

2.  Fare clic su **connessione** nella finestra di dialogo Ldp e fare clic su **Connetti**.  

    ![account protetti e gruppi](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_9.gif)  

3.  Nel **Connetti** finestra di dialogo digitare il nome del controller di dominio per il dominio che detiene il ruolo emulatore PDC (emulatore PDC), fare clic su **OK**.  

    ![account protetti e gruppi](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_10.png)  

4.  Verificare che è stata stabilita, come indicato dalla **Dn: (RootDSE)** nella schermata seguente, fare clic su **connessione** e fare clic su **binding**.  

    ![account protetti e gruppi](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_11.png)  

5.  Nel **binding** finestra di dialogo, digitare le credenziali di un account utente che dispone dell'autorizzazione per modificare l'oggetto rootDSE. (Se si è connessi come utente, è possibile selezionare **binding come** utente attualmente connesso.) Fare clic su **OK**.  

    ![account protetti e gruppi](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_12.png)  

6.  Dopo aver completato l'operazione di binding, fare clic su **Sfoglia**e fare clic su **modifica**.  

    ![account protetti e gruppi](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_13.png)  

7.  Nel **modifica** la finestra di dialogo, lasciare il **DN** campo vuoto. Nel **Modifica attributo voce** digitare **FixUpInheritance**e il **valori** , digitare **Sì**. Fare clic su **invio** per popolare il **elenco voci** come illustrato nella schermata seguente.  

    ![account protetti e gruppi](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_14.gif)  

8.  Nella casella di dialogo Modifica popolato, fare clic su Esegui e verificare che le modifiche apportate all'oggetto AdminSDHolder visualizzata su tale oggetto.  

> [!NOTE]  
> Per informazioni sulla modifica AdminSDHolder per consentire a un account senza privilegiato designati modificare l'appartenenza di gruppi protetti, vedere [appendice i: creazione di account di gestione per gli account protetti e gruppi in Active Directory](../../../ad-ds/manage/component-updates/Appendix-I--Creating-Management-Accounts-for-Protected-Accounts-and-Groups-in-Active-Directory.md).  

Se si preferisce eseguire SDProp manualmente tramite LDIFDE o uno script, è possibile creare una voce di modifica come illustrato di seguito:  

![account protetti e gruppi](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_15.gif)  

###### <a name="running-sdprop-manually-in-windows-server-2012-or-windows-server-2008-r2"></a>Esecuzione manuale SDProp in Windows Server 2012 o Windows Server 2008 R2  
È anche possibile forzare SDProp per l'esecuzione con Ldp.exe o eseguendo uno script di modifica LDAP. Per eseguire SDProp tramite Ldp.exe, eseguire i passaggi seguenti dopo aver apportato le modifiche apportate all'oggetto AdminSDHolder in un dominio:  

1.  Avviare **Ldp.exe**.  

2.  Nel **Ldp** la finestra di dialogo, fare clic su **connessione**e fare clic su **Connetti**.  

    ![account protetti e gruppi](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_16.gif)  

3.  Nel **Connetti** finestra di dialogo digitare il nome del controller di dominio per il dominio che detiene il ruolo emulatore PDC (emulatore PDC), fare clic su **OK**.  

    ![account protetti e gruppi](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_17.gif)  

4.  Verificare che è stata stabilita, come indicato dalla **Dn: (RootDSE)** nella schermata seguente, fare clic su **connessione** e fare clic su **binding**.  

    ![account protetti e gruppi](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_18.gif)  

5.  Nel **binding** finestra di dialogo, digitare le credenziali di un account utente che dispone dell'autorizzazione per modificare l'oggetto rootDSE. (Se si è connessi come utente, è possibile selezionare **binding come utente connesso**.) Fare clic su **OK**.  

    ![account protetti e gruppi](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_19.gif)  

6.  Dopo aver completato l'operazione di binding, fare clic su **Sfoglia**e fare clic su **modifica**.  

    ![account protetti e gruppi](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_20.gif)  

7.  Nel **modifica** la finestra di dialogo, lasciare il **DN** campo vuoto. Nel **Modifica attributo voce** digitare **RunProtectAdminGroupsTask**e il **valori** , digitare **1**. Fare clic su **invio** per popolare l'elenco di voci, come illustrato di seguito.  

    ![account protetti e gruppi](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_21.gif)  

8.  In popolato **modifica** la finestra di dialogo, fare clic su **eseguire**e verificare che le modifiche apportate all'oggetto AdminSDHolder visualizzata su tale oggetto.  

Se si preferisce eseguire SDProp manualmente tramite LDIFDE o uno script, è possibile creare una voce di modifica come illustrato di seguito:  

![account protetti e gruppi](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_22.gif)  
