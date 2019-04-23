---
title: Stazioni MultiPoint
description: Informazioni sulle stazioni servizi MultiPoint, tra cui le diverse opzioni per gli utenti
ms.custom: na
ms.date: 07/22/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: f9f9d618-ccfe-41ea-a52c-00c3c7adb51a
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: e747826a7cd84521bc62e48abedf3092bf6d844c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59855652"
---
# <a name="multipoint--stations"></a>Le stazioni multiPoint
In un ambiente di sistema servizi MultiPoint *stazioni* sono gli endpoint di utente per la connessione al computer che esegue MultiPoint Services. Ogni stazione fornisce all'utente un'esperienza di Windows 10 indipendenti. Sono supportati i seguenti tipi di stazione:  
  
-   Stazioni Direct-video-connessi  
  
-   Stazioni USB zero-client connessi (inclusi i client USB-over-Ethernet zero)  
  
-   RDP-over-LAN-connessi stazioni (per rich client o i computer thin client)  
  
Complete PC con installato il connettore MultiPoint può anche essere monitorato e controllato tramite Dashboard MultiPoint. In Windows 10 il connettore MultiPoint può essere abilitato tramite il pannello di controllo per le funzionalità di Windows. 

Servizi multipoint supporta qualsiasi combinazione di questi tipi di espansione, ma è consigliabile che una postazione in una stazione di direct-video-connessi, che può essere usato come stazione principale. Il motivo di questa raccomandazione è in grado di prevedere scenari di supporto. Ad esempio, per interagire con il sistema BIOS del prima di servizi MultiPoint è in esecuzione.  
  
## <a name="primary-stations-and-standard-stations"></a>Le stazioni primarie e stazioni standard  
Una stazione di direct-video-connessi viene definita come il *stazione principale*. Le stazioni rimanenti sono dette *stazioni standard*.  
  
La stazione principale visualizza le schermate di avvio quando il computer è acceso. Fornisce accesso alle informazioni che è disponibile solo durante l'avvio di risoluzione e la configurazione di sistema. La stazione principale deve essere una stazione di direct-video-connessi. Dopo l'avvio, è possibile usare la stazione principale come qualsiasi altra stazione MultiPoint.  
  
## <a name="direct-video-connected-stations"></a>Stazioni Direct-video-connessi  
Il computer che esegue servizi MultiPoint può contenere più schede video, ognuno dei quali può avere uno o più porte video. In questo modo è possibile collegare i monitoraggi per più stazioni direttamente al computer. Tastiere e mouse sono connessi tramite hub USB che sono associati a ogni monitoraggio. Questi hub sono dette *hub di stazione*. Sono disponibili solo per l'utente di tale stazione e altre periferiche, ad esempio altoparlanti, cuffie o dispositivi di archiviazione USB possono anche essere connessa a un hub di stazione.  
  
> [!IMPORTANT]  
> Deve essere presente almeno un *stazione di direct-video-connessi* per ogni server di agire come la stazione principale per visualizzare il processo di avvio quando il computer è acceso.  
  
![Immagine del layout del sistema basato su USB servizi MultiPoint](./media/WMSMultiPointServerUSBSystemLayout.gif)  
  
**Figura 1** MultiPoint servizi di sistema con quattro stazioni direct-video-connessi  
  
### <a name="BKMK_PS2stations"></a>Stazioni PS/2  
Con MultiPoint Services, è possibile mappare la tastiera PS/2 e il mouse sulla scheda madre di un monitor connesso video diretto per creare una stazione di PS/2. Ad alta definizione audio analogico di scheda madre è l'audio associato a questo tipo di stazioni. Ciò non si applica ai computer in cui non sono presenti alcun prese di PS/2 della scheda madre.  
  
## <a name="usb-zero-client-connected-stations"></a>Stazioni USB zero-client connesso  
Le stazioni connessi tramite USB zero-client usano un *USB zero-client* come hub di stazione. I client USB zero vengono talvolta detta un hub multifunzione con video. Si tratta di un hub che si connette al computer tramite un cavo USB e questi hub supportano in genere un video monitor, un mouse e tastiera (PS/2 o USB), audio e altri dispositivi USB. Questa guida fa riferimento a questi hub specializzati come USB zero client.  
  
Il diagramma seguente viene illustrato un sistema MultiPoint server con una stazione principale (video diretta stazione con connessione) e due client USB zero aggiuntivi stazioni con connessione.  
  
![Stazioni con connessione USB zero-client](./media/WMS11_diagram7.gif)  
  
**Figura 2** sistema MultiPoint Services con una stazione principale e due stazioni USB zero-client connesso  
  
### <a name="usb-over-ethernet-zero-clients"></a>Client USB-over-Ethernet zero  
I client USB-over-Ethernet zero sono una variante del client USB zero che inviano USB tramite LAN al sistema MultiPoint Services. Questi tipi di client USB zero funzionano in modo analogo a altri USB zero client, ma non sono limitati da valori massimi di lunghezza del cavo USB. I client USB-over-Ethernet zero non sono i thin client tradizionali, e vengono visualizzati come dispositivi USB virtuali nel sistema MultiPoint Services. Quando si usano questi dispositivi, vedere il produttore del dispositivo per specifici di prestazioni e pianificazione consigli del sito. La maggior parte dei dispositivi hanno un plug-in di terze parti per la gestione MultiPoint che consente di associare e connettere dispositivi al sistema MultiPoint Services.  
  
