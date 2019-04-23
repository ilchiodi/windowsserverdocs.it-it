---
title: Pianificazione del sito di Servizi MultiPoint
description: Informazioni sulla pianificazione per le distribuzioni di servizi MultiPoint in Windows Server 2016
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 063783cd-d748-489e-b175-46eadc993f7a
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 60d2e1fedc018136f5668d55a8afb2e0574d94de
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845072"
---
# <a name="multipoint-services-site-planning"></a>Pianificazione del sito di Servizi MultiPoint
È necessario considerare la posizione in cui verrà distribuito uno o più computer che esegue MultiPoint Services e le stazioni associate.  
  
Il computer che esegue MultiPoint Services ruolo deve avere un pratico accesso a un alimentatore e per le periferiche che sono connessi direttamente a essa, ad esempio una stampante. Inoltre, il computer che esegue MultiPoint Services deve avere un pratico accesso a una connessione di rete. È necessaria per l'accesso a Internet, una connessione di rete e in cui è disponibile, una rete LAN.  
  
Altri fattori da considerare includono quanto segue:  
  
-   Sarà nel sistema MultiPoint Services configurato in una camera specifica, o essere configurerà in un carrello della spesa in sequenza o una tabella, in modo che può essere spostato da una posizione?  
  
    > [!NOTE]  
    > Se si prevede di usare una configurazione per dispositivi mobili, è possibile *associare* le stazioni servizi MultiPoint ogni volta che vengono riconnessi per assicurarsi che ogni tastiera e mouse è associato il monitoraggio appropriato.  
  
-   La stazione principale si troverà accanto alle altre stazioni o verrà separato? Ad esempio, se il sistema MultiPoint Services è configurato in una classe, verrà la stazione principale in ufficio del docente e le stazioni standard posizionata altrove nella chat room? Quando viene riavviato il computer che esegue servizi MultiPoint, stazione principale avranno accesso per le schermate di avvio. Se si è preoccupati per questo livello di accesso in un'impostazione in aula, è preferibile inserire la stazione principale alla scrivania del docente.  
  
-   Le stazioni di quanti possa essere inserito nella chat room?  
  
-   È necessaria una rete? Una soluzione server singolo che utilizza connesse video diretta o USB zero-client connesso stazioni non è necessaria una rete.  
  
-   Sono presenti connessioni di rete sufficienti nella chat room per supportare il numero necessario di computer che eseguono MultiPoint Services  
  
-   Dove si trovano le prese di corrente?  
  
-   È necessario un dispositivo di visualizzazione aggiuntive, ad esempio un proiettore? Se si prevede di usare un proiettore, verrà interrotta dal tetto massimo, o posizionato in una tabella?  
  
-   Tipologia di cavi verrà richiesto e il numero di essere necessaria?  
  
-   Prendere in considerazione come si potrebbe voler espansione in futuro. Verranno aggiunte altre stazioni?  
  
## <a name="station-layout-and-configuration"></a>Configurazione e il layout di stazione  
Il layout fisico del sito potrebbe influire sulla scelta del tipo stazione. Per altre informazioni sui tipi diversi di stazione, vedere [stazioni MultiPoint](MultiPoint-services-Stations.md) in questa Guida. Più tipi di stazione sono consentiti in un singolo di MultiPoint Services. Ciò consente una flessibilità aggiuntiva per soddisfare le esigenze di installazione.  
  
### <a name="layout-for-direct-video-connected-stations"></a>Layout per le stazioni direct-video-connessi  
  
-   Per una stazione di direct-video-connessi, la distanza tra i monitoraggi e il computer è limitata dalla lunghezza del video via cavo.  
  
-   Uso di hub intermedio o hub di stazione a cascata è supportata per semplificare la distribuzione, ma il numero massimo consigliato di hub consecutivi è tre. Ciò significa che la distanza massima dal computer per l'hub di stazione è 15 metri, perché ogni cavo USB 2.0 è la lunghezza massima di cinque misuratori.  
  
> [!IMPORTANT]  
> Deve esistere sempre almeno una diretta stazione connesso video per ogni computer di agire come la stazione principale.  
  
### <a name="layout-for-usb-zero-client-connected-stations"></a>Layout per client USB zero stazioni con connessione  
  
