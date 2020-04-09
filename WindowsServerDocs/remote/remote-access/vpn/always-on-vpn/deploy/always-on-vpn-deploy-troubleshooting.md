---
title: Risolvere i problemi di VPN Always On
description: Questo argomento fornisce istruzioni per la verifica e la risoluzione dei problemi relativi alla distribuzione Always On VPN in Windows Server 2016.
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 4d08164e-3cc8-44e5-a319-9671e1ac294a
ms.localizationpriority: medium
ms.date: 06/11/2018
ms.author: v-tea
author: Teresa-MOTIV
ms.openlocfilehash: 209567ccd88f4b20f98caecc2a13cc671ef09072
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80854024"
---
# <a name="troubleshoot-always-on-vpn"></a>Risolvere i problemi di VPN Always On 

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

Se la configurazione della VPN Always On non riesce a connettere i client alla rete interna, la causa è probabilmente un certificato VPN non valido, criteri NPS non corretti o problemi con gli script di distribuzione client o in routing e accesso remoto. Il primo passaggio per la risoluzione dei problemi e il test della connessione VPN consiste nel comprendere i componenti principali dell'infrastruttura VPN Always On. 

È possibile risolvere i problemi di connessione in diversi modi. Per i problemi lato client e la risoluzione dei problemi generali, i registri applicazioni nei computer client sono inestimabili. Per i problemi specifici dell'autenticazione, il log NPS nel server NPS può aiutare a determinare l'origine del problema.

## <a name="error-codes"></a>Codici di errore

### <a name="error-code-800"></a>Codice di errore: 800

- **Descrizione dell'errore.** La connessione remota non è stata eseguita perché i tunnel VPN tentati non sono riusciti. Il server VPN potrebbe non essere raggiungibile. Se questa connessione tenta di utilizzare un tunnel L2TP/IPsec, i parametri di sicurezza necessari per la negoziazione IPsec potrebbero non essere configurati correttamente.

- **Possibile cause.** Questo errore si verifica quando il tipo di tunnel VPN è **automatico** e il tentativo di connessione non riesce per tutti i tunnel VPN.

- **Possibili soluzioni:**

    - Se si conosce il tunnel da usare per la distribuzione, impostare il tipo di VPN su quel particolare tipo di tunnel sul lato client VPN.

    - Se si effettua una connessione VPN con un determinato tipo di tunnel, la connessione avrà esito negativo, ma si verificherà un errore più specifico del tunnel (ad esempio, "GRE bloccato per PPTP").

    - Questo errore si verifica anche quando non è possibile raggiungere il server VPN o la connessione del tunnel non riesce.

- **Assicurarsi:**

    - Le porte IKE (porte UDP 500 e 4500) non sono bloccate.

    - I certificati corretti per IKE sono presenti sia nel client che nel server.

### <a name="error-code-809"></a>Codice di errore: 809

- **Descrizione dell'errore.**  Non è stato possibile stabilire la connessione di rete tra il computer e il server VPN perché il server remoto non risponde. Il problema potrebbe essere dovuto al fatto che uno dei dispositivi di rete (ad esempio, firewall, NAT, router) tra il computer e il server remoto non è configurato per consentire le connessioni VPN. Contattare l'amministratore o il provider di servizi per determinare il dispositivo che potrebbe causare il problema.

- **Possibile cause.** Questo errore è causato da porte UDP 500 o 4500 bloccate nel server VPN o nel firewall.

- **Possibile soluzione.** Verificare che le porte UDP 500 e 4500 siano consentite attraverso tutti i firewall tra il client e il server RRAS.

### <a name="error-code-812"></a>Codice di errore: 812

- **Descrizione dell'errore.** Non è possibile connettersi alla VPN Always On. La connessione è stata impedita a causa di un criterio configurato sul server RAS/VPN. In particolare, il metodo di autenticazione utilizzato dal server per verificare il nome utente e la password potrebbe non corrispondere al metodo di autenticazione configurato nel profilo di connessione. Contattare l'amministratore del server RAS e notificare l'errore.

