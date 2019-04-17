---
title: Rete privata virtuale (VPN)
description: È possibile utilizzare questo argomento per informazioni sulle caratteristiche e funzionalità VPN di Windows 10 e Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: cd4908f0-0d6f-4c02-8f98-4dc88c3dcb65
ms.date: 11/05/2018
ms.author: pashort
author: shortpatti
ms.localizationpriority: medium
ms.openlocfilehash: bf38995f0a2b396044d1f45b45eff8c3c2de329d
ms.sourcegitcommit: 4893d79345cea85db427224bb106fc1bf88ffdbc
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/05/2018
ms.locfileid: "6067302"
---
# \(VPN\) di rete privata virtuale

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows 10

## Gateway RAS come Server VPN singolo Tenant

In Windows Server 2016, il ruolo del server Accesso remoto è un raggruppamento logico di tecnologie di accesso della rete correlati seguenti.

- Servizio di accesso remoto (RAS)
- Routing
- Proxy applicazione Web

Queste tecnologie sono i servizi ruolo del ruolo del server Accesso remoto.

Quando si installa il ruolo del server Accesso remoto con Aggiungi ruoli e funzionalità della procedura guidata Windows PowerShell, è possibile installare uno o più di questi tre servizi ruolo.

Quando installi il servizio ruolo **DirectAccess e VPN (RAS)** , stai distribuendo il Gateway di servizio di accesso remoto \ (**Gateway RAS**\). È possibile distribuire Gateway RAS come un server singolo tenant Gateway RAS virtual private network \(VPN\) che offre molte funzionalità avanzate e funzionalità avanzate.

>[!NOTE]
>Puoi anche distribuire Gateway RAS come server VPN multi-tenant per l'uso con Software Defined Networking \(SDN\) o come un server DirectAccess. Per altre informazioni, vedi [Gateway RAS](https://docs.microsoft.com/windows-server/remote/remote-access/ras-gateway/ras-gateway), [DirectAccess](https://docs.microsoft.com/windows-server/remote/remote-access/directaccess/directaccess)e [Rete SDN (Software Defined)](https://docs.microsoft.com/windows-server/networking/sdn/software-defined-networking).

## Argomenti correlati
- [Caratteristiche e funzionalità VPN always On](vpn-map-da.md): In questo argomento, scoprire le funzionalità di VPN Always On. 

- [Configurare i tunnel dispositivo VPN in Windows 10](vpn-device-tunnel-config.md): VPN Always On offre la possibilità di creare un profilo VPN dedicato per dispositivo o un computer. Le connessioni VPN Always On includono due tipi di tunnel: _tunnel dei dispositivi_ e _utenti tunnel_. Tunnel dei dispositivi viene utilizzato per scopi di gestione dispositivi e scenari di connettività di pre-accesso. Tunnel utente consente agli utenti di accedere a risorse dell'organizzazione tramite server VPN.

- [Sempre in VPN distribuzione per Windows Server 2016 e Windows 10](always-on-vpn/deploy/always-on-vpn-deploy.md): fornisce istruzioni sulla distribuzione di accesso remoto come un singolo tenant di Gateway RAS VPN per le connessioni VPN point\-to\-sito che consentono ai dipendenti remoti di connettersi alla tua organizzazione rete con connessioni VPN Always On. Si consiglia di esaminare le guide alla distribuzione e progettazione per ciascuna delle tecnologie che vengono utilizzate durante la distribuzione.

- [Guida tecnica alle reti VPN Windows 10](https://docs.microsoft.com/windows/access-protection/vpn/vpn-guide): illustra le decisioni da prendere per i client Windows 10 nella tua soluzione VPN aziendale e su come configurare la distribuzione. Puoi trovare i riferimenti a VPNv2 Configuration Service Provider (CSP) e offre le istruzioni di configurazione (MDM) con Microsoft Intune e il modello di profilo VPN per Windows 10 mobile device management.

- [Come i profili VPN di creare in System Center Configuration Manager](https://docs.microsoft.com/sccm/protect/deploy-use/create-vpn-profiles): In questo argomento, scopriremo come creare profili VPN in System Center Configuration Manager (SCCM).

- [Configurare Windows 10 Client sempre su connessioni VPN](https://docs.microsoft.com/windows-server/remote/remote-access/vpn/always-on-vpn/deploy/vpn-deploy-client-vpn-connections): in questo argomento descrive il ProfileXML opzioni e lo schema e come creare la VPN ProfileXML. Dopo aver configurato l'infrastruttura di server, è necessario configurare i computer client Windows 10 per comunicare con tale infrastruttura con una connessione VPN. 

- [Opzioni del profilo VPN](https://docs.microsoft.com/windows/access-protection/vpn/vpn-profile-options): in questo argomento descrive le impostazioni del profilo VPN in Windows 10 e Scopri come configurare i profili VPN con Intune o SCCM. È possibile configurare tutte le impostazioni VPN in Windows 10 tramite il nodo ProfileXML nel CSP VPNv2.

---