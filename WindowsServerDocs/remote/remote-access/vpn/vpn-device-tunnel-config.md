---
title: Configurare il tunnel dei dispositivi VPN in Windows 10
description: Scopri come creare un tunnel dei dispositivi VPN in Windows 10.
ms.prod: windows-server-threshold
ms.date: 11/05/2018
ms.technology: networking-ras
ms.topic: article
ms.assetid: 158b7a62-2c52-448b-9467-c00d5018f65b
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: 005721873ad3a0df942bc9e23eba13728965ccba
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067310"
---
# Configurare i dispositivi tunnel VPN in Windows 10

>Si applica a: Windows 10 versione 1709

VPN Always On offre la possibilità di creare un profilo VPN dedicato per dispositivo o un computer. Le connessioni VPN Always On includono due tipi di tunnel: 

- _Tunnel dei dispositivi_ si connette al server VPN specificato prima di utenti di accedere al dispositivo. Scenari di connettività di pre-accesso e scopi di gestione del dispositivo usano tunnel dei dispositivi.

- _Tunnel utente_ si connette solo dopo che un utente accede al dispositivo. Tunnel utente consente agli utenti di accedere a risorse dell'organizzazione tramite server VPN.

A differenza di _tunnel utente_, che connette solo dopo un utente accede il dispositivo o un computer, _al tunnel dei dispositivi_ consente la connessione VPN stabilire la connessione prima che l'utente accede. Sia _al tunnel dei dispositivi_ e _utenti tunnel_ funzionano in modo indipendente con i profili VPN, possono essere connessi allo stesso tempo e possono usare diversi metodi di autenticazione e altre impostazioni di configurazione VPN come appropriato. Tunnel utente supporta il protocollo SSTP e IKEv2 e tunnel dei dispositivi supporta IKEv2 solo con nessun supporto per il fallback SSTP.

Tunnel utente è supportata in appartenenti al dominio, appartenenti a non di dominio (gruppo di lavoro) o AD – dispositivi aggiunti ad Azure per consentire per gli scenari BYOD e aziendali. È disponibile in tutte le edizioni di Windows e le funzionalità della piattaforma sono disponibili a terze parti tramite il supporto del plug-in VPN UWP.

Tunnel dei dispositivi possa essere configurato solo nei dispositivi aggiunti al dominio che esegue Windows 10 Enterprise o Education versione 1709 o successiva. Non esiste alcun supporto per il controllo di terze parti del tunnel dei dispositivi.


## Requisiti Tunnel dei dispositivi e funzionalità
Devi abilitare l'autenticazione del certificato computer per le connessioni VPN e definire un'autorità di certificazione radice per l'autenticazione di connessioni VPN in ingresso. 

```PowerShell
$VPNRootCertAuthority = “Common Name of trusted root certification authority”
$RootCACert = (Get-ChildItem -Path cert:LocalMachine\root | Where-Object {$_.Subject -Like “*$VPNRootCertAuthority*” })
Set-VpnAuthProtocol -UserAuthProtocolAccepted Certificate, EAP -RootCertificateNameToAccept $RootCACert -PassThru
```

![Requisiti e funzionalità Tunnel dei dispositivi](../../media/device-tunnel-feature-and-requirements.png)

## Configurazione Tunnel dei dispositivi VPN

L'esempio di profilo XML riportato di seguito vengono fornite indicazioni ottimale per gli scenari in cui solo client avviato stacca sono necessari tramite il tunnel dei dispositivi.  Filtri del traffico vengono usati per limitare il tunnel dei dispositivi a solo il traffico di gestione.  Questa configurazione è ideale per Windows Update, tipico criteri di gruppo e gli scenari di aggiornamento di System Center Configuration Manager (SCCM), nonché la connettività VPN per primo accesso senza credenziali memorizzate nella cache o scenari di reimpostazione della password. 

Per i casi di push avviata dal server, come la gestione remota Windows (WinRM), GPUpdate remoto e scenari di aggiornamento SCCM remoti – è necessario consentire il traffico in ingresso il tunnel dei dispositivi, in modo che non possono essere usati filtri del traffico.  Se nel profilo tunnel dispositivo puoi attivare filtri del traffico, quindi il Tunnel dei dispositivi Nega il traffico in ingresso.  Questa limitazione sta per essere rimosse nelle versioni future.