- **Possibili cause:**

    - La tipica cause di questo errore è che il server dei criteri di server ha specificato una condizione di autenticazione che il client non è in grado di soddisfare. Ad esempio, il server dei criteri di server può specificare l'uso di un certificato per proteggere la connessione PEAP, ma il client sta provando a usare EAP-MSCHAPv2.

    - Il registro eventi 20276 viene registrato nel Visualizzatore eventi quando l'impostazione del protocollo di autenticazione server VPN basato su RRAS non corrisponde a quella del computer client VPN.

- **Possibile soluzione.** Verificare che la configurazione client corrisponda alle condizioni specificate nel server NPS.

### <a name="error-code-13806"></a>Codice di errore: 13806

- **Descrizione dell'errore.** IKE non è riuscito a trovare un certificato macchina valido. Contattare l'amministratore della sicurezza di rete per informazioni sull'installazione di un certificato valido nell'archivio certificati appropriato.

- **Possibile cause.** Questo errore si verifica in genere quando nel server VPN non è presente alcun certificato computer o certificato del computer radice.

- **Possibile soluzione.** Verificare che i certificati descritti in questa distribuzione siano installati sia nel computer client che nel server VPN.

### <a name="error-code-13801"></a>Codice di errore: 13801

- **Descrizione dell'errore.** Le credenziali di autenticazione IKE sono inaccettabili.

- **Possibili cause.** Questo errore si verifica in genere in uno dei casi seguenti:

    - Il certificato del computer utilizzato per la convalida del IKEv2 nel server RAS non dispone dell' **autenticazione server** con **utilizzo chiavi avanzato**.

    - Il certificato del computer nel server RAS è scaduto.

    - Il certificato radice per la convalida del certificato del server RAS non è presente nel computer client.

    - Il nome del server VPN utilizzato nel computer client non corrisponde al **soggettore** del certificato del server.

- **Possibile soluzione.** Verificare che il certificato del server includa **autenticazione server** in **utilizzo chiavi avanzato**. Verificare che il certificato del server sia ancora valido. Verificare che la CA utilizzata sia elencata in **autorità di certificazione radice attendibili** nel server RRAS. Verificare che il client VPN si connetta usando il nome di dominio completo (FQDN) del server VPN, come illustrato nel certificato del server VPN.

### <a name="error-code-0x80070040"></a>Codice di errore: 0x80070040

- **Descrizione dell'errore.** Il certificato del server non dispone di **autenticazione server** come una delle voci di utilizzo del certificato.

- **Possibile cause.** Questo errore può verificarsi se nel server RAS non è installato alcun certificato di autenticazione server.

- **Possibile soluzione.** Verificare che il certificato del computer usato dal server RAS per **IKEv2** disponga dell' **autenticazione server** come una delle voci di utilizzo del certificato.

### <a name="error-code-0x800b0109"></a>Codice di errore: 0x800B0109

Il computer client VPN viene in genere aggiunto al dominio basato su Active Directory. Se si utilizzano le credenziali di dominio per accedere al server VPN, il certificato viene installato automaticamente nell'archivio Autorità di certificazione radice attendibili. Tuttavia, se il computer non è stato aggiunto al dominio o se si usa una catena di certificati alternativa, è possibile che si verifichi questo problema.

- **Descrizione dell'errore.** Una catena di certificati è stata elaborata ma terminata in un certificato radice non attendibile dal provider di attendibilità.

- **Possibile cause.** Questo errore può verificarsi se il certificato CA radice attendibile appropriato non è installato nell'archivio Autorità di certificazione radice attendibili nel computer client.

- **Possibile soluzione.** Verificare che il certificato radice sia installato nel computer client nell'archivio Autorità di certificazione radice attendibili.

## <a name="logs"></a>Registri

### <a name="application-logs"></a>Log applicazioni

I registri applicazioni nei computer client registrano la maggior parte dei dettagli di livello superiore degli eventi di connessione VPN.

Cerca gli eventi dall'origine RasClient. Tutti i messaggi di errore restituiscono il codice di errore alla fine del messaggio. Di seguito sono riportati alcuni dei codici di errore più comuni, ma è disponibile un elenco completo dei [codici di errore di routing e accesso remoto](https://msdn.microsoft.com/library/windows/desktop/bb530704.aspx).

