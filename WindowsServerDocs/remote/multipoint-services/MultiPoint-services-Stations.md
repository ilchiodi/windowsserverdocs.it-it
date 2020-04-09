---
title: Stazioni MultiPoint
description: Informazioni sulle stazioni in MultiPoint Services, incluse le diverse opzioni per gli utenti
ms.date: 07/22/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: f9f9d618-ccfe-41ea-a52c-00c3c7adb51a
author: evaseydl
manager: scottman
ms.author: evas
ms.openlocfilehash: 3bcdd2d3f7492b29ecf92c59714f1d93b910c9b5
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853424"
---
# <a name="multipoint--stations"></a>Stazioni MultiPoint
In un ambiente di sistema MultiPoint Services, le *stazioni* sono gli endpoint utente per la connessione al computer che esegue multipoint Services. Ogni stazione offre all'utente un'esperienza Windows 10 indipendente. Sono supportati i tipi di stazione seguenti:  
  
-   Direct-video-stazioni connesse  
  
-   USB-zero-client-stazioni connesse (inclusi i client USB-over-Ethernet zero)  
  
-   Stazioni connesse RDP-over-LAN (per computer client avanzati o thin client)  
  
È anche possibile monitorare e controllare i PC completi che hanno installato MultiPoint Connector usando il Dashboard MultiPoint. In Windows 10 MultiPoint Connector può essere abilitato tramite il pannello di controllo per le funzionalità di Windows. 

Multipoint Services supporta qualsiasi combinazione di questi tipi di stazioni, ma è consigliabile che una stazione sia una stazione connessa con video diretto, che può fungere da stazione primaria. Il motivo di questa raccomandazione è quello di poter prevedere scenari di supporto. Ad esempio, per interagire con il BIOS del sistema prima che MultiPoint Services sia in esecuzione.  
  
## <a name="primary-stations-and-standard-stations"></a>Stazioni primarie e stazioni standard  
Una stazione con connessione diretta a video è definita come *stazione primaria*. Le stazioni rimanenti sono denominate *stazioni standard*.  
  
La stazione primaria Visualizza le schermate di avvio quando il computer è acceso. Consente di accedere alla configurazione del sistema e alle informazioni per la risoluzione dei problemi disponibili solo durante l'avvio. La stazione primaria deve essere una stazione connessa a video diretto. Dopo l'avvio, è possibile usare la stazione primaria come qualsiasi altra stazione MultiPoint.  
  
## <a name="direct-video-connected-stations"></a>Direct-video-stazioni connesse  
Il computer che esegue MultiPoint Services può contenere più schede video, ciascuna delle quali può avere una o più porte video. In questo modo è possibile collegare i monitoraggi per più stazioni direttamente nel computer. Tastiere e topi sono connessi tramite hub USB associati a ogni monitoraggio. Questi hub sono detti hub di *stazione*. Altre periferiche, ad esempio altoparlanti, cuffie o dispositivi di archiviazione USB, possono anche essere connesse a un hub di stazione e sono disponibili solo per l'utente di tale stazione.  
  
> [!IMPORTANT]  
> È necessario che sia presente almeno una *stazione connessa a video diretto* per ogni server che funge da stazione primaria per visualizzare il processo di avvio quando il computer è acceso.  
  
![Immagine del layout del sistema basato su USB servizi MultiPoint](./media/WMSMultiPointServerUSBSystemLayout.gif)  
  
**Figura 1** Sistema MultiPoint Services con quattro stazioni connesse a video diretto  
  
### <a name="ps2-stations"></a><a name="BKMK_PS2stations"></a>Stazioni PS/2  
Con MultiPoint Services è possibile eseguire il mapping della tastiera e del mouse PS/2 sulla scheda madre a un monitor connesso video diretto per creare una stazione PS/2. Audio analogico ad alta definizione sulla scheda madre è l'audio associato a questo tipo di stazione. Questa operazione non si applica ai computer in cui non sono presenti Jack PS/2 sulla scheda madre.  
  
