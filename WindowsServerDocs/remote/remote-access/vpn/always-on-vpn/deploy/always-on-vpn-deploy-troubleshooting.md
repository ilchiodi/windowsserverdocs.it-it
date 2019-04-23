---
title: Risolvere i problemi di VPN Always On
description: Questo argomento fornisce istruzioni per la verifica e risoluzione dei problemi di distribuzione VPN Always On in Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: 4d08164e-3cc8-44e5-a319-9671e1ac294a
ms.localizationpriority: medium
ms.date: 06/11/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 47d22d566407f45fe6ac78931ffea7b5b2854a1c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59839092"
---
# <a name="troubleshoot-always-on-vpn"></a>Risolvere i problemi di VPN Always On 

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

Se la configurazione VPN Always On non riesce a connettersi ai client alla rete interna, la causa è probabilmente un certificato VPN non valido, i criteri di criteri di rete non corretti o problemi con gli script di distribuzione client o nel Routing e accesso remoto. Il primo passaggio nella risoluzione dei problemi e il test della connessione VPN è comprendere i componenti principali dell'infrastruttura VPN Always On. 

È possibile risolvere i problemi di connessione in diversi modi. Per i problemi lato client e la risoluzione dei problemi generale, i log dell'applicazione nei computer client sono estremamente utile. Per problemi specifici di autenticazione, i log dei criteri di rete nel server NPS consente di determinare la causa del problema.


## <a name="error-codes"></a>Codici di errore


### <a name="error-code-800"></a>Codice errore: 800

-   **Descrizione dell'errore.** La connessione remota non è stata effettuata perché i tentativi di tunnel VPN non è riuscita. Il server VPN potrebbe non essere raggiungibile. Se questa connessione sta tentando di usare un tunnel L2TP/IPsec, parametri di sicurezza necessarie per la negoziazione non sia configurata correttamente.


-   **Possibile causa.** Questo errore si verifica quando il tipo di tunnel VPN **automatica** e il tentativo di connessione ha esito negativo per tutti i tunnel VPN.

-   **Possibili soluzioni:**

    -   Se si sa quale tunnel da utilizzare per la distribuzione, impostare il tipo di VPN per tale tipo di tunnel particolare sul lato client VPN.

    -   Per stabilire una connessione VPN con un tipo particolare di tunnel, la connessione avrà comunque esito negativo, ma si verificherà un errore più specifico del tunnel (ad esempio, "GRE bloccato per PPTP").

    -   Questo errore si verifica anche quando il server VPN non è raggiungibile o errore di connessione del tunnel.

-   **Assicurarsi:**

    -   Le porte di IKE (porte UDP 500 e 4500) non sono bloccate.

    -   I certificati corretti per IKE sono presenti sia il client e server.

### <a name="error-code-809"></a>Codice errore: 809

-   **Descrizione dell'errore.**  Non è stato possibile stabilire la connessione di rete tra il computer e il server VPN, perché il server remoto non risponde. È possibile che uno dei dispositivi di rete \(ad esempio, i router di firewall, NAT,\) tra il computer e il server remoto non è configurato per consentire le connessioni VPN. Contattare l'amministratore o il provider di servizi per determinare quale dispositivo potrebbe causare il problema.

-   **Possibile causa.** Questo errore è causato da 4500 porte nel server VPN o il firewall o bloccato UDP 500.

-   **Possibile soluzione.** Assicurarsi che le porte UDP 500 e 4500 siano consentite attraverso tutti i firewall tra il client e il server RRAS. 
    
    
### <a name="error-code-812"></a>Codice errore: 812

-   **Descrizione dell'errore.** Impossibile connettersi alla VPN Always On. La connessione è stata impedita a causa di un criterio configurato nel server RAS/VPN. In particolare, il metodo di autenticazione server usato per verificare il nome utente e password potrebbe non corrispondere al metodo di autenticazione configurato nel profilo di connessione. Contattare l'amministratore del server RAS e inviare una notifica all'utente di questo errore.

-   **Possibili cause:**

    -  La causa tipica di questo errore è che i criteri di rete ha specificato una condizione di autenticazione che non supportano il client. Ad esempio, i criteri di rete possono specificare l'uso di un certificato per proteggere la connessione di PEAP, ma il client tenta di utilizzare EAP-MSCHAPv2.

    -  Registro eventi 20276 viene registrato nel Visualizzatore eventi quando l'impostazione del protocollo VPN basato su RRAS autenticazione server non corrisponde a quello del computer client VPN.

-   **Possibile soluzione.** Assicurarsi che la configurazione client soddisfa le condizioni specificate nel server NPS.


