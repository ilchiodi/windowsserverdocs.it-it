---
ms.assetid: ''
title: Anteprima Insider per le funzionalità del Servizio Ora di Windows in Windows Server 2019
description: Nuove funzionalità del Servizio Ora di Windows in Windows Server 2019
author: shortpatti
ms.author: pashort
ms.date: 09/05/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 38682afa37a4c6882ee2e63a4abf4cd9fdbd2b27
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405221"
---
# <a name="insider-preview"></a>Anteprima Insider 


## <a name="leap-second-support"></a>Supporto del secondo intercalare


>Si applica a: Windows Server 2019 e Windows 10, versione 1809

Il secondo intercalare è una correzione occasionale di 1 secondo all'ora UTC. Con il rallentamento della rotazione della terra, il tempo [UTC](https://en.wikipedia.org/wiki/Coordinated_Universal_Time) (scala di tempo atomica) si discosta dal [tempo solare medio](https://en.wikipedia.org/wiki/Solar_time#Mean_solar_time) o dal tempo astronomico.  Quando questa differenza raggiunge 0,9 secondi, viene inserito un [secondo intercalare](https://en.wikipedia.org/wiki/Leap_second) per mantenere la sincronizzazione tra il tempo UTC e l'ora solare media.

I secondi intercalari sono diventati importanti per soddisfare i requisiti normativi di accuratezza e tracciabilità di Stati Uniti e Unione europea.

Per altre informazioni, vedere:

-  Il [blog dell'annuncio](https://blogs.technet.microsoft.com/networking/2018/07/18/top10-ws2019-hatime/)

-  Guida alla convalida per gli [sviluppatori](https://aka.ms/Dev-LeapSecond)

-  Guida alla convalida per i [professionisti IT](https://aka.ms/ITPro-LeapSecond)


## <a name="precision-time-protocol"></a>Precision Time Protocol

>Si applica a: Windows Server 2019 e Windows 10, versione 1809

Un nuovo provider di servizi temporali incluso in Windows Server 2019 e Windows 10 (versione 1809) consente di sincronizzare l'ora con il protocollo PTP (Precision Time Protocol). Quando l'orario viene distribuito in rete, subisce un ritardo (latenza) che, se non viene considerato o non è simmetrico, diventa sempre più difficile capire il timestamp inviato dal server di tempo. Il protocollo PTP consente ai dispositivi di rete di aggiungere la latenza introdotta da ogni dispositivo di rete nelle misurazioni temporali, fornendo così al client Windows un campione di tempo molto più accurato.

Per altre informazioni, vedere:

-  Il [blog dell'annuncio](https://blogs.technet.microsoft.com/networking/2018/07/18/top10-ws2019-hatime/)

-  Guida alla convalida per i [professionisti IT](https://aka.ms/PTPValidation)


## <a name="software-timestamping"></a>Timestamp del software

>Si applica a: Windows Server 2019 e Windows 10, versione 1809

Quando si riceve un pacchetto temporizzato in rete da un server di riferimento dell'ora, deve essere elaborato dallo stack di rete del sistema operativo prima di essere poter usato nel servizio Ora. Ogni componente dello stack di rete introduce una quantità variabile di latenza che incide sull'accuratezza della misurazione temporale.

![timestamp del software](../media/Windows-Time-Service/software-timestamping.png)

Per risolvere questo problema, il timestamp del software consente di eseguire il timestamp dei pacchetti prima e dopo i "componenti di rete di Windows" indicati in precedenza per poter tenere conto del ritardo nel sistema operativo.

Per altre informazioni, vedere:

-  Il [blog dell'annuncio](https://blogs.technet.microsoft.com/networking/2018/07/18/top10-ws2019-hatime/)

-  Guida alla convalida per i [professionisti IT](https://github.com/Microsoft/SDN/blob/master/FeatureGuide/Validation%20Guide%20-%20RS5%20-%20Software%20Timestamping.docx)



---