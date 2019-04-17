---
ms.assetid: 35de490f-c506-4b73-840c-b239b72decc2
title: Configurare l'accesso condizionale basato su dispositivo locale
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 08/11/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 47afd0c6963bd8b8b4dde82650cf807c1954b40b
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="configure-on-premises-conditional-access-using-registered-devices"></a>Configurare On-Premise accesso condizionale utilizzando i dispositivi registrati

>Si applica a: Windows Server 2016, Windows Server 2012 R2  

Il documento seguente costituisce una Guida per l'installazione e configurazione di accesso condizionale locale con i dispositivi registrati.

![Accesso condizionale](media/Using-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  

## <a name="infrastructure-pre-requisites"></a>Prerequisiti dell'infrastruttura
I prerequisiti seguenti sono necessari prima di iniziare con accesso condizionale locale. 

|Requisito|Descrizione
|-----|-----
|Sottoscrizione di Azure AD con Azure AD Premium | Per abilitare la scrittura di dispositivo per accesso condizionale locale - [una versione di valutazione gratuita è bene](https://azure.microsoft.com/en-us/trial/get-started-active-directory/)  
|Sottoscrizione Intune|Richiesto solo per l'integrazione con MDM per scenari di conformità dispositivo -[una versione di valutazione gratuita è bene](https://portal.office.com/Signup/Signup.aspx?OfferId=40BE278A-DFD1-470a-9EF7-9F2596EA7FF9&dl=INTUNE_A&ali=1#0)
|Azure AD Connect|QFE novembre 2015 o versione successiva.  Ottenere la versione più recente [qui](https://www.microsoft.com/en-us/download/details.aspx?id=47594).  
|Windows Server 2016|Compilazione 10586 o versione successiva per ADFS  
|Schema di Active Directory di Windows Server 2016|Livello di schema 85 o versione successiva è richiesto.
|Controller di dominio di Windows Server 2016|Questo è solo necessario per le distribuzioni di attendibilità chiave Hello per Business.  Ulteriori informazioni, visitare [qui](https://aka.ms/whfbdocs).  
|Client Windows 10|Compilare 10586 o successive, unito al dominio precedente è necessaria per Windows 10 aggiunta a un dominio e Microsoft Passport lavoro solo per gli scenari  
|Account utente AD Azure con licenza Azure AD Premium|Per la registrazione del dispositivo  


 
## <a name="upgrade-your-active-directory-schema"></a>Aggiornare lo Schema di Active Directory
Per utilizzare l'accesso condizionale locale con i dispositivi registrati, è necessario prima aggiornare lo schema di Active Directory.  Devono essere soddisfatte le condizioni seguenti:
    - Lo schema deve essere 85 o versione successiva
    - Questo è solo necessario per la foresta che ADFS è stato aggiunto a

> [!NOTE]
> Se hai installato Azure AD Connect prima dell'aggiornamento alla versione dello schema (livello 85 o superiore) in Windows Server 2016, è necessario eseguire nuovamente l'installazione di Azure AD Connect e locale di aggiornamento dello schema di Active Directory per garantire la regola di sincronizzazione per msDS-KeyCredentialLink è configurata.

### <a name="verify-your-schema-level"></a>Verificare il livello di schema
Per verificare il livello di schema, eseguire le operazioni seguenti:

1.  È possibile utilizzare Modifica ADSI o LDP e connettersi al contesto dei nomi di Schema.  
2.  Uso di ADSIEdit, fare clic su "CN = Schema, CN = Configuration, DC =<domain>, DC =<com> e selezionare proprietà.  Dominio Relpace e le parti di com con le informazioni sull'insieme di strutture.
3.  In Editor attributi individuare l'attributo objectVersion e ti farà sapere, la versione in uso.  

![Modifica ADSI](media/Configure-Device-Based-Conditional-Access-on-Premises/adsiedit.png)  

È inoltre possibile utilizzare il seguente cmdlet PowerShell (sostituire l'oggetto con lo schema di denominazione delle informazioni di contesto):

``` powershell
Get-ADObject "cn=schema,cn=configuration,dc=domain,dc=local" -Property objectVersion
    
```

![PowerShell](media/Configure-Device-Based-Conditional-Access-on-Premises/pshell1.png) 

Per ulteriori informazioni sull'aggiornamento, vedere [aggiornare controller di dominio a Windows Server 2016](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2016.md). 

## <a name="enable-azure-ad-device-registration"></a>Abilitare la registrazione del dispositivo AD Azure  
Per configurare questo scenario, è necessario configurare la funzionalità di registrazione del dispositivo in Azure AD.  

A tale scopo, seguire i passaggi descritti nella [impostazione aggiunta ad Azure AD nell'organizzazione](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-setup/)  

## <a name="setup-ad-fs"></a>Installazione di ADFS  
1. Creazione di un [nuova farm AD FS 2016](https://technet.microsoft.com/library/dn486775.aspx).   
2.  O [eseguire la migrazione](../../ad-fs/deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md) una farm con AD FS 2016 da AD FS 2012 R2  
4. Distribuire [Azure AD Connect](https://azure.microsoft.com/documentation/articles/active-directory-aadconnectfed-whatis/) utilizzando il percorso personalizzato per la connessione AD FS in Azure AD.  

## <a name="configure-device-write-back-and-device-authentication"></a>Configurare nuovamente scrittura dispositivo e l'autenticazione del dispositivo  
> [!NOTE]
> Se si eseguisse Azure AD Connect con impostazioni rapide, gli oggetti di Active Directory corretti sono stati creati per te.  Tuttavia, nella maggior parte degli scenari di ADFS, Azure AD Connect è stato eseguito con impostazioni personalizzate per configurare ADFS, quindi i passaggi seguenti sono necessari.  

### <a name="create-ad-objects-for-ad-fs-device-authentication"></a>Creare gli oggetti Active Directory per l'autenticazione del dispositivo AD FS  
Se la farm ADFS non è già configurata per l'autenticazione del dispositivo (si può vedere nella console di gestione di ADFS in servizio -> registrazione del dispositivo), utilizzare la procedura seguente per creare la configurazione e oggetti di dominio Active Directory corretti.  

![Registrazione del dispositivo](media/Configure-Device-Based-Conditional-Access-on-Premises/device1.png)

>Nota: I comandi seguenti richiedono gli strumenti di amministrazione di Active Directory, pertanto, se il server federativo non è anche un controller di dominio, innanzitutto installare gli strumenti mediante il passaggio 1 riportato di seguito.  In caso contrario è possibile ignorare il passaggio 1.  

1.  Eseguire il **Aggiungi ruoli e funzionalità** procedura guidata e selezionare funzionalità **strumenti di amministrazione remota del Server** -> **strumenti di amministrazione ruoli** -> **di dominio Active Directory e AD LDS strumenti** -> scegliere entrambi **modulo Active Directory per Windows PowerShell** e **strumenti di AD DS**.

![Registrazione del dispositivo](media/Configure-Device-Based-Conditional-Access-on-Premises/device2.png)
  
2.  Nel server ADFS primario, assicurarsi di essere connessi come utente di dominio Active Directory con privilegi di amministratore di Enterprise (EA) e aprire un prompt dei comandi di powershell con privilegi elevati.  Quindi, eseguire i comandi di PowerShell seguenti:  
    
    `Import-module activedirectory`  
    `PS C:\> Initialize-ADDeviceRegistration -ServiceAccountName "<your service account>" ` 
3.  Nella finestra a comparsa premere Sì.

>Nota: Se il servizio ADFS è configurato per utilizzare un account gestito, immettere il nome dell'account nel formato "domain\accountname$"

![Registrazione del dispositivo](media/Configure-Device-Based-Conditional-Access-on-Premises/device3.png)  

PSH precedente crea gli oggetti seguenti:  


- Contenitore RegisteredDevices nella partizione di dominio Active Directory  
- Contenitore del servizio Registrazione dispositivi e sull'oggetto configurazione--> Servizi--> Configurazione registrazione del dispositivo  
- Contenitore DKM servizio Registrazione dispositivi e sull'oggetto configurazione--> Servizi--> Configurazione registrazione del dispositivo  

![Registrazione del dispositivo](media/Configure-Device-Based-Conditional-Access-on-Premises/device4.png)  

4.  Al termine, vedrai un messaggio di completamento.

![Registrazione del dispositivo](media/Configure-Device-Based-Conditional-Access-on-Premises/device5.png) 

###        <a name="create-service-connection-point-scp-in-ad"></a>Creare il punto di connessione del servizio (SCP) in Active Directory  
Se si prevede di utilizzare aggiunta al dominio di Windows 10 (con la registrazione automatica in Azure AD) come descritto in questo caso, eseguire i comandi seguenti per creare un punto di connessione del servizio in Active Directory  
1.  Aprire Windows PowerShell ed eseguire le operazioni seguenti:
    
    `PS C:>Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1" ` 

>Nota: se necessario, copiare il file adsyncprep. Psm1 dal server di Azure AD Connect.  Questo file si trova nel programma c:\Programmi\Microsoft Azure Active Directory Connect\AdPrep

![Registrazione del dispositivo](media/Configure-Device-Based-Conditional-Access-on-Premises/device6.png)   

2. Fornire le credenziali di amministratore globale Azure AD  

    `PS C:>$aadAdminCred = Get-Credential`

![Registrazione del dispositivo](media/Configure-Device-Based-Conditional-Access-on-Premises/device7.png) 

3.  Eseguire il seguente comando di PowerShell 

    `PS C:>Initialize-ADSyncDomainJoinedComputerSync -AdConnectorAccount [AD connector account name] -AzureADCredentials $aadAdminCred ` 

Dove il valore [Active Directory connector account name] è il nome dell'account è stato configurato in Azure AD Connect quando si aggiungono locale directory di Active directory.
  
I comandi precedenti consentono ai client di Windows 10 di trovare le impostazioni corrette dominio Azure AD a cui aggiungere creando l'oggetto serviceConnectionpoint in Active Directory.  

### <a name="prepare-ad-for-device-write-back"></a>Preparare Active Directory per la scrittura di dispositivo indietro   
Per garantire contenitori e oggetti di dominio Active Directory sono nello stato corretto per la scrittura posteriore di dispositivi da Azure AD, eseguire le operazioni seguenti.

1.  Aprire Windows PowerShell ed eseguire le operazioni seguenti:  

    `PS C:>Initialize-ADSyncDeviceWriteBack -DomainName <AD DS domain name> -AdConnectorAccount [AD connector account name] ` 

Dove il valore [Active Directory connector account name] è il nome dell'account è stato configurato in Azure AD Connect quando si aggiungono locale directory di Active directory in formato domain\accountname  

Il comando precedente crea gli oggetti seguenti per la scrittura di dispositivo al dominio Active Directory, se non esistono già e consente l'accesso al nome dell'account specificato Active Directory connector  

- Contenitore RegisteredDevices nella partizione di dominio Active Directory  
- Contenitore del servizio Registrazione dispositivi e sull'oggetto configurazione--> Servizi--> Configurazione registrazione del dispositivo  

### <a name="enable-device-write-back-in-azure-ad-connect"></a>Abilitare la scrittura di dispositivo nuovamente in Azure AD Connect  
Se non è già pertanto, prima, abilitare la scrittura di dispositivo in Azure AD Connect eseguendo la procedura guidata una seconda volta e scegliendo **"Personalizzare le opzioni di sincronizzazione"**, quindi selezionando la casella per la scrittura di dispositivo indietro e selezionare la foresta in cui è stato eseguito il cmdlet precedente  

### <a name="configure-device-authentication-in-ad-fs"></a>Configurare l'autenticazione del dispositivo in ADFS  
Utilizzando una finestra di comando di PowerShell con privilegi elevata, configurare i criteri di ADFS eseguendo il comando seguente  

`PS C:>Set-AdfsGlobalAuthenticationPolicy -DeviceAuthenticationEnabled $true -DeviceAuthenticationMethod All` 

### <a name="check-your-configuration"></a>Controllare la configurazione  
Per riferimento, ecco un elenco completo dei dispositivi, contenitori e autorizzazioni necessari per l'autenticazione e writeback del dispositivo per l'utilizzo di dominio Active Directory
 


- Oggetto di tipo ms-DS-DeviceContainer in CN = RegisteredDevices, DC =&lt;dominio&gt;        
    - Accesso in lettura all'account del servizio ADFS   
    - Accesso in lettura/scrittura per l'account di Active Directory connector di sincronizzazione di Azure AD Connect</br></br>

- Contenitore CN = dispositivo registrazione Configuration, CN = Services, CN = Configuration, DC =&lt;dominio&gt;  
- Contenitore dispositivo registrazione servizio distribuite sotto il contenitore precedente

![Registrazione del dispositivo](media/Configure-Device-Based-Conditional-Access-on-Premises/device8.png) 
 


- Oggetto di tipo serviceConnectionpoint in CN =&lt;guid&gt;, CN = registrazione del dispositivo

- Configuration, CN = Services, CN = Configuration, DC =&lt;dominio&gt;  
 - Accesso in lettura/scrittura per il nome di Active Directory connector account specificato per il nuovo oggetto</br></br> 


- Oggetto di tipo msDS-DeviceRegistrationServiceContainer in CN = dispositivo registrazione Services, CN = dispositivo registrazione Configuration, CN = Services, CN = Configuration, DC = & ltdomain >  


- Oggetto di tipo msDS-DeviceRegistrationService nel contenitore precedente  

### <a name="see-it-work"></a>Verificarne il funzionamento  
Per valutare le nuove attestazioni e i criteri, innanzitutto per registrare un dispositivo.  Ad esempio, è possibile un computer Windows 10 tramite l'app impostazioni nel sistema -> informazioni su appartenenza ad Azure AD o aggiunta al dominio di Windows 10 è possibile impostare con registrazione automatica dei dispositivi seguendo i passaggi aggiuntivi [qui](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/).  Per informazioni sull'unione di Windows 10 i dispositivi mobili, vedere il documento [qui](https://technet.microsoft.com/itpro/windows/manage/join-windows-10-mobile-to-azure-active-directory).  

Per la valutazione più semplice, accedere ad ADFS con un'applicazione di test viene visualizzato un elenco di attestazioni. Sarà in grado di vedere nuove attestazioni, tra cui isManaged isCompliant e trusttype.  Se si abilita Microsoft Passport per il lavoro, si vedrà anche la prt di attestazione.  
 

## <a name="configure-additional-scenarios"></a>Configurare scenari aggiuntivi  
### <a name="automatic-registration-for-windows-10-domain-joined-computers"></a>Computer di registrazione per Windows 10 aggiunti a un dominio automatico  
Per abilitare la registrazione automatica dei dispositivi per Windows 10 dominio associato al computer, seguire i passaggi 1 e 2 [qui](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/).   
Ciò consentirà di ottenere quanto segue:  

1. Verificare il punto di connessione del servizio di dominio Active Directory esista e disponga delle autorizzazioni appropriate (abbiamo creato questo oggetto precedente, ma non guasta controllare).  
2. Verificare che ADFS è configurato correttamente  
3. Assicurarsi che il sistema di ADFS dispone di endpoint corretto abilitato e configurate regole attestazione   
4. Configurare le impostazioni di criteri di gruppo necessari per la registrazione automatica dei dispositivi dei computer appartenenti a un dominio   

### <a name="microsoft-passport-for-work"></a>Microsoft Passport for Work   
Per informazioni sull'attivazione di Windows 10 con Microsoft Passport for Work, vedere [abilitare Microsoft Passport for Work all'interno dell'organizzazione.](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-passport-deployment/)  

### <a name="automatic-mdm-enrollment"></a>Registrazione automatica in MDM   
Per abilitare la registrazione MDM automatica dei dispositivi registrati in modo che è possibile utilizzare l'attestazione isCompliant nei criteri di controllo di accesso, attenersi alla procedura [qui.](https://blogs.technet.microsoft.com/ad/2015/08/14/windows-10-azure-ad-and-microsoft-intune-automatic-mdm-enrollment-powered-by-the-cloud/)  

## <a name="troubleshooting"></a>Risoluzione dei problemi  
1.  Se si verifica un errore `Initialize-ADDeviceRegistration`che visualizza un errore su un oggetto già esistente in stato non corretto, ad esempio "oggetto del servizio drs è stata trovata senza tutti gli attributi obbligatori", è possibile che sia eseguita Azure AD Connect in precedenza i comandi di powershell e dispone di una configurazione parziale in Active Directory.  Provare a eliminare manualmente gli oggetti in **CN = dispositivo registrazione Configuration, CN = Services, CN = Configuration, DC =&lt;dominio&gt;** e riprovare.  
2.  Per Windows 10 dominio aggiunto client  
    1. Per verificare che l'autenticazione del dispositivo è funzionante, accedere al dominio associato al client come un account utente test. Per attivare rapidamente il provisioning, bloccare e sbloccare il desktop almeno una volta.   
    2. Istruzioni per verificare la presenza di credenziali chiave stk collegano nell'oggetto di dominio Active Directory (sincronizzazione ancora necessario eseguire due volte?)  
3.  Se ricevi un errore durante il tentativo di registrare un computer Windows che il dispositivo è stato già registrato, ma sono in grado o hanno già annullata registrazione del dispositivo, potrebbe essere un frammento di configurazione della registrazione dispositivo nel Registro di sistema.  Per ricercare e rimuovere questo, attenersi alla seguente procedura:  
    1. Nel computer Windows, aprire Regedit e passare a **HKLM\Software\Microsoft\Enrollments**   
    2. In questa chiave verrà sottochiavi molti nel formato GUID.  Passare alla sottochiave che contiene i valori ~ 17 e ha "EnrollmentType" di "6" [MDM unita in join] o "13" (Azure AD unita in join)  
    3. Modificare **EnrollmentType** a **0** 
    4. Provare a eseguire nuovamente la registrazione del dispositivo o registrazione  

### <a name="related-articles"></a>Articoli correlati  
* [Protezione dell'accesso a Office 365 e altre App connesso ad Azure Active Directory](https://azure.microsoft.com/en-us/documentation/articles/active-directory-conditional-access/)  
* [Criteri di dispositivi di accesso condizionale per servizi di Office 365](https://azure.microsoft.com/en-us/documentation/articles/active-directory-conditional-access-device-policies/)  
* [Configurazione di accesso condizionale locale con Azure Active Directory Device Registration](https://docs.microsoft.com/en-us/azure/active-directory/active-directory-device-registration-on-premises-setup)  
* [Connettere dispositivi appartenenti a un dominio ad Azure AD per Windows 10 esperienze](https://azure.microsoft.com/en-us/documentation/articles/active-directory-azureadjoin-devices-group-policy/)  
