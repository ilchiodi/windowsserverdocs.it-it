---
title: Configurare il tunnel del dispositivo VPN in Windows 10
description: Informazioni su come creare un tunnel del dispositivo VPN in Windows 10.
ms.prod: windows-server-threshold
ms.date: 11/05/2018
ms.technology: networking-ras
ms.topic: article
ms.assetid: 158b7a62-2c52-448b-9467-c00d5018f65b
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: 989216f90e78689b464240cff957bab1d9c1229b
ms.sourcegitcommit: 0948a1abff1c1be506216eeb51ffc6f752a9fe7e
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/06/2019
ms.locfileid: "66749563"
---
# <a name="configure-vpn-device-tunnels-in-windows-10"></a>Configurare il tunnel di dispositivo VPN in Windows 10

>Si applica a: Windows 10 versione 1709

VPN Always On offre la possibilità di creare un profilo VPN dedicato per computer o dispositivo. Le connessioni VPN Always On includono due tipi di tunnel: 

- _Tunnel dispositivo_ si connette al server VPN specificati prima agli utenti di accedere al dispositivo. Scenari di connettività di pre-accesso e gestione dei dispositivi Usa tunnel di dispositivo.

- _Utente tunnel_ si connette solo dopo che un utente accede al dispositivo. Tunnel utente consente agli utenti di accedere alle risorse dell'organizzazione tramite i server VPN.

A differenza _utente tunnel_, che si connette solo dopo un utente accede al dispositivo o computer _tunnel dispositivo_ consente la connessione VPN stabilire la connessione prima che l'utente accede. Entrambe _tunnel periferica_ e _utente tunnel_ funzionano in modo indipendente con i propri profili VPN, possono essere connesse allo stesso tempo e possono usare diversi metodi di autenticazione e altre impostazioni di configurazione VPN a seconda dei casi. Utente tunnel supporta SSTP e IKEv2 e tunnel dispositivo supporta IKEv2 solo con alcun supporto per il fallback a SSTP.

Utente tunnel è supportato in aggiunti al dominio, aggiunto non di dominio (gruppo di lavoro) o Active Directory-dispositivi aggiunti ad Azure per consentire enterprise sia gli scenari BYOD. È disponibile in tutte le edizioni di Windows e le funzionalità della piattaforma sono disponibili a terze parti tramite il supporto del plug-in VPN di piattaforma UWP.

Tunnel di dispositivo può essere configurata solo nei dispositivi appartenenti a un dominio che eseguono Windows 10 Enterprise o Education versione 1709 o successiva. Non vi è alcun supporto per il controllo di terze parti del tunnel della periferica.


## <a name="device-tunnel-requirements-and-features"></a>Dispositivo Tunnel requisiti e funzionalità
È necessario abilitare l'autenticazione del certificato computer per le connessioni VPN e definire un'autorità di certificazione radice per autenticare le connessioni VPN in ingresso. 

```PowerShell
$VPNRootCertAuthority = “Common Name of trusted root certification authority”
$RootCACert = (Get-ChildItem -Path cert:LocalMachine\root | Where-Object {$_.Subject -Like “*$VPNRootCertAuthority*” })
Set-VpnAuthProtocol -UserAuthProtocolAccepted Certificate, EAP -RootCertificateNameToAccept $RootCACert -PassThru
```

![Requisiti e le funzionalità del dispositivo Tunnel](../../media/device-tunnel-feature-and-requirements.png)

## <a name="vpn-device-tunnel-configuration"></a>Configurazione del dispositivo Tunnel VPN

Il profilo di esempio XML riportato di seguito vengono fornite indicazioni ottimale per scenari in cui solo client avviata pull sono necessarie tramite il tunnel di dispositivo.  I filtri traffico automatico vengono usati per limitare il tunnel di dispositivo solo il traffico di gestione.  Questa configurazione funziona bene per Windows Update, criteri di gruppo tipiche (GP) e gli scenari di aggiornamento di System Center Configuration Manager (SCCM), nonché connettività VPN per primo accesso senza credenziali memorizzate nella cache o gli scenari di reimpostazione della password. 

