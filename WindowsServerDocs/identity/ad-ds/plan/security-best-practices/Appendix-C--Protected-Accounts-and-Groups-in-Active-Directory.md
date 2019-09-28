---
ms.assetid: 5b2876ac-fe7d-4054-bfba-b692e57bc0d2
title: Appendice C-account e gruppi protetti in Active Directory
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 606b3a42d70ee5c2a3479f9c9df2f95a495d6afd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408729"
---
# <a name="appendix-c-protected-accounts-and-groups-in-active-directory"></a>Appendice C: Account e gruppi protetti in Active Directory

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

## <a name="appendix-c-protected-accounts-and-groups-in-active-directory"></a>Appendice C: Account e gruppi protetti in Active Directory

All'interno Active Directory, un set predefinito di account e gruppi con privilegi elevati viene considerato come account e gruppi protetti. Con la maggior parte degli oggetti in Active Directory, gli amministratori delegati (gli utenti a cui sono state delegate le autorizzazioni per la gestione di oggetti Active Directory) possono modificare le autorizzazioni per gli oggetti, inclusa la modifica delle autorizzazioni per consentire la modifica delle appartenenze di i gruppi, ad esempio.  

Tuttavia, con gli account e i gruppi protetti, le autorizzazioni degli oggetti vengono impostate e applicate tramite un processo automatico che garantisce che le autorizzazioni per gli oggetti rimangano coerenti anche se gli oggetti vengono spostati nella directory. Anche se un utente modifica manualmente le autorizzazioni di un oggetto protetto, questo processo assicura che le autorizzazioni vengano restituite rapidamente ai valori predefiniti.  

### <a name="protected-groups"></a>Gruppi protetti

La tabella seguente contiene i gruppi protetti in Active Directory elencati dal sistema operativo del controller di dominio.  

#### <a name="protected-accounts-and-groups-in-active-directory-by-operating-system"></a>Account e gruppi protetti in Active Directory dal sistema operativo

| Windows Server 2003 RTM | Windows Server 2003 SP1 + | Windows Server 2012, <br> Windows Server 2008 R2, <br> Windows Server 2008 | Windows Server 2016 |
| --- | --- | --- | --- |
|Account Operators|Account Operators|Account Operators|Account Operators|
|Administrator|Administrator|Administrator|Administrator|
|Administrators|Administrators|Administrators|Administrators|
|Backup Operators|Backup Operators|Backup Operators|Backup Operators|
|Cert Publishers|||
|Domain Admins|Domain Admins|Domain Admins|Domain Admins|
|Controller di dominio|Controller di dominio|Controller di dominio|Controller di dominio|
|Enterprise Admins|Enterprise Admins|Enterprise Admins|Enterprise Admins|
||||Enterprise Key Admins|
||||Amministratori chiave|
|Krbtgt|Krbtgt|Krbtgt|Krbtgt|
|Print Operators|Print Operators|Print Operators|Print Operators|
|||Controller di dominio di sola lettura|Controller di dominio di sola lettura|
|Replicator|Replicator|Replicator|Replicator|
|Schema Admins|Schema Admins|Schema Admins|Schema Admins|
|Server Operators|Server Operators|Server Operators|Server Operators|

#### <a name="adminsdholder"></a>AdminSDHolder

Lo scopo dell'oggetto AdminSDHolder è fornire le autorizzazioni "template" per gli account e i gruppi protetti nel dominio. AdminSDHolder viene creato automaticamente come oggetto nel contenitore di sistema di ogni dominio Active Directory. Il percorso è: **CN = AdminSDHolder, CN = System, DC = < domain_component >, DC = < domain_component >?.**  

A differenza della maggior parte degli oggetti del dominio Active Directory, di proprietà del gruppo Administrators, AdminSDHolder è di proprietà del gruppo Domain Admins. Per impostazione predefinita, EAs può apportare modifiche all'oggetto AdminSDHolder di qualsiasi dominio, così come i gruppi Domain Admins e Administrators del dominio. Inoltre, anche se il proprietario predefinito di AdminSDHolder è il gruppo Domain Admins del dominio, i membri di amministratori o Enterprise Admins possono assumere la proprietà dell'oggetto.  

#### <a name="sdprop"></a>SDProp