### <a name="error-code-13806"></a>Codice errore: 13806

-   **Descrizione dell'errore.** IKE non è stato possibile trovare un certificato di computer valido. Contattare l'amministratore della sicurezza di rete sull'installazione di un certificato valido nell'archivio certificati appropriato.

-   **Possibile causa.** Questo errore si verifica in genere quando nessun certificato di computer o certificato radice del computer è presente nel server VPN.

-   **Possibile soluzione.** Assicurarsi che i certificati descritti in questa distribuzione siano installati nel computer client sia il server VPN.

### <a name="error-code-13801"></a>Codice errore: 13801

-   **Descrizione dell'errore.** Le credenziali di autenticazione IKE sono inaccettabili.

-   **Possibili cause.** Questo errore si verifica in genere in uno dei seguenti casi:

    -   Non ha il certificato del computer usato per la convalida di IKEv2 nel server RAS **autenticazione Server** sotto **Enhanced Key Usage**.

    -   Il certificato del computer nel server di RAS è scaduto.

    -   Il certificato radice da convalidare il certificato del server RAS non è presente nel computer client.

    -   Il nome del server VPN usato nel computer client non corrisponde la **subjectName** del certificato del server.

-   **Possibile soluzione.** Verificare che il certificato del server includa **autenticazione Server** sotto **Enhanced Key Usage**. Verificare che il certificato del server sia ancora valido. Verificare che l'autorità di certificazione usata sia elencato sotto **autorità di certificazione radice attendibili** nel server RRAS. Verificare che il client VPN si connette usando il nome FQDN del server VPN come presentata nel certificato del server VPN.


### <a name="error-code-0x80070040"></a>Codice errore: 0x80070040

-   **Descrizione dell'errore.** Il certificato del server non dispone **autenticazione Server** come una delle relative voci dell'utilizzo di certificati.

-   **Possibile causa.** Questo errore può verificarsi se non è installato alcun certificato di autenticazione server nel server RAS.

-   **Possibile soluzione.** Assicurarsi che il certificato del computer del server RAS usi per **IKEv2** ha **autenticazione Server** come una delle voci di utilizzo del certificato.

### <a name="error-code-0x800b0109"></a>Codice errore: 0x800B0109

In genere, il computer client VPN è aggiunto al dominio basato su Active Directory. Se si usano le credenziali di dominio per l'accesso al server VPN, il certificato viene installato automaticamente in Autorità di certificazione radice attendibili archiviare. Tuttavia, se il computer non è unito al dominio o se si usa una catena di certificati alternativo, che si verifichi questo problema.

-   **Descrizione dell'errore.** Una catena di certificati elaborata, ma termina con un certificato radice che il provider di attendibilità non considera attendibile.

-   **Possibile causa.** Questo errore può verificarsi se il certificato CA radice attendibile appropriato non è installato in Autorità di certificazione radice attendibili archiviare nel computer client.

-   **Possibile soluzione.** Assicurarsi che il certificato radice sia installato nel computer client nell'archivio Autorità di certificazione radice attendibili.

## <a name="logs"></a>Log

### <a name="application-logs"></a>Log dell'applicazione

L'applicazione nei computer client nei registri la maggior parte dei dettagli degli eventi di connessione VPN di livello superiore.

Cercare gli eventi dall'origine ClientRas. Tutti i messaggi di errore restituiscono il codice di errore alla fine del messaggio. Di seguito sono descritte alcune dei codici di errore più comuni, ma è disponibile in un elenco completo [Routing e i codici di errore di accesso remoto](https://msdn.microsoft.com/library/windows/desktop/bb530704.aspx).

## <a name="nps-logs"></a>Log dei criteri di rete
Criteri di rete crea e archivia i log di accounting di NPS. Per impostazione predefinita, questi vengono archiviati in % SYSTEMROOT %\\System32\\Logfiles\\ in un file denominato nella*XXXX.* txt, dove *XXXX* è la data di creazione del file.

Per impostazione predefinita, questi log sono in formato con valori delimitati da virgole, ma non includono una riga di intestazione. La riga di intestazione è:

