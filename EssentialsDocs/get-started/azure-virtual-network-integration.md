---
title: Integrazione della rete virtuale di Azure
description: Viene descritto come usare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d7d38505-cff5-4f15-9fd5-ae6dba15ce88
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 92c8241d861e72d5f9f409a334e6edbeed5eae4c
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80310570"
---
# <a name="azure-virtual-network-integration"></a>Integrazione della rete virtuale di Azure

>Si applica a: Windows Server 2016 Essentials

Poiché le organizzazioni sono in grado di cloud computing, raramente spostano tutte le risorse 100% alla volta, ma piuttosto un approccio in cui alcune risorse si trovano nel cloud e altre ancora in locale. Questo approccio ibrido consente alle organizzazioni di non solo spostare alcune risorse di elaborazione nel cloud, ma anche di ampliare la propria infrastruttura IT senza dover acquisire nuovo hardware.

Quando si implementa questo approccio ibrido all'elaborazione, è necessario un modo semplice per le risorse in entrambe le posizioni per comunicare tra loro. Rete virtuale di Azure è un servizio di Azure che consente alle organizzazioni di creare una rete privata virtuale da punto a punto (P2P) o da sito a sito (S2S) che rende le risorse in esecuzione in Azure (ad esempio macchine virtuali e archiviazione) come se fossero nella rete locale per l'accesso trasparente alle applicazioni e alle risorse.

La configurazione di una rete virtuale di Azure può essere complessa. Con Windows Server Essentials 2016 è possibile configurare facilmente la rete virtuale di Azure tramite una semplice procedura guidata che consente di scegliere le impostazioni predefinite più appropriate per l'ambiente di rete. Come illustrato nello screenshot seguente, una nuova attività di integrazione della rete virtuale di Azure è stata aggiunta alla sezione servizi Microsoft Cloud del dashboard di Windows Essentials per introdurre la rete virtuale di Azure e fornire un collegamento rapido per avviare l'integrazione. .

![Screenshot che mostra la scheda attività iniziali nella Home page del dashboard di Windows Server Essentials. Nella scheda attività iniziali, è stata selezionata la sezione servizi e il dashboard indica in integrazione Microsoft Cloud Services che la rete virtuale di Azure è attualmente disabilitata.](media/azure-virtual-network-1.PNG)

Quando si fa clic sul collegamento **integra ora** per la rete virtuale di Azure nella schermata precedente, viene visualizzata una finestra di dialogo in cui viene chiesto di accedere all'account Microsoft Azure. Se non si dispone di un account Microsoft Azure, sarà possibile effettuare l'iscrizione a uno di questi in questa schermata, in modo da reindirizzare l'utente al portale di iscrizione all'account Azure:

![Screenshot che mostra la pagina Accedi a Microsoft Azure della procedura guidata integrazione con la rete virtuale di Azure.](media/azure-virtual-network-2.PNG)

Una volta effettuato l'accesso ad Azure, verrà visualizzata l'opzione per scegliere la sottoscrizione da associare al servizio di rete virtuale di Azure:

![Screenshot che mostra la pagina sottoscrizione My Microsoft Azure della procedura guidata integrazione con rete virtuale di Azure.](media/azure-virtual-network-3.PNG)

Dopo aver scelto la sottoscrizione di Azure da usare per la rete virtuale di Azure, verrà visualizzata l'opzione per creare una nuova rete virtuale di Azure o, se ne è già stata configurata una in questa sottoscrizione, la casella a discesa indicherà che è disponibile. Sarà inoltre possibile scegliere un nome per la rete locale che la rete virtuale di Azure utilizzerà per identificare le risorse nella rete locale. Infine, si sceglierà l'area di Azure in cui si vuole che la rete virtuale di Azure venga ospitata. La scelta di un percorso fisicamente più vicino alla rete locale è in genere ideale per ottimizzare la velocità della larghezza di banda per la comunicazione con le risorse che possono essere ospitate nei servizi di Azure:

