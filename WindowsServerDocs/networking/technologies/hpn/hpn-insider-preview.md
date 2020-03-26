---
ms.assetid: ''
title: Funzionalità di anteprima di insider per HPN in Windows Server 2019
description: Informazioni sulle nuove funzionalità di rete ad alte prestazioni di Windows Server 2019.
manager: dougkim
author: shortpatti
ms.author: pashort
ms.date: 09/12/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: bca603344047ea5cc890bf9976ca5a6c79514136
ms.sourcegitcommit: 9feb093a0acb8834c9ef3c066667c7062d85e6e1
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80289796"
---
# <a name="new-hpn-features-in-windows-server-2019"></a>Nuove funzionalità di HPN in Windows Server 2019


## <a name="dynamic-vrss-and-vmmq"></a>VRSS e VMMQ dinamici

>Si applica a: Windows Server 2019

In passato, le code di macchine virtuali e le code multicoda di macchine virtuali consentivano una velocità effettiva molto più elevata per le singole macchine virtuali, mentre le velocità effettiva di rete raggiungono prima il contrassegno 10GbE e oltre. Sfortunatamente, la pianificazione, la baseline, l'ottimizzazione e il monitoraggio necessari per il successo sono diventati una grande impresa; spesso più che l'amministratore IT intendeva spendere. 

Windows Server 2019 migliora queste ottimizzazioni distribuendo e ottimizzando dinamicamente l'elaborazione dei carichi di lavoro di rete in base alle esigenze. Windows Server 2019 garantisce una massima efficienza e rimuove il carico di configurazione per gli amministratori IT.

Per altre informazioni, vedi:

-   [Blog di annuncio](https://blogs.technet.microsoft.com/networking/2018/08/22/netperf4vw/)

-   [Guida alla convalida per i professionisti IT](https://aka.ms/DVMMQ-Validation)

## <a name="receive-segment-coalescing-rsc-in-the-vswitch"></a>Unione segmenti ricevuti (RSC) nel commutatore virtuale

>Si applica a: Windows Server 2019 e Windows 10, versione 1809

La funzionalità di Unione del segmento Receive (RSC) in vSwitch è un miglioramento che unisce più segmenti TCP in un segmento più grande prima che i dati attraversino il vSwitch. Il segmento grande migliora le prestazioni di rete per i carichi di lavoro virtuali.

In precedenza, si trattava di un offload implementato dalla scheda di interfaccia di rete. Sfortunatamente, questa operazione è stata disabilitata nel momento in cui l'adapter è stato collegato a un commutire virtuale. RSC nell'aggiornamento di vSwitch in Windows Server 2019 e Windows 10 ottobre 2018 rimuove questa limitazione.

Per impostazione predefinita, RSC in vSwitch è abilitato in commutatori virtuali esterni.

Per altre informazioni, vedi:

-  [Blog di annuncio](https://blogs.technet.microsoft.com/networking/2018/08/22/netperf4vw/)

-  [Guida alla convalida per i professionisti IT](https://aka.ms/RSC-Validation)
