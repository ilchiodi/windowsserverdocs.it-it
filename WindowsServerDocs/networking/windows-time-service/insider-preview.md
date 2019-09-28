---
ms.assetid: ''
title: Anteprima di insider per le funzionalità del servizio ora di Windows in Windows Server 2019
description: Nuove funzionalità del servizio ora di Windows in Windows Server 2019
author: shortpatti
ms.author: pashort
ms.date: 09/05/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 38682afa37a4c6882ee2e63a4abf4cd9fdbd2b27
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405221"
---
# <a name="insider-preview"></a>Anteprima di Insider 


## <a name="leap-second-support"></a>Supporto Leap Second


>Si applica a: Windows Server 2019 e Windows 10, versione 1809

Un secondo intercalare è una modifica occasionale di 1 secondo all'ora UTC. Con il rallentamento della rotazione della terra, l'ora [UTC](https://en.wikipedia.org/wiki/Coordinated_Universal_Time) (una scala cronologica atomica) si differenzia dall'ora [solare media](https://en.wikipedia.org/wiki/Solar_time#Mean_solar_time) o dal tempo astronomico.  Una volta che l'ora UTC è stata rilasciata per un massimo di,9 secondi, viene inserito un [secondo intercalare](https://en.wikipedia.org/wiki/Leap_second) per la sincronizzazione UTC con l'ora solare media.

I secondi intercalari sono diventati importanti per soddisfare i requisiti normativi di accuratezza e tracciabilità sia nell'Stati Uniti che nell'Unione europea.

Per altre informazioni, vedere:

-  Il nostro [Blog di annuncio](https://blogs.technet.microsoft.com/networking/2018/07/18/top10-ws2019-hatime/)

-  Guida alla convalida per gli [sviluppatori](https://aka.ms/Dev-LeapSecond)

-  Guida alla convalida per i [professionisti it](https://aka.ms/ITPro-LeapSecond)


## <a name="precision-time-protocol"></a>Protocollo ora di precisione

>Si applica a: Windows Server 2019 e Windows 10, versione 1809

Un nuovo provider di servizi temporali incluso in Windows Server 2019 e Windows 10 (versione 1809) consente di sincronizzare l'ora con il protocollo PTP (Precision Time Protocol). Quando il tempo si distribuisce in una rete, rileva un ritardo (latenza), che, se non è considerato, o se non è simmetrico, diventa sempre più difficile comprendere il timestamp inviato dal server di tempo. PTP consente ai dispositivi di rete di aggiungere la latenza introdotta da ogni dispositivo di rete nelle misurazioni temporali, fornendo un campione temporale molto più accurato per il client Windows.

Per altre informazioni, vedere:

-  Il nostro [Blog di annuncio](https://blogs.technet.microsoft.com/networking/2018/07/18/top10-ws2019-hatime/)

-  Guida alla convalida per i [professionisti it](https://aka.ms/PTPValidation)


## <a name="software-timestamping"></a>Timestamp del software

>Si applica a: Windows Server 2019 e Windows 10, versione 1809

Quando si riceve un pacchetto temporizzato in rete da un server di ora, deve essere elaborato dallo stack di rete del sistema operativo prima di essere utilizzato nel servizio ora. Ogni componente nello stack di rete introduce una quantità variabile di latenza che influiscono sull'accuratezza della misurazione temporizzata.

![Timestamp del software](../media/Windows-Time-Service/software-timestamping.png)

Per risolvere questo problema, il timestamp software consente di timestamp i pacchetti prima e dopo i "componenti di rete di Windows" indicati in precedenza per tenere conto del ritardo nel sistema operativo.

Per altre informazioni, vedere:

-  Il nostro [Blog di annuncio](https://blogs.technet.microsoft.com/networking/2018/07/18/top10-ws2019-hatime/)

-  Guida alla convalida per i [professionisti it](https://github.com/Microsoft/SDN/blob/master/FeatureGuide/Validation%20Guide%20-%20RS5%20-%20Software%20Timestamping.docx)



---