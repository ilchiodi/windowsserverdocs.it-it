---
title: Integrazione della rete virtuale Azure
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
ms.openlocfilehash: 673cb5a2292bab113aefb1de37f80bf4d880b467
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66433899"
---
# <a name="azure-virtual-network-integration"></a>Integrazione della rete virtuale Azure

>Si applica a: Windows Server 2016 Essentials

Come le organizzazioni arrivino il cloud computing, raramente sarà questi spostare tutte le relative risorse 100% in una sola volta, ma piuttosto adottare un approccio in cui alcune risorse nel cloud e altri ancora in locale. Questo approccio ibrido rende più semplice per le organizzazioni non solo spostano alcune risorse di calcolo nel cloud, ma anche consente loro di crescere la propria infrastruttura IT senza dover acquistare nuovo hardware.

Quando si implementa questo approccio ibrido alla computing, è necessario un modo semplice per le risorse in entrambe le posizioni per comunicare tra loro. La rete virtuale di Azure è un servizio di Azure che consente alle organizzazioni di creare un punto a punto (P2P) o site-to-site (S2S) rete privata virtuale che rende le risorse che sono in esecuzione in ricerca di Azure (ad esempio le macchine virtuali e archiviazione), come se fossero nella rete locale per l'accesso trasparente applicazioni e risorse.

Configurazione di una rete virtuale di Azure può essere complessi. Con Windows Server Essentials 2016 è possibile configurare facilmente la rete virtuale di Azure tramite una semplice procedura guidata che consente di scegliere le impostazioni predefinite più appropriate per l'ambiente di rete. Come illustrato nello screenshot seguente, una nuova attività di integrazione rete virtuale di Azure è stato aggiunto alla sezione del dashboard Windows Essentials per introdurre la rete virtuale di Azure, nonché di fornire un collegamento rapido per avviare l'integrazione di servizi Cloud Microsoft .

![Screenshot che mostra la scheda iniziare nella Home page del dashboard di Windows Server Essentials. Nella scheda, iniziare a usare la sezione servizi è stata selezionata e il dashboard indica in integrazione con servizi Cloud Microsoft che la rete virtuale di Azure è attualmente disabilitata.](media/azure-virtual-network-1.PNG)

Quando si fa clic il **integrare ora** collegamento per la rete virtuale di Azure nella schermata precedente, verrà visualizzata una finestra di dialogo che chiede di accedere al tuo account Microsoft Azure. Se non hai un account Microsoft Azure, si avranno la possibilità di iscrizione per una in questa schermata, che ti reindirizzerà l'utente al portale di iscrizione di account di Azure:

![Screenshot che mostra la pagina di accesso a Microsoft Azure della procedura guidata integrazione con Azure Virtual network.](media/azure-virtual-network-2.PNG)

Una volta la firma di partecipare a Azure, verrà visualizzato con la possibilità di scegliere quale sottoscrizione si vuole associare virtuale di Azure del servizio di rete:

![Screenshot che mostra la pagina sottoscrizione personale di Microsoft Azure dell'integrazione con Creazione guidata rete virtuale di Azure.](media/azure-virtual-network-3.PNG)

Dopo aver selezionato la sottoscrizione di Azure da usare per la rete virtuale di Azure, verrà visualizzata con l'opzione per creare una nuova rete virtuale di Azure o se uno è già stata impostata in questa sottoscrizione, nella casella di riepilogo a discesa verrà visualizzato che è disponibile. Inoltre si sceglierà un nome per la rete locale che userà la rete virtuale di Azure per identificare le risorse nella rete locale. Infine, si sceglierà l'area di Azure in cui si desidera la rete virtuale di Azure deve essere ospitato. Scelta di un percorso che si trova fisicamente più vicino alla rete locale è in genere migliore per ottimizzare la velocità della larghezza di banda per la comunicazione con le risorse che è possibile ospitare i servizi di Azure:

![Screenshot che mostra la pagina di rete impostare backup virtuale di Azure della procedura guidata integrazione con Azure Virtual network.](media/azure-virtual-network-4.PNG)

L'ultimo passaggio del processo di integrazione è configurare il dispositivo VPN che verrà usato per la connessione VPN S2S. Poiché le aziende più piccole sono solo alcuni server nel proprio ambiente e non dispongono di personale IT per configurare correttamente un Router VPN per connettersi a Microsoft Azure, la selezione predefinita sarà per configurare il server di Windows Server Essentials come server VPN che risorse nella rete locale si connetterà a poter accedere alle risorse nella rete virtuale di Azure. Tuttavia, se si preferisce usare un altro server nell'ambiente in uso come Server VPN o si preferisce usare un Router VPN, è possibile selezionare queste opzioni.

A causa la variazione nei tipi di router e i modelli, Windows Server Essentials non tenta di configurare automaticamente il Router VPN. Selezionando il Router VPN in questa procedura guidata integrazione notifica solo la rete virtuale di Azure di tipo di dispositivo per configurazioni di routine appropriate necessarie in Azure per la connettività.

Dopo aver completato la procedura guidata di integrazione, una nuova scheda sarà visibile nel dashboard di Windows Server Essentials per la rete virtuale di Azure:

![Screenshot che mostra la pagina rete virtuale di Azure del dashboard di Windows Server Essentials. La scheda di rete virtuale di Azure è selezionata e Mostra lo stato di configurazione.](media/azure-virtual-network-5.PNG)

>! Nota di completare la configurazione di una rete virtuale di Azure nel cloud può richiedere molto tempo, verso l'alto per 30 minuti. Durante questo periodo, lo stato di configurazione sarà presente nella pagina Stato rete virtuale di Azure del dashboard.

Una volta completata la configurazione della rete virtuale di Azure, lo stato cambia il a connesso e visualizzare i dettagli della rete virtuale di Azure, ad esempio i dati in ingresso/uscita, indirizzo IP del gateway, dettagli di account e indirizzo IP locali:

![Screenshot che mostra la pagina rete virtuale di Azure del dashboard di Windows Server Essentials. La scheda di rete virtuale di Azure sia selezionata e viene visualizzato lo stato connesso e in queste informazioni sullo stato vengono visualizzati i dettagli della rete virtuale.](media/azure-virtual-network-6.PNG)

Nel riquadro attività a destra del dashboard sono riportate le varie attività che di è possibile eseguire con la rete virtuale di Azure.

-   **Disconnessione dalla rete virtuale di Azure** la configurazione di una rete virtuale di Azure è gratuita, ma è previsto un addebito per il gateway VPN che si connette a un'istanza locale e altre reti virtuali in Azure. Disconnessione dalla rete virtuale di Azure viene arrestata la fatturazione tutti.

-   **Passare nel dispositivo VPN** nel caso in cui si desidera modificare da un Server VPN a un Router VPN, questa attività verrà consentono il passaggio e inviare una notifica di rete virtuale di Azure.

-   **Configurare una rete virtuale di Azure** questa attività consente di modificare le opzioni di configurazione avanzate della rete virtuale di Azure mediante il reindirizzamento alla pagina di configurazione del portale di Azure per la rete virtuale di Azure.

-   **Aggiorna stato** Aggiorna la pagina di stato, l'aggiornamento dello stato di connessione di rete virtuale di Azure tra cui i dati in/out.

-   **Disabilitare l'integrazione rete virtuale di Azure** disconnessioni di rete virtuale di Azure e rimuove l'integrazione dal dashboard di Windows Server Essentials. Tenere presente questa operazione non elimina la rete virtuale di Azure, le impostazioni vengono mantenute ancora in Azure se si vuole reintegrare in un secondo momento rete virtuale di Azure con il dashboard.

-   **Altre informazioni su rete virtuale di Azure** [ https://azure.microsoft.com/services/virtual-network/ ](https://azure.microsoft.com/services/virtual-network/).

<a name="see-also"></a>Vedere anche
--------
[Introduzione a Windows Server Essentials](get-started.md)