## <a name="rdp-over-lan-connected-stations"></a>Stazioni con connessione RDP-over-LAN  
Thin client e desktop tradizionali, laptop o tablet PC, è possibile connettersi al computer che esegue MultiPoint Services tramite la rete locale (LAN) usando Remote Desktop Protocol (RDP) o un protocollo proprietario e Remote Desktop Protocol Provider. Le connessioni RDP offrono un'esperienza utente finale è molto simile a qualsiasi altra stazione MultiPoint, ma Usa hardware del computer client locale. Altre informazioni sui nostri remote desktop le applicazioni disponibili per Android, iOS, Mac e Windows in [i client Desktop remoto](../remote-desktop-services/clients/remote-desktop-clients.md). 
  
I client e dispositivi che eseguono Microsoft RemoteFX possono fornire un'esperienza multimediale sfruttando il processore e video funzionalità hardware del locale thin client o del computer fornire video in alta definizione tramite la rete.  
  
Se si dispone di client LAN esistente servizi MultiPoint possono fornire un modo rapido ed economico aggiornare contemporaneamente tutti gli utenti a un'esperienza di Windows 10.  
  
Da una prospettiva di distribuzione e l'amministrazione, le differenze seguenti esistono quando si usano RDP over-LAN-stazioni connesso:  
  
-   Non limitato a fisico le distanze di connessione USB  
  
-   Possibilità di riutilizzare l'hardware del computer precedente come le stazioni  
  
-   La scalabilità per un numero maggiore di stazioni. Qualsiasi client nella rete potenzialmente può essere usato come stazione remota  
  
-   Nessun hardware risoluzione dei problemi tramite la console di gestione MultiPoint  
  
-   Alcuna funzionalità con schermo diviso.  
  
    Per altre informazioni, vedere [stazioni con schermo diviso](#a-namebkmksplitscreenstationsasplit-screen-stations) più avanti in questo argomento  
  
-   Non rinominare stazione o configurare l'accesso automatico tramite la console di gestione MultiPoint  
  
![Stazione con connessione USB zero-client](./media/Diagram1.gif)  
  
**Figura 3** sistema MultiPoint Services con le stazioni RDP-over-LAN-connesso  
  
## <a name="additional-configuration-options"></a>Opzioni di configurazione aggiuntive  
  
### <a name="BKMK_SplitscreenStations"></a>Stazioni con schermo diviso  
Servizi multiPoint offre un'opzione di schermo split nei computer con le stazioni direct-video-connessi o in altre stazioni USB zero-client connesso. Un'unica schermata offre la possibilità di creare una stazione aggiuntiva per ogni monitoraggio. Invece di due monitor, è possibile utilizzare un monitoraggio con due configurazioni di hub di stazione per creare due stazioni con un solo monitor. È possibile aumentare il numero di stazioni disponibile rapidamente senza dover acquistare altri monitoraggi, USB zero client o le schede video.  
  
Vantaggi dell'uso di una stazione con schermo diviso possono includere:  
  
-   Riduzione dei costi e lo spazio da tenere conto di altri utenti in un sistema MultiPoint Services.  
  
-   Consentire agli due utenti collaborare side-by-side in un progetto.  
  
-   Consentendo un insegnante di illustrare una procedura su una stazione mentre uno studente segue su altra stazione.  
  
Tutti i servizi MultiPoint stazione monitor con risoluzione 1024 x 768 o versione successiva può essere suddivisa in due schermate di stazione. Per la divisione dello schermo utente un'esperienza ottimale, è consigliabile una schermata con una minima risoluzione di 1600 x 900 wide. È anche consigliabile una tastiera mini senza un tastierino numerico consente due tastiere davanti il monitoraggio per.  
  
Per creare stazioni con schermo diviso, configuri una stazione di direct-video-connessi o USB zero-client connesso. Quindi aggiungere un hub di stazione aggiuntive mediante l'inserimento di una tastiera e mouse a un hub USB connesso al server. È quindi possibile convertire la stazione in due stazioni tramite Gestione MultiPoint per suddividere lo schermo ed eseguire il mapping del nuovo hub a metà del monitoraggio. La metà sinistra dello schermo diventa una postazione e metà destra diventa una stazione di secondo.  
  
Dopo una stazione è stata divisa, un utente può accedere alla stazione a sinistra mentre un altro utente accede alla stazione sulla destra.  
  
![Workstation con schermo diviso](./media/WMS_diagram3.gif)  
  
**Figura 4** sistema MultiPoint Services con stazioni con schermo split  
  
## <a name="BKMK_StationTypeComparison"></a>Confronto di tipo stazione  
  
||Video diretta connesso|USB Zero-Client connesso|RDP-over-LAN Connected|  
|-|--------------------------|-----------------------------|----------------------------|  
|Video sulle prestazioni|Consigliato per ottenere prestazioni ottimali video||Usare i thin client che supportano RemoteFX per la migliore qualità video inferiore larghezza di banda di rete|  
|Limiti fisici|Limitati dalla lunghezza del video via cavo e hub USB e cavo lungo (lunghezza massima di 15 consigliato misuratore)|Limitazioni di frequenza da hub USB e cavo lungo (lunghezza massima di 15 consigliato misuratore)|Limitazioni di frequenza da distribuzione LAN|  
|Numero di stazioni consentiti |Limitato dal numero di slot PCIe della scheda madre volte le porte video per ogni scheda video|Numero totale può essere limitato dal produttore del client USB zero (per altre informazioni, vedere la nota dopo questa tabella).|Limitato per le porte disponibili nel commutatore di rete|  
|Con schermo diviso|Yes|Yes|No|  
|Stazione di gestione multiPoint stazione periferico, configurazione di accesso automatico, ridenominazione|Yes|Yes|No|  
|Accesso ai menu di avvio server|Yes|No|No|  
  
> [!NOTE]  
> Il numero totale di client USB zero connessi al server può essere limitato dal produttore o le funzionalità hardware del computer che esegue MultiPoint Services.