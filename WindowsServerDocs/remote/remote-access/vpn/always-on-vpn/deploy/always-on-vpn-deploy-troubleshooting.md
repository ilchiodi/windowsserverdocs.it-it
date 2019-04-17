---
title: Risolvere i problemi di VPN Always On
description: In questo argomento vengono fornite istruzioni per la verifica e risoluzione dei problemi di distribuzione di VPN Always On in Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: 4d08164e-3cc8-44e5-a319-9671e1ac294a
ms.localizationpriority: medium
ms.date: 06/11/2018
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 47d22d566407f45fe6ac78931ffea7b5b2854a1c
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067374"
---
# Risolvere i problemi di VPN Always On 

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

Se l'installazione di VPN Always On non riesce a connettersi client alla rete interna, la causa è probabilmente problemi con gli script di distribuzione di client o in Routing e accesso remoto, i criteri dei criteri di rete non corretti o un certificato VPN non valido. Il primo passaggio per la risoluzione dei problemi e il test della connessione VPN è comprendere i componenti fondamentali dell'infrastruttura di VPN Always On. 

È possibile risolvere i problemi di connessione in diversi modi. Per la risoluzione dei problemi generali e problemi lato client, i registri di applicazioni nei computer client si rivelano particolarmente utili. Per i problemi di autenticazione specifiche, il registro dei criteri di rete nel server dei criteri di rete possa aiutarti a determinare l'origine del problema.


## Codici di errore


### Codice di errore: 800

-   **Descrizione dell'errore.** La connessione remota non è stata effettuata perché non è riuscito il tentativo di VPN tunnel. Il server VPN potrebbe non essere raggiungibile. Se questa connessione è il tentativo di usare un tunnel IPsec/L2TP, i parametri di sicurezza necessario per IPsec negoziazione potrebbe non essere configurata correttamente.


-   **Possibile causa.** Questo errore si verifica quando il tipo di tunnel VPN è **automatica** e il tentativo di connessione ha esito negativo per tutti i tunnel VPN.

-   **Soluzioni possibili:**

    -   Se sai quali tunnel da usare per la distribuzione, impostare il tipo di VPN a quel tipo di tunnel particolare sul lato client VPN.

    -   Effettuando una connessione VPN con un tipo di tunnel particolare, la connessione ha esito negativo, ma il risultato sarà un errore più tunnel specifico (ad esempio, "GRE bloccato per PPTP").

    -   Questo errore si verifica anche quando il server VPN non raggiungibile o la connessione tunnel ha esito negativo.

-   **Assicurarsi:**

    -   Porte IKE (UDP ports500 e 4500) non vengono bloccati.

    -   I certificati corretti per IKE sono presenti sul client e server.

### Codice di errore: 809

-   **Descrizione dell'errore.**  La connessione di rete tra computer e il server VPN potrebbe non essere stabilita perché il server remoto non risponde. Ciò potrebbe essere perché una delle periferiche di rete \ (ad esempio, firewall, NAT, routers\) tra il computer e il server remoto non è configurato per consentire le connessioni VPN. Contatta l'amministratore o il provider di servizi per determinare quale dispositivo potrebbe potrebbero causare il problema.

-   **Possibile causa.** Questo errore è causato da 500 UDP bloccato o 4500 porte nel server VPN o del firewall.

-   **Soluzioni possibili.** Assicurarsi che tutti i firewall tra il client e server RRAS sono consentiti 4500 e UDP ports500. 
    
    
### Codice di errore: 812

-   **Descrizione dell'errore.** Non riesce a connettersi alla VPN Always On. La connessione è stata bloccata a causa di un criterio configurato nel server RAS/VPN. In particolare, il metodo di autenticazione server utilizzato per verificare il nome utente e password potrebbe non corrispondere al metodo di autenticazione configurato nel tuo profilo di connessione. Contatta l'amministratore del server RAS e inviare una notifica all'utente di questo errore.

-   **Possibili cause:**

    -  La causa tipica di questo errore è che i criteri di rete ha specificato una condizione di autenticazione che il client non è possibile soddisfare. Ad esempio, i criteri di rete possono specificare l'uso di un certificato per proteggere la connessione PEAP, ma il client sta tentando di usare EAP-MSCHAPv2.

    -  Registro eventi 20276 viene registrato nel Visualizzatore eventi quando l'impostazione del protocollo di autenticazione server VPN basate su RRAS non corrispondere a quella del computer client VPN.