Per i casi avviate dal server push, come gestione remota Windows (WinRM), GPUpdate remoto e gli scenari di aggiornamento SCCM remoti, è necessario consentire il traffico in ingresso nel tunnel dispositivo, non è possibile usare i filtri traffico.  Se nel profilo di tunnel del dispositivo attiva i filtri traffico, il Tunnel dispositivo rifiuta il traffico in ingresso.  Questa limitazione verrà rimossa a partire da versioni future.


### <a name="sample-vpn-profilexml"></a>Esempio VPN profileXML

Seguito è riportato il profileXML VPN di esempio.

``` xml
<VPNProfile>  
  <NativeProfile>  
<Servers>vpn.contoso.com</Servers>  
<NativeProtocolType>IKEv2</NativeProtocolType>  
<Authentication>  
  <MachineMethod>Certificate</MachineMethod>  
</Authentication>  
<RoutingPolicyType>SplitTunnel</RoutingPolicyType>  
 <!-- disable the addition of a class based route for the assigned IP address on the VPN interface -->
<DisableClassBasedDefaultRoute>true</DisableClassBasedDefaultRoute>  
  </NativeProfile> 
  <!-- use host routes(/32) to prevent routing conflicts -->  
  <Route>  
<Address>10.10.0.2</Address>  
<PrefixSize>32</PrefixSize>  
  </Route>  
  <Route>  
<Address>10.10.0.3</Address>  
<PrefixSize>32</PrefixSize>  
  </Route>  
<!-- traffic filters for the routes specified above so that only this traffic can go over the device tunnel --> 
  <TrafficFilter>  
<RemoteAddressRanges>10.10.0.2, 10.10.0.3</RemoteAddressRanges>  
  </TrafficFilter>
<!-- need to specify always on = true --> 
  <AlwaysOn>true</AlwaysOn> 
<!-- new node to specify that this is a device tunnel -->  
 <DeviceTunnel>true</DeviceTunnel>
<!--new node to register client IP address in DNS to enable manage out -->
<RegisterDNS>true</RegisterDNS>
</VPNProfile>
```

A seconda delle esigenze di ogni scenario di distribuzione specifico, è un'altra funzionalità VPN che può essere configurata con il tunnel della periferica [rilevamento rete attendibile](https://social.technet.microsoft.com/wiki/contents/articles/38546.new-features-for-vpn-in-windows-10-and-windows-server-2016.aspx#Trusted_Network_Detection).

```
 <!-- inside/outside detection -->
  <TrustedNetworkDetection>corp.contoso.com</TrustedNetworkDetection>
```

## <a name="deployment-and-testing"></a>Distribuzione e test

