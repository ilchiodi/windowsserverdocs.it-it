---
ms.assetid: 24c4b9bb-928a-4118-acf1-5eb06c6b08e5
title: Configurare AD FS 2016 e Azure MFA
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 11/01/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: be8e88ae36f344f1265fb76e66c19e0ac8aeb533
ms.sourcegitcommit: ffdfbb76c7f775e20d089d3f662d4f9a7d642f1e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/05/2018
---
# <a name="configure-ad-fs-2016-and-azure-mfa"></a>Configurare AD FS 2016 e Azure MFA

>Si applica a: Windows Server 2016

Se l'azienda è federata con Azure AD, è possibile utilizzare Azure multi-Factor Authentication per proteggere le risorse di ADFS, sia in locale e nel cloud. Autenticazione a più fattori Azure consente di eliminare le password e fornire un modo più sicuro per l'autenticazione.  A partire da Windows Server 2016, è possibile configurare autenticazione a più fattori di Azure per l'autenticazione principale. 
  
A differenza con AD FS in Windows Server 2012 R2, la scheda di autenticazione a più fattori Azure AD FS 2016 si integra direttamente con Azure AD e non richiede un server di autenticazione a più fattori di Azure in locale.   La scheda Azure MFA integrata in Windows Server 2016 e non è necessario per l'installazione aggiuntivi.

## <a name="registering-users-for-azure-mfa-with-ad-fs-2016"></a>Registering users for Azure MFA with AD FS 2016
ADFS non supporta i backup in linea "prova", o registrazione delle informazioni di verifica di protezione di Azure MFA, ad esempio il numero di telefono o app mobile. Questo significa che devono ottenere poter inavvertitamente agli utenti di visitando https://account.activedirectory.windowsazure.com/Proofup.aspx prima di utilizzare autenticazione a più fattori di Azure per l'autenticazione per le applicazioni di ADFS. Quando un utente che ha non ancora poter inavvertitamente backup in Azure AD tenta di eseguire l'autenticazione con Azure MFA in ADFS, verrà visualizzato un errore di ADFS.  In qualità di amministratore di AD FS, è possibile personalizzare questa esperienza di errore per guidare l'utente alla pagina di ufficio invece.  È possibile eseguire questa operazione utilizzando onload.js personalizzazione per rilevare la stringa di messaggio di errore all'interno della pagina di ADFS e mostrare un nuovo messaggio per gli utenti per visitare https://aka.ms/mfasetup, quindi riprovare a eseguire l'autenticazione. Per istruzioni dettagliate, vedere "personalizzare AD FS pagina web per guidare gli utenti per registrare i metodi di autenticazione a più fattori verifica" seguito in questo articolo.

>[!NOTE]
> In precedenza, gli utenti veniva richiesto di eseguire l'autenticazione con autenticazione a più fattori per la registrazione (visitare https://account.activedirectory.windowsazure.com/Proofup.aspx, ad esempio tramite il collegamento aka.ms/mfasetup).  A questo punto, un utente AD FS che non è ancora registrato le informazioni di verifica di autenticazione a più fattori può accedere ad Azure AD dell'ufficio pagina tramite il collegamento aka.ms/mfasetup utilizzando l'autenticazione solo principale (ad esempio l'autenticazione integrata di Windows o nome utente e password tramite AD FS pagine web) .  Se l'utente non ha configurato alcun metodo di verifica, Azure AD eseguirà la registrazione in linea in cui l'utente visualizza il messaggio "l'amministratore è necessario impostare questo account per la verifica aggiuntiva di protezione", e l'utente può quindi selezionare "Configurarlo ora".
> Gli utenti che hanno già almeno un metodo di verifica di autenticazione a più fattori configurato verranno comunque richiesto per fornire l'autenticazione a più fattori quando si visita la pagina di ufficio.

### <a name="recommended-deployment-topologies"></a>Topologie di distribuzione consigliata

