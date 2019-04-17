---
title: Configurare le connessioni VPN Always On dei client Windows 10
description: In questo passaggio, puoi scoprire le opzioni ProfileXML e lo schema e configurare i computer client Windows 10 per comunicare con tale infrastruttura con una connessione VPN.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.date: 05/29/2018
ms.assetid: d165822d-b65c-40a2-b440-af495ad22f42
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.reviewer: deverette
ms.openlocfilehash: 6b383076686092e20448977bed3766f7d7d1c2b8
ms.sourcegitcommit: 4147e092d77d9458696e6686bccefe3788c90508
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/28/2019
ms.locfileid: "9031315"
---
# Passaggio 6. Configurare le connessioni VPN Always On client di Windows 10

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10


& #171;  [ **Precedente:** passaggio 5. Configurare le impostazioni di Firewall e DNS](vpn-deploy-dns-firewall.md)<br>
& #187; [ **Successivo:** passaggio 7. (Facoltativo) Accesso condizionale per la connettività VPN con Azure AD](../../ad-ca-vpn-connectivity-windows10.md)

In questo passaggio, puoi scoprire le opzioni ProfileXML e lo schema e configurare i computer client Windows 10 per comunicare con tale infrastruttura con una connessione VPN. 

È possibile configurare il client VPN Always On tramite PowerShell, SCCM o Intune. Tutti e tre richiedono un profilo VPN di XML per configurare le impostazioni VPN appropriate. La registrazione per le organizzazioni senza SCCM o Intune automazione di PowerShell è possibile.

>[!NOTE]
>Criteri di gruppo non includono i modelli amministrativi per configurare il client di Windows 10 remoto accesso VPN Always On.  Tuttavia, è possibile utilizzare gli script di accesso.

## Panoramica di ProfileXML

ProfileXML è un nodo URI all'interno del CSP VPNv2. Invece di configurare ogni nodo del CSP VPNv2 singolarmente, ad esempio di trigger, il routing elenchi e protocolli di autenticazione, questo nodo da utilizzare per configurare un client VPN di Windows 10 offrendo tutte le impostazioni come un singolo blocco XML a un singolo nodo CSP. Lo schema ProfileXML corrisponda allo schema dei nodi del CSP VPNv2 quasi identico, ma alcuni termini sono leggermente diversi.

Puoi usare ProfileXML in tutti i metodi di recapito che viene descritta questa distribuzione, incluso Windows PowerShell, System Center Configuration Manager e Intune. Esistono due modi per configurare il nodo del CSP VPNv2 ProfileXML in questa distribuzione:

- **OMA-DM**. Un modo consiste nell'usare un provider MDM usando OMA-DM, come illustrato in precedenza nella sezione [del CSP VPNv2](../always-on-vpn-technology-overview.md#vpnv2-csp-nodes). Con questo metodo, è possibile facilmente inserire markup XML di configurazione di profilo VPN il nodo ProfileXML CSP quando si utilizza Intune.

- **Strumentazione gestione Windows (WMI) - a - bridge CSP**. Il secondo metodo di configurazione del nodo ProfileXML CSP consiste nell'usare il bridge da WMI al provider, ovvero una classe WMI chiamato **MDM_VPNv2_01**, ovvero che possono accedere ai CSP VPNv2 e il nodo ProfileXML. Quando si crea una nuova istanza della classe WMI, WMI utilizza il CSP per creare il profilo VPN quando si utilizza System Center Configuration Manager e Windows PowerShell.

Anche se questi metodi di configurazione sono diversi, entrambi richiedono un profilo VPN XML formattato correttamente. Per usare l'impostazione CSP VPNv2 ProfileXML, puoi creare codice XML usando lo schema ProfileXML per configurare il tag necessari per lo scenario di distribuzione semplice. Per altre informazioni, vedi [ProfileXML XSD](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/vpnv2-profile-xsd).

Di seguito è riportata ogni le impostazioni necessarie e del tag ProfileXML corrispondente. Configurare ogni impostazione in un tag specifico all'interno dello schema ProfileXML e non tutti gli elementi si trovano sotto il profilo nativo. Per il posizionamento dei tag aggiuntive, vedere lo schema ProfileXML.

[!INCLUDE [important-lower-case-true-include](../../../includes/important-lower-case-true-include.md)]

**Tipo di connessione:** IKEv2 nativo

ProfileXML elemento:

    <NativeProtocolType>IKEv2</NativeProtocolType>

**Routing:** Dividere tunneling

ProfileXML elemento:

    <RoutingPolicyType>SplitTunnel</RoutingPolicyType>

**La risoluzione dei nomi:** Suffisso DNS e Domain Name Information List

ProfileXML elementi:

    <DomainNameInformation>
    <DomainName>.corp.contoso.com</DomainName>
    <DnsServers>10.10.1.10,10.10.1.50</DnsServers>
    </DomainNameInformation>
    
    <DnsSuffix>corp.contoso.com</DnsSuffix>


**Attivando:** Rilevamento della rete sempre On e attendibili

ProfileXML elementi:

    <AlwaysOn>true</AlwaysOn>
    <TrustedNetworkDetection>corp.contoso.com</TrustedNetworkDetection>


**Autenticazione:** PEAP-TLS con i certificati utente protetta da TPM

ProfileXML elementi:

    <Authentication>
    <UserMethod>Eap</UserMethod>
    <Eap>
    <Configuration>...</Configuration>
    </Eap>
    </Authentication>

È possibile utilizzare i tag semplice per configurare alcuni metodi di autenticazione VPN. Tuttavia, EAP e PEAP sono più complesse. Il modo più semplice per creare i tag XML è configurare un client VPN con le relative impostazioni EAP e quindi esportare la configurazione in formato XML. 

Per ulteriori informazioni sulle impostazioni EAP, vedi [configurazione EAP](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/eap-configuration). 

## <a name="bkmk_profile"></a>Creare manualmente un profilo di connessione del modello

In questo passaggio, usare \(PEAP\) Protected Extensible Authentication Protocol per proteggere la comunicazione tra il client e server. A differenza di un semplice nome utente e una password, questa connessione richiede una sezione EAPConfiguration univoca nel profilo della VPN a funzionare. 

Invece che descrivono come creare i tag XML da zero, utilizzare le impostazioni in Windows per creare un modello di profilo VPN. Dopo aver creato il modello di profilo VPN, utilizzare Windows PowerShell per utilizzare la parte EAPConfiguration da tale modello per creare il ProfileXML finale che distribuire in un secondo momento nella distribuzione.

### Registrare le impostazioni di certificati dei criteri di rete

Prima di creare il modello, Prendi nota il nome host o il nome di dominio completo (FQDN) del server dei criteri di rete dal certificato del server e il nome della CA che ha emesso il certificato.

**Procedura:**

1.  Il server dei criteri di rete, Apri il Server dei criteri di rete.

