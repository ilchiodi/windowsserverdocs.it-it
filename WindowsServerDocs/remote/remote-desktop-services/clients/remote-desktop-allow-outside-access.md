---
title: Desktop remoto - consentire l'accesso al PC dall'esterno della rete
description: Scopri le opzioni per accedere in remoto il PC all'esterno di rete del computer
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.tgt_pltfrm: na
ms.topic: article
author: haley-rowland
manager: dongill
ms.author: elizapo
ms.date: 04/04/2018
ms.localizationpriority: medium
ms.openlocfilehash: d77c362d9d06b70ad0747002ed8853a39e05b7ff
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59888152"
---
# <a name="remote-desktop---allow-access-to-your-pc-from-outside-your-pcs-network"></a>Desktop remoto - consentire l'accesso al PC all'esterno di rete del PC

>Si applica a: Windows 10,  Windows Server 2016

Quando ci si connette al computer tramite un client Desktop remoto, si crea una connessione peer-to-peer. Ciò significa che è necessario l'accesso diretto al PC (talvolta denominate "host"). Se è necessario connettersi al PC dall'esterno della rete in che al computer è in esecuzione, è necessario abilitare l'accesso. Sono disponibili due opzioni: usare l'inoltro alla porta o configurare una VPN.

## <a name="enable-port-forwarding-on-your-router"></a>Abilitare il port forwarding sul router

L'inoltro alla porta esegue il mapping semplicemente la porta sull'indirizzo IP del router (l'indirizzo IP pubblico) alla porta e indirizzo IP del computer che si desidera accedere. 

I passaggi specifici per abilitare l'inoltro alla porta dipendono il router in uso, pertanto sarà necessario eseguire la ricerca online per le istruzioni del router. Per una discussione generale dei passaggi, consultare [wikiHow per impostare backup di Port Forwarding sul Router](https://www.wikihow.com/Set-Up-Port-Forwarding-on-a-Router).

Prima eseguire il mapping della porta è necessario quanto segue:

- Indirizzo IP interno del PC: Cerca in **Impostazioni > rete e Internet > stato > consente di visualizzare le proprietà della rete**. Trovare la configurazione di rete con stato "Operational" e quindi ottenere il **indirizzo IPv4**.

   ![Configurazione di rete operativa](../media/rdclient-operational-network.png)

- L'indirizzo IP pubblico (indirizzo IP del router). Esistono diversi modi per trovarlo, è possibile cercare, in Bing o Google, "my IP" o visualizzare il [proprietà della rete Wi-Fi](https://binged.it/2Gwob34) (per Windows 10).
- Numero di porta da mappare. Nella maggior parte dei casi si tratta 3389, ovvero la porta predefinita usata per le connessioni Desktop remoto.
- Accesso amministrativo al router.  

   >[!WARNING]
   > Si sta aprendo il PC fino a internet, assicurarsi di avere una password complessa impostato per il PC.

Dopo il mapping della porta, sarà in grado di connettersi al PC host dall'esterno della rete locale tramite la connessione all'indirizzo IP pubblico del router (il secondo punto elenco precedente).

Indirizzo IP del router può essere modificato: provider di servizi internet (ISP) l'assegnazione di un nuovo indirizzo IP in qualsiasi momento. Per evitare di riscontrare questo problema, provare a usare DNS dinamico: ciò consente di connettersi al computer utilizzando un facile da ricordare il nome di dominio, anziché l'indirizzo IP. Il router Aggiorna automaticamente il servizio DNS dinamico con il nuovo indirizzo IP, deve modificare.

Con la maggior parte dei router è possibile definire quale indirizzo IP di origine o di una rete di origine può utilizzare il mapping della porta. Pertanto, se si sa che verrà utilizzato solo per la connessione dal lavoro, è possibile aggiungere l'indirizzo IP per la rete di lavoro - che consente di evitare di aprire la porta per l'intera rete internet pubblica. Se l'host che si usa per la connessione Usa indirizzo IP dinamico, impostare la limitazione di origine per consentire l'accesso dall'intera gamma di tale particolare ISP.

È inoltre possibile configurare un [indirizzo IP statico](/windows-hardware/customize/mobile/mcsf/enable-static-ip) nel PC in modo da non modifica l'indirizzo IP interno. Se si esegue l'operazione che, quindi il router il port forwarding farà sempre riferimento all'indirizzo IP corretto.


## <a name="use-a-vpn"></a>Usare una VPN

Se ci si connette alla rete locale tramite una rete privata virtuale (VPN), non devi aprire del PC per la rete internet pubblica. Al contrario, quando ci si connette alla rete VPN, il client desktop remoto agisce come se fa parte della stessa rete e in grado di accedere al computer. Sono disponibili numerosi servizi VPN, è possibile trovare e usare a seconda del valore adatta alle tue esigenze.