-   Uso di hub intermedio o hub di stazione a cascata è supportata per semplificare la distribuzione, ma il numero massimo consigliato di hub consecutivi è tre. Ciò significa che la distanza massima dal computer per l'hub di stazione è 15 metri, perché ogni cavo USB 2.0 è la lunghezza massima di cinque misuratori.  
  
-   Il numero massimo consigliato di USB zero client connessi a un singolo hub intermedio è tre.  
  
    > [!NOTE]  
    > Alcuni computer dotati di un hub generico sulla scheda madre, che ha l'effetto dell'aggiunta di un hub aggiuntivo tra le *hub radice* del computer e gli hub di stazione.  
  
-   Se verrà utilizzata principalmente video, è consigliabile che i client USB zero non più di due sia connettersi a una porta USB nel server. Ad esempio, se viene usato un hub intermedio, i client USB zero solo due devono connesso. O se sei un collegamento a margherita concatenamento client USB zero, solo due USB zero client devono essere incatenati insieme. L'aggiunta di ogni client USB zero per la porta USB del server consente di ridurre la larghezza di banda video disponibile.  
  
-   Se si prevede di connettere più di tre client USB zero a una singola porta USB nel server, è consigliabile utilizzare USB 3.0 tra il server e l'hub intermedio.  
  
> [!NOTE]  
> È consigliabile che è verificare le prestazioni usando le applicazioni e hardware per decidere quanti USB zero client, è possibile connettersi a una porta USB nel server.  
  
![Hub downstream](./media/WMS_diagram6.gif)  
  
**Figura 5** sistema MultiPoint Services con tre USB zero client connessi a un singolo hub intermediario  
  
### <a name="layout-for-rdp-over-lan-connected-stations"></a>Stazioni con connessione layout per RDP-over-LAN  
Non esistono limitazioni distanza fisica per i client LAN. Purché siano nella rete LAN, possono essere connessi al sistema MultiPoint Services.  
  
## <a name="using-additional-hubs"></a>Uso di hub aggiuntive  
Altri hub è utilizzabile per semplificare l'installazione. Esistono tre tipi di hub che vengono usati in un sistema MultiPoint Services:  
  
