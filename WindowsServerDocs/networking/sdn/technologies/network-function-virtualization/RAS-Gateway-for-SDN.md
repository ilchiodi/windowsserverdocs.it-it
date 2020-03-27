---
title: Gateway RAS per SDN
description: È possibile utilizzare questo argomento per informazioni sul gateway RAS, ovvero un router con supporto BGP (software-based, multi-tenant Border Gateway Protocol) in Windows Server 2016.
manager: brianlic
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: networking-sdn
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a32357a5-ab1a-4a4c-848a-7a4ed65b1921
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 415a5f4728b1b08e59630935e47f22d74b0267e8
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80312936"
---
# <a name="ras-gateway-for-sdn"></a>Gateway RAS per SDN

>Si applica a: Windows Server (canale semestrale), Windows Server 2016 # # gateway RAS per SDN  


Il gateway RAS è un router con supporto per software, multi-tenant, Border Gateway Protocol (BGP) progettato per i provider di servizi cloud (CSP) e per le aziende che ospitano più reti virtuali tenant con la virtualizzazione di rete Hyper-V. Gateway RAS instrada il traffico di rete tra la rete fisica e le risorse di rete della macchina virtuale, indipendentemente dalla posizione. È possibile instradare il traffico di rete nella stessa posizione fisica o in molte posizioni diverse.   

Il multitenant è la capacità di un'infrastruttura cloud di supportare i carichi di lavoro delle macchine virtuali di più tenant, ma di isolarli tra loro, mentre tutti i carichi di lavoro vengono eseguiti nella stessa infrastruttura. I carichi di lavoro multipli di un singolo tenant possono connettersi tra loro ed essere gestiti in modalità remota, ma questi sistemi non sono collegati con i carichi di lavoro di altri tenant né altri tenant possono gestirli in modalità remota.

  
> [!NOTE]  
> Oltre a questo argomento, sono disponibili i seguenti argomenti del gateway RAS.  
>   
> -   [Novità del gateway RAS](../../../sdn/technologies/network-function-virtualization/What-s-New-in-RAS-Gateway.md)  
> -   [Architettura di distribuzione gateway RAS](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-Deployment-Architecture.md)  
> -   [Disponibilità elevata del gateway RAS](../../../sdn/technologies/network-function-virtualization/RAS-Gateway-High-Availability.md)  
> -   [Border Gateway Protocol &#40;BGP&#41;](../../../../remote/remote-access/bgp/Border-Gateway-Protocol-BGP.md)  
> -   [Riferimento per i comandi di Windows PowerShell per BGP](../../../../remote/remote-access/bgp/BGP-Windows-PowerShell-Command-Reference.md)  
  
    
## <a name="prerequisites-for-installing-ras-gateway-for-sdn"></a>Prerequisiti per l'installazione del gateway RAS per SDN  
Non è possibile usare l'interfaccia di Windows per installare accesso remoto quando si vuole distribuire il gateway RAS in modalità multi-tenant per l'uso con SDN. È invece necessario utilizzare Windows PowerShell.  
  
Tuttavia, prima di poter installare il gateway RAS usando Windows PowerShell, è necessario usare Windows PowerShell per aggiungere la funzionalità **RemoteAccess** di Windows. A tale scopo, eseguire il comando seguente al prompt di Windows PowerShell.  
  
`Add-WindowsFeature -Name RemoteAccess -IncludeAllSubFeature -IncludeManagementTools`  
  
Questo comando aggiunge la funzionalità **RemoteAccess** e i comandi di Windows PowerShell per la funzionalità.  
  
Dopo aver aggiunto **RemoteAccess** al server, è possibile installare accesso remoto come gateway RAS con modalità multi-tenant e Border Gateway Protocol (BGP).  
  
Per ulteriori informazioni, vedere l'argomento di riferimento di Windows PowerShell [Install-RemoteAccess](https://technet.microsoft.com/library/hh918408.aspx).  
  
## <a name="ras-gateway-features"></a>Funzionalità del gateway RAS  
Di seguito sono riportate le funzionalità del gateway RAS in Windows Server 2016. È possibile distribuire il gateway RAS nei pool a disponibilità elevata che usano tutte queste funzionalità in una sola volta.  
  
-   **VPN da sito a sito**. Questa funzionalità del gateway RAS consente di connettere due reti in posizioni fisiche diverse su Internet tramite una connessione VPN da sito a sito. Per i CSP che ospitano molti tenant nel proprio Data Center, il gateway RAS fornisce una soluzione gateway multi-tenant che consente ai tenant di accedere alle proprie risorse e gestirle tramite connessioni VPN da sito a sito da siti remoti e che consente il flusso del traffico di rete tra risorse virtuali nel Data Center e nella rete fisica.  
  
-   **VPN da punto a sito**. Questa funzionalità gateway RAS consente ai dipendenti o agli amministratori dell'organizzazione di connettersi alla rete dell'organizzazione da posizioni remote.  Per le distribuzioni multi-tenant, gli amministratori di rete tenant possono usare connessioni VPN da punto a sito per accedere alle risorse di rete virtuale nel data center CSP.  
  
-   **Tunneling GRE**. I tunnel basati su GRE (Generic Routing Encapsulation) consentono la connettività tra reti virtuali tenant e reti esterne. Poiché il protocollo GRE è leggero e il supporto per GRE è disponibile nella maggior parte dei dispositivi di rete, questo protocollo rappresenta la scelta ideale per il tunneling nei casi in cui non è richiesta la crittografia dei dati. Il supporto di GRE nei tunnel da sito a sito (S2S) risolve il problema dell'invio tra reti virtuali tenant e reti esterne tenant tramite un gateway multi-tenant, come descritto più avanti in questo argomento.  
  
-   **Routing dinamico con Border Gateway Protocol (BGP)** . BGP, essendo un protocollo di routing dinamico che apprende automaticamente le route tra i siti connessi da connessioni VPN da sito a sito, riduce la necessità di configurare manualmente le route sui router. Se l'organizzazione dispone di più siti connessi tramite router abilitati per BGP, ad esempio gateway RAS, BGP consente ai router di calcolare e utilizzare automaticamente route valide in caso di interruzione della rete o di errore. Per ulteriori informazioni, vedere [RFC 4271](https://tools.ietf.org/html/rfc4271).  
  

  


