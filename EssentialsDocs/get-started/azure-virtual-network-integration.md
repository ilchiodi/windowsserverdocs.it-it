---
title: Integrazione di rete virtuale Azure
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d7d38505-cff5-4f15-9fd5-ae6dba15ce88
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 21057359967c73d8d9fc434694b8f203ad5c1f6a
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
#<a name="azure-virtual-network-integration"></a>Integrazione di rete virtuale Azure

>Si applica a: Windows Server 2016 Essentials

Quando le organizzazioni verso il cloud computing, raramente verrà tali spostare tutte le proprie risorse 100% in una sola volta, ma piuttosto adottare un approccio in cui alcune risorse sono nel cloud e altri ancora in locale. Questo approccio ibrido rende più semplice per le organizzazioni non solo spostare alcune risorse di elaborazione nel cloud, ma consente inoltre di aumentare l'infrastruttura IT senza dover acquistare nuovo hardware.

Quando si implementa questo approccio ibrida di elaborazione, è necessario un modo semplice per le risorse in entrambe le posizioni per comunicare tra loro. Rete virtuale Azure è un servizio di Azure che consente alle organizzazioni di creare un punto (P2P) o un sito a sito (S2S) rete privata virtuale che rende le risorse di cui sono in esecuzione in cerca di Azure (ad esempio, le macchine virtuali e archiviazione), come se fossero nella rete locale per l'accesso trasparente applicazioni e risorse.

Configurazione di una rete virtuale di Azure può essere complesso. Con Windows Server 2016 Essentials è possibile configurare facilmente la rete virtuale di Azure tramite una semplice procedura guidata che consente di scegliere le impostazioni predefinite più appropriate per l'ambiente di rete. Come illustrato nella schermata seguente, una nuova attività di integrazione rete virtuale di Azure è stato aggiunto alla sezione del dashboard di Windows Essentials per introdurre una rete virtuale di Azure, oltre a fornire un collegamento per avviare l'integrazione di servizi Cloud Microsoft.

![Screenshot che mostra la scheda iniziare nella Home page del dashboard di Windows Server Essentials. Nella scheda per iniziare, è stata selezionata la sezione servizi e in servizi di integrazione con Microsoft Cloud dashboard indica che la rete virtuale di Azure è attualmente disabilitata.](media/azure-virtual-network-1.PNG)

Quando si sceglie di **integrare ora** collegare per le reti virtuali di Azure nella schermata precedente, verrà visualizzata una finestra di dialogo in cui viene richiesto di accedere al tuo account Microsoft Azure. Se non hai un account di Microsoft Azure, hai la possibilità di iscrizione per uno in questa schermata, che eseguirà il reindirizzamento è il portale per gli account Azure iscrizione:

![Screenshot che mostra la pagina della procedura guidata integrazione con Azure virtuale rete Sign In per Microsoft Azure.](media/azure-virtual-network-2.PNG)

Dopo la firma-in Azure, verrà visualizzata con la possibilità di scegliere quali sottoscrizione che vuoi associare la macchina virtuale di Azure del servizio di rete:

![Screenshot che mostra la pagina di iscrizione Microsoft Azure dell'integrazione con configurazione guidata rete virtuale di Azure.](media/azure-virtual-network-3.PNG)

Dopo aver selezionato le sottoscrizione Azure da utilizzare per le reti virtuali di Azure, verrà visualizzata con l'opzione per creare una nuova rete virtuale di Azure o se uno è già stata impostata in questa sottoscrizione, la casella di riepilogo a discesa mostrerà che è disponibile. Inoltre, si sceglierà un nome per la rete locale che verrà utilizzata la rete virtuale di Azure per identificare le risorse nella rete locale. Infine, si sceglierà quale area Azure in cui si desidera la rete virtuale di Azure per essere ospitato. Scegliere un percorso che si trova fisicamente più vicino nella rete locale è in genere migliore per l'ottimizzazione della velocità della larghezza di banda per la comunicazione con le risorse che può ospitare i servizi di Azure:

![Screenshot che mostra la pagina rete impostare backup di Azure virtuale della procedura guidata integrazione con Azure virtuale rete.](media/azure-virtual-network-4.PNG)

L'ultimo passaggio del processo di integrazione è per configurare il dispositivo VPN che verrà utilizzato per la connessione VPN S2S. Poiché più piccole e medie imprese con solo pochi server nel proprio ambiente e non dispongono di personale IT per configurare correttamente un Router VPN per connettersi a Microsoft Azure, la selezione predefinita è necessario impostare il server di Windows Server Essentials come server VPN che si connetteranno risorse nella rete locale per accedere alle risorse nella rete virtuale di Azure. Tuttavia, se si preferisce utilizzare un altro server nell'ambiente in uso come Server VPN o si preferisce utilizzare un Router VPN, è possibile selezionare queste opzioni.

A causa la variazione di modelli e tipi di router, non tenta di configurare automaticamente il Router VPN di Windows Server Essentials. Selezionando il Router di VPN in questa procedura guidata integrazione notifica solo la rete virtuale di Azure del tipo di dispositivo per configurazioni di routing appropriati necessari in Azure per la connettività.

Al termine della procedura guidata di integrazione, una nuova scheda saranno visibile nel dashboard di Windows Server Essentials per le reti virtuali di Azure:

![Screenshot che mostra la pagina di rete virtuale di Azure del dashboard di Windows Server Essentials. La scheda di rete virtuali di Azure è selezionata e viene visualizzato lo stato di configurazione.](media/azure-virtual-network-5.PNG)

>! Nota completamento della configurazione di una rete virtuale di Azure nel cloud può richiedere molto tempo, verso l'alto per 30 minuti. Durante questo periodo, lo stato di configurazione sarà presente nella pagina Stato rete virtuale di Azure del dashboard.

Una volta completata la configurazione della rete virtuale di Azure, lo stato verrà modificare in connesso e visualizzare i dettagli della rete virtuale di Azure ad esempio dati o la disconnessione, indirizzo IP del gateway, dettagli di account e l'indirizzo IP locali:

![Screenshot che mostra la pagina di rete virtuale di Azure del dashboard di Windows Server Essentials. La scheda di rete virtuali di Azure è selezionata e viene visualizzato lo stato come connesso e in queste informazioni sullo stato vengono visualizzati i dettagli della rete virtuale.](media/azure-virtual-network-6.PNG)

Nel riquadro attività sul lato destro del dashboard sono le diverse attività che è possibile eseguire con la rete virtuale di Azure.

-   **Disconnessione dalla rete virtuale di Azure** configurare una rete virtuale di Azure è disponibile, ma esiste un addebito per il gateway VPN che si connette a in locale e altri VNETs in Azure. Disconnessione dalla rete virtuale di Azure Arresta tutti fatturazione.

-   **Passare sul dispositivo VPN** nel caso in cui si desidera modificare da un Server VPN a un Router VPN, questa operazione consentirà di rendere il commutatore e inviare una notifica di rete virtuale di Azure.

-   **Configurare una rete virtuale di Azure** questa attività consente di modificare le opzioni di configurazione avanzata di rete virtuale di Azure mediante il reindirizzamento alla pagina di configurazione del portale Azure per la rete virtuale di Azure.

-   **Aggiornare lo stato di** Aggiorna la pagina di stato, aggiornando lo stato di connessione di rete virtuale di Azure compresi i dati in entrata.

-   **Disabilitare l'integrazione di Azure VNET** disconnessioni di rete virtuale di Azure e rimuove integrazione dal dashboard di Windows Server Essentials. Tieni presente ciò non elimina la rete virtuale di Azure, le impostazioni vengono conservate ancora in Azure, se si desidera in un secondo momento reintegrare rete virtuale di Azure con il dashboard.

-   **Scopri di più sulla rete virtuale di Azure**[https://azure.microsoft.com/en-us/services/virtual-network/](https://azure.microsoft.com/en-us/services/virtual-network/).

<a name="see-also"></a>Vedere anche
--------
[Introduzione a Windows Server Essentials](get-started.md)