2.  Nella console di criteri di rete, i criteri, fai clic su **Criteri di rete**.

3.  Fai **le connessioni di rete privata virtuale \(VPN\)** e fare clic su **proprietà**.

4.  Fare clic sulla scheda **vincoli** e fai clic su **Metodi di autenticazione**.

5.  In tipi EAP, fai clic su **Microsoft: Protected EAP (PEAP)**, fare clic su **Modifica**.

6.  Registrare i valori per **certificato rilasciato a** e **dell'autorità di certificazione**.<p>Usare questi valori nella configurazione del modello VPN futura. Ad esempio, se il nome FQDN del server è nps01.corp.contoso.com e il nome host è NPS01, il nome del certificato si basa il nome FQDN o DNS del server - ad esempio, nps01.corp.contoso.com.

7.  Annullare la finestra di dialogo Modifica proprietà PEAP.

8.  Annullare la finestra di dialogo Proprietà le connessioni di rete privata virtuale (VPN).

9.  Chiudere il Server dei criteri di rete.

>[!NOTE]
>Se hai più server dei criteri di rete, completa questi passaggi su ciascuno di essi in modo che il profilo VPN possa verificare ognuno di essi deve essere utilizzati.

### Configurare il modello di profilo VPN in un computer client aggiunto al domain \

Ora che hai le informazioni necessarie di configurare il modello di profilo VPN in un computer client appartenenti al dominio. Il tipo di account utente che usi \ (vale a dire utente standard o administrator\) per questa parte del processo non è importante. 

Tuttavia, se il computer non è stato riavviato dopo la registrazione automatica certificato di configurazione, farlo prima di configurare il modello di connessione VPN in modo da garantire un certificato utilizzabile registrato su di esso.

>[!NOTE]
>Non è possibile aggiungere manualmente le proprietà avanzate di VPN, ad esempio regole NRPT, sempre attiva, Trusted rilevamento della rete e così via. Nel passaggio successivo, creare una connessione VPN di test per verificare la configurazione del server VPN e che è possibile stabilire una connessione VPN al server.

**Creare manualmente un singolo test connessione VPN**

1.  Accedi a un computer client appartenenti al dominio in qualità di membro del gruppo di **Utenti VPN** .

2.  Nel menu Start, digita **VPN**e premi INVIO.

3.  Nel riquadro dei dettagli, fai clic su **Aggiungi una connessione VPN**.

4.  Nell'elenco Provider VPN, fai clic su **Windows (predefinito)**.

5.  Nel nome della connessione, tipo di **modello**.

6.  Nome del Server o l'indirizzo, digita l' **esterno** nome FQDN del server VPN \ (ad esempio,**vpn.contoso.com**\).

7.  Fai clic su **Salva**.

8.  In impostazioni correlate, fai clic su **Modifica delle opzioni di scheda**.

9.  Destro del mouse sul **modello**e fare clic su **proprietà**.

10. Nella scheda **sicurezza** , nel **Tipo di VPN**, fai clic su **IKEv2**.

11. Nella crittografia dei dati, fai clic su **massimo livello di crittografia**.

12. Fai clic su **Usa Extensible Authentication Protocol (EAP)**; quindi, in **Uso EAP Extensible Authentication Protocol ()**, fai clic su **Microsoft: protetti PEAP (EAP) (crittografia abilitata)**.

13. Fai clic su **proprietà** per aprire la finestra di dialogo Proprietà PEAP e completare la procedura seguente:

    a. Nella casella **Connetti a questi server** , digita il nome del server dei criteri di rete che hai recuperato dalle impostazioni di autenticazione del server dei criteri di rete in precedenza in questa sezione (ad esempio, NPS01).

    >[!NOTE]
    >Digitare il nome del server deve corrispondere al nome nel certificato. Recuperare il nome in precedenza in questa sezione. Se il nome non corrispondono, la connessione avrà esito negativo, che informa che "la connessione è stata bloccata a causa di un criterio configurato nel server RAS/VPN."

    b.  In Autorità di certificazione radice attendibili, seleziona la CA che ha emesso il certificato del server dei criteri di rete (ad esempio, contoso-CA) principale.

    c.  Nelle notifiche prima della connessione, fai clic su **non chiedere utente per autorizzare i nuovi server o autorità di certificazione attendibili**.

    d.  Nel metodo di autenticazione seleziona, fai clic su **Smart Card o un altro certificato**e fare clic su **Configura**. Si apre la Smart Card o altri finestra di dialogo di proprietà del certificato.

    e.  Fai clic su **utilizza un certificato su questo computer**.

    f.  Nella finestra Connetti a questi server, Immetti il nome del server dei criteri di rete che hai recuperato dalle impostazioni di autenticazione del server dei criteri di rete nei passaggi precedenti.

    g.  In Autorità di certificazione radice attendibili, seleziona la CA che ha emesso il certificato del server dei criteri di rete principale.

    h.  Seleziona il **non richiedere utente di autorizzare nuovi server o autorità di certificazione attendibili** casella di controllo.

    i.  Fai clic su **OK** per chiudere la Smart Card o altri nella finestra di dialogo di proprietà del certificato.

    j.  Fai clic su **OK** per chiudere la finestra di dialogo Proprietà PEAP.

10. Fai clic su **OK** per chiudere la finestra di dialogo le proprietà dei modelli.

11. Chiudere la finestra di connessioni di rete.

12. In impostazioni, testare la connessione VPN facendo clic sul **modello**e facendo clic su **Connetti**.

>[!IMPORTANT]
>Assicurati che il modello di connessione VPN al server VPN esito positivo. In questo modo garantisce che le impostazioni EAP siano corrette prima di utilizzarli nell'esempio seguente. Devi collegare almeno una volta prima di continuare; in caso contrario, il profilo non conterrà tutte le informazioni necessarie per connettersi alla VPN.

## <a name="bkmk_ProfileXML"></a>Creare i file di configurazione ProfileXML