## <a name="nps-logs"></a>Log NPS

NPS crea e archivia i log di contabilità NPS. Per impostazione predefinita, questi file vengono archiviati in% SYSTEMROOT%\\system32\\LogFiles\\ in un file denominato IN*xxxx*. txt, dove *xxxx* è la data di creazione del file.

Per impostazione predefinita, questi log sono in formato con valori delimitati da virgole, ma non includono una riga di intestazione. La riga di intestazione è:

```
ComputerName,ServiceName,Record-Date,Record-Time,Packet-Type,User-Name,Fully-Qualified-Distinguished-Name,Called-Station-ID,Calling-Station-ID,Callback-Number,Framed-IP-Address,NAS-Identifier,NAS-IP-Address,NAS-Port,Client-Vendor,Client-IP-Address,Client-Friendly-Name,Event-Timestamp,Port-Limit,NAS-Port-Type,Connect-Info,Framed-Protocol,Service-Type,Authentication-Type,Policy-Name,Reason-Code,Class,Session-Timeout,Idle-Timeout,Termination-Action,EAP-Friendly-Name,Acct-Status-Type,Acct-Delay-Time,Acct-Input-Octets,Acct-Output-Octets,Acct-Session-Id,Acct-Authentic,Acct-Session-Time,Acct-Input-Packets,Acct-Output-Packets,Acct-Terminate-Cause,Acct-Multi-Ssn-ID,Acct-Link-Count,Acct-Interim-Interval,Tunnel-Type,Tunnel-Medium-Type,Tunnel-Client-Endpt,Tunnel-Server-Endpt,Acct-Tunnel-Conn,Tunnel-Pvt-Group-ID,Tunnel-Assignment-ID,Tunnel-Preference,MS-Acct-Auth-Type,MS-Acct-EAP-Type,MS-RAS-Version,MS-RAS-Vendor,MS-CHAP-Error,MS-CHAP-Domain,MS-MPPE-Encryption-Types,MS-MPPE-Encryption-Policy,Proxy-Policy-Name,Provider-Type,Provider-Name,Remote-Server-Address,MS-RAS-Client-Name,MS-RAS-Client-Version
```

Se si incolla questa riga di intestazione come prima riga del file di log, quindi si importa il file in Microsoft Excel, le colonne saranno etichettate correttamente.