### Esempio VPN profileXML

Di seguito è il profileXML VPN di esempio.

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

A seconda delle esigenze di ogni scenario di distribuzione specifico, un'altra funzionalità VPN che può essere configurata con il tunnel dei dispositivi è [Trusted Network Detection](https://social.technet.microsoft.com/wiki/contents/articles/38546.new-features-for-vpn-in-windows-10-and-windows-server-2016.aspx#Trusted_Network_Detection).

```
 <!-- inside/outside detection --> 
  <TrustedNetworkDetection>corp.contoso.com</TrustedNetworkDetection> 
```

## Distribuzione e test

È possibile configurare tunnel dispositivo usando uno script di Windows PowerShell e con il bridge \(WMI\) Strumentazione gestione Windows. Il tunnel dei dispositivi VPN Always On deve essere configurato nel contesto dell'account di **Sistema locale** . A tale scopo, sarà necessario utilizzare [PsExec](https://docs.microsoft.com/sysinternals/downloads/psexec), uno della [Panoramica](https://docs.microsoft.com/sysinternals/downloads/pstools) incluso nel gruppo di [Sysinternals](https://docs.microsoft.com/sysinternals/) di utilità.

Per le linee guida su come distribuire una per ogni dispositivo `(.\Device)` e una per ogni utente `(.\User)` del profilo, vedi [script di PowerShell usando con il Provider di Bridge WMI](https://docs.microsoft.com/windows/client-management/mdm/using-powershell-scripting-with-the-wmi-bridge-provider). 

Esegui il seguente comando Windows PowerShell per verificare di aver distribuito correttamente un profilo del dispositivo:

    `Get-VpnConnection -AllUserConnection`

L'output visualizza un elenco dei profili VPN device\ a livello che vengono distribuite nel dispositivo.


### Script di esempio Windows PowerShell

È possibile utilizzare il seguente script di Windows PowerShell per facilitare la creazione di uno script personalizzato per la creazione di profilo.

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

## Risorse aggiuntive

Di seguito sono risorse aggiuntive per assistere l'utente con la distribuzione di VPN.

### Risorse di configurazione client VPN

Si tratta di risorse di configurazione client VPN.

- [Come i profili VPN di creare in System Center Configuration Manager](https://docs.microsoft.com/sccm/protect/deploy-use/create-vpn-profiles)
- [Configurare le connessioni VPN Always On dei client Windows 10](always-on-vpn/deploy/vpn-deploy-client-vpn-connections.md)
- [Opzioni del profilo VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-profile-options)

### \(RAS\) Server di accesso remoto risorse Gateway

Di seguito sono le risorse di Gateway RAS.

- [Configurare RRAS con un certificato di autenticazione di Computer](https://technet.microsoft.com/library/dd458982.aspx)
- [Risoluzione dei problemi di connessioni VPN IKEv2](https://technet.microsoft.com/library/dd941612.aspx)
- [Configurare l'accesso remoto basate su IKEv2](https://technet.microsoft.com/library/ff687731.aspx)

>[!IMPORTANT]
>Quando usi Tunnel dei dispositivi con un gateway RAS Microsoft, dovrai configurare il server RRAS per supportare l'autenticazione del certificato computer IKEv2 abilitando il metodo di autenticazione di **autenticazione del certificato Consenti computer per IKEv2** , come descritto [di seguito](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/ee922682%28v=ws.10%29). Una volta che questa impostazione è abilitata, è consigliabile che il cmdlet di PowerShell **Set-VpnAuthProtocol** , insieme al parametro facoltativo **RootCertificateNameToAccept** , viene utilizzato per garantire che le connessioni RRAS IKEv2 sono consentite solo per Client VPN certificati a una definiti in modo esplicito internal/private autorità di certificazione radice. In alternativa, l'archivio delle **Autorità di certificazione radice attendibili** per il server RRAS deve essere modificato per garantire che non contiene autorità di certificazione pubbliche come illustrato [](https://blogs.technet.microsoft.com/rrasblog/2009/06/10/what-type-of-certificate-to-install-on-the-vpn-server/). Metodi simili potrebbe essere necessario anche essere considerati per altri gateway VPN.

---
