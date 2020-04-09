---
title: Gestire l'hardware delle stazioni
description: Viene fornita una panoramica su come gestire l'hardware per le stazioni MultiPoint
ms.prod: windows-server
ms.technology: multipoint-services
ms.topic: article
ms.assetid: 429b8539-b17a-4e01-9576-860600466451
author: lizap
manager: dongill
ms.author: elizapo
ms.date: 08/04/2016
ms.openlocfilehash: 8ffb6fd714293471a0e9aa020390943b201261c4
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80853524"
---
# <a name="manage-station-hardware"></a>Gestire l'hardware delle stazioni
Un sistema MultiPoint Services è costituito da un singolo computer e almeno una stazione. L'hardware della stazione è in genere costituito da un hub di stazione, un mouse, una tastiera e un monitor. Di norma le stazioni sono collegate fisicamente al computer.  
  
Le illustrazioni seguenti mostrano un layout di esempio di un sistema MultiPoint Services con quattro stazioni. Ogni stazione è connessa al computer Servizi MultiPoint usando un hub USB e schede video a più monitor. Questa illustrazione non rappresenta stazioni collegate mediante hub multifunzione.  
   
![Immagine del layout del sistema basato su USB servizi MultiPoint](./media/WMSMultiPointServerUSBSystemLayout.gif)  
  
Gli argomenti di questa sezione descrivono come visualizzare lo stato dell'hardware collegato al sistema MultiPoint Services e forniscono informazioni dettagliate sui tipi di dispositivi USB e su altre periferiche hardware che è possibile usare per configurare una stazione MultiPoint Services. Di seguito è riportata una breve descrizione degli argomenti in questa sezione che possono semplificare la selezione dell'hardware e la configurazione della stazione MultiPoint Services.  
  
## <a name="view-hardware-status"></a>Visualizzare lo stato dell'hardware  
In Gestione MultiPoint è possibile usare la scheda **stations (stazioni** ) per visualizzare lo stato delle stazioni e dei dispositivi hardware connessi al computer Servizi multipoint. Per altre informazioni su come visualizzare lo stato del computer e dei dispositivi hardware MultiPoint Services connessi, vedere l'argomento [Visualizzazione dello stato dell'hardware](View-Hardware-Status.md).  
  
## <a name="work-with-usb-devices"></a>Utilizzare dispositivi USB  
Quando al sistema MultiPoint Services sono collegati dispositivi USB e altre dispositivi hardware, questi funzionano diversamente a seconda che siano collegati al computer MultiPoint Services o a una stazione MultiPoint Services. L'argomento [Utilizzare dispositivi USB](Work-with-USB-Devices.md) descrive come i diversi dispositivi hardware possono funzionare in questi scenari e fornisce informazioni dettagliate sul funzionamento degli hub di stazione.  
  
## <a name="work-with-video-devices"></a>Utilizzare dispositivi video  
L'argomento [Utilizzo di dispositivi video](Work-with-Video-Devices.md) descrive il funzionamento di dispositivi video, ad esempio un monitor o un proiettore, collegati a un computer nel sistema MultiPoint Services o a una stazione MultiPoint Services.  
  
## <a name="set-up-a-station"></a>Configurare una stazione  
L'argomento [Configurazione di una stazione](Set-Up-a-Station.md) descrive come connettere i dispositivi hardware a un hub di stazione MultiPoint Services per creare una stazione MultiPoint Services. MultiPoint Services supporta due tipi di hub di stazione:  
  
-   *Hub USB* a più porte alimentato esternamente o bus in grado di supportare un mouse, una tastiera e altre periferiche USB  
  
-   Un *hub multifunzione* che può includere un controller video integrato e porte per mouse, tastiera e periferiche audio  
  
Entrambi i tipi di hub di stazione sono collegati al computer tramite un cavo USB. Le procedure riportate nell'argomento [Configurazione di una stazione](Set-Up-a-Station.md) descrivono come connettere i dispositivi hardware a ciascun tipo di hub di stazione.  
  
## <a name="see-also"></a>Vedi anche  
[Visualizzare lo stato dell'hardware](View-Hardware-Status.md)  
[Utilizzare dispositivi USB](Work-with-USB-Devices.md)  
[Utilizzare dispositivi video](Work-with-Video-Devices.md)  
[Configurare una stazione](Set-Up-a-Station.md)