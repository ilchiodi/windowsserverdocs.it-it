---
title: Pianificazione del sito di Servizi MultiPoint
description: Informazioni sulla pianificazione per le distribuzioni MultiPoint Services in Windows Server 2016
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 063783cd-d748-489e-b175-46eadc993f7a
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 744e49f47f7144dac82dbe68c885060b0c08490d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71389599"
---
# <a name="multipoint-services-site-planning"></a>Pianificazione del sito di Servizi MultiPoint
È necessario prendere in considerazione la posizione in cui verranno distribuiti uno o più computer che eseguono MultiPoint Services e le stazioni associate.  
  
Il computer che esegue il ruolo Servizi MultiPoint deve disporre di un comodo accesso a un alimentatore e ai dispositivi periferici connessi direttamente, ad esempio una stampante. Inoltre, il computer in cui è in esecuzione MultiPoint Services deve disporre di un comodo accesso a una connessione di rete. È necessaria una connessione di rete per l'accesso a Internet e, laddove disponibile, una LAN.  
  
Di seguito sono riportati alcuni fattori da considerare:  
  
-   Il sistema di servizi MultiPoint verrà configurato in una stanza specifica oppure verrà configurato in un carrello o in una tabella in sequenza, in modo che possa essere spostato da una posizione all'altra?  
  
    > [!NOTE]  
    > Se si prevede di usare una configurazione per dispositivi mobili, è possibile *associare* le stazioni con servizi multipoint ogni volta che vengono riconnesse per assicurarsi che ogni tastiera e mouse siano associati al monitor appropriato.  
  
-   La stazione primaria sarà posizionata accanto alle altre stazioni o sarà separata? Se, ad esempio, il sistema MultiPoint Services è configurato in una classe, la stazione primaria si troverà sulla scrivania dell'insegnante e sulle stazioni standard posizionate altrove nella stanza? Quando il computer che esegue MultiPoint Services viene riavviato, la stazione primaria avrà accesso alle schermate di avvio. Se si è interessati a questo livello di accesso in una classe, è preferibile inserire la stazione primaria presso la scrivania del docente.  
  
-   Quante stazioni rientreranno nello spazio?  
  
-   È necessaria una rete? Una soluzione a server singolo che usa Direct video Connected o USB zero client Station non necessita di una rete.  
  
-   Sono disponibili connessioni di rete sufficienti per supportare il numero necessario di computer che eseguono MultiPoint Services  
  
-   Dove si trovano i Power Outlet?  
  
-   Sarà necessario un dispositivo di visualizzazione aggiuntivo, ad esempio un proiettore? Se si prevede di usare un proiettore, questo verrà bloccato dal limite o sarà posizionato in una tabella?  
  
-   Quali tipi di cavi saranno necessari e quanti saranno necessari?  
  
-   Si consideri come si desidera espandersi in futuro. Verranno aggiunte altre stazioni?  
  
## <a name="station-layout-and-configuration"></a>Layout e configurazione della stazione  
Il layout fisico del sito potrebbe influire sulla scelta del tipo di stazione. Per altri dettagli sui diversi tipi di stazioni, vedere le [stazioni MultiPoint](MultiPoint-services-Stations.md) in questa guida. In un singolo servizio MultiPoint sono consentiti più tipi di stazione. Questo offre una maggiore flessibilità per soddisfare le esigenze di installazione.  
  
### <a name="layout-for-direct-video-connected-stations"></a>Layout per le stazioni con connessione video diretta  
  
-   Per una stazione connessa con video diretto, la distanza tra i monitoraggi e il computer è limitata dalla lunghezza del cavo video.  
  
-   L'uso di hub intermedi o di hub di stazione concatenati a Margherita è supportato per semplificare la distribuzione, ma il numero massimo consigliato di hub consecutivi è tre. Ciò significa che la distanza massima dal computer all'hub di stazione è 15 metri, perché ogni cavo USB 2,0 ha la lunghezza massima di cinque contatori.  
  
> [!IMPORTANT]  
> Deve essere sempre presente almeno una stazione video connessa diretta per computer per fungere da stazione primaria.  
  
### <a name="layout-for-usb-zero-client-connected-stations"></a>Layout per le stazioni USB zero client connesse  
  