SDProp è un processo che viene eseguito ogni 60 minuti (per impostazione predefinita) sul controller di dominio che include l'emulatore PDC del dominio (emulatore PDC). SDProp Confronta le autorizzazioni per l'oggetto AdminSDHolder del dominio con le autorizzazioni per gli account e i gruppi protetti nel dominio. Se le autorizzazioni per uno degli account e dei gruppi protetti non corrispondono alle autorizzazioni per l'oggetto AdminSDHolder, le autorizzazioni per gli account e i gruppi protetti vengono reimpostate in modo da corrispondere a quelle dell'oggetto AdminSDHolder del dominio.  

Inoltre, l'ereditarietà delle autorizzazioni è disabilitata per i gruppi e gli account protetti, il che significa che anche se gli account e i gruppi vengono spostati in percorsi diversi nella directory, non ereditano le autorizzazioni dai nuovi oggetti padre. L'ereditarietà è disabilitata nell'oggetto AdminSDHolder in modo che le modifiche alle autorizzazioni per gli oggetti padre non modifichino le autorizzazioni di AdminSDHolder.  

##### <a name="changing-sdprop-interval"></a>Modifica dell'intervallo di SDProp

In genere, non è necessario modificare l'intervallo di esecuzione di SDProp, eccetto a scopo di test. Se è necessario modificare l'intervallo di SDProp, in emulatore PDC per il dominio usare regedit per aggiungere o modificare il valore DWORD AdminSDProtectFrequency in HKLM\SYSTEM\CurrentControlSet\Services\NTDS\Parameters.  

L'intervallo di valori è in secondi da 60 a 7200 (da un minuto a due ore). Per invertire le modifiche, eliminare la chiave AdminSDProtectFrequency, in modo che SDProp ritorni all'intervallo di 60 minuti. In genere non è consigliabile ridurre questo intervallo nei domini di produzione perché può aumentare l'overhead di elaborazione LSASS sul controller di dominio. L'effetto di questo aumento dipende dal numero di oggetti protetti nel dominio.  

##### <a name="running-sdprop-manually"></a>Esecuzione manuale di SDProp

Un approccio migliore per il test delle modifiche di AdminSDHolder è l'esecuzione manuale di SDProp, che fa sì che l'attività venga eseguita immediatamente, ma non influisce sull'esecuzione pianificata. L'esecuzione manuale di SDProp viene eseguita in modo leggermente diverso nei controller di dominio che eseguono Windows Server 2008 e versioni precedenti rispetto ai controller di dominio che eseguono Windows Server 2012 o Windows Server 2008 R2.  

