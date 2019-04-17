---
title: Gestire le macchine virtuali IaaS di Azure
description: Gestione delle macchine virtuali IaaS di Azure con Windows Admin Center (Project Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 09/07/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: 6f162294076d7e12df09f31b0bbd7d9244abe679
ms.sourcegitcommit: f1edfc6525e09dd116b106293f9260123a94de0c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/12/2019
ms.locfileid: "9297014"
---
# Gestire le macchine virtuali IaaS di Azure con Windows Admin Center

È possibile utilizzare Windows Admin Center per gestire le macchine virtuali di Azure, nonché computer locale. Sono disponibili diverse configurazioni possibili, scegliere la configurazione che abbia senso per l'ambiente:
- [Gestire macchine virtuali di Azure da un gateway Windows Admin Center in locale](#manage-with-an-on-premises-windows-admin-center-gateway)
- [Gestire macchine virtuali di Azure da un gateway Windows Admin Center installato in una macchina virtuale di Azure](#use-a-windows-admin-center-gateway-deployed-in-azure)

## Gestire con un gateway Windows Admin Center in locale

Se hai già installato Windows Admin Center in un gateway in locale (sia in Windows 10 o Windows Server 2016), è possibile utilizzare questo stesso gateway per gestire Windows 10 o Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2 Macchine virtuali in Azure. 

### La connessione alle macchine virtuali con un indirizzo IP pubblico

Se la destinazione di macchine virtuali (le macchine virtuali che desideri gestire con Windows Admin Center) hanno gli indirizzi IP pubblici, aggiungerle al tuo gateway Windows Admin Center in base all'indirizzo IP o dal nome di dominio completo. Ci sono due considerazioni di cui tenere conto:

- È necessario abilitare WinRM accesso alla macchina virtuale di destinazione eseguendo il comando seguente in PowerShell o il prompt dei comandi nella destinazione della macchina virtuale: `winrm quickconfig`
- Se si non ancora aggiunti al dominio macchina virtuale di Azure, la macchina virtuale si comporta come un server nel gruppo di lavoro, pertanto è necessario prendere in che considerazione per [l'uso di Windows Admin Center in un gruppo di lavoro](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup).
- Anche abilitare connessioni in ingresso alla porta 5985 per WinRM su HTTP in modo che Windows Admin Center gestire la macchina virtuale di destinazione:
   1. Nella macchina virtuale per abilitare le connessioni in ingresso alla porta 5985 sul sistema operativo guest destinazione, eseguire lo script di PowerShell seguente:   
`Set-NetFirewallRule -Name WINRM-HTTP-In-TCP-PUBLIC -RemoteAddress Any`

   2. È anche necessario aprire la porta nella rete di Azure:

    - Seleziona la macchina virtuale di Azure, seleziona **di rete**, quindi **Aggiungi regola di porta in ingresso**. 
    - Assicurati di **che base** è selezionata nella parte superiore del riquadro **Aggiungi regola di sicurezza in entrata** .
    - Nel campo di **intervalli di porte** , immettere **5985**.
    
    Se il gateway Windows Admin Center ha un indirizzo IP statico, è possibile selezionare per consentire solo WinRM l'accesso in ingresso dal gateway Windows Admin Center per una maggiore sicurezza.
    A tale scopo, seleziona **Avanzate** nella parte superiore del riquadro **Aggiungi regola di sicurezza in entrata** .

    Per l' **origine**, seleziona **Gli indirizzi IP**, quindi immetti l'indirizzo IP di origine corrispondente alla tua gateway Windows Admin Center.

    - Per il **protocollo** selezionare **TCP**.
    - Il resto possa essere lasciato come predefinito.

> [!NOTE]
> Devi creare una regola di porta personalizzata. La regola di porta WinRM fornita da reti Azure utilizza la porta 5986 (tramite HTTPS) invece di 5985 (tramite HTTP). 

### La connessione alle macchine virtuali senza un indirizzo IP pubblico

Se la destinazione di macchine virtuali di Azure non hanno gli indirizzi IP pubblici e si desidera gestire queste macchine virtuali da un gateway Windows Admin Center distribuito nella rete locale, è necessario configurare la rete locale per disporre della connettività al con in cui sono la destinazione di macchine virtuali connesso. Esistono 3 modi eseguire questa operazione: ExpressRoute, VPN da sito a sito o VPN da punto a sito. [Scopri quale opzione connettività abbia senso nel tuo ambiente.](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design) 

>[!TIP]
>Se lo desideri usare una connessione VPN da punto a sito per connettere il gateway Windows Admin Center di un con Azure per gestire macchine virtuali di Azure in tale con, è possibile utilizzare la funzionalità di [Scheda di rete di Azure](https://aka.ms/WACNetworkAdapter) in Windows Admin Center. A tale scopo, connettersi al server in cui è installato Windows Admin Center, passa allo strumento di rete e selezionare "Aggiungi di scheda di rete di Azure". Quando Fornisci le informazioni necessarie e fai clic su "configurare", Windows Admin Center consente di configurare una rete VPN da punto a sito per il con Azure specificare, dopo i quali è possibile connettersi e gestire macchine virtuali di Azure dal tuo gateway Windows Admin Center in locale.

Verificare che WinRM è in esecuzione in macchine virtuali di destinazione eseguendo il comando seguente in PowerShell o il prompt dei comandi nella destinazione della macchina virtuale: `winrm quickconfig`

Se si non ancora aggiunti al dominio macchina virtuale di Azure, la macchina virtuale si comporta come un server nel gruppo di lavoro, pertanto è necessario prendere in che considerazione per [l'uso di Windows Admin Center in un gruppo di lavoro](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup).

Se si verificano problemi, consultare la [Risoluzione dei problemi Windows Admin Center](../support/troubleshooting.md) per vedere se sono necessari per la configurazione passaggi aggiuntivi (ad esempio, se ci si connette con un account amministratore locale o non sono aggiunti al dominio).

## Utilizzare un gateway Windows Admin Center distribuito in Azure

È possibile gestire macchine virtuali di Azure senza qualsiasi dipendenza locale tramite la distribuzione di Windows Admin Center nel con cui sono connesso le macchine virtuali di destinazione. 

Per gestire le macchine virtuali di fuori di con in cui viene distribuito il gateway Windows Admin Center, devi stabilire con a con connettività tra con del gateway Windows Admin Center e con il server di destinazione. È possibile stabilire la connettività con il Peering con, con a con connessione o una connessione da sito a sito. [Scopri di più su quali la connettività con a con opzione abbia senso nel tuo ambiente.](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal)

Windows Admin Center può essere installato su una macchina virtuale esistente o appena distribuita nell'ambiente in uso. La macchina virtuale scelto per l'installazione di Windows Admin Center deve avere un nome DNS e IP pubblico.

[Altre informazioni sulla distribuzione di un gateway Windows Admin Center in Azure](deploy-wac-in-azure.md)