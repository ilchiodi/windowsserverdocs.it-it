---
title: Desktop remoto - Consentire l'accesso al PC dall'esterno della rete
description: Informazioni sulle opzioni per accedere in remoto al PC dall'esterno della relativa rete
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.topic: article
author: haley-rowland
manager: dongill
ms.author: elizapo
ms.date: 04/04/2018
ms.localizationpriority: medium
ms.openlocfilehash: 9cc1b7568006ef9e32132d772702212c5fd78ec4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857424"
---
# <a name="remote-desktop---allow-access-to-your-pc-from-outside-your-pcs-network"></a>Desktop remoto - Consentire l'accesso al PC dall'esterno della relativa rete

>Si applica a: Windows 10, Windows Server 2016

Quando ti connetti al tuo PC usando un client Desktop remoto, crei una connessione peer-to-peer. Ciò significa che hai necessità dell'accesso diretto al PC, a volte definito "host". Se devi connetterti al tuo PC dall'esterno della rete su cui il computer è in funzione, devi abilitare tale accesso. Le opzioni disponibili sono due: usare il port forwarding o impostare una rete privata virtuale (VPN).

## <a name="enable-port-forwarding-on-your-router"></a>Abilitare il port forwarding sul router

Il port forwarding esegue semplicemente il mapping tra la porta che si trova all'indirizzo IP del router (il tuo IP pubblico) e la porta e l'indirizzo IP del PC a cui vuoi accedere. 

I passaggi specifici da eseguire per abilitare il port forwarding variano in base al router in uso, pertanto dovrai cercare online le istruzioni relative a quest'ultimo. Per una descrizione generale dei passaggi, vedi [wikiHow - How to Set Up Port Forwarding on a Router](https://www.wikihow.com/Set-Up-Port-Forwarding-on-a-Router) (Come impostare il port forwarding su un router).

Prima di eseguire il mapping della porta, sarà necessario disporre di quanto segue:

- L'indirizzo IP interno del PC: cerca in **Impostazioni > Rete e Internet > Stato > Visualizza le proprietà della rete**. Trova la configurazione di rete con stato "Operativo" e quindi ottieni l'**indirizzo IPv4**.

   ![Configurazione di rete operativa](../media/rdclient-operational-network.png)

- Il tuo indirizzo IP pubblico, ovvero l'IP del router. Per trovarlo puoi procedere in diversi modi: cercare "mio IP" (in Bing o Google) oppure visualizzare le [proprietà della rete Wi-Fi](https://binged.it/2Gwob34) (per Windows 10).
- Il numero di porta per cui viene eseguito il mapping. Nella maggior parte dei casi, è 3389, ovvero la porta predefinita usata dalle connessioni Desktop remoto.
- L'accesso come amministratore al router.  

   >[!WARNING]
   > Stai aprendo il tuo PC a Internet, pertanto assicurati che per tale computer sia impostata una password complessa.

Dopo aver eseguito il mapping della porta, potrai connetterti al tuo PC host dall'esterno della rete locale effettuando la connessione all'indirizzo IP pubblico del router (secondo punto dell'elenco precedente).

L'indirizzo IP del router può cambiare, ad esempio il provider di servizi Internet (ISP) può assegnarti ogni volta un nuovo IP. Per evitare di incorrere in questo problema, considera la possibilità di usare un DNS dinamico, che consente di connetterti al PC usando un nome di dominio facile da ricordare anziché l'indirizzo IP. Il router aggiornerà automaticamente il servizio DDNS con il nuovo indirizzo IP, qualora dovesse cambiare.

Con la maggior parte dei router hai la possibilità di definire l'IP o la rete di origine che può usare il mapping della porta. Pertanto, se sai già di doverti connettere solo dal lavoro, puoi aggiungere l'indirizzo IP della rete aziendale. In questo modo eviterai di aprire la porta all'intera Internet pubblica. Se l'host di cui ti servi per la connessione usa un indirizzo IP dinamico, imposta la restrizione relativa all'origine per consentire l'accesso dall'intero intervallo dell'ISP in questione.

Puoi anche considerare l'eventualità di impostare un [indirizzo IP statico](/windows-hardware/customize/mobile/mcsf/enable-static-ip) sul PC in modo che l'indirizzo IP interno resti invariato. In tal caso, il port forwarding del router punterà sempre all'indirizzo IP corretto.


## <a name="use-a-vpn"></a>Usare una rete privata virtuale

Se ti connetti alla rete LAN tramite una rete privata virtuale (VPN), non devi aprire il PC alla rete Internet pubblica. Quando ti connetti alla VPN, il client Desktop remoto opera invece come se facesse parte della stessa rete e potrà accedere al tuo PC. Sono disponibili diversi servizi VPN. Puoi trovare e usare quello più adatto alle tue esigenze.