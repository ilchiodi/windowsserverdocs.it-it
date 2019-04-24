---
title: Configurare le connessioni VPN Always On dei client Windows 10
description: In questo passaggio, informazioni sulle opzioni ProfileXML e dello schema e configurare i computer client Windows 10 per comunicare con tale infrastruttura con una connessione VPN.
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59865892"
---
# <a name="step-6-configure-windows-10-client-always-on-vpn-connections"></a>Passaggio 6. Configurare le connessioni VPN Always On di client Windows 10

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10


&#171;  [**Precedente:** Passaggio 5. Configurare DNS e le impostazioni del Firewall](vpn-deploy-dns-firewall.md)<br>
&#187; [ **Next:** Passaggio 7. (Facoltativo) Accesso condizionale per la connettività VPN con Azure AD](../../ad-ca-vpn-connectivity-windows10.md)

In questo passaggio, informazioni sulle opzioni ProfileXML e dello schema e configurare i computer client Windows 10 per comunicare con tale infrastruttura con una connessione VPN. 

È possibile configurare il client VPN Always On tramite PowerShell, SCCM o Intune. Tutte e tre richiedono un profilo VPN XML per configurare le impostazioni VPN appropriate. Automazione di PowerShell è possibile che la registrazione per le organizzazioni senza SCCM o Intune.

>[!NOTE]
>Criteri di gruppo non includono modelli amministrativi per configurare il client di Windows 10 Remote Access VPN Always On.  Tuttavia, è possibile usare gli script di accesso.

## <a name="profilexml-overview"></a>Panoramica di ProfileXML

ProfileXML è un nodo URI all'interno di VPNv2 CSP. Anziché configurare singolarmente ogni nodo VPNv2 CSP, ad esempio i trigger, indirizzare gli elenchi e i protocolli di autenticazione, usare il nodo per configurare un client VPN di Windows 10 grazie a tutte le impostazioni come un singolo blocco XML in un singolo nodo CSP. Lo schema ProfileXML corrispondenti allo schema dei nodi VPNv2 CSP in modo quasi identico, ma alcuni termini sono leggermente diversi.

In tutti i metodi di distribuzione che descritto in questa distribuzione, tra cui Windows PowerShell, System Center Configuration Manager e Intune si usano ProfileXML. Esistono due modi per configurare il nodo ProfileXML VPNv2 CSP in questa distribuzione:

- **OMA-DM**. Un modo consiste nell'usare un provider MDM tramite OMA-DM, come illustrato in precedenza nella sezione [nodi VPNv2 CSP](../always-on-vpn-technology-overview.md#vpnv2-csp-nodes). In questo modo, è possibile inserire facilmente markup XML configurazione profilo VPN al nodo ProfileXML CSP quando si usa Intune.

- **Windows Management Instrumentation (WMI) - a - bridge CSP**. Il secondo metodo di configurazione del nodo ProfileXML CSP consiste nell'usare il bridge di provider WMI per CSP, ovvero una classe WMI denominata **MDM_VPNv2_01**, ovvero in grado di accedere VPNv2 CSP e il nodo ProfileXML. Quando si crea una nuova istanza della classe WMI, WMI utilizza il provider CSP per creare il profilo VPN quando si usa Windows PowerShell e System Center Configuration Manager.

Anche se questi metodi di configurazione sono diversi, entrambi richiedono un profilo VPN XML formattato correttamente. Per usare l'impostazione ProfileXML VPNv2 CSP, sarà possibile creare XML utilizzando lo schema ProfileXML per configurare i tag necessari per lo scenario di distribuzione semplici. Per altre informazioni, vedere [ProfileXML XSD](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/vpnv2-profile-xsd).

Di seguito trovare tutte le impostazioni necessarie e il relativo tag ProfileXML corrispondente. È configurare ogni impostazione in un tag specifico all'interno dello schema ProfileXML e non tutte sono disponibile sotto il profilo nativo. Per la selezione host aggiuntive tag, vedere lo schema ProfileXML.

[!INCLUDE [important-lower-case-true-include](../../../includes/important-lower-case-true-include.md)]

**Tipo di connessione:** Native IKEv2

Elemento ProfileXML:

    <NativeProtocolType>IKEv2</NativeProtocolType>

**Routing:** Lo split tunneling

Elemento ProfileXML:

    <RoutingPolicyType>SplitTunnel</RoutingPolicyType>

**Risoluzione dei nomi:** Suffisso DNS e l'elenco di informazioni sul nome di dominio

Elementi ProfileXML:

    <DomainNameInformation>
    <DomainName>.corp.contoso.com</DomainName>
    <DnsServers>10.10.1.10,10.10.1.50</DnsServers>
    </DomainNameInformation>
    
    <DnsSuffix>corp.contoso.com</DnsSuffix>


**Attivazione:** Rilevamento di rete Always On e attendibile

Elementi ProfileXML:

    <AlwaysOn>true</AlwaysOn>
    <TrustedNetworkDetection>corp.contoso.com</TrustedNetworkDetection>


**Autenticazione:** PEAP-TLS con i certificati utente protette da TPM

Elementi ProfileXML:

    <Authentication>
    <UserMethod>Eap</UserMethod>
    <Eap>
    <Configuration>...</Configuration>
    </Eap>
    </Authentication>