I log NPS possono essere utili per la diagnosi dei problemi relativi ai criteri. Per ulteriori informazioni sui log NPS, vedere [interpretare i file di log del formato del database NPS](https://technet.microsoft.com/library/cc771748.aspx).

## <a name="vpn_profileps1-script-issues"></a>Problemi di script VPN_Profile. ps1

I problemi più comuni quando si esegue manualmente lo script VPN_ profile. ps1 includono:

- Si usa uno strumento di connessione remota?  Assicurarsi di non usare il protocollo RDP o un altro metodo di connessione remota perché si verificano problemi con il rilevamento degli accessi degli utenti.

- L'utente è un amministratore di tale computer locale?  Assicurarsi che durante l'esecuzione dello script VPN_Profile. ps1 che l'utente disponga dei privilegi di amministratore.

- Sono abilitate altre funzionalità di sicurezza di PowerShell? Assicurarsi che i criteri di esecuzione di PowerShell non blocchino lo script. Prima di eseguire lo script, è consigliabile disattivare la modalità del linguaggio vincolato, se abilitata. È possibile attivare la modalità del linguaggio vincolato dopo che lo script è stato completato correttamente.

## <a name="always-on-vpn-client-connection-issues"></a>Problemi di connessione del client VPN Always On

Una piccola configurazione errata può causare un errore di connessione del client e può essere difficile individuare la ragione.  Una Always On client VPN esegue diversi passaggi prima di stabilire una connessione. Quando si risolvono i problemi di connessione del client, eseguire il processo di eliminazione con quanto segue:

1. Il computer modello è connesso esternamente? Un'analisi **whatismyip** dovrebbe visualizzare un indirizzo IP pubblico che non appartiene all'utente.

2. È possibile risolvere il nome del server VPN o di accesso remoto in un indirizzo IP? Nel **Pannello di controllo** > **rete** e **Internet** > **connessioni di rete**, aprire le proprietà del profilo VPN. Il valore nella scheda **generale** dovrebbe essere risolvibile pubblicamente tramite DNS.

3. È possibile accedere al server VPN da una rete esterna? Prendere in considerazione l'apertura di Internet Control Message Protocol (ICMP) all'interfaccia esterna e il ping del nome dal client remoto. Quando un ping ha esito positivo, è possibile rimuovere la regola Consenti ICMP.

4. Sono state configurate correttamente le schede di rete interne ed esterne nel server VPN? Si trovano in subnet diverse? La scheda di interfaccia di rete esterna si connette all'interfaccia corretta nel firewall?

5. Le porte UDP 500 e 4500 vengono aperte dal client all'interfaccia esterna del server VPN? Controllare il firewall del client, il firewall del server e gli eventuali firewall hardware. IPSEC utilizza la porta UDP 500, quindi assicurarsi che IPSec non sia disabilitato o bloccato in un punto qualsiasi.

6. La convalida del certificato non riesce? Verificare che il server NPS disponga di un certificato di autenticazione server in grado di soddisfare le richieste IKE. Assicurarsi di disporre dell'indirizzo IP del server VPN corretto specificato come client NPS. Assicurarsi di eseguire l'autenticazione con PEAP e che le proprietà EAP protette debbano consentire solo l'autenticazione con un certificato. È possibile controllare i registri eventi NPS per gli errori di autenticazione. Per ulteriori informazioni, vedere [installare e configurare il server NPS](vpn-deploy-nps.md)

7. Ci si connette ma non si ha accesso alla rete Internet/locale? Controllare i pool di indirizzi IP del server DHCP/VPN per individuare i problemi di configurazione.

8. Ci si connette e si ha un indirizzo IP interno valido ma non si ha accesso alle risorse locali?  Verificare che i client sappiano come ottenere tali risorse. È possibile usare il server VPN per instradare le richieste.

## <a name="azure-ad-conditional-access-connection-issues"></a>Problemi di connessione dell'accesso condizionale Azure AD

### <a name="oops---you-cant-get-to-this-yet"></a>Non è ancora possibile trovare questo

- **Descrizione dell'errore.** Quando il criterio di accesso condizionale non viene soddisfatto, blocca la connessione VPN, ma si connette dopo che l'utente seleziona **X** per chiudere il messaggio.  Se si seleziona **OK** , viene eseguito un altro tentativo di autenticazione che termina con un altro messaggio "Oops". Questi eventi vengono registrati nel registro eventi operativi di AAD del client.

- **Possibili cause**

  - L'utente dispone di un certificato di autenticazione client valido nell'archivio certificati personale che non è stato emesso da Azure AD.

  - La sezione del profilo VPN \<TLSExtensions\> manca o non contiene il **\<EKUName\>AAD Conditional Access\</EKUName\>\<EKUOID\>1.3.6.1.4.1.311.87 </EKUOID\>\<EKUName > AAD Conditional access </EKUName\>\<EKUOID\>1.3.6.1.4.1.311.87 </EKUOID\>** le voci. Le voci \<EKUName > e \<EKUOID > indicano al client VPN quale certificato recuperare dall'archivio certificati dell'utente quando passa il certificato al server VPN. Senza questo, il client VPN utilizza qualsiasi certificato di autenticazione client valido presente nell'archivio certificati dell'utente e l'autenticazione ha esito positivo. 

  - Il server RADIUS (NPS) non è stato configurato per accettare solo i certificati client che contengono l'OID di **accesso condizionale di AAD** .

- **Possibile soluzione.** Per eseguire l'escape di questo ciclo, procedere come segue:

  1. In Windows PowerShell eseguire il cmdlet **Get-WmiObject** per eseguire il dump della configurazione del profilo VPN. 
  2. Verificare che le sezioni **\<TLSExtensions >** , **\<EKUName >** e **\<EKUOID >** esistano e indichino il nome e l'OID corretti.
      
      ```powershell
      PS C:\> Get-WmiObject -Class MDM_VPNv2_01 -Namespace root\cimv2\mdm\dmmap

      __GENUS                 : 2
      __CLASS                 : MDM_VPNv2_01
      __SUPERCLASS            :
      __DYNASTY               : MDM_VPNv2_01
      __RELPATH               : MDM_VPNv2_01.InstanceID="AlwaysOnVPN",ParentID="./Vendor/MSFT/VPNv2"
      __PROPERTY_COUNT        : 10
      __DERIVATION            : {}
      __SERVER                : DERS2
      __NAMESPACE             : root\cimv2\mdm\dmmap
      __PATH                  : \\DERS2\root\cimv2\mdm\dmmap:MDM_VPNv2_01.InstanceID="AlwaysOnVPN",ParentID="./Vendor/MSFT/VP
                                  Nv2"
      AlwaysOn                :
      ByPassForLocal          :
      DnsSuffix               :
      EdpModeId               :
      InstanceID              : AlwaysOnVPN
      LockDown                :
      ParentID                : ./Vendor/MSFT/VPNv2
      ProfileXML              : <VPNProfile><RememberCredentials>false</RememberCredentials><DeviceCompliance><Enabled>true</
                                  Enabled><Sso><Enabled>true</Enabled></Sso></DeviceCompliance><NativeProfile><Servers>derras2.
                                  corp.deverett.info;derras2.corp.deverett.info</Servers><RoutingPolicyType>ForceTunnel</Routin
                                  gPolicyType><NativeProtocolType>Ikev2</NativeProtocolType><Authentication><UserMethod>Eap</Us
                                  erMethod><MachineMethod>Eap</MachineMethod><Eap><Configuration><EapHostConfig
                                  xmlns="https://www.microsoft.com/provisioning/EapHostConfig"><EapMethod><Type
                                  xmlns="https://www.microsoft.com/provisioning/EapCommon">25</Type><VendorId
                                  xmlns="https://www.microsoft.com/provisioning/EapCommon">0</VendorId><VendorType
                                  xmlns="https://www.microsoft.com/provisioning/EapCommon">0</VendorType><AuthorId
                                  xmlns="https://www.microsoft.com/provisioning/EapCommon">0</AuthorId></EapMethod><Config
                                  xmlns="https://www.microsoft.com/provisioning/EapHostConfig"><Eap xmlns="https://www.microsoft.
                                  com/provisioning/BaseEapConnectionPropertiesV1"><Type>25</Type><EapType xmlns="https://www.mic
                                  rosoft.com/provisioning/MsPeapConnectionPropertiesV1"><ServerValidation><DisableUserPromptFor
                                  ServerValidation>true</DisableUserPromptForServerValidation><ServerNames></ServerNames></Serv
                                  erValidation><FastReconnect>true</FastReconnect><InnerEapOptional>false</InnerEapOptional><Ea
                                  p xmlns="https://www.microsoft.com/provisioning/BaseEapConnectionPropertiesV1"><Type>13</Type>
                                  <EapType xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV1"><Credenti
                                  alsSource><CertificateStore><SimpleCertSelection>true</SimpleCertSelection></CertificateStore
                                  ></CredentialsSource><ServerValidation><DisableUserPromptForServerValidation>true</DisableUse
                                  rPromptForServerValidation><ServerNames></ServerNames><TrustedRootCA>5a 89 fe cb 5b 49 a7 0b
                                  1a 52 63 b7 35 ee d7 1c c2 68 be 4b </TrustedRootCA></ServerValidation><DifferentUsername>fal
                                  se</DifferentUsername><PerformServerValidation xmlns="https://www.microsoft.com/provisioning/E
                                  apTlsConnectionPropertiesV2">true</PerformServerValidation><AcceptServerName xmlns="https://ww
                                  w.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">false</AcceptServerName><TLSExtens
                                  ions
                                  xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2"><FilteringInfo xml
                                  ns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV3"><EKUMapping><EKUMap><
                                  EKUName>AAD Conditional
                                  Access</EKUName><EKUOID>1.3.6.1.4.1.311.87</EKUOID></EKUMap></EKUMapping><ClientAuthEKUList
                                  Enabled="true"><EKUMapInList><EKUName>AAD Conditional Access</EKUName></EKUMapInList></Client
                                  AuthEKUList></FilteringInfo></TLSExtensions></EapType></Eap><EnableQuarantineChecks>false</En
                                  ableQuarantineChecks><RequireCryptoBinding>false</RequireCryptoBinding><PeapExtensions><Perfo
                                  rmServerValidation xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV2"
                                  >false</PerformServerValidation><AcceptServerName xmlns="https://www.microsoft.com/provisionin
                                  g/MsPeapConnectionPropertiesV2">false</AcceptServerName></PeapExtensions></EapType></Eap></Co
                                  nfig></EapHostConfig></Configuration></Eap></Authentication></NativeProfile></VPNProfile>
      RememberCredentials     : False
      TrustedNetworkDetection :
      PSComputerName          : DERS2
      ```

  3. Per determinare se sono presenti certificati validi nell'archivio certificati dell'utente, eseguire il comando **certutil** :

     ```powershell
     C:\>certutil -store -user My

      My "Personal"
      ================ Certificate 0 ================
      Serial Number: 32000000265259d0069fa6f205000000000026
      Issuer: CN=corp-DEDC0-CA, DC=corp, DC=deverett, DC=info
       NotBefore: 12/8/2017 8:07 PM
       NotAfter: 12/8/2018 8:07 PM
      Subject: E=winfed@deverett.info, CN=WinFed, OU=Users, OU=Corp, DC=corp, DC=deverett, DC=info
      Certificate Template Name (Certificate Type): User
      Non-root Certificate
      Template: User
      Cert Hash(sha1): a50337ab015d5612b7dc4c1e759d201e74cc2a93
        Key Container = a890fd7fbbfc072f8fe045e680c501cf_5834bfa9-1c4a-44a8-a128-c2267f712336
        Simple container name: te-User-c7bcc4bd-0498-4411-af44-da2257f54387
        Provider = Microsoft Enhanced Cryptographic Provider v1.0
      Encryption test passed
        
      ================ Certificate 1 ================
      Serial Number: 367fbdd7e6e4103dec9b91f93959ac56
      Issuer: CN=Microsoft VPN root CA gen 1
       NotBefore: 12/8/2017 6:24 PM
       NotAfter: 12/8/2017 7:29 PM
      Subject: CN=WinFed@deverett.info
      Non-root Certificate
      Cert Hash(sha1): 37378a1b06dcef1b4d4753f7d21e4f20b18fbfec
        Key Container = 31685cae-af6f-48fb-ac37-845c69b4c097
        Unique container name: bf4097e20d4480b8d6ebc139c9360f02_5834bfa9-1c4a-44a8-a128-c2267f712336
        Provider = Microsoft Software Key Storage Provider
      Private key is NOT exportable
      Encryption test passed
     ```
     >[!NOTE]
     >Se un certificato dell'autorità emittente **CN = Microsoft VPN root CA gen 1** è presente nell'archivio personale dell'utente, ma l'utente ha ottenuto l'accesso selezionando **X** per chiudere il messaggio oops, raccogliere i registri eventi di CAPI2 per verificare che il certificato usato per l'autenticazione sia un certificato di autenticazione client valido non emesso dalla CA radice VPN Microsoft.

  4. Se un certificato di autenticazione client valido è presente nell'archivio personale dell'utente, la connessione ha esito negativo (come dovrebbe) dopo che l'utente ha selezionato la **X** e se le sezioni **\<TLSExtensions >** , **\<EKUName >** e **\<EKUOID >** sono presenti e contengono le informazioni corrette.
   
     Viene visualizzato un messaggio di errore che indica che non è stato possibile trovare un certificato da usare con il protocollo Extensible Authenticate.

### <a name="unable-to-delete-the-certificate-from-the-vpn-connectivity-blade"></a>Non è possibile eliminare il certificato dal pannello connettività VPN

- **Descrizione dell'errore.** Non è possibile eliminare i certificati nel pannello connettività VPN.

- **Possibile cause.** Il certificato è impostato come **primario**.

- **Possibile soluzione.**

    1. Nel pannello connettività VPN selezionare il certificato.
    2. In **primario**selezionare **No**, quindi selezionare **Salva**.
    3. Nel pannello connettività VPN selezionare di nuovo il certificato.
    4. Selezionare **Elimina**.
