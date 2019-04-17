---
title: Composta attestazioni di servizi di dominio Active Directory e l'autenticazione in Active Directory Federation Services
description: In questo documento illustra l'autenticazione composta e attestazioni di dominio Active Directory in ADFS.
author: billmath
ms.author: billmath
manager: femila
ms.date: 09/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: f5f80cbcbdb2deca9b098014627365b8ea819b5f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="compound-authentication-and-ad-ds-claims-in-ad-fs"></a>Autenticazione composta e attestazioni di dominio Active Directory in ADFS
Windows Server 2012 migliora l'autenticazione Kerberos grazie all'introduzione di autenticazione composta.  Autenticazione composta consente una richiesta di servizio Kerberos di concessione Ticket (TGS) includere le due identità: 

- l'identità dell'utente
- l'identità del dispositivo dell'utente.  

È possibile effettuare l'autenticazione composta estendendo [Kerberos FAST Flexible Authentication Secure Tunneling () o la blindatura Kerberos](https://technet.microsoft.com/library/hh831747.aspx). 

AD FS 2012 e versioni successive consente l'utilizzo da parte di Active Directory rilasciate le attestazioni utente o dispositivo che si trovano in un ticket di autenticazione Kerberos. Nelle versioni precedenti di ADFS, le attestazioni motore solo leggere utente e gruppo ID di protezione (SID) da Kerberos ma non è in grado di leggere le informazioni contenute all'interno di un ticket Kerberos di attestazioni.

È possibile abilitare il controllo di accesso più completo per applicazioni federate tramite servizi di dominio Active Directory (AD DS)-rilasciate le attestazioni utente e dispositivo, insieme con Active Directory Federation Services (ADFS).

## <a name="requirements"></a>Requisiti
1.  I computer che accedono alle applicazioni federate, deve autenticarsi presso con AD FS **autenticazione integrata di Windows**. 
    - Autenticazione integrata di Windows è disponibile solo per la connessione al server back-end di ADFS.
    - I computer devono essere in grado di raggiungere il server ADFS back-end per nome servizio federativo
    - Server ADFS deve offrire l'autenticazione integrata di Windows come metodo di autenticazione principale per le impostazioni Intranet.

2.  I criteri **supporto client Kerberos per attestazioni autenticazione composta e blindatura Kerberos** deve essere applicato a tutti i computer che accedono alle applicazioni federate che sono protetti da autenticazione composta. Questa opzione è disponibile in caso di singola foresta o tra insiemi di strutture differenti.

3.  Il dominio che ospita il server ADFS deve disporre di **supporto KDC per attestazioni autenticazione composta e blindatura Kerberos** impostazione dei criteri applicato ai controller di dominio.

## <a name="steps-for-configuring-ad-fs-in-windows-server-2012-r2"></a>Passaggi per la configurazione di ADFS in Windows Server 2012 R2
Attenersi alla seguente procedura per la configurazione di attestazioni e autenticazione composta 

### <a name="step-1--enable-kdc-support-for-claims-compound-authentication-and-kerberos-armoring-on-the-default-domain-controller-policy"></a>Passaggio 1: Abilitare il supporto KDC per attestazioni, autenticazione composta e blindatura Kerberos nel criterio Controller di dominio predefinito
1.  In Server Manager, selezionare strumenti, **Gestione criteri di gruppo**.
2.  Spostarsi verso il basso il **criterio Controller di dominio predefinito**del mouse e scegliere **modifica**.
![Gestione criteri di gruppo](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc1.png)
3.  Nel **Editor Gestione criteri di gruppo**, in **configurazione Computer**, espandere **criteri**, espandere **modelli amministrativi**, espandere **sistema**e seleziona **KDC**.
4.  Nel riquadro di destra fare doppio clic su **supporto KDC per attestazioni, autenticazione composta e blindatura Kerberos**.
![Gestione criteri di gruppo](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc2.png)
5.  Nella finestra di dialogo Nuovo supporto KDC di attestazioni per impostare **abilitato**.
6.  In Opzioni selezionare **supportati** dal menu a discesa e quindi fare clic su **applica** e **OK**.
![Gestione criteri di gruppo](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc3.png)

### <a name="step-2-enable-kerberos-client-support-for-claims-compound-authentication-and-kerberos-armoring-on-computers-accessing-federated-applications"></a>Passaggio 2: Abilitare il supporto client Kerberos per attestazioni, autenticazione composta e blindatura Kerberos in computer che accedono alle applicazioni federate