È possibile usare i tag semplici per configurare alcuni meccanismi di autenticazione VPN. EAP e PEAP sono tuttavia più complesso. Il modo più semplice per creare il markup XML consiste nel configurare un client VPN con le impostazioni EAP e quindi esportare la configurazione in formato XML. 

Per altre informazioni sulle impostazioni EAP, vedere [configurazione EAP](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/eap-configuration). 

## <a name="bkmk_profile"></a>Creare manualmente un modello di profilo di connessione

In questo passaggio, si usa Protected Extensible Authentication Protocol \(PEAP\) per proteggere la comunicazione tra client e il server. A differenza di una semplice coppia nome utente e password, questa connessione richiede una sezione EAPConfiguration univoca nel profilo VPN per lavorare. 

Invece di descrivere come creare il markup XML da zero, si usano le impostazioni in Windows per creare un modello di profilo VPN. Dopo aver creato il modello di profilo VPN, si usa Windows PowerShell per utilizzare la parte EAPConfiguration da tale modello per creare il ProfileXML finale che si distribuisce in un secondo momento durante la distribuzione.

### <a name="record-nps-certificate-settings"></a>Registrare le impostazioni del certificato dei criteri di rete

Prima di creare il modello, prendere nota il nome host o nome di dominio completo (FQDN) del server dei criteri di rete dal certificato del server e il nome dell'autorità di certificazione che ha emesso il certificato.

**Procedura:**

1.  Nel server NPS, aprire Server dei criteri di rete.

2.  Nella console Criteri di rete, criteri, fare clic su **i criteri di rete**.

3.  Fare doppio clic su **Virtual Private Network \(VPN\) connessioni**, fare clic su **proprietà**.

4.  Scegliere il **vincoli** scheda e fare clic su **metodi di autenticazione**.

5.  Nei tipi EAP, fare clic su **Microsoft: PEAP (Protected EAP)**, fare clic su **modifica**.

6.  Registrare i valori per **certificato emesso per** e **autorità di certificazione**.<p>Si utilizzano questi valori della configurazione del modello VPN imminente. Ad esempio, se il FQDN del server è nps01.corp.contoso.com e il nome host è NPS01, al nome DNS o FQDN del server - nps01.corp.contoso.com, ad esempio, si basa il nome del certificato.

7.  Annullare la finestra di dialogo Modifica proprietà PEAP.

8.  Annullare la finestra di dialogo proprietà delle connessioni di rete privata virtuale (VPN).

9.  Chiudere il Server dei criteri di rete.

>[!NOTE]
>Se si dispone di più server NPS, completare questi passaggi su ciascuna di esse in modo che il profilo VPN possa verificare ognuno di essi deve essere utilizzati.

### <a name="configure-the-template-vpn-profile-on-a-domain-joined-client-computer"></a>Configurare il modello di profilo VPN in un dominio\-computer client aggiunti

Dopo aver creato le informazioni necessarie configurare il modello di profilo VPN in un computer client aggiunti al dominio. Il tipo di account utente usato \(, ovvero utente standard o un amministratore\) per questa parte del processo non è rilevante. 

Tuttavia, se è stato ancora riavviato il computer dopo la configurazione di registrazione automatica dei certificati, farlo prima di configurare il modello di connessione VPN per verificare che sia disponibile un certificato utilizzabile registrato su di esso.

>[!NOTE]
>Non è possibile aggiungere manualmente le proprietà avanzate della VPN, ad esempio le regole NRPT, AlwaysOn, attendibili rilevamento rete e così via. Nel passaggio successivo, si crea una connessione VPN di test per verificare la configurazione del server VPN e che è possibile stabilire una connessione VPN al server.

**Creare manualmente un singolo test connessione VPN**

1.  Accedi a un computer client aggiunti al dominio come membro del **gli utenti VPN** gruppo.

2.  Nel menu Start, digitare **VPN**, quindi premere INVIO.

3.  Nel riquadro dei dettagli, fare clic su **aggiungere una connessione VPN**.

4.  Nell'elenco di Provider di VPN, fare clic su **Windows (predefinito)**.

5.  Nel nome della connessione, digitare **modello**.

6.  Nome del Server o l'indirizzo, digitare il **esterne** FQDN del server VPN \(ad esempio, **vpn.contoso.com**\).

7.  Fare clic su **Salva**.

8.  In impostazioni correlate, fare clic su **modificare le opzioni di adapter**.

9.  Fare doppio clic su **modello**, fare clic su **proprietà**.

10. Nel **sicurezza** nella scheda **tipo di VPN**, fare clic su **IKEv2**.

11. Nella crittografia dei dati, fare clic su **massimo livello di crittografia**.

12. Fare clic su **utilizza Extensible Authentication Protocol (EAP)**; quindi, nel **utilizza Extensible Authentication Protocol (EAP)**, fare clic su **Microsoft: PEAP (Protected EAP) (crittografia abilitata)**.