-   L'uso di hub intermedi o di hub di stazione concatenati a Margherita è supportato per semplificare la distribuzione, ma il numero massimo consigliato di hub consecutivi è tre. Ciò significa che la distanza massima dal computer all'hub di stazione è 15 metri, perché ogni cavo USB 2,0 ha la lunghezza massima di cinque contatori.  
  
-   Il numero massimo consigliato di client USB zero connessi a un singolo hub intermedio è tre.  
  
    > [!NOTE]  
    > Alcuni computer dispongono di un hub generico sulla scheda madre, che ha l'effetto di aggiungere un hub aggiuntivo tra l' *hub radice* del computer e gli hub di stazione.  
  
-   Se si usa molto spesso il video, è consigliabile connettere non più di due client USB zero a una porta USB sul server. Se, ad esempio, si usa un hub intermedio, è necessario che siano connessi solo due client USB zero. In alternativa, se si esegue il concatenamento di un client USB zero, solo due client USB zero devono essere concatenati. L'aggiunta di ogni client USB zero alla porta USB sul server riduce la larghezza di banda del video disponibile.  
  
-   Se si prevede di connettere più di tre client USB zero a una singola porta USB sul server, è consigliabile usare USB 3,0 tra il server e l'hub intermedio.  
  
> [!NOTE]  
> Si consiglia di verificare le prestazioni utilizzando le applicazioni e l'hardware per stabilire il numero di client USB zero che è possibile connettere a una porta USB sul server.  
  
![Hub downstream](./media/WMS_diagram6.gif)  
  
**Figura 5** Sistema MultiPoint Services con tre client USB zero connessi a un singolo hub intermedio  
  
### <a name="layout-for-rdp-over-lan-connected-stations"></a>Layout per le stazioni connesse RDP-over-LAN  
Non sono previste limitazioni a distanza fisica per i client LAN. Fino a quando si trovano nella LAN, possono connettersi al sistema MultiPoint Services.  
  
## <a name="using-additional-hubs"></a>Uso di hub aggiuntivi  
Hub aggiuntivi può essere usato per semplificare l'installazione. Sono disponibili tre tipi di hub usati in un sistema MultiPoint Services:  
  
