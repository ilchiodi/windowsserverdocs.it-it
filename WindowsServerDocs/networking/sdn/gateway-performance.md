---
title: Prestazioni del Gateway Windows Server 2019
description: ''
manager: dougkim
ms.prod: windows-server-threshold
ms.technology: networking-hv-switch
ms.topic: get-started-article
ms.assetid: ''
ms.author: pashort
author: shortpatti
ms.date: 08/22/2018
ms.openlocfilehash: a6530b29ce7ffb0d18e0266e70cb2ca45188915c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845632"
---
# <a name="windows-server-2019-gateway-performance"></a>Prestazioni del Gateway Windows Server 2019

>Si applica a: Windows Server


In Windows Server 2016, una delle preoccupazioni dei clienti era l'impossibilità di gateway di rete SDN per soddisfare i requisiti di velocità effettiva delle reti moderne. La velocità effettiva di rete del tunnel IPsec e GRE soggetto a limitazioni con la velocità effettiva di singola connessione per la connettività IPsec in corso di circa 300 MB/s e per la connettività GRE da circa 2,5 GB/s.

Sono state migliorate significativamente in Windows Server 2019, con i numeri di salita a 1,8 Gbps e 15 Gbps per le connessioni IPsec e GRE, rispettivamente. Tutte queste funzioni, con riduzioni significative dei cicli di CPU / per byte, offrendo così la velocità effettiva ad alte prestazioni con molto minore utilizzo della CPU.

## <a name="enable-high-performance-with-gateways-in-windows-server-2019"></a>Abilitati consentano alte prestazioni con i gateway in Windows Server 2019

Per la **connessioni GRE**, una volta distribuzione e aggiornamento per le build di Windows Server 2019 sulle VM gateway, verrà visualizzato automaticamente il miglioramento delle prestazioni. Richiedere passaggi manuali non sono coinvolti.

Per la **le connessioni IPsec**, per impostazione predefinita, quando si crea la connessione per le reti virtuali, otterrai i numeri di percorso e le prestazioni dei dati Windows Server 2016. Per abilitare il percorso dei dati di Windows Server 2019, eseguire le operazioni seguenti:

   1. In un gateway di rete SDN della macchina virtuale, passare a **Services** console (Services. msc).
   2. Trovare il servizio denominato **servizio Gateway di Azure**e impostare il tipo di avvio **automatica**.
   3. Riavviare la macchina virtuale gateway.
      Le connessioni attive in questo failover del gateway a un gateway con ridondanza della macchina virtuale.
   4. Ripetere i passaggi precedenti per rest di macchine virtuali gateway.

>[!TIP]
>Per prestazioni ottimali, verificare che il cipherTransformationConstant e authenticationTransformConstant nelle impostazioni di modalità rapida di connessione IPsec utilizza il **GCMAES256** pacchetto di crittografia.
>
>Per ottenere prestazioni ottimali, l'hardware del gateway host deve supportare AES-NI e set di istruzioni CPU PCLMULQDQ. Sono disponibili su qualsiasi Westmere (32nm) e versioni successive CPU Intel, ad eccezione per i modelli in cui è stata disabilitata AES-NI. È possibile esaminare la documentazione del fornitore dell'hardware per verificare se la CPU supporta AES-NI e la CPU PCLMULQDQ istruzione imposta.

Di seguito è riportato un esempio di REST della connessione IPsec con gli algoritmi di sicurezza ottimale:

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

## <a name="testing-results"></a>I risultati dei test

È stato eseguito il test per i gateway di rete SDN nei nostri laboratori di test delle prestazioni complete. Nei test, è stato confrontato con le prestazioni di rete gateway con Windows Server 2019 negli scenari SDN e gli scenari non SDN. È possibile trovare i risultati e testare i dettagli di installazione acquisiti nell'articolo di blog [qui](https://blogs.technet.microsoft.com/networking/2018/08/15/high-performance-gateways/).

---