-   **Soluzioni possibili.** Assicurarsi che la configurazione client soddisfa le condizioni che vengono specificate nel server dei criteri di rete.


### Codice di errore: 13806

-   **Descrizione dell'errore.** IKE non è riuscito a trovare il certificato del computer valido. Contatta l'amministratore della sicurezza di rete sull'installazione di un certificato valido nell'archivio certificati appropriato.

-   **Possibile causa.** Questo errore si verifica in genere quando nessun certificato del computer o certificato radice della macchina è presente nel server VPN.

-   **Soluzioni possibili.** Assicurarsi che i certificati illustrati in questa distribuzione siano installati nel computer client sia il server VPN.

### Codice di errore: 13801

-   **Descrizione dell'errore.** Le credenziali di autenticazione IKE sono semplicemente inaccettabile.

-   **Possibili cause.** Questo errore si verifica in genere in uno dei seguenti casi:

    -   Certificato del computer usato per la convalida IKEv2 nel server RAS non dispone di **Autenticazione Server** in **Utilizzo chiavi avanzato**.

    -   Il certificato del computer nel server RAS è scaduto.

    -   Il certificato radice per convalidare il certificato del server RAS non è presente nel computer client.

    -   Il nome del server VPN utilizzato nel computer client non corrisponde il **subjectName** del certificato server.

-   **Soluzioni possibili.** Verifica che il certificato del server include l' **Autenticazione Server** in **Utilizzo chiavi avanzato**. Verifica che il certificato del server sia ancora valido. Verificare che l'autorità di certificazione usato è elencato in **Autorità di certificazione radice attendibili** per il server RRAS. Verifica che il client VPN si connette con il nome FQDN del server VPN, così come presentato nel certificato del server VPN.


### Codice di errore: 0x80070040

-   **Descrizione dell'errore.** Il certificato del server non dispone di **Autenticazione Server** come uno dei relativi movimenti di utilizzo del certificato.

-   **Possibile causa.** Questo errore può verificarsi se nessun certificato di autenticazione server è installato nel server RAS.

-   **Soluzioni possibili.** Assicurati che il certificato del computer che il server RAS utilizza per **IKEv2** dispone di **Autenticazione Server** come uno dei movimenti di utilizzo di certificato.

### Codice di errore: 0x800B0109

In generale, il computer client VPN viene aggiunto al dominio basata su Active Directory. Se usi le credenziali di dominio per accedere al server VPN, il certificato viene installato automaticamente nell'autorità di certificazione radice attendibili archiviare. Tuttavia, se il computer non viene aggiunto al dominio o se si utilizza una catena di certificati alternativo, si potrebbe riscontrare questo problema.

-   **Descrizione dell'errore.** Una catena di certificati elaborata termina in un certificato radice che non considera attendibile il provider di trust.

-   **Possibile causa.** Questo errore può verificarsi se il certificato CA radice attendibile appropriato non è installato nell'autorità di certificazione radice attendibili archiviare nel computer client.

-   **Soluzioni possibili.** Assicurati che il certificato radice è installato nel computer client nell'archivio Autorità di certificazione radice attendibili.

## Log

### Registri di applicazione

I registri di applicazioni nei computer client registrano la maggior parte dei dettagli di livello superiore di eventi di connessione VPN.

