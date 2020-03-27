---
title: Configurare le connessioni VPN Always On dei client Windows 10
description: In questo passaggio vengono fornite informazioni sulle opzioni e sullo schema di ProfileXML e vengono configurati i computer client Windows 10 per comunicare con tale infrastruttura con una connessione VPN.
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.date: 05/29/2018
ms.assetid: d165822d-b65c-40a2-b440-af495ad22f42
ms.localizationpriority: medium
ms.author: lizross
author: eross-msft
ms.reviewer: deverette
ms.openlocfilehash: 11906c737dd1604bf064e25a01289fe2ee5a23ad
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80310492"
---
# <a name="step-6-configure-windows-10-client-always-on-vpn-connections"></a>Passaggio 6. Configurare le connessioni VPN Always On client Windows 10

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Precedente:** Passaggio 5. Configurare le impostazioni di DNS e firewall](vpn-deploy-dns-firewall.md)<br>
- [Passaggio **successivo:** Passaggio 7. Opzionale Accesso condizionale per la connettività VPN con Azure AD](../../ad-ca-vpn-connectivity-windows10.md)

In questo passaggio si apprenderanno informazioni sulle opzioni e sullo schema di ProfileXML e si configureranno i computer client Windows 10 per comunicare con tale infrastruttura con una connessione VPN.

È possibile configurare il client VPN Always On tramite PowerShell, Microsoft endpoint Configuration Manager o Intune. Per configurare le impostazioni VPN appropriate, tutte e tre richiedono un profilo VPN XML. L'automazione della registrazione di PowerShell per organizzazioni senza Configuration Manager o Intune è possibile.

>[!NOTE]
>Criteri di gruppo non include modelli amministrativi per la configurazione di accesso remoto Windows 10 Always On client VPN.  Tuttavia, è possibile utilizzare gli script di accesso.

## <a name="profilexml-overview"></a>Panoramica di ProfileXML

ProfileXML è un nodo URI all'interno del CSP VPNv2. Invece di configurare ogni singolo nodo VPNv2 CSP, ad esempio trigger, elenchi di route e protocolli di autenticazione, usare questo nodo per configurare un client VPN Windows 10 fornendo tutte le impostazioni come un singolo blocco XML a un singolo nodo CSP. Lo schema ProfileXML corrisponde quasi esattamente allo schema dei nodi VPNv2 CSP, ma alcuni termini sono leggermente diversi.

Si usa ProfileXML in tutti i metodi di recapito descritti da questa distribuzione, tra cui Windows PowerShell, Microsoft endpoint Configuration Manager e Intune. Esistono due modi per configurare il nodo CSP ProfileXML VPNv2 in questa distribuzione:

- **OMA-DM**. Un modo consiste nell'usare un provider MDM usando OMA-DM, come illustrato in precedenza nella sezione [nodi VPNV2 CSP](../always-on-vpn-technology-overview.md#vpnv2-csp-nodes). Con questo metodo è possibile inserire facilmente il markup XML di configurazione del profilo VPN nel nodo ProfileXML CSP quando si usa Intune.

- **Bridge Strumentazione gestione Windows (WMI) a CSP**. Il secondo metodo di configurazione del nodo CSP ProfileXML consiste nell'usare il Bridge WMI-to-CSP, una classe WMI denominata **MDM_VPNv2_01**, che può accedere al VPNv2 CSP e al nodo ProfileXML. Quando si crea una nuova istanza della classe WMI, WMI usa il CSP per creare il profilo VPN quando si usa Windows PowerShell e Configuration Manager.

Anche se questi metodi di configurazione sono diversi, è necessario un profilo VPN XML correttamente formattato. Per usare l'impostazione ProfileXML VPNv2 CSP, è possibile costruire codice XML usando lo schema ProfileXML per configurare i tag necessari per lo scenario di distribuzione semplice. Per ulteriori informazioni, vedere [PROFILEXML XSD](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/vpnv2-profile-xsd).

Di seguito sono riportate tutte le impostazioni necessarie e il corrispondente tag ProfileXML. È possibile configurare ogni impostazione in un tag specifico all'interno dello schema ProfileXML e non tutte le impostazioni sono disponibili nel profilo nativo. Per la posizione aggiuntiva dei tag, vedere lo schema ProfileXML.

[!INCLUDE [important-lower-case-true-include](../../../includes/important-lower-case-true-include.md)]

**Tipo di connessione:** IKEv2 nativo

Elemento ProfileXML:

```xml
<NativeProtocolType>IKEv2</NativeProtocolType>
```

**Routing:** Split tunneling

Elemento ProfileXML:

```xml
<RoutingPolicyType>SplitTunnel</RoutingPolicyType>
```

**Risoluzione dei nomi:** Elenco di informazioni sul nome di dominio e suffisso DNS

Elementi ProfileXML:

```xml
<DomainNameInformation>
<DomainName>.corp.contoso.com</DomainName>
<DnsServers>10.10.1.10,10.10.1.50</DnsServers>
</DomainNameInformation>

<DnsSuffix>corp.contoso.com</DnsSuffix>
```

**Attivazione:** Rilevamento della rete Always On e attendibile

Elementi ProfileXML:

```xml
<AlwaysOn>true</AlwaysOn>
<TrustedNetworkDetection>corp.contoso.com</TrustedNetworkDetection>
```

**Autenticazione:** PEAP-TLS con certificati utente protetti con TPM

Elementi ProfileXML:

```xml
<Authentication>
<UserMethod>Eap</UserMethod>
<Eap>
<Configuration>...</Configuration>
</Eap>
</Authentication>
```

È possibile usare semplici tag per configurare alcuni meccanismi di autenticazione VPN. Tuttavia, EAP e PEAP sono più interessati. Il modo più semplice per creare il markup XML consiste nel configurare un client VPN con le relative impostazioni EAP, quindi esportare la configurazione in XML.

