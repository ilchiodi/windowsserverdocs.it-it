---
title: Attestazioni di autenticazione e Active Directory Domain Services composte in Active Directory Federation Services
description: Il documento seguente illustra l'autenticazione composta e le attestazioni di servizi di dominio Active Directory in AD FS.
author: billmath
ms.author: billmath
manager: femila
ms.date: 09/07/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 78db6f8b6961cecea55b8d371e9abf952cafdab3
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71358671"
---
# <a name="compound-authentication-and-ad-ds-claims-in-ad-fs"></a>Autenticazione composta e attestazioni di Active Directory Domain Services in AD FS
Windows Server 2012 migliora l'autenticazione Kerberos introducendo l'autenticazione composta.  L'autenticazione composta consente a una richiesta del servizio di concessione ticket (TGS) Kerberos di includere due identità: 

- identità dell'utente
- identità del dispositivo dell'utente.  

Windows consente di eseguire l'autenticazione composta mediante l'estensione della [blindatura Kerberos (Fast) Kerberos, o della blindatura Kerberos](https://technet.microsoft.com/library/hh831747.aspx). 

AD FS 2012 e versioni successive consente l'utilizzo di attestazioni di utenti o dispositivi rilasciate da servizi di dominio Active Directory che risiedono in un ticket di autenticazione Kerberos. Nelle versioni precedenti di AD FS, il motore delle attestazioni poteva leggere solo gli ID di sicurezza (SID) di utenti e gruppi da Kerberos, ma non è stato in grado di leggere le informazioni sulle attestazioni contenute in un ticket Kerberos.

È possibile abilitare il controllo degli accessi più completo per le applicazioni federate utilizzando le attestazioni di utenti e dispositivi rilasciate Active Directory Domain Services (AD DS) insieme, con Active Directory Federation Services (AD FS).

## <a name="requirements"></a>Requisiti
1.  I computer che accedono alle applicazioni federate devono eseguire l'autenticazione a AD FS usando **l'autenticazione integrata di Windows**. 
    - Autenticazione integrata di Windows è disponibile solo quando ci si connette ai server AD FS back-end.
    - I computer devono essere in grado di raggiungere i server di AD FS back-end per Servizio federativo nome
    - I server AD FS devono offrire l'autenticazione integrata di Windows come metodo di autenticazione principale nelle impostazioni Intranet.

2.  Il criterio **supporto client Kerberos per l'autenticazione composta di attestazioni e la blindatura Kerberos** deve essere applicato a tutti i computer che accedono alle applicazioni federate protette da autenticazione composta. Questa operazione è applicabile in caso di scenari a foresta singola o tra foreste.

3.  Il dominio che ospita i server AD FS deve avere l'impostazione del criterio **Supporto KDC per autenticazione composta attestazioni e blindatura Kerberos** applicata ai controller di dominio.

## <a name="steps-for-configuring-ad-fs-in-windows-server-2012-r2"></a>Passaggi per la configurazione di AD FS in Windows Server 2012 R2
Usare la procedura seguente per la configurazione di attestazioni e autenticazione composta 

### <a name="step-1--enable-kdc-support-for-claims-compound-authentication-and-kerberos-armoring-on-the-default-domain-controller-policy"></a>Passaggio 1:  Abilitare il supporto KDC per attestazioni, autenticazione composta e blindatura Kerberos nei criteri del controller di dominio predefinito
1.  In Server Manager selezionare strumenti, **gestione criteri di gruppo**.
2.  Passare al **criterio controller di dominio predefinito**, fare clic con il pulsante destro del mouse e scegliere **modifica**.
![Gestione Criteri di gruppo](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc1.png)
3.  Nel **Editor gestione criteri di gruppo**, in **Configurazione computer**espandere **criteri**, espandere **modelli amministrativi**, espandere **sistema**e selezionare **KDC**.
4.  Nel riquadro di destra fare doppio clic su **Supporto KDC per attestazioni, autenticazione composta e blindatura Kerberos**.
![Gestione Criteri di gruppo](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc2.png)
5.  Nella finestra di dialogo nuovo impostare Supporto KDC per attestazioni su **abilitato**.
6.  In Opzioni scegliere **supportato** dal menu a discesa e quindi fare clic su **applica** e **OK**.
![Gestione Criteri di gruppo](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc3.png)

### <a name="step-2-enable-kerberos-client-support-for-claims-compound-authentication-and-kerberos-armoring-on-computers-accessing-federated-applications"></a>Passaggio 2: Abilitare il supporto client Kerberos per attestazioni, autenticazione composta e blindatura Kerberos nei computer che accedono alle applicazioni federate

1.  In una Criteri di gruppo applicata ai computer che accedono alle applicazioni federate, nel **Editor gestione criteri di gruppo**, in **Configurazione computer**espandere **criteri**, espandere **modelli amministrativi**, espandere **sistema** e selezionare **Kerberos**.
2.  Nel riquadro destro della finestra di Editor Gestione Criteri di gruppo fare doppio clic su **supporto client Kerberos per attestazioni, autenticazione composta e blindatura Kerberos.**
3.  Nella finestra di dialogo nuovo impostare supporto client Kerberos su **abilitato** , quindi fare clic su **applica** e **OK**.
![Gestione Criteri di gruppo](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc4.png)
4.  Chiudi l'Editor Gestione Criteri di gruppo.

### <a name="step-3-ensure-the-ad-fs-servers-have-been-updated"></a>Passaggio 3: Verificare che i server di AD FS siano stati aggiornati.
È necessario assicurarsi che i seguenti aggiornamenti siano installati nei server AD FS.

|Aggiornamenti|Descrizione|
|----- | ----- |
|[KB2919355](https://www.microsoft.com/download/details.aspx?id=42335)|Aggiornamento cumulativo della sicurezza (include KB2919355, KB2932046, KB2934018, KB2937592, KB2938439)|
|[KB2959977](https://www.microsoft.com/download/details.aspx?id=42530)|Aggiornamento per Server 2012 R2|
|[Hotfix 3052122](https://support.microsoft.com/help/3052122/update-adds-support-for-compound-id-claims-in-ad-fs-tokens-in-windows)|Questo aggiornamento aggiunge il supporto per le attestazioni con ID composto in Active Directory Federation Services.|

### <a name="step-4-configure-the-primary-authentication-provider"></a>Passaggio 4: Configurare il provider di autenticazione primario

1. Impostare il provider di autenticazione primario per l' **autenticazione di Windows** per ad FS le impostazioni Intranet.
2. In gestione AD FS, in **criteri di autenticazione**selezionare **autenticazione primaria** e in **Impostazioni globali** fare clic su **modifica**.
3. In **modifica criteri di autenticazione globali** in **Intranet** selezionare **autenticazione di Windows**.
4. Fare clic su **applica** e **OK**.

![Gestione Criteri di gruppo](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc5.png)

5. Usando PowerShell è possibile usare il cmdlet **set-AdfsGlobalAuthenticationPolicy** .

``` powershell
Set-AdfsGlobalAuthenticationPolicy -PrimaryIntranetAuthenticationProvider 'WindowsAuthentication'
```
>[!NOTE]
>In una farm basata su WID, il comando di PowerShell deve essere eseguito nel server di AD FS primario.
>In una farm basata su SQL, il comando di PowerShell può essere eseguito in qualsiasi AD FS server membro della farm.

### <a name="step-5--add-the-claim-description-to-ad-fs"></a>Passaggio 5:  Aggiungere la descrizione dell'attestazione a AD FS
1. Aggiungere la descrizione dell'attestazione seguente alla farm. Questa descrizione dell'attestazione non è presente per impostazione predefinita in ADFS 2012 R2 e deve essere aggiunta manualmente.
2. In gestione AD FS, in **servizio**, fare clic con il pulsante destro del mouse su **Richiedi Descrizione** e scegliere **Aggiungi descrizione attestazione** .
3. Immettere le informazioni seguenti nella descrizione dell'attestazione
   - Nome visualizzato: ' Gruppo di dispositivi Windows ' 
   - Descrizione attestazione:<https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup>'''
4. Inserire un segno di spunta in entrambe le caselle.
5. Fare clic su **OK**.

![Descrizione attestazione](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc6.png)

6. Usando PowerShell è possibile usare il cmdlet **Add-AdfsClaimDescription** .
   ``` powershell
   Add-AdfsClaimDescription -Name 'Windows device group' -ClaimType 'https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup' `
   -ShortName 'windowsdevicegroup' -IsAccepted $true -IsOffered $true -IsRequired $false -Notes 'The windows group SID of the device' 
   ```


>[!NOTE]
>In una farm basata su WID, il comando di PowerShell deve essere eseguito nel server di AD FS primario.
>In una farm basata su SQL, il comando di PowerShell può essere eseguito in qualsiasi AD FS server membro della farm.

### <a name="step-6--enable-the-compound-authentication-bit-on-the-msds-supportedencryptiontypes-attribute"></a>Passaggio 6:  Abilitare il bit di autenticazione composta nell'attributo msDS-SupportedEncryptionTypes

1.  Abilitare il bit di autenticazione composta nell'attributo msDS-SupportedEncryptionTypes dell'account designato per eseguire il servizio AD FS usando il cmdlet di PowerShell **Set-ADServiceAccount** .  

>[!NOTE]
>Se si modifica l'account del servizio, è necessario abilitare manualmente l'autenticazione composta eseguendo i cmdlet di Windows PowerShell **set-aduser-compoundIdentitySupported: $true** .

``` powershell
Set-ADServiceAccount -Identity “ADFS Service Account” -CompoundIdentitySupported:$true 
```
2. Riavviare il servizio ADFS.

>[!NOTE]
>Quando "CompoundIdentitySupported" è impostato su true, l'installazione dello stesso gMSA nei nuovi server (2012R2/2016) ha esito negativo con il **seguente errore – Install-ADServiceAccount: Impossibile installare l'account del servizio. Messaggio di errore: ' Il contesto specificato non corrisponde alla destinazione .'** .
>
>**Soluzione**: Impostare temporaneamente CompoundIdentitySupported su $false. Questo passaggio fa sì che ADFS interrompa l'emissione di attestazioni WindowsDeviceGroup. Set-ADServiceAccount-Identity ' account del servizio ADFS '-CompoundIdentitySupported: $false installare gMSA nel nuovo server e quindi abilitare di nuovo il CompoundIdentitySupported $True.
La disabilitazione di CompoundIdentitySupported e la successiva riabilitazione non richiede il riavvio del servizio ADFS.

### <a name="step-7-update-the-ad-fs-claims-provider-trust-for-active-directory"></a>Passaggio 7: Aggiornare il trust del provider di attestazioni AD FS per Active Directory

1. Aggiornare l'attendibilità del provider di attestazioni AD FS per Active Directory includere la regola di attestazione pass-through seguente per l'attestazione ' WindowsDeviceGroup '.
2.  In **gestione ad FS**fare clic su **attendibilità provider di attestazioni** e nel riquadro destro fare clic con il pulsante destro del mouse su **Active Directory** e scegliere **modifica regole attestazione**.
3.  In **modifica regole attestazione per Active Director** fare clic su **Aggiungi regola**.
4.  Nella **procedura guidata Aggiungi regola attestazione di trasformazione** selezionare **pass-through o filtrare un'attestazione in ingresso** e fare clic su **Avanti**.
5.  Aggiungere un nome visualizzato e selezionare **gruppo di dispositivi Windows** dall'elenco a discesa **tipo di attestazione in ingresso** .
6.  Scegliere **Fine**.  Fare clic su **applica** e **OK**. 
![Descrizione attestazione](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc7.png)

### <a name="step-8-on-the-relying-party-where-the-windowsdevicegroup-claims-are-expected-add-a-similar-pass-through-or-transform-claim-rule"></a>Passaggio 8: Nella relying party in cui sono previste le attestazioni ' WindowsDeviceGroup ', aggiungere una regola di attestazione ' pass-through ' o ' Transform ' simile.
2. In **gestione ad FS**fare clic su **attendibilità** del componente e nel riquadro destro fare clic con il pulsante destro del mouse sul componente e scegliere **modifica regole attestazione**.
3. In **regole di trasformazione rilascio** fare clic su **Aggiungi regola**.
4. Nella **procedura guidata Aggiungi regola attestazione di trasformazione** selezionare **pass-through o filtrare un'attestazione in ingresso** e fare clic su **Avanti**.
5. Aggiungere un nome visualizzato e selezionare **gruppo di dispositivi Windows** dall'elenco a discesa **tipo di attestazione in ingresso** .
6. Scegliere **Fine**.  Fare clic su **applica** e **OK**.
   ![Descrizione attestazione](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc8.png)


## <a name="steps-for-configuring-ad-fs-in-windows-server-2016"></a>Passaggi per la configurazione di AD FS in Windows Server 2016
Di seguito vengono descritte in dettaglio i passaggi per la configurazione dell'autenticazione composta in AD FS per Windows Server 2016.

### <a name="step-1--enable-kdc-support-for-claims-compound-authentication-and-kerberos-armoring-on-the-default-domain-controller-policy"></a>Passaggio 1:  Abilitare il supporto KDC per attestazioni, autenticazione composta e blindatura Kerberos nei criteri del controller di dominio predefinito
1.  In Server Manager selezionare strumenti, **gestione criteri di gruppo**.
2.  Passare al **criterio controller di dominio predefinito**, fare clic con il pulsante destro del mouse e scegliere **modifica**.
3.  Nel **Editor gestione criteri di gruppo**, in **Configurazione computer**espandere **criteri**, espandere **modelli amministrativi**, espandere **sistema**e selezionare **KDC**.
4.  Nel riquadro di destra fare doppio clic su **Supporto KDC per attestazioni, autenticazione composta e blindatura Kerberos**.
5.  Nella finestra di dialogo nuovo impostare Supporto KDC per attestazioni su **abilitato**.
6.  In Opzioni scegliere **supportato** dal menu a discesa e quindi fare clic su **applica** e **OK**.


### <a name="step-2-enable-kerberos-client-support-for-claims-compound-authentication-and-kerberos-armoring-on-computers-accessing-federated-applications"></a>Passaggio 2: Abilitare il supporto client Kerberos per attestazioni, autenticazione composta e blindatura Kerberos nei computer che accedono alle applicazioni federate

1.  In una Criteri di gruppo applicata ai computer che accedono alle applicazioni federate, nel **Editor gestione criteri di gruppo**, in **Configurazione computer**espandere **criteri**, espandere **modelli amministrativi**, espandere **sistema** e selezionare **Kerberos**.
2.  Nel riquadro destro della finestra di Editor Gestione Criteri di gruppo fare doppio clic su **supporto client Kerberos per attestazioni, autenticazione composta e blindatura Kerberos.**
3.  Nella finestra di dialogo nuovo impostare supporto client Kerberos su **abilitato** , quindi fare clic su **applica** e **OK**.
4.  Chiudi l'Editor Gestione Criteri di gruppo.

### <a name="step-3-configure-the-primary-authentication-provider"></a>Passaggio 3: Configurare il provider di autenticazione primario

1. Impostare il provider di autenticazione primario per l' **autenticazione di Windows** per ad FS le impostazioni Intranet.
2. In gestione AD FS, in **criteri di autenticazione**selezionare **autenticazione primaria** e in **Impostazioni globali** fare clic su **modifica**.
3. In **modifica criteri di autenticazione globali** in **Intranet** selezionare **autenticazione di Windows**.
4. Fare clic su **applica** e **OK**.
5. Usando PowerShell è possibile usare il cmdlet **set-AdfsGlobalAuthenticationPolicy** .

``` powershell
Set-AdfsGlobalAuthenticationPolicy -PrimaryIntranetAuthenticationProvider 'WindowsAuthentication'
```
>[!NOTE]
>In una farm basata su WID, il comando di PowerShell deve essere eseguito nel server di AD FS primario.
>In una farm basata su SQL, il comando di PowerShell può essere eseguito in qualsiasi AD FS server membro della farm.

### <a name="step-4--enable-the-compound-authentication-bit-on-the-msds-supportedencryptiontypes-attribute"></a>Passaggio 4:  Abilitare il bit di autenticazione composta nell'attributo msDS-SupportedEncryptionTypes

1.  Abilitare il bit di autenticazione composta nell'attributo msDS-SupportedEncryptionTypes dell'account designato per eseguire il servizio AD FS usando il cmdlet di PowerShell **Set-ADServiceAccount** .  

>[!NOTE]
>Se si modifica l'account del servizio, è necessario abilitare manualmente l'autenticazione composta eseguendo i cmdlet di Windows PowerShell **set-aduser-compoundIdentitySupported: $true** .

``` powershell
Set-ADServiceAccount -Identity “ADFS Service Account” -CompoundIdentitySupported:$true 
```
2. Riavviare il servizio ADFS.

>[!NOTE]
>Quando "CompoundIdentitySupported" è impostato su true, l'installazione dello stesso gMSA nei nuovi server (2012R2/2016) ha esito negativo con il **seguente errore – Install-ADServiceAccount: Impossibile installare l'account del servizio. Messaggio di errore: ' Il contesto specificato non corrisponde alla destinazione .'** .
>
>**Soluzione**: Impostare temporaneamente CompoundIdentitySupported su $false. Questo passaggio fa sì che ADFS interrompa l'emissione di attestazioni WindowsDeviceGroup. Set-ADServiceAccount-Identity ' account del servizio ADFS '-CompoundIdentitySupported: $false installare gMSA nel nuovo server e quindi abilitare di nuovo il CompoundIdentitySupported $True.
La disabilitazione di CompoundIdentitySupported e la successiva riabilitazione non richiede il riavvio del servizio ADFS.

### <a name="step-5-update-the-ad-fs-claims-provider-trust-for-active-directory"></a>Passaggio 5: Aggiornare il trust del provider di attestazioni AD FS per Active Directory

1. Aggiornare l'attendibilità del provider di attestazioni AD FS per Active Directory includere la regola di attestazione pass-through seguente per l'attestazione ' WindowsDeviceGroup '.
2.  In **gestione ad FS**fare clic su **attendibilità provider di attestazioni** e nel riquadro destro fare clic con il pulsante destro del mouse su **Active Directory** e scegliere **modifica regole attestazione**.
3.  In **modifica regole attestazione per Active Director** fare clic su **Aggiungi regola**.
4.  Nella **procedura guidata Aggiungi regola attestazione di trasformazione** selezionare **pass-through o filtrare un'attestazione in ingresso** e fare clic su **Avanti**.
5.  Aggiungere un nome visualizzato e selezionare **gruppo di dispositivi Windows** dall'elenco a discesa **tipo di attestazione in ingresso** .
6.  Scegliere **Fine**.  Fare clic su **applica** e **OK**. 


### <a name="step-6-on-the-relying-party-where-the-windowsdevicegroup-claims-are-expected-add-a-similar-pass-through-or-transform-claim-rule"></a>Passaggio 6: Nella relying party in cui sono previste le attestazioni ' WindowsDeviceGroup ', aggiungere una regola di attestazione ' pass-through ' o ' Transform ' simile.
2. In **gestione ad FS**fare clic su **attendibilità** del componente e nel riquadro destro fare clic con il pulsante destro del mouse sul componente e scegliere **modifica regole attestazione**.
3. In **regole di trasformazione rilascio** fare clic su **Aggiungi regola**.
4. Nella **procedura guidata Aggiungi regola attestazione di trasformazione** selezionare **pass-through o filtrare un'attestazione in ingresso** e fare clic su **Avanti**.
5. Aggiungere un nome visualizzato e selezionare **gruppo di dispositivi Windows** dall'elenco a discesa **tipo di attestazione in ingresso** .
6. Scegliere **Fine**.  Fare clic su **applica** e **OK**.

## <a name="validation"></a>Convalida
Per convalidare il rilascio di attestazioni ' WindowsDeviceGroup ', creare un'applicazione di test attestazioni con .NET 4,6. Con WIF SDK 4,0.
Configurare l'applicazione come relying party in ADFS e aggiornarla con la regola attestazione, come specificato nei passaggi precedenti.
Quando si esegue l'autenticazione all'applicazione utilizzando il provider di autenticazione integrata di Windows ADFS, viene eseguito il cast delle attestazioni seguenti.
![Convalida](media/AD-FS-Compound-Authentication-and-AD-DS-claims/gpmc9.png)

Le attestazioni per il computer/dispositivo possono ora essere utilizzate per controlli di accesso più completi.

Ad esempio, il **AdditionalAuthenticationRules** seguente indica ad FS richiamare l'autenticazione a più fattori se – l'utente che esegue l'autenticazione non è membro del gruppo di sicurezza "-1-5-21-2134745077-1211275016-3050530490-1117" e del computer (dove è l'utente L'autenticazione da) non è membro del gruppo di sicurezza "S-1-5-21-2134745077-1211275016-3050530490-1115 (WindowsDeviceGroup)"

Tuttavia, se viene soddisfatta una delle condizioni precedenti, non richiamare l'autenticazione a più fattori.

```
'NOT EXISTS([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/windowsdevicegroup", Value =~ "S-1-5-21-2134745077-1211275016-3050530490-1115"])
&& NOT EXISTS([Type == "https://schemas.microsoft.com/ws/2008/06/identity/claims/groupsid", Value =~ "S-1-5-21-2134745077-1211275016-3050530490-1117"])
=> issue(Type = "https://schemas.microsoft.com/ws/2008/06/identity/claims/authenticationmethod", Value = "https://schemas.microsoft.com/claims/multipleauthn");'
```
