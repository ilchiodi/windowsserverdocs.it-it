---
ms.assetid: 35de490f-c506-4b73-840c-b239b72decc2
title: Configurare l'accesso condizionale locale basato su dispositivo
author: billmath
ms.author: billmath
manager: femila
ms.date: 08/11/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 19e139df53cd1c076f8d5597c1c68b8ffe2cfe91
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80817174"
---
# <a name="configure-on-premises-conditional-access-using-registered-devices"></a>Configura On-Premise accesso condizionale utilizzando i dispositivi registrati


Il documento seguente costituisce una Guida per l'installazione e configurazione dell'accesso condizionale locale con i dispositivi registrati.

![accesso condizionale](media/Using-Device-based-Conditional-Access-on-Premises/ADFS_ITPRO4.png)  

## <a name="infrastructure-pre-requisites"></a>Prerequisiti dell'infrastruttura
I prerequisiti seguenti sono necessari prima di iniziare con accesso condizionale locale. 

|Requisito|Descrizione
|-----|-----
|Sottoscrizione di Azure AD con Azure AD Premium | Per consentire la scrittura di dispositivo per accesso condizionale locale - [è una versione di valutazione gratuita](https://azure.microsoft.com/trial/get-started-active-directory/)  
|Sottoscrizione a Intune|richiesto solo per l'integrazione con MDM per scenari di conformità dispositivo -[è una versione di valutazione gratuita](https://portal.office.com/Signup/Signup.aspx?OfferId=40BE278A-DFD1-470a-9EF7-9F2596EA7FF9&dl=INTUNE_A&ali=1#0)
|Azure AD Connect|QFE novembre 2015 o versione successiva.  Ottenere la versione più recente [qui](https://www.microsoft.com/download/details.aspx?id=47594).  
|Windows Server 2016|Compilazione 10586 o versione successiva per ADFS  
|Schema di Active Directory di Windows Server 2016|È richiesto il livello di schema 85 o superiore.
|Controller di dominio di Windows Server 2016|Questa operazione è necessaria solo per le distribuzioni con attendibilità delle chiavi di Hello for business.  Altre informazioni sono reperibili [qui](https://aka.ms/whfbdocs).  
|Client di Windows 10|Compilare 10586 o successive, unito al dominio precedente è necessario per Windows 10 aggiunta a un dominio e Microsoft Passport lavoro solo per gli scenari  
|Account utente AD Azure con licenza Azure AD Premium|Per la registrazione del dispositivo  


 
## <a name="upgrade-your-active-directory-schema"></a>Aggiornare lo schema di Active Directory
Per usare l'accesso condizionale locale con i dispositivi registrati, è necessario prima aggiornare lo schema di AD.  Devono essere soddisfatte le condizioni seguenti:
    - Lo schema deve essere 85 o versione successiva
    - Questa operazione è necessaria solo per la foresta a cui AD FS viene aggiunto

> [!NOTE]
> Se è stato installato Azure AD Connect prima di eseguire l'aggiornamento alla versione dello schema (livello 85 o superiore) in Windows Server 2016, sarà necessario eseguire di nuovo l'installazione di Azure AD Connect e aggiornare lo schema di AD locale per assicurarsi che sia configurata la regola di sincronizzazione per msDS-KeyCredentialLink.

### <a name="verify-your-schema-level"></a>Verificare il livello di schema
Per verificare il livello di schema, eseguire le operazioni seguenti:

1.  È possibile utilizzare ADSIEdit o LDP e connettersi al contesto dei nomi di schema.  
2.  Con ADSIEdit, fare clic con il pulsante destro del mouse su "CN = schema, CN = Configuration, DC =<domain>, DC =<com> e selezionare Proprietà.  Il dominio relpace e le parti com con le informazioni sulla foresta.
3.  Nell'Editor attributi individuare l'attributo objectVersion, che indica la versione.  

![Modifica ADSI](media/Configure-Device-Based-Conditional-Access-on-Premises/adsiedit.png)  

È anche possibile usare il cmdlet di PowerShell seguente (sostituire l'oggetto con le informazioni sul contesto di denominazione dello schema):

``` powershell
Get-ADObject "cn=schema,cn=configuration,dc=domain,dc=local" -Property objectVersion
    
```

![PowerShell](media/Configure-Device-Based-Conditional-Access-on-Premises/pshell1.png) 

Per ulteriori informazioni sull'aggiornamento di, vedere [aggiornare controller di dominio a Windows Server 2016](../../ad-ds/deploy/Upgrade-Domain-Controllers-to-Windows-Server-2016.md). 

## <a name="enable-azure-ad-device-registration"></a>Abilitare la registrazione del dispositivo AD Azure  
Per configurare questo scenario, è necessario configurare la funzionalità di registrazione del dispositivo in Azure AD.  

A tale scopo, seguire i passaggi descritti nella [impostazione aggiunta ad Azure AD nell'organizzazione](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-setup/)  

## <a name="setup-ad-fs"></a>Installazione di ADFS  
1. Creazione di un [nuova farm AD FS 2016](https://technet.microsoft.com/library/dn486775.aspx).   
2.  O [migrare](../../ad-fs/deployment/Upgrading-to-AD-FS-in-Windows-Server-2016.md) una farm con AD FS 2016 da AD FS 2012 R2  
4. Distribuire [Azure AD Connect](https://azure.microsoft.com/documentation/articles/active-directory-aadconnectfed-whatis/) utilizzando il percorso personalizzato per la connessione AD FS in Azure AD.  

## <a name="configure-device-write-back-and-device-authentication"></a>Configurare nuovamente scrittura dispositivo e l'autenticazione del dispositivo  
> [!NOTE]
> Se è stato eseguito Azure AD Connect usando le impostazioni rapide, gli oggetti di Active Directory corretti sono stati creati automaticamente.  Tuttavia, nella maggior parte degli scenari di ADFS, Azure AD Connect è stato eseguito con impostazioni personalizzate per configurare ADFS, pertanto i passaggi seguenti sono necessari.  

### <a name="create-ad-objects-for-ad-fs-device-authentication"></a>Creare gli oggetti Active Directory per l'autenticazione del dispositivo AD FS  
Se la farm di ADFS non è già configurata per l'autenticazione del dispositivo (si può vedere nella console di gestione di ADFS in servizio -> registrazione del dispositivo), attenersi alla seguente procedura per creare la configurazione e gli oggetti di dominio Active Directory corretti.  

![Registrazione del dispositivo](media/Configure-Device-Based-Conditional-Access-on-Premises/device1.png)

>Nota: I comandi seguenti sono necessari strumenti di amministrazione di Active Directory, pertanto, se il server federativo non è anche un controller di dominio, innanzitutto installare gli strumenti mediante il passaggio 1 riportato di seguito.  In caso contrario è possibile ignorare il passaggio 1.  

1.  Eseguire il **Aggiungi ruoli e funzionalità** procedura guidata e selezionare funzionalità **Strumenti di amministrazione remota del Server** -> **Strumenti di amministrazione ruoli** -> **di dominio Active Directory e gli strumenti di AD LDS** -> scegliere entrambi il **modulo Active Directory per Windows PowerShell** e **Strumenti di AD DS**.

![Registrazione del dispositivo](media/Configure-Device-Based-Conditional-Access-on-Premises/device2.png)
  
2. Il server ADFS primario, assicurarsi si è connessi come utente di dominio Active Directory con privilegi di amministratore di Enterprise (EA) e aprire un prompt dei comandi di powershell con privilegi elevati.  Quindi, eseguire i comandi di PowerShell seguenti:  
    
   `Import-module activedirectory`  
   `PS C:\> Initialize-ADDeviceRegistration -ServiceAccountName "<your service account>" ` 
3. Nella finestra a comparsa premere Sì.

>Nota: Se il servizio ADFS è configurato per utilizzare un account GESTITO, immettere il nome dell'account nel formato "domain\accountname$"

![Registrazione del dispositivo](media/Configure-Device-Based-Conditional-Access-on-Premises/device3.png)  

PSH precedente crea gli oggetti seguenti:  


- Contenitore RegisteredDevices nella partizione di dominio Active Directory  
- Contenitore del servizio Registrazione dispositivi e sull'oggetto configurazione--> Servizi--> Configurazione registrazione del dispositivo  
- Contenitore DKM servizio Registrazione dispositivi e sull'oggetto configurazione--> Servizi--> Configurazione registrazione del dispositivo  

![Registrazione del dispositivo](media/Configure-Device-Based-Conditional-Access-on-Premises/device4.png)  

4. Al termine, si verrà visualizzato un messaggio di completamento.

![Registrazione del dispositivo](media/Configure-Device-Based-Conditional-Access-on-Premises/device5.png) 

###        <a name="create-service-connection-point-scp-in-ad"></a>Creare il punto di connessione del servizio (SCP) in Active Directory  
Se si prevede di utilizzare Windows 10 aggiunta al dominio (con la registrazione automatica in Azure AD) come descritto in questo caso, eseguire i comandi seguenti per creare un punto di connessione del servizio in Active Directory  
1.  Aprire Windows PowerShell ed eseguire le operazioni seguenti:
    
    `PS C:>Import-Module -Name "C:\Program Files\Microsoft Azure Active Directory Connect\AdPrep\AdSyncPrep.psm1" ` 

>Nota: se necessario, copiare il file AdSyncPrep.psm1 dal server Azure AD Connect.  Questo file si trova nel programma c:\Programmi\Microsoft Azure Active Directory Connect\AdPrep

![Registrazione del dispositivo](media/Configure-Device-Based-Conditional-Access-on-Premises/device6.png)   

2. Fornire le credenziali di amministratore globale di Azure AD  

    `PS C:>$aadAdminCred = Get-Credential`

![Registrazione del dispositivo](media/Configure-Device-Based-Conditional-Access-on-Premises/device7.png) 

3. Eseguire il seguente comando di PowerShell 

   `PS C:>Initialize-ADSyncDomainJoinedComputerSync -AdConnectorAccount [AD connector account name] -AzureADCredentials $aadAdminCred ` 

Dove il valore [Active Directory connector account name] è il nome dell'account è stato configurato in Azure AD Connect quando si aggiunge locale di servizi di dominio Active directory.
  
I comandi precedenti consentono ai client di Windows 10 trovare le impostazioni corrette dominio Azure AD a cui aggiungere creando l'oggetto serviceConnectionpoint in Active Directory.  

### <a name="prepare-ad-for-device-write-back"></a>Preparare Active Directory per la scrittura di dispositivo indietro   
Per garantire contenitori e oggetti di dominio Active Directory sono nello stato corretto per la scrittura posteriore di dispositivi da Azure AD, eseguire le operazioni seguenti.

1.  Aprire Windows PowerShell ed eseguire le operazioni seguenti:  

    `PS C:>Initialize-ADSyncDeviceWriteBack -DomainName <AD DS domain name> -AdConnectorAccount [AD connector account name] ` 

Dove il valore [Active Directory connector account name] è il nome dell'account è stato configurato in Azure AD Connect quando si aggiungono locale directory di Active Directory in formato domain\accountname  

Il comando precedente crea gli oggetti seguenti per la scrittura di dispositivo al dominio di Active Directory, se non esistono già e consente l'accesso al nome dell'account specificato Active Directory connector  

- Contenitore RegisteredDevices nella partizione di dominio Active Directory  
- Contenitore del servizio Registrazione dispositivi e sull'oggetto configurazione--> Servizi--> Configurazione registrazione del dispositivo  

### <a name="enable-device-write-back-in-azure-ad-connect"></a>Abilitare la scrittura di dispositivo in Azure AD Connect  
Se non è pertanto, prima di fatto, abilitare la scrittura di dispositivo in Azure AD Connect eseguendo la procedura guidata una seconda volta e scegliendo **"Personalizzare le opzioni di sincronizzazione"** , quindi selezionando la casella per la scrittura di dispositivo indietro e selezionare la foresta in cui è stato eseguito il cmdlet precedente  

### <a name="configure-device-authentication-in-ad-fs"></a>Configurare l'autenticazione del dispositivo in ADFS  
Utilizzando un prompt dei comandi PowerShell con privilegi elevato, configurare i criteri di ADFS eseguendo il comando seguente  

`PS C:>Set-AdfsGlobalAuthenticationPolicy -DeviceAuthenticationEnabled $true -DeviceAuthenticationMethod All` 

### <a name="check-your-configuration"></a>Controllare la configurazione  
Per riferimento, di seguito è un elenco completo dei dispositivi, contenitori e autorizzazioni necessarie per l'autenticazione e writeback del dispositivo per utilizzare Active Directory
 


- oggetto di tipo ms-DS-DeviceContainer in CN = RegisteredDevices, DC =&lt;dominio&gt;          
    - accesso in lettura all'account del servizio ADFS   
    - accesso in lettura/scrittura per l'account di Active Directory connector di sincronizzazione Azure AD Connect</br></br>

- Contenitore CN=Configurazione registrazione dispositivi,CN=Servizi,CN=Configurazione,DC=&lt;dominio&gt;  
- Contenitore dispositivo registrazione servizio distribuite sotto il contenitore precedente

![Registrazione del dispositivo](media/Configure-Device-Based-Conditional-Access-on-Premises/device8.png) 
 


- oggetto di tipo serviceConnectionpoint in CN =&lt;guid&gt;, CN = registrazione del dispositivo

- Configuration, CN=Servizi,CN=Configurazione,DC=&lt;dominio&gt;  
  - accesso in lettura/scrittura per il nome di account del connettore di Active Directory specificato per il nuovo oggetto</br></br> 


- oggetto di tipo msDS-DeviceRegistrationServiceContainer in CN = Servizi di registrazione del dispositivo, CN = dispositivo registrazione Configuration, CN = Services, CN = Configuration, DC = & ltdomain >  


- oggetto di tipo msDS-DeviceRegistrationService nel contenitore precedente  

### <a name="see-it-work"></a>Verificarne il funzionamento  
Per valutare le nuove attestazioni e i criteri, prima di registrare un dispositivo.  Ad esempio, è possibile un computer Windows 10 tramite l'applicazione delle impostazioni nel sistema -> informazioni su aggiunta ad Azure AD o aggiunta a un dominio Windows 10 è possibile configurare la registrazione automatica dei dispositivi seguendo i passaggi aggiuntivi [qui](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/).  Per informazioni sull'unione di Windows 10 i dispositivi mobili, vedere il documento [qui](https://technet.microsoft.com/itpro/windows/manage/join-windows-10-mobile-to-azure-active-directory).  

Per la valutazione più semplice, accedere ad ADFS con un'applicazione di test viene visualizzato un elenco di attestazioni. Sarà in grado di vedere nuove attestazioni, tra cui isManaged isCompliant e trusttype.  Se si abilita Microsoft Passport per il lavoro, si vedrà anche la prt di attestazione.  
 

## <a name="configure-additional-scenarios"></a>Configurare scenari aggiuntivi  
### <a name="automatic-registration-for-windows-10-domain-joined-computers"></a>Computer di registrazione per Windows 10 aggiunti a un dominio automatica  
Per abilitare la registrazione automatica dei dispositivi per Windows 10 dominio associato al computer, seguire i passaggi 1 e 2 [qui](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/).   
Ciò consentirà di ottenere quanto segue:  

1. Verificare il punto di connessione del servizio in Active Directory esista e disponga delle autorizzazioni appropriate (è stato creato questo oggetto precedente, ma non guasta controllo double).  
2. Assicurarsi di che ADFS è configurato correttamente  
3. Verificare che il sistema di ADFS dispone di endpoint corretto abilitato e configurate le regole di attestazione   
4. Configurare le impostazioni di criteri di gruppo necessarie per la registrazione automatica dei dispositivi dei computer appartenenti a un dominio   

### <a name="microsoft-passport-for-work"></a>Microsoft Passport for Work   
Per informazioni sull'attivazione di Windows 10 con Microsoft Passport for Work, vedere [abilitare Microsoft Passport for Work all'interno dell'organizzazione.](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-passport-deployment/)  

### <a name="automatic-mdm-enrollment"></a>Registrazione MDM automatica   
Per abilitare la registrazione MDM automatica dei dispositivi registrati in modo che è possibile utilizzare l'attestazione isCompliant nei criteri di controllo di accesso, attenersi alla procedura [qui.](https://blogs.technet.microsoft.com/ad/2015/08/14/windows-10-azure-ad-and-microsoft-intune-automatic-mdm-enrollment-powered-by-the-cloud/)  

## <a name="troubleshooting"></a>Risoluzione dei problemi  
1.  Se si verifica un errore `Initialize-ADDeviceRegistration` che visualizza un errore su un oggetto già esistente nello stato errato, ad esempio "l'oggetto servizio drs trovare senza tutti gli attributi obbligatori", è possibile che sia eseguita Azure AD Connect powershell comandi in precedenza e dispone di una configurazione parziale in Active Directory.  Provare a eliminare manualmente gli oggetti sotto **CN = dispositivo registrazione Configuration, CN = Services, CN = Configuration, DC =&lt;dominio&gt;** e riprovare.  
2.  Per Windows 10 dominio aggiunto client  
    1. Per verificare che l'autenticazione del dispositivo è funzionante, accedere al dominio associato al client come account utente di test. Per attivare rapidamente il provisioning, bloccare e sbloccare il desktop almeno una volta.   
    2. Istruzioni per verificare la presenza di credenziali chiave stk collegano nell'oggetto di dominio Active Directory (sincronizzazione ancora necessario eseguire due volte?)  
3.  Se viene visualizzato un errore durante il tentativo di registrare un computer Windows che il dispositivo è stato già registrato, ma non sono in grado o hanno già annullata registrazione del dispositivo, potrebbe essere un frammento di configurazione della registrazione dispositivo nel Registro di sistema.  Per ricercare e rimuovere questo, attenersi alla procedura seguente:  
    1. Nel computer Windows, aprire Regedit e passare alla **HKLM\Software\Microsoft\Enrollments**   
    2. In questa chiave verrà sottochiavi molti nel formato GUID.  Passare alla sottochiave che contiene i valori ~ 17 e ha "EnrollmentType" di "6" [MDM unita in join] o "13" (Azure AD unita in join)  
    3. Modificare **EnrollmentType** per **0** 
    4. Provare a eseguire nuovamente la registrazione del dispositivo o registrazione  

### <a name="related-articles"></a>Articoli correlati  
* [Protezione dell'accesso a Office 365 e ad altre app connesse a Azure Active Directory](https://azure.microsoft.com/documentation/articles/active-directory-conditional-access/)  
* [Criteri di accesso condizionale dei dispositivi per i servizi di Office 365](https://azure.microsoft.com/documentation/articles/active-directory-conditional-access-device-policies/)  
* [Configurazione dell'accesso condizionale locale usando Registrazione dispositivo Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-device-registration-on-premises-setup)  
* [Connetti i dispositivi aggiunti a un dominio al Azure AD per le esperienze di Windows 10](https://azure.microsoft.com/documentation/articles/active-directory-azureadjoin-devices-group-policy/)  
