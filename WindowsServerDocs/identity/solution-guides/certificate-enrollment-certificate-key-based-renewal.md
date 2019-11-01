---
title: Configurazione di Servizio Web di registrazione certificati per il rinnovo basato su chiave del certificato su una porta personalizzata
description: ''
author: Deland-Han
ms.author: delhan
manager: dcscontentpm
ms.date: 11/1/2019
ms.topic: article
ms.prod: windows-server
ms.openlocfilehash: a5fea6681376abf6ea9ada67e31b10369256ea81
ms.sourcegitcommit: 9e123d475f3755218793a130dda88455eac9d4ab
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/01/2019
ms.locfileid: "73413458"
---
# <a name="configuring-certificate-enrollment-web-service-for-certificate-key-based-renewal-on-a-custom-port"></a>Configurazione di Servizio Web di registrazione certificati per il rinnovo basato su chiave del certificato su una porta personalizzata

<!-- <Author(s): Jitesh Thakur, Meera Mohideen, Ankit Tyagi.-->  

## <a name="summary"></a>Riepilogo

In questo articolo vengono fornite istruzioni dettagliate per implementare il Servizio Web di registrazione certificati (o il servizio di registrazione certificati (CEP) o il servizio di registrazione certificati (CES)) su una porta personalizzata diversa da 443 per il rinnovo basato su chiave del certificato da eseguire vantaggi della funzionalità di rinnovo automatico di CEP e CES.

Questo articolo illustra anche il funzionamento di CEP e CES e fornisce linee guida per la configurazione.

> [!Note]
> Il flusso di lavoro incluso in questo articolo si applica a uno scenario specifico. Lo stesso flusso di lavoro potrebbe non funzionare per una situazione diversa. Tuttavia, i principi rimangono invariati.
>
> Dichiarazione di non responsabilità: questa installazione viene creata per un requisito specifico in cui non si vuole usare la porta 443 per la comunicazione HTTPS predefinita per i server CEP e CES. Sebbene sia possibile, questa configurazione ha un supporto limitato. I servizi e il supporto tecnico possono essere utili se si segue con attenzione questa guida utilizzando la deviazione minima dalla configurazione del server Web specificata.

## <a name="scenario"></a>Scenario

Per questo esempio, le istruzioni sono basate su un ambiente che usa la configurazione seguente:

- Foresta Contoso.com che dispone di un'infrastruttura A chiave pubblica (PKI) di Servizi certificati Active Directory.

- Due istanze CEP/CES configurate in un server in esecuzione con un account del servizio. Per la registrazione iniziale, un'istanza usa nome utente e password. L'altro usa l'autenticazione basata su certificati per il rinnovo basato su chiavi in modalità solo rinnovo.

- Un utente dispone di un gruppo di lavoro o di un computer non appartenente a un dominio per il quale registrerà il certificato del computer utilizzando le credenziali nome utente e password.

- La connessione dall'utente a CEP e CES su HTTPS viene eseguita su una porta personalizzata, ad esempio 49999. Questa porta è selezionata da un intervallo di porte dinamiche e non viene utilizzata come porta statica da altri servizi.

- Quando la durata del certificato si avvicina alla sua fine, il computer usa il rinnovo basato su chiavi CES basato su certificati per rinnovare il certificato sullo stesso canale.

![distribuzione](media/certificate-enrollment-certificate-key-based-renewal-1.png)

## <a name="configuration-instructions"></a>Istruzioni di configurazione

### <a name="overview"></a>Panoramica 

1. Configurare il modello per il rinnovo basato su chiave.

2. Come prerequisito, configurare un server CEP e CES per l'autenticazione con nome utente e password.   
   In questo ambiente viene fatto riferimento all'istanza di come "CEPCES01".

3.  Configurare un'altra istanza CEP e CES usando PowerShell per l'autenticazione basata su certificati nello stesso server. L'istanza di CES utilizzerà un account del servizio.

    In questo ambiente viene fatto riferimento all'istanza di come "CEPCES02". L'account di servizio usato è "cepcessvc".

4.  Configurare le impostazioni lato client.

### <a name="configuration"></a>Configurazione

Questa sezione illustra la procedura per configurare la registrazione iniziale.

> [!Note]
> È inoltre possibile configurare qualsiasi account del servizio utente, MSA o GMSA per il funzionamento di CES.