13. Fare clic su **proprietà** per aprire la finestra di dialogo Proprietà PEAP e completare i passaggi seguenti:

    a. Nel **Connetti ai server seguenti** , digitare il nome del server NPS recuperati dalle impostazioni di autenticazione server NPS in precedenza in questa sezione (ad esempio, NPS01).

    >[!NOTE]
    >Il nome del server digitato deve corrispondere al nome del certificato. Sono ripristinate questo nome in precedenza in questa sezione. Se il nome non corrisponde, la connessione avrà esito negativo, indicando che "la connessione è stata impedita a causa di un criterio configurato nel server RAS/VPN."

    b.  In Autorità di certificazione radice attendibili, selezionare la CA che ha emesso il certificato del server dei criteri di rete (ad esempio, contoso-CA) radice.

    c.  Nelle notifiche prima della connessione, fare clic su **non chiedere all'utente di autorizzare nuovi server o CA attendibili**.

    d.  In Selezione metodo di autenticazione, fare clic su **Smart Card o altro certificato**, fare clic su **configura**. Verrà visualizzata la finestra di Smart Card o altra finestra di dialogo proprietà del certificato.

    e.  Fare clic su **usare un certificato su questo computer**.

    f.  Nella finestra di Connect to questi server, immettere il nome del server NPS recuperato dalle impostazioni di autenticazione del server dei criteri di rete nei passaggi precedenti.

    g.  In Autorità di certificazione radice attendibili, selezionare la radice della CA che ha emesso il certificato del server dei criteri di rete.

    h.  Selezionare il **non richiedere all'utente di autorizzare nuovi server o autorità di certificazione attendibili** casella di controllo.

    i.  Fare clic su **OK** per chiudere la Smart Card o altra finestra di dialogo proprietà del certificato.

    j.  Fare clic su **OK** per chiudere la finestra di dialogo Proprietà PEAP.

10. Fare clic su **OK** per chiudere la finestra di dialogo proprietà del modello.

11. Chiudere la finestra connessioni di rete.

12. In impostazioni, verificare la connessione VPN facendo **modello**e scegliendo **Connect**.

>[!IMPORTANT]
>Assicurarsi che il modello di connessione VPN al server VPN ha esito positivo. In modo da garantire che le impostazioni EAP siano corrette prima di usarli nell'esempio seguente. È necessario connettere almeno una volta prima di continuare; in caso contrario, il profilo non conterrà tutte le informazioni necessarie per connettersi alla VPN.

## <a name="bkmk_ProfileXML"></a>Creare i file di configurazione ProfileXML

