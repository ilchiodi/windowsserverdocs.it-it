---
ms.assetid: 074e63e9-976c-49da-8cba-9ae0b3325e34
title: Introduzione ai miglioramenti apportati centro di amministrazione di Active Directory (livello 100)
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 7f52b22ec74ba12c383952e68b412f871a56474c
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="introduction-to-active-directory-administrative-center-enhancements-level-100"></a>Introduzione ai miglioramenti apportati centro di amministrazione di Active Directory (livello 100)

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

CENTRO in Windows Server 2012 include funzionalità di gestione per le operazioni seguenti:

-   [Cestino per Active Directory](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#ad_recycle_bin_mgmt)

-   [Criteri granulari per le Password](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#fine_grained_pswd_policy_mgmt)

-   [Visualizzatore della cronologia di PowerShell di Windows](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#windows_powershell_history_viewer)

## <a name="ad_recycle_bin_mgmt"></a>Cestino per Active Directory
L'eliminazione accidentale di oggetti Active Directory è un problema comune per gli utenti di servizi di dominio Active Directory (AD DS) e Active Directory Lightweight Directory Services (AD LDS). Nelle versioni precedenti di Windows Server, prima di Windows Server 2008 R2, è possibile ripristinare oggetti eliminati accidentalmente in Active Directory, ma le soluzioni presentano svantaggi.

In Windows Server 2008, è possibile utilizzare la funzionalità di Windows Server Backup e **ntdsutil** comando ripristino autorevole per contrassegnare gli oggetti come autorevoli e garantire che i dati ripristinati nell'intero dominio. Lo svantaggio per la soluzione di ripristino autorevole è doveva essere eseguita in ripristino servizi Directory modalità (). Durante la modalità ripristino servizi directory, il controller di dominio da ripristinare deve rimanere offline. Pertanto, non è stato in grado di soddisfare le richieste client.

In Active Directory di Windows Server 2003 e Windows Server 2008 Active Directory, è possibile ripristinare oggetti Active Directory eliminati tramite scoprirai. Tuttavia, rianimati attributi degli oggetti con valori di collegamento (ad esempio l'appartenenza dell'account utente) che sono stati rimossi fisicamente e non vengono ripristinati gli attributi con valori collegamento non sono stati cancellati. Di conseguenza, gli amministratori possono non avvalersi scoprirai come soluzione ideale per l'eliminazione accidentale di oggetti. Per ulteriori informazioni su scoprirai, vedere [recupero degli oggetti Active Directory per la rimozione definitiva](https://go.microsoft.com/fwlink/?LinkID=125452).

Cestino per Active Directory, a partire da Windows Server 2008 R2, si basa sull'infrastruttura esistente per la rimozione definitiva e amplia le possibilità di salvaguardare e recuperare oggetti Active Directory eliminati accidentalmente.

Quando si abilita Cestino per Active Directory, tutti con valori di collegamento e gli attributi non-con valori di collegamento degli oggetti Active Directory eliminati vengono mantenuti e gli oggetti vengono interamente ripristinati allo stesso stato logico coerenza in cui erano immediatamente prima dell'eliminazione. Ad esempio, gli account utente ripristinati riottengono automaticamente tutte le appartenenze ai gruppi e i diritti di accesso corrispondenti che avevano immediatamente prima dell'eliminazione, all'interno e tra domini. Cestino per Active Directory supporta gli ambienti di dominio Active Directory sia AD LDS. Per una descrizione dettagliata del Cestino per Active Directory, vedere [What's New in Active Directory: Cestino per Active Directory](https://technet.microsoft.com/library/dd391916(WS.10).aspx).

**Novità?** In Windows Server 2012, la funzionalità Cestino per Active Directory è stata migliorata con una nuova interfaccia utente grafica per gli utenti di gestire e ripristinare oggetti eliminati. Gli utenti possono ora visivamente individuare un elenco di oggetti eliminati e ripristinarli nei percorsi originali o desiderato.

Se si intende abilitare Cestino per Active Directory in Windows Server 2012, considerare quanto segue:

-   Per impostazione predefinita, Cestino per Active Directory è disabilitato. Per abilitarlo, è prima necessario aumentare il livello di funzionalità della foresta dell'ambiente di dominio Active Directory o AD LDS a Windows Server 2008 R2 o versione successiva. A sua volta richiede che tutti i controller di dominio nella foresta o tutti i server che ospitano istanze dei set di configurazione di AD LDS in esecuzione Windows Server 2008 R2 o versione successiva.

-   Il processo di abilitazione del Cestino per Active Directory è irreversibile. Dopo aver abilitato Cestino per Active Directory nell'ambiente in uso, è possibile disabilitarlo.

-   Per gestire la funzionalità Cestino tramite un'interfaccia utente, è necessario installare la versione di centro di amministrazione di Active Directory in Windows Server 2012.

    > [!NOTE]
    > È possibile utilizzare **Server Manager** per installare strumenti di amministrazione remota Server (RSAT) nei computer Windows Server 2012 per utilizzare la versione corretta di centro di amministrazione di Active Directory per gestire il Cestino tramite un'interfaccia utente.
    > 
    > È possibile utilizzare [RSAT](https://go.microsoft.com/fwlink/?LinkID=238560) in Windows&reg; 8 computer come utilizzare la versione corretta di centro di amministrazione di Active Directory per gestire il Cestino tramite un'interfaccia utente.

### <a name="active-directory-recycle-bin-step-by-step"></a>Cestino per Active Directory dettagliate
Nei passaggi seguenti si utilizzerà centro di eseguire le attività seguenti Cestino per Active Directory in Windows Server 2012:

-   [Passaggio 1: Aumentare il livello di funzionalità foresta](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_raise_ffl)

-   [Passaggio 2: Abilitare il Cestino](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_enable_recycle_bin)

-   [Passaggio 3: Creare utenti di test, gruppo e unità organizzativa](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_test_env)

-   [Passaggio 4: Ripristinare gli oggetti eliminati](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_restore_del_obj)

> [!NOTE]
> Appartenenza al gruppo Enterprise Admins o disporre di autorizzazioni equivalenti è necessario per eseguire la procedura seguente.

### <a name="bkmk_raise_ffl"></a>Passaggio 1: Aumentare il livello di funzionalità foresta
In questo passaggio si aumenterà il livello di funzionalità foresta. È prima necessario aumentare il livello di funzionalità in una foresta di destinazione da Windows Server 2008 R2 come minimo, prima di abilitare Cestino per Active Directory.

##### <a name="to-raise-the-functional-level-on-the-target-forest"></a>Per aumentare il livello di funzionalità in una foresta di destinazione

1.  Fare clic sull'icona di Windows PowerShell, fare clic su **Esegui come amministratore** e il tipo **dsac.exe** per aprire Centro.

2.  Fare clic su **Gestisci**, fare clic su **Aggiungi nodi di spostamento** e selezionare il dominio di destinazione appropriato nel **Aggiungi nodi di spostamento** la finestra di dialogo e quindi fare clic su **OK**.

3.  Fare clic sul dominio di destinazione nel riquadro di spostamento a sinistra e nel **attività** riquadro, fare clic su **aumentare il livello funzionale di foresta**. Selezionare un livello funzionalità foresta pari ad almeno Windows Server 2008 R2 o versione successiva e quindi fare clic su **OK**.

![Introduzione all'interfaccia di amministrazione di Active Directory](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *

Il file o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet su una singola riga, anche se può sembrare che siano divisi su più righe a causa dei limiti di formattazione.

```
Set-ADForestMode -Identity contoso.com -ForestMode Windows2008R2Forest -Confirm:$false
```

Per il **-identità** argomento, specificare il nome DNS completo.

### <a name="bkmk_enable_recycle_bin"></a>Passaggio 2: Abilitare il Cestino
In questo passaggio si abiliterà il Cestino per ripristinare oggetti eliminati in servizi di dominio Active Directory.

##### <a name="to-enable-active-directory-recycle-bin-in-adac-on-the-target-domain"></a>Per abilitare Cestino per Active Directory in centro sul dominio di destinazione

1.  Fare clic sull'icona di Windows PowerShell, fare clic su **Esegui come amministratore** e il tipo **dsac.exe** per aprire Centro.

2.  Fare clic su **Gestisci**, fare clic su **Aggiungi nodi di spostamento** e selezionare il dominio di destinazione appropriato nel **Aggiungi nodi di spostamento** la finestra di dialogo e quindi fare clic su **OK**.

3.  Nel **attività** riquadro, fare clic su **Abilita Cestino... **nel **attività** riquadro, fare clic su **OK** nella finestra di messaggio di avviso e quindi fare clic su **OK** al messaggio di centro di aggiornamento.

4.  Premere F5 per aggiornare centro.

![Introduzione all'interfaccia di amministrazione di Active Directory](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *

Il file o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet su una singola riga, anche se può sembrare che siano divisi su più righe a causa dei limiti di formattazione.

```
Enable-ADOptionalFeature -Identity 'CN=Recycle Bin Feature,CN=Optional Features,CN=Directory Service,CN=Windows NT,CN=Services,CN=Configuration,DC=contoso,DC=com' -Scope ForestOrConfigurationSet -Target 'contoso.com'
```

### <a name="bkmk_create_test_env"></a>Passaggio 3: Creare utenti di test, gruppo e unità organizzativa
Nelle procedure seguenti, si creerà due utenti di prova. È quindi verrà creato un gruppo di test e aggiungere gli utenti al gruppo. Inoltre, si creerà un'unità Organizzativa.

##### <a name="to-create-test-users"></a>Per creare gli utenti di test

1.  Fare clic sull'icona di Windows PowerShell, fare clic su **Esegui come amministratore** e il tipo **dsac.exe** per aprire Centro.

2.  Fare clic su **Gestisci**, fare clic su **Aggiungi nodi di spostamento** e selezionare il dominio di destinazione appropriato nel **Aggiungi nodi di spostamento** la finestra di dialogo e quindi fare clic su **OK**.

3.  Nel **attività** riquadro, fare clic su **New** e quindi fare clic su **utente**.

    ![Introduzione all'interfaccia di amministrazione di Active Directory](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/ADDS_ADACNewUser.gif)

4.  Immettere le informazioni seguenti in **Account** e quindi fare clic su OK:

    -   Nome completo: test1

    -   Accesso utente SamAccountName: test1

    -   Password:p@ssword1

    -   Conferma password:p@ssword1

5.  Ripetere i passaggi precedenti per creare un secondo utente test2.

##### <a name="to-create-a-test-group-and-add-users-to-the-group"></a>Per creare un gruppo di test e aggiungere utenti al gruppo

1.  Fare clic sull'icona di Windows PowerShell, fare clic su **Esegui come amministratore** e il tipo **dsac.exe** per aprire Centro.

2.  Fare clic su **Gestisci**, fare clic su **Aggiungi nodi di spostamento** e selezionare il dominio di destinazione appropriato nel **Aggiungi nodi di spostamento** la finestra di dialogo e quindi fare clic su **OK**.

3.  Nel **attività** riquadro, fare clic su **New** e quindi fare clic su **gruppo**.

4.  Immettere le informazioni seguenti in **gruppo** e quindi fare clic su **OK**:

    -   **Nome gruppo: group1**

5.  Fare clic su **group1**e quindi nel **attività** riquadro, fare clic su **proprietà**.

6.  Fare clic su **membri**, fare clic su **Aggiungi**, tipo **test1; test2**, quindi fare clic su **OK**.

![Introduzione all'interfaccia di amministrazione di Active Directory](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *

Il file o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet su una singola riga, anche se può sembrare che siano divisi su più righe a causa dei limiti di formattazione.

```
Add-ADGroupMember -Identity group1 -Member test1
```

##### <a name="to-create-an-organizational-unit"></a>Per creare un'unità organizzativa

1.  Fare clic sull'icona di Windows PowerShell, fare clic su **Esegui come amministratore** e il tipo **dsac.exe** per aprire Centro.

2.  Fare clic su **Gestisci**, fare clic su **Aggiungi nodi di spostamento** e selezionare il dominio di destinazione appropriato nel **Aggiungi nodi di spostamento** la finestra di dialogo e quindi fare clic su **OK**.

3.  Nel **attività** riquadro, fare clic su **New** e quindi fare clic su **unità organizzativa**.

4.  Immettere le informazioni seguenti in **unità organizzativa** e quindi fare clic su **OK**:

    -   **NameOU1**

![Introduzione all'interfaccia di amministrazione di Active Directory](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *

Il file o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet su una singola riga, anche se può sembrare che siano divisi su più righe a causa dei limiti di formattazione.

```
1..2 | ForEach-Object {New-ADUser -SamAccountName test$_ -Name "test$_" -Path "DC=fabrikam,DC=com" -AccountPassword (ConvertTo-SecureString -AsPlainText "p@ssword1" -Force) -Enabled $true}
New-ADGroup -Name "group1" -SamAccountName group1 -GroupCategory Security -GroupScope Global -DisplayName "group1"
New-ADOrganizationalUnit -Name OU1 -Path "DC=fabrikam,DC=com"

```

### <a name="bkmk_restore_del_obj"></a>Passaggio 4: Ripristinare gli oggetti eliminati
Nelle procedure seguenti, ripristinare oggetti eliminati dal **oggetti eliminati** contenitore nel percorso originale e in un percorso diverso.

##### <a name="to-restore-deleted-objects-to-their-original-location"></a>Per ripristinare oggetti eliminati nel percorso originale

1.  Fare clic sull'icona di Windows PowerShell, fare clic su **Esegui come amministratore** e il tipo **dsac.exe** per aprire Centro.

2.  Fare clic su **Gestisci**, fare clic su **Aggiungi nodi di spostamento** e selezionare il dominio di destinazione appropriato nel **Aggiungi nodi di spostamento** la finestra di dialogo e quindi fare clic su **OK**.

3.  Selezionare gli utenti **test1** e **test2**, fare clic su **eliminare** nel **attività** riquadro e quindi fare clic su **Sì** per confermare l'eliminazione.

    ![Introduzione all'interfaccia di amministrazione di Active Directory](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *

    Il file o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet su una singola riga, anche se può sembrare che siano divisi su più righe a causa dei limiti di formattazione.

    ```
    Get-ADUser -Filter 'Name -Like "*test*"'|Remove-ADUser -Confirm:$false
    ```

4.  Passare al **oggetti eliminati** contenitore, selezionare **test2** e **test1** e quindi fare clic su **ripristinare** nel **attività** riquadro.

5.  Per verificare che gli oggetti sono stati ripristinati nel percorso originale, passare al dominio di destinazione e verificare che siano elencati gli account utente.

    > [!NOTE]
    > Se si passa al **proprietà** degli account utente **test1** e **test2** e quindi fare clic su **membro di**, si noterà che è stata ripristinata anche l'appartenenza ai gruppi.

Il file o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet su una singola riga, anche se può sembrare che siano divisi su più righe a causa dei limiti di formattazione.

![Introduzione all'interfaccia di amministrazione di Active Directory](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *

```
Get-ADObject -Filter 'Name -Like "*test*"' -IncludeDeletedObjects | Restore-ADObject
```

##### <a name="to-restore-deleted-objects-to-a-different-location"></a>Per ripristinare oggetti eliminati in un percorso diverso

1.  Fare clic sull'icona di Windows PowerShell, fare clic su **Esegui come amministratore** e il tipo **dsac.exe** per aprire Centro.

2.  Fare clic su **Gestisci**, fare clic su **Aggiungi nodi di spostamento** e selezionare il dominio di destinazione appropriato nel **Aggiungi nodi di spostamento** la finestra di dialogo e quindi fare clic su **OK**.

3.  Selezionare gli utenti **test1** e **test2**, fare clic su **eliminare** nel **attività** riquadro e quindi fare clic su **Sì** per confermare l'eliminazione.

4.  Passare al **oggetti eliminati** contenitore, selezionare **test2** e **test1** e quindi fare clic su **Ripristina in** nel **attività** riquadro.

5.  Selezionare **OU1** e quindi fare clic su **OK**.

6.  Per confermare che gli oggetti sono stati ripristinati **OU1**, passare al dominio di destinazione, fare doppio clic su **OU1** e verificare che siano elencati gli account utente.

![Introduzione all'interfaccia di amministrazione di Active Directory](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *

Il file o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet su una singola riga, anche se può sembrare che siano divisi su più righe a causa dei limiti di formattazione.

```
Get-ADObject -Filter 'Name -Like "*test*"' -IncludeDeletedObjects | Restore-ADObject -TargetPath "OU=OU1,DC=contoso,DC=com"
```

## <a name="fine_grained_pswd_policy_mgmt"></a>Criteri granulari per le Password
Il sistema operativo Windows Server 2008 offre alle organizzazioni un modo per definire i criteri di blocco degli account per diversi set di utenti e password diverse in un dominio. Nei domini di Active Directory prima di Windows Server 2008, un solo criterio password e criterio di blocco account è possibile applicare a tutti gli utenti nel dominio. Questi criteri sono stati specificati nel criterio dominio predefinito per il dominio. Di conseguenza, le organizzazioni che vogliono diverse impostazioni di blocco account e password per diversi set di utenti dovevano creare un filtro password o distribuire più domini. Entrambi sono opzioni costose.

È possibile utilizzare criteri granulari per le password per specificare più criteri password in un unico dominio e applicare restrizioni diverse per i criteri di blocco degli account e password per diversi set di utenti in un dominio. Ad esempio, è possibile applicare impostazioni più rigide ad account con privilegi e impostazioni meno rigide per gli account di altri utenti. In altri casi, è possibile applicare criteri password speciali per gli account le cui password vengono sincronizzate con altre origini dati. Per una descrizione dettagliata dei criteri Password granulari, vedere [di dominio Active Directory: i criteri granulari per le Password](https://technet.microsoft.com/library/cc770394(WS.10).aspx)

**Novità?** In Windows Server 2012, gestione dei criteri granulari per le password è più facile e più visual fornendo un'interfaccia utente per gli amministratori di dominio Active Directory per gestirli in centro. Ora gli amministratori possono visualizzare i criteri risultanti di un determinato utente, visualizzare e ordinare tutti i criteri password in un dominio specifico e gestire visivamente singoli criteri password.

Se si prevede di usare i criteri granulari per le password in Windows Server 2012, considerare quanto segue:

-   Criteri granulari per le password si applicano solo gruppi di sicurezza globali e gli oggetti utente (o a oggetti inetOrgPerson se vengono utilizzati invece gli oggetti utente). Per impostazione predefinita, solo i membri del gruppo Domain Admins possono impostare criteri granulari per le password. Tuttavia, è inoltre possibile delegare la possibilità di impostare questi criteri ad altri utenti. Il livello di funzionalità del dominio deve essere Windows Server 2008 o versione successiva.

-   È necessario utilizzare la versione di Windows Server 2012 del centro di amministrazione di Active Directory per amministrare criteri granulari per le password tramite un'interfaccia utente grafica.

    > [!NOTE]
    > È possibile utilizzare **Server Manager** per installare strumenti di amministrazione remota Server (RSAT) nei computer Windows Server 2012 per utilizzare la versione corretta di centro di amministrazione di Active Directory per gestire il Cestino tramite un'interfaccia utente.
    > 
    > È possibile utilizzare [RSAT](https://go.microsoft.com/fwlink/?LinkID=238560) in Windows&reg; 8 computer come utilizzare la versione corretta di centro di amministrazione di Active Directory per gestire il Cestino tramite un'interfaccia utente.

### <a name="fine-grained-password-policy-step-by-step"></a>Criteri di Password granulari per le istruzioni dettagliate
Nei passaggi seguenti si utilizzerà centro di eseguire le seguenti attività dei criteri granulari per le password:

-   [Passaggio 1: Aumentare il livello di funzionalità dominio](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_raise_dfl)

-   [Passaggio 2: Creare utenti di test, gruppo e unità organizzativa](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk2_test_fgpp)

-   [Passaggio 3: Creare nuovi criteri granulari per le password](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp)

-   [Passaggio 4: Visualizzare un insieme risultante di criteri per un utente](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_view_resultant_fgpp)

-   [Passaggio 5: Modificare un criterio granulare per le password](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_edit_fgpp)

-   [Passaggio 6: Eliminare criteri granulari per le password](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_delete_fgpp)

> [!NOTE]
> Appartenenza al gruppo Domain Admins o disporre di autorizzazioni equivalenti è necessario per eseguire la procedura seguente.

#### <a name="bkmk_raise_dfl"></a>Passaggio 1: Aumentare il livello di funzionalità dominio
Nella procedura seguente si aumenterà il livello di funzionalità del dominio del dominio di destinazione a Windows Server 2008 o versione successiva. Per abilitare i criteri granulari per le password, è necessario un livello di funzionalità del dominio di Windows Server 2008 o versione successiva.

###### <a name="to-raise-the-domain-functional-level"></a>Per aumentare il livello funzionale di dominio

1.  Fare clic sull'icona di Windows PowerShell, fare clic su **Esegui come amministratore** e il tipo **dsac.exe** per aprire Centro.

2.  Fare clic su **Gestisci**, fare clic su **Aggiungi nodi di spostamento** e selezionare il dominio di destinazione appropriato nel **Aggiungi nodi di spostamento** la finestra di dialogo e quindi fare clic su **OK**.

3.  Fare clic sul dominio di destinazione nel riquadro di spostamento a sinistra e nel **attività** riquadro, fare clic su **aumentare il livello di funzionalità dominio**. Selezionare un livello funzionalità foresta pari ad almeno Windows Server 2008 o versione successiva e quindi fare clic su **OK**.

![Introduzione all'interfaccia di amministrazione di Active Directory](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *

Il file o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet su una singola riga, anche se può sembrare che siano divisi su più righe a causa dei limiti di formattazione.

```
Set-ADDomainMode -Identity contoso.com -DomainMode 3
```

#### <a name="bkmk2_test_fgpp"></a>Passaggio 2: Creare utenti di test, gruppo e unità organizzativa
Per creare gli utenti di test e il gruppo per questo passaggio, eseguire le procedure descritte qui: [passaggio 3: creare utenti di test, gruppo e unità organizzativa](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_test_env) (non è necessario creare l'unità Organizzativa per dimostrare i criteri granulari per le password).

#### <a name="bkmk_create_fgpp"></a>Passaggio 3: Creare nuovi criteri granulari per le password
Nella procedura seguente si creerà un nuovo criterio granulare per le password tramite l'interfaccia utente in centro.

###### <a name="to-create-a-new-fine-grained-password-policy"></a>Per creare un nuovo criterio granulare per le password

1.  Fare clic sull'icona di Windows PowerShell, fare clic su **Esegui come amministratore** e il tipo **dsac.exe** per aprire Centro.

2.  Fare clic su **Gestisci**, fare clic su **Aggiungi nodi di spostamento** e selezionare il dominio di destinazione appropriato nel **Aggiungi nodi di spostamento** la finestra di dialogo e quindi fare clic su **OK**.

3.  Nel riquadro di spostamento di centro, aprire il **sistema** contenitore e quindi fare clic su **contenitore Impostazioni Password**.

4.  Nel **attività** riquadro, fare clic su **New**, quindi fare clic su **impostazioni Password**.

    Compilare o modificare i campi nella pagina delle proprietà per creare un nuovo **impostazioni Password** oggetto. Il **nome** e **precedenza** i campi sono obbligatori.

    ![Introduzione all'interfaccia di amministrazione di Active Directory](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/ADDS_ADACNewFGPP.gif)

5.  In **si applica direttamente a**, fare clic su **Aggiungi**, tipo **group1**, quindi fare clic su **OK**.

    Consente di associare l'oggetto Criteri di Password con i membri del gruppo globale creato per l'ambiente di prova.

6.  Fare clic su **OK** per completare la creazione.

![Introduzione all'interfaccia di amministrazione di Active Directory](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *

Il file o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet su una singola riga, anche se può sembrare che siano divisi su più righe a causa dei limiti di formattazione.

```
New-ADFineGrainedPasswordPolicy TestPswd -ComplexityEnabled:$true -LockoutDuration:"00:30:00" -LockoutObservationWindow:"00:30:00" -LockoutThreshold:"0" -MaxPasswordAge:"42.00:00:00" -MinPasswordAge:"1.00:00:00" -MinPasswordLength:"7" -PasswordHistoryCount:"24" -Precedence:"1" -ReversibleEncryptionEnabled:$false -ProtectedFromAccidentalDeletion:$true
Add-ADFineGrainedPasswordPolicySubject TestPswd -Subjects group1
```

#### <a name="bkmk_view_resultant_fgpp"></a>Passaggio 4: Visualizzare un insieme risultante di criteri per un utente
Nella procedura seguente si visualizzeranno le impostazioni password risultanti per un utente che è un membro del gruppo a cui è stato assegnato un criterio granulare per le password in [passaggio 3: creare nuovi criteri granulari per le password](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp).

###### <a name="to-view-a-resultant-set-of-policies-for-a-user"></a>Per visualizzare un insieme risultante di criteri per un utente

1.  Fare clic sull'icona di Windows PowerShell, fare clic su **Esegui come amministratore** e il tipo **dsac.exe** per aprire Centro.

2.  Fare clic su **Gestisci**, fare clic su **Aggiungi nodi di spostamento** e selezionare il dominio di destinazione appropriato nel **Aggiungi nodi di spostamento** la finestra di dialogo e quindi fare clic su **OK**.

3.  Selezionare un utente **test1** che appartiene al gruppo **group1** che è stato associato un criterio granulare per le password all'interno di [passaggio 3: creare nuovi criteri granulari per le password](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp).

4.  Fare clic su **Visualizza impostazioni Password risultanti** nel **attività** riquadro.

5.  Esaminare i criteri di impostazione password e quindi fare clic su **Annulla**.

![Introduzione all'interfaccia di amministrazione di Active Directory](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *

Il file o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet su una singola riga, anche se può sembrare che siano divisi su più righe a causa dei limiti di formattazione.

```
Get-ADUserResultantPasswordPolicy test1
```

#### <a name="bkmk_edit_fgpp"></a>Passaggio 5: Modificare un criterio granulare per le password
Nella procedura seguente si modificherà il criterio granulare per le password creato in [passaggio 3: creare nuovi criteri granulari per le password](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp)

###### <a name="to-edit-a-fine-grained-password-policy"></a>Per modificare un criterio granulare per le password

1.  Fare clic sull'icona di Windows PowerShell, fare clic su **Esegui come amministratore** e il tipo **dsac.exe** per aprire Centro.

2.  Fare clic su **Gestisci**, fare clic su **Aggiungi nodi di spostamento** e selezionare il dominio di destinazione appropriato nel **Aggiungi nodi di spostamento** la finestra di dialogo e quindi fare clic su **OK**.

3.  Nel centro **riquadro di spostamento**, espandere **sistema** e quindi fare clic su **contenitore Impostazioni Password**.

4.  Selezionare il criterio granulare per le password creato in [passaggio 3: creare nuovi criteri granulari per le password](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp) e fare clic su **proprietà** nel **attività** riquadro.

5.  In **Imponi cronologia password**, modificare il valore di **numero di password memorizzate** a **30**.

6.  Fare clic su **OK**.

![Introduzione all'interfaccia di amministrazione di Active Directory](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *

Il file o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet su una singola riga, anche se può sembrare che siano divisi su più righe a causa dei limiti di formattazione.

```
Set-ADFineGrainedPasswordPolicy TestPswd -PasswordHistoryCount:"30"
```

#### <a name="bkmk_delete_fgpp"></a>Passaggio 6: Eliminare criteri granulari per le password

###### <a name="to-delete-a-fine-grained-password-policy"></a>Per eliminare un criterio granulare per le password

1.  Fare clic sull'icona di Windows PowerShell, fare clic su **Esegui come amministratore** e il tipo **dsac.exe** per aprire Centro.

2.  Fare clic su **Gestisci**, fare clic su **Aggiungi nodi di spostamento** e selezionare il dominio di destinazione appropriato nel **Aggiungi nodi di spostamento** la finestra di dialogo e quindi fare clic su **OK**.

3.  Nel riquadro di spostamento del centro, espandere **sistema** e quindi fare clic su **contenitore Impostazioni Password**.

4.  Selezionare il criterio granulare per le password creato in [passaggio 3: creare nuovi criteri granulari per le password](../../../ad-ds/get-started/adac/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-.md#bkmk_create_fgpp) e il **attività** riquadro **proprietà**.

5.  Cancella il **Proteggi da eliminazioni accidentali** casella di controllo e fare clic su **OK**.

6.  Selezionare il criterio granulare per le password e il **attività** riquadro **eliminare**.

7.  Fare clic su **OK** nella finestra di dialogo di conferma.

![Introduzione all'interfaccia di amministrazione di Active Directory](media/Introduction-to-Active-Directory-Administrative-Center-Enhancements--Level-100-/PowerShellLogoSmall.gif)Windows PowerShell equivalente comandi * * *

Il file o i cmdlet di Windows PowerShell seguenti eseguono la stessa funzione della procedura precedente. Immettere ogni cmdlet su una singola riga, anche se può sembrare che siano divisi su più righe a causa dei limiti di formattazione.

```
Set-ADFineGrainedPasswordPolicy -Identity TestPswd -ProtectedFromAccidentalDeletion $False
Remove-ADFineGrainedPasswordPolicy TestPswd -Confirm
```

## <a name="windows_powershell_history_viewer"></a>Visualizzatore della cronologia di PowerShell di Windows
CENTRO è uno strumento di interfaccia utente basato su Windows PowerShell.  In Windows Server 2012, gli amministratori IT possono sfruttare Centro per informazioni su Windows PowerShell per i cmdlet di Active Directory utilizzando il Visualizzatore della cronologia di Windows PowerShell. Quando le azioni vengono eseguite nell'interfaccia utente, il comando di Windows PowerShell equivalente viene visualizzato all'utente nel Visualizzatore della cronologia di Windows PowerShell. Ciò consente agli amministratori di creare script automatizzati e ridurre le attività ripetitive, incrementando la produttività.  Inoltre, questa funzionalità consente di ridurre i tempi di apprendimento di Windows PowerShell per Active Directory e aumenta la confidenza degli utenti di correttezza degli script di automazione creati.

Quando si utilizza il Visualizzatore della cronologia di Windows PowerShell in Windows Server 2012, considerare quanto segue:

-   Per utilizzare il Visualizzatore di Script di Windows PowerShell, è necessario utilizzare la versione di Windows Server 2012 di centro

    > [!NOTE]
    > È possibile utilizzare **Server Manager** per installare strumenti di amministrazione remota Server (RSAT) nei computer Windows Server 2012 per utilizzare la versione corretta di centro di amministrazione di Active Directory per gestire il Cestino tramite un'interfaccia utente.
    > 
    > È possibile utilizzare [RSAT](https://go.microsoft.com/fwlink/?LinkID=238560) in Windows&reg; 8 computer come utilizzare la versione corretta di centro di amministrazione di Active Directory per gestire il Cestino tramite un'interfaccia utente.

-   Avere una conoscenza base di Windows PowerShell. Ad esempio, è necessario conoscere il funzionamento piping in Windows PowerShell. Per ulteriori informazioni sul piping in Windows PowerShell, vedere [Piping e Pipeline in Windows PowerShell](https://technet.microsoft.com/library/ee176927.aspx).

### <a name="windows-powershell-history-viewer-step-by-step"></a>Visualizzatore della cronologia di Windows PowerShell dettagliate
Nella procedura seguente si utilizzerà il Visualizzatore della cronologia di Windows PowerShell nel centro per creare uno script di Windows PowerShell.  Prima di iniziare questa procedura, rimuovere l'utente **test1** dal gruppo, **group1**.

##### <a name="to-construct-a-script-using-powershell-history-viewer"></a>Per creare uno script con il Visualizzatore della cronologia di PowerShell

1.  Fare clic sull'icona di Windows PowerShell, fare clic su **Esegui come amministratore** e il tipo **dsac.exe** per aprire Centro.

2.  Fare clic su **Gestisci**, fare clic su **Aggiungi nodi di spostamento** e selezionare il dominio di destinazione appropriato nel **Aggiungi nodi di spostamento** la finestra di dialogo e quindi fare clic su **OK**.

3.  Espandere il **cronologia di Windows PowerShell** riquadro nella parte inferiore dello schermo centro.

4.  Selezionare l'utente **test1**.

5.  Fare clic su **Aggiungi al gruppo... **nel **attività** riquadro.

6.  Passare a **group1** e fare clic su **OK** nella finestra di dialogo.

7.  Passare al **cronologia di Windows PowerShell** riquadro e individuare il comando appena generato.

8.  Copiare il comando e incollarlo nell'editor desiderato per creare lo script.

    Ad esempio, è possibile modificare il comando per aggiungere un altro utente **group1**, o aggiungere **test1** in un altro gruppo.

## <a name="see-also"></a>Vedere anche
[Avanzate AD DS gestione utilizzando Centro di amministrazione di Active Directory & #40; Livello 200 & #41;](Advanced-AD-DS-Management-Using-Active-Directory-Administrative-Center--Level-200-.md)


