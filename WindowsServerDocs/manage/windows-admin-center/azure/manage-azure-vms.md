---
title: Gestire macchine virtuali IaaS di Azure
description: Gestione delle macchine virtuali IaaS di Azure con l'interfaccia di amministrazione di Windows (Project Honolulu)
ms.technology: manage
ms.topic: article
author: haley-rowland
ms.author: harowl
ms.date: 09/07/2018
ms.localizationpriority: medium
ms.prod: windows-server
ms.openlocfilehash: 7b85f64d108283d4865b718b565ad3ba40f14f02
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71357345"
---
# <a name="manage-azure-iaas-virtual-machines-with-windows-admin-center"></a>Gestire le macchine virtuali IaaS di Azure con l'interfaccia di amministrazione di Windows

Puoi usare Windows Admin Center per gestire sia macchine virtuali di Azure che computer locali. Esistono diverse configurazioni possibili. scegliere la configurazione che ha senso per l'ambiente:
- [Gestire macchine virtuali di Azure da un gateway dell'interfaccia di amministrazione di Windows locale](#manage-with-an-on-premises-windows-admin-center-gateway)
- [Gestire macchine virtuali di Azure da un gateway dell'interfaccia di amministrazione di Windows installato in una macchina virtuale di Azure](#use-a-windows-admin-center-gateway-deployed-in-azure)

## <a name="manage-with-an-on-premises-windows-admin-center-gateway"></a>Gestire con un gateway dell'interfaccia di amministrazione di Windows locale

Se l'interfaccia di amministrazione di Windows è già stata installata in un gateway locale (in Windows 10 o Windows Server 2016), è possibile usare lo stesso gateway per gestire Windows 10 o Windows Server 2016, Windows Server 2012 R2, Windows Server 2012 o Windows Server 2008 R2 Macchine virtuali in Azure. 

### <a name="connecting-to-vms-with-a-public-ip"></a>Connessione alle VM con un indirizzo IP pubblico

Se le macchine virtuali di destinazione (le VM che si vuole gestire con l'interfaccia di amministrazione di Windows) hanno indirizzi IP pubblici, aggiungerli al gateway dell'interfaccia di amministrazione di Windows in base all'indirizzo IP o al nome di dominio completo. Ci sono alcune considerazioni da tenere in considerazione:

- È necessario abilitare l'accesso WinRM alla macchina virtuale di destinazione eseguendo il comando seguente in PowerShell o il prompt dei comandi nella macchina virtuale di destinazione: `winrm quickconfig`
- Se la macchina virtuale di Azure non è stata aggiunta a un dominio, la macchina virtuale si comporta come un server in un gruppo di lavoro, quindi è necessario assicurarsi di aver preso in considerazione l'uso dell'interfaccia di [amministrazione di Windows in un gruppo di](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup)lavoro.
- È anche necessario abilitare le connessioni in ingresso alla porta 5985 per WinRM su HTTP per fare in modo che l'interfaccia di amministrazione di Windows gestisca la VM di destinazione:
  1. Eseguire lo script di PowerShell seguente nella macchina virtuale di destinazione per consentire le connessioni in ingresso alla porta 5985 nel sistema operativo guest:   
     `Set-NetFirewallRule -Name WINRM-HTTP-In-TCP-PUBLIC -RemoteAddress Any`

  2. È anche necessario aprire la porta in rete di Azure:

     - Selezionare la macchina virtuale di Azure, selezionare **rete**e quindi **Aggiungi regola porta in ingresso**. 
     - Assicurarsi che sia selezionata l'opzione di **base** nella parte superiore del riquadro **Aggiungi regola di sicurezza in ingresso** .
     - Nel campo **intervalli di porte** immettere **5985**.
    
     Se il gateway dell'interfaccia di amministrazione di Windows ha un indirizzo IP statico, è possibile scegliere di consentire solo l'accesso in ingresso WinRM dal gateway dell'interfaccia di amministrazione di Windows per una maggiore sicurezza.
     A tale scopo, selezionare **Avanzate** nella parte superiore del riquadro **Aggiungi regola di sicurezza in ingresso** .

     In **origine**selezionare **indirizzi IP**, quindi immettere l'indirizzo IP di origine corrispondente al gateway dell'interfaccia di amministrazione di Windows.

     - Per **protocollo** selezionare **TCP**.
     - Il resto può essere lasciato come predefinito.

> [!NOTE]
> È necessario creare una regola di porta personalizzata. La regola di porta WinRM fornita dalla rete di Azure usa la porta 5986 (su HTTPS) invece di 5985 (su HTTP). 

### <a name="connecting-to-vms-without-a-public-ip"></a>Connessione alle macchine virtuali senza un indirizzo IP pubblico

Se le macchine virtuali di Azure di destinazione non hanno indirizzi IP pubblici e si vuole gestire queste macchine virtuali da un gateway dell'interfaccia di amministrazione di Windows distribuito nella rete locale, è necessario configurare la rete locale per avere la connettività a VNet in cui si trovano le macchine virtuali di destinazione connesso. È possibile eseguire questa operazione in tre modi: ExpressRoute, una VPN da sito a sito o una VPN da punto a sito. [Informazioni sull'opzione di connettività più sensata nell'ambiente in uso.](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-plan-design) 

>[!TIP]
>Se si vuole usare una VPN da punto a sito per connettere il gateway dell'interfaccia di amministrazione di Windows a una VNet di Azure per gestire le macchine virtuali di Azure in tale VNet, è possibile usare la funzionalità [scheda di rete di Azure](https://aka.ms/WACNetworkAdapter) nell'interfaccia di amministrazione di Windows. A tale scopo, connettersi al server in cui è installato Windows Admin Center, passare allo strumento di rete e selezionare "Aggiungi scheda di rete di Azure". Quando si forniscono i dettagli necessari e si fa clic su "Configura", l'interfaccia di amministrazione di Windows configura una VPN da punto a sito per la VNet di Azure specificata. successivamente, è possibile connettersi alle macchine virtuali di Azure e gestirle dal gateway dell'interfaccia di amministrazione di Windows locale.

Assicurarsi che WinRM sia in esecuzione nelle VM di destinazione eseguendo il comando seguente in PowerShell o il prompt dei comandi nella macchina virtuale di destinazione: `winrm quickconfig`

Se la macchina virtuale di Azure non è stata aggiunta a un dominio, la macchina virtuale si comporta come un server in un gruppo di lavoro, quindi è necessario assicurarsi di aver preso in considerazione l'uso dell'interfaccia di [amministrazione di Windows in un gruppo di](../support/troubleshooting.md#using-windows-admin-center-in-a-workgroup)lavoro.

Se si verificano problemi, consultare la [sezione risolvere](../support/troubleshooting.md) i problemi dell'interfaccia di amministrazione di Windows per verificare se sono necessari passaggi aggiuntivi per la configurazione, ad esempio se ci si connette con un account amministratore locale o non si è aggiunti a un dominio.

## <a name="use-a-windows-admin-center-gateway-deployed-in-azure"></a>Usare un gateway dell'interfaccia di amministrazione di Windows distribuito in Azure

È possibile gestire le macchine virtuali di Azure senza alcuna dipendenza locale distribuendo l'interfaccia di amministrazione di Windows nella VNet in cui sono connesse le VM di destinazione. 

Per gestire le macchine virtuali al di fuori della VNet in cui viene distribuito il gateway dell'interfaccia di amministrazione di Windows, è necessario stabilire la connettività da VNet a VNet tra il VNet del gateway dell'interfaccia di amministrazione di Windows e il VNet dei server di destinazione. È possibile stabilire questa connettività con il peering VNet, la connessione da VNet a VNet o una connessione da sito a sito. [Altre informazioni sull'opzione di connettività da VNet a VNet sono sensate nell'ambiente in uso.](https://docs.microsoft.com/azure/vpn-gateway/vpn-gateway-howto-vnet-vnet-resource-manager-portal)

L'interfaccia di amministrazione di Windows può essere installata in una macchina virtuale esistente o appena distribuita nell'ambiente in uso. La VM scelta per l'installazione dell'interfaccia di amministrazione di Windows deve avere un indirizzo IP pubblico e un nome DNS.

[Scopri di più sulla distribuzione di un gateway di Windows Admin Center in Azure](deploy-wac-in-azure.md)