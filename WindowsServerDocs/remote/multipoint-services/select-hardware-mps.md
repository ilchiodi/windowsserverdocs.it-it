---
title: Scelta dell'hardware per il sistema di Servizi MultiPoint
description: Considerazioni sull'hardware per servizi MultiPoint
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: e74961a2-bd38-48ae-b1c0-4b3eff761b4a
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 9cfd6572c82bf5c3754165420e61054ec12b9617
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71388999"
---
# <a name="selecting-hardware-for-your-multipoint-services-system"></a>Scelta dell'hardware per il sistema di Servizi MultiPoint
Quando si compila un sistema di servizi MultiPoint, è consigliabile selezionare un computer che soddisfi i requisiti di sistema di Windows Server 2016. Se si decide di selezionare i componenti, considerare quanto segue:  
  
-   Intervallo di prezzi di destinazione della soluzione completa.  
  
-   I tipi di scenari di utilizzo previsto per il sistema di servizi MultiPoint, ad esempio se gli utenti sono in esecuzione programmi multimediali, utilizzando l'elaborazione di testi o applicazioni di produttività o esplorazione di Internet.  
  
-   Se lo scenario ha le richieste di elaborazione o di memoria elevate.  
  
-   Il numero di utenti che potrebbero utilizzare il sistema nello stesso momento. Se si prevede di usare contemporaneamente molti utenti nel sistema o se gli utenti usano programmi a elevato utilizzo di sistema, è consigliabile pianificare una maggiore potenza di calcolo per il sistema.  
  
-   Il tipo di stazioni. Il numero di porte USB o porte video è necessario?  
  
-   Piani di espansione futura. Si intende aggiungere stazioni al sistema MultiPoint servizi in un secondo momento? Si avrà sufficiente slot della scheda video, porte USB o connettività di rete? Il numero di utenti aggiuntivo sarà necessario per supportare l'hardware?  
  
-   Layout fisico. Per ulteriori informazioni, vedere [pianificazione del sito servizi MultiPoint](MultiPoint-services-Site-Planning.md).  
  
In genere, un sistema di servizi MultiPoint include i componenti seguenti:  
  
-   Un computer che esegue servizi MultiPoint, che include una CPU, RAM, dischi rigidi e le schede video.  
  
-   Un monitoraggio, hub stazione, tastiera e mouse per ogni postazione.  
  
-   Facoltative periferiche per le stazioni MultiPoint servizi, tra cui altoparlanti, cuffie, microfoni o i dispositivi di archiviazione sono disponibili solo per l'utente dell'oggetto.  
  
-   Facoltative periferiche che sono disponibili a tutti gli utenti del sistema, ai servizi MultiPoint connesso direttamente al computer host, ad esempio stampanti, dischi rigidi esterni e dispositivi di archiviazione USB.  
  
Utilizzare le seguenti informazioni per prendere decisioni hardware:  
  
