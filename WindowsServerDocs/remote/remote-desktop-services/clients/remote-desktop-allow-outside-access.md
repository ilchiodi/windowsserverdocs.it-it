---
title: Desktop remoto - consentire l'accesso al computer dall'esterno della rete
description: Informazioni sulle opzioni per l'accesso a PC da fuori rete del computer
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
ms.sourcegitcommit: 1533d994a6ddea54ac189ceb316b7d3c074307db
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/01/2018
ms.locfileid: "1708659"
---
# <a name="remote-desktop---allow-access-to-your-pc-from-outside-your-pcs-network"></a>Desktop remoto - consentire l'accesso al computer da fuori rete del computer

>Si applica a: Windows 10, Windows Server 2016

Quando si connette al computer tramite un client Desktop remoto, si sta creando una connessione peer-to-peer. Ciò significa che è necessario l'accesso diretto a PC (denominato anche "host"). Se si desidera connettersi al PC dall'esterno alla rete che è in esecuzione il PC, è necessario abilitare questo tipo di accesso. Sono disponibili due opzioni: utilizzare l'inoltro di porta o impostare una connessione VPN.

## <a name="enable-port-forwarding-on-your-router"></a>Attivare l'inoltro di porta nel router

Inoltro della porta mappa semplicemente la porta sull'indirizzo IP del router (l'IP pubblico) per la porta e l'indirizzo IP del computer che si desidera accedere. 

Passaggi specifici per abilitare l'inoltro della porta dipendono dal router che si sta utilizzando, pertanto sarà necessario eseguire la ricerca online per le istruzioni del router. Per una discussione generale delle procedure, vedere [wikiHow ai Set Up porta inoltro su un Router](https://www.wikihow.com/Set-Up-Port-Forwarding-on-a-Router).

Prima eseguire il mapping della porta è necessario quanto segue:

- Indirizzo IP interno PC: cercare nella **Impostazioni > rete e Internet > stato > consente di visualizzare le proprietà della rete**. Individuare la configurazione di rete il cui stato è "Operativo" e quindi acquisire l' **indirizzo IPv4**.

   ![Configurazione di rete funzionante](../media/rdclient-operational-network.png)

- L'indirizzo IP pubblico (del router IP). Esistono diversi modi per trovare: è possibile cercare "my IP" (nel Bing o Google) o visualizzare le [proprietà di rete Wi-Fi](https://binged.it/2Gwob34) (per Windows 10).
- Numero di porta mappata. Nella maggior parte dei casi si tratta di 3389 - è la porta predefinita utilizzata per le connessioni Desktop remoto.
- Accesso amministrativo a router.  

   >[!WARNING]
   > Si desidera aprire il PC fino a internet, assicurarsi di avere una password complessa impostato per PC.

Dopo che si esegue il mapping della porta, sarà in grado di connettersi all'host locale PC dall'esterno alla rete locale tramite la connessione all'indirizzo IP pubblico del router (il secondo punto elenco precedente).

Modifica l'indirizzo IP del router: provider di servizi internet (ISP) l'assegnazione di un nuovo PI in qualsiasi momento. Per evitare l'esecuzione in questo problema, è consigliabile utilizzare DNS dinamiche - consente di connettersi al computer con un semplice ricordare nome di dominio, anziché l'indirizzo IP. Router viene aggiornato automaticamente il servizio DNS dinamico con il nuovo indirizzo IP, devono modificare.

Con la maggior parte dei router è possibile stabilire quali indirizzo IP di origine o di una rete di origine può utilizzare il mapping di porta. Pertanto, se si conosce che sarà solo per la connessione dall'ufficio, è possibile aggiungere l'indirizzo IP per la rete aziendale - che consente di evitare di aprire la porta per l'intera internet pubblico. Se l'host che si sta utilizzando per la connessione utilizza l'indirizzo IP dinamico, impostare la limitazione di origine per consentire l'accesso dall'intera gamma di tale provider di servizi Internet specifico.

Considerare inoltre l'impostazione di un [indirizzo IP statico](/windows-hardware/customize/mobile/mcsf/enable-static-ip) sul computer in modo che non viene modificato l'indirizzo IP interno. In caso contrario, che quindi il router punti sempre inoltro della porta nell'indirizzo IP corretto.


## <a name="use-a-vpn"></a>Utilizzare una rete VPN

Se ci si connette alla rete locale tramite una rete privata virtuale (VPN), non è necessario aprire il PC per internet pubblico. In realtà, quando ci si connette alla rete VPN, il client desktop remoto possa sembrare che fa parte della stessa rete ed essere in grado di accedere al PC. Sono disponibili numerosi servizi VPN: è possibile trovare e utilizzare più adatta alle proprie è.