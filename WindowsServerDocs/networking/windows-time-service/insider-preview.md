---
ms.assetid: ''
title: Anteprima di Insider per le funzionalità del servizio ora di Windows in Windows Server 2019
description: Nuove funzionalità del servizio ora di Windows in Windows Server 2019
author: shortpatti
ms.author: pashort
ms.date: 09/05/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: ef0ff317f5957add5ecbe9f88ef83753b805ec41
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829022"
---
# <a name="insider-preview"></a>Anteprima di Insider 


## <a name="leap-second-support"></a>Supporto di secondo intercalare


>Si applica a: 2019 Server Windows e Windows 10, versione 1809

Un secondo intercalare è una regolazione UTC occasionale 1 secondo. Poiché rallenta la rotazione della terra, [UTC](https://en.wikipedia.org/wiki/Coordinated_Universal_Time) (una scala cronologica atomica) si differenzia dal [significa che ora solare](https://en.wikipedia.org/wiki/Solar_time#Mean_solar_time) o astronomico ora.  Una volta UTC è diversa al massimo.9 secondi, un [secondo intercalare](https://en.wikipedia.org/wiki/Leap_second) viene inserito per sincronizzare UTC con ora solare medio.

I secondi intercalari divenuti essenziali per garantire l'accuratezza e requisiti normativi tracciabilità degli Stati Uniti e Unione europea.

Per altre informazioni, vedi:

-  Nostro [blog sull'annuncio](https://blogs.technet.microsoft.com/networking/2018/07/18/top10-ws2019-hatime/)

-  Guida di convalida per il [gli sviluppatori](https://aka.ms/Dev-LeapSecond)

-  Guida di convalida per il [IT Pro](https://aka.ms/ITPro-LeapSecond)


## <a name="precision-time-protocol"></a>Protocollo di tempo precisione

>Si applica a: 2019 Server Windows e Windows 10, versione 1809

Un nuovo provider servizi orari incluso in Windows Server 2019 e Windows 10 (versione 1809) consente di sincronizzare l'ora con la precisione ora PTP (Protocol). Poiché ora consente di distribuire attraverso una rete, rileva ritardo (latenza), che se non presi in considerazione, o se non è simmetrica, diventa sempre più difficile da comprendere il timestamp inviato dal server di tempo. PTP consente ai dispositivi di rete aggiungere la latenza introdotta da ogni dispositivo di rete nelle misure di intervallo offrendo così un campione ora più accurato per il client windows.

Per altre informazioni, vedi:

-  Nostro [blog sull'annuncio](https://blogs.technet.microsoft.com/networking/2018/07/18/top10-ws2019-hatime/)

-  Guida di convalida per il [IT Pro](https://aka.ms/PTPValidation)


## <a name="software-timestamping"></a>Aggiunta di timestamp software

>Si applica a: 2019 Server Windows e Windows 10, versione 1809

Quando si riceve un pacchetto di temporizzazione attraverso la rete da un server, devono essere elaborato dallo stack di rete del sistema operativo prima utilizzato nel servizio ora di. Ogni componente nello stack di rete presenta una quantità variabile di latenza che influisce sull'accuratezza di misurazione di temporizzazione.

![aggiunta di timestamp software](../media/Windows-Time-Service/software-timestamping.png)

Per risolvere questo problema, timestamp software consente ai pacchetti di timestamp prima e dopo la "Windows Networking componenti" illustrato in precedenza per tenere conto del ritardo nel sistema operativo.

Per altre informazioni, vedi:

-  Nostro [blog sull'annuncio](https://blogs.technet.microsoft.com/networking/2018/07/18/top10-ws2019-hatime/)

-  Guida di convalida per il [IT Pro](https://github.com/Microsoft/SDN/blob/master/FeatureGuide/Validation%20Guide%20-%20RS5%20-%20Software%20Timestamping.docx)



---