#### <a name="azure-mfa-as-primary-authentication"></a>Autenticazione a più fattori Azure come l'autenticazione principale
Esistono un paio di ottimi motivi per usare l'autenticazione a più fattori Azure come l'autenticazione principale con AD FS:
 - Per evitare le password per l'accesso ad Azure AD, Office 365 e altre app di ADFS
 - Per proteggere le password basati su accesso richiedendo un fattore aggiuntivo, ad esempio il codice di verifica prima la password

Se si desidera utilizzare autenticazione a più fattori Azure come metodo di autenticazione principale in ADFS per ottenere questi vantaggi, è possibile mantenere la possibilità di usare Azure AD condizionale accesso inclusi "true autenticazione a più fattori" richiedendo per altri fattori in ADFS.

È ora possibile farlo configurando l'impostazione del dominio Azure AD per eseguire l'autenticazione a più fattori in locale (impostazione "SupportsMfa" $True).  In questa configurazione, ADFS può essere richiesto da Azure AD di eseguire l'autenticazione aggiuntiva o "true autenticazione a più fattori" per gli scenari di accesso condizionale che lo richiedono.  

Come descritto in precedenza, qualsiasi utente di ADFS che non è ancora stato registrato (configurato autenticazione a più fattori verifica informazioni) devono essere richiesto tramite una pagina di errore personalizzata di ADFS a visitare aka.ms/mfasetup per configurare le informazioni di verifica, quindi ritentare accesso di ADFS.  
Poiché l'autenticazione a più fattori Azure come principale viene considerato un singolo fattore, dopo la configurazione iniziale è necessario fornire un fattore aggiuntivo per gestire o aggiornare le informazioni di verifica in Azure AD o per accedere ad altre risorse che richiedono l'autenticazione a più fattori.


#### <a name="azure-mfa-as-additional-authentication-to-office-365"></a>Autenticazione a più fattori Azure come l'autenticazione aggiuntiva a Office 365
In precedenza, se si desidera disporre di autenticazione a più fattori Azure come metodo di autenticazione aggiuntiva in ADFS per Office 365 o altri relying party, l'opzione migliore consiste nel configurare Azure AD per composta autenticazione a più fattori, in cui viene eseguita l'autenticazione principale in locale in AD FS e autenticazione a più fattori è tr iggered da Azure AD. A questo punto, è possibile utilizzare autenticazione a più fattori Azure come l'autenticazione aggiuntiva in ADFS, quando l'impostazione SupportsMfa del dominio è impostato su $True.  

Come descritto in precedenza, qualsiasi utente di ADFS che non è ancora stato registrato (configurato autenticazione a più fattori verifica informazioni) devono essere richiesto tramite una pagina di errore personalizzata di ADFS a visitare aka.ms/mfasetup per configurare le informazioni di verifica, quindi ritentare accesso di ADFS.  


## <a name="pre-requisites"></a>Prerequisiti  
Quando si utilizza l'autenticazione a più fattori di Azure per l'autenticazione con ADFS, sono necessari i seguenti prerequisiti:  
  