![Screenshot che mostra la pagina di configurazione della rete virtuale di Azure della procedura guidata di integrazione con la rete virtuale di Azure.](media/azure-virtual-network-4.PNG)

L'ultimo passaggio del processo di integrazione consiste nel configurare il dispositivo VPN che verrà usato per la connessione VPN S2S. Poiché la maggior parte delle piccole aziende dispone solo di pochi server nell'ambiente in uso e non dispone del personale IT per configurare correttamente un router VPN per la connessione a Microsoft Azure, la selezione predefinita consiste nel configurare il server di Windows Server Essentials come server VPN per le risorse nella rete locale si connetterà a per accedere alle risorse nella rete virtuale di Azure. Tuttavia, se si preferisce usare un altro server nell'ambiente come server VPN oppure si preferisce usare un router VPN, è possibile selezionare tali opzioni.

A causa della variazione nei modelli e tipi di router, Windows Server Essentials non tenta di configurare automaticamente il router VPN. Selezionando il router VPN in questa integrazione guidata viene notificata solo la rete virtuale di Azure del tipo di dispositivo per le configurazioni di routing appropriate necessarie in Azure per la connettività.

Al termine dell'integrazione guidata, nel dashboard di Windows Server Essentials per la rete virtuale di Azure verrà visualizzata una nuova scheda:

![Screenshot che mostra la pagina Azure VNet del dashboard di Windows Server Essentials. La scheda rete virtuale di Azure è selezionata e Mostra lo stato come configurazione.](media/azure-virtual-network-5.PNG)

>! Nota il completamento della configurazione di una rete virtuale di Azure nel cloud può richiedere molto tempo, a partire da 30 minuti. Durante questo periodo di tempo, lo stato della configurazione sarà presente nella pagina stato rete virtuale di Azure del dashboard.

Una volta completata la configurazione della rete virtuale di Azure, lo stato cambierà in connesso e visualizzerà i dettagli della rete virtuale di Azure, ad esempio i dati in entrata/in uscita, l'indirizzo IP del gateway, l'indirizzo IP locale e i dettagli dell'account:

![Screenshot che mostra la pagina Azure VNet del dashboard di Windows Server Essentials. La scheda rete virtuale di Azure è selezionata e Mostra lo stato come connesso e in queste informazioni sullo stato vengono visualizzati i dettagli della rete virtuale.](media/azure-virtual-network-6.PNG)

Nel riquadro attività sul lato destro del dashboard sono disponibili le varie attività che è possibile eseguire con la rete virtuale di Azure.

-   **Disconnetti da Azure VNET** La configurazione di una rete virtuale di Azure è gratuita, ma è previsto un addebito per il gateway VPN che si connette ad ambienti locali e ad altre reti virtuali in Azure. La disconnessione dalla VNET di Azure interrompe tutta la fatturazione.

-   **Passa al dispositivo VPN** Se si vuole passare da un server VPN a un router VPN, questa attività consente di eseguire il commutatore e inviare una notifica alla VNET di Azure.

-   **Configurare Azure VNET** Questa attività consente di modificare le opzioni di configurazione avanzate della VNET di Azure tramite il reindirizzamento alla pagina di configurazione portale di Azure per Azure VNET.

-   **Stato aggiornamento** Aggiorna la pagina stato, aggiornando lo stato di connessione della VNET di Azure, inclusi i dati in uscita/in uscita.

-   **Disabilitare integrazione rete virtuale di Azure** Disconnette la VNET di Azure e rimuove l'integrazione dal dashboard di Windows Server Essentials. Si noti che questa operazione non elimina la VNET di Azure. le impostazioni vengono comunque mantenute in Azure se si vuole riintegrare in seguito Azure VNET con il dashboard.

-   **Scopri di più su Azure VNET** [https://azure.microsoft.com/services/virtual-network/](https://azure.microsoft.com/services/virtual-network/).

<a name="see-also"></a>Vedere anche
--------
[Introduzione a Windows Server Essentials](get-started.md)
