---
title: Rete privata virtuale (VPN)
description: È possibile utilizzare questo argomento per informazioni sulle funzionalità e le funzionalità VPN di Windows Server 2016 e Windows 10.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: cd4908f0-0d6f-4c02-8f98-4dc88c3dcb65
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: 6b647d8cbf9586408b49c1519b57d32e596fce10
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71404224"
---
# <a name="virtual-private-networking-vpn"></a>Rete privata virtuale (VPN)

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows 10

## <a name="ras-gateway-as-a-single-tenant-vpn-server"></a>Gateway RAS come server VPN a tenant singolo

In Windows Server 2016, il ruolo del server accesso remoto è un raggruppamento logico delle tecnologie di accesso alla rete correlate seguenti.

- Servizio di accesso remoto (RAS)
- Routing
- Proxy applicazione Web

Queste tecnologie sono i servizi ruolo del ruolo del server accesso remoto.

Quando si installa il ruolo del server accesso remoto con l'aggiunta guidata ruoli e funzionalità o Windows PowerShell, è possibile installare uno o più di questi tre servizi ruolo.

Quando si installa il servizio ruolo **DirectAccess e VPN (RAS)** , si distribuisce il gateway del servizio di accesso remoto (**gateway RAS**). È possibile distribuire il gateway RAS come server VPN (Virtual Private Network) gateway RAS a tenant singolo che fornisce molte funzionalità avanzate e funzionalità avanzate.

>[!NOTE]
>È anche possibile distribuire il gateway RAS come server VPN multi-tenant per l'uso con SDN (Software Defined Networking) o come server DirectAccess. Per altre informazioni, vedere [gateway RAS](https://docs.microsoft.com/windows-server/remote/remote-access/ras-gateway/ras-gateway), [Software Defined Networking (SDN)](https://docs.microsoft.com/windows-server/networking/sdn/software-defined-networking)e [DirectAccess](https://docs.microsoft.com/windows-server/remote/remote-access/directaccess/directaccess).

## <a name="related-topics"></a>Argomenti correlati
- Funzionalità [e funzionalità VPN di always on](vpn-map-da.md): In questo argomento vengono illustrate le funzionalità e le funzionalità di Always On VPN. 

- [Configurare i tunnel del dispositivo VPN in Windows 10](vpn-device-tunnel-config.md): Always On VPN offre la possibilità di creare un profilo VPN dedicato per un dispositivo o un computer. Always On le connessioni VPN includono due tipi di tunnel: tunnel del _dispositivo_ e _tunnel utente_. Il tunnel del dispositivo viene usato per gli scenari di connettività di pre-accesso e per la gestione dei dispositivi. Il tunnel utente consente agli utenti di accedere alle risorse dell'organizzazione tramite i server VPN.

- [Always on la distribuzione VPN per Windows Server 2016 e Windows 10](always-on-vpn/deploy/always-on-vpn-deploy.md): Vengono fornite istruzioni sulla distribuzione di accesso remoto come gateway RAS VPN a tenant singolo per le connessioni VPN da punto a sito che consentono ai dipendenti remoti di connettersi alla rete dell'organizzazione con Always On connessioni VPN. Si consiglia di consultare le guide di progettazione e distribuzione per ognuna delle tecnologie utilizzate in questa distribuzione.

- [Guida tecnica VPN di Windows 10](https://docs.microsoft.com/windows/access-protection/vpn/vpn-guide): Vengono illustrate le decisioni da effettuare per i client Windows 10 nella soluzione VPN aziendale e come configurare la distribuzione. È possibile trovare riferimenti al provider del servizio di configurazione VPNv2 (CSP) e fornisce istruzioni di configurazione per la gestione di dispositivi mobili (MDM) usando Microsoft Intune e il modello di profilo VPN per Windows 10.

- [Come creare profili VPN in System Center Configuration Manager](https://docs.microsoft.com/sccm/protect/deploy-use/create-vpn-profiles): In questo argomento viene illustrato come creare profili VPN in System Center Configuration Manager (SCCM).

- [Configurare il client Windows 10 always on connessioni VPN](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections): Questo argomento descrive le opzioni e lo schema di ProfileXML e illustra come creare la VPN ProfileXML. Dopo aver configurato l'infrastruttura server di, è necessario configurare i computer client Windows 10 in modo che comunicano con tale infrastruttura con una connessione VPN.

- [Opzioni profilo VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-profile-options): In questo argomento vengono descritte le impostazioni del profilo VPN in Windows 10 e viene illustrato come configurare i profili VPN con Intune o SCCM. È possibile configurare tutte le impostazioni VPN in Windows 10 usando il nodo ProfileXML in VPNv2 CSP.