```
ComputerName,ServiceName,Record-Date,Record-Time,Packet-Type,User-Name,Fully-Qualified-Distinguished-Name,Called-Station-ID,Calling-Station-ID,Callback-Number,Framed-IP-Address,NAS-Identifier,NAS-IP-Address,NAS-Port,Client-Vendor,Client-IP-Address,Client-Friendly-Name,Event-Timestamp,Port-Limit,NAS-Port-Type,Connect-Info,Framed-Protocol,Service-Type,Authentication-Type,Policy-Name,Reason-Code,Class,Session-Timeout,Idle-Timeout,Termination-Action,EAP-Friendly-Name,Acct-Status-Type,Acct-Delay-Time,Acct-Input-Octets,Acct-Output-Octets,Acct-Session-Id,Acct-Authentic,Acct-Session-Time,Acct-Input-Packets,Acct-Output-Packets,Acct-Terminate-Cause,Acct-Multi-Ssn-ID,Acct-Link-Count,Acct-Interim-Interval,Tunnel-Type,Tunnel-Medium-Type,Tunnel-Client-Endpt,Tunnel-Server-Endpt,Acct-Tunnel-Conn,Tunnel-Pvt-Group-ID,Tunnel-Assignment-ID,Tunnel-Preference,MS-Acct-Auth-Type,MS-Acct-EAP-Type,MS-RAS-Version,MS-RAS-Vendor,MS-CHAP-Error,MS-CHAP-Domain,MS-MPPE-Encryption-Types,MS-MPPE-Encryption-Policy,Proxy-Policy-Name,Provider-Type,Provider-Name,Remote-Server-Address,MS-RAS-Client-Name,MS-RAS-Client-Version
```

Se si incolla la riga di intestazione come prima riga del file di log, quindi importare il file in Microsoft Excel, le colonne saranno contrassegnate in modo corretto.