## <a name="usb-zero-client-connected-stations"></a>USB-zero-client-stazioni connesse  
Le stazioni USB-zero-client-Connected utilizzano un *USB zero client* come hub di stazione. I client USB zero sono talvolta definiti hub multifunzione con video. Si tratta di un hub che si connette al computer tramite un cavo USB e questi hub in genere supportano un monitor video, un mouse e una tastiera (PS/2 o USB), audio e dispositivi USB aggiuntivi. Questa guida si riferisce a questi hub specializzati come client USB zero.  
  
Il diagramma seguente mostra un sistema MultiPoint Server con una stazione primaria (Direct video Connected Station) e due stazioni di connessione USB zero client aggiuntive.  
  
![USB zero-stazioni client connesse](./media/WMS11_diagram7.gif)  
  
**Figura 2** Sistema MultiPoint Services con una stazione primaria e due stazioni USB con connessione a zero client  
  
### <a name="usb-over-ethernet-zero-clients"></a>Client USB-over-Ethernet zero  
I client USB-over-Ethernet zero sono una variante dei client USB zero che inviano USB tramite LAN al sistema MultiPoint Services. Questi tipi di client USB zero funzionano in modo analogo ad altri client USB zero, ma non sono limitati dalla lunghezza massima del cavo USB. I client USB-over-Ethernet zero non sono client thin tradizionali e vengono visualizzati come dispositivi USB virtuali nel sistema MultiPoint Services. Quando si usano questi dispositivi, fare riferimento al produttore del dispositivo per raccomandazioni specifiche relative a prestazioni e pianificazione del sito. La maggior parte dei dispositivi dispone di un plug-in di terze parti per Gestione MultiPoint che consente di associare e connettere i dispositivi al sistema MultiPoint Services.  
  
## <a name="rdp-over-lan-connected-stations"></a>Stazioni connesse RDP-over-LAN  
I thin client e i computer desktop, portatili o tablet tradizionali possono connettersi al computer che esegue MultiPoint Services tramite la rete locale (LAN) utilizzando Remote Desktop Protocol (RDP) o un protocollo proprietario e il provider di Remote Desktop Protocol. Le connessioni RDP forniscono un'esperienza utente finale molto simile a qualsiasi altra stazione MultiPoint, ma usa l'hardware del computer client locale. Scopri di più sulle nostre applicazioni desktop remoto disponibili per Android, iOS, Mac e Windows in [Desktop remoto client](../remote-desktop-services/clients/remote-desktop-clients.md). 
  
I client e i dispositivi che eseguono Microsoft RemoteFX possono fornire un'esperienza multimediale avanzata sfruttando le funzionalità hardware del processore e dei video del thin client o del computer locale per fornire video ad alta definizione sulla rete.  
  
Se si dispone di client LAN esistenti, MultiPoint Services può fornire un modo rapido e conveniente per aggiornare simultaneamente tutti gli utenti a un'esperienza di Windows 10.  
  
Dal punto di vista della distribuzione e dell'amministrazione, quando si usano le stazioni con connessione RDP-over-LAN sono presenti le differenze seguenti:  
  
-   Non limitato alle distanze di connessione USB fisiche  
  
-   Possibilità di riutilizzare l'hardware del computer meno recente come stazioni  
  
-   Semplifica la scalabilità a un numero maggiore di stazioni. Qualsiasi client della rete può essere potenzialmente utilizzato come stazione remota  
  
-   Risoluzione dei problemi hardware tramite la console di gestione MultiPoint  
  
