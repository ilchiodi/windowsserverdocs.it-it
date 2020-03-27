---
title: Pianificare la distribuzione di VPN Always On
description: Questo argomento fornisce istruzioni di pianificazione per la distribuzione di Always On VPN in Windows Server 2016.
ms.prod: windows-server
ms.technology: networking-ras
ms.topic: article
ms.assetid: 3c9de3ec-4bbd-4db0-b47a-03507a315383
ms.localizationpriority: medium
ms.author: lizross
author: eross-msft
ms.date: 11/05/2018
ms.openlocfilehash: e0e061a38170242a3808fbad0c82a4154bf9c536
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80313299"
---
# <a name="step-1-plan-the-always-on-vpn-deployment"></a>Passaggio 1. Pianificare la distribuzione di VPN Always On

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows 10

- [**Precedente:** Informazioni sul flusso di lavoro per la distribuzione di Always On VPN](always-on-vpn-deploy-deployment.md)
- [Passaggio **successivo:** Passaggio 2. Configurare l'infrastruttura server](vpn-deploy-server-infrastructure.md)

In questo passaggio si inizia a pianificare e preparare la distribuzione di Always On VPN. Prima di installare il ruolo del server accesso remoto nel computer che si sta pianificando di usare come server VPN, eseguire le attività seguenti. Dopo la pianificazione corretta, è possibile distribuire Always On VPN e, facoltativamente, configurare l'accesso condizionale per la connettività VPN usando Azure AD.

[!INCLUDE [always-on-vpn-standard-config-considerations-include](../../../includes/always-on-vpn-standard-config-considerations-include.md)]

## <a name="prepare-the-remote-access-server"></a>Preparare il server di accesso remoto

Nel computer utilizzato come server VPN è necessario eseguire le operazioni seguenti:

- **Verificare che la configurazione hardware e software del server VPN sia corretta**. Installare Windows Server 2016 nel computer che si intende usare come server VPN di accesso remoto. Questo server deve disporre di due schede di rete fisiche installate, una per la connessione alla rete perimetrale esterna e una per la connessione alla rete perimetrale interna.

- **Identificare la scheda di rete connessa a Internet e la scheda di rete connessa alla rete privata**. Configurare la scheda di rete che si trova in Internet con un indirizzo IP pubblico, mentre la scheda che si trova sulla rete Intranet può utilizzare un indirizzo IP della rete locale.

    >[!TIP]
    >Se si preferisce non usare un indirizzo IP pubblico nella rete perimetrale, è possibile configurare il firewall perimetrale con un indirizzo IP pubblico e quindi configurare il firewall in modo che inoltri le richieste di connessione VPN al server VPN.

- **Connettere il server VPN alla rete**. Installare il server VPN in una rete perimetrale tra il firewall perimetrale e il firewall perimetrale.

## <a name="plan-authentication-methods"></a>Pianificare i metodi di autenticazione

IKEv2 è un protocollo di tunneling VPN descritto in [Internet Engineering Task Force Request per i commenti 7296](https://datatracker.ietf.org/doc/rfc7296/). Il vantaggio principale di IKEv2 è che tollera le interruzioni nella connessione di rete sottostante. Se, ad esempio, si verifica una perdita temporanea di connessione o se un utente sposta un computer client da una rete a un'altra, quando viene ristabilita la connessione di rete IKEv2 ripristina automaticamente la connessione VPN, senza l'intervento dell'utente.

>[!TIP]
>È possibile configurare il server VPN di accesso remoto per supportare le connessioni IKEv2, disabilitando al tempo stesso i protocolli non usati, riducendo il footprint di sicurezza del server. 

## <a name="plan-ip-addresses-for-remote-clients"></a>Pianificare gli indirizzi IP per i client remoti

È possibile configurare il server VPN per assegnare indirizzi ai client VPN da un pool di indirizzi statici configurato o da indirizzi IP da un server DHCP. 

## <a name="prepare-the-environment"></a>Preparare l'ambiente

- Assicurarsi **di disporre delle autorizzazioni per configurare il firewall esterno e di disporre di un indirizzo IP pubblico valido**. Aprire le porte del firewall per supportare le connessioni VPN IKEv2. È necessario anche un indirizzo IP pubblico per accettare le connessioni da client esterni.

- **Scegliere un intervallo di indirizzi IP statici per i client VPN**. Determinare il numero massimo di client VPN simultanei che si desidera supportare. Pianificare anche un intervallo di indirizzi IP statici nella rete perimetrale interna per soddisfare tale requisito, ovvero il pool di *indirizzi statici*. Se si usa DHCP per fornire gli indirizzi IP nella rete perimetrale interna, potrebbe essere necessario anche creare un'esclusione per gli indirizzi IP statici in DHCP.

- Assicurarsi **di poter modificare la zona DNS pubblica**. Aggiungere i record DNS al dominio DNS pubblico per supportare l'infrastruttura VPN. 

- Assicurarsi **che tutti gli utenti VPN dispongano di account utente in Active Directory utente (ad DS)** . Prima che gli utenti possano connettersi alla rete con le connessioni VPN, è necessario che dispongano di account utente in servizi di dominio Active Directory.

## <a name="prepare-routing-and-firewall"></a>Preparare il routing e il firewall 

Installare il server VPN all'interno della rete perimetrale, che esegue il partizionamento della rete perimetrale in reti perimetrali interne ed esterne. A seconda dell'ambiente di rete, potrebbe essere necessario apportare diverse modifiche al routing.

- **Opzionale Configurare l'invio di porte.** Il firewall perimetrale deve aprire le porte e gli ID di protocollo associati a una VPN IKEv2 e inviarli al server VPN. Nella maggior parte degli ambienti, in questo modo è necessario configurare l'invio delle porte. Reindirizza le porte UDP (Universal Datagram Protocol) 500 e 4500 al server VPN.

- **Configurare il routing in modo che i server DNS e i server VPN possano raggiungere Internet**. Questa distribuzione USA IKEv2 e Network Address Translation (NAT). Verificare che il server VPN sia in grado di raggiungere tutte le reti interne e le risorse di rete necessarie. Non è possibile raggiungere qualsiasi rete o risorsa irraggiungibile dal server VPN tramite connessioni VPN da posizioni remote.

Nella maggior parte degli ambienti, per raggiungere la nuova rete perimetrale interna, modificare le route statiche nel firewall perimetrale e nel server VPN. In ambienti più complessi, tuttavia, potrebbe essere necessario aggiungere route statiche ai router interni o modificare le regole interne del firewall per il server VPN e il blocco di indirizzi IP associati ai client VPN.

## <a name="next-steps"></a>Passaggi successivi

[Passaggio 2. Configurare l'infrastruttura server](vpn-deploy-server-infrastructure.md): in questo passaggio si installano e configurano i componenti lato server necessari per supportare la VPN. I componenti lato server includono la configurazione dell'infrastruttura a chiave pubblica per distribuire i certificati utilizzati dagli utenti, dal server VPN e dal server dei criteri di rete.