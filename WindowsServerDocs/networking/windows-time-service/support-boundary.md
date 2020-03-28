---
ms.assetid: ''
title: Limiti di supporto per ora con accuratezza elevata
description: Questo articolo descrive i limiti di supporto per il servizio Ora di Windows (W32Time) in ambienti che richiedono elevate accuratezza e stabilità dell'ora di sistema.
author: eross-msft
ms.author: dacuo
manager: dougkim
ms.date: 10/17/2018
ms.topic: article
ms.prod: windows-server
ms.technology: networking
ms.openlocfilehash: 5748d598272c8f096876bab0d24142d38c2fd64b
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80314967"
---
# <a name="support-boundary-for-high-accuracy-time"></a>Limiti di supporto per ora con accuratezza elevata

>Si applica a: Windows Server 2016 e Windows 10, versione 1607 o successive

Questo articolo descrive i limiti di supporto per il servizio Ora di Windows (W32Time) in ambienti che richiedono elevate accuratezza e stabilità dell'ora di sistema.

## <a name="high-accuracy-support-for-windows-81-and-2012-r2-or-prior"></a>Supporto dell'accuratezza elevata per Windows 8.1 e 2012 R2 (o versioni precedenti)

Le versioni precedenti di Windows (anteriori a Windows 10 1607 o Windows Server 2016 1607) non possono garantire un'accuratezza elevata dell'ora. Il servizio Ora di Windows in questi sistemi:

-   Offre l'accuratezza dell'ora necessaria per soddisfare i requisiti di autenticazione Kerberos versione 5

-   Offre una limitata accuratezza dell'ora per i client e i server Windows aggiunti a una foresta Active Directory comune

I requisiti di accuratezza più rigorosi esulano dalla specifica di progettazione del servizio Ora di Windows in questi sistemi operativi e non sono supportati.

## <a name="windows-10-and-windows-server-2016"></a>Windows 10 e Windows Server 2016

L'accuratezza dell'ora in Windows 10 e Windows Server 2016 è stata migliorata in modo significativo, mantenendo contemporaneamente la completa compatibilità NTP con le versioni precedenti di Windows. Nelle condizioni operative appropriate i sistemi che eseguono Windows 10 o Windows Server 2016 e versioni successive possono fornire un'accuratezza di 1 secondo, 50 ms (millisecondi) o 1 ms.

>[!IMPORTANT]
>**Origini ora estremamente accurate**<br>
>L'accuratezza dell'ora risultante nella tua topologia dipende in modo significativo dall'uso di un'origine ora radice (strato 1) stabile e accurata. Sono disponibili hardware Windows e non Windows di origine ora NTP estremamente accurati, compatibili con Windows, venduti da fornitori di terze parti. Rivolgiti al tuo fornitore per verificare l'accuratezza dei suoi prodotti.

>[!IMPORTANT]
>**Accuratezza dell'ora**<br>
>L'accuratezza dell'ora comporta la distribuzione end-to-end dell'ora esatta da un'origine ora autorevole estremamente accurata al dispositivo finale. Qualsiasi elemento introduca asimmetria di rete influirà negativamente sull'accuratezza, ad esempio dispositivi di rete fisica o un carico elevato della CPU nel sistema di destinazione.

## <a name="high-accuracy-requirements"></a>Requisiti per l'accuratezza elevata

Nella parte restante di questo documento vengono illustrati i requisiti ambientali che devono essere soddisfatti per supportare gli obiettivi di accuratezza elevata corrispondenti.

### <a name="target-accuracy-1-second-1s"></a>Accuratezza nella destinazione: 1 secondo (1s)

Per ottenere l'accuratezza 1S per un computer di destinazione specifico rispetto a un'origine ora altamente accurata:

-   Il sistema di destinazione deve eseguire Windows 10, Windows Server 2016.

-   Il sistema di destinazione deve sincronizzare l'ora da una gerarchia NTP di server di riferimento ora, fino a ottenere un'origine ora NTP altamente accurata compatibile con Windows.

-   Tutti i sistemi operativi Windows nella gerarchia NTP sopra menzionati devono essere configurati come indicato nella documentazione [Configurazione dei sistemi per l'accuratezza elevata](configuring-systems-for-high-accuracy.md).

-   La latenza di rete unidirezionale cumulativa tra la destinazione e l'origine non deve superare 100 ms. Il ritardo della rete cumulativo viene misurato aggiungendo i singoli ritardi unidirezionali tra coppie di nodi client-server NTP nella gerarchia, iniziando dalla destinazione e terminando nell'origine. Per altre informazioni, vedi il documento relativo alla sincronizzazione dell'ora con accuratezza elevata.

### <a name="target-accuracy-50-milliseconds"></a>Accuratezza nella destinazione: 50 millisecondi

Si applicano tutti i requisiti descritti nella sezione **Accuratezza nella destinazione: 1 secondo**, tranne nei casi in cui sono descritti i controlli più severi in questa sezione.

I requisiti aggiuntivi per ottenere l'accuratezza di 50 ms per un sistema di destinazione specifico sono:

-   Il computer di destinazione deve avere una latenza di rete tra l'origine ora maggiore di 5 ms

-   Il sistema di destinazione non deve superare lo strato 5 da un'origine ora con accuratezza elevata

    >[!Note]
    >Esegui "w32tm/query/status" dalla riga di comando per visualizzare lo strato.

-   Il sistema di destinazione deve trovarsi entro sei hop di rete dall'origine ora con accuratezza elevata

-   L'utilizzo medio giornaliero della CPU su tutti gli strati non deve superare il 90%

-   Per i sistemi virtualizzati, l'utilizzo medio giornaliero della CPU dell'host non deve superare il 90%

### <a name="target-accuracy-1-millisecond"></a>Accuratezza nella destinazione: 1 millisecondo

Si applicano tutti i requisiti descritti nella sezione **Accuratezza nella destinazione: 1 secondo** e **Accuratezza nella destinazione: 50 millisecondi**, tranne nei casi in cui sono descritti i controlli più severi in questa sezione.

I requisiti aggiuntivi per ottenere l'accuratezza di 1 ms per un sistema di destinazione specifico sono:

-   Il computer di destinazione deve avere una latenza di rete tra l'origine ora maggiore di 0,1 ms

-   Il sistema di destinazione non deve superare lo strato 5 da un'origine ora con accuratezza elevata

    >[!Note]
    >Esegui "w32tm/query/status" dalla riga di comando per visualizzare lo strato.

-   Il sistema di destinazione deve trovarsi entro quattro hop di rete dall'origine ora con accuratezza elevata

-   L'utilizzo medio giornaliero della CPU su ogni strato non deve superare l'80%

-   Per i sistemi virtualizzati, l'utilizzo medio giornaliero della CPU dell'host non deve superare l'80%