-   Nessuna funzionalità a schermo diviso.  
  
    Per ulteriori informazioni, vedere [stazioni a schermo diviso](#split-screen-stations) più avanti in questo argomento.  
  
-   Nessuna stazione Rinomina o configurazione dell'accesso automatico tramite la console di gestione MultiPoint  
  
![Stazione con connessione USB zero-client](./media/Diagram1.gif)  
  
**Figura 3** Sistema di servizi MultiPoint con stazioni con connessione RDP-over-LAN  
  
## <a name="additional-configuration-options"></a>Opzioni di configurazione aggiuntive  
  
### <a name="split-screen-stations"></a>Workstation con schermo diviso  
MultiPoint Services offre un'opzione di schermata suddivisa nei computer con stazioni con connessione diretta a video o stazioni USB-zero-client-connected. Una schermata divisa consente di creare una stazione aggiuntiva per ogni monitoraggio. Anziché richiedere due monitoraggi, è possibile usare un monitoraggio con due configurazioni di hub di stazione per creare due stazioni con un monitor. È possibile aumentare rapidamente il numero di stazioni disponibili senza dover acquistare monitoraggi aggiuntivi, client USB-zero o schede video.  
  
I vantaggi derivanti dall'utilizzo di una stazione Split screen possono includere:  
  
-   Riduzione dei costi e dello spazio grazie alla necessità di ospitare un numero maggiore di utenti in un sistema MultiPoint Services.  
  
-   Consentire a due utenti di collaborare side-by-side in un progetto.  
  
-   Consentire a un insegnante di dimostrare una procedura in una stazione mentre uno studente segue l'altra stazione.  
  
Qualsiasi monitoraggio della stazione MultiPoint Services con una risoluzione 1024x768 o superiore può essere suddiviso in due schermate di stazione. Per la migliore esperienza utente della schermata divisa, è consigliabile usare una schermata a schermo intero con una risoluzione 1600x900 minima. È inoltre consigliabile utilizzare una mini tastiera senza un tastierino numerico per consentire l'adattamento delle due tastiere davanti al monitor.  
  
Per creare stazioni a schermo diviso, è necessario configurare una stazione diretta-video-connessa o USB-zero-client-connected. Quindi si aggiunge un hub di stazione aggiuntivo collegando una tastiera e un mouse a un hub USB connesso al server. È quindi possibile convertire la stazione in due stazioni usando Gestione MultiPoint per suddividere lo schermo ed eseguire il mapping del nuovo hub alla metà del monitor. La metà sinistra della schermata diventa una stazione e la metà destra diventa una seconda stazione.  
  
Dopo la suddivisione di una stazione, un utente può accedere alla stazione a sinistra mentre un altro utente accede alla stazione corretta.  
  
![Workstation con schermo diviso](./media/WMS_diagram3.gif)  
  
**Figura 4** Sistema MultiPoint Services con stazioni Split screen  
  
## <a name="station-type-comparison"></a><a name="BKMK_StationTypeComparison"></a>Confronto tra tipi di stazione  
  
||Video diretto connesso|USB zero client connesso|Connessione RDP-over-LAN|  
|-|--------------------------|-----------------------------|----------------------------|  
|Prestazioni video|Consigliato per ottimizzare le prestazioni video||USA thin client che supportano RemoteFX per migliorare la qualità dei video con una larghezza di banda di rete inferiore|  
|Limitazioni fisiche|Limitato dalla lunghezza del cavo video e dall'hub USB e dalla lunghezza del cavo (lunghezza massima consigliata di 15 contatori)|Limitato dall'hub USB e dalla lunghezza del cavo (lunghezza massima consigliata di 15 contatori)|Limitato dalla distribuzione LAN|  
|Numero di stazioni consentite |Limitato dal numero di slot PCIe disponibili sulla scheda madre per le porte video per scheda video|Il numero totale può essere limitato dal produttore del client USB zero (per altre informazioni, vedere la nota che segue questa tabella).|Limitato dalle porte disponibili sul Commuter di rete|  
|Schermata di suddivisione|Sì|Sì|No|  
|Stato periferica della stazione di gestione MultiPoint, configurazione dell'accesso automatico, ridenominazione delle stazioni|Sì|Sì|No|  
|Accesso ai menu di avvio del server|Sì|No|No|  
  
> [!NOTE]  
> Il numero totale di client USB zero connessi al server può essere limitato dal produttore o dalla capacità hardware del computer che esegue MultiPoint Services.
