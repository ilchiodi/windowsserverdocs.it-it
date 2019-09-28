---
ms.assetid: ''
title: Limiti di supporto per ora ad alta precisione
description: Questo articolo descrive il limite di supporto per il servizio ora di Windows (W32Time) in ambienti che richiedono un'ora di sistema altamente accurata e stabile.
author: shortpatti
ms.author: dacuo
manager: dougkim
ms.date: 10/17/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 212b9c79bc2e43e966180b928c865a9053332c3f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405263"
---
# <a name="support-boundary-for-high-accuracy-time"></a>Limiti di supporto per ora ad alta precisione

>Si applica a: Windows Server 2016 e Windows 10 versione 1607 o successiva

Questo articolo descrive i limiti di supporto per il servizio ora di Windows (W32Time) in ambienti che richiedono un'ora di sistema altamente accurata e stabile.

## <a name="high-accuracy-support-for-windows-81-and-2012-r2-or-prior"></a>Supporto per la precisione elevata per Windows 8.1 e 2012 R2 (o versioni precedenti)

Le versioni precedenti di Windows (precedenti a Windows 10 1607 o Windows Server 2016 1607) non possono garantire un tempo molto accurato. Il servizio ora di Windows su questi sistemi:

-   È stata fornita la precisione temporale necessaria per soddisfare i requisiti di autenticazione Kerberos versione 5

-   Fornito tempo molto accurato per i client e i server Windows aggiunti a una foresta Active Directory comune

I requisiti di accuratezza più rigorosi esulano dalla specifica di progettazione del servizio ora di Windows su questi sistemi operativi e non sono supportati.

## <a name="windows-10-and-windows-server-2016"></a>Windows 10 e Windows Server 2016

L'accuratezza dell'ora in Windows 10 e Windows Server 2016 è stata notevolmente migliorata, mantenendo al tempo stesso la compatibilità completa con le versioni precedenti di Windows. Secondo le condizioni operative appropriate, i sistemi che eseguono Windows 10 o Windows Server 2016 e versioni successive possono fornire un secondo, 50 ms (millisecondi) o accuratezza 1 ms.

>[!IMPORTANT]
>**Origini ora molto accurate**<br>
>L'accuratezza temporale risultante nella topologia dipende molto dall'uso di un'origine dell'ora radice stabile (strato 1) accurata. Sono presenti hardware basato su Windows e non Windows altamente accurato, compatibile con Windows e ora NTP di origine, venduto da fornitori di terze parti. Rivolgersi al fornitore per verificare l'accuratezza dei propri prodotti.

>[!IMPORTANT]
>**Accuratezza dell'ora**<br>
>L'accuratezza dell'ora comporta la distribuzione end-to-end dell'ora esatta da un'origine ora autorevole molto accurata al dispositivo finale. Qualsiasi elemento che introduce l'asimmetria di rete influirà negativamente sull'accuratezza, ad esempio i dispositivi di rete fisica o un carico elevato della CPU nel sistema di destinazione.

## <a name="high-accuracy-requirements"></a>Requisiti di accuratezza elevata

Nella parte restante di questo documento vengono illustrati i requisiti ambientali che devono essere soddisfatti per supportare i rispettivi obiettivi di accuratezza elevata.

### <a name="target-accuracy-1-second-1s"></a>Accuratezza della destinazione: 1 secondo (1s)

Per ottenere l'accuratezza 1S per un computer di destinazione specifico rispetto a un'origine ora estremamente accurata:

-   Il sistema di destinazione deve eseguire Windows 10, Windows Server 2016.

-   Il sistema di destinazione deve sincronizzare l'ora da una gerarchia NTP di server temporali, che culminano con un'origine dell'ora NTP compatibile con Windows molto accurata.

-   Tutti i sistemi operativi Windows nella gerarchia NTP menzionata in precedenza devono essere configurati come documentati nella documentazione relativa alla [configurazione dei sistemi per la precisione elevata](configuring-systems-for-high-accuracy.md) .

-   La latenza di rete unidirezionale cumulativa tra la destinazione e l'origine non deve superare 100 ms. Il ritardo della rete cumulativa viene misurato aggiungendo i singoli ritardi unidirezionali tra coppie di nodi client-server NTP nella gerarchia, iniziando dalla destinazione e terminando nell'origine. Per altre informazioni, vedere il documento relativo alla sincronizzazione dell'ora di accuratezza elevata.

### <a name="target-accuracy-50-milliseconds"></a>Accuratezza della destinazione: 50 millisecondi

Tutti i requisiti descritti nella sezione **Target Precision: 1 secondo @ no__t-0 si applica, tranne nel caso in cui i controlli più severi sono descritti in questa sezione.

I requisiti aggiuntivi per ottenere l'accuratezza 50 ms per un sistema di destinazione specifico sono:

-   Il computer di destinazione deve avere una maggiore di 5 ms di latenza di rete tra l'origine dell'ora.

-   Il sistema di destinazione non deve essere maggiore di quello di strato 5 da un'origine ora molto accurata

    >[!Note]
    >Eseguire "w32tm/query/status" dalla riga di comando per visualizzare lo strato.

-   Il sistema di destinazione deve essere compreso tra 6 o meno hop di rete dall'origine dell'ora a precisione elevata

-   L'utilizzo medio della CPU di un giorno su tutti i Strat non deve superare il 90%

-   Per i sistemi virtualizzati, l'utilizzo medio della CPU dell'host di un giorno non deve superare il 90%

### <a name="target-accuracy-1-millisecond"></a>Accuratezza della destinazione: 1 millisecondo

Tutti i requisiti descritti nelle sezioni @no__t 0Target-accuratezza: 1 secondo @ no__t-0 e precisione **Target: 50 millisecondi @ no__t-0 si applicano, tranne nei casi in cui i controlli più severi sono descritti in questa sezione.

I requisiti aggiuntivi per ottenere l'accuratezza di 1 ms per un sistema di destinazione specifico sono:

-   Il computer di destinazione deve avere una latenza di rete superiore a 0,1 ms tra l'origine dell'ora

-   Il sistema di destinazione non deve essere maggiore di quello di strato 5 da un'origine ora molto accurata

    >[!Note]
    >Eseguire ' w32tm/query/status ' dalla riga di comando per visualizzare lo strato

-   Il sistema di destinazione deve essere compreso tra 4 o meno hop di rete dall'origine dell'ora a precisione elevata

-   L'utilizzo medio della CPU di un giorno in ogni strato non deve superare il 80%

-   Per i sistemi virtualizzati, l'utilizzo medio della CPU dell'host di un giorno non deve superare il 80%