-   [Hub di stazione](#Station-hubs)  
  
-   [Hub intermediario](#Intermediate-hubs)  
  
-   [Hub downstream](#Downstream-hubs)  
  
### <a name="station-hubs"></a>Hub di stazione  
Hub di stazione è un hub esterno che è stato associato a una stazione MultiPoint Services. Come minimo, l'hub di stazione avrà una tastiera alimentazione da rete elettrica aggiuntivo a esso. È anche possibile che altre periferiche collegate. Un hub di stazione può essere un hub USB generico che è conforme all'USB 2.0 o versioni successive. Hub di stazione deve essere spenti esternamente se i dispositivi avanzati verranno plug-in a essi.  
  
**Hub radice** hub USB A predefiniti per il controller host sulla scheda madre del computer è noto come un *hub radice*. Hub di stazione sono in genere alimentazione da rete elettrica aggiuntivo per l'hub radice del computer che esegue MultiPoint Services.  
  
> [!NOTE]  
> Non usare gli hub radice come hub di stazione. Quando le porte USB sono incorporate in un computer, spesso non è possibile determinare quale hub USB root internamente sono connessi a. Di conseguenza, se è collegato aggiuntivo stazione tastiera e mouse direttamente alle porte USB del computer, si può effettivamente essere inserimento-in di tastiera e mouse hub radice USB diversi. Per garantire che la tastiera e mouse sullo stesso hub, plug-in un hub di stazione a porta USB del computer e quindi plug-in di tastiera e mouse che hub di stazione.  
  
**Collegamento a margherita concatenamento stazioni** può risultare più hub di stazione a un altro hub di stazione anziché direttamente nel computer. In questo modo è possibile collegare un hub USB a un hub di stazione che è già collegato al computer, in modo da avere un hub di stazione collegato a un altro hub di stazione.  
  
Dovrebbe esserci client USB zero non più di tre o stazione hub cascata consecutivamente. Prestare attenzione che non venga superata la larghezza di banda USB quando margherita hub di stazione.  
  
![Stazioni con collegamento a margherita](./media/WMS_diagram5.gif)  
  
**Figura 6** sistema MultiPoint Services con oggetti finestra a cascata  
  
### <a name="intermediate-hubs"></a>Hub intermediario  
Un hub intermedio è un hub di lunghezza compresa tra il server e un hub di stazione. Viene in genere usato per aumentare il numero di porte disponibili per gli hub di stazione o per estendere la distanza delle stazioni dal computer. Si consiglia di non più di due hub intermediario vengono utilizzati tra i server e un hub di stazione.  
  
Hub intermediario deve essere USB 2.0 o versione successiva e deve essere spenti esternamente. Se ci si connette più di tre client USB zero a un hub intermedio, USB 3.0 è consigliabile tra il server e l'hub intermedio.  
  
### <a name="downstream-hubs"></a>Hub downstream  
Un hub downstream è connesso a un hub di stazione per aggiungere porte non è più disponibile per i dispositivi stazione. Un hub downstream può essere alimentato esternamente o bus-con tecnologia, a seconda di dispositivi collegati in all'hub.  
  
![Più connessioni client USB zero](./media/WMS_diagram4.gif "WMS_diagram4")  
  
**Figura 7** sistema MultiPoint Services con un hub intermedio, un hub di stazione e un hub downstream  
  
## <a name="users-stations-and-computers"></a>Le stazioni, utenti e computer  
Il numero di stazioni che sarà necessario dipende dal numero di persone che dovranno accedere ai computer che esegue MultiPoint Services nello stesso momento. Allo stesso modo, il numero di computer che esegue MultiPoint Services, sarà necessario dipende dal numero totale delle stazioni necessari. Le stazioni Direct-video-connessi e stazioni connessi tramite USB zero-client RDP-over-LAN-connessi stazioni sono tutte le stazioni considerate. Inoltre, se viene utilizzata la funzionalità con schermo diviso, ogni semestre viene considerato una stazione.  
  
## <a name="power-considerations"></a>Considerazioni di risparmio energia  
I componenti seguenti richiedono l'accesso a una striscia di alimentazione o outlet:  
  
-   Server  
-   Monitoraggi
-   Hub a livello intermedio \(se usato\) 
-   Alcuni client USB zero  
-   Dispositivi con tecnologia USB, ad esempio alcuni dispositivi di archiviazione esterni e unità DVD  
  
## <a name="sample-multipoint-services-system-layouts"></a>Layout del sistema MultiPoint Services di esempio  
A seconda di arredi disponibili, le dimensioni della chat room, il numero di computer che eseguono MultiPoint Services e le stazioni nella chat room, esistono diversi modi che possono essere disposte le stazioni fisiche. I diagrammi seguenti illustrano cinque alternative possibili.  
  
> [!NOTE]  
> Alcuni di questi diagrammi mostrano un proiettore connesso al sistema MultiPoint Services. Questo è solo un esempio; tra cui un proiettore in un sistema MultiPoint Services è facoltativo.  
  
**Laboratorio informatico** In questa configurazione, le stazioni sono disposti intorno le pareti del stanza, insieme agli studenti le pareti affiancate.  
  
![Configurazione di una classe con un lab informatico](./media/WMS_ComputerLabLayout.gif)  
  
**Gruppi** In questa configurazione, sono presenti tre computer che eseguono MultiPoint Services, con le stazioni intorno a ogni computer in cluster.  
  
![Classe configurata con gruppi di server](./media/WMS_ClassroomPods.gif)  
  
**Lezione chat room** In questa configurazione le stazioni impostate nelle righe. Un vantaggio di questa configurazione è che tutti gli studenti devono affrontare l'insegnante.  
  
![Classe configurata come sala conferenze](./media/WMS_LectureRoom.gif)  
  
**Centro attività** questa configurazione è costituito da un layout tradizionale di sala conferenze per la prossimità e ha un'area separata con un singolo computer che esegue MultiPoint Services con le stazioni associate.  
  
![MultiPoint Services Activity Center](./media/WMSActivityCenter.gif)  
  
**Office Small business** In questa configurazione, il computer che esegue MultiPoint Services viene posizionato in una posizione centrale e gli utenti in tutta l'office connettersi ad esso tramite una rete locale \(LAN\).  
  
![Stazione con connessione USB zero-client](./media/Diagram1.gif)