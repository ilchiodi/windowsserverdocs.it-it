---
title: Rete privata virtuale (VPN)
description: È possibile utilizzare questo argomento per apprendere le funzionalità VPN di Windows 10 e Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: cd4908f0-0d6f-4c02-8f98-4dc88c3dcb65
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: bf38995f0a2b396044d1f45b45eff8c3c2de329d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59891072"
---
# <a name="virtual-private-networking-vpn"></a>Rete privata virtuale \(VPN\)

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows 10

## <a name="ras-gateway-as-a-single-tenant-vpn-server"></a>Gateway RAS come Server VPN Single-Tenant

In Windows Server 2016, il ruolo server Accesso remoto è un raggruppamento logico delle tecnologie di accesso di rete correlate seguenti.

- Remote Access Service (RAS)
- Routing
- Proxy applicazione Web

Queste tecnologie sono i servizi ruolo del ruolo del server Accesso remoto.

Quando si installa il ruolo server Accesso remoto con l'aggiunta guidata ruoli e funzionalità o Windows PowerShell, è possibile installare uno o più di questi servizi tre ruolo.

Quando si installa il **DirectAccess e VPN (RAS)** servizio ruolo, si sta distribuendo il Gateway di servizi di accesso remoto \( **Gateway RAS**\). È possibile distribuire una rete privata virtuale single-tenant RAS Gateway RAS Gateway \(VPN\) server che offre molte funzionalità avanzate e funzionalità più avanzate.

>[!NOTE]
>È anche possibile distribuire Gateway RAS come server VPN multi-tenant per l'utilizzo con Software Defined Networking \(SDN\), o come server DirectAccess. Per altre informazioni, vedere [Gateway RAS](https://docs.microsoft.com/windows-server/remote/remote-access/ras-gateway/ras-gateway), [reti SDN (Software Defined)](https://docs.microsoft.com/windows-server/networking/sdn/software-defined-networking), e [DirectAccess](https://docs.microsoft.com/windows-server/remote/remote-access/directaccess/directaccess).

## <a name="related-topics"></a>Argomenti correlati
- [Le funzionalità di VPN Always On](vpn-map-da.md): In questo argomento illustra le funzionalità di VPN Always On. 

- [Configurare il tunnel di dispositivo VPN in Windows 10](vpn-device-tunnel-config.md): VPN Always On offre la possibilità di creare un profilo VPN dedicato per computer o dispositivo. Le connessioni VPN Always On includono due tipi di tunnel: _tunnel periferica_ e _utente tunnel_. Tunnel di dispositivo viene usato per gli scenari di pre-logon connettività e gestione dei dispositivi. Tunnel utente consente agli utenti di accedere alle risorse dell'organizzazione tramite i server VPN.

- [Sempre nella distribuzione VPN per Windows Server 2016 e Windows 10](always-on-vpn/deploy/always-on-vpn-deploy.md): Fornisce istruzioni sulla distribuzione di accesso remoto come un singolo tenant VPN RAS Gateway per il punto\-a\-sito connessioni VPN da consentono ai dipendenti remoti per connettersi alla rete dell'organizzazione con le connessioni VPN Always On. È consigliabile rivedere la progettazione e alla distribuzione per ognuna delle tecnologie che vengono usate in questa distribuzione.

- [Guida tecnica di Windows 10 VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-guide): Illustra in dettaglio le scelte effettuate per i client Windows 10 nell'azienda soluzione VPN e come configurare la distribuzione. È possibile trovare i riferimenti a VPNv2 Configuration Service Provider (CSP) e fornisce la gestione dei dispositivi mobili le istruzioni di configurazione (MDM) con Microsoft Intune e il modello di profilo VPN per Windows 10.

- [Come creare profili VPN in System Center Configuration Manager](https://docs.microsoft.com/sccm/protect/deploy-use/create-vpn-profiles): In questo argomento descrive come creare profili VPN in System Center Configuration Manager (SCCM).

- [Configurare sempre i Client Windows 10 su connessioni VPN](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections): Questo argomento viene descritto il ProfileXML opzioni e dello schema e come creare la VPN ProfileXML. Dopo aver configurato l'infrastruttura di server, è necessario configurare i computer client Windows 10 per comunicare con tale infrastruttura con una connessione VPN. 

- [Opzioni del profilo VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-profile-options): Questo argomento descrive le impostazioni del profilo VPN in Windows 10 e informazioni su come configurare i profili VPN tramite SCCM o Intune. È possibile configurare tutte le impostazioni VPN in Windows 10 tramite il nodo ProfileXML in VPNv2 CSP.

---