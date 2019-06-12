---
title: Gestire le macchine virtuali IaaS di Azure
description: La gestione delle macchine virtuali IaaS di Azure con Windows Admin Center (progetto Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 09/07/2018
ms.localizationpriority: medium
ms.prod: windows-server-threshold
ms.openlocfilehash: ac98f42c4ad5606cc8d2b142f209f9bdb2b9611c
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445906"
---
# <a name="manage-azure-iaas-virtual-machines-with-windows-admin-center"></a>Gestire le macchine virtuali IaaS di Azure con Windows Admin Center

È possibile utilizzare Windows Admin Center per gestire le macchine virtuali di Azure, nonché macchine virtuali locali. Esistono molte configurazioni diverse possibili - scegliere la configurazione appropriata per l'ambiente:
- [Gestire macchine virtuali di Azure da un gateway di Windows Admin Center in locale](#manage-with-an-on-premises-windows-admin-center-gateway)
- [Gestire macchine virtuali di Azure da un gateway di Windows Admin Center installato in una VM di Azure](#use-a-windows-admin-center-gateway-deployed-in-azure)

## <a name="manage-with-an-on-premises-windows-admin-center-gateway"></a>Gestire con un gateway di Windows Admin Center in locale

Se è già stato installato Windows Admin Center in un gateway da sito locale (sia in Windows 10 o Windows Server 2016), è possibile usare questo stesso gateway per gestire Windows 10 o Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2 Macchine virtuali in Azure. 

### <a name="connecting-to-vms-with-a-public-ip"></a>Connessione alle macchine virtuali con un indirizzo IP pubblico

Se le macchine virtuali (le macchine virtuali che si desidera gestire con Windows Admin Center) di destinazione dispone di indirizzi IP pubblici, aggiungerle al gateway di Windows Admin Center, in base all'indirizzo IP o nome di dominio completo. Esistono due considerazioni da tenere conto:

- È necessario abilitare l'accesso WinRM per la macchina virtuale di destinazione eseguendo le operazioni seguenti in PowerShell o Prompt dei comandi nella macchina virtuale di destinazione: `winrm quickconfig`
- Se è stato ancora aggiunto al dominio macchina virtuale di Azure, la macchina virtuale si comporta come un server nel gruppo di lavoro, pertanto è necessario assicurarsi che non è spiegare [utilizzo di Windows Admin Center in un gruppo di lavoro](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup).
- È anche necessario abilitare le connessioni in ingresso alla porta 5985 per gestione remota Windows su HTTP affinché Windows Admin Center gestire la macchina virtuale di destinazione:
  1. Eseguire il seguente script PowerShell nella VM per abilitare le connessioni in ingresso alla porta 5985 del sistema operativo guest di destinazione:   
     `Set-NetFirewallRule -Name WINRM-HTTP-In-TCP-PUBLIC -RemoteAddress Any`

  2. È anche necessario aprire la porta nella rete di Azure:

     - Selezionare la VM di Azure, selezionare **Networking**, quindi **Aggiungi regola porta in ingresso**. 
     - Assicurarsi **base** sia selezionata nella parte superiore delle **Aggiungi regola di sicurezza in ingresso** riquadro.
     - Nel **intervalli di porta** immettere **5985**.
    
     Se il gateway di Windows Admin Center ha un indirizzo IP statico, è possibile selezionare per consentire solo accesso in ingresso WinRM dal gateway di Windows Admin Center per una maggiore sicurezza.
     A questo scopo, selezionare **avanzate** nella parte superiore delle **Aggiungi regola di sicurezza in ingresso** riquadro.

     Per la **origine**, selezionare **gli indirizzi IP**, quindi immettere l'indirizzo IP di origine corrispondente al gateway di Windows Admin Center.

     - Per la **Protocol** selezionate **TCP**.
     - Il resto può essere lasciato come impostazione predefinita.

> [!NOTE]
> È necessario creare una regola di porta personalizzato. La regola di porta WinRM fornita dalla rete di Azure Usa la porta 5986 (tramite HTTPS) anziché 5985 (tramite HTTP). 

### <a name="connecting-to-vms-without-a-public-ip"></a>Connessione alle macchine virtuali senza un indirizzo IP pubblico

Se le macchine virtuali di Azure di destinazione non dispone di indirizzi IP pubblici e si desidera gestire queste macchine virtuali da un gateway di Windows Admin Center distribuito nella rete locale, è necessario configurare la rete locale per potersi connettere alla rete virtuale in cui sono le macchine virtuali di destinazione connesso. Esistono 3 modi per farlo: ExpressRoute, VPN Site-to-Site o Point-to-Site VPN. [Informazioni su quale opzione di connettività senso nel proprio ambiente.](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design) 

>[!TIP]
>Se si vuole usare una VPN Point-to-Site per collegare il gateway di Windows Admin Center alla rete virtuale di Azure per gestire macchine virtuali di Azure, in quanto VNet, è possibile usare la [scheda di rete di Azure](https://aka.ms/WACNetworkAdapter) funzionalità in Windows Admin Center. A tale scopo, connettersi al server in cui è installato Windows Admin Center, passare allo strumento di rete e selezionare "Aggiungi scheda rete di Azure". Quando si forniscono i dettagli necessari e fare clic su "configurare", Windows Admin Center verrà configurato una VPN Point-to-Site alla rete virtuale di Azure specificate, dopo questa operazione, è possibile connettersi e gestire macchine virtuali di Azure dal gateway di Windows Admin Center in locale.

Verificare che WinRM è in esecuzione in macchine virtuali di destinazione eseguendo il comando seguente in PowerShell o Prompt dei comandi nella macchina virtuale di destinazione: `winrm quickconfig`

Se è stato ancora aggiunto al dominio macchina virtuale di Azure, la macchina virtuale si comporta come un server nel gruppo di lavoro, pertanto è necessario assicurarsi che non è spiegare [utilizzo di Windows Admin Center in un gruppo di lavoro](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup).

Se si verificano problemi, consultare [risoluzione dei problemi Windows Admin Center](../support/troubleshooting.md) per vedere se sono necessari per la configurazione eseguire passaggi aggiuntivi (ad esempio, se l'utente si connette utilizzando un account amministratore locale o non sono aggiunti a un dominio).

## <a name="use-a-windows-admin-center-gateway-deployed-in-azure"></a>Usare un gateway di Windows Admin Center distribuito in Azure

È possibile gestire macchine virtuali di Azure senza dipendenze da un'istanza locale tramite la distribuzione di Windows Admin Center nella rete virtuale in cui sono connesse le macchine virtuali di destinazione. 

Per gestire le macchine virtuali all'esterno della rete virtuale in cui viene distribuito il gateway di Windows Admin Center, è necessario stabilire la connettività VNet-to-VNet tra la rete virtuale del gateway Windows Admin Center e la rete virtuale dei server di destinazione. È possibile stabilire la connettività con il peering reti virtuali, rete virtuale a rete virtuale o una connessione Site-to-Site. [Scopri di più su cui la connettività VNet-to-VNet opzione ha senso in ambiente.](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal)

Windows Admin Center può essere installato in una VM esistente o appena distribuita nell'ambiente in uso. La macchina virtuale scelto per l'installazione di Windows Admin Center deve avere un nome DNS e IP pubblico.

[Altre informazioni sulla distribuzione di un gateway Microsoft Windows Admin Center in Azure](deploy-wac-in-azure.md)