Le procedure per l'esecuzione manuale di SDProp nei sistemi operativi precedenti sono disponibili nell' [articolo supporto tecnico Microsoft 251343](https://support.microsoft.com/kb/251343)e seguono le istruzioni dettagliate per i sistemi operativi precedenti e più recenti. In entrambi i casi, è necessario connettersi all'oggetto rootDSE in Active Directory ed eseguire un'operazione di modifica con un DN null per l'oggetto rootDSE, specificando il nome dell'operazione come attributo da modificare. Per ulteriori informazioni sulle operazioni modificabili sull'oggetto rootDSE, vedere [operazioni di modifica RootDSE](https://msdn.microsoft.com/library/cc223297.aspx) sul sito Web MSDN.  

###### <a name="running-sdprop-manually-in-windows-server-2008-or-earlier"></a>Esecuzione manuale di SDProp in Windows Server 2008 o versioni precedenti

È possibile forzare l'esecuzione di SDProp con LDP. exe o eseguendo uno script di modifica LDAP. Per eseguire SDProp utilizzando LDP. exe, eseguire la procedura seguente dopo avere apportato modifiche all'oggetto AdminSDHolder in un dominio:  

1. Avviare **LDP. exe**.  
2. Fare clic su **connessione** nella finestra di dialogo LDP e quindi su **Connetti**.  

   ![account e gruppi protetti](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_9.gif)  

3. Nella finestra di dialogo **Connetti** Digitare il nome del controller di dominio per il dominio che include il ruolo emulatore PDC (emulatore PDC) e fare clic su **OK**.  

   ![account e gruppi protetti](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_10.png)  

4. Verificare che la connessione sia stata eseguita correttamente, come indicato da **Dn: (RootDSE)**  nella schermata seguente, fare clic su **connessione** e quindi su **associa**.  

   ![account e gruppi protetti](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_11.png)  

5. Nella finestra di dialogo **associa** Digitare le credenziali di un account utente che dispone dell'autorizzazione per modificare l'oggetto RootDSE. Se si è connessi con tale utente, è possibile selezionare **associa come** utente attualmente connesso. Scegliere **OK**.  

   ![account e gruppi protetti](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_12.png)  

6. Al termine dell'operazione di binding, fare clic su **Sfoglia**e quindi su **modifica**.  

   ![account e gruppi protetti](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_13.png)  

7. Nella finestra di dialogo **modifica** lasciare vuoto il campo **DN** . Nel campo **modifica voce attributo** digitare **FixUpInheritance**, quindi digitare **Yes**nel campo **valori** . Fare clic su **invio** per popolare l' **elenco di voci** come illustrato nello screenshot seguente.  

   ![account e gruppi protetti](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_14.gif)  

8. Nella finestra di dialogo modifica compilata fare clic su Esegui e verificare che le modifiche apportate all'oggetto AdminSDHolder siano visualizzate su tale oggetto.  

> [!NOTE]  
> Per informazioni sulla modifica di AdminSDHolder per consentire agli account senza privilegi designati di modificare l'appartenenza dei gruppi protetti, vedere [Appendix I: Creazione di account di gestione per account e gruppi protetti in Active Directory @ no__t-0.  

Se si preferisce eseguire manualmente SDProp tramite LDIFDE o uno script, è possibile creare una voce di modifica, come illustrato di seguito:  

![account e gruppi protetti](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_15.gif)  

###### <a name="running-sdprop-manually-in-windows-server-2012-or-windows-server-2008-r2"></a>Esecuzione manuale di SDProp in Windows Server 2012 o Windows Server 2008 R2

È anche possibile forzare l'esecuzione di SDProp con LDP. exe o eseguendo uno script di modifica LDAP. Per eseguire SDProp utilizzando LDP. exe, eseguire la procedura seguente dopo avere apportato modifiche all'oggetto AdminSDHolder in un dominio:  

1. Avviare **LDP. exe**.  

2. Nella finestra di dialogo **LDP** fare clic su **connessione**e quindi su **Connetti**.  

   ![account e gruppi protetti](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_16.gif)  

3. Nella finestra di dialogo **Connetti** Digitare il nome del controller di dominio per il dominio che include il ruolo emulatore PDC (emulatore PDC) e fare clic su **OK**.  

   ![account e gruppi protetti](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_17.gif)  

4. Verificare che la connessione sia stata eseguita correttamente, come indicato da **Dn: (RootDSE)**  nella schermata seguente, fare clic su **connessione** e quindi su **associa**.  

   ![account e gruppi protetti](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_18.gif)  

5. Nella finestra di dialogo **associa** Digitare le credenziali di un account utente che dispone dell'autorizzazione per modificare l'oggetto RootDSE. Se si è connessi con tale utente, è possibile selezionare **associa come utente attualmente connesso**. Scegliere **OK**.  

   ![account e gruppi protetti](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_19.gif)  

6. Al termine dell'operazione di binding, fare clic su **Sfoglia**e quindi su **modifica**.  

   ![account e gruppi protetti](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_20.gif)  

7. Nella finestra di dialogo **modifica** lasciare vuoto il campo **DN** . Nel campo **Edit Entry Attribute** digitare **RunProtectAdminGroupsTask**e nel campo **values** digitare **1**. Fare clic su **invio** per popolare l'elenco di voci come illustrato di seguito.  

   ![account e gruppi protetti](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_21.gif)  

8. Nella finestra di dialogo **modifica** compilata fare clic su **Esegui**e verificare che le modifiche apportate all'oggetto AdminSDHolder siano visualizzate su tale oggetto.  

Se si preferisce eseguire manualmente SDProp tramite LDIFDE o uno script, è possibile creare una voce di modifica, come illustrato di seguito:  

![account e gruppi protetti](media/Appendix-C--Protected-Accounts-and-Groups-in-Active-Directory/SAD_22.gif)  