Prima di completare questa sezione, assicurati che hai creato progettata e testata il modello di connessione VPN descritto nella sezione [creare manualmente un profilo di connessione del modello](#bkmk_profile) . Verifica la connessione VPN è necessario per assicurarsi che il profilo contiene tutte le informazioni necessarie per connettersi alla VPN.

Lo script di Windows PowerShell nel listato 1 crea due file sul desktop, che contengono entrambi i tag **EAPConfiguration** in base al profilo di connessione modello creato in precedenza:

-   **VPN_Profile.Xml.** Questo file contiene i tag XML necessario per configurare il nodo ProfileXML CSP VPNv2. Usare questo file con servizi OMA-DM compatibili con MDM, come Intune.

-   **VPN_Profile.ps1.** Questo file è uno script di Windows PowerShell che è possibile eseguire nei computer client per configurare il nodo ProfileXML nel CSP VPNv2. È inoltre possibile configurare il provider CSP tramite la distribuzione di questo script tramite System Center Configuration Manager. È Impossibile eseguire lo script in una sessione Desktop remoto, tra cui una sessione avanzata Hyper-V.

>[!IMPORTANT]
>I comandi di esempio riportato di seguito richiedono Build di Windows 10 1607 o successiva.

**Creare VPN_Profile.xml e VPN_Proflie.ps1**

1. Accedi al computer client appartenenti al dominio che contiene il modello di profilo VPN con lo stesso utente conto che la sezione "Creare manualmente un profilo di connessione modello" descritto.

2. Incollare listato 1 in ambiente di scripting integrata di Windows PowerShell, \(ISE\) e personalizzare i parametri descritti nei commenti. Questi sono $Template, $ProfileName, $Servers, $DnsSuffix, $DomainName, $TrustedNetwork e $DNSServers. Una descrizione completa di ogni impostazione è nei commenti.

3.  Esegui lo script per generare **VPN_Profile.xml** e **VPN_Profile.ps1** sul desktop.

#### Presentazione 1. Informazioni sulle MakeProfile.ps1

Questa sezione illustra il codice di esempio che puoi usare per ottenere informazioni su come creare un profilo VPN, in modo specifico per la configurazione ProfileXML nel CSP VPNv2.

Dopo che è assemblare uno script da questo codice di esempio e di eseguire lo script, lo script genera due file: VPN_Profile.xml e VPN_Profile.ps1. Utilizzare VPN_Profile.xml per configurare ProfileXML in OMA-DM conformi servizi MDM, ad esempio Microsoft Intune.

Utilizzare lo script **VPN_Profile.ps1** in Windows PowerShell o System Center Configuration Manager per configurare ProfileXML nel desktop di Windows 10.

>[!NOTE]
>Per visualizzare lo script di esempio completo, vedi la sezione [MakeProfile.ps1 Full Script](#bkmk_fullscript).

#### Parameters

Configurare i parametri seguenti:

**$Template**. Il nome del modello da cui recuperare la configurazione EAP.

**$ProfileName**. Identificatore univoco alfanumerici per il profilo. Il nome del profilo non deve includere una barra (/). Se il nome del profilo ha una quantità di spazio o altri caratteri alfanumerici, deve essere codificata in modo corretto in base allo standard di codifica URL.

**$Servers**. Indirizzo IP instradabile o pubblico o il nome DNS per il gateway VPN. Può puntare a esterno indirizzo IP del gateway o un indirizzo IP virtuale per una server farm. Esempi, 208.147.66.130 o vpn.contoso.com.

**$DnsSuffix**. Specifica che una o più virgole da virgole di suffissi DNS. La prima nell'elenco viene anche utilizzata come suffisso DNS specifico della connessione per l'interfaccia VPN. L'intero elenco verrà aggiunto anche in SuffixSearchList.

**$DomainName**. Usato per indicare lo spazio dei nomi a cui si applica il criterio. Quando viene inviata una query di nome, il client DNS confronta il nome della query per tutti gli spazi dei nomi in /domainnameinformationlist per trovare una corrispondenza. Questo parametro può essere uno dei tipi seguenti:

- Nome di dominio completo - nome di dominio completo
- Suffisso - un suffisso del dominio che verrà aggiunti alla query shortname per la risoluzione DNS. Per specificare un suffisso, anteporre un periodo \ (.) per il suffisso DNS.

**$DNSServers**. Elenco di IP di Server DNS delimitati da virgole di indirizzi da usare per lo spazio dei nomi.

**$TrustedNetwork**. Stringa delimitato da virgole per identificare la rete attendibile. VPN non si connetterà automaticamente quando l'utente è sulla rete wireless aziendale in cui le risorse protette sono direttamente accessibili sul dispositivo.

Sono i seguenti valori di esempio per i parametri usati in comandi riportati di seguito. Assicurati che modificare questi valori per il tuo ambiente.

    $TemplateName = 'Template'
    $ProfileName = 'Contoso AlwaysOn VPN'
    $Servers = 'vpn.contoso.com'
    $DnsSuffix = 'corp.contoso.com'
    $DomainName = '.corp.contoso.com'
    $DNSServers = '10.10.0.2,10.10.0.3'
    $TrustedNetwork = 'corp.contoso.com'


### Preparare e creare XML del profilo

I seguenti comandi di esempio ottenere le impostazioni EAP tramite il modello di profilo:


    $Connection = Get-VpnConnection -Name $TemplateName
    if(!$Connection)
    {
    $Message = "Unable to get $TemplateName connection profile: $_"
    Write-Host "$Message"
    exit
    }
    $EAPSettings= $Connection.EapConfigXmlStream.InnerXml


### Creare il file XML del profilo

[!INCLUDE [important-lower-case-true-include](../../../includes/important-lower-case-true-include.md)]

    $ProfileXML =
    '<VPNProfile>
      <DnsSuffix>' + $DnsSuffix + '</DnsSuffix>
      <NativeProfile>
    <Servers>' + $Servers + '</Servers>
    <NativeProtocolType>IKEv2</NativeProtocolType>
    <Authentication>
      <UserMethod>Eap</UserMethod>
      <Eap>
       <Configuration>
     '+ $EAPSettings + '
       </Configuration>
      </Eap>
    </Authentication>
    <RoutingPolicyType>SplitTunnel</RoutingPolicyType>
      </NativeProfile>
     <AlwaysOn>true</AlwaysOn>
     <RememberCredentials>true</RememberCredentials>
     <TrustedNetworkDetection>' + $TrustedNetwork + '</TrustedNetworkDetection>
      <DomainNameInformation>
    <DomainName>' + $DomainName + '</DomainName>
    <DnsServers>' + $DNSServers + '</DnsServers>
    </DomainNameInformation>
    </VPNProfile>'


### Output VPN_Profile.xml per Intune

Per salvare il file XML del profilo, è possibile utilizzare il comando di esempio seguente:

    $ProfileXML | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.xml')

### VPN_Profile.ps1 output per il desktop e System Center Configuration Manager

Esempio di codice seguente consente di configurare una connessione VPN IKEv2 di AlwaysOn tramite il nodo ProfileXML in CSP VPNv2.

È possibile utilizzare questo script nel desktop di Windows 10 o in System Center Configuration Manager.

### Definire i parametri di profilo VPN chiavi

    $Script = '$ProfileName = ''' + $ProfileName + '''
    $ProfileNameEscaped = $ProfileName -replace ' ', '%20'

## Definire ProfileXML VPN

    $ProfileXML = ''' + $ProfileXML + '''
    
### Caratteri speciali di escape nel profilo

    $ProfileXML = $ProfileXML -replace '<', '&lt;'
    $ProfileXML = $ProfileXML -replace '>', '&gt;'
    $ProfileXML = $ProfileXML -replace '"', '&quot;'
    
### Definire le proprietà di Bridge WMI al provider

    $nodeCSPURI = "./Vendor/MSFT/VPNv2"
    $namespaceName = "root\cimv2\mdm\dmmap"
    $className = "MDM_VPNv2_01"

### Determinare SID utente per il profilo VPN:

    try
    {
    $username = Gwmi -Class Win32_ComputerSystem | select username
    $objuser = New-Object System.Security.Principal.NTAccount($username.username)
    $sid = $objuser.Translate([System.Security.Principal.SecurityIdentifier])
    $SidValue = $sid.Value
    $Message = "User SID is $SidValue."
    Write-Host "$Message"
    }
    catch [Exception]
    {
    $Message = "Unable to get user SID. User may be logged on over Remote Desktop: $_"
    Write-Host "$Message"
    exit
    }
    

### Definire la sessione WMI:

    $session = New-CimSession
    $options = New-Object Microsoft.Management.Infrastructure.Options.CimOperationOptions
    $options.SetCustomOption("PolicyPlatformContext_PrincipalContext_Type", "PolicyPlatform_UserContext", $false)
    $options.SetCustomOption("PolicyPlatformContext_PrincipalContext_Id", "$SidValue", $false)


### Rilevare ed eliminare il profilo VPN precedente:

    try
    {
        $deleteInstances = $session.EnumerateInstances($namespaceName, $className, $options)
        foreach ($deleteInstance in $deleteInstances)
        {
            $InstanceId = $deleteInstance.InstanceID
            if ("$InstanceId" -eq "$ProfileNameEscaped")
            {
                $session.DeleteInstance($namespaceName, $deleteInstance, $options)
                $Message = "Removed $ProfileName profile $InstanceId"
                Write-Host "$Message"
            } else {
                $Message = "Ignoring existing VPN profile $InstanceId"
                Write-Host "$Message"
            }
        }
    }
    catch [Exception]
    {
        $Message = "Unable to remove existing outdated instance(s) of $ProfileName profile: $_"
        Write-Host "$Message"
        exit
    }
    

### Creare il profilo VPN:

    try
    {
        $newInstance = New-Object Microsoft.Management.Infrastructure.CimInstance $className, $namespaceName
        $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("ParentID", "$nodeCSPURI", "String", "Key")
        $newInstance.CimInstanceProperties.Add($property)
        $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("InstanceID", "$ProfileNameEscaped", "String", "Key")
        $newInstance.CimInstanceProperties.Add($property)
        $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("ProfileXML", "$ProfileXML", "String", "Property")
        $newInstance.CimInstanceProperties.Add($property)
        $session.CreateInstance($namespaceName, $newInstance, $options)
        $Message = "Created $ProfileName profile."


        Write-Host "$Message"
    }
    catch [Exception]
    {
    $Message = "Unable to create $ProfileName profile: $_"
    Write-Host "$Message"
    exit
    }
    
    $Message = "Script Complete"
    Write-Host "$Message"'

### Salva il file XML del profilo

    $Script | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.ps1')
    
    $Message = "Successfully created VPN_Profile.xml and VPN_Profile.ps1 on the desktop."
    Write-Host "$Message"

## <a name="bkmk_fullscript"></a>Script completo MakeProfile.ps1

La maggior parte degli esempi di usano il cmdlet Set-WmiInstance Windows PowerShell per inserire ProfileXML in una nuova istanza della classe WMI MDM_VPNv2_01. 

Tuttavia, ciò non funziona in System Center Configuration Manager perché non è possibile eseguire il pacchetto nel contesto degli utenti finali. Di conseguenza, questo script Usa il modello di informazioni comuni per creare una sessione WMI nel contesto dell'utente e quindi crea una nuova istanza della classe WMI MDM_VPNv2_01 in tale sessione. Questa classe WMI Usa il bridge da WMI al provider per configurare il CSP VPNv2. Di conseguenza, aggiungendo l'istanza della classe, configurare il CSP. 

>[!IMPORTANT]
>Bridge da WMI al provider richiede diritti di amministratore locale, in base alla progettazione. Per distribuire per i profili VPN utente dovrebbe essere uso SCCM o sistema MDM.

>[!NOTE]
>Lo script VPN_Profile.ps1 utilizza il SID dell'utente corrente per identificare il contesto dell'utente. Poiché non SID è disponibile in una sessione di Desktop remoto, lo script non funziona in una sessione di Desktop remoto. Analogamente, non funziona in una sessione avanzata Hyper-V. Se stai testando un Remote Access VPN Always On in macchine virtuali, disabilitare sessione avanzata in macchine virtuali client prima di eseguire questo script.

Lo script di esempio seguente include tutti gli esempi di codice nelle sezioni precedenti. Assicurati che modificare i valori di esempio per i valori appropriati per l'ambiente.
    
   ```XML
    $TemplateName = 'Template'
    $ProfileName = 'Contoso AlwaysOn VPN'
    $Servers = 'vpn.contoso.com'
    $DnsSuffix = 'corp.contoso.com'
    $DomainName = '.corp.contoso.com'
    $DNSServers = '10.10.0.2,10.10.0.3'
    $TrustedNetwork = 'corp.contoso.com'

    
    $Connection = Get-VpnConnection -Name $TemplateName
    if(!$Connection)
    {
    $Message = "Unable to get $TemplateName connection profile: $_"
    Write-Host "$Message"
    exit
    }
    $EAPSettings= $Connection.EapConfigXmlStream.InnerXml
    
    $ProfileXML =
    '<VPNProfile>
      <DnsSuffix>' + $DnsSuffix + '</DnsSuffix>
      <NativeProfile>
    <Servers>' + $Servers + '</Servers>
    <NativeProtocolType>IKEv2</NativeProtocolType>
    <Authentication>
      <UserMethod>Eap</UserMethod>
      <Eap>
       <Configuration>
     '+ $EAPSettings + '
       </Configuration>
      </Eap>
    </Authentication>
    <RoutingPolicyType>SplitTunnel</RoutingPolicyType>
      </NativeProfile>
    <AlwaysOn>true</AlwaysOn>
    <RememberCredentials>true</RememberCredentials>
    <TrustedNetworkDetection>' + $TrustedNetwork + '</TrustedNetworkDetection>
      <DomainNameInformation>
    <DomainName>' + $DomainName + '</DomainName>
    <DnsServers>' + $DNSServers + '</DnsServers>
    </DomainNameInformation>
    </VPNProfile>'
    
    $ProfileXML | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.xml')
    
    $Script = '$ProfileName = ''' + $ProfileName + '''
    $ProfileNameEscaped = $ProfileName -replace ' ', '%20'
    
    $ProfileXML = ''' + $ProfileXML + '''
    
    $ProfileXML = $ProfileXML -replace '<', '&lt;'
    $ProfileXML = $ProfileXML -replace '>', '&gt;'
    $ProfileXML = $ProfileXML -replace '"', '&quot;'
    
    $nodeCSPURI = "./Vendor/MSFT/VPNv2"
    $namespaceName = "root\cimv2\mdm\dmmap"
    $className = "MDM_VPNv2_01"
    
    try
    {
    $username = Gwmi -Class Win32_ComputerSystem | select username
    $objuser = New-Object System.Security.Principal.NTAccount($username.username)
    $sid = $objuser.Translate([System.Security.Principal.SecurityIdentifier])
    $SidValue = $sid.Value
    $Message = "User SID is $SidValue."
    Write-Host "$Message"
    }
    catch [Exception]
    {
    $Message = "Unable to get user SID. User may be logged on over Remote Desktop: $_"
    Write-Host "$Message"
    exit
    }
    
    $session = New-CimSession
    $options = New-Object Microsoft.Management.Infrastructure.Options.CimOperationOptions
    $options.SetCustomOption("PolicyPlatformContext_PrincipalContext_Type", "PolicyPlatform_UserContext", $false)
    $options.SetCustomOption("PolicyPlatformContext_PrincipalContext_Id", "$SidValue", $false)
    
        try
    {
        $deleteInstances = $session.EnumerateInstances($namespaceName, $className, $options)
        foreach ($deleteInstance in $deleteInstances)
        {
            $InstanceId = $deleteInstance.InstanceID
            if ("$InstanceId" -eq "$ProfileNameEscaped")
            {
                $session.DeleteInstance($namespaceName, $deleteInstance, $options)
                $Message = "Removed $ProfileName profile $InstanceId"
                Write-Host "$Message"
            } else {
                $Message = "Ignoring existing VPN profile $InstanceId"
                Write-Host "$Message"
            }
        }
    }
    catch [Exception]
    {
        $Message = "Unable to remove existing outdated instance(s) of $ProfileName profile: $_"
        Write-Host "$Message"
        exit
    }
    
    try
    {
        $newInstance = New-Object Microsoft.Management.Infrastructure.CimInstance $className, $namespaceName
        $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("ParentID", "$nodeCSPURI", "String", "Key")
        $newInstance.CimInstanceProperties.Add($property)
        $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("InstanceID", "$ProfileNameEscaped", "String", "Key")
        $newInstance.CimInstanceProperties.Add($property)
        $property = [Microsoft.Management.Infrastructure.CimProperty]::Create("ProfileXML", "$ProfileXML", "String", "Property")
        $newInstance.CimInstanceProperties.Add($property)
        $session.CreateInstance($namespaceName, $newInstance, $options)
        $Message = "Created $ProfileName profile."

        Write-Host "$Message"
    }
    catch [Exception]
    {
        $Message = "Unable to create $ProfileName profile: $_"
        Write-Host "$Message"
        exit
    }
    
    $Message = "Script Complete"
    Write-Host "$Message"'
    
    $Script | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.ps1')
    
    $Message = "Successfully created VPN_Profile.xml and VPN_Profile.ps1 on the desktop."
    Write-Host "$Message"
   ```

## Configurare il client VPN tramite Windows PowerShell

Per configurare il CSP VPNv2 in un computer client Windows 10, eseguire lo script VPN_Profile.ps1 Windows PowerShell che hai creato nella sezione [Crea XML del profilo](#create-the-profile-xml) . Apri Windows PowerShell come amministratore. in caso contrario, riceverai un errore che indica, _accesso negato_.

Dopo aver eseguito VPN_Profile.ps1 per configurare il profilo VPN, è possibile verificare in qualsiasi momento che sia riuscita eseguendo il comando seguente in Windows PowerShell ISE:

    Get-WmiObject -Namespace root\cimv2\mdm\dmmap -Class MDM_VPNv2_01

**Risultati corretti dal cmdlet Get-WmiObject**


    __GENUS : 2
    __CLASS : MDM_VPNv2_01
    __SUPERCLASS:
    __DYNASTY   : MDM_VPNv2_01
    __RELPATH   : MDM_VPNv2_01.InstanceID="Contoso%20AlwaysOn%20VPN",ParentID
      ="./Vendor/MSFT/VPNv2"
    __PROPERTY_COUNT: 10
    __DERIVATION: {}
    __SERVER: WIN01
    __NAMESPACE : root\cimv2\mdm\dmmap
    __PATH  : \\WIN01\root\cimv2\mdm\dmmap:MDM_VPNv2_01.InstanceID="Conto
      so%20AlwaysOn%20VPN",ParentID="./Vendor/MSFT/VPNv2"
    AlwaysOn: True
    ByPassForLocal  :
    DnsSuffix   : corp.contoso.com
    EdpModeId   :
    InstanceID  : Contoso%20AlwaysOn%20VPN
    LockDown:
    ParentID: ./Vendor/MSFT/VPNv2
    ProfileXML  : <VPNProfile><RememberCredentials>true</RememberCredentials>
      <AlwaysOn>true</AlwaysOn><DnsSuffix>corp.contoso.com</DnsSu
      ffix><TrustedNetworkDetection>corp.contoso.com</TrustedNetw
      orkDetection><NativeProfile><Servers>vpn.contoso.com;vpn.co
      ntoso.com</Servers><RoutingPolicyType>SplitTunnel</RoutingP
      olicyType><NativeProtocolType>Ikev2</NativeProtocolType><Au
      thentication><UserMethod>Eap</UserMethod><MachineMethod>Eap
      </MachineMethod><Eap><Configuration><EapHostConfig xmlns="h
      ttp://www.microsoft.com/provisioning/EapHostConfig"><EapMet
      hod><Type xmlns="https://www.microsoft.com/provisioning/EapC
      ommon">25</Type><VendorId xmlns="https://www.microsoft.com/p
      rovisioning/EapCommon">0</VendorId><VendorType xmlns="http:
      //www.microsoft.com/provisioning/EapCommon">0</VendorType><
      AuthorId xmlns="https://www.microsoft.com/provisioning/EapCo
      mmon">0</AuthorId></EapMethod><Config xmlns="https://www.mic
      rosoft.com/provisioning/EapHostConfig"><Eap xmlns="https://w
      ww.microsoft.com/provisioning/BaseEapConnectionPropertiesV1
      "><Type>25</Type><EapType xmlns="https://www.microsoft.com/p
      rovisioning/MsPeapConnectionPropertiesV1"><ServerValidation
      ><DisableUserPromptForServerValidation>true</DisableUserPro
      mptForServerValidation><ServerNames>NPS</ServerNames><Trust
      edRootCA>3f 07 88 e8 ac 00 32 e4 06 3f 30 f8 db 74 25 e1
      2e 5b 84 d1 </TrustedRootCA></ServerValidation><FastReconne
      ct>true</FastReconnect><InnerEapOptional>false</InnerEapOpt
      ional><Eap xmlns="https://www.microsoft.com/provisioning/Bas
      eEapConnectionPropertiesV1"><Type>13</Type><EapType xmlns="
      https://www.microsoft.com/provisioning/EapTlsConnectionPrope
      rtiesV1"><CredentialsSource><CertificateStore><SimpleCertSe
      lection>true</SimpleCertSelection></CertificateStore></Cred
      entialsSource><ServerValidation><DisableUserPromptForServer
      Validation>true</DisableUserPromptForServerValidation><Serv
      erNames>NPS</ServerNames><TrustedRootCA>3f 07 88 e8 ac 00
      32 e4 06 3f 30 f8 db 74 25 e1 2e 5b 84 d1 </TrustedRootCA><
      /ServerValidation><DifferentUsername>false</DifferentUserna
      me><PerformServerValidation xmlns="https://www.microsoft.com
      /provisioning/EapTlsConnectionPropertiesV2">true</PerformSe
      rverValidation><AcceptServerName xmlns="https://www.microsof
      t.com/provisioning/EapTlsConnectionPropertiesV2">true</Acce
      ptServerName></EapType></Eap><EnableQuarantineChecks>false<
      /EnableQuarantineChecks><RequireCryptoBinding>false</Requir
      eCryptoBinding><PeapExtensions><PerformServerValidation xml
      ns="https://www.microsoft.com/provisioning/MsPeapConnectionP
      ropertiesV2">true</PerformServerValidation><AcceptServerNam
      e xmlns="https://www.microsoft.com/provisioning/MsPeapConnec
      tionPropertiesV2">true</AcceptServerName></PeapExtensions><
      /EapType></Eap></Config></EapHostConfig></Configuration></E
      ap></Authentication></NativeProfile><DomainNameInformation>
      <DomainName>corp.contoso.com</DomainName><DnsServers>10.10.
      0.2,10.10.0.3</DnsServers><AutoTrigger>true</AutoTrigger></
      DomainNameInformation></VPNProfile>
    RememberCredentials : True
    TrustedNetworkDetection : corp.contoso.com
    PSComputerName  : WIN01

La configurazione ProfileXML deve essere corretta nella struttura, il controllo ortografico, configurazione e talvolta maiuscole e minuscole. Se viene visualizzata un'altra nella struttura di presentazione 1, il markup ProfileXML probabilmente contiene un errore.

Se hai bisogno di risolvere i problemi di markup, è più semplice per mettere in pratica in un editor XML rispetto alla risoluzione dei problemi in Windows PowerShell ISE. In entrambi i casi, inizia con la versione più semplice del profilo e aggiungere nuovamente componenti uno per volta finché il problema persiste.

## <a name="vpn-deploy-client-sccm"></a>Configurare il client VPN tramite System Center Configuration Manager

In System Center Configuration Manager, puoi distribuire i profili VPN tramite il nodo ProfileXML CSP, proprio come hai fatto in Windows PowerShell. In questo caso, usare lo script VPN_Profile.ps1 Windows PowerShell che hai creato nella sezione [creare i file di configurazione ProfileXML](#bkmk_ProfileXML).

Per usare System Center Configuration Manager per distribuire un profilo Remote Access VPN Always On per i computer client Windows 10, è necessario avviare creando un gruppo di computer o utenti a cui distribuire il profilo. In questo scenario, crea un gruppo di utenti per distribuire lo script di configurazione.

### Creare un gruppo di utenti

1.  Nella console di Configuration Manager, aprire le raccolte Compliance\\User e gli asset.

2.  Sulla barra multifunzione **Home** , nel gruppo di **creare** , clic **Crea raccolta utente**.

3.  Nella pagina generale, completa i passaggi seguenti:

    a. Nel **nome**, digita **Gli utenti della VPN**.

    b. Fare clic su **Sfoglia**, fai clic su **Tutti gli utenti** e fai clic su **OK**.

    c. Fai clic su **Avanti**.

4.  Nella pagina regole di appartenenza, completa i passaggi seguenti:

    a.  Nelle **regole di appartenenza**, fai clic su **Aggiungi regola**e fai clic su **Regola diretta**. In questo esempio, aggiungere singoli utenti al raccolta utente. Tuttavia, è possibile utilizzare una regola di query per aggiungere utenti a questa raccolta in modo dinamico per una distribuzione su larga scala.

    b.  Nella pagina **iniziale** , fai clic su **Avanti**.

    c.  La ricerca per la pagina delle risorse, nel **valore**, digitare il nome dell'utente da aggiungere. Il nome di risorsa include il dominio dell'utente. Per includere i risultati in base a una corrispondenza parziale, Inserisci il **%** carattere alle estremità del criterio di ricerca. Ad esempio, per trovare tutti gli utenti che contiene la stringa "lori", digitare **% lori %**. Fai clic su **Avanti**.

    d.  Nella pagina Seleziona risorse, seleziona gli utenti che vuoi aggiungere al gruppo e fare clic su **Avanti**.

    e.  Nella pagina di riepilogo, fare clic su **Avanti**.

    f.  Nella pagina Completamento fai clic su **Chiudi**.

6.  Indietro nella pagina regole di appartenenza della creazione guidata raccolta dell'utente, fare clic su **Avanti**.

7.  Nella pagina di riepilogo, fare clic su **Avanti**.

8.  Nella pagina Completamento fai clic su **Chiudi**.

Dopo aver creato il gruppo di utenti per ricevere il profilo VPN, è possibile creare un pacchetto e programma per distribuire lo script di configurazione di Windows PowerShell che hai creato nella sezione [creare i file di configurazione ProfileXML](#bkmk_ProfileXML).

### Creare un pacchetto che contiene lo script di configurazione ProfileXML

1.  Ospitare lo script VPN_Profile.ps1 in una condivisione di rete che l'account di computer server del sito può accedere.

2.  Nella console di Configuration Manager, Apri **Applicazioni\\pacchetti Library\\Application Software**.

3.  Sulla barra multifunzione **Home** , nel gruppo di **creare** , clic **Crea pacchetto** per avviare la creazione guidata pacchetto e programma.

4.  Nella pagina pacchetto, completa i passaggi seguenti:

    a. Nel **nome**, digita **Windows 10 sempre nel profilo VPN**.

    b. Seleziona la casella di controllo **il pacchetto contiene i file di origine** e fare clic su **Sfoglia**.

    c. Nella finestra di dialogo Imposta cartella di origine, fare clic su **Sfoglia**, seleziona la condivisione di file che contiene VPN_Profile.ps1 e fai clic su **OK**.<p>Assicurati di che selezionare un percorso di rete, non un percorso locale. In altre parole, il percorso dovrebbe essere simile al seguente *\\fileserver\\vpnscript*, non *c:\\vpnscript*.

1.  Fai clic su **Avanti**.

2.  Nella pagina tipo di programma, fare clic su **Avanti**.

3.  Nella pagina programma Standard, completa i passaggi seguenti:

    a.  Nel **nome**, digita **Script del profilo VPN**.

    b.  Nella **riga di comando**, digita **PowerShell.exe - ExecutionPolicy ignorare - File "VPN_Profile.ps1"**.

    c.  In **modalità di esecuzione**, fai clic su **Esegui con diritti amministrativi**.

    d.  Fai clic su **Avanti**.

4.  Nella pagina requisiti, completa i passaggi seguenti:

    a.  Seleziona **questo programma è possibile eseguire solo su piattaforme specificate**.

    b.  Seleziona le caselle di controllo **tutti Windows 10 (32-bit)** e **tutti i Windows 10 (64 bit)** .

    c.  Nello **spazio su disco stimato**, digitare **1**.

    d.  In **fase di esecuzione (minuti) consentito massimo**, digitare **15**.

    e.  Fai clic su **Avanti**.

5.  Nella pagina di riepilogo, fare clic su **Avanti**.

6.  Nella pagina Completamento fai clic su **Chiudi**.

Con il pacchetto e programma creato, è necessario per la distribuzione al gruppo **Utenti di VPN** .

### Distribuire lo script di configurazione ProfileXML

1.  Nella console di Configuration Manager, Apri applicazioni\\pacchetti Library\\Application Software.

2.  In **pacchetti**, fai clic su **Windows 10 sempre nel profilo VPN**.

3.  Nella scheda **programmi** , nella parte inferiore del riquadro dei dettagli fai **Script del profilo VPN**, fare clic su **proprietà**e la procedura seguente:

    a.  Nella scheda **Avanzate** , in **quando questo programma viene assegnato a un computer**, fai clic su **una sola volta per ogni utente che esegue l'accesso**.

    b.  Scegli **OK**.

4.  Il pulsante destro **Script del profilo VPN** e fai clic su **Distribuisci** per avviare la distribuzione guidata del Software.

5.  Nella pagina generale, completa i passaggi seguenti:

    a.  Accanto a **raccolta**, fare clic su **Sfoglia**.

    b.  Nell'elenco di **Tipi di raccolta** (da in alto a sinistra), fai clic su **Insiemi di utenti**.

    c.  Fai clic sugli **Utenti della VPN**e fai clic su **OK**.

    d.  Fai clic su **Avanti**.

6.  Nella pagina del contenuto, completa i passaggi seguenti:

    a.  Fai clic su **Aggiungi**e fai clic sul **Punto di distribuzione**.

    b.  In **punti di distribuzione disponibili**, seleziona i punti di distribuzione che si desidera utilizzare distribuire lo script di configurazione ProfileXML e fai clic su **OK**.

    c.  Fai clic su **Avanti**.

7.  Nella pagina Impostazioni distribuzione, fare clic su **Avanti**.

8.  Nella pagina pianificazione, completa i passaggi seguenti:

    a.  Fai clic sul **Nuovo** per aprire la finestra di dialogo Pianificazione assegnazione.

    b.  Fai clic su **Assegna subito dopo questo evento**e fai clic su **OK**.

    c.  Fai clic su **Avanti**.

9.  Nella pagina esperienza utente, completa i passaggi seguenti:

    1.  Seleziona la casella di controllo **Installazione Software** .

    2.  Fai clic su **Riepilogo**.

10. Nella pagina di riepilogo, fare clic su **Avanti**.

11. Nella pagina Completamento fai clic su **Chiudi**.

Con lo script di configurazione ProfileXML distribuito, accedere a un computer client Windows 10 con l'account utente selezionato durante la compilazione dell'insieme di utenti. Verificare la configurazione del client VPN.

>[!NOTE]
>Lo script VPN_Profile.ps1 non funziona in una sessione di Desktop remoto. Analogamente, non funziona in una sessione avanzata Hyper-V. Se stai testando un Remote Access VPN Always On in macchine virtuali, disabilitare sessione avanzata in macchine virtuali client prima di continuare.

### Verifica la configurazione del client VPN

1.  Nel Pannello di controllo in **System\\Security**, fai clic su **Configuration Manager**. 

2.  Nella finestra di dialogo di proprietà di Configuration Manager, nella scheda **Azioni** , completa i passaggi seguenti:

    a.  Fai clic su **computer criteri vengono recuperati & ciclo di valutazione**, fai clic su **Esegui**e fai clic su **OK**.

    b.  Fai clic su **criteri utente vengono recuperati & ciclo di valutazione**, fai clic su **Esegui**e fai clic su **OK**.

    c.  Scegli **OK**.

3.  Chiudi il pannello di controllo.

Dovresti vedere il nuovo profilo VPN al più presto.

## Configurare il client VPN tramite Intune

Per usare Intune per distribuire i profili di Windows 10 remoto accesso VPN Always On, è possibile configurare il nodo ProfileXML CSP con il profilo VPN creato nella sezione [creare i file di configurazione ProfileXML](#bkmk_ProfileXML)oppure è possibile utilizzare l'esempio XML EAP base fornito di seguito.

>[!NOTE]
>A questo punto, Intune Usa i gruppi di Azure AD. Se Azure AD Connect sincronizzati con il gruppo di utenti VPN da locale ad Azure AD e gli utenti vengono assegnati al gruppo utenti di VPN, sei pronto per procedere.

Creare il criterio di configurazione del dispositivo VPN per configurare i computer client Windows 10 per tutti gli utenti aggiunti al gruppo. Poiché il modello di Intune fornisce i parametri VPN, copiare solo la parte \<EapHostConfig> \</EapHostConfig> del file VPN_ProfileXML. 


### Creare il criterio di configurazione di VPN Always On

1.  Accedi al [portale di Azure](https://portal.azure.com/).

2.  Vai a **Intune** > **configurazione dei dispositivi** > **profili**.

3.  Fai clic su **Crea profilo** per avviare il procedura guidata Crea profilo.

4.  Immetti un **nome** per il profilo VPN e una descrizione (facoltativa).

5.   Nella **piattaforma**, seleziona **Windows 10 o versioni successive**e scegliere **VPN** profilo tipo elenco a discesa.

     >[!TIP]
     >Se si sta creando un profileXML VPN personalizzati, [applicare ProfileXML con Intune](https://docs.microsoft.com/windows/security/identity-protection/vpn/vpn-profile-options#apply-profilexml-using-intune) per vedere le istruzioni.

6. Nella scheda **VPN di Base** , verificare o impostare le impostazioni seguenti:

    - **Nome della connessione:** Immetti il nome della connessione VPN come appare nel computer client nella scheda **VPN** in **Impostazioni**, ad esempio, _Contoso AutoVPN_.  
    
    - **Server:** Aggiungere uno o più server VPN, fai clic su **Aggiungi.**
    
    - **Descrizione** e **FQDN o l'indirizzo IP:** immettere la descrizione e l'indirizzo IP o FQDN del server VPN. Questi valori devono essere allineati con il nome del soggetto nel certificato di autenticazione del server VPN. 
    
    - **Server predefinito:** Se questo è il server VPN predefinito, impostato su **True**. Questa operazione consente al server come server predefinito che usano i dispositivi per stabilire la connessione. 
    
    - **Tipo di connessione:** Imposta su **IKEv2**.  
    
    - **Always On:** Impostato per **consentire** di connettersi alla VPN automaticamente al momento dell'accesso e rimangono connessi finché l'utente si disconnette manualmente.
    
    - **Memorizza le credenziali a ogni accesso**: valore booleano (true o false) per la memorizzazione nella cache le credenziali. Se impostato su true, credenziali vengono memorizzate nella cache quando possibile.

7.  Copiare la stringa di codice XML seguente in un editor di testo:<p>
 
    [!INCLUDE [important-lower-case-true-include](../../../includes/important-lower-case-true-include.md)]
    <p>
    
    ```XML
    <EapHostConfig xmlns="https://www.microsoft.com/provisioning/EapHostConfig"><EapMethod><Type xmlns="https://www.microsoft.com/provisioning/EapCommon">25</Type><VendorId xmlns="https://www.microsoft.com/provisioning/EapCommon">0</VendorId><VendorType xmlns="https://www.microsoft.com/provisioning/EapCommon">0</VendorType><AuthorId xmlns="https://www.microsoft.com/provisioning/EapCommon">0</AuthorId></EapMethod><Config xmlns="https://www.microsoft.com/provisioning/EapHostConfig"><Eap xmlns="https://www.microsoft.com/provisioning/BaseEapConnectionPropertiesV1"><Type>25</Type><EapType xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV1"><ServerValidation><DisableUserPromptForServerValidation>true</DisableUserPromptForServerValidation><ServerNames>NPS.contoso.com</ServerNames><TrustedRootCA>5a 89 fe cb 5b 49 a7 0b 1a 52 63 b7 35 ee d7 1c c2 68 be 4b </TrustedRootCA></ServerValidation><FastReconnect>true</FastReconnect><InnerEapOptional>false</InnerEapOptional><Eap xmlns="https://www.microsoft.com/provisioning/BaseEapConnectionPropertiesV1"><Type>13</Type><EapType xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV1"><CredentialsSource><CertificateStore><SimpleCertSelection>true</SimpleCertSelection></CertificateStore></CredentialsSource><ServerValidation><DisableUserPromptForServerValidation>true</DisableUserPromptForServerValidation><ServerNames>NPS.contoso.com</ServerNames><TrustedRootCA>5a 89 fe cb 5b 49 a7 0b 1a 52 63 b7 35 ee d7 1c c2 68 be 4b </TrustedRootCA></ServerValidation><DifferentUsername>false</DifferentUsername><PerformServerValidation xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">true</PerformServerValidation><AcceptServerName xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">true</AcceptServerName></EapType></Eap><EnableQuarantineChecks>false</EnableQuarantineChecks><RequireCryptoBinding>false</RequireCryptoBinding><PeapExtensions><PerformServerValidation xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV2">true</PerformServerValidation><AcceptServerName xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV2">true</AcceptServerName></PeapExtensions></EapType></Eap></Config></EapHostConfig>
    ```

8.  Sostituire il **\<TrustedRootCA>5a 89 fe cb 5b 49 a7 0b 1a 52 63 b7 35 ee d7 1C c2 68 essere 4b<\/TrustedRootCA>** nell'esempio con l'identificazione personale del certificato dell'autorità di certificazione radice in locale in entrambe le posizioni.
  
    >[!Important]
    >Non usare l'identificazione personale di esempio nella sezione \<TrustedRootCA>\</TrustedRootCA> seguente.  Il TrustedRootCA deve essere l'identificazione personale del certificato dell'autorità di certificazione locale principale che ha emesso il certificato di autenticazione server per server RRAS e dei criteri di rete. **Questo non deve essere il certificato radice cloud, né l'identificazione personale intermedia certificato di autorità di certificazione emittente**.

10. Sostituisci il **\<ServerNames>NPS.contoso.com\</ServerNames>** nel file XML di esempio con il nome FQDN del NPS appartenenti al dominio in cui l'autenticazione viene eseguita. 

11. Copiare la stringa XML rivista e incollalo nella casella di **Xml EAP** nella scheda VPN di Base e fai clic su **OK**.<p>Viene creato un criterio di sempre nel dispositivo configurazione di reti VPN utilizzando il protocollo EAP in Intune.


### Sincronizzare il criterio di configurazione VPN Always On con Intune

Per testare i criteri di configurazione, accedere a un computer client Windows 10 come l'utente che hai aggiunto il gruppo **Sempre su utenti VPN** e quindi eseguire la sincronizzazione con Intune.

1.  Nel menu Start, fai clic su **Impostazioni**.

2.  In impostazioni, fai clic su **account**e fai clic su **Accedi all'azienda o all'istituto di istruzione**.

3.  Fare clic sul profilo MDM e fai clic su **Info**.

4.  Fai clic su **sincronizzazione** per imporre un valutazione dei criteri di Intune e il recupero.

5.  Chiudere le impostazioni. Dopo la sincronizzazione, visualizzare il profilo VPN disponibile nel computer.

## Passaggio successivo
Hai finito di distribuzione VPN Always On.  Per altre funzionalità, che è possibile configurare, vedi la tabella seguente:

|Se si desidera...  |Vedi quindi...  |
|---------|---------|
|Configurare l'accesso condizionale per la VPN    |[Passaggio 7. (Facoltativo) Configurare l'accesso condizionale per la connettività VPN con Azure AD](../../ad-ca-vpn-connectivity-windows10.md): In questo passaggio, puoi ottimizzare accesso agli utenti VPN come autorizzato le risorse con [l'accesso condizionale di Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal). Con l'accesso condizionale di Azure AD per la connettività di rete privata virtuale (VPN), puoi proteggere le connessioni VPN. L'accesso condizionale è un motore di valutazione basato su criteri che ti consente di creare regole di accesso per qualsiasi applicazione connessa ad Azure Active Directory (Azure AD).         |
|Altre informazioni sulle funzionalità avanzate di VPN  |[Funzionalità avanzate di VPN](always-on-vpn-adv-options.md#advanced-vpn-features): questa pagina fornisce indicazioni su come abilitare filtri del traffico VPN, come configurare le connessioni VPN automatica mediante trigger di App e come configurare dei criteri di rete per consentire solo le connessioni VPN da client che utilizzano i certificati rilasciati da Azure ACTIVE DIRECTORY.        |


---