Come prerequisito, è necessario configurare CEP e CES in un server usando l'autenticazione con nome utente e password.

#### <a name="configure-the-template-for-key-based-renewal"></a>Configurare il modello per il rinnovo basato su chiave

È possibile duplicare un modello di computer esistente e configurare le impostazioni seguenti del modello:

1. Nella scheda nome soggetto del modello di certificato, verificare che siano selezionate le opzioni specificare le informazioni di **richiesta** e **utilizza le informazioni del soggetto da certificati esistenti per le richieste di rinnovo della registrazione automatica** .
   ![nuovi modelli](media/certificate-enrollment-certificate-key-based-renewal-2.png) 

2. Passare alla scheda **requisiti di rilascio** , quindi selezionare la casella di controllo **Approvazione gestore certificati CA** .
   ![requisiti di rilascio](media/certificate-enrollment-certificate-key-based-renewal-3.png) 

3. Assegnare le autorizzazioni **lettura** e **registrazione** per l'account del servizio **cepcessvc** per questo modello.

4. Pubblicare il nuovo modello nell'autorità di certificazione.

> [!Note]
> Verificare che le impostazioni di compatibilità nel modello siano impostate su **Windows server 2012 R2** poiché esiste un problema noto in cui i modelli non sono visibili se la compatibilità è impostata su windows server 2016 o versione successiva. Per altre informaiton, vedere [non è possibile selezionare i modelli di certificato compatibili con la CA di Windows server 2016 da Windows server 2016 o da CA basate su server CEP o versioni successive ](https://support.microsoft.com/en-in/help/4508802/cannot-select-certificate-templates-in-windows-server-2016).


#### <a name="configure-the-cepces01-instance"></a>Configurare l'istanza di CEPCES01

##### <a name="step-1-install-the-instance"></a>Passaggio 1: installare l'istanza

Per installare l'istanza di CEPCES01, usare uno dei metodi seguenti.

**Metodo 1**

Per istruzioni dettagliate su come abilitare CEP e CES per l'autenticazione di nome utente e password, vedere gli articoli seguenti:

[Guida Servizio Web di informazioni sulle registrazioni di certificati](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831625(v=ws.11))

[Guida Servizio Web di registrazione certificati](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/hh831822(v=ws.11)#configure-a-ca-for-the-certificate-enrollment-web-service)

> [!Note]
> Assicurarsi di non selezionare l'opzione "Abilita rinnovo basato su chiavi" Se si configurano le istanze CEP e CES dell'autenticazione con nome utente e password.

**Metodo 2**

Per installare le istanze CEP e CES, è possibile usare i cmdlet di PowerShell seguenti:

```PowerShell
Import-Module ServerManager
Add-WindowsFeature Adcs-Enroll-Web-Pol
Add-WindowsFeature Adcs-Enroll-Web-Svc
```

```PowerShell
Install-AdcsEnrollmentPolicyWebService -AuthenticationType Username -SSLCertThumbprint "sslCertThumbPrint"
```

Questo comando installa il Servizio Web di informazioni sulle registrazioni di certificati (CEP) specificando che per l'autenticazione vengono usati un nome utente e una password. 

> [!Note]
> In questo comando \<**SSLCertThumbPrint**\> è l'identificazione personale del certificato che verrà utilizzato per associare IIS.

```PowerShell
Install-AdcsEnrollmentWebService -ApplicationPoolIdentity -CAConfig "CA1.contoso.com\contoso-CA1-CA" -SSLCertThumbprint "sslCertThumbPrint" -AuthenticationType Username
```

Questo comando installa il Servizio Web di registrazione certificati (CES) per usare l'autorità di certificazione per il nome computer **CA1.contoso.com** e il nome comune della CA **Contoso-CA1-CA**. L'identità del CES viene specificata come identità del pool di applicazioni predefinita. Il tipo di autenticazione è **username**. SSLCertThumbPrint è l'identificazione personale del certificato che verrà utilizzato per associare IIS.

##### <a name="step-2-check-the-internet-information-services-iis-manager-console"></a>Passaggio 2 controllare la console di gestione Internet Information Services (IIS)

Al termine dell'installazione, nella console di gestione Internet Information Services (IIS) dovrebbe essere visualizzata la seguente visualizzazione.
![Gestione IIS](media/certificate-enrollment-certificate-key-based-renewal-4.png) 

In **sito Web predefinito**selezionare **KeyBasedRenewal_ADPolicyProvider_CEP_Certificate**e quindi aprire **Impostazioni applicazione**. Prendere nota dell' **ID** e dell' **URI**.

È possibile aggiungere un **nome descrittivo** per la gestione.

#### <a name="configure-the-cepces02-instance"></a>Configurare l'istanza di CEPCES02

##### <a name="step-1-install-the-cep-and-ces-for-key-based-renewal-on-the-same-server"></a>Passaggio 1: installare CEP e CES per il rinnovo basato su chiavi nello stesso server. 

Eseguire il comando seguente in PowerShell:

```PowerShell
Install-AdcsEnrollmentPolicyWebService -AuthenticationType Certificate -SSLCertThumbprint "sslCertThumbPrint" -KeyBasedRenewal
```

Tramite questo comando viene installato il Servizio Web di informazioni sulle registrazioni di certificati (CEP) e viene specificato che un certificato viene utilizzato per l'autenticazione. 

> [!Note]
> In questo comando \<SSLCertThumbPrint\> è l'identificazione personale del certificato che verrà utilizzato per associare IIS. 

Il rinnovo basato su chiavi consente ai client di certificati di rinnovare i propri certificati usando la chiave del certificato esistente per l'autenticazione. Quando si usa la modalità di rinnovo basata su chiavi, il servizio restituirà solo i modelli di certificato impostati per il rinnovo basato su chiave.

```PowerShell
Install-AdcsEnrollmentWebService -CAConfig "CA1.contoso.com\contoso-CA1-CA" -SSLCertThumbprint "sslCertThumbPrint" -AuthenticationType Certificate -ServiceAccountName "Contoso\cepcessvc" -ServiceAccountPassword (read-host "Set user password" -assecurestring) -RenewalOnly -AllowKeyBasedRenewal
```

Questo comando installa il Servizio Web di registrazione certificati (CES) per usare l'autorità di certificazione per il nome computer **CA1.contoso.com** e il nome comune della CA **Contoso-CA1-CA**. 

In questo comando, l'identità del Servizio Web di registrazione certificati viene specificata come account del servizio **cepcessvc** . Il tipo di autenticazione è **Certificate**. **SSLCertThumbPrint** è l'identificazione personale del certificato che verrà utilizzato per associare IIS.

Il cmdlet **RenewalOnly** consente l'esecuzione di CES in modalità solo rinnovo. Il cmdlet **AllowKeyBasedRenewal** specifica inoltre che il CES accetterà richieste di rinnovo basate su chiave per il server di registrazione. Si tratta di certificati client validi per l'autenticazione che non eseguono direttamente il mapping a un'entità di sicurezza.

> [!Note]
> È necessario che l'account del servizio faccia parte del gruppo **IISUsers** nel server.

##### <a name="step-2-check-the-iis-manager-console"></a>Passaggio 2 controllare la console di gestione IIS

Una volta completata l'installazione, nella console Gestione IIS si prevede di visualizzare la seguente visualizzazione.
![Gestione IIS](media/certificate-enrollment-certificate-key-based-renewal-5.png) 

Selezionare **KeyBasedRenewal_ADPolicyProvider_CEP_Certificate** in **sito Web predefinito** e aprire **Impostazioni applicazione**. Prendere nota dell' **ID** e dell' **URI**. È possibile aggiungere un **nome descrittivo** per la gestione.

> [!Note]
> Se l'istanza di è installata in un nuovo server e l'ID è diverso e Incase l'ID è diverso, assicurarsi che l'ID corrisponda a quello usato nell'istanza di CEPCES01. È possibile copiare e incollare direttamente il valore.

#### <a name="complete-certificate-enrollment-web-services-configuration"></a>Completa la configurazione dei servizi Web di registrazione certificati

Per poter registrare il certificato per conto della funzionalità di CEP e CES, è necessario configurare l'account computer del gruppo di lavoro in Active Directory e quindi configurare la delega vincolata nell'account del servizio.

##### <a name="step-1-create-a-computer-account-of-the-workgroup-computer-in-active-directory"></a>Passaggio 1: creare un account computer del computer del gruppo di lavoro in Active Directory

Questo account verrà usato per l'autenticazione per il rinnovo basato su chiavi e l'opzione "pubblica per Active Directory" nel modello di certificato.

![Nuovo oggetto](media/certificate-enrollment-certificate-key-based-renewal-6.png) 
 
##### <a name="step-2-configure-the-service-account-for-constrained-delegation-s4u2self"></a>Passaggio 2: configurare l'account del servizio per la delega vincolata (S4U2Self)

Eseguire il comando di PowerShell seguente per abilitare la delega vincolata (S4U2Self o qualsiasi protocollo di autenticazione):

```PowerShell
Get-ADUser -Identity cepcessvc | Set-ADAccountControl -TrustedToAuthForDelegation $True
Set-ADUser -Identity cepcessvc -Add @{'msDS-AllowedToDelegateTo'=@('HOST/CA1.contoso.com','RPCSS/CA1.contoso.com')}
```

> [!Note]
> In questo comando <cepcessvc> è l'account del servizio e < CA1. contoso. com > è l'autorità di certificazione.

> [!Important]
> Non è in corso l'abilitazione del flag RENEWALONBEHALOF sulla CA in questa configurazione perché si sta usando la delega vincolata per eseguire lo stesso processo. In questo modo è possibile evitare di aggiungere l'autorizzazione per l'account del servizio alla sicurezza della CA.

##### <a name="step-3-configure-a-custom-port-on-the-iis-web-server"></a>Passaggio 3: configurare una porta personalizzata sul server Web IIS

1. Nella console Gestione IIS, selezionare sito Web predefinito.

2. Nel riquadro azioni selezionare Modifica binding sito. 

3. Modificare l'impostazione della porta predefinita da 443 alla porta personalizzata. Nella schermata di esempio viene illustrata l'impostazione della porta 49999.
   ![modificare la porta](media/certificate-enrollment-certificate-key-based-renewal-7.png) 

##### <a name="step-4-edit-the-ca-enrollment-services-object"></a>Passaggio 4: modificare l'oggetto servizi di registrazione CA

1. In un controller di dominio aprire ADSIEdit. msc.

2. Connettersi alla partizione di configurazione e passare all'oggetto servizi di registrazione CA:
   
   CN = CAORG, CN = Servizi di registrazione, CN = Servizi chiave pubblica, CN = Services, CN = Configuration, DC = contoso, DC = com

3. Modificare l'oggetto e modificare l'attributo msPKI-iscrizione-Servers usando la porta personalizzata con gli URI del server CEP e CES trovati nelle impostazioni dell'applicazione. Ad esempio:

   140 https://cepces.contoso.com:49999/ENTCA_CES_UsernamePassword/service.svc/CES0   
   181 https://cepces.contoso.com:49999/ENTCA_CES_Certificate/service.svc/CES1

   ![Modifica ADSI](media/certificate-enrollment-certificate-key-based-renewal-8.png) 

#### <a name="configure-the-client-computer"></a>Configurare il computer client

Nel computer client configurare i criteri di registrazione e i criteri di registrazione automatica. A tale scopo, attieniti alla seguente procedura:

1. Selezionare **avvia** > **Esegui**, quindi immettere **gpedit. msc**.

2. Passare a **Configurazione Computer** > **impostazioni di Windows** > **impostazioni di sicurezza**e quindi fare clic su **Criteri chiave pubblica**.

3. Abilitare i **criteri di registrazione automatica del client di Servizi certificati** in modo che corrispondano alle impostazioni nello screenshot seguente.
   ![](media/certificate-enrollment-certificate-key-based-renewal-9.png) criteri di gruppo di certificati
 
4. Abilitare i **criteri di registrazione certificati client di Servizi certificati**.

   a. Fare clic su **Aggiungi** per aggiungere i criteri di registrazione e immettere l'URI CEP con **USERNAMEPASSWORD** modificato in ADSI.
   
   b. In **tipo di autenticazione**selezionare **nome utente/password**.
   
   c. Impostare una priorità pari a **10**, quindi convalidare il server dei criteri.
      ![](media/certificate-enrollment-certificate-key-based-renewal-10.png) dei criteri di registrazione

   > [!Note]
   > Verificare che il numero di porta venga aggiunto all'URI ed è consentito sul firewall.

5. Registrare il primo certificato per il computer tramite certlm. msc.
   ![](media/certificate-enrollment-certificate-key-based-renewal-11.png) dei criteri di registrazione

   Selezionare il modello KBR e registrare il certificato.
   ![](media/certificate-enrollment-certificate-key-based-renewal-12.png) dei criteri di registrazione

6. Aprire nuovamente **gpedit. msc** . Modificare il **client di Servizi certificati-criterio di registrazione certificati**e quindi aggiungere i criteri di registrazione del rinnovo basato su chiavi:

   a. Fare clic su **Aggiungi**, immettere l'URI CEP con il **certificato** modificato in ADSI. 
   
   b. Impostare una priorità pari a **1**, quindi convalidare il server dei criteri. Verrà richiesto di eseguire l'autenticazione e di scegliere il certificato inizialmente registrato.

   ![Criteri di registrazione](media/certificate-enrollment-certificate-key-based-renewal-13.png) 

> [!Note]
> Verificare che il valore di priorità del criterio di registrazione del rinnovo basato su chiave sia inferiore alla priorità della priorità dei criteri di registrazione della password del nome utente. La prima preferenza viene assegnata alla priorità più bassa.

## <a name="testing-the-setup"></a>Test dell'installazione

Per assicurarsi che il rinnovo automatico sia funzionante, verificare che il rinnovo manuale funzioni utilizzando la stessa chiave. Non verrà richiesto di immettere il nome utente e la password.  Viene inoltre richiesto di selezionare un certificato "quando si esegue la registrazione. Si tratta di un comportamento previsto.

Aprire l'archivio certificati personal computer e aggiungere la visualizzazione "certificati archiviati". A tale scopo, usare uno dei metodi seguenti.

### <a name="method-1"></a>Metodo 1 

Eseguire il comando seguente:

```PowerShell
certreq -machine -q -enroll -cert <thumbprint> renew
```

![.](media/certificate-enrollment-certificate-key-based-renewal-14.png)

### <a name="method-2"></a>Method 2

Anticipare l'impostazione dell'ora in multipli di 8 per la scadenza della validità del certificato.

Ad esempio, il certificato di esempio è stato emesso alle 4:00 del 18 ° giorno del mese, scade alle 4:00 del ventesimo e ha un'impostazione di validità di 2 giorni e un'impostazione di rinnovo di 8 ore. Il motore di registrazione automatica viene attivato al riavvio a un intervallo di circa 8 ore.

Quindi, se si avanza il tempo alle 8:10 nel 19, eseguendo **certutil-Pulse** (per attivare il motore AE), il certificato viene registrato per l'utente.

![.](media/certificate-enrollment-certificate-key-based-renewal-15.png)
 
Al termine del test, ripristinare l'impostazione dell'ora sul valore originale, quindi riavviare il computer client.

> [!Note]
> Lo screenshot precedente è un esempio per dimostrare che il motore di registrazione automatica funziona come previsto perché la data della CA è ancora impostata sul 18 °. Continua quindi a emettere certificati. In una situazione reale, questa grande quantità di rinnovi non verrà eseguita.

## <a name="references"></a>Riferimenti

[Guida al Lab di test: dimostrazione del rinnovo basato su chiave del certificato](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/jj590165(v%3Dws.11))

[Servizi Web di registrazione certificati](https://techcommunity.microsoft.com/t5/Ask-the-Directory-Services-Team/Certificate-Enrollment-Web-Services/ba-p/397385)

[Install-AdcsEnrollmentPolicyWebService](https://docs.microsoft.com/powershell/module/adcsdeployment/install-adcsenrollmentpolicywebservice?view=win10-ps)

[Install-AdcsEnrollmentWebService](https://docs.microsoft.com/powershell/module/adcsdeployment/install-adcsenrollmentwebservice?view=win10-ps)

Vedi anche

[forum sulla sicurezza di Windows Server](https://aka.ms/adcsforum)

[Domande frequenti su infrastruttura a chiave pubblica (PKI) di Servizi certificati Active Directory](https://aka.ms/adcsfaq)

[raccolta e documentazione di riferimento di Infrastruttura a chiave pubblica (PKI) di Windows](https://social.technet.microsoft.com/wiki/contents/articles/987.windows-pki-documentation-reference-and-library.aspx)

[blog su Infrastruttura a chiave pubblica (PKI) di Windows](https://blogs.technet.com/b/pki/)

[Come configurare la delega vincolata Kerberos (solo S4U2Proxy o Kerberos) in un account del servizio personalizzato per le pagine proxy di registrazione Web](https://support.microsoft.com/help/4494313/configuring-web-enrollment-proxy-for-s4u2proxy-constrained-delegation)