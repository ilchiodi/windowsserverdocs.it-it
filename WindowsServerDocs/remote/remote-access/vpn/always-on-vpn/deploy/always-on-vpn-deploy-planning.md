---
title: Pianificare la distribuzione di VPN Always On
description: In questo argomento fornisce istruzioni di pianificazione per la distribuzione VPN Always On in Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking-ras
ms.topic: article
ms.assetid: 3c9de3ec-4bbd-4db0-b47a-03507a315383
ms.localizationpriority: medium
ms.author: pashort
author: shortpatti
ms.date: 11/05/2018
ms.openlocfilehash: d629f04abda0ce22deb75e487f5b485f50a60a53
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59881912"
---
# <a name="step-1-plan-the-always-on-vpn-deployment"></a>Passaggio 1. Pianificare la distribuzione VPN Always On

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10


&#171;  [**Precedente:** Informazioni sul flusso di lavoro per la distribuzione VPN Always On](always-on-vpn-deploy-deployment.md)<br>
&#187;  [**Next:** Passaggio 2. Configurare l'infrastruttura di Server](vpn-deploy-server-infrastructure.md)

In questo passaggio, si inizia a pianificare e preparare la distribuzione VPN Always On. Prima di installare il ruolo server Accesso remoto al computer in cui che si prevede di utilizzare come server VPN, eseguire le attività seguenti. Dopo la corretta pianificazione, è possibile distribuire VPN Always On e, facoltativamente, configurare l'accesso condizionale per la connettività VPN con Azure AD. 

[!INCLUDE [always-on-vpn-standard-config-considerations-include](../../../includes/always-on-vpn-standard-config-considerations-include.md)]


## <a name="prepare-the-remote-access-server"></a>Preparare il Server di accesso remoto

È necessario eseguire le operazioni seguenti nel computer usato come server VPN: 

- **Assicurarsi che la configurazione hardware e software del server VPN sia corretta**. Installare Windows Server 2016 nel computer in cui si prevede di usare come server Accesso remoto VPN. Questo server deve avere due schede di rete fisiche installate, uno per la connessione alla rete perimetrale esterno e uno per la connessione alla rete perimetrale interna.

- **Identificare quale scheda di rete si connette a Internet e quale scheda di rete alla rete privata**. Configurare la scheda di rete con connessione Internet con un indirizzo IP pubblico, mentre la scheda con connessione Intranet può usare un indirizzo IP dalla rete locale.

    >[!TIP]
    >Se si preferisce non usare un indirizzo IP pubblico nella rete perimetrale, è possibile configurare il Firewall perimetrale con un indirizzo IP pubblico e quindi configurare il firewall per inoltrare le richieste di connessione VPN al server VPN.

- **Connettersi al server VPN alla rete**. Installare il server VPN in una rete perimetrale, tra i firewall perimetrali e firewall perimetrale.

## <a name="plan-authentication-methods"></a>Pianificare i metodi di autenticazione

IKEv2 è una VPN descritta nel protocollo di tunneling [Internet Engineering Task Force richiesta per i commenti 7296](https://datatracker.ietf.org/doc/rfc7296/). Il vantaggio principale di IKEv2 è che tollera interruzioni della connessione di rete sottostante. Ad esempio, se una perdita temporanea nella connessione o se un utente si sposta un computer client da una rete a un altro, quando che venga ristabilita la connessione di rete IKEv2 consente di ripristinare la connessione VPN automaticamente, senza l'intervento dell'utente.

>[!TIP]
>È possibile configurare il server di accesso remoto VPN per supportare le connessioni IKEv2 durante anche la disabilitazione di protocolli inutilizzati, riducendo così il footprint di sicurezza del server. 

## <a name="plan-ip-addresses-for-remote-clients"></a>Pianificare gli indirizzi IP per i client remoti

È possibile configurare il server VPN per assegnare gli indirizzi IP da un server DHCP o indirizzi ai client VPN da un pool di indirizzi statici configurato. 

## <a name="prepare-the-environment"></a>Preparare l'ambiente

- **Assicurarsi di disporre di autorizzazioni per configurare il firewall esterno e di disporre di un indirizzo IP pubblico valido**. Aprire le porte del firewall per supportare le connessioni VPN IKEv2. È anche necessario un indirizzo IP pubblico per accettare connessioni dai client esterni.

- **Scegliere un intervallo di indirizzi IP statici per i client VPN**. Determinare il numero massimo di client simultanei di VPN che si desidera supportare. Inoltre, pianificare un intervallo di indirizzi IP statici nella rete perimetrale interna per soddisfare tale requisito, vale a dire il *pool di indirizzi statici*. Se si usa DHCP per fornire indirizzi IP nella rete Perimetrale interno, si potrebbe essere necessario anche creare un'esclusione per gli indirizzi IP statici in DHCP.

- **Assicurarsi che è possibile modificare la zona DNS pubblica**. Aggiungere i record DNS per il dominio DNS pubblico per supportare l'infrastruttura VPN. 

- **Assicurarsi che tutti gli utenti VPN dispongono di account utente in utente di Active Directory \(AD DS\)**. Prima che gli utenti possono connettersi alla rete con le connessioni VPN, devono avere account utente in Active Directory Domain Services.

## <a name="prepare-routing-and-firewall"></a>Preparare il Routing e del Firewall 

Installare il server VPN nella rete perimetrale, che partiziona la rete perimetrale in una rete perimetrale interni ed esterni. A seconda dell'ambiente di rete, si potrebbe essere necessario apportare diverse modifiche al routing.

- **\(Facoltativo\) configurare il port forwarding.** Firewall perimetrale è necessario aprire le porte e protocolli ID associati a una VPN IKEv2 e li inoltrano al server VPN. Nella maggior parte degli ambienti, questa operazione richiede di configurare il port forwarding. Reindirizzare le porte Universal Datagram Protocol (UDP) 500 e 4500 per il server VPN.

- **Configurare il routing in modo che il server DNS e server VPN possano raggiungere Internet**. Questa distribuzione Usa IKEv2 e Network Address Translation \(NAT\). Assicurarsi che il server VPN possa raggiungere tutte le reti interne necessarie e le risorse di rete. Qualsiasi risorsa non è raggiungibile dal server VPN o rete è raggiungibile anche tramite connessioni VPN da posizioni remote.

Nella maggior parte degli ambienti, per raggiungere la nuova rete perimetrale interna, regolare le route statiche nel firewall periferico e nel server VPN. In ambienti più complessi, tuttavia, potrebbe essere necessario aggiungere le route statiche ai router interno o modificare le regole del firewall interno per il server VPN e il blocco di indirizzi IP associati con i client VPN.

## <a name="next-step"></a>Passaggio successivo
[Passaggio 2. Configurare l'infrastruttura Server](vpn-deploy-server-infrastructure.md): In questo passaggio installare e configurare i componenti lato server necessari per supportare la connessione VPN. I componenti lato server è inclusa la configurazione di infrastruttura a chiave pubblica per distribuire i certificati usati da utenti, il server VPN e il server NPS. 

---