---
title: Configurare il tunnel del dispositivo VPN in Windows 10
description: Informazioni su come creare un tunnel del dispositivo VPN in Windows 10.
ms.prod: windows-server
ms.date: 11/05/2018
ms.technology: networking-ras
ms.topic: article
ms.assetid: 158b7a62-2c52-448b-9467-c00d5018f65b
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: b5be8827cee22b35fb31bf08d1c960b150dcad84
ms.sourcegitcommit: 07c9d4ea72528401314e2789e3bc2e688fc96001
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 01/29/2020
ms.locfileid: "76822654"
---
# <a name="configure-vpn-device-tunnels-in-windows-10"></a>Configurare i tunnel dei dispositivi VPN in Windows 10

>Si applica a: Windows 10 versione 1709

Always On VPN offre la possibilità di creare un profilo VPN dedicato per un dispositivo o un computer. Always On le connessioni VPN includono due tipi di tunnel: 

- Il _tunnel del dispositivo_ si connette ai server VPN specificati prima che gli utenti accedano al dispositivo. Gli scenari di connettività di pre-accesso e la gestione dei dispositivi usano il tunneling del dispositivo.

- Il _tunnel utente_ si connette solo dopo che un utente ha eseguito l'accesso al dispositivo. Il tunnel utente consente agli utenti di accedere alle risorse dell'organizzazione tramite i server VPN.

A differenza del _tunnel utente_, che si connette solo dopo che un utente accede al dispositivo o al computer, il _tunnel del dispositivo_ consente alla VPN di stabilire la connettività prima che l'utente effettui l'accesso. Sia il tunnel del _dispositivo_ che il _tunnel utente_ operano in modo indipendente con i profili VPN, possono essere connessi nello stesso momento e possono usare metodi di autenticazione diversi e altre impostazioni di configurazione VPN in base alle esigenze. Il tunnel utente supporta SSTP e IKEv2 e il tunnel del dispositivo supporta solo IKEv2 senza supporto per il fallback SSTP.

Il tunnel utente è supportato nei dispositivi aggiunti a un dominio, non aggiunti a un dominio (gruppo di lavoro) o aggiunti a Azure AD per consentire gli scenari Enterprise e BYOD. È disponibile in tutte le edizioni di Windows e le funzionalità della piattaforma sono disponibili per terze parti tramite il supporto per il plug-in VPN UWP.

Il tunnel del dispositivo può essere configurato solo su dispositivi aggiunti a un dominio che eseguono Windows 10 Enterprise o Education versione 1709 o successiva. Non è disponibile alcun supporto per il controllo di terze parti del tunnel del dispositivo.


## <a name="device-tunnel-requirements-and-features"></a>Requisiti e funzionalità del tunnel del dispositivo
È necessario abilitare l'autenticazione del certificato del computer per le connessioni VPN e definire un'autorità di certificazione radice per l'autenticazione delle connessioni VPN in ingresso. 

```PowerShell
$VPNRootCertAuthority = “Common Name of trusted root certification authority”
$RootCACert = (Get-ChildItem -Path cert:LocalMachine\root | Where-Object {$_.Subject -Like “*$VPNRootCertAuthority*” })
Set-VpnAuthProtocol -UserAuthProtocolAccepted Certificate, EAP -RootCertificateNameToAccept $RootCACert -PassThru
```

![Funzionalità e requisiti del tunnel del dispositivo](../../media/device-tunnel-feature-and-requirements.png)

## <a name="vpn-device-tunnel-configuration"></a>Configurazione del tunnel del dispositivo VPN

Il codice XML del profilo di esempio seguente fornisce indicazioni valide per gli scenari in cui sono necessari solo pull avviati dal client sul tunnel del dispositivo.  I filtri di traffico vengono utilizzati per limitare il tunnel del dispositivo al solo traffico di gestione.  Questa configurazione funziona correttamente per gli scenari di Windows Update, di Criteri di gruppo (GP) e Microsoft endpoint Configuration Manager di aggiornamento, nonché per la connettività VPN per il primo accesso senza credenziali memorizzate nella cache o per gli scenari di reimpostazione della password. 

Per i casi di push avviati dal server, ad esempio Gestione remota Windows (WinRM), GPUpdate remoto e scenari di aggiornamento remoto Configuration Manager: è necessario consentire il traffico in ingresso nel tunnel del dispositivo, in modo che i filtri di traffico non possano essere utilizzati.  Se nel profilo del tunnel del dispositivo si attivano i filtri di traffico, il tunnel del dispositivo nega il traffico in ingresso.  Questa limitazione verrà rimossa nelle versioni future.