-   [Selezione di una CPU](#selecting-a-cpu)  
-   [Selezione dei componenti hardware](#selecting-hardware-components)  
  
## <a name="selecting-a-cpu"></a>Selezione di una CPU  
Un sistema di servizi MultiPoint è un multiplo\-ambiente utente, con tutti gli utenti connessi a un singolo computer host. In questo modo si aumenta l'utilizzo della CPU in quanto tutti gli utenti condividono lo stesso computer. Alcune attività, ad esempio programmi multimediali \(ad esempio, lettori multimediali o video\-software di montaggio\), sono richieste di elaborazione di dimensioni maggiori. Pertanto, assicurarsi di selezionare una CPU in grado di gestire i requisiti di elaborazione per il numero di utenti e i tipi di scenari che è necessario supportare.  
  
Servizi multiPoint richiede un x64\-basato su CPU e deve soddisfare i requisiti di sistema per il computer come descritto in [i requisiti Hardware e consigli sulle prestazioni](Hardware-Requirements-and-Performance-Recommendations.md).  
  
I seguenti tipi di processori sono stati testati per essere utilizzato in un sistema di servizi MultiPoint con elevata\-richiedono programmi di elaborazione, ad esempio programmi multimediali:  
  
-   **Processore\-Dual Core:** Può supportare fino a 8 stazioni.  
-   **Processore\-quad core:** Può supportare fino a 16 stazioni.
-   **Processore\-quad core con multithreading:** Può supportare fino a 20 stazioni.      
-   **Processore\-a sei core con multithreading:** Può supportare fino a 24 stazioni.  
  
Con queste informazioni, selezionare una CPU che soddisfi i requisiti di elaborazione per il sistema di servizi MultiPoint. 
> [!NOTE] 
> Se si eseguono applicazioni a elevato utilizzo video si consiglia almeno una base per stazione di base. 
  
## <a name="selecting-hardware-components"></a>Selezione dei componenti hardware  
Quando si compila un sistema di servizi MultiPoint, considerare i seguenti componenti hardware che potrebbe essere necessario:  
  
-   Hardware video  
  
-   Servizi multiPoint stazione hardware  
  
    -   *Hub USB*  
  
    -   Client USB zero  
  
    -   Tastiere e mouse  
  
    -   Monitoraggi  
  
-   Periferiche  
  
    -   Dispositivi audio, ad esempio altoparlanti e cuffie  
  
    -   Microfoni  
  
    -   Dispositivi di archiviazione di massa USB  
  
Dopo aver selezionato i componenti hardware del sistema MultiPoint servizi, assicurarsi di ottenere corrente, 64\-i driver per i componenti di bit.  
  
Gli argomenti seguenti forniscono informazioni dettagliate che consentono di selezionare i componenti del sistema MultiPoint servizi:  
  
[Selezione dell'hardware video](#selecting-video-hardware)  
[Selezione dei\-dispositivi\-Direct video Connected o USB zero client Station](#BKMK_Selectingdirect-video-connectedorUSBzeroclientstationdevices)   
[Selezione di altre periferiche della stazione](#selecting-other-station-peripheral-devices)  
[Selezione dell'\-hardware\-della\-stazione connessa RDP su LAN](#BKMK_SelectingRDP-over-LAN-connectedstationhardware)  
[Selezione dei dispositivi audio](#selecting-audio-devices)  
  
## <a name="selecting-video-hardware"></a>Scelta dell'hardware video
L'hardware video selezionato deve supportare il numero di monitor che risulteranno necessari per il numero di utenti che si intende disporre funzionante in stazioni servizi MultiPoint. Inoltre, i diversi tipi di hardware video possono fornire un livello più elevato\-Esplora prestazioni per la grafica\-programmi con utilizzo intensivo, ad esempio il contenuto multimediale.  
  
Selezionare l'hardware video in grado di supportare il numero massimo di monitor per il tipo di prestazioni richiesto dal sistema servizi MultiPoint. Assicurarsi di convalidare le prestazioni dell'hardware del video che si desidera assicurarsi che soddisfi i requisiti di prestazioni.  
  
> [!NOTE]  
> È necessario installare un driver video che supporta l'estensione del desktop su più monitor.  
  
Le opzioni hardware video includono:  
  
-   Schede video che utilizzano un PCI o un'interfaccia di bus PCIe  
  
-   Controller video esterno connesso tramite USB  
  
Nelle sezioni seguenti descrivono le funzionalità di ognuno di questi tipi di hardware video. È possibile combinare schede video e controller video esterni per il sistema che desidera creare.  
  
### <a name="internal-video-cards"></a>Schede video  
Una scheda video interna sia collegata\-nella scheda madre del computer. La scheda video interna è una soluzione che può migliorare le prestazioni della grafica\-programmi multimediali con utilizzo intensivo. Tuttavia, è necessario uno slot PCI o PCIe disponibile per collegare una scheda video interna\-nella scheda madre. Molti elevata\-schede video prestazioni richiedono uno slot PCIe, ma sono presenti un numero limitato di slot PCIe della scheda madre. È necessario conoscere il tipo di slot della scheda video sono disponibili nel computer in modo che è possibile acquistare il tipo corretto di schede video.  
  
Il numero di monitor in grado di connettersi a ogni scheda video varia a seconda della GPU da utilizza per la scheda e il numero di porte che supporta, in genere compreso tra 2 e 6.  
  
Quando si seleziona schede video, selezionare le schede video che supportano il numero di monitor necessarie per creare il numero desiderato di stazioni connesse video dirette. Il numero massimo di monitor che possono essere supportati è uguale al numero di schede video che sono collegati\-nella scheda madre moltiplicato per il numero di porte di monitoraggio in ognuno di tali schede video. Ad esempio, se si dispone di due schede video e ogni scheda ha due porte di monitoraggio, si possono supportare fino a quattro monitor.    
  
### <a name="external-video-controllers"></a>Controller video esterni  
I client USB zero contengono un controller video esterno per collegare un monitor al client. Il client USB zero può includere inoltre le connessioni per le cuffie, altoparlanti, un microfono o altre periferiche.  
  
Selezionare una porta USB zero client se si desidera abilitare il supporto per monitor aggiuntivi senza aprire il computer o se si desidera supportare ulteriori stazioni di output video disponibili. Ad esempio, se in precedenza erano quattro monitor collegato\-in schede video e si desidera aggiungere due ulteriori monitor, è possibile collegare\-in due controller video esterno al computer e per i due più monitor. In questo modo, è possibile combinare un USB zero-client con il controller video e non utilizzare slot PCI o PCIe aggiuntivi sulla scheda madre.  
  
## <a name="BKMK_Selectingdirect-video-connectedorUSBzeroclientstationdevices"></a>Selezione dei\-dispositivi\-Direct video Connected o USB zero client Station  
Una stazione di servizi MultiPoint è costituito da un hub di stazione o USB zero client con una tastiera e mouse collegato\-in e un monitor collegato\-al computer host o in un client USB zero. Altre periferiche possono essere inserite\-in per la stazione di hub o USB zero client, ma non è necessario creare stazione MultiPoint. Le altre periferiche sono descritti in [selezionando altre periferiche stazione](#selecting-other-station-peripheral-devices).  
  
I dispositivi che si desidera per creare una stazione di servizi MultiPoint devono soddisfare i requisiti minimi per lavorare con i servizi MultiPoint. In questo argomento vengono fornite informazioni dettagliate sui requisiti per i dispositivi stazione MultiPoint servizi seguenti:  
  
-   [Selezione degli hub USB](#selecting-usb-hubs)  
-   [Selezione dei client USB zero](#selecting-usb-zero-clients)  
-   [Selezione di tastiere e dispositivi mouse](#selecting-keyboards-and-mouse-devices)  
-   [Selezione di monitoraggi](#selecting-monitors)  
  
### <a name="selecting-usb-hubs"></a>Selezionare hub USB  
Gli hub USB che vengono utilizzati in un sistema di servizi MultiPoint possono essere un hub USB generico. Tale hub dispongono in genere di quattro o più porte USB e consentono a più dispositivi USB da collegare a una singola porta USB del computer. Altri dispositivi, ad esempio tastiere e monitor, possono inoltre comportare un hub USB in fase di progettazione.  
  
Si consideri inoltre l'utilizzo di un *esternamente con tecnologia* hub, anziché un *bus\-con tecnologia* hub. Con bus\-hub spenta, la quantità di corrente che viene fornito dall'host del computer deve essere sufficiente a fornire la potenza per tutte le periferiche che sono collegati\-nell'hub, senza compromettere le prestazioni di sistema. Un hub esternamente spenta consente di collegare più periferiche e forniscono potenza sufficiente per tutti gli elementi. L'utilizzo di hub esternamente spenta può aiutare a evitare i problemi di prestazioni, errori e altri problemi intermittenti.  
  
Quando si seleziona un hub USB per il sistema di servizi MultiPoint, prendere in considerazione l'utilizzo. L'hub può essere utilizzato come un *hub stazione*, un *hub intermedio*, o un *hub downstream*. Fare riferimento alla tabella seguente per una descrizione su ogni tipo di hub. Si consiglia di tutti i dispositivi USB da USB 2.0 o versione successiva.
  
||Con tecnologia|  
|-|-----------|  
|Stazione Hub|Può essere bus\-alimentato a meno che non elevata\-dispositivi con tecnologia verranno inseriti\-nelle o un hub downstream verrà connesso.|  
|Hub intermedia |L'alimentazione esternamente|  
|Hub downstream|Possono essere spenti esternamente o bus con tecnologia a seconda dei dispositivi che sono collegati\-all'hub|  
|Cavo di estensione USB Active|Cavi USB attivi che includono un hub USB sono in genere bus alimentato; Pertanto, non sono consigliate per la connessione hub stazione al computer.|  
  
### <a name="selecting-usb-zero-clients"></a>Selezione USB zero client  
Un client USB zero è un hub USB che contiene un output video. Pertanto, consente un monitoraggio di essere connessi al computer tramite una connessione USB. Per ulteriori informazioni sull'utilizzo di client USB zero per video, vedere [scelta dell'hardware video](#selecting-video-hardware) in questo documento. Un client USB zero anche possibile abilitare la connessione di una varietà di USB e non\-di dispositivi USB per l'hub. I client USB zero vengono prodotte dai produttori di hardware specifico e richiede l'installazione di un dispositivo\-driver specifico.  
  
### <a name="selecting-keyboards-and-mouse-devices"></a>Selezione di tastiere e mouse  
I dispositivi di tastiera e mouse che si collega\-alla stazione corrisponderà in genere i dispositivi USB. Alcuni USB zero client forniscono PS\/2 porte, nel qual caso, la tastiera e mouse debba utilizzare PS\/2 per la connessione all'hub di espansione. È inoltre possibile utilizzare un server di elaborazione\/2 della tastiera e mouse se si configura un server di elaborazione\/2 diretto\-video\-stazione con connessione.  
  
Come hub di espansione, è possibile utilizzare una tastiera con un hub interno. Tuttavia, tutti gli altri dispositivi stazione necessario connettersi a hub interno utilizzando porte sulla tastiera. Se tali una tastiera è connesso al computer tramite un altro hub, tale hub verranno considerati come un hub intermediario.  
  
Se si utilizza split\-stazioni con schermo, si consiglia di utilizzare una tastiera mini che non dispone di un tastierino numerico, in modo che sia in grado di soddisfare due tastiere davanti il monitoraggio.  
  
### <a name="selecting-monitors"></a>Selezione di monitor  
Dovrebbe esserci un monitor fornito per ogni stazione MultiPoint servizi, a meno che una divisione\-schermata è stata pianificata. I monitoraggi sono collegati alla scheda video del computer, il client USB zero o il client basato su\-LAN. Qualsiasi tipo di monitoraggio supportati dalla scheda video, USB zero-client o LAN\-basata su client può essere utilizzato, tra cui i monitor CRT.  
  
Alcuni monitoraggi speciali includono una LAN interna\-basato su client o USB zero-client. Tali monitoraggi in genere contiene input audio\/output jack e hub USB interno per la connessione tastiere e mouse. Questi si connettono al server tramite USB o una connessione LAN.  
  
#### <a name="display-resolution"></a>Risoluzione dello schermo  
La risoluzione minima supportata per l'area di visualizzazione di una stazione è 512 x 768 pixel. Se il sistema MultiPoint Services viene avviato e rileva che l'area di visualizzazione di una stazione è inferiore alla risoluzione minima, verrà visualizzata una schermata vuota in tale stazione e la stazione non sarà utilizzabile.  
  
Se un monitor di visualizzazione sta per essere condiviso da due stazioni come divisione\-schermata stazioni, il requisito minimo per la visualizzazione è 1024 x 768, in modo che le aree dello schermo stazione singoli risultanti siano almeno 512 x 768. Per il miglior suddivise\-esperienza utente di schermata, è consigliabile una schermata con un minimo di risoluzione di 1600 x 900 wide.  
  
## <a name="selecting-other-station-peripheral-devices"></a>Selezione di altre periferiche stazione  
Servizi multiPoint supporta periferiche connesse a un hub, stazione un USB zero-client, o direttamente al computer. I dispositivi collegati a un hub stazione verrà associati a tale stazione specifica. Altri dispositivi sono disponibili per ogni stazione quando collegato direttamente al computer. I client LAN possono inoltre supportare periferiche.  
  
> [!IMPORTANT]  
> Non è possibile connettere una tastiera a un hub downstream \(ad esempio, un hub che è collegato a un hub stazione\). Se si collega una tastiera a un hub downstream, i dispositivi che sono collegati\-in hub downstream non saranno disponibili per tale stazione. Questo comportamento consente il supporto del collegamento a margherita\-concatenate hub stazione.  
  
**Disponibile a tutte le stazioni** dispositivo USB A cui è collegato al computer \(ad esempio, non tramite un hub stazione\) è disponibile per tutte le stazioni. A seconda del dispositivo e può essere utilizzato contemporaneamente da più utenti o solo un utente può accedere alla volta. Nella tabella seguente viene illustrato come accedere ai dispositivi USB.  
  
> [!NOTE]  
> La colonna "Connesso al Computer Host" nella tabella si riferisca al comportamento quando il computer che esegue servizi MultiPoint è in esecuzione in modalità stazione con stazioni. Se si esegue in modalità console, le periferiche che sono collegate ovunque si comportano esattamente come un server standard in una sessione della console.  
  
||Connesso al Computer Host|Connesso a Hub di stazione o a valle|  
|-|------------------------------|----------------------------------------------|  
|Tastiera|Non sono funzionanti, a meno che non fa parte di una stazione di PS/2. |Stazione disponibile a singole<br /><br />Non può essere connessa a un hub downstream|  
|Mouse|Non sono funzionanti, a meno che non fa parte di una stazione di PS/2. |Stazione disponibile a singole|  
|Altoparlanti/cuffie|Non sono funzionanti, a meno che non fa parte di una stazione di PS/2.|Stazione disponibile a singole|  
|Dispositivo di archiviazione USB|Disponibile a tutte le stazioni|Stazione disponibile a singole|  
|Controllo Consumer HID|Non funzionante|Stazione disponibile a singole|  
|Altri dispositivi USB, quali fotocamere, lettori di documenti e unità DVD|Disponibile a tutte le stazioni se supportato da Windows Server 2012|Disponibile a tutte le stazioni se supportato da Windows Server 2008 R2 Servizi Desktop remoto|  
  
## <a name="BKMK_SelectingRDP-over-LAN-connectedstationhardware"></a>Selezione dell'\-hardware\-della\-stazione connessa RDP su LAN  
Qualsiasi client LAN che possono connettersi a Servizi Desktop remoto, utilizzando Remote Desktop Protocol, può diventare una stazione di servizi MultiPoint.  
  
Se si desidera che il client di LAN a essere utilizzato solo come stazione MultiPoint, si desidera "bloccare" il client di rete LAN. Ad esempio, configurare il thin client in modo che può connettersi a una sessione di servizi MultiPoint, o configurare i computer desktop in modo che l'accesso alle icone del desktop e Menu Start elementi come un web browser viene rimossa per impedire l'accesso diretto a Internet. È possibile rendere tali configurazioni tramite strumenti di configurazione client LAN o gruppo o criteri locali.  
  
## <a name="selecting-audio-devices"></a>Selezionare i dispositivi audio  
È importante assicurarsi che quando si selezionano dispositivi audio, e possono essere inseriti nell'hub stazione, USB zero-client o client LAN. Alcuni hub USB, USB zero client e client delle reti LAN sono spina audio analogico che può essere utilizzato con i dispositivi audio analogici tradizionali \(ad esempio le cuffie o auricolari\). Hub stazione che non dispongono di jack analogico possono usare dispositivi audio USB.  
  
Se è stata configurata\/una stazione\-di\-connessione video diretta di PS\/2 usando le porte PS 2 sulla scheda madre del computer per la tastiera e il mouse, è necessario usare l'audio analogico sulla scheda madre del computer in ordine di disponibilità del dispositivo audio per questa stazione quando il sistema MultiPoint Services è in esecuzione in modalità stazione.  
  
Se non si dispone di una stazione\/di connessione\-video\-diretta di PS 2, il dispositivo audio host sulla scheda madre del sistema sarà disponibile solo quando il sistema di servizi multipoint è in esecuzione in modalità console.  