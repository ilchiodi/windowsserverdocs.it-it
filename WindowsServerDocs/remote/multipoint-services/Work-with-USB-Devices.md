---
title: Utilizzare dispositivi USB
description: Informazioni sul funzionano di dispositivi USB con MultiPoint Services
ms.custom: na
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a33f2b83-bbc2-4fc1-8a94-aaa985dfe1f9
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: f961f9270183b17855151f11ce222e3a5cc8df7d
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883772"
---
# <a name="work-with-usb-devices"></a>Utilizzare dispositivi USB
È possibile connettere dispositivi al computer nel sistema MultiPoint Services o a un hub di stazione MultiPoint. Il tipo del dispositivo e la posizione in cui è connesso determina la disponibilità del dispositivo stesso per tutti gli utenti collegati al sistema, soltanto per singoli utenti oppure per nessun utente. Di seguito sono riportati esempi di diversi tipi di connessione:  
  
-   Se si collega un dispositivo, ad esempio una stampante o un dispositivo di archiviazione di massa USB, direttamente al computer, a tale dispositivo potranno accedere tutti gli utenti delle sessioni attive sul sistema MultiPoint Services. Gli utenti delle stazioni desktop virtuali non saranno in grado di accedere ai dispositivi connessi direttamente al computer.  
  
-   Se si collega un dispositivo, ad esempio una tastiera, un mouse, un *dispositivo audio* o un dispositivo di archiviazione di massa, a un hub di stazione, tale dispositivo sarà disponibile solo per l'utente che ha effettuato l'accesso alla stazione MultiPoint Services.  
  
-   Se al computer si collegano determinati tipi di dispositivi, ad esempio una tastiera o un mouse, tali dispositivi non saranno disponibili per alcun utente del sistema.  
  
La tabella seguente riporta un elenco di dispositivi e illustra il relativo comportamento a seconda della modalità di connessione al sistema. Informazioni sull'hub di stazione sono descritte nella [utilizzo degli hub di stazione](#working-with-station-hubs). Altre informazioni su come collegare monitor video a una stazione sono descritto nella [utilizzare dispositivi Video](Work-with-Video-Devices.md).  
  
|||||  
|-|-|-|-|  
|**Dispositivo**|**Comportamento quando è connesso direttamente al computer**|**Comportamento quando è connesso a una stazione**|**Note**|  
|Tastiera|Non è consigliabile collegare una tastiera direttamente al computer.|Accessibile solo all'utente della stazione.|Se nella tastiera è presente una porta USB, l'hub USB all'interno della tastiera può fungere da hub di stazione. Altri dispositivi USB collegati a tale porta saranno disponibili solo per l'utente che usa la tastiera.<br /><br />Alcuni hub di stazione sono dotati di una porta per mouse PS\/2 convertita in connessione USB all'interno dell'hub.|  
|Mouse|Non è consigliabile collegare un mouse direttamente al computer.|Accessibile solo all'utente della stazione.|Alcuni hub di stazione sono dotati di una porta per mouse PS\/2 convertita in connessione USB all'interno dell'hub.|  
|Hub USB|Visualizzare [utilizzo degli hub di stazione](#working-with-station-hubs).|Visualizzare [utilizzo degli hub di stazione](#working-with-station-hubs).||  
|Monitor video|Visualizzare [dispositivi Video MultiPoint Services](work-with-video-devices.md).|Visualizzare [dispositivi Video MultiPoint Services](work-with-video-devices.md).||  
|Dispositivi di output audio (ad esempio cuffie)|Non è consigliabile collegare un dispositivo di output audio direttamente al computer.|Accessibile solo all'utente della stazione.|Alcuni hub di stazione sono dotati di una porta audio analogica convertita in connessione audio USB all'interno dell'hub.|  
|Dispositivi di input audio (ad esempio microfoni)|Non è consigliabile collegare un dispositivo di input audio direttamente al computer.|Accessibile solo all'utente della stazione.|Alcuni hub di stazione sono dotati di una porta audio analogica convertita in connessione audio USB all'interno dell'hub.|  
|Stampanti|Accessibile a tutti gli utenti nel sistema *|Accessibile solo all'utente della stazione.||  
|Dispositivo di archiviazione di massa USB|Accessibile a tutti gli utenti nel sistema.\*|Accessibile solo all'utente della stazione.|Questi dispositivi includono unità flash USB, unità disco rigido esterne e fotocamere digitali.|  
|Webcam|Accessibile a tutti gli utenti nel sistema *|Accessibile solo all'utente della stazione.|Alla fotocamera si può collegare un solo utente alla volta.|  
  
* I dispositivi connessi al computer host non sono visibili agli utenti che sono connessi a stazioni di desktop virtuali.  
  
Per altre informazioni su come configurare una stazione, vedere [Configurazione di una stazione](Set-Up-a-Station.md).  
  
### <a name="working-with-station-hubs"></a>Utilizzo degli hub di stazione  
Esistono quattro scenari di modalità di utilizzo di un hub USB connesso a un sistema MultiPoint Services. Ogni scenario offre un accesso differente ai dispositivi connessi a seconda del tipo di hub e della posizione in cui questo è collegato al sistema.  
  
-   Un hub di stazione connesso al computer sul sistema MultiPoint Services con una tastiera collegata può essere usato per creare una stazione MultiPoint Services. Una tastiera e un mouse sono collegati all'hub di stazione tramite le porte disponibili sull'hub. Un monitor video è collegato a una porta video sul computer o a una scheda video sull'hub di stazione, se disponibile. La tastiera, il mouse e il monitor vengono quindi *associati* a una stazione MultiPoint Services.  
  
-   Un hub USB connesso al computer nel sistema MultiPoint Services senza alcuna tastiera collegata può essere usato per connettere altri dispositivi al computer quando quest'ultimo non dispone di porte sufficienti per i dispositivi richiesti. Tutti i dispositivi collegati a questo hub USB sono disponibili per tutti gli utenti del sistema MultiPoint Services. Questa non è considerata una stazione MultiPoint Services.  
  
-   Un hub USB acceso connesso al computer nel sistema MultiPoint Services, detto anche hub intermedio, può essere utilizzato per connettere altri hub USB usati per creare stazioni MultiPoint.  
  
-   Un hub USB connesso a un hub di stazione può essere usato per connettere altri dispositivi all'hub di stazione. Le tastiere devono essere connesse direttamente all'hub di stazione.  
  
Per altre informazioni su come configurare una stazione MultiPoint Services, vedere [Configurazione di una stazione](Set-Up-a-Station.md).  
  
## <a name="see-also"></a>Vedere anche  
[Utilizzare dispositivi Video](Work-with-Video-Devices.md)  
[Gestire l'Hardware delle stazioni](Manage-Station-Hardware.md)  
[Configurare una stazione](Set-Up-a-Station.md)