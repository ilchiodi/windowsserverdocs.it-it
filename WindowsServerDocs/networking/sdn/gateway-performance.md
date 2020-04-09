---
title: Prestazioni del gateway Windows Server 2019
manager: grcusanz
ms.prod: windows-server
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.author: anpaul
author: AnirbanPaul
ms.date: 08/22/2018
ms.openlocfilehash: 34890a5d93d6e2e214e401f5566cbb0ffde37508
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80855704"
---
# <a name="windows-server-2019-gateway-performance"></a>Prestazioni del gateway Windows Server 2019

>Si applica a: Windows Server


In Windows Server 2016, una delle preoccupazioni dei clienti è l'impossibilità del gateway SDN di soddisfare i requisiti di velocità effettiva delle reti moderne. La velocità effettiva di rete dei tunnel IPsec e GRE presentava alcune limitazioni con la velocità effettiva di connessione singola per la connettività IPsec di circa 300 Mbps e per la connettività GRE circa 2,5 Gbps.

Sono state migliorate significativamente in Windows Server 2019, con i numeri che salgono rispettivamente a 1,8 Gbps e 15 Gbps per le connessioni IPsec e GRE. Tutto questo, con riduzioni significative dei cicli CPU/per byte, garantendo così una velocità effettiva elevatissima delle prestazioni con un utilizzo molto inferiore della CPU.

## <a name="enable-high-performance-with-gateways-in-windows-server-2019"></a>Abilitare prestazioni elevate con i gateway in Windows Server 2019

Per le **connessioni GRE**, dopo la distribuzione o l'aggiornamento a Windows Server 2019 si basa sulle VM del gateway, è possibile visualizzare automaticamente le prestazioni migliorate. Non sono previsti passaggi manuali.

Per le **connessioni IPSec**, per impostazione predefinita, quando si crea la connessione per le reti virtuali, si ottengono i valori di percorso dati e prestazioni di Windows Server 2016. Per abilitare il percorso dati di Windows Server 2019, procedere come segue:

   1. In una macchina virtuale del gateway SDN passare alla console **dei servizi** (Services. msc).
   2. Trovare il servizio denominato **servizio gateway di Azure**e impostare il tipo di avvio su **automatico**.
   3. Riavviare la macchina virtuale del gateway.
      Le connessioni attive in questo gateway si failover a una VM gateway ridondante.
   4. Ripetere i passaggi precedenti per le altre macchine virtuali del gateway.

>[!TIP]
>Per ottenere risultati ottimali, verificare che le impostazioni cipherTransformationConstant e authenticationTransformConstant in quickMode della connessione IPsec utilizzino il pacchetto di crittografia **GCMAES256** .
>
>Per ottenere prestazioni ottimali, l'hardware host del gateway deve supportare i set di istruzioni CPU AES-NI e PCLMULQDQ. Sono disponibili in qualsiasi CPU Intel Westmere (32nm) e versioni successive, ad eccezione dei modelli in cui AES-NI è stato disabilitato. È possibile esaminare la documentazione del fornitore dell'hardware per verificare se la CPU supporta i set di istruzioni CPU AES-NI e PCLMULQDQ.

Di seguito è riportato un esempio REST di connessione IPsec con algoritmi di sicurezza ottimali:

```PowerShell
# NOTE: The virtual gateway must be created before creating the IPsec connection. More details here.
# Create a new object for Tenant Network IPsec Connection  
$nwConnectionProperties = New-Object Microsoft.Windows.NetworkController.NetworkConnectionProperties   

# Update the common object properties  
$nwConnectionProperties.ConnectionType = "IPSec"   
$nwConnectionProperties.OutboundKiloBitsPerSecond = 2000000   
$nwConnectionProperties.InboundKiloBitsPerSecond = 2000000  

# Update specific properties depending on the Connection Type  
$nwConnectionProperties.IpSecConfiguration = New-Object Microsoft.Windows.NetworkController.IpSecConfiguration   
$nwConnectionProperties.IpSecConfiguration.AuthenticationMethod = "PSK"   
$nwConnectionProperties.IpSecConfiguration.SharedSecret = "111_aaa"   

$nwConnectionProperties.IpSecConfiguration.QuickMode = New-Object Microsoft.Windows.NetworkController.QuickMode   
$nwConnectionProperties.IpSecConfiguration.QuickMode.PerfectForwardSecrecy = "PFS2048"   
$nwConnectionProperties.IpSecConfiguration.QuickMode.AuthenticationTransformationConstant = "GCMAES256"   
$nwConnectionProperties.IpSecConfiguration.QuickMode.CipherTransformationConstant = "GCMAES256"   
$nwConnectionProperties.IpSecConfiguration.QuickMode.SALifeTimeSeconds = 3600   
$nwConnectionProperties.IpSecConfiguration.QuickMode.IdleDisconnectSeconds = 500   
$nwConnectionProperties.IpSecConfiguration.QuickMode.SALifeTimeKiloBytes = 2000   

$nwConnectionProperties.IpSecConfiguration.MainMode = New-Object Microsoft.Windows.NetworkController.MainMode   
$nwConnectionProperties.IpSecConfiguration.MainMode.DiffieHellmanGroup = "Group2"   
$nwConnectionProperties.IpSecConfiguration.MainMode.IntegrityAlgorithm = "SHA256"   
$nwConnectionProperties.IpSecConfiguration.MainMode.EncryptionAlgorithm = "AES256"   
$nwConnectionProperties.IpSecConfiguration.MainMode.SALifeTimeSeconds = 28800
$nwConnectionProperties.IpSecConfiguration.MainMode.SALifeTimeKiloBytes = 2000   

# L3 specific configuration (leave blank for IPSec)  
$nwConnectionProperties.IPAddresses = @()   
$nwConnectionProperties.PeerIPAddresses = @()   

# Update the IPv4 Routes that are reachable over the site-to-site VPN Tunnel  
$nwConnectionProperties.Routes = @()   
$ipv4Route = New-Object Microsoft.Windows.NetworkController.RouteInfo   
$ipv4Route.DestinationPrefix = "<<On premise subnet that must be reachable over the VPN tunnel. Ex: 10.0.0.0/24>>"   
$ipv4Route.metric = 10   
$nwConnectionProperties.Routes += $ipv4Route   

# Tunnel Destination (Remote Endpoint) Address  
$nwConnectionProperties.DestinationIPAddress = "<<Public IP address of the On-Premise VPN gateway. Ex: 192.168.3.4>>"   

# Add the new Network Connection for the tenant. Note that the virtual gateway must be created before creating the IPsec connection. $uri is the REST URI of your deployment and must be in the form of “https://<REST URI>”  
New-NetworkControllerVirtualGatewayNetworkConnection -ConnectionUri $uri -VirtualGatewayId $virtualGW.ResourceId -ResourceId "Contoso_IPSecGW" -Properties $nwConnectionProperties -Force
```

## <a name="testing-results"></a>Risultati dei test

Sono stati eseguiti test delle prestazioni completi per i gateway SDN nei Lab di test. Nei test sono state confrontate le prestazioni della rete gateway con Windows Server 2019 in scenari SDN e in scenari non SDN. I risultati e i dettagli di installazione dei test sono disponibili nell'articolo del Blog [qui](https://blogs.technet.microsoft.com/networking/2018/08/15/high-performance-gateways/).

---