È possibile configurare tunnel di dispositivo usando uno script di Windows PowerShell e usando il bridge di Strumentazione gestione Windows (WMI). Il tunnel del dispositivo VPN Always On deve essere configurato nel contesto del **LOCALSYSTEM** account. A tale scopo, sarà necessario usare [PsExec](https://docs.microsoft.com/sysinternals/downloads/psexec), uno delle [PsTools](https://docs.microsoft.com/sysinternals/downloads/pstools) inclusi nel [Sysinternals](https://docs.microsoft.com/sysinternals/) suite di utilità.

Per le linee guida su come distribuire una per ogni dispositivo `(.\Device)` e una per ogni utente `(.\User)` del profilo, vedere [Using PowerShell scripting con il Provider di Bridge WMI](https://docs.microsoft.com/windows/client-management/mdm/using-powershell-scripting-with-the-wmi-bridge-provider).

Eseguire il comando di Windows PowerShell seguente per verificare di aver correttamente distribuito un profilo di dispositivo:

  ```powershell
  Get-VpnConnection -AllUserConnection
  ```

L'output viene visualizzato un elenco del dispositivo\-wide profili VPN distribuiti nel dispositivo.

### <a name="example-windows-powershell-script"></a>Esempio di Script Windows PowerShell

È possibile utilizzare il seguente script Windows PowerShell per la creazione di uno script personalizzato per la creazione del profilo.

```PowerShell
Param(
[string]$xmlFilePath,
[string]$ProfileName
)

$a = Test-Path $xmlFilePath
echo $a

$ProfileXML = Get-Content $xmlFilePath

echo $XML

$ProfileNameEscaped = $ProfileName -replace ' ', '%20'

$Version = 201606090004

$ProfileXML = $ProfileXML -replace '<', '&lt;'
$ProfileXML = $ProfileXML -replace '>', '&gt;'
$ProfileXML = $ProfileXML -replace '"', '&quot;'

$nodeCSPURI = './Vendor/MSFT/VPNv2'
$namespaceName = "root\cimv2\mdm\dmmap"
$className = "MDM_VPNv2_01"

$session = New-CimSession

try
{
$newInstance = New-Object Microsoft.Management.Infrastructure.CimInstance $className, $namespaceName
$property = [Microsoft.Management.Infrastructure.CimProperty]::Create("ParentID", "$nodeCSPURI", 'String', 'Key')
$newInstance.CimInstanceProperties.Add($property)
$property = [Microsoft.Management.Infrastructure.CimProperty]::Create("InstanceID", "$ProfileNameEscaped", 'String', 'Key')
$newInstance.CimInstanceProperties.Add($property)
$property = [Microsoft.Management.Infrastructure.CimProperty]::Create("ProfileXML", "$ProfileXML", 'String', 'Property')
$newInstance.CimInstanceProperties.Add($property)

$session.CreateInstance($namespaceName, $newInstance)
$Message = "Created $ProfileName profile."
Write-Host "$Message"
}
catch [Exception]
{
$Message = "Unable to create $ProfileName profile: $_"
Write-Host "$Message"
exit
}
$Message = "Complete."
Write-Host "$Message"
```

## <a name="additional-resources"></a>Risorse aggiuntive

Di seguito sono risorse aggiuntive per agevolare la distribuzione VPN.

### <a name="vpn-client-configuration-resources"></a>Risorse di configurazione client VPN

Di seguito sono le risorse di configurazione del client VPN.

- [Come creare profili VPN in System Center Configuration Manager](https://docs.microsoft.com/sccm/protect/deploy-use/create-vpn-profiles)
- [Configurare sempre i Client Windows 10 su connessioni VPN](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)
- [Opzioni del profilo VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-profile-options)

### <a name="remote-access-server-gateway-resources"></a>Risorse Gateway Server di accesso remote

Di seguito sono le risorse Gateway Server di accesso remoto (RAS).

- [Configurare RRAS con un certificato di autenticazione](https://technet.microsoft.com/library/dd458982.aspx)
- [Risoluzione dei problemi relativi a connessioni VPN IKEv2](https://technet.microsoft.com/library/dd941612.aspx)
- [Configurare l'accesso remoto basata su IKEv2](https://technet.microsoft.com/library/ff687731.aspx)

>[!IMPORTANT]
>Quando si usano dispositivi Tunnel con un gateway RAS di Microsoft, è necessario configurare il server RRAS per supportare l'autenticazione del certificato computer IKEv2, abilitando il **Consenti autenticazione del certificato computer per IKEv2** metodo di autenticazione come descritto [qui](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee922682%28v=ws.10%29). Dopo aver abilitata questa impostazione, è fortemente consigliabile che il **Set-VpnAuthProtocol** cmdlet di PowerShell, insieme al **RootCertificateNameToAccept** parametro facoltativo, viene utilizzato per garantire che Connessioni IKEv2 RRAS sono consentite solo per i certificati client VPN concatenati a una definite in modo esplicito interne/private autorità di certificazione radice. In alternativa, il **autorità di certificazione radice attendibili** store nel server RRAS deve essere modificati per assicurarsi che non contiene le autorità di certificazione pubblica come illustrato [qui](https://blogs.technet.microsoft.com/rrasblog/2009/06/10/what-type-of-certificate-to-install-on-the-vpn-server/). Metodi simili potrebbe essere necessario anche da considerare per altri gateway VPN.

