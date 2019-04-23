---
title: Composta di servizi di dominio di Active Directory e l'autenticazione delle attestazioni in Active Directory Federation Services
description: Il documento seguente illustra l'autenticazione composta e attestazioni di Active Directory Domain Services in AD FS.
author: billmath
ms.author: billmath
manager: femila
ms.date: 09/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 270fb6efd63e6355c410ee45d09e6fd16b14222b
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59867992"
---
# <a name="compound-authentication-and-ad-ds-claims-in-ad-fs"></a>Autenticazione composta e attestazioni di Active Directory Domain Services in AD FS
Windows Server 2012 migliora l'autenticazione Kerberos con l'introduzione di autenticazione composta.  L'autenticazione composta consente a una richiesta Kerberos Ticket-Granting Service (TGS) da includere due identità: 

- l'identità dell'utente
- l'identità del dispositivo dell'utente.  

Windows esegue l'autenticazione composta estendendo [Kerberos FAST Flexible Authentication Secure Tunneling () o la blindatura Kerberos](https://technet.microsoft.com/library/hh831747.aspx). 

AD FS 2012 e versioni successive consente l'utilizzo di Active Directory rilasciate le attestazioni utente o dispositivo che si trovano in un ticket di autenticazione Kerberos. Nelle versioni precedenti di ADFS, le attestazioni motore è stato possibile leggere solo utente e ID (SID) di sicurezza del gruppo da Kerberos ma non è in grado di leggere qualsiasi attestazioni le informazioni contenute all'interno di un ticket Kerberos.

È possibile abilitare controllo di accesso più completo per applicazioni federate con Active Directory Domain Services (AD DS)-rilasciate le attestazioni utente e dispositivo, con Active Directory Federation Services (ADFS).

## <a name="requirements"></a>Requisiti
1.  Il computer che accedono alle applicazioni federate, è necessario eseguire l'autenticazione all'uso di AD FS **l'autenticazione integrata di Windows**. 
    - L'autenticazione integrata di Windows è disponibile solo quando ci si connette ai server back-end AD FS.
    - I computer devono essere in grado di raggiungere il server back-end AD FS per nome servizio federativo
    - I server AD FS deve offrire l'autenticazione integrata di Windows come metodo di autenticazione primario nelle relative impostazioni di Intranet.

2.  Il criterio **supporto client Kerberos per attestazioni di autenticazione composta e blindatura Kerberos** deve essere applicato a tutti i computer l'accesso alle applicazioni federate che sono protetti mediante l'autenticazione composta. Questo vale in caso di foresta singola o tra insiemi di strutture differenti.

3.  Il dominio che ospita il server ADFS deve avere il **supporto KDC per attestazioni autenticazione composta e blindatura Kerberos** impostazione dei criteri applicato ai controller di dominio.

## <a name="steps-for-configuring-ad-fs-in-windows-server-2012-r2"></a>Passaggi per la configurazione di ADFS in Windows Server 2012 R2
Usare la procedura seguente per la configurazione di attestazioni e autenticazione composta 

### <a name="step-1--enable-kdc-support-for-claims-compound-authentication-and-kerberos-armoring-on-the-default-domain-controller-policy"></a>Passaggio 1:  Abilitare il supporto KDC per attestazioni, autenticazione composta e blindatura Kerberos su criterio Controller di dominio predefinito
1.  In Server Manager, selezionare Strumenti di **Gestione criteri di gruppo**.
2.  Spostarsi verso il basso per il **criterio Controller di dominio predefinito**, fare doppio clic e selezionare **modificare**.
![Gestione criteri di gruppo](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc1.png)
3.  Nel **Editor Gestione criteri di gruppo**, in **configurazione del Computer**, espandere **criteri**, espandere **modelli amministrativi** , espandere **System**e selezionare **KDC**.
4.  Nel riquadro di destra, fare doppio clic su **supporto KDC per attestazioni, autenticazione composta e blindatura Kerberos**.
![Gestione criteri di gruppo](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc2.png)
5.  Nella finestra di dialogo del nuovo supporto KDC di attestazioni per impostare **abilitato**.
6.  Nelle opzioni, selezionare **Supported** dal menu a discesa e quindi fare clic su **Apply** e **OK**.
![Gestione criteri di gruppo](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc3.png)

