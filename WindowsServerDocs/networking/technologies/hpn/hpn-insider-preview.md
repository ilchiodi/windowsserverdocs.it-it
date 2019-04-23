---
ms.assetid: ''
title: Insider Preview per le funzionalità HPN in Windows Server 2019
description: Informazioni sulle nuove funzionalità di rete ad alte prestazioni in Windows Server 2019.
manager: dougkim
author: shortpatti
ms.author: pashort
ms.date: 09/12/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: 3d3e974472f28c30d093fbd1094ef3693d984d19
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59886272"
---
# <a name="insider-preview"></a>Insider Preview


## <a name="dynamic-vrss-and-vmmq"></a>VRSS dinamico e VMMQ

>Si applica a: Windows Server 2019

In passato, code di macchine virtuali e macchine virtuali multi-abilitato velocità effettiva notevolmente superiore a singole macchine virtuali come rete effettive raggiunge prima di tutto il contrassegno di 10 GbE e altro ancora. Sfortunatamente, la pianificazione, base, ottimizzazione e monitoraggio necessaria per operazioni riuscite è diventato una grande impresa. spesso più l'amministratore IT può dedicare. 

Windows Server 2019 migliora queste ottimizzazioni di distribuzione e l'elaborazione di carichi di lavoro di rete in base alle esigenze di ottimizzazione in modo dinamico. Windows Server 2019 garantisce la massima efficienza e rimuove il carico di lavoro di configurazione per gli amministratori IT.

Per altre informazioni, vedi:

-   [Blog sull'annuncio](https://blogs.technet.microsoft.com/networking/2018/08/22/netperf4vw/)

-   [Guida per la convalida per IT Pro](https://aka.ms/DVMMQ-Validation)

## <a name="receive-segment-coalescing-rsc-in-the-vswitch"></a>Unione segmenti ricevuti (RSC) nel commutatore virtuale

>Si applica a: 2019 Server Windows e Windows 10, versione 1809

Ricezione segmento Coalescing (RSC) nel commutatore virtuale è un miglioramento che riunisce più segmenti TCP in segmenti più grandi prima che i dati che attraversa il commutatore virtuale. Il segmento di grandi dimensioni consente di migliorare le prestazioni di rete per i carichi di lavoro virtuali.

In precedenza, questo era un offload implementato dall'interfaccia di rete. Sfortunatamente, ciò è stato disabilitato nel momento in cui che l'adapter sono stati collegati a un commutatore virtuale. RSC nel commutatore virtuale in Windows Server 2019 e Windows 10 ottobre 2018 Update rimuove questa limitazione.

Per impostazione predefinita, RSC nel commutatore virtuale è abilitata nei commutatori virtuali esterni.

Per altre informazioni, vedi:

-  [Blog sull'annuncio](https://blogs.technet.microsoft.com/networking/2018/08/22/netperf4vw/)

-  [Guida per la convalida per IT Pro](https://aka.ms/RSC-Validation)