Prima di completare questa sezione, assicurarsi aver creato e testato il modello di connessione VPN che la sezione [creare manualmente un modello di profilo di connessione](#bkmk_profile) descrive. Test della connessione VPN è necessario assicurarsi che il profilo contiene tutte le informazioni necessarie per connettersi alla VPN.

Lo script di Windows PowerShell nel listato 1 crea due file sul desktop, entrambi contenenti **EAPConfiguration** tag basato sul profilo di connessione del modello creato in precedenza:

-   **VPN_Profile.xml.** Questo file contiene il markup XML necessario per configurare il nodo ProfileXML in VPNv2 CSP. Questo file può essere usato con i servizi compatibili con OMA-DM MDM, ad esempio Intune.

-   **VPN_Profile.ps1.** Questo file è uno script di Windows PowerShell che è possibile eseguire nei computer client per configurare il nodo ProfileXML in VPNv2 CSP. È anche possibile configurare il provider CSP tramite la distribuzione di questo script tramite System Center Configuration Manager. È possibile eseguire questo script in una sessione Desktop remoto, tra cui una sessione avanzata Hyper-V.

>[!IMPORTANT]
>I comandi di esempio seguenti richiedono Windows 10 Build 1607 o successiva.

**Creare VPN_Profile.xml e VPN_Proflie.ps1**

1. Accedere al computer client aggiunti a un dominio che contiene il modello di profilo VPN con lo stesso nome utente account che la sezione "Creare manualmente un modello di profilo di connessione" descritto.

2. Incollare il listato 1 in ambiente di scripting integrata di Windows PowerShell \(ISE\)e personalizzare i parametri descritti nei commenti. These are $Template, $ProfileName, $Servers, $DnsSuffix, $DomainName, $TrustedNetwork, and $DNSServers. Una descrizione completa di ogni impostazione è nei commenti.

3.  Eseguire lo script per generare **VPN_Profile.xml** e **VPN_Profile.ps1** sul desktop.

#### <a name="listing-1-understanding-makeprofileps1"></a>Listato 1. Understanding MakeProfile.ps1

Questa sezione viene illustrato il codice di esempio che è possibile usare per comprendere come creare un profilo VPN, in particolare per la configurazione ProfileXML in VPNv2 CSP.

Dopo avere assemblare uno script in questo esempio di codice ed eseguito lo script, lo script genera due file: VPN_Profile.XML e VPN_Profile.ps1. Usare VPN_Profile.xml configurare ProfileXML OMA-DM conformi MDM servizi, ad esempio Microsoft Intune.

Usare la **VPN_Profile.ps1** script in Windows PowerShell o System Center Configuration Manager per configurare ProfileXML sul desktop di Windows 10.

>[!NOTE]
>Per visualizzare lo script di esempio completo, vedere la sezione [Script completo MakeProfile.ps1](#bkmk_fullscript).

#### <a name="parameters"></a>Parametri

Configurare i parametri seguenti:

**$Template**. Il nome del modello da cui recuperare la configurazione EAP.

**$ProfileName**. Identificatore alfanumerico univoco per il profilo. Il nome del profilo non deve includere una barra (/). Se il nome del profilo è uno spazio o altri caratteri non alfanumerici, deve essere codificata correttamente in base a standard di codifica URL.

**$Servers**. Indirizzo IP pubblico o instradabile o nome DNS per il gateway VPN. Indirizzo IP di un gateway o un indirizzo IP virtuale per una server farm può puntare all'esterno. Esempi, 208.147.66.130 o vpn.contoso.com.

**$DnsSuffix**. Specifica che una o più virgole separati i suffissi DNS. Il primo nell'elenco viene anche utilizzato come il suffisso DNS specifico della connessione per l'interfaccia VPN. Verrà aggiunti nel SuffixSearchList l'intero elenco.

**$DomainName**. Utilizzato per indicare lo spazio dei nomi a cui si applica il criterio. Quando viene eseguita una query del nome, il client DNS confronta il nome nella query per tutti gli spazi dei nomi in DomainNameInformationList per trovare una corrispondenza. Questo parametro può essere uno dei tipi seguenti:

- Nome di dominio completo - nome di dominio completo
- Suffisso - un suffisso di dominio che verrà aggiunto alla query di nome breve per la risoluzione DNS. Per specificare un suffisso, anteporre un periodo \(.) per il suffisso DNS.

**$DNSServers**. Elenco di IP del Server DNS delimitato da virgole degli indirizzi da utilizzare per lo spazio dei nomi.

**$TrustedNetwork**. Stringa separato da virgole per identificare la rete attendibile. VPN non si connette automaticamente quando l'utente è nella propria rete wireless aziendale in cui le risorse protette sono direttamente accessibili per il dispositivo.

Valori di esempio per i parametri utilizzati in comandi riportati di seguito. Assicurarsi di modificare questi valori per l'ambiente.

    $TemplateName = 'Template'
    $ProfileName = 'Contoso AlwaysOn VPN'
    $Servers = 'vpn.contoso.com'
    $DnsSuffix = 'corp.contoso.com'
    $DomainName = '.corp.contoso.com'
    $DNSServers = '10.10.0.2,10.10.0.3'
    $TrustedNetwork = 'corp.contoso.com'


### <a name="prepare-and-create-the-profile-xml"></a>Preparare e creare il file XML del profilo

I comandi nell'esempio seguente, impostazioni EAP ottengono il modello di profilo:


    $Connection = Get-VpnConnection -Name $TemplateName
    if(!$Connection)
    {
    $Message = "Unable to get $TemplateName connection profile: $_"
    Write-Host "$Message"
    exit
    }
    $EAPSettings= $Connection.EapConfigXmlStream.InnerXml


### <a name="create-the-profile-xml"></a>Creare il profilo XML

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


### <a name="output-vpnprofilexml-for-intune"></a>Output VPN_Profile.xml per Intune

È possibile usare il comando seguente per salvare il file XML del profilo:

    $ProfileXML | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.xml')

### <a name="output-vpnprofileps1-for-the-desktop-and-system-center-configuration-manager"></a>VPN_Profile.ps1 di output per il desktop e System Center Configuration Manager

Esempio di codice seguente consente di configurare una connessione VPN IKEv2 di AlwaysOn tramite il nodo ProfileXML in VPNv2 CSP.

È possibile usare questo script in Windows 10 desktop o in System Center Configuration Manager.

### <a name="define-key-vpn-profile-parameters"></a>Definire i parametri del profilo VPN chiave

    $Script = '$ProfileName = ''' + $ProfileName + '''
    $ProfileNameEscaped = $ProfileName -replace ' ', '%20'

## <a name="define-vpn-profilexml"></a>Definire ProfileXML VPN

    $ProfileXML = ''' + $ProfileXML + '''
    
### <a name="escape-special-characters-in-the-profile"></a>Caratteri speciali di escape nel profilo

    $ProfileXML = $ProfileXML -replace '<', '&lt;'
    $ProfileXML = $ProfileXML -replace '>', '&gt;'
    $ProfileXML = $ProfileXML -replace '"', '&quot;'
    
### <a name="define-wmi-to-csp-bridge-properties"></a>Definire le proprietà di Bridge WMI-a-CSP

    $nodeCSPURI = "./Vendor/MSFT/VPNv2"
    $namespaceName = "root\cimv2\mdm\dmmap"
    $className = "MDM_VPNv2_01"

### <a name="determine-user-sid-for-vpn-profile"></a>Determinare SID dell'utente per il profilo VPN:

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
    

### <a name="define-wmi-session"></a>Definire la sessione di WMI:

    $session = New-CimSession
    $options = New-Object Microsoft.Management.Infrastructure.Options.CimOperationOptions
    $options.SetCustomOption("PolicyPlatformContext_PrincipalContext_Type", "PolicyPlatform_UserContext", $false)
    $options.SetCustomOption("PolicyPlatformContext_PrincipalContext_Id", "$SidValue", $false)


### <a name="detect-and-delete-previous-vpn-profile"></a>Rilevare ed eliminare il profilo VPN precedente:

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
    

### <a name="create-the-vpn-profile"></a>Creare il profilo VPN:

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

### <a name="save-the-profile-xml-file"></a>Salvare il file XML del profilo

    $Script | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.ps1')
    
    $Message = "Successfully created VPN_Profile.xml and VPN_Profile.ps1 on the desktop."
    Write-Host "$Message"

## <a name="bkmk_fullscript"></a>Script completo MakeProfile.ps1

La maggior parte degli esempi usano il cmdlet Set-WmiInstance Windows PowerShell per inserire una nuova istanza della classe WMI MDM_VPNv2_01 ProfileXML. 

Tuttavia, non funziona in System Center Configuration Manager perché non è possibile eseguire il pacchetto nel contesto degli utenti finali. Di conseguenza, questo script Usa Common Information Model per creare una sessione WMI nel contesto dell'utente e quindi crea una nuova istanza della classe WMI MDM_VPNv2_01 in tale sessione. Questa classe WMI utilizza il bridge di provider WMI per CSP per configurare VPNv2 CSP. Di conseguenza, aggiungendo l'istanza della classe, configuri il CSP. 

>[!IMPORTANT]
>Provider WMI per CSP bridge richiede diritti di amministratore locale, per impostazione predefinita. Per distribuire per i profili VPN di utente è consigliabile usare SCCM o MDM.

>[!NOTE]
>Lo script VPN_Profile.ps1 Usa SID dell'utente corrente per identificare il contesto dell'utente. Poiché nessun SID non è disponibile in una sessione Desktop remoto, lo script non funziona in una sessione Desktop remoto. Analogamente, non funziona in una sessione avanzata Hyper-V. Se si sta testando un Remote Access VPN Always On nelle macchine virtuali, disabilitare la sessione avanzata nel client le macchine virtuali prima di eseguire questo script.

Lo script di esempio seguente include tutti gli esempi di codice nella sezione precedente. Assicurarsi di modificare i valori di esempio per i valori appropriati per l'ambiente.
    
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

## <a name="configure-the-vpn-client-by-using-windows-powershell"></a>Configurare il client VPN tramite Windows PowerShell

Per configurare VPNv2 CSP in un computer client Windows 10, eseguire lo script VPN_Profile.ps1 Windows PowerShell che è stato creato nel [crea il profilo XML](#create-the-profile-xml) sezione. Aprire Windows PowerShell come amministratore. in caso contrario, si riceverà un errore, ovviamente _accesso negato_.

Dopo aver eseguito VPN_Profile.ps1 per configurare il profilo VPN, è possibile verificare in qualsiasi momento che sia stato completato eseguendo il comando seguente in Windows PowerShell ISE:

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

La configurazione ProfileXML deve essere corretta nella struttura, il controllo ortografico, configurazione e talvolta maiuscole/minuscole. Se viene visualizzato qualcosa di diverso nella struttura nel listato 1, il markup ProfileXML probabilmente contiene un errore.

Se è necessario risolvere i problemi di markup, risulta più semplice per inserirlo in un editor XML rispetto alla risoluzione dei problemi in Windows PowerShell ISE. In entrambi i casi, iniziare con la versione più semplice del profilo e aggiungere componenti nuovamente uno alla volta finché il problema si ripete.

## <a name="vpn-deploy-client-sccm"></a>Configurare il client VPN tramite System Center Configuration Manager

In System Center Configuration Manager, è possibile distribuire i profili VPN usando il nodo ProfileXML CSP, proprio come in Windows PowerShell. In questo caso, è usare lo script di PowerShell di Windows VPN_Profile.ps1 creato nella sezione [creare i file di configurazione ProfileXML](#bkmk_ProfileXML).

Per usare System Center Configuration Manager per distribuire un profilo Remote Access VPN Always On nei computer client Windows 10, è necessario avviare creando un gruppo di computer o utenti a cui si distribuisce il profilo. In questo scenario, creare un gruppo di utenti per distribuire lo script di configurazione.

### <a name="create-a-user-group"></a>Creare un gruppo di utenti

1.  Nella console di Configuration Manager, aprire asset e conformità\\raccolte utenti.

2.  Nel **Home** della barra multifunzione e nel **Create** raggruppare, fare clic su **Crea raccolta utenti**.

3.  Nella pagina generale, completare i passaggi seguenti:

    a. Nelle **Name**, digitare **gli utenti VPN**.

    b. Fare clic su **esplorare**, fare clic su **tutti gli utenti** e fare clic su **OK**.

    c. Fare clic su **Avanti**.

4.  Nella pagina regole di appartenenza, completare i passaggi seguenti:

    a.  Nelle **regole di appartenenza**, fare clic su **Add Rule**, fare clic su **regola diretta**. In questo esempio, si stanno aggiungendo singoli utenti per la raccolta di utenti. Tuttavia, è possibile utilizzare una regola di query per aggiungere utenti a questa raccolta in modo dinamico per una distribuzione su larga scala.

    b.  Nella pagina iniziale**** fare clic su **Avanti**.

    c.  Nella pagina delle risorse, la ricerca nella **valore**, digitare il nome dell'utente da aggiungere. Il nome di risorsa include il dominio dell'utente. Per includere i risultati in base a una corrispondenza parziale, inserire il **%** carattere su ciascuna estremità del criterio di ricerca. Ad esempio, per trovare tutti gli utenti che contengono la stringa "lori", digitare **% % lori**. Fare clic su **Avanti**.

    d.  Nella pagina Seleziona risorse, selezionare gli utenti a cui si desidera aggiungere al gruppo e fare clic su **successivo**.

    e.  Nella pagina di riepilogo, fare clic su **successivo**.

    f.  Nella pagina di completamento, fare clic su **Chiudi**.

6.  Tornare alla pagina regole di appartenenza della creazione guidata raccolta utenti, fare clic su **successivo**.

7.  Nella pagina di riepilogo, fare clic su **successivo**.

8.  Nella pagina di completamento, fare clic su **Chiudi**.

Dopo aver creato il gruppo di utenti per ricevere il profilo VPN, è possibile creare un pacchetto e programma per distribuire lo script di configurazione di Windows PowerShell che è stato creato nella sezione [creare i file di configurazione ProfileXML](#bkmk_ProfileXML).

### <a name="create-a-package-containing-the-profilexml-configuration-script"></a>Creare un pacchetto che contiene lo script di configurazione ProfileXML

1.  Ospitare VPN_Profile.ps1 lo script in una condivisione di rete che può accedere l'account computer server del sito.

2.  Nella console di Configuration Manager, aprire **raccolta Software\\Gestione applicazioni\\pacchetti**.

3.  Nel **Home** della barra multifunzione e nel **Create** raggruppare, fare clic su **Crea pacchetto** per avviare la creazione guidata pacchetto e programma.

4.  Nella pagina del pacchetto, completare i passaggi seguenti:

    a. Nelle **Name**, digitare **Windows 10 sempre sul profilo VPN**.

    b. Selezionare il **questo pacchetto contiene file di origine** casella di controllo e fare clic su **Sfoglia**.

    c. Nella finestra di dialogo Imposta cartella di origine, fare clic su **esplorare**, selezionare la condivisione di file che contiene VPN_Profile.ps1 e fare clic su **OK**.<p>Assicurarsi di che selezionare un percorso di rete, non un percorso locale. In altre parole, il percorso sarà simile a  *\\fileserver\\vpnscript*, non *c:\\vpnscript*.

1.  Fare clic su **Avanti**.

2.  Nella pagina tipo di programma, fare clic su **successivo**.

3.  Nella pagina del programma Standard, completare i passaggi seguenti:

    a.  Nelle **Name**, digitare **Script del profilo VPN**.

    b.  Nelle **riga di comando**, digitare **PowerShell.exe - ExecutionPolicy Bypass - File "VPN_Profile.ps1"**.

    c.  Nelle **modalità di esecuzione**, fare clic su **Esegui con diritti amministrativi**.

    d.  Fare clic su **Avanti**.

4.  Nella pagina dei requisiti, completare i passaggi seguenti:

    a.  Selezionare **questo programma può essere eseguito solo in piattaforme specifiche**.

    b.  Selezionare il **tutti i Windows 10 (32 bit)** e **tutti i Windows 10 (64 bit)** caselle di controllo.

    c.  Nelle **spazio su disco stimato**, digitare **1**.

    d.  Nelle **tempo massimo consentito fase (minuti)**, digitare **15**.

    e.  Fare clic su **Avanti**.

5.  Nella pagina di riepilogo, fare clic su **successivo**.

6.  Nella pagina di completamento, fare clic su **Chiudi**.

Con il pacchetto e programma creato, è necessario eseguire la distribuzione per il **gli utenti VPN** gruppo.

### <a name="deploy-the-profilexml-configuration-script"></a>Distribuire lo script di configurazione ProfileXML

1.  Nella console di Configuration Manager, aprire Raccolta Software\\Gestione applicazioni\\pacchetti.

2.  Nelle **pacchetti**, fare clic su **Windows 10 sempre sul profilo VPN**.

3.  Nel **i programmi** della scheda nella parte inferiore del riquadro dei dettagli, fare doppio clic su **Script del profilo VPN**, fare clic su **proprietà**e completare i passaggi seguenti:

    a.  Nel **avanzate** nella scheda **quando il programma viene assegnato a un computer**, fare clic su **una volta per ogni utente che si connette**.

    b.  Fare clic su **OK**.

4.  Fare doppio clic su **Script del profilo VPN** e fare clic su **Distribuisci** per avviare distribuzione guidata del Software.

5.  Nella pagina generale, completare i passaggi seguenti:

    a.  Accanto **Collection**, fare clic su **Sfoglia**.

    b.  Nel **tipi di raccolta** elenco (in alto a sinistra), fare clic su **raccolte utenti**.

    c.  Fare clic su **gli utenti VPN**, fare clic su **OK**.

    d.  Fare clic su **Avanti**.

6.  Nella pagina contenuto, completare i passaggi seguenti:

    a.  Fare clic su **Add**, fare clic su **punto di distribuzione**.

    b.  Nelle **punti di distribuzione disponibili**, selezionare i punti di distribuzione a cui si desidera distribuire lo script di configurazione ProfileXML e fare clic su **OK**.

    c.  Fare clic su **Avanti**.

7.  Nella pagina Impostazioni distribuzione, fare clic su **successivo**.

8.  Nella pagina pianificazione, completare i passaggi seguenti:

    a.  Fare clic su **New** per aprire la finestra di dialogo Pianificazione assegnazione.

    b.  Fare clic su **assegnare immediatamente dopo questo evento**, fare clic su **OK**.

    c.  Fare clic su **Avanti**.

9.  Nella pagina esperienza utente, completare i passaggi seguenti:

    1.  Selezionare il **installazione Software** casella di controllo.

    2.  Fare clic su **riepilogo**.

10. Nella pagina di riepilogo, fare clic su **successivo**.

11. Nella pagina di completamento, fare clic su **Chiudi**.

Con lo script di configurazione ProfileXML distribuito, accedere a un computer client Windows 10 con l'account utente che è stata selezionata durante la compilazione nella raccolta di utenti. Verificare la configurazione del client VPN.

>[!NOTE]
>Lo script VPN_Profile.ps1 non funziona in una sessione Desktop remoto. Analogamente, non funziona in una sessione avanzata Hyper-V. Se si sta testando un Remote Access VPN Always On nelle macchine virtuali, disabilitare la sessione avanzata nel client le macchine virtuali prima di continuare.

### <a name="verify-the-configuration-of-the-vpn-client"></a>Verificare la configurazione del client VPN

1.  Nel Pannello di controllo sotto **System\\Security**, fare clic su **Configuration Manager**. 

2.  Nella finestra di dialogo proprietà di Configuration Manager, sul **azioni** scheda, completare i passaggi seguenti:

    a.  Fare clic su **Machine criterio ciclo di recupero e valutazione**, fare clic su **Run Now**, fare clic su **OK**.

    b.  Fare clic su **ciclo di valutazione e il recupero dei criteri utente**, fare clic su **Run Now**, fare clic su **OK**.

    c.  Fare clic su **OK**.

3.  Chiudere il pannello di controllo.

Il nuovo profilo VPN verrà visualizzato a breve.

## <a name="configure-the-vpn-client-by-using-intune"></a>Configurare il client VPN usando Intune

Per usare Intune per distribuire i profili di Windows 10 Remote Access VPN Always On, è possibile configurare il nodo ProfileXML CSP tramite il profilo VPN è stato creato nella sezione [creare i file di configurazione ProfileXML](#bkmk_ProfileXML), oppure è possibile usare il protocollo EAP base Esempio XML fornita di seguito.

>[!NOTE]
>Intune Usa ora i gruppi di Azure AD. Se Azure AD Connect sincronizzato il gruppo di utenti VPN da locale ad Azure AD e gli utenti vengono assegnati al gruppo di utenti VPN, si è pronti per procedere.

Creare i criteri di configurazione del dispositivo VPN per configurare i computer client Windows 10 per tutti gli utenti aggiunti al gruppo. Poiché il modello di Intune fornisce i parametri VPN, copiare solo le \<EapHostConfig > \</EapHostConfig > parte del file VPN_ProfileXML. 


### <a name="create-the-always-on-vpn-configuration-policy"></a>Creare i criteri di configurazione VPN Always On

1.  Accedi il [portale di Azure](https://portal.azure.com/).

2.  Passare a **Intune** > **configurazione del dispositivo** > **profili**.

3.  Fare clic su **crea un profilo** per avviare il profilo di creazione guidata.

4.  Immettere un **nome** per il profilo VPN e (facoltativamente) una descrizione.

5.   Sotto **piattaforma**, selezionare **Windows 10 o versione successiva**, quindi scegliere **VPN** dal profilo tipo elenco a discesa.

     >[!TIP]
     >Se si sta creando un profileXML VPN personalizzata, vedere [ProfileXML applicare tramite Intune](https://docs.microsoft.com/windows/security/identity-protection/vpn/vpn-profile-options#apply-profilexml-using-intune) per le istruzioni.

6. Sotto il **VPN di Base** scheda, verificare o configurare le impostazioni seguenti:

    - **Nome connessione:** Immettere il nome della connessione VPN che viene visualizzato nel computer client nel **VPN** disponibile nella scheda **impostazioni**, ad esempio _Contoso AutoVPN_.  
    
    - **Server:** Aggiungere uno o più server VPN facendo **Add.**
    
    - **Descrizione** e **FQDN o indirizzo IP:** Immettere la descrizione e indirizzo IP o FQDN del server VPN. Questi valori devono essere allineati con il nome del soggetto nel certificato di autenticazione del server VPN. 
    
    - **Server predefinito:** Se questo è il server VPN predefinito, impostato su **True**. In questo modo abilita questo server come server predefinito che i dispositivi usano per stabilire la connessione. 
    
    - **Tipo di connessione:** Impostare su **IKEv2**.  
    
    - **Always On:** Impostare su **abilitare** connettere alla VPN automaticamente all'accesso e restare connessi fino a quando l'utente si disconnette manualmente.
    
    - **Memorizza credenziali a ogni accesso**:  Valore booleano (true o false) per la memorizzazione nella cache le credenziali. Se impostato su true, credenziali viene memorizzate nella cache laddove possibile.

7.  Copiare la stringa XML seguente in un editor di testo:<p>
 
    [!INCLUDE [important-lower-case-true-include](../../../includes/important-lower-case-true-include.md)]
    <p>
    
    ```XML
    <EapHostConfig xmlns="https://www.microsoft.com/provisioning/EapHostConfig"><EapMethod><Type xmlns="https://www.microsoft.com/provisioning/EapCommon">25</Type><VendorId xmlns="https://www.microsoft.com/provisioning/EapCommon">0</VendorId><VendorType xmlns="https://www.microsoft.com/provisioning/EapCommon">0</VendorType><AuthorId xmlns="https://www.microsoft.com/provisioning/EapCommon">0</AuthorId></EapMethod><Config xmlns="https://www.microsoft.com/provisioning/EapHostConfig"><Eap xmlns="https://www.microsoft.com/provisioning/BaseEapConnectionPropertiesV1"><Type>25</Type><EapType xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV1"><ServerValidation><DisableUserPromptForServerValidation>true</DisableUserPromptForServerValidation><ServerNames>NPS.contoso.com</ServerNames><TrustedRootCA>5a 89 fe cb 5b 49 a7 0b 1a 52 63 b7 35 ee d7 1c c2 68 be 4b </TrustedRootCA></ServerValidation><FastReconnect>true</FastReconnect><InnerEapOptional>false</InnerEapOptional><Eap xmlns="https://www.microsoft.com/provisioning/BaseEapConnectionPropertiesV1"><Type>13</Type><EapType xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV1"><CredentialsSource><CertificateStore><SimpleCertSelection>true</SimpleCertSelection></CertificateStore></CredentialsSource><ServerValidation><DisableUserPromptForServerValidation>true</DisableUserPromptForServerValidation><ServerNames>NPS.contoso.com</ServerNames><TrustedRootCA>5a 89 fe cb 5b 49 a7 0b 1a 52 63 b7 35 ee d7 1c c2 68 be 4b </TrustedRootCA></ServerValidation><DifferentUsername>false</DifferentUsername><PerformServerValidation xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">true</PerformServerValidation><AcceptServerName xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">true</AcceptServerName></EapType></Eap><EnableQuarantineChecks>false</EnableQuarantineChecks><RequireCryptoBinding>false</RequireCryptoBinding><PeapExtensions><PerformServerValidation xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV2">true</PerformServerValidation><AcceptServerName xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV2">true</AcceptServerName></PeapExtensions></EapType></Eap></Config></EapHostConfig>
    ```

8.  Sostituire il  **\<TrustedRootCA > 5a 89 fe cb 5b 49 a7 0b 1a 52 63 b7 35 ee d7 1C c2 68 essere 4b <\/TrustedRootCA >** dell'esempio con l'identificazione personale del certificato dell'autorità di certificazione radice in locale in entrambe le posizioni.
  
    >[!Important]
    >Non usare l'identificazione personale di esempio nella \<TrustedRootCA >\</TrustedRootCA > sezione riportata di seguito.  Il TrustedRootCA deve essere l'identificazione personale del certificato dell'autorità di certificazione radice in locale che ha emesso il certificato di autenticazione server per i server NPS e RRAS. **Questo non deve essere il certificato radice di cloud, né l'identificazione personale certificato autorità di certificazione emittente intermedia**.

10. Sostituire il  **\<per nomi server > NPS.contoso.com\</ServerNames >** nell'esempio di codice XML con il nome FQDN del dominio NPS in cui l'autenticazione viene eseguita. 

11. Copiare la stringa XML rivista e incollare il **EAP Xml** casella nella scheda VPN di Base e fare clic su **OK**.<p>Viene creato un criterio sempre nella configurazione del dispositivo VPN con EAP in Intune.


### <a name="sync-the-always-on-vpn-configuration-policy-with-intune"></a>Sincronizzare i criteri di configurazione VPN Always On con Intune

Per testare i criteri di configurazione, accedere a un computer client Windows 10 come l'utente aggiunto in precedenza per il **sempre su VPN agli utenti** gruppo e quindi la sincronizzazione con Intune.

1.  Nel menu Start, fare clic su **impostazioni**.

2.  Nelle impostazioni, fare clic su **conti**, fare clic su **Access work OR o dell'istituto di istruzione**.

3.  Fare clic sul profilo MDM e fare clic su **Info**.

4.  Fare clic su **sincronizzazione** per forzare una valutazione dei criteri di Intune e il recupero.

5.  Chiudere le impostazioni. Dopo la sincronizzazione, noterete che il profilo VPN disponibile nel computer.

## <a name="next-step"></a>Passaggio successivo
Aver completato la distribuzione VPN Always On.  Per altre funzionalità che configurabili, vedere la tabella seguente:

|Se si vuole...  |Vedere quindi...  |
|---------|---------|
|Configurare l'accesso condizionale per VPN    |[Passaggio 7. (Facoltativo) Configurare l'accesso condizionale per la connettività VPN con Azure AD](../../ad-ca-vpn-connectivity-windows10.md): In questo passaggio, è possibile ottimizzare la modalità VPN gli utenti autorizzati accedono le risorse usando [accesso condizionale di Azure Active Directory (Azure AD)](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal). Con accesso condizionale di Azure AD per la connettività di rete privata virtuale (VPN), è possibile proteggere le connessioni VPN. L'accesso condizionale è un motore di valutazione basato su criteri che ti consente di creare regole di accesso per qualsiasi applicazione connessa ad Azure Active Directory (Azure AD).         |
|Altre informazioni sulle funzionalità di VPN avanzate  |[Funzionalità VPN avanzate](always-on-vpn-adv-options.md#advanced-vpn-features): Questa pagina fornisce istruzioni su come abilitare i filtri traffico VPN, come configurare connessioni VPN automatiche tramite i trigger di App e come configurare criteri di rete per consentire solo le connessioni VPN dal client che usano i certificati rilasciati da Azure AD.        |


---