I log dei criteri di rete possono essere utili per diagnosticare i problemi relativi ai criteri. Per altre informazioni sui log dei criteri di rete, vedere [interpretare NPS Log file in formato Database](https://technet.microsoft.com/library/cc771748.aspx).

## <a name="vpnprofileps1-script-issues"></a>Problemi degli script VPN_Profile.ps1
I problemi più comuni quando si esegue manualmente lo script VPN_ Profile.ps1 includono:

- Viene utilizzato uno strumento di connessione remota?  Assicurarsi di non usare RDP o un altro metodo di connessione remota, come lo dovuto sistemare i problemi con il rilevamento di account di accesso utente.

- È l'utente amministratore del computer locale?  Assicurarsi che durante l'esecuzione dello script VPN_Profile.ps1 che l'utente disponga dei privilegi di amministratore.

- Si dispone di ulteriori funzionalità di sicurezza di PowerShell abilitata? Assicurarsi che i criteri di esecuzione di PowerShell non stia bloccando lo script. È possibile considerare la disattivazione modalità linguaggio vincolato, se abilitata, prima di eseguire lo script. È possibile attivare la modalità linguaggio vincolata dopo il completamento dello script.

## <a name="always-on-vpn-client-connection-issues"></a>Problemi di connessione client VPN Always On
Una configurazione errata di piccole dimensioni può causare l'errore di connessione client e può essere difficile per individuare la causa.  Un client VPN Always On esegue diversi passaggi prima di stabilire una connessione. Risolvere i problemi di connessione client, seguire la procedura di eliminazione con gli elementi seguenti:


1. È connesso il computer di modello esternamente? Oggetto **whatismyip** analisi dovrebbe mostrare un indirizzo IP pubblico che non appartiene all'utente.

2. È possibile risolvere il nome del server Accesso remoto/VPN a un indirizzo IP? Nelle **Pannello di controllo** \> **rete** e **Internet** \> **connessioni di rete**, aprire le proprietà per il profilo VPN. Il valore di **generale** scheda deve essere pubblicamente risolvibile con DNS.

3. È possibile accedere al server VPN verso una rete esterna? È consigliabile aprire messaggio protocollo ICMP (Internet Control) per l'interfaccia esterna e il ping del nome dal client remoto. Dopo un ping ha esito positivo, è possibile rimuovere il protocollo ICMP regola di assenso.

4. Si hanno le schede NIC interna ed esterna del server VPN configurata in modo corretto? Sono in subnet diverse? L'interfaccia di rete esterno la connessione all'interfaccia corretta nel firewall?

5. UDP 500 e 4500 porte siano aperte dal client per interfaccia esterna del server VPN? Controllare il firewall del client, firewall del server e tutti i firewall hardware. IPSEC utilizza UDP porta 500,. assicurati pertanto verificare che non è disabilitato o bloccato in un punto qualsiasi di IPSec.

7. La convalida dei certificati ha esito negativo? Verificare che il server NPS disponga di un certificato di autenticazione Server che riesce a servire le richieste di IKE. Assicurarsi di avere l'indirizzo IP server VPN corretta specificato come un client di criteri di rete. Assicurarsi che si esegue l'autenticazione con il protocollo PEAP e la finestra delle proprietà PEAP deve consentire solo l'autenticazione con un certificato. È possibile controllare i registri eventi dei criteri di rete per gli errori di autenticazione. Per altre informazioni, vedere [installare e configurare il Server dei criteri di rete](vpn-deploy-nps.md)

8. È la connessione ma non ha accesso alla rete Internet o locale? Controllare il pool IP di server DHCP, VPN per i problemi di configurazione.

9.  La connessione e avere un indirizzo IP interno valido senza non hanno accesso alle risorse locali?  Verificare che i client sappiano come accedere a tali risorse. È possibile usare il server VPN per indirizzare le richieste.


## <a name="azure-ad-conditional-access-connection-issues"></a>Problemi di connessione di accesso condizionale AD Azure

### <a name="oops---you-cant-get-to-this-yet"></a>Si - è possibile ottenere questo ancora

-   **Descrizione dell'errore.** Quando i criteri di accesso condizionale è non soddisfatto, il blocco della connessione VPN, ma si connette dopo che l'utente fa clic **X** per chiudere il messaggio.  Facendo clic **OK** fa sì che un altro tentativo di autenticazione, che termina in un'altra _Oops_ messaggio. Questi eventi vengono registrati nel log eventi operativi di AAD del client. 

-   **Possibile causa.** 

    - L'utente dispone di un certificato di autenticazione client valido nel certificato personale archivio che non è stato rilasciato da Azure AD.

    - Il profilo VPN \<TLSExtensions\> sezione è mancante o non non contengono le **\<EKUName\>accesso condizionale di AAD\</EKUName\> \< EKUOID\>1.3.6.1.4.1.311.87 < / EKUOID\>\<EKUName > accesso condizionale di AAD < / EKUName\>\<EKUOID\>1.3.6.1.4.1.311.87 < / EKUOID\>** voci. Il \<EKUName > e \<EKUOID > le voci di indicano il client VPN certificato da recuperare dall'archivio certificati dell'utente quando si passa il certificato per il server VPN. In caso contrario, il client VPN utilizza qualsiasi certificato di autenticazione Client valido sia nell'archivio certificati dell'utente e l'autenticazione ha esito positivo. 

    - Il server RADIUS (NPS) non è stato configurato per accettare solo i certificati client che contengono il **accesso condizionale di AAD** OID.

-   **Possibile soluzione.** Per eseguire l'escape di questo ciclo, eseguire le operazioni seguenti:

    1. In Windows PowerShell, eseguire la **Get-WmiObject** cmdlet per eseguire il dump della configurazione del profilo VPN. 
    2. Verificare che il  **\<TLSExtensions >**,  **\<EKUName >**, e  **\<EKUOID >** sezioni esiste e viene illustrato il valore corretto Name e OID. 
        ```
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

    3. Per determinare se sono presenti certificati validi nell'archivio certificati dell'utente, eseguire la **Certutil** comando:

       ```
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
       >Se un certificato dall'autorità di certificazione **CN = generazione di autorità di certificazione radice Microsoft VPN 1** è presente nell'archivio personale dell'utente, ma l'utente ha ottenuto l'accesso facendo clic **X** per chiudere il messaggio Oops, raccogliere i registri eventi di CAPI2 verificare il certificato usato per l'autenticazione è un certificato di autenticazione Client valido che non è stato rilasciato dalla CA radice VPN di Microsoft.

   4. Se esiste un certificato di autenticazione Client valido nell'archivio personale dell'utente, la connessione ha esito negativo (come dovrebbe essere) dopo che l'utente fa clic il **X** e, se il  **\<TLSExtensions >**,  **\<EKUName >**, e  **\<EKUOID >** sezioni presenti e contenere le informazioni corrette.<p>Il _non è stato trovato un certificato che può essere utilizzato con il protocollo di autenticazione estendibile._ viene visualizzato il messaggio di errore.

### <a name="unable-to-delete-the-certificate-from-the-vpn-connectivity-blade"></a>Non è possibile eliminare il certificato nel Pannello di connettività VPN

-   **Descrizione dell'errore.** Non è possibile eliminare i certificati nel Pannello di connettività della VPN.

-   **Possibile causa.** Il certificato è impostato su **primaria**.

-   **Possibile soluzione.** 

    1. Nel Pannello di connettività VPN, selezionare il certificato.
    2. Sotto **primari**, selezionare **senza** e fare clic su **Salva**.
    3. Nel pannello della connettività VPN, selezionare di nuovo il certificato.
    4. Fare clic su **Elimina**.


---