1.  In un criterio di gruppo applicati ai computer l'accesso alle applicazioni federate, nel **Editor Gestione criteri di gruppo**, in **configurazione Computer**, espandere **criteri**, espandere **amministrazione Modelli**, espandere **sistema**e seleziona **Kerberos**.
2.  Nel riquadro destro della finestra Editor Gestione criteri di gruppo, fare doppio clic su **supporto client Kerberos per attestazioni, autenticazione composta e blindatura Kerberos.**
3.  Nella finestra di dialogo Nuovo, impostare il supporto client Kerberos **abilitato** e fare clic su **applica** e **OK**.
![Gestione criteri di gruppo](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc4.png)
4.  Chiudere Editor Gestione criteri di gruppo.

### <a name="step-3-ensure-the-ad-fs-servers-have-been-updated"></a>Passaggio 3: Verificare che i server ADFS sono stati aggiornati.
È necessario assicurarsi che i seguenti aggiornamenti siano installati nel server ADFS.

|Aggiornamento|Descrizione|
|----- | ----- |
|[(KB2919355)](https://www.microsoft.com/download/details.aspx?id=42335)|Aggiornamento cumulativo (include KB2919355, KB2932046, KB2934018, KB2937592, KB2938439)|
|[KB2959977](https://www.microsoft.com/download/details.aspx?id=42530)|Aggiornamento per Server 2012 R2|
|[Hotfix 3052122](https://support.microsoft.com/help/3052122/update-adds-support-for-compound-id-claims-in-ad-fs-tokens-in-windows)|Questo aggiornamento aggiunge il supporto per composta ID attestazioni in Active Directory Federation Services.|

### <a name="step-4-configure-the-primary-authentication-provider"></a>Passaggio 4: Configurare il Provider di autenticazione principale

1. Impostare il Provider di autenticazione primaria **l'autenticazione di Windows** per le impostazioni Intranet di AD FS.
2. In Gestione di ADFS, in **criteri di autenticazione**selezionare **autenticazione primaria** e in **impostazioni globali** fare clic su **modifica**.
3. In **Modifica criteri di autenticazione globali** in **Intranet** selezionare **l'autenticazione di Windows**.
4. Fare clic su **applicare** e **Ok**.

![Gestione criteri di gruppo](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc5.png)

5. Utilizzo di PowerShell è possibile utilizzare il **Set-AdfsGlobalAuthenticationPolicy** cmdlet.

``` powershell
Set-AdfsGlobalAuthenticationPolicy -PrimaryIntranetAuthenticationProvider 'WindowsAuthentication'
```
>[!NOTE]
>In un database interno di Windows in base farm, il comando deve essere eseguito il Server ADFS primario di PowerShell.
>In una farm basato su SQL, il comando di PowerShell può essere eseguito in qualsiasi server ADFS che è un membro della farm.

### <a name="step-5--add-the-claim-description-to-ad-fs"></a>Passaggio 5: Aggiungere la descrizione di attestazione ad ADFS
1.  Aggiungere la seguente descrizione di attestazione alla farm. Questa descrizione di attestazione non è presente per impostazione predefinita in ADFS 2012 R2 ed è necessario aggiungere manualmente.
2.  In Gestione di ADFS, in **servizio**, fare doppio clic su **descrizione di attestazione** e seleziona **Aggiungi descrizione di attestazione**
3.  Immettere le informazioni seguenti nella descrizione dell'attestazione
    - Nome visualizzato: 'Windows dispositivo gruppo' 
    - Descrizione di attestazione: 'https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup' '
4. Inserire un segno di spunta in entrambe le caselle.
5. Fare clic su **OK**.

![Descrizione di attestazione](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc6.png)

6. Utilizzo di PowerShell è possibile utilizzare il **Aggiungi AdfsClaimDescription** cmdlet.
``` powershell
Add-AdfsClaimDescription -Name 'Windows device group' -ClaimType 'https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup' `
-ShortName 'windowsdevicegroup' -IsAccepted $true -IsOffered $true -IsRequired $false -Notes 'The windows group SID of the device' 
```


>[!NOTE]
>In un database interno di Windows in base farm, il comando deve essere eseguito il Server ADFS primario di PowerShell.
>In una farm basato su SQL, il comando di PowerShell può essere eseguito in qualsiasi server ADFS che è un membro della farm.

### <a name="step-6--enable-the-compound-authentication-bit-on-the-msds-supportedencryptiontypes-attribute"></a>Passaggio 6: Abilitare l'autenticazione composta bit sull'attributo msDS-SupportedEncryptionTypes

1.  Abilitare l'autenticazione composta bit per l'account definito per l'esecuzione del servizio ADFS utilizzando l'attributo msDS-SupportedEncryptionTypes il **Set-ADServiceAccount** cmdlet di PowerShell.  

>[!NOTE]
>Se si modifica l'account di servizio, quindi è necessario abilitare l'autenticazione composta manualmente eseguendo il **Set-ADUser - compoundIdentitySupported: $true** cmdlet di Windows PowerShell.

``` powershell
Set-ADServiceAccount -Identity “ADFS Service Account” -CompoundIdentitySupported:$true 
```
2.  Riavviare il servizio ADFS.

>[!NOTE]
>Una volta 'CompoundIdentitySupported' è impostato su true, installazione di gMSA stesso nel nuovo server (2012 R2/2016) si verifica un errore con il seguente errore: **Install-ADServiceAccount: Impossibile installare l'account del servizio. Messaggio di errore: "il contesto fornito non corrisponde la destinazione.'**.
>
>**Soluzione**: impostare temporaneamente CompoundIdentitySupported su $ $false per. Questo passaggio fa sì che ADFS arrestare rilasciare le attestazioni WindowsDeviceGroup. Set-ADServiceAccount-identità 'Account del servizio ADFS ' - CompoundIdentitySupported: $false installare gMSA nel nuovo Server e quindi abilitare CompoundIdentitySupported al $True.
La disattivazione CompoundIdentitySupported e quindi riabilitare non è necessario riavvio del servizio ADFS.

### <a name="step-7-update-the-ad-fs-claims-provider-trust-for-active-directory"></a>Passaggio 7: Aggiornamento dell'attendibilità Provider di attestazioni AD FS per Active Directory

1. Aggiornamento di AD FS attendibilità Provider di attestazioni di Active Directory includere la seguente regola attestazioni 'Pass-through' per l'attestazione 'WindowsDeviceGroup'.
2.  In **gestione di ADFS**, fare clic su **Provider di attestazioni** e nel riquadro di destra, destra scegliere **Active Directory** e seleziona **Modifica regole attestazione**.
3.  Nel **Modifica regole attestazione per Active Directory** fare clic su **Aggiungi regola**.
4.  Nel **Aggiunta guidata regole attestazione trasformare** selezionare **Pass-Through o filtro di un'attestazione in ingresso** e fare clic su **Avanti**.
5.  Aggiungere un nome visualizzato e selezionare **gruppo di dispositivi Windows** dal **tipo di attestazione in ingresso** elenco a discesa.
6.  Fare clic su **fine **.  Fare clic su **applicare** e **Ok**. 
![Descrizione di attestazione](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc7.png)

### <a name="step-8-on-the-relying-party-where-the-windowsdevicegroup-claims-are-expected-add-a-similar-pass-through-or-transform-claim-rule"></a>Passaggio 8: Nella Relying Party in cui sono previsti le attestazioni 'WindowsDeviceGroup', aggiungere una regola di attestazione 'Pass-through' o 'Trasformare' simile.
2.  In **gestione di ADFS**, fare clic su **attendibilità** e nel riquadro di destra, destra scegliere l'applicazione relying Party e selezionare **Modifica regole attestazione**.
3.  Nel **regole di trasformazione rilascio** fare clic su **Aggiungi regola**.
4.  Nel **Aggiunta guidata regole attestazione trasformare** selezionare **Pass-Through o filtro di un'attestazione in ingresso** e fare clic su **Avanti**.
5.  Aggiungere un nome visualizzato e selezionare **gruppo di dispositivi Windows** dal **tipo di attestazione in ingresso** elenco a discesa.
6.  Fare clic su **fine **.  Fare clic su **applicare** e **Ok**.
![Descrizione di attestazione](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc8.png)


##<a name="steps-for-configuring-ad-fs-in-windows-server-2016"></a>Passaggi per la configurazione di ADFS in Windows Server 2016
Di seguito descrive i passaggi per configurare l'autenticazione composta su ADFS per Windows Server 2016.

### <a name="step-1--enable-kdc-support-for-claims-compound-authentication-and-kerberos-armoring-on-the-default-domain-controller-policy"></a>Passaggio 1: Abilitare il supporto KDC per attestazioni, autenticazione composta e blindatura Kerberos nel criterio Controller di dominio predefinito
1.  In Server Manager, selezionare strumenti, **Gestione criteri di gruppo**.
2.  Spostarsi verso il basso il **criterio Controller di dominio predefinito**del mouse e scegliere **modifica**.
3.  Nel **Editor Gestione criteri di gruppo**, in **configurazione Computer**, espandere **criteri**, espandere **modelli amministrativi**, espandere **sistema**e seleziona **KDC**.
4.  Nel riquadro di destra fare doppio clic su **supporto KDC per attestazioni, autenticazione composta e blindatura Kerberos**.
5.  Nella finestra di dialogo Nuovo supporto KDC di attestazioni per impostare **abilitato**.
6.  In Opzioni selezionare **supportati** dal menu a discesa e quindi fare clic su **applica** e **OK**.


### <a name="step-2-enable-kerberos-client-support-for-claims-compound-authentication-and-kerberos-armoring-on-computers-accessing-federated-applications"></a>Passaggio 2: Abilitare il supporto client Kerberos per attestazioni, autenticazione composta e blindatura Kerberos in computer che accedono alle applicazioni federate

1.  In un criterio di gruppo applicati ai computer l'accesso alle applicazioni federate, nel **Editor Gestione criteri di gruppo**, in **configurazione Computer**, espandere **criteri**, espandere **amministrazione Modelli**, espandere **sistema**e seleziona **Kerberos**.
2.  Nel riquadro destro della finestra Editor Gestione criteri di gruppo, fare doppio clic su **supporto client Kerberos per attestazioni, autenticazione composta e blindatura Kerberos.**
3.  Nella finestra di dialogo Nuovo, impostare il supporto client Kerberos **abilitato** e fare clic su **applica** e **OK**.
4.  Chiudere Editor Gestione criteri di gruppo.

### <a name="step-3-configure-the-primary-authentication-provider"></a>Passaggio 3: Configurare il Provider di autenticazione principale

1. Impostare il Provider di autenticazione primaria **l'autenticazione di Windows** per le impostazioni Intranet di AD FS.
2. In Gestione di ADFS, in **criteri di autenticazione**selezionare **autenticazione primaria** e in **impostazioni globali** fare clic su **modifica**.
3. In **Modifica criteri di autenticazione globali** in **Intranet** selezionare **l'autenticazione di Windows**.
4. Fare clic su **applicare** e **Ok**.
5. Utilizzo di PowerShell è possibile utilizzare il **Set-AdfsGlobalAuthenticationPolicy** cmdlet.

``` powershell
Set-AdfsGlobalAuthenticationPolicy -PrimaryIntranetAuthenticationProvider 'WindowsAuthentication'
```
>[!NOTE]
>In un database interno di Windows in base farm, il comando deve essere eseguito il Server ADFS primario di PowerShell.
>In una farm basato su SQL, il comando di PowerShell può essere eseguito in qualsiasi server ADFS che è un membro della farm.

### <a name="step-4--enable-the-compound-authentication-bit-on-the-msds-supportedencryptiontypes-attribute"></a>Passaggio 4: Abilitare l'autenticazione composta bit sull'attributo msDS-SupportedEncryptionTypes

1.  Abilitare l'autenticazione composta bit per l'account definito per l'esecuzione del servizio ADFS utilizzando l'attributo msDS-SupportedEncryptionTypes il **Set-ADServiceAccount** cmdlet di PowerShell.  

>[!NOTE]
>Se si modifica l'account di servizio, quindi è necessario abilitare l'autenticazione composta manualmente eseguendo il **Set-ADUser - compoundIdentitySupported: $true** cmdlet di Windows PowerShell.

``` powershell
Set-ADServiceAccount -Identity “ADFS Service Account” -CompoundIdentitySupported:$true 
```
2.  Riavviare il servizio ADFS.

>[!NOTE]
>Una volta 'CompoundIdentitySupported' è impostato su true, installazione di gMSA stesso nel nuovo server (2012 R2/2016) si verifica un errore con il seguente errore: **Install-ADServiceAccount: Impossibile installare l'account del servizio. Messaggio di errore: "il contesto fornito non corrisponde la destinazione.'**.
>
>**Soluzione**: impostare temporaneamente CompoundIdentitySupported su $ $false per. Questo passaggio fa sì che ADFS arrestare rilasciare le attestazioni WindowsDeviceGroup. Set-ADServiceAccount-identità 'Account del servizio ADFS ' - CompoundIdentitySupported: $false installare gMSA nel nuovo Server e quindi abilitare CompoundIdentitySupported al $True.
La disattivazione CompoundIdentitySupported e quindi riabilitare non è necessario riavvio del servizio ADFS.

### <a name="step-5-update-the-ad-fs-claims-provider-trust-for-active-directory"></a>Passaggio 5: Aggiornamento dell'attendibilità Provider di attestazioni AD FS per Active Directory

1. Aggiornamento di AD FS attendibilità Provider di attestazioni di Active Directory includere la seguente regola attestazioni 'Pass-through' per l'attestazione 'WindowsDeviceGroup'.
2.  In **gestione di ADFS**, fare clic su **Provider di attestazioni** e nel riquadro di destra, destra scegliere **Active Directory** e seleziona **Modifica regole attestazione**.
3.  Nel **Modifica regole attestazione per Active Directory** fare clic su **Aggiungi regola**.
4.  Nel **Aggiunta guidata regole attestazione trasformare** selezionare **Pass-Through o filtro di un'attestazione in ingresso** e fare clic su **Avanti**.
5.  Aggiungere un nome visualizzato e selezionare **gruppo di dispositivi Windows** dal **tipo di attestazione in ingresso** elenco a discesa.
6.  Fare clic su **fine **.  Fare clic su **applicare** e **Ok**. 


### <a name="step-6-on-the-relying-party-where-the-windowsdevicegroup-claims-are-expected-add-a-similar-pass-through-or-transform-claim-rule"></a>Passaggio 6: Nella Relying Party in cui sono previsti le attestazioni 'WindowsDeviceGroup', aggiungere una regola di attestazione 'Pass-through' o 'Trasformare' simile.
2.  In **gestione di ADFS**, fare clic su **attendibilità** e nel riquadro di destra, destra scegliere l'applicazione relying Party e selezionare **Modifica regole attestazione**.
3.  Nel **regole di trasformazione rilascio** fare clic su **Aggiungi regola**.
4.  Nel **Aggiunta guidata regole attestazione trasformare** selezionare **Pass-Through o filtro di un'attestazione in ingresso** e fare clic su **Avanti**.
5.  Aggiungere un nome visualizzato e selezionare **gruppo di dispositivi Windows** dal **tipo di attestazione in ingresso** elenco a discesa.
6.  Fare clic su **fine **.  Fare clic su **applicare** e **Ok**.

## <a name="validation"></a>Convalida
Per convalidare il rilascio delle attestazioni 'WindowsDeviceGroup', creare un test di attestazioni applicazione abilitata per l'uso di .net 4.6. Con WIF SDK 4.0.
Configurare l'applicazione come Relying Party in ADFS e modificalo con attestazione regola come specificato nei passaggi precedenti.
Quando l'autenticazione per l'applicazione tramite il provider di autenticazione integrata di Windows di ADFS, le attestazioni seguenti sono cast.
![Convalida](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc9.png)

Le attestazioni per il computer o dispositivo possono ora essere utilizzate per i controlli di accesso più completo.

Ad esempio, le operazioni seguenti **AdditionalAuthenticationRules** indica AD FS per richiamare l'autenticazione a più fattori se – l'autenticazione utente non è membro del gruppo di sicurezza "-1-5-21-2134745077-1211275016-3050530490-1117" e il Computer (dove è l'utente è L'autenticazione da) non è membro del gruppo di sicurezza "S-1-5-21-2134745077-1211275016-3050530490-1115 (WindowsDeviceGroup)"

Se viene soddisfatta una delle condizioni precedenti, tuttavia, non richiamare l'autenticazione a più fattori.

```
'NOT EXISTS([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup", Value =~ "S-1-5-21-2134745077-1211275016-3050530490-1115"])
&& NOT EXISTS([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "S-1-5-21-2134745077-1211275016-3050530490-1117"])
=> issue(Type = "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", Value = "https://schemas.microsoft.com/claims/multipleauthn");'
```