Cerca gli eventi da source ClientRas. Tutti i messaggi di errore restituiscono il codice di errore alla fine del messaggio. Alcuni dei codici di errore più comuni sono descritte di seguito, ma un elenco completo è disponibile in [Routing e codici di errore di accesso remoto](https://msdn.microsoft.com/library/windows/desktop/bb530704.aspx).

## Registri dei criteri di rete
Criteri di rete crea e archivia i registri di accounting dei criteri di rete. Per impostazione predefinita, questi vengono archiviati in %SYSTEMROOT%\\System32\\Logfiles\\ in un file denominato nel*XXXX.* txt, dove *XXXX* è la data di creazione del file.

Per impostazione predefinita, questi log sono in formato con valori delimitati da virgole, ma non includono una riga dell'intestazione. La riga dell'intestazione è:

```
ComputerName,ServiceName,Record-Date,Record-Time,Packet-Type,User-Name,Fully-Qualified-Distinguished-Name,Called-Station-ID,Calling-Station-ID,Callback-Number,Framed-IP-Address,NAS-Identifier,NAS-IP-Address,NAS-Port,Client-Vendor,Client-IP-Address,Client-Friendly-Name,Event-Timestamp,Port-Limit,NAS-Port-Type,Connect-Info,Framed-Protocol,Service-Type,Authentication-Type,Policy-Name,Reason-Code,Class,Session-Timeout,Idle-Timeout,Termination-Action,EAP-Friendly-Name,Acct-Status-Type,Acct-Delay-Time,Acct-Input-Octets,Acct-Output-Octets,Acct-Session-Id,Acct-Authentic,Acct-Session-Time,Acct-Input-Packets,Acct-Output-Packets,Acct-Terminate-Cause,Acct-Multi-Ssn-ID,Acct-Link-Count,Acct-Interim-Interval,Tunnel-Type,Tunnel-Medium-Type,Tunnel-Client-Endpt,Tunnel-Server-Endpt,Acct-Tunnel-Conn,Tunnel-Pvt-Group-ID,Tunnel-Assignment-ID,Tunnel-Preference,MS-Acct-Auth-Type,MS-Acct-EAP-Type,MS-RAS-Version,MS-RAS-Vendor,MS-CHAP-Error,MS-CHAP-Domain,MS-MPPE-Encryption-Types,MS-MPPE-Encryption-Policy,Proxy-Policy-Name,Provider-Type,Provider-Name,Remote-Server-Address,MS-RAS-Client-Name,MS-RAS-Client-Version
```

Se si incolla questa riga del titolo come prima riga del file di log, quindi importare il file in Microsoft Excel, saranno contrassegnate correttamente le colonne.

I registri dei criteri di rete possono essere utili per diagnosticare i problemi correlati ai criteri. Per ulteriori informazioni sui registri dei criteri di rete, vedi [I file di Log di interpretare NPS Database formato](https://technet.microsoft.com/library/cc771748.aspx).

## Problemi di script VPN_Profile.ps1
I problemi più comuni quando si esegue manualmente lo script VPN_ Profile.ps1 includono:

- Usare uno strumento di connessione remota?  Assicurati di non utilizzare RDP o un altro metodo di connessione remota come lo fanno con il rilevamento di accesso utente.

- È un amministratore del computer locale dell'utente?  Assicurati che l'esecuzione dello script VPN_Profile.ps1 che l'utente abbia privilegi di amministratore.

- Hai ulteriori funzionalità di sicurezza di PowerShell abilitata? Assicurati che i criteri di esecuzione di PowerShell non sta bloccando lo script. Valuta la disattivazione della modalità di linguaggio vincolato, se abilitata, prima di eseguire lo script. Dopo che lo script viene completato correttamente, è possibile attivare la modalità di linguaggio vincolato.

## Problemi di connessione client VPN Always On
Una configurazione errata di piccole dimensioni può causare la connessione client esito negativo e possa essere difficili da individuare la causa.  Un client VPN Always On prevede diversi passaggi prima di stabilire una connessione. Durante la risoluzione dei problemi di connessione client, sottoposto al processo di eliminazione con quanto segue:


1. Il computer modello esternamente è connesso? Un'analisi **whatismyip** dovrebbero visualizzare un indirizzo IP pubblico che non appartiene a te.

2. È possibile risolvere il nome del server Accesso remoto/VPN in un indirizzo IP? Nel **Pannello di controllo** \> **rete** e **Internet** \> **Connessioni di rete**, Apri le proprietà per il profilo VPN. Il valore nella scheda **Generale** deve essere pubblicamente risolvibile tramite DNS.

3. È possibile accedere ai server VPN da una rete esterna? Prendi in considerazione il ping il nome dal client remoto e apertura di controllo protocollo ICMP (Internet Message) per l'interfaccia esterna. Dopo un ping ha esito positivo, è possibile rimuovere la ICMP consentire regola.

4. Hai le schede NIC interne ed esterne nel server VPN configurato correttamente? Sono in una subnet diversa? Scheda di rete esterna la connessione per l'interfaccia corretta sul firewall?

5. UDP 500 e 4500 porte sono aperte dal client all'interfaccia esterna del server VPN? Controlla il firewall client, server firewall e qualsiasi firewall hardware. IPSEC utilizza marca così di UDP porta 500, assicurati che non hai IPEC disabilitato o bloccato in un punto qualsiasi.

7. Convalida del certificato non riesce? Verificare che il server dei criteri di rete dispone di un certificato di autenticazione Server che può IKE richieste di servizio. Assicurati che hai IP del server VPN corretto specificato come un client dei criteri di rete. Assicurati che eseguono l'autenticazione con PEAP e le proprietà PEAP devono consentire solo l'autenticazione con un certificato. Puoi controllare i registri eventi dei criteri di rete per gli errori di autenticazione. Per altri dettagli, vedi [installare e configurare il Server dei criteri di rete](vpn-deploy-nps.md)

8. Si connette, ma non hanno accesso alla rete Internet/locale? Controlla i pool di IP DHCP/VPN server i problemi di configurazione.

9.  Si connette e avere un indirizzo IP interno valido senza non hanno accesso alle risorse locali?  Verifica che i client sappiano come accedere a tali risorse. È possibile utilizzare il server VPN per indirizzare le richieste.


## Problemi di connessione Azure AD accesso condizionale

### E poi - non è possibile ottenere a questa ancora

-   **Descrizione dell'errore.** Quando il criterio di accesso condizionale non viene soddisfatta, bloccare la connessione VPN, ma si connette dopo l'utente fa clic **X** per chiudere il messaggio.  Facendo clic su **OK** fa sì che un altro tentativo di autenticazione e termina con un altro messaggio _Oops_ . Questi eventi vengono registrati nel registro eventi operative AAD del client. 

-   **Possibile causa.** 

    - L'utente ha un certificato di autenticazione client valido nel proprio certificato personale archiviare che non è stato rilasciato da Azure AD.

    - Il profilo VPN \ < TLSExtensions\ > sezione è mancante o non non contengono il **\ < EKUName\ > Access\ condizionale AAD < / EKUName\ > \ < EKUOID\ > 1.3.6.1.4.1.311.87 < / EKUOID\ > \ < EKUName > accesso condizionale AAD < / EKUName\ > \ < EKUOID\ > 1.3.6.1.4.1.311.87 < / EKUOID\ >** le voci. Il \ < EKUName > e \ voci < EKUOID > indicano il client VPN il certificato da recuperare dall'archivio certificati dell'utente quando si passa il certificato al server VPN. In caso contrario, il client VPN Usa qualsiasi certificato di autenticazione Client valido è nell'archivio certificati dell'utente e l'autenticazione ha esito positivo. 

    - Il server RADIUS (NPS) non è stato configurato per accettare solo i certificati client che contengono l' **Accesso condizionale AAD** OID.

-   **Soluzioni possibili.** Per questo ciclo di escape, Esegui le operazioni seguenti:

    1. In Windows PowerShell, eseguire il cmdlet **Get-WmiObject** per eseguire il dump la configurazione del profilo VPN. 
    2. Verificare che il **\ < TLSExtensions >**, **\ < EKUName >**, e **\ < EKUOID >** sezioni esistenti e Mostra il nome e corretti OID. 
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

    3. Per determinare se sono presenti certificati validi nell'archivio certificati dell'utente, eseguire il comando **Certutil** :

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
       >Se un certificato di autorità emittente **CN = generazione CA radice Microsoft VPN 1** è presente nell'archivio personale dell'utente, ma l'utente ottenuto l'accesso, fai clic su **X** per chiudere il messaggio Oops, raccogliere i registri eventi CAPI2 per verificare il certificato utilizzato per l'autenticazione è un certificato di autenticazione Client valido che non è stato rilasciato dall'autorità di certificazione radice Microsoft VPN.

   4. Se esiste un certificato di autenticazione Client valido nell'archivio personale dell'utente, la connessione ha esito negativo (come dovrebbe) dopo che l'utente fa clic sulla **X** e se la **\ < TLSExtensions >**, **\ < EKUName >**, e **\ < EKUOID >** le sezioni esistono e contengono le informazioni corrette.<p>Il _un certificato non è possibile trovare che può essere usato con il protocollo di autenticazione Extensible._ viene visualizzato il messaggio di errore.

### In grado di eliminare il certificato dalla blade di connettività VPN

-   **Descrizione dell'errore.** Certificati nel blade di connettività VPN non possono essere eliminati.

-   **Possibile causa.** Il certificato è impostato su **primario**.

-   **Soluzioni possibili.** 

    1. Nel pannello connettività VPN, selezionare il certificato.
    2. In **primario**, selezionare **No** e fai clic su **Salva**.
    3. Nel pannello connettività VPN selezioni nuovamente il certificato.
    4. Fai clic su **Elimina**.


---