-   [Hub di stazione](#station-hubs)  
  
-   [Hub intermedi](#intermediate-hubs)  
  
-   [Hub downstream](#downstream-hubs)  
  
### <a name="station-hubs"></a>Hub di stazione  
Un hub di stazione è un hub esterno che è stato associato a una stazione MultiPoint Services. Come minimo, l'hub di stazione avrà una tastiera collegata. Potrebbero inoltre essere presenti periferiche aggiuntive collegate. Un hub di stazione può essere un hub USB generico che è conforme alla specifica USB 2,0 o successiva. L'hub di stazione deve essere alimentato esternamente se i dispositivi a elevato consumo eseguiranno il plug-in.  
  
**Hub radice** Un hub USB incorporato nel controller host sulla scheda madre di un computer è noto come *hub radice*. Gli hub di stazione vengono in genere collegati all'hub radice nel computer in cui è in esecuzione MultiPoint Services.  
  
> [!NOTE]  
> Gli hub radice non devono essere usati come hub di stazione. Quando le porte USB sono incorporate in un computer, spesso non è possibile determinare l'hub radice USB a cui sono connesse internamente. Di conseguenza, se la tastiera e il mouse sono stati collegati direttamente alle porte USB del computer, è possibile che si stia effettivamente collegando la tastiera e il mouse a hub radice USB diversi. Per garantire che la tastiera e il mouse si trovino nello stesso hub, collegare un hub di stazione alla porta USB del computer e quindi collegare la tastiera e il mouse all'hub di stazione.  
  
**Stazioni di concatenamento Daisy** Potrebbe essere più facile connettere Hub stazione a un altro hub di stazione invece che direttamente al computer. In questo modo è possibile connettere un hub USB a un hub di stazione già collegato al computer, in modo da avere un hub di stazione collegato a un altro hub di stazione.  
  
Non devono essere presenti più di tre client USB zero o della Margherita concatenati di hub di stazione consecutivi. È necessario prestare attenzione che la larghezza di banda USB non venga superata quando si concatenano gli hub di stazione a Margherita.  
  
![Stazioni con collegamento a margherita](./media/WMS_diagram5.gif)  
  
**Figura 6** Sistema MultiPoint Services con stazioni concatenate a Margherita  
  
### <a name="intermediate-hubs"></a>Hub intermedi  
Un hub intermedio è un hub tra il server e un hub di stazione. Viene in genere usato per aumentare il numero di porte disponibili per gli hub di stazione o per estendere la distanza delle stazioni dal computer. Si consiglia di usare non più di due hub intermedi tra un hub di stazione e il server.  
  
Gli hub intermedi devono essere USB 2,0 o versioni successive e devono essere basati esternamente. Se si connettono più di tre client zero a un hub intermedio, è consigliabile usare USB 3,0 tra il server e l'hub intermedio.  
  
### <a name="downstream-hubs"></a>Hub downstream  
Un hub downstream è connesso a un hub di stazione per aggiungere altre porte disponibili per i dispositivi di stazione. Un hub downstream può essere alimentato esternamente o alimentato da bus, a seconda dei dispositivi collegati all'hub.  
  
![Più connessioni USB zero client](./media/WMS_diagram4.gif "WMS_diagram4")  
  
**Figura 7** Sistema MultiPoint Services con un hub intermedio, un hub di stazione e un hub downstream  
  
## <a name="users-stations-and-computers"></a>Utenti, stazioni e computer  
Il numero di stazioni necessarie dipende dal numero di utenti che dovranno accedere ai computer che eseguono MultiPoint Services nello stesso momento. Analogamente, il numero di computer che eseguono MultiPoint Services sarà necessario dipende dal numero totale di stazioni necessarie. Le stazioni con connessione diretta a video, le stazioni USB-zero-client-connected e le stazioni con connessione RDP-over-LAN sono tutte considerate stazioni. Inoltre, se viene utilizzata la funzionalità a schermo diviso, ciascuna metà viene considerata una stazione.  
  
## <a name="power-considerations"></a>Considerazioni sull'alimentazione  
I componenti seguenti richiedono l'accesso a un Power Strip o un Outlet:  
  
-   Server  
-   Monitoraggi
-   Hub \(intermedi se usati\) 
-   Alcuni client USB zero  
-   Dispositivi USB accesi, ad esempio alcuni dispositivi di archiviazione esterni e unità DVD  
  
## <a name="sample-multipoint-services-system-layouts"></a>Layout di sistema MultiPoint Services di esempio  
A seconda della mobilia disponibile, delle dimensioni della stanza, del numero di computer che eseguono MultiPoint Services e delle stazioni della stanza, esistono diversi modi in cui è possibile disporre le stazioni fisiche. I diagrammi seguenti illustrano cinque possibili alternative.  
  
> [!NOTE]  
> Alcuni di questi diagrammi mostrano un proiettore connesso al sistema MultiPoint Services. Questo è solo un esempio. l'inclusione di un proiettore in un sistema MultiPoint Services è facoltativo.  
  
**Lab del computer** In questa configurazione, le stazioni sono disposte intorno alle pareti della stanza, con gli studenti che si trovano alle pareti.  
  
![Configurazione di una classe con un lab informatico](./media/WMS_ComputerLabLayout.gif)  
  
**Gruppi** di In questa configurazione sono presenti tre computer che eseguono MultiPoint Services, con le stazioni raggruppate intorno a ogni computer.  
  
![Classe configurata con gruppi di server](./media/WMS_ClassroomPods.gif)  
  
**Sala conferenze** In questa configurazione le stazioni sono configurate in righe. Un vantaggio di questa configurazione è che tutti gli studenti facciano parte dell'insegnante.  
  
![Classe configurata come sala conferenze](./media/WMS_LectureRoom.gif)  
  
**Centro attività** Questa configurazione è costituita da un layout tradizionale di conferenze per le scrivanie e ha un'area separata con un singolo computer che esegue MultiPoint Services con le stazioni associate.  
  
![Centro attività servizi MultiPoint](./media/WMSActivityCenter.gif)  
  
**Small Business Office** In questa configurazione, il computer in cui è in esecuzione MultiPoint Services si trova in una posizione centrale e gli utenti in tutta la sede si connettono a \(quest\)' area utilizzando una LAN di rete locale.  
  
![Stazione con connessione USB zero-client](./media/Diagram1.gif)