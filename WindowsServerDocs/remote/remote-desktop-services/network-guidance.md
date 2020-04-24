---
title: Linee guida relative alla rete
description: Consigli sulla larghezza di banda per le distribuzioni Desktop remoto.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: helohr
ms.date: 12/12/2019
ms.topic: article
author: Heidilohr
manager: lizross
ms.openlocfilehash: 79db56d467ae0913446faebffc5a9598aae0b767
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "80852994"
---
# <a name="network-guidance"></a>Linee guida relative alla rete

Quando usi una sessione remota di Windows, la larghezza di banda disponibile della rete influisce significativamente sulla qualità dell'esperienza utente. Diverse applicazioni e risoluzioni dello schermo richiedono configurazioni di rete differenti ed è quindi importante assicurarsi che la rete sia configurata in maniera conforme alle esigenze.

>[!NOTE]
>I consigli seguenti si applicano alle reti con una perdita inferiore allo 0,1%. Questi consigli si applicano indipendentemente dal numero di sessioni ospitate nelle macchine virtuali (VM).

## <a name="applications"></a>Applicazioni

La tabella seguente elenca le larghezze di banda minime consigliate per un'esperienza utente uniforme. Questi consigli si basano sulle linee guida riportate in [Carichi di lavoro di Desktop remoto](remote-desktop-workloads.md).

| Tipo di carico di lavoro   | Larghezza di banda consigliata |
|-----------------|-----------------------|
| Chiaro           | 1,5 Mbps              |
| Medio          | 3 Mbps                |
| Pesante           | 5 Mbps                |
| Potenza           | 15 Mbps               |

Tieni presente che lo stress sulla rete dipende sia dalla frequenza dei fotogrammi di output del carico di lavoro dell'app che dalla risoluzione dello schermo. Se aumenta la frequenza dei fotogrammi o la risoluzione dello schermo, aumenterà anche la larghezza di banda richiesta. Un carico di lavoro leggero con schermo ad alta risoluzione, ad esempio, richiede una disponibilità della larghezza di banda maggiore rispetto a un carico di lavoro leggero con risoluzione normale o bassa.

Altri scenari possono modificare i requisiti della larghezza di banda in base alla modalità d'uso, ad esempio:

- Conferenze vocali o videoconferenze
- Comunicazione in tempo reale
- Streaming video 4K

Assicurati di eseguire il test di carico di questi scenari nella distribuzione usando strumenti di simulazione, ad esempio Login VSI. Per comprendere meglio i requisiti della rete, modifica le dimensioni del carico, esegui test di stress e testa gli scenari utente comuni nelle sessioni remote.

## <a name="display-resolutions"></a>Risoluzioni dello schermo

Diverse risoluzioni dello schermo richiedono una disponibilità di larghezze di banda differenti. La tabella seguente elenca le larghezze di banda consigliate per un'esperienza utente uniforme con risoluzioni dello schermo tipiche, con una frequenza di fotogrammi pari a 30 fotogrammi al secondo (fps). Questi consigli si applicano a scenari utente singoli e multipli. Tieni presente che gli scenari con una frequenza di fotogrammi inferiore a 30 fps, ad esempio la lettura di testo statico, richiedono una disponibilità di larghezza di banda inferiore.

| Risoluzioni dello schermo tipiche a 30 fps    | Larghezza di banda consigliata |
|------------------------------------------|-----------------------|
| Circa 1024×768 px                      | 1,5 Mbps              |
| Circa 1280×720 px                      | 3 Mbps                |
| Circa 1920×1080 px                     | 5 Mbps                |
| Circa 3840×2160 px (4K)                | 15 Mbps               |

## <a name="additional-resources"></a>Risorse aggiuntive

L'area di Azure in cui ti trovi può influire sull'esperienza utente tanto quanto le condizioni della rete. Per altre informazioni, vedi [Strumento di valutazione dell'esperienza di Desktop virtuale Windows](https://azure.microsoft.com/services/virtual-desktop/assessment/).