- Un [sottoscrizione di Azure con Azure Active Directory](https://azure.microsoft.com/pricing/free-trial/).  
- [Azure multi-Factor Authentication](https://azure.microsoft.com/documentation/articles/multi-factor-authentication/)  

>[!NOTE]   
> Azure AD e autenticazione a più fattori di Azure sono incluse in Azure AD Premium ed Enterprise Mobility Suite (EMS).  Se si dispone di uno di questi non è necessario sottoscrizioni individuali.   
- Un ambiente locale di ADFS a Windows Server 2016.  
- L'ambiente locale sia [federata con Azure AD.](https://azure.microsoft.com/documentation/articles/active-directory-aadconnect-get-started-custom/#configuring-federation-with-ad-fs)  
- [Windows Azure Active Directory Module for Windows PowerShell](https://go.microsoft.com/fwlink/p/?linkid=236297).  
- Autorizzazioni di amministratore globale sull'istanza di Azure AD per configurarlo mediante Azure AD PowerShell.  
- Credenziali di amministratore dell'organizzazione per configurare la farm di ADFS per l'autenticazione a più fattori di Azure.  
  
  
## <a name="configure-the-ad-fs-servers"></a>Configurare il server ADFS  
Per completare la configurazione per Azure MFA per ADFS, è necessario configurare ogni server ADFS eseguendo la procedura descritta. 

>[!NOTE]
>Assicurarsi che questi passaggi vengono eseguiti su **tutti** server ADFS nella farm. Se si dispone di più server ADFS nella farm, è possibile eseguire la configurazione necessaria in remoto mediante Azure AD Powershell.  
  
  
  
### <a name="step-1-generate-a-certificate-for-azure-mfa-on-each-ad-fs-server-using-the-new-adfsazuremfatenantcertificate-cmdlet"></a>Passaggio 1: Generare un certificato per autenticazione a più fattori di Azure su ogni server ADFS utilizzando il `New-AdfsAzureMfaTenantCertificate`cmdlet.   
La prima cosa che è necessario eseguire è generare un certificato per autenticazione a più fattori di Azure da utilizzare.  Questa operazione può essere eseguita tramite PowerShell.  Il certificato generato è reperibile nell'archivio certificati computer locali ed è contrassegnata con un nome soggetto contenente il TenantID per la directory di Azure AD.    
  
![AD FS e autenticazione a più fattori](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA3.png)  
  
Si noti che TenantID è il nome della directory in Azure AD.  Utilizzare il seguente cmdlet PowerShell per generare il nuovo certificato.  
    `$certbase64 = New-AdfsAzureMfaTenantCertificate -TenantID <tenantID>`  
      
![AD FS e autenticazione a più fattori](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA1.PNG)  
  
### <a name="step-2-add-the-new-credentials-to-azure-multi-factor-auth-client-spn"></a>Passaggio 2: Aggiungere le nuove credenziali per Azure multi-Factor Authentication Client SPN   
Per abilitare i server ADFS di comunicare con il Client di Azure multi-Factor Authentication, è necessario aggiungere le credenziali per il nome SPN per il Client di Azure multi-Factor Authentication. I certificati generati utilizzando il `New-AdfsAzureMFaTenantCertificate`cmdlet fungerà da queste credenziali. Eseguire le operazioni seguenti tramite PowerShell per aggiungere le nuove credenziali per Azure multi-Factor Authentication Client SPN.  

>[!NOTE]   
>Per completare questo passaggio è necessario per connettersi all'istanza di Azure Active Directory con PowerShell Connect-MsolService.  Questi passaggi presuppongono che si sia già connessi tramite PowerShell.  Per informazioni vedere [Connect-MsolService.](https://msdn.microsoft.com/library/dn194123.aspx)  
     
  
2. **Impostare il certificato come le nuove credenziali per il Client di Azure multi-Factor Authentication**  
    
    `New-MsolServicePrincipalCredential -AppPrincipalId 981f26a1-7f43-403b-a875-f8b09b8cd720 -Type asymmetric -Usage verify -Value $certBase64 `

>[!IMPORTANT]
>  This command needs to be run on all of the AD FS servers in your farm.  Azure AD MFA will fail on servers that have not have the certificate set as the new credential against the Azure Multi-Factor Auth Client. 

>[!NOTE]  
> 981f26a1-7f43-403b-a875-f8b09b8cd720 è il guid per il Client di Azure multi-Factor Authentication.  
  
## <a name="configure-the-ad-fs-farm"></a>Configurare la Farm ADFS  
  
Dopo aver completato la sezione precedente per ogni server ADFS, è necessario eseguire il `Set-AdfsAzureMfaTenant`cmdlet.  
  
Questo cmdlet deve essere eseguito una sola volta per una farm ADFS.  Utilizzare PowerShell per completare questo passaggio.    
  
>[!NOTE]  
>È necessario riavviare il servizio ADFS in ogni server della farm, prima che le modifiche diventano effettive.  
  
    Set-AdfsAzureMfaTenant -TenantId <tenant ID> -ClientId 981f26a1-7f43-403b-a875-f8b09b8cd720  
      
![AD FS e autenticazione a più fattori](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA5.png)  
  
Successivamente, si noterà che è disponibile come metodo di autenticazione principale per intranet ed extranet utilizzare autenticazione a più fattori di Azure.    
  
![AD FS e autenticazione a più fattori](media/Configure-AD-FS-2016-and-Azure-MFA/ADFS_AzureMFA6.png)  

## <a name="renew-and-manage-ad-fs-azure-mfa-certificates"></a>Il rinnovo e AD FS Azure di gestire i certificati di autenticazione a più fattori
Le indicazioni seguenti vengono illustrati come gestire i certificati di autenticazione a più fattori di Azure nel server ADFS.
Per impostazione predefinita, quando si configura ADFS con autenticazione a più fattori di Azure, i certificati generati tramite il cmdlet PowerShell New-AdfsAzureMfaTenantCertificate sono validi per 2 anni.  Per determinare come chiudere a scadenza dei certificati sono e quindi utilizzare la seguente procedura per rinnovare e installare nuovi certificati.

### <a name="assess-ad-fs-azure-mfa-certificate-expiration-date"></a>Valutare data di scadenza certificato di autenticazione a più fattori Azure di AD FS
In ogni server ADFS, il computer locale archivio personale, sarà presente un certificato autofirmato con "OU = Microsoft AD FS Azure MFA" nell'oggetto dell'autorità di certificazione.  Si tratta del certificato di autenticazione a più fattori di Azure.  Controllare il periodo di validità del certificato in ogni server ADFS per determinare la data di scadenza.  

### <a name="create-new-ad-fs-azure-mfa-certificate-on-each-ad-fs-server"></a>Creare nuovi Azure MFA certificato ADFS in ogni server ADFS
Se il periodo di validità dei certificati è prossimo alla fine, avviare il processo di rinnovo tramite la generazione di un nuovo certificato di autenticazione a più fattori di Azure in ogni server ADFS. In una finestra di comando di powershell, generare un nuovo certificato in ogni server ADFS utilizzando il cmdlet seguente:

```
PS C:\> $newcert = New-AdfsAzureMfaTenantCertificate -TenantId <tenant id such as contoso.onmicrosoft.com> -Renew $true
```

In seguito a questo cmdlet, verrà generato un nuovo certificato valido da 2 giorni in futuro per 2 giorni + 2 anni.  Operazioni di ADFS e Azure MFA non interesserà questo cmdlet o il nuovo certificato. (Nota: il ritardo di 2 giorni è intenzionale e tempo di eseguire la procedura seguente per configurare il nuovo certificato in tenant prima dell'avvio di ADFS usando per Azure MFA.)


### <a name="configure-each-new-ad-fs-azure-mfa-certificate-in-the-azure-ad-tenant"></a>Configurare ogni nuovo certificato di autenticazione a più fattori Azure di AD FS tenant di Azure AD
Con il modulo di Azure AD PowerShell, per ogni nuovo certificato (in ogni server AD FS), aggiornare le impostazioni di tenant di Azure AD come descritto di seguito (Nota: è innanzitutto necessario connettere per il tenant mediante Connect-MsolService per eseguire i comandi seguenti).

```
PS C:/> New-MsolServicePrincipalCredential -AppPrincipalId 981f26a1-7f43-403b-a875-f8b09b8cd720 -Type Asymmetric -Usage Verify -Value $certbase64
```
    Where $certbase64 is the new certificate.  The base64 encoded certificate can be obtained by exporting the certificate (without the private key) as a DER encoded file and opening in Notepad.exe, then copy/pasting to the PSH session and assigning to the variable $certbase64

### <a name="verify-that-the-new-certificates-will-be-used-for-azure-mfa"></a>Verificare che i nuovi certificati da utilizzare per l'autenticazione a più fattori di Azure
Una volta i nuovi certificati diventino validi, AD FS ritiro e iniziare a usare ogni rispettivo certificato per autenticazione a più fattori Azure entro poche ore al giorno.  Una volta in tal caso, in ogni server verrà visualizzato un evento registrato nel registro eventi dell'amministratore di AD FS con le seguenti informazioni: nome registro: AD ADFS/Amministratore origine: AD FS data: 27/2/2018 ID evento 7:33:31 PM: 547 categoria attività: None livello: informazioni parole chiave : Utente di ADFS di Active Directory: Computer DOMAIN\adfssvc: ADFS.domain.contoso.com descrizione: il certificato di tenant per l'autenticazione a più fattori di Azure è stato rinnovato.  

TenantId: contoso.onmicrosoft.com. Identificazione personale precedente: 7CC103D60967318A11D8C51C289EF85214D9FC63. Data di scadenza precedente: 15/9/2019 9:43:17 PM. Identificazione personale del nuovo: 8110D7415744C9D4D5A4A6309499F7B48B5F3CCF. Nuova data di scadenza: 27/2/2020 2:16:07 AM.

## <a name="customize-the-ad-fs-web-page-to-guide-users-to-register-mfa-verification-methods"></a>Personalizzare la pagina web di ADFS per guidare gli utenti per registrare i metodi di verifica di autenticazione a più fattori

Usa gli esempi seguenti per personalizzare le pagine web di ADFS per gli utenti che hanno non ancora poter inavvertitamente backup (configurato le informazioni di verifica di autenticazione a più fattori).

### <a name="find-the-error"></a>Individuare l'errore
Prima di tutto, ci sono un paio di diversi messaggi di errore che ADFS restituirà nel caso in cui l'utente non dispone di informazioni di verifica.
Se si utilizza l'autenticazione a più fattori Azure come l'autenticazione principale, l'utente non scalabile visualizzerà una pagina di errore di ADFS che contiene i messaggi seguenti:
```
    <div id="errorArea"> 
        <div id="openingMessage" class="groupMargin bigText">
            An error occurred 
        </div> 
        <div id="errorMessage" class="groupMargin">
            Authentication attempt failed. Select a different sign in option or close the web browser and sign in again. Contact your administrator for more information. 
        </div>
```
Quando il tentativo di Azure AD come l'autenticazione aggiuntiva, l'utente non scalabile Visualizza una pagina di errore di ADFS che contiene i messaggi seguenti:
```
<div id='mfaGreetingDescription' class='groupMargin'>For security reasons, we require additional information to verify your account (mahesh@jenfield.net)</div>
    <div id="errorArea"> 
        <div id="openingMessage" class="groupMargin bigText">
            An error occurred 
        </div> 
        <div id="errorMessage" class="groupMargin">
            The selected authentication method is not available for &#39;username@contoso.com&#39;. Choose another authentication method or contact your system administrator for details. 
        </div>
```

### <a name="catch-the-error-and-update-the-page-text"></a>Intercettare l'errore e aggiornare il testo di pagina
Intercettare l'errore e mostrare all'utente personalizzato, è necessario aggiungere javascript alla fine del file onload.js che fa parte di AD FS indicazioni web tema per (1) cercare le stringhe di identificazione errore e (2) fornire personalizzato contenuto Web.  (Per istruzioni in generale su come personalizzare il file onload.js, vedere l'articolo [qui](https://docs.microsoft.com/en-us/windows-server/identity/ad-fs/operations/advanced-customization-of-ad-fs-sign-in-pages).)