### <a name="step-2-enable-kerberos-client-support-for-claims-compound-authentication-and-kerberos-armoring-on-computers-accessing-federated-applications"></a>Passaggio 2: Abilitare il supporto client Kerberos per attestazioni, autenticazione composta e blindatura Kerberos nel computer che accedono alle applicazioni federate

1.  In un criterio di gruppo applicati ai computer di accedere alle applicazioni federate, nelle **Editor Gestione criteri di gruppo**, in **configurazione del Computer**, espandere **criteri**, espandere **modelli amministrativi**, espandere **sistema**, selezionare **Kerberos**.
2.  Nel riquadro destro della finestra Editor Gestione criteri di gruppo, fare doppio clic su **supporto client Kerberos per attestazioni, autenticazione composta e blindatura Kerberos.**
3.  Nella finestra di dialogo Nuovo, impostare supporto client Kerberos **Enabled** e fare clic su **Apply** e **OK**.
![Gestione criteri di gruppo](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc4.png)
4.  Chiudi l'Editor Gestione Criteri di gruppo.

### <a name="step-3-ensure-the-ad-fs-servers-have-been-updated"></a>Passaggio 3: Verificare che i server AD FS sono stati aggiornati.
È necessario assicurarsi che i seguenti aggiornamenti siano installati nel server AD FS.