Per ulteriori informazioni sulle impostazioni EAP, vedere [configurazione EAP](https://msdn.microsoft.com/windows/hardware/commercialize/customize/mdm/eap-configuration).

## <a name="manually-create-a-template-connection-profile"></a>Creare manualmente un profilo di connessione a un modello

In questo passaggio viene utilizzato PEAP (Protected Extensible Authentication Protocol) per proteggere le comunicazioni tra il client e il server. Diversamente da un nome utente e una password semplici, per il funzionamento di questa connessione è necessaria una sezione EAPConfiguration univoca nel profilo VPN.

Invece di descrivere come creare il markup XML da zero, è possibile usare le impostazioni in Windows per creare un profilo VPN del modello. Dopo aver creato il profilo VPN del modello, usare Windows PowerShell per usare la parte EAPConfiguration di tale modello per creare il ProfileXML finale che si distribuirà in un secondo momento nella distribuzione.

### <a name="record-nps-certificate-settings"></a>Registrare le impostazioni del certificato NPS

Prima di creare il modello, prendere nota del nome host o del nome di dominio completo (FQDN) del server NPS dal certificato del server e il nome dell'autorità di certificazione che ha emesso il certificato.

**Procedura**

1. Nel server dei criteri di rete aprire Server dei criteri di rete.

2. Nella console server dei criteri di rete, in criteri, fare clic su **criteri di rete**.

3. Fare clic con il pulsante destro del mouse su **connessioni di rete privata virtuale (VPN)** e scegliere **Proprietà**.

4. Fare clic sulla scheda **vincoli** e quindi su **metodi di autenticazione**.

5. In tipi EAP, fare clic su **Microsoft: Protected EAP (PEAP)** , quindi fare clic su **modifica**.

6. Registrare i valori per il **certificato emesso in** e l' **emittente**.

    Questi valori vengono usati nella configurazione del modello VPN imminente. Se, ad esempio, il nome di dominio completo del server è nps01.corp.contoso.com e il nome host è NPS01, il nome del certificato è basato sul nome di dominio completo o DNS del server, ad esempio nps01.corp.contoso.com.

7. Annullare la finestra di dialogo Modifica proprietà EAP protetto.

8. Annullare la finestra di dialogo Proprietà connessioni di rete privata virtuale (VPN).

9. Chiudere Server dei criteri di rete.

>[!NOTE]
>Se si dispone di più server dei criteri di rete, eseguire questi passaggi in ognuno di essi in modo che il profilo VPN possa verificarne l'utilizzo.

### <a name="configure-the-template-vpn-profile-on-a-domain-joined-client-computer"></a>Configurare il profilo VPN del modello in un computer client aggiunto al dominio

Ora che sono disponibili le informazioni necessarie, configurare il profilo VPN del modello in un computer client aggiunto al dominio. Il tipo di account utente usato (ovvero l'utente standard o l'amministratore) per questa parte del processo non è rilevante.

Tuttavia, se il computer non è stato riavviato dopo la configurazione della registrazione automatica dei certificati, effettuare questa operazione prima di configurare la connessione VPN del modello per assicurarsi di disporre di un certificato utilizzabile registrato.

>[!NOTE]
>Non è possibile aggiungere manualmente le proprietà avanzate della VPN, ad esempio le regole della tabella dei criteri di risoluzione dei nomi, Always On, il rilevamento della rete attendibile e così via. Nel passaggio successivo viene creata una connessione VPN di test per verificare la configurazione del server VPN e che è possibile stabilire una connessione VPN al server.

**Creare manualmente una connessione VPN di test singolo**

1.  Accedere a un computer client aggiunto al dominio come membro del gruppo **utenti VPN** .

2.  Nel menu Start digitare **VPN**e premere INVIO.

3.  Nel riquadro dei dettagli fare clic su **Aggiungi una connessione VPN**.

4.  Nell'elenco provider VPN fare clic su **Windows (predefinito)** .

5.  In nome connessione digitare **modello**.

6.  In nome server o indirizzo digitare l'FQDN **esterno** del server VPN (ad esempio, **VPN.contoso.com**).

7.  Fare clic su **Save**.

8.  In impostazioni correlate fare clic su **modifica opzioni scheda**.

9.  Fare clic con il pulsante destro del mouse su **modello**e scegliere **Proprietà**.

10. Nella scheda **sicurezza** , in **tipo di VPN**, fare clic su **IKEv2**.

11. In crittografia dei dati fare clic su **crittografia massima forza**.

12. Fare clic su **USA Extensible Authentication Protocol (EAP)** ; quindi, in **use Extensible Authentication Protocol (EAP)** fare clic su **Microsoft: Protected EAP (PEAP) (Encryption Enabled)** .

13. Fare clic su **Proprietà** per aprire la finestra di dialogo Proprietà EAP protette e completare i passaggi seguenti:

    a. Nella casella **Connetti a questi server** Digitare il nome del server NPS recuperato dalle impostazioni di autenticazione server dei criteri di ricerca in precedenza in questa sezione (ad esempio, NPS01).

    >[!NOTE]
    >Il nome del server digitato deve corrispondere al nome del certificato. Questo nome è stato recuperato in precedenza in questa sezione. Se il nome non corrisponde, la connessione avrà esito negativo, indicando che è stata impedita la connessione a causa di un criterio configurato sul server RAS/VPN.

    b.  In autorità di certificazione radice attendibili selezionare la CA radice che ha emesso il certificato del server NPS, ad esempio contoso-CA.

    c.  In notifiche prima della connessione fare clic su **non richiedere all'utente di autorizzare nuovi server o CA attendibili**.

    d.  In Seleziona metodo di autenticazione fare clic su **Smart Card o altro certificato**, quindi fare clic su **Configura**. Verrà visualizzata la finestra di dialogo Proprietà smart card o altro certificato.

    e.  Fare clic **su Usa un certificato nel computer**.

    f.  Nella casella Connetti a questi server immettere il nome del server NPS recuperato dalle impostazioni di autenticazione server NPS nei passaggi precedenti.

    g.  In autorità di certificazione radice attendibili selezionare la CA radice che ha emesso il certificato del server NPS.

    h.  Selezionare la casella **di controllo non richiedere all'utente di autorizzare nuovi server o autorità di certificazione attendibili** .

    i.  Fare clic su **OK** per chiudere la finestra di dialogo Proprietà smart card o altro certificato.

    j.  Fare clic su **OK** per chiudere la finestra di dialogo Proprietà EAP protette.

14. Fare clic su **OK** per chiudere la finestra di dialogo Proprietà modello.

15. Chiudere la finestra Connessioni di rete.

16. In impostazioni, testare la VPN facendo clic su **modello**e quindi su **Connetti**.

>[!IMPORTANT]
>Assicurarsi che la connessione VPN del modello al server VPN abbia esito positivo. In questo modo si garantisce che le impostazioni EAP siano corrette prima di utilizzarle nell'esempio successivo. È necessario connettersi almeno una volta prima di continuare; in caso contrario, il profilo non conterrà tutte le informazioni necessarie per connettersi alla VPN.

## <a name="create-the-profilexml-configuration-files"></a>Creare i file di configurazione ProfileXML

Prima di completare questa sezione, verificare di aver creato e testato la connessione VPN del modello che la sezione [crea manualmente un profilo di connessione del modello](#manually-create-a-template-connection-profile) . Il test della connessione VPN è necessario per garantire che il profilo contenga tutte le informazioni necessarie per connettersi alla VPN.

Lo script di Windows PowerShell nel listato 1 crea due file sul desktop, entrambi contenenti tag **EAPConfiguration** basati sul profilo di connessione del modello creato in precedenza:

- **VPN_Profile. XML.** Questo file contiene il markup XML necessario per configurare il nodo ProfileXML in VPNv2 CSP. Usare questo file con servizi MDM compatibili con OMA-DM, ad esempio Intune.

- **VPN_Profile. ps1.** Questo file è uno script di Windows PowerShell che è possibile eseguire nei computer client per configurare il nodo ProfileXML in VPNv2 CSP. È anche possibile configurare il CSP distribuendo questo script tramite Configuration Manager. Non è possibile eseguire questo script in una sessione di Desktop remoto, inclusa una sessione avanzata Hyper-V.

>[!IMPORTANT]
>I comandi di esempio seguenti richiedono Windows 10 Build 1607 o versione successiva.

**Creare VPN_Profile. XML e VPN_Proflie. ps1**

1. Accedere al computer client aggiunto al dominio contenente il profilo VPN modello con lo stesso account utente che la sezione [crea manualmente un profilo di connessione del modello](#manually-create-a-template-connection-profile) descritto.

2. Incollare il listato 1 in Windows PowerShell Integrated Scripting Environment (ISE) e personalizzare i parametri descritti nei commenti. Si tratta di $Template, $ProfileName, $Servers, $DnsSuffix, $DomainName, $TrustedNetwork e $DNSServers. Una descrizione completa di ogni impostazione è nei commenti.

3. Eseguire lo script per generare **VPN_Profile. XML** e **VPN_Profile. ps1** sul desktop.

#### <a name="listing-1-understanding-makeprofileps1"></a>Listato 1. Informazioni su MakeProfile. ps1

Questa sezione illustra il codice di esempio che è possibile usare per ottenere informazioni su come creare un profilo VPN, in particolare per la configurazione di ProfileXML in VPNv2 CSP.

Dopo aver assemblato uno script di questo codice di esempio ed eseguito lo script, lo script genera due file: VPN_Profile. XML e VPN_Profile. ps1. Usare VPN_Profile. XML per configurare ProfileXML in Servizi MDM conformi a OMA-DM, ad esempio Microsoft Intune.

Usare lo script **VPN_Profile. ps1** in Windows PowerShell o Microsoft endpoint Configuration Manager per configurare ProfileXML sul desktop di Windows 10.

>[!NOTE]
>Per visualizzare lo script di esempio completo, vedere la sezione [script completo MakeProfile. ps1](#makeprofileps1-full-script).

#### <a name="parameters"></a>Parametri

Configurare i parametri seguenti:

**$Template**. Nome del modello da cui recuperare la configurazione EAP.

**$ProfileName**. Identificatore alfanumerico univoco per il profilo. Il nome del profilo non deve includere una barra (/). Se il nome del profilo ha uno spazio o un altro carattere non alfanumerico, deve essere preceduto da un carattere di escape in base allo standard di codifica URL.

**$Servers**. Indirizzo IP pubblico o instradabile o nome DNS per il gateway VPN. Può puntare all'indirizzo IP esterno di un gateway o un indirizzo IP virtuale per un server farm. Esempi, 208.147.66.130 o vpn.contoso.com.

**$DNSSuffix**. Specifica uno o più suffissi DNS separati da virgole. Il primo nell'elenco viene usato anche come suffisso DNS specifico della connessione primario per l'interfaccia VPN. L'intero elenco verrà aggiunto anche a SuffixSearchList.

**$DomainName**. Utilizzato per indicare lo spazio dei nomi a cui si applicano i criteri. Quando viene eseguita una query del nome, il client DNS confronta il nome nella query con tutti gli spazi dei nomi in DomainNameInformationList per trovare una corrispondenza. Questo parametro può essere uno dei tipi seguenti:

- FQDN-nome di dominio completo
- Suffisso: un suffisso di dominio che verrà accodato alla query ShortName per la risoluzione DNS. Per specificare un suffisso, anteporre un punto (.) al suffisso DNS.

**$DNSServers**. Elenco di indirizzi IP del server DNS separati da virgole da usare per lo spazio dei nomi.

**$TrustedNetwork**. Stringa separata da virgole per identificare la rete attendibile. VPN non si connette automaticamente quando l'utente si trova nella rete wireless aziendale in cui le risorse protette sono direttamente accessibili al dispositivo.

Di seguito sono riportati i valori di esempio per i parametri utilizzati nei comandi seguenti. Assicurarsi di modificare questi valori per l'ambiente in uso.

```xml
$TemplateName = 'Template'
$ProfileName = 'Contoso AlwaysOn VPN'
$Servers = 'vpn.contoso.com'
$DnsSuffix = 'corp.contoso.com'
$DomainName = '.corp.contoso.com'
$DNSServers = '10.10.0.2,10.10.0.3'
$TrustedNetwork = 'corp.contoso.com'
```

### <a name="prepare-and-create-the-profile-xml"></a>Preparare e creare il file XML del profilo

I comandi di esempio seguenti ottengono le impostazioni EAP dal profilo del modello:

```xml
$Connection = Get-VpnConnection -Name $TemplateName
if(!$Connection)
{
$Message = "Unable to get $TemplateName connection profile: $_"
Write-Host "$Message"
exit
}
$EAPSettings= $Connection.EapConfigXmlStream.InnerXml
```

### <a name="create-the-profile-xml"></a>Creare il file XML del profilo

[!INCLUDE [important-lower-case-true-include](../../../includes/important-lower-case-true-include.md)]

```xml
$ProfileXML = @("
<VPNProfile>
  <DnsSuffix>$DnsSuffix</DnsSuffix>
  <NativeProfile>
<Servers>$Servers</Servers>
<NativeProtocolType>IKEv2</NativeProtocolType>
<Authentication>
  <UserMethod>Eap</UserMethod>
  <Eap>
    <Configuration>
     $EAPSettings
    </Configuration>
  </Eap>
</Authentication>
<RoutingPolicyType>SplitTunnel</RoutingPolicyType>
  </NativeProfile>
  <AlwaysOn>true</AlwaysOn>
  <RememberCredentials>true</RememberCredentials>
  <TrustedNetworkDetection>$TrustedNetwork</TrustedNetworkDetection>
  <DomainNameInformation>
<DomainName>$DomainName</DomainName>
<DnsServers>$DNSServers</DnsServers>
</DomainNameInformation>
</VPNProfile>
")
```

### <a name="output-vpn_profilexml-for-intune"></a>Output VPN_Profile. XML per Intune

Per salvare il file XML del profilo, è possibile usare il comando di esempio seguente:

```xml
$ProfileXML | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.xml')
```

### <a name="output-vpn_profileps1-for-the-desktop-and-configuration-manager"></a>Output VPN_Profile. ps1 per desktop e Configuration Manager

Il codice di esempio seguente configura una connessione VPN IKEv2 AlwaysOn usando il nodo ProfileXML in VPNv2 CSP.

È possibile usare questo script in Windows 10 desktop o in Configuration Manager.

### <a name="define-key-vpn-profile-parameters"></a>Definire i parametri principali del profilo VPN

```xml
$Script = '$ProfileName = ''' + $ProfileName + ''''
$ProfileNameEscaped = $ProfileName -replace ' ', '%20'
```

### <a name="escape-special-characters-in-the-profile"></a>Caratteri di escape speciali nel profilo

```xml
$ProfileXML = $ProfileXML -replace '<', '&lt;'
$ProfileXML = $ProfileXML -replace '>', '&gt;'
$ProfileXML = $ProfileXML -replace '"', '&quot;'
```

### <a name="define-wmi-to-csp-bridge-properties"></a>Definire le proprietà del Bridge WMI-to-CSP

```xml
$nodeCSPURI = "./Vendor/MSFT/VPNv2"
$namespaceName = "root\cimv2\mdm\dmmap"
$className = "MDM_VPNv2_01"
```

### <a name="determine-user-sid-for-vpn-profile"></a>Determinare il SID utente per il profilo VPN:

```xml
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
```

### <a name="define-wmi-session"></a>Definire la sessione WMI:

```xml
$session = New-CimSession
$options = New-Object Microsoft.Management.Infrastructure.Options.CimOperationOptions
$options.SetCustomOption("PolicyPlatformContext_PrincipalContext_Type", "PolicyPlatform_UserContext", $false)
$options.SetCustomOption("PolicyPlatformContext_PrincipalContext_Id", "$SidValue", $false)
```

### <a name="detect-and-delete-previous-vpn-profile"></a>Rilevare ed eliminare il profilo VPN precedente:

```xml
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
```

### <a name="create-the-vpn-profile"></a>Creare il profilo VPN:

```xml
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
Write-Host "$Message"
```

### <a name="save-the-profile-xml-file"></a>Salvare il file XML del profilo

```xml
$Script | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.ps1')

$Message = "Successfully created VPN_Profile.xml and VPN_Profile.ps1 on the desktop."
Write-Host "$Message"
```

## <a name="makeprofileps1-full-script"></a>Script completo MakeProfile. ps1

Per la maggior parte degli esempi viene usato il cmdlet di Windows PowerShell Set-WmiInstance per inserire ProfileXML in una nuova istanza del MDM_VPNv2_01 classe WMI. 

Tuttavia, questo non funziona in Configuration Manager perché non è possibile eseguire il pacchetto nel contesto degli utenti finali. Questo script usa quindi il Common Information Model per creare una sessione WMI nel contesto dell'utente e quindi crea una nuova istanza della classe MDM_VPNv2_01 WMI in tale sessione. Questa classe WMI usa il Bridge WMI-to-CSP per configurare il CSP VPNv2. Pertanto, aggiungendo l'istanza della classe, si configura il CSP. 

>[!IMPORTANT]
>Per il Bridge da WMI a CSP sono necessari i diritti di amministratore locale, in base alla progettazione. Per distribuire i profili VPN per utente, è necessario usare Configuration Manager o MDM.

>[!NOTE]
>Lo script VPN_Profile. ps1 usa il SID dell'utente corrente per identificare il contesto dell'utente. Poiché non è disponibile alcun SID in una sessione di Desktop remoto, lo script non funziona in una sessione di Desktop remoto. Analogamente, non funziona in una sessione avanzata di Hyper-V. Se si sta testando un accesso remoto Always On VPN in macchine virtuali, disabilitare la sessione avanzata nelle VM client prima di eseguire questo script.

Lo script di esempio seguente include tutti gli esempi di codice delle sezioni precedenti. Assicurarsi di modificare i valori di esempio in valori appropriati per l'ambiente in uso.
    
   ```makeProfile.ps1
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
    
    $ProfileXML = @("
    <VPNProfile>
      <DnsSuffix>$DnsSuffix</DnsSuffix>
      <NativeProfile>
    <Servers>$Servers</Servers>
    <NativeProtocolType>IKEv2</NativeProtocolType>
    <Authentication>
      <UserMethod>Eap</UserMethod>
      <Eap>
       <Configuration>
        $EAPSettings
       </Configuration>
      </Eap>
    </Authentication>
    <RoutingPolicyType>SplitTunnel</RoutingPolicyType>
      </NativeProfile>
    <AlwaysOn>true</AlwaysOn>
    <RememberCredentials>true</RememberCredentials>
    <TrustedNetworkDetection>$TrustedNetwork</TrustedNetworkDetection>
      <DomainNameInformation>
    <DomainName>$DomainName</DomainName>
    <DnsServers>$DNSServers</DnsServers>
    </DomainNameInformation>
    </VPNProfile>
    ")
    
    $ProfileXML | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.xml')
    
     $Script = @("
      `$ProfileName = '$ProfileName'
      `$ProfileNameEscaped = `$ProfileName -replace ' ', '%20'
 
      `$ProfileXML = '$ProfileXML'
 
      `$ProfileXML = `$ProfileXML -replace '<', '&lt;'
      `$ProfileXML = `$ProfileXML -replace '>', '&gt;'
      `$ProfileXML = `$ProfileXML -replace '`"', '&quot;'
 
      `$nodeCSPURI = `"./Vendor/MSFT/VPNv2`"
      `$namespaceName = `"root\cimv2\mdm\dmmap`"
      `$className = `"MDM_VPNv2_01`"
 
      try
      {
      `$username = Gwmi -Class Win32_ComputerSystem | select username
      `$objuser = New-Object System.Security.Principal.NTAccount(`$username.username)
      `$sid = `$objuser.Translate([System.Security.Principal.SecurityIdentifier])
      `$SidValue = `$sid.Value
      `$Message = `"User SID is `$SidValue.`"
      Write-Host `"`$Message`"
      }
      catch [Exception]
      {
      `$Message = `"Unable to get user SID. User may be logged on over Remote Desktop: `$_`"
      Write-Host `"`$Message`"
      exit
      }
 
      `$session = New-CimSession
      `$options = New-Object Microsoft.Management.Infrastructure.Options.CimOperationOptions
      `$options.SetCustomOption(`"PolicyPlatformContext_PrincipalContext_Type`", `"PolicyPlatform_UserContext`", `$false)
      `$options.SetCustomOption(`"PolicyPlatformContext_PrincipalContext_Id`", `"`$SidValue`", `$false)
 
      try
      {
    `$deleteInstances = `$session.EnumerateInstances(`$namespaceName, `$className, `$options)
    foreach (`$deleteInstance in `$deleteInstances)
    {
        `$InstanceId = `$deleteInstance.InstanceID
        if (`"`$InstanceId`" -eq `"`$ProfileNameEscaped`")
        {
            `$session.DeleteInstance(`$namespaceName, `$deleteInstance, `$options)
            `$Message = `"Removed `$ProfileName profile `$InstanceId`"
            Write-Host `"`$Message`"
        } else {
            `$Message = `"Ignoring existing VPN profile `$InstanceId`"
            Write-Host `"`$Message`"
        }
    }
      }
      catch [Exception]
      {
    `$Message = `"Unable to remove existing outdated instance(s) of `$ProfileName profile: `$_`"
    Write-Host `"`$Message`"
    exit
      }
 
      try
      {
    `$newInstance = New-Object Microsoft.Management.Infrastructure.CimInstance `$className, `$namespaceName
    `$property = [Microsoft.Management.Infrastructure.CimProperty]::Create(`"ParentID`", `"`$nodeCSPURI`", `"String`", `"Key`")
    `$newInstance.CimInstanceProperties.Add(`$property)
    `$property = [Microsoft.Management.Infrastructure.CimProperty]::Create(`"InstanceID`", `"`$ProfileNameEscaped`", `"String`",      `"Key`")
    `$newInstance.CimInstanceProperties.Add(`$property)
    `$property = [Microsoft.Management.Infrastructure.CimProperty]::Create(`"ProfileXML`", `"`$ProfileXML`", `"String`", `"Property`")
    `$newInstance.CimInstanceProperties.Add(`$property)
    `$session.CreateInstance(`$namespaceName, `$newInstance, `$options)
    `$Message = `"Created `$ProfileName profile.`"

    Write-Host `"`$Message`"
      }
      catch [Exception]
      {
    `$Message = `"Unable to create `$ProfileName profile: `$_`"
    Write-Host `"`$Message`"
    exit
      }
 
      `$Message = `"Script Complete`"
      Write-Host `"`$Message`"
      ")
 
      $Script | Out-File -FilePath ($env:USERPROFILE + '\desktop\VPN_Profile.ps1')
    
    $Message = "Successfully created VPN_Profile.xml and VPN_Profile.ps1 on the desktop."
    Write-Host "$Message"
   ```

## <a name="configure-the-vpn-client-by-using-windows-powershell"></a>Configurare il client VPN tramite Windows PowerShell

Per configurare il CSP VPNv2 in un computer client Windows 10, eseguire lo script di Windows PowerShell VPN_Profile. ps1 creato nella sezione [creare il file XML del profilo](#create-the-profile-xml) . Aprire Windows PowerShell come amministratore; in caso contrario, si riceverà un errore che informa che _l'accesso_è stato negato.

Dopo aver eseguito VPN_Profile. ps1 per configurare il profilo VPN, è possibile verificare in qualsiasi momento che sia stato eseguito correttamente eseguendo il comando seguente nel Windows PowerShell ISE:

```powershell
Get-WmiObject -Namespace root\cimv2\mdm\dmmap -Class MDM_VPNv2_01
```

**Risultati riusciti dal cmdlet Get-WmiObject**

```powershell
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
```

La configurazione di ProfileXML deve essere corretta nei casi di struttura, ortografia, configurazione e talvolta letterale. Se è presente un elemento diverso nella struttura per elencare 1, il markup ProfileXML probabilmente contiene un errore.

Se è necessario risolvere i problemi relativi al markup, è più semplice inserirlo in un editor XML anziché risolverlo nella Windows PowerShell ISE. In entrambi i casi, iniziare con la versione più semplice del profilo e aggiungere i componenti indietro uno alla volta fino a quando il problema non si ripete.

## <a name="configure-the-vpn-client-by-using-configuration-manager"></a>Configurare il client VPN usando Configuration Manager

In Configuration Manager, è possibile distribuire i profili VPN usando il nodo ProfileXML CSP, esattamente come in Windows PowerShell. In questo caso si usa lo script di Windows PowerShell VPN_Profile. ps1 creato nella sezione [creare i file di configurazione ProfileXML](#create-the-profilexml-configuration-files).

Per usare Configuration Manager per distribuire un profilo VPN Always On accesso remoto ai computer client Windows 10, è necessario iniziare creando un gruppo di computer o utenti a cui si distribuisce il profilo. In questo scenario, creare un gruppo di utenti per distribuire lo script di configurazione.

### <a name="create-a-user-group"></a>Creare un gruppo di utenti

1.  Nella console di Configuration Manager aprire asset e conformità\\raccolte utenti.

2.  Nella scheda **Home** della barra multifunzione, nel gruppo **Crea** , fare clic su **Crea raccolta utenti**.

3.  Nella pagina generale completare i passaggi seguenti:

    a. In **nome**digitare **utenti VPN**.

    b. Fare clic su **Sfoglia**, selezionare **tutti gli utenti** e fare clic su **OK**.

    c. Fare clic su **Avanti**.

4.  Nella pagina regole di appartenenza completare i passaggi seguenti:

    a.  In **regole di appartenenza**fare clic su **Aggiungi regola**e quindi su **regola diretta**. In questo esempio vengono aggiunti singoli utenti alla raccolta di utenti. Tuttavia, è possibile usare una regola di query per aggiungere utenti a questa raccolta in modo dinamico per una distribuzione su larga scala.

    b.  Nella pagina inizialefare clic su **Avanti**.

    c.  Nella pagina Cerca risorse, in **valore**, digitare il nome dell'utente che si desidera aggiungere. Il nome della risorsa include il dominio dell'utente. Per includere i risultati in base a una corrispondenza parziale, inserire il carattere **%** a una delle estremità del criterio di ricerca. Ad esempio, per trovare tutti gli utenti che contengono la stringa "lori", digitare **% Lori%** . Fare clic su **Avanti**.

    d.  Nella pagina Seleziona risorse selezionare gli utenti che si desidera aggiungere al gruppo e fare clic su **Avanti**.

    e.  Nella pagina Riepilogo fare clic su **Avanti**.

    f.  Nella pagina Completamento fare clic su **Chiudi**.

6.  Tornare alla pagina regole di appartenenza della creazione guidata raccolta utenti, fare clic su **Avanti**.

7.  Nella pagina Riepilogo fare clic su **Avanti**.

8.  Nella pagina Completamento fare clic su **Chiudi**.

Dopo aver creato il gruppo di utenti per la ricezione del profilo VPN, è possibile creare un pacchetto e un programma per distribuire lo script di configurazione di Windows PowerShell creato nella sezione [creare i file di configurazione ProfileXML](#create-the-profilexml-configuration-files).

### <a name="create-a-package-containing-the-profilexml-configuration-script"></a>Creare un pacchetto contenente lo script di configurazione ProfileXML

1.  Ospitare lo script VPN_Profile. ps1 in una condivisione di rete a cui può accedere l'account computer del server del sito.

2.  Nella console di Configuration Manager aprire **raccolta Software\\gestione applicazioni\\pacchetti**.

3.  Nella scheda **Home** della barra multifunzione, nel gruppo **Crea** , fare clic su **Crea pacchetto** per avviare la creazione guidata pacchetto e programma.

4.  Nella pagina del pacchetto completare i passaggi seguenti:

    a. In **nome**digitare **Windows 10 always on profilo VPN**.

    b. Selezionare la casella di controllo **questo pacchetto contiene i file di origine** e fare clic su **Sfoglia**.

    c. Nella finestra di dialogo Imposta cartella di origine fare clic su **Sfoglia**, selezionare la condivisione file contenente VPN_Profile. ps1, quindi fare clic su **OK**.
        Assicurarsi di selezionare un percorso di rete, non un percorso locale. In altre parole, il percorso dovrebbe essere simile *\\fileserver\\vpnscript*, non *c:\\vpnscript*.

1.  Fare clic su **Avanti**.

2.  Nella pagina tipo di programma fare clic su **Avanti**.

3.  Nella pagina programma standard completare i passaggi seguenti:

    a.  In **nome**digitare **script del profilo VPN**.

    b.  Nella **riga di comando**digitare **PowerShell. exe-ExecutionPolicy bypass-file "VPN_Profile. ps1"** .

    c.  In **modalità di esecuzione**fare clic su **Esegui con diritti amministrativi**.

    d.  Fare clic su **Avanti**.

4.  Nella pagina requisiti completare i passaggi seguenti:

    a.  Selezionare **questo programma può essere eseguito solo su piattaforme specifiche**.

    b.  Selezionare le caselle di controllo **tutte le finestre 10 (a 32 bit)** e **tutte le finestre di windows 10 (64-bit)** .

    c.  In **spazio su disco stimato**Digitare **1**.

    d.  In **tempo di esecuzione massimo consentito (minuti)** Digitare **15**.

    e.  Fare clic su **Avanti**.

5.  Nella pagina Riepilogo fare clic su **Avanti**.

6.  Nella pagina Completamento fare clic su **Chiudi**.

Dopo aver creato il pacchetto e il programma, è necessario distribuirlo al gruppo **utenti VPN** .

### <a name="deploy-the-profilexml-configuration-script"></a>Distribuire lo script di configurazione ProfileXML

1.  Nella console di Configuration Manager aprire raccolta software\\gestione applicazioni\\pacchetti.

2.  In **pacchetti**fare clic su **Windows 10 always on profilo VPN**.

3.  Nella scheda **programmi** , nella parte inferiore del riquadro dei dettagli, fare clic con il pulsante destro del mouse su **script profilo VPN**, scegliere **proprietà**e completare i passaggi seguenti:

    a.  Nella scheda **Avanzate** , in **quando il programma è assegnato a un computer**, fare clic **una volta per ogni utente che esegue l'accesso**.

    b.  Fare clic su **OK**.

4.  Fare clic con il pulsante destro del mouse su **script profilo VPN** e scegliere **Distribuisci** per avviare la distribuzione guidata del software.

5.  Nella pagina generale completare i passaggi seguenti:

    a.  Accanto a **raccolta**fare clic su **Sfoglia**.

    b.  Nell'elenco **tipi di raccolta** (in alto a sinistra) fare clic su **raccolte utenti**.

    c.  Fare clic su **utenti VPN**e quindi su **OK**.

    d.  Fare clic su **Avanti**.

6.  Nella pagina contenuto completare i passaggi seguenti:

    a.  Fare clic su **Aggiungi**e quindi su **punto di distribuzione**.

    b.  In **punti di distribuzione disponibili**selezionare i punti di distribuzione a cui si desidera distribuire lo script di configurazione ProfileXML e fare clic su **OK**.

    c.  Fare clic su **Avanti**.

7.  Nella pagina Impostazioni distribuzione fare clic su **Avanti**.

8.  Nella pagina pianificazione completare i passaggi seguenti:

    a.  Fare clic su **nuovo** per aprire la finestra di dialogo pianificazione assegnazione.

    b.  Fare clic su **assegna subito dopo questo evento**, quindi fare clic su **OK**.

    c.  Fare clic su **Avanti**.

9.  Nella pagina esperienza utente completare i passaggi seguenti:

    1.  Selezionare la casella di controllo **Installazione software** .

    2.  Fare clic su **Riepilogo**.

10. Nella pagina Riepilogo fare clic su **Avanti**.

11. Nella pagina Completamento fare clic su **Chiudi**.

Con lo script di configurazione ProfileXML distribuito, accedere a un computer client Windows 10 con l'account utente selezionato quando è stata creata la raccolta utenti. Verificare la configurazione del client VPN.

>[!NOTE]
>Lo script VPN_Profile. ps1 non funziona in una sessione di Desktop remoto. Analogamente, non funziona in una sessione avanzata di Hyper-V. Se si sta testando un accesso remoto Always On VPN in macchine virtuali, disabilitare la sessione avanzata nelle VM client prima di continuare.

### <a name="verify-the-configuration-of-the-vpn-client"></a>Verificare la configurazione del client VPN

1.  Nel pannello di controllo, in **sistema\\sicurezza**, fare clic su **Configuration Manager**. 

2.  Nella finestra di dialogo Proprietà Configuration Manager, nella scheda **azioni** , completare i passaggi seguenti:

    a.  Fare clic su **recupero criteri computer & ciclo di valutazione**, fare clic su **Esegui ora**, quindi fare clic su **OK**.

    b.  Fare clic su **recupero criteri utente & ciclo di valutazione**, fare clic su **Esegui ora**e quindi su **OK**.

    c.  Fare clic su **OK**.

3.  Chiudere il pannello di controllo.

Il nuovo profilo VPN verrà visualizzato a breve.

## <a name="configure-the-vpn-client-by-using-intune"></a>Configurare il client VPN con Intune

Per usare Intune per la distribuzione di accesso remoto Windows 10 Always On profili VPN, è possibile configurare il nodo ProfileXML CSP usando il profilo VPN creato nella sezione [creare i file di configurazione di ProfileXML](#create-the-profilexml-configuration-files). in alternativa, è possibile usare l'esempio di base XML EAP fornito di seguito.

>[!NOTE]
>Intune USA ora i gruppi di Azure AD. Se Azure AD Connect sincronizzato il gruppo utenti VPN da locale a Azure AD e gli utenti sono assegnati al gruppo utenti VPN, è possibile procedere.

Creare i criteri di configurazione del dispositivo VPN per configurare i computer client Windows 10 per tutti gli utenti aggiunti al gruppo. Poiché il modello di Intune fornisce parametri VPN, copiare solo la parte \<EapHostConfig > \</EapHostConfig > del file VPN_ProfileXML.

### <a name="create-the-always-on-vpn-configuration-policy"></a>Creare i criteri di configurazione della VPN Always On

1.  Accedi al [portale di Azure](https://portal.azure.com/).

2.  Passare a **Intune** > **configurazione del dispositivo** > **profili**.

3.  Fare clic su **Crea profilo** per avviare la creazione guidata profilo.

4.  Immettere un **nome** per il profilo VPN e, facoltativamente, una descrizione.

1.   In **piattaforma**selezionare **Windows 10 o versione successiva**e scegliere **VPN** dall'elenco a discesa tipo di profilo.

     >[!TIP]
     >Se si sta creando un profileXML VPN personalizzato, vedere [applicare profileXML usando Intune](https://docs.microsoft.com/windows/security/identity-protection/vpn/vpn-profile-options#apply-profilexml-using-intune) per le istruzioni.

2. Nella scheda **VPN di base** verificare o impostare le impostazioni seguenti:

    - **Nome connessione:** Immettere il nome della connessione VPN così come viene visualizzato nel computer client nella scheda **VPN** in **Impostazioni**, ad esempio, _Contoso AutoVPN_.  
    
    - **Server:** Aggiungere uno o più server VPN facendo clic su **Aggiungi.**
    
    - **Descrizione** e **indirizzo IP o FQDN:** immettere la descrizione e l'indirizzo IP o il nome di dominio completo (FQDN) del server VPN. Questi valori devono essere allineati con il nome del soggetto nel certificato di autenticazione del server VPN. 
    
    - **Server predefinito:** Se si tratta del server VPN predefinito, impostare su **true**. Questa operazione Abilita questo server come server predefinito che i dispositivi usano per stabilire la connessione. 
    
    - **Tipo di connessione:** Impostare su **IKEv2**.  
    
    - **Always on:** Impostare questa impostazione per **consentire** a di connettersi automaticamente alla VPN all'accesso e rimanere connessi fino a quando l'utente non si disconnette manualmente.
    
    - **Ricordare le credenziali a ogni accesso**: valore booleano (true o false) per la memorizzazione delle credenziali nella cache. Se è impostato su true, le credenziali vengono memorizzate nella cache quando possibile.

3.  Copiare la stringa XML seguente in un editor di testo:
 
    [!INCLUDE [important-lower-case-true-include](../../../includes/important-lower-case-true-include.md)]
    
    
    ```XML
    <EapHostConfig xmlns="https://www.microsoft.com/provisioning/EapHostConfig"><EapMethod><Type xmlns="https://www.microsoft.com/provisioning/EapCommon">25</Type><VendorId xmlns="https://www.microsoft.com/provisioning/EapCommon">0</VendorId><VendorType xmlns="https://www.microsoft.com/provisioning/EapCommon">0</VendorType><AuthorId xmlns="https://www.microsoft.com/provisioning/EapCommon">0</AuthorId></EapMethod><Config xmlns="https://www.microsoft.com/provisioning/EapHostConfig"><Eap xmlns="https://www.microsoft.com/provisioning/BaseEapConnectionPropertiesV1"><Type>25</Type><EapType xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV1"><ServerValidation><DisableUserPromptForServerValidation>true</DisableUserPromptForServerValidation><ServerNames>NPS.contoso.com</ServerNames><TrustedRootCA>5a 89 fe cb 5b 49 a7 0b 1a 52 63 b7 35 ee d7 1c c2 68 be 4b </TrustedRootCA></ServerValidation><FastReconnect>true</FastReconnect><InnerEapOptional>false</InnerEapOptional><Eap xmlns="https://www.microsoft.com/provisioning/BaseEapConnectionPropertiesV1"><Type>13</Type><EapType xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV1"><CredentialsSource><CertificateStore><SimpleCertSelection>true</SimpleCertSelection></CertificateStore></CredentialsSource><ServerValidation><DisableUserPromptForServerValidation>true</DisableUserPromptForServerValidation><ServerNames>NPS.contoso.com</ServerNames><TrustedRootCA>5a 89 fe cb 5b 49 a7 0b 1a 52 63 b7 35 ee d7 1c c2 68 be 4b </TrustedRootCA></ServerValidation><DifferentUsername>false</DifferentUsername><PerformServerValidation xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">true</PerformServerValidation><AcceptServerName xmlns="https://www.microsoft.com/provisioning/EapTlsConnectionPropertiesV2">true</AcceptServerName></EapType></Eap><EnableQuarantineChecks>false</EnableQuarantineChecks><RequireCryptoBinding>false</RequireCryptoBinding><PeapExtensions><PerformServerValidation xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV2">true</PerformServerValidation><AcceptServerName xmlns="https://www.microsoft.com/provisioning/MsPeapConnectionPropertiesV2">true</AcceptServerName></PeapExtensions></EapType></Eap></Config></EapHostConfig>
    ```

4.  Sostituire il **\<TrustedRootCA > 5a 89 FE CB 5b 49 a7 0b 1a 52 63 b7 35 ee D7 1C c2 68 be 4b <\/TrustedRootCA >** nell'esempio con l'identificazione personale del certificato dell'autorità di certificazione radice locale in entrambe le posizioni.
  
    >[!Important]
    >Non usare l'identificazione personale di esempio nella sezione \<TrustedRootCA >\</TrustedRootCA > riportata di seguito.  TrustedRootCA deve essere l'identificazione personale del certificato dell'autorità di certificazione radice locale che ha emesso il certificato di autenticazione server per i server RRAS e NPS. **Non deve essere il certificato radice cloud, né l'identificazione personale del certificato della CA emittente intermedia**.

5.  Sostituire il **\<nomeservers > NPS. contoso. com\</ServerNames >** nel codice XML di esempio con il nome di dominio completo del server dei criteri di dominio aggiunto al dominio in cui si verifica l'autenticazione. 

6.  Copiare la stringa XML modificata e incollarla nella casella **EAP XML** nella scheda VPN di base e fare clic su **OK**.
    In Intune viene creato un criterio di configurazione del dispositivo VPN Always On usando EAP.


### <a name="sync-the-always-on-vpn-configuration-policy-with-intune"></a>Sincronizzare i criteri di configurazione della VPN Always On con Intune

Per testare i criteri di configurazione, accedere a un computer client Windows 10 con l'utente aggiunto al gruppo **Always on utenti VPN** e quindi sincronizzarlo con Intune.

1.  Dal menu Start fare clic su **Impostazioni**.

2.  In impostazioni fare clic su **account**e quindi su **Accedi all'ufficio o all'Istituto di istruzione**.

3.  Fare clic sul profilo MDM e quindi su **info**.

4.  Fare clic su **Sincronizza** per forzare la valutazione e il recupero dei criteri di Intune.

5.  Chiudere le impostazioni. Dopo la sincronizzazione, viene visualizzato il profilo VPN disponibile nel computer.

## <a name="next-steps"></a>Passaggi successivi

La distribuzione di Always On VPN è stata eseguita.  Per altre funzionalità che è possibile configurare, vedere la tabella seguente:

|Se vuoi...  |Vedere...  |
|---------|---------|
|Configurare l'accesso condizionale per VPN    |[Passaggio 7. Opzionale Configurare l'accesso condizionale per la connettività VPN con Azure AD](../../ad-ca-vpn-connectivity-windows10.md): in questo passaggio è possibile ottimizzare il modo in cui gli utenti VPN autorizzati accedono alle risorse usando [l'accesso condizionale Azure Active Directory (Azure ad)](https://docs.microsoft.com/azure/active-directory/active-directory-conditional-access-azure-portal). Con Azure AD l'accesso condizionale per la connettività di rete privata virtuale (VPN), è possibile proteggere le connessioni VPN. L'accesso condizionale è un motore di valutazione basato su criteri che ti consente di creare regole di accesso per qualsiasi applicazione connessa ad Azure Active Directory (Azure AD).         |
|Altre informazioni sulle funzionalità VPN avanzate  |[Funzionalità VPN avanzate](always-on-vpn-adv-options.md#advanced-vpn-features): Questa pagina fornisce indicazioni su come abilitare i filtri del traffico VPN, come configurare le connessioni VPN automatiche usando i trigger di app e come configurare NPS per consentire solo le connessioni VPN dai client che usano i certificati rilasciati da Azure ad.        |