### <a name="sample-vpn-profilexml"></a>ProfileXML VPN di esempio

Di seguito è riportato il profileXML VPN di esempio.

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

A seconda delle esigenze di ogni particolare scenario di distribuzione, un'altra funzionalità VPN che può essere configurata con il tunnel del dispositivo è il [rilevamento della rete attendibile](https://social.technet.microsoft.com/wiki/contents/articles/38546.new-features-for-vpn-in-windows-10-and-windows-server-2016.aspx#Trusted_Network_Detection).

```
 <!-- inside/outside detection -->
  <TrustedNetworkDetection>corp.contoso.com</TrustedNetworkDetection>
```

## <a name="deployment-and-testing"></a>Distribuzione e test

È possibile configurare i tunnel dei dispositivi usando uno script di Windows PowerShell e il Bridge Strumentazione gestione Windows (WMI). È necessario configurare il tunnel del dispositivo VPN Always On nel contesto dell'account di **sistema locale** . A tale scopo, sarà necessario usare [PsExec](https://docs.microsoft.com/sysinternals/downloads/psexec), uno dei [PsTools](https://docs.microsoft.com/sysinternals/downloads/pstools) inclusi nella suite di utilità [Sysinternals](https://docs.microsoft.com/sysinternals/) .

Per le linee guida su come distribuire un per ogni dispositivo `(.\Device)` rispetto a un profilo di `(.\User)` per utente, vedere [uso dello scripting di PowerShell con il provider del Bridge WMI](https://docs.microsoft.com/windows/client-management/mdm/using-powershell-scripting-with-the-wmi-bridge-provider).

Eseguire il comando di Windows PowerShell seguente per verificare di avere distribuito correttamente un profilo di dispositivo:

  ```powershell
  Get-VpnConnection -AllUserConnection
  ```

L'output visualizza un elenco del dispositivo\-profili VPN Wide distribuiti nel dispositivo.

### <a name="example-windows-powershell-script"></a>Esempio di script di Windows PowerShell

È possibile usare il seguente script di Windows PowerShell per semplificare la creazione di uno script personalizzato per la creazione del profilo.

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

Di seguito sono riportate risorse aggiuntive per facilitare la distribuzione VPN.

### <a name="vpn-client-configuration-resources"></a>Risorse di configurazione del client VPN

Di seguito sono riportate le risorse di configurazione del client VPN.

- [Come creare profili VPN in Configuration Manager](https://docs.microsoft.com/configmgr/protect/deploy-use/create-vpn-profiles)
- [Configurare le connessioni VPN Always On client Windows 10](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)
- [Opzioni profilo VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-profile-options)

### <a name="remote-access-server-gateway-resources"></a>Risorse del gateway del server di accesso remoto

Di seguito sono riportate le risorse del gateway RAS (Remote Access Server).

- [Configurare RRAS con un certificato di autenticazione del computer](https://technet.microsoft.com/library/dd458982.aspx)
- [Risoluzione dei problemi relativi alle connessioni VPN IKEv2](https://technet.microsoft.com/library/dd941612.aspx)
- [Configurare l'accesso remoto basato su IKEv2](https://technet.microsoft.com/library/ff687731.aspx)

>[!IMPORTANT]
>Quando si usa il tunnel del dispositivo con un gateway RAS Microsoft, è necessario configurare il server RRAS per supportare l'autenticazione del certificato computer IKEv2 abilitando il metodo di autenticazione **Consenti autenticazione del certificato del computer per IKEv2** come descritto [qui](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee922682%28v=ws.10%29). Una volta abilitata questa impostazione, si consiglia vivamente di utilizzare il cmdlet di PowerShell **set-VpnAuthProtocol** , insieme al parametro facoltativo **RootCertificateNameToAccept** , per garantire che le connessioni RRAS IKEv2 siano consentite solo per i certificati client VPN concatenati a un'autorità di certificazione radice interna/privata definita in modo esplicito. In alternativa, è necessario modificare l'archivio **autorità di certificazione radice attendibili** nel server RRAS per assicurarsi che non contenga autorità di certificazione pubbliche, come illustrato [qui](https://blogs.technet.microsoft.com/rrasblog/2009/06/10/what-type-of-certificate-to-install-on-the-vpn-server/). Per altri gateway VPN può essere necessario considerare anche metodi simili.