|Aggiornamenti|Descrizione|
|----- | ----- |
|[KB2919355](https://www.microsoft.com/download/details.aspx?id=42335)|Aggiornamento cumulativo della sicurezza (che include KB2919355, KB2932046, KB2934018, KB2937592, KB2938439)|
|[KB2959977](https://www.microsoft.com/download/details.aspx?id=42530)|Aggiornamento per Server 2012 R2|
|[Hotfix 3052122](https://support.microsoft.com/help/3052122/update-adds-support-for-compound-id-claims-in-ad-fs-tokens-in-windows)|Questo aggiornamento aggiunge il supporto per composta ID attestazioni in Active Directory Federation Services.|

### <a name="step-4-configure-the-primary-authentication-provider"></a>Passaggio 4: Configurare il Provider di autenticazione principale

1. Impostare il Provider di autenticazione primaria **Windows autenticazione** per le impostazioni di AD FS Intranet.
2. In Gestione AD FS, sotto **i criteri di autenticazione**, selezionare **autenticazione primaria** e in **impostazioni globali** fare clic su **edit**.
3. Sul **Modifica criteri di autenticazione globali** sotto **Intranet** seleziona **l'autenticazione di Windows**.
4. Fare clic su **applicano** e **Ok**.

![Gestione Criteri di gruppo](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc5.png)

5. Uso di PowerShell è possibile usare la **Set-AdfsGlobalAuthenticationPolicy** cmdlet.

``` powershell
Set-AdfsGlobalAuthenticationPolicy -PrimaryIntranetAuthenticationProvider 'WindowsAuthentication'
```
>[!NOTE]
>In un database interno di Windows basata farm, il comando di PowerShell comando deve essere eseguito nel Server AD FS primario.
>In una farm SQL basata, il comando di PowerShell può essere eseguito su qualsiasi server AD FS che è un membro della farm.

### <a name="step-5--add-the-claim-description-to-ad-fs"></a>Passaggio 5:  Aggiungere la descrizione di attestazione ad AD FS
1.  Aggiungere la descrizione di attestazione seguenti nella farm. Questa descrizione di attestazione non è presente per impostazione predefinita in ad FS 2012 R2 e deve essere aggiunta manualmente.
2.  In Gestione AD FS, sotto **assistenza**, fare doppio clic su **descrizione attestazione** e selezionare **Aggiungi descrizione attestazione**
3.  Immettere le informazioni seguenti nella descrizione dell'attestazione
    - Nome visualizzato: 'Il gruppo di dispositivi di Windows' 
    - Descrizione di attestazione: 'https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup' '
4. Inserire un segno di spunta in entrambe le caselle.
5. Fare clic su **OK**.

![Descrizione di attestazione](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc6.png)

6. Uso di PowerShell è possibile usare la **Add-AdfsClaimDescription** cmdlet.
``` powershell
Add-AdfsClaimDescription -Name 'Windows device group' -ClaimType 'https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup' `
-ShortName 'windowsdevicegroup' -IsAccepted $true -IsOffered $true -IsRequired $false -Notes 'The windows group SID of the device' 
```


>[!NOTE]
>In un database interno di Windows basata farm, il comando di PowerShell comando deve essere eseguito nel Server AD FS primario.
>In una farm SQL basata, il comando di PowerShell può essere eseguito su qualsiasi server AD FS che è un membro della farm.

### <a name="step-6--enable-the-compound-authentication-bit-on-the-msds-supportedencryptiontypes-attribute"></a>Passaggio 6:  Abilitare l'autenticazione composta bit dell'attributo msDS-SupportedEncryptionTypes

1.  Abilitare l'autenticazione composta di bit dell'attributo msDS-SupportedEncryptionTypes dell'account è stato designato per l'esecuzione del servizio ADFS utilizzando il **Set-ADServiceAccount** cmdlet di PowerShell.  

>[!NOTE]
>Se si modifica l'account del servizio, quindi è necessario abilitare l'autenticazione composta manualmente eseguendo il **Set-ADUser - compoundIdentitySupported: $true** i cmdlet di Windows PowerShell.

``` powershell
Set-ADServiceAccount -Identity “ADFS Service Account” -CompoundIdentitySupported:$true 
```
2.  Riavviare il servizio ad FS.

>[!NOTE]
>Una volta 'CompoundIdentitySupported' è impostata su true, installazione dei gMSA stesso nel nuovo server (2012 R2 o 2016) ha esito negativo con l'errore seguente: **Install-ADServiceAccount: Non è possibile installare l'account del servizio. Messaggio di errore: 'Il contesto fornito non corrisponde la destinazione'.** .
>
>**Soluzione**: Impostare temporaneamente CompoundIdentitySupported su $false false per. Questo passaggio fa sì che ad FS interrompere l'emissione di attestazioni WindowsDeviceGroup. Set-ADServiceAccount-identità "Account del servizio ad FS" - CompoundIdentitySupported: $false installare gMSA nel nuovo Server e quindi abilitare CompoundIdentitySupported al $True.
Disabilitazione CompoundIdentitySupported e quindi riabilitare riavvio del servizio ad FS non è necessario.

### <a name="step-7-update-the-ad-fs-claims-provider-trust-for-active-directory"></a>Passaggio 7: Trust del Provider di attestazioni AD FS di aggiornamento per Active Directory

1. Aggiornamento di AD FS attendibilità Provider di attestazioni per Active Directory includere la seguente regola attestazioni 'Pass-through' per l'attestazione 'WindowsDeviceGroup'.
2.  Nelle **gestione di AD FS**, fare clic su **attendibilità Provider di attestazioni** e nel riquadro di destra, con clic **Active Directory** e selezionare **Edit Claim Rules**.
3.  Nel **Modifica regole attestazione per Active Directory** fare clic su **Add Rule**.
4.  Nel **Nell'Aggiunta guidata regole attestazione di trasformazione** selezionate **Pass Through or Filter an Incoming Claim** e fare clic su **Avanti**.
5.  Aggiungere un nome visualizzato e selezionare **gruppo di dispositivi di Windows** dalle **tipo di attestazione in ingresso** elenco a discesa.
6.  Scegliere **Fine**.  Fare clic su **applicano** e **Ok**. 
![Descrizione di attestazione](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc7.png)

### <a name="step-8-on-the-relying-party-where-the-windowsdevicegroup-claims-are-expected-add-a-similar-pass-through-or-transform-claim-rule"></a>Passaggio 8: Nella Relying Party in cui si prevede le attestazioni 'WindowsDeviceGroup', aggiungere una regola di attestazione 'Pass-through' o 'Transform' simile.
2.  Nella **gestione di AD FS**, fare clic su **Relying Party Trusts** e nel riquadro di destra, con clic l'applicazione relying Party e selezionare **Edit Claim Rules**.
3.  Nel **regole di trasformazione rilascio** fare clic su **Add Rule**.
4.  Nel **Nell'Aggiunta guidata regole attestazione di trasformazione** selezionate **Pass Through or Filter an Incoming Claim** e fare clic su **Avanti**.
5.  Aggiungere un nome visualizzato e selezionare **gruppo di dispositivi di Windows** dalle **tipo di attestazione in ingresso** elenco a discesa.
6.  Scegliere **Fine**.  Fare clic su **applicano** e **Ok**.
![Descrizione di attestazione](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc8.png)


##<a name="steps-for-configuring-ad-fs-in-windows-server-2016"></a>Passaggi per la configurazione di ADFS in Windows Server 2016
Di seguito illustra in dettaglio i passaggi per configurare l'autenticazione composta su AD FS per Windows Server 2016.

### <a name="step-1--enable-kdc-support-for-claims-compound-authentication-and-kerberos-armoring-on-the-default-domain-controller-policy"></a>Passaggio 1:  Abilitare il supporto KDC per attestazioni, autenticazione composta e blindatura Kerberos su criterio Controller di dominio predefinito
1.  In Server Manager, selezionare Strumenti di **Gestione criteri di gruppo**.
2.  Spostarsi verso il basso per il **criterio Controller di dominio predefinito**, fare doppio clic e selezionare **modificare**.
3.  Nel **Editor Gestione criteri di gruppo**, in **configurazione del Computer**, espandere **criteri**, espandere **modelli amministrativi** , espandere **System**e selezionare **KDC**.
4.  Nel riquadro di destra, fare doppio clic su **supporto KDC per attestazioni, autenticazione composta e blindatura Kerberos**.
5.  Nella finestra di dialogo del nuovo supporto KDC di attestazioni per impostare **abilitato**.
6.  Nelle opzioni, selezionare **Supported** dal menu a discesa e quindi fare clic su **Apply** e **OK**.


### <a name="step-2-enable-kerberos-client-support-for-claims-compound-authentication-and-kerberos-armoring-on-computers-accessing-federated-applications"></a>Passaggio 2: Abilitare il supporto client Kerberos per attestazioni, autenticazione composta e blindatura Kerberos nel computer che accedono alle applicazioni federate

1.  In un criterio di gruppo applicati ai computer di accedere alle applicazioni federate, nelle **Editor Gestione criteri di gruppo**, in **configurazione del Computer**, espandere **criteri**, espandere **modelli amministrativi**, espandere **sistema**, selezionare **Kerberos**.
2.  Nel riquadro destro della finestra Editor Gestione criteri di gruppo, fare doppio clic su **supporto client Kerberos per attestazioni, autenticazione composta e blindatura Kerberos.**
3.  Nella finestra di dialogo Nuovo, impostare supporto client Kerberos **Enabled** e fare clic su **Apply** e **OK**.
4.  Chiudi l'Editor Gestione Criteri di gruppo.

### <a name="step-3-configure-the-primary-authentication-provider"></a>Passaggio 3: Configurare il Provider di autenticazione principale

1. Impostare il Provider di autenticazione primaria **Windows autenticazione** per le impostazioni di AD FS Intranet.
2. In Gestione AD FS, sotto **i criteri di autenticazione**, selezionare **autenticazione primaria** e in **impostazioni globali** fare clic su **edit**.
3. Sul **Modifica criteri di autenticazione globali** sotto **Intranet** seleziona **l'autenticazione di Windows**.
4. Fare clic su **applicano** e **Ok**.
5. Uso di PowerShell è possibile usare la **Set-AdfsGlobalAuthenticationPolicy** cmdlet.

``` powershell
Set-AdfsGlobalAuthenticationPolicy -PrimaryIntranetAuthenticationProvider 'WindowsAuthentication'
```
>[!NOTE]
>In un database interno di Windows basata farm, il comando di PowerShell comando deve essere eseguito nel Server AD FS primario.
>In una farm SQL basata, il comando di PowerShell può essere eseguito su qualsiasi server AD FS che è un membro della farm.

### <a name="step-4--enable-the-compound-authentication-bit-on-the-msds-supportedencryptiontypes-attribute"></a>Passaggio 4:  Abilitare l'autenticazione composta bit dell'attributo msDS-SupportedEncryptionTypes

1.  Abilitare l'autenticazione composta di bit dell'attributo msDS-SupportedEncryptionTypes dell'account è stato designato per l'esecuzione del servizio ADFS utilizzando il **Set-ADServiceAccount** cmdlet di PowerShell.  

>[!NOTE]
>Se si modifica l'account del servizio, quindi è necessario abilitare l'autenticazione composta manualmente eseguendo il **Set-ADUser - compoundIdentitySupported: $true** i cmdlet di Windows PowerShell.

``` powershell
Set-ADServiceAccount -Identity “ADFS Service Account” -CompoundIdentitySupported:$true 
```
2.  Riavviare il servizio ad FS.

>[!NOTE]
>Una volta 'CompoundIdentitySupported' è impostata su true, installazione dei gMSA stesso nel nuovo server (2012 R2 o 2016) ha esito negativo con l'errore seguente: **Install-ADServiceAccount: Non è possibile installare l'account del servizio. Messaggio di errore: 'Il contesto fornito non corrisponde la destinazione'.** .
>
>**Soluzione**: Impostare temporaneamente CompoundIdentitySupported su $false false per. Questo passaggio fa sì che ad FS interrompere l'emissione di attestazioni WindowsDeviceGroup. Set-ADServiceAccount-identità "Account del servizio ad FS" - CompoundIdentitySupported: $false installare gMSA nel nuovo Server e quindi abilitare CompoundIdentitySupported al $True.
Disabilitazione CompoundIdentitySupported e quindi riabilitare riavvio del servizio ad FS non è necessario.

### <a name="step-5-update-the-ad-fs-claims-provider-trust-for-active-directory"></a>Passaggio 5: Trust del Provider di attestazioni AD FS di aggiornamento per Active Directory

1. Aggiornamento di AD FS attendibilità Provider di attestazioni per Active Directory includere la seguente regola attestazioni 'Pass-through' per l'attestazione 'WindowsDeviceGroup'.
2.  Nelle **gestione di AD FS**, fare clic su **attendibilità Provider di attestazioni** e nel riquadro di destra, con clic **Active Directory** e selezionare **Edit Claim Rules**.
3.  Nel **Modifica regole attestazione per Active Directory** fare clic su **Add Rule**.
4.  Nel **Nell'Aggiunta guidata regole attestazione di trasformazione** selezionate **Pass Through or Filter an Incoming Claim** e fare clic su **Avanti**.
5.  Aggiungere un nome visualizzato e selezionare **gruppo di dispositivi di Windows** dalle **tipo di attestazione in ingresso** elenco a discesa.
6.  Scegliere **Fine**.  Fare clic su **applicano** e **Ok**. 


### <a name="step-6-on-the-relying-party-where-the-windowsdevicegroup-claims-are-expected-add-a-similar-pass-through-or-transform-claim-rule"></a>Passaggio 6: Nella Relying Party in cui si prevede le attestazioni 'WindowsDeviceGroup', aggiungere una regola di attestazione 'Pass-through' o 'Transform' simile.
2.  Nella **gestione di AD FS**, fare clic su **Relying Party Trusts** e nel riquadro di destra, con clic l'applicazione relying Party e selezionare **Edit Claim Rules**.
3.  Nel **regole di trasformazione rilascio** fare clic su **Add Rule**.
4.  Nel **Nell'Aggiunta guidata regole attestazione di trasformazione** selezionate **Pass Through or Filter an Incoming Claim** e fare clic su **Avanti**.
5.  Aggiungere un nome visualizzato e selezionare **gruppo di dispositivi di Windows** dalle **tipo di attestazione in ingresso** elenco a discesa.
6.  Scegliere **Fine**.  Fare clic su **applicano** e **Ok**.

## <a name="validation"></a>Convalida
Per convalidare il rilascio di attestazioni 'WindowsDeviceGroup', creare un test di attestazioni con riconoscimento dell'applicazione con .net 4.6. Con Windows Identity Foundation SDK 4.0.
Configurare l'applicazione come Relying Party in ad FS e lo aggiorna con attestazione regola come specificato nei passaggi precedenti.
Quando si esegue l'autenticazione all'applicazione tramite provider di autenticazione integrata di Windows di ad FS, le attestazioni seguenti sono sottoposto a cast.
![Validation](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc9.png)

Le attestazioni per il computer/dispositivo possono essere ora utilizzate per i controlli di accesso più avanzati.

Seguente, ad esempio – **AdditionalAuthenticationRules** indica a AD FS per richiamare l'autenticazione a più fattori se il parametro-l'autenticazione utente non è membro del gruppo di sicurezza "-1-5-21-2134745077-1211275016-3050530490-1117" e il Computer (dove è il utente è l'autenticazione da) non è membro del gruppo di sicurezza "S-1-5-21-2134745077-1211275016-3050530490-1115 (WindowsDeviceGroup)"

Se viene soddisfatta una delle condizioni precedenti, tuttavia, non richiamare l'autenticazione a più fattori.

```
'NOT EXISTS([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup", Value =~ "S-1-5-21-2134745077-1211275016-3050530490-1115"])
&& NOT EXISTS([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "S-1-5-21-2134745077-1211275016-3050530490-1117"])
=> issue(Type = "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", Value = "https://schemas.microsoft.com/claims/multipleauthn");'
```
