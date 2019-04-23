---
ms.assetid: ''
title: Limiti di supporto per ora ad alta precisione
description: Questo articolo descrive i limiti di supporto per il servizio ora di Windows (W32Time) in ambienti che richiedono ora di sistema estremamente accurate e stabile.
author: shortpatti
ms.author: dacuo
manager: dougkim
ms.date: 10/17/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: networking
ms.openlocfilehash: 991bf4502546771dae9f092c6d5732f96b1278ab
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59866272"
---
# <a name="support-boundary-for-high-accuracy-time"></a>Limiti di supporto per ora ad alta precisione

>Si applica a: Windows Server 2016 e Windows 10 versione 1607 o successiva

Questo articolo descrive i limiti del supporto per il servizio ora di Windows (W32Time) in ambienti che richiedono ora di sistema estremamente accurate e stabile.

## <a name="high-accuracy-support-for-windows-81-and-2012-r2-or-prior"></a>Supporto della precisione elevata per Windows 8.1 e 2012 R2 (o precedente)

Le versioni precedenti di Windows (antecedente a Windows 10 1607 o Windows Server 2016 versione 1607) non possono garantire tempo estremamente preciso. Il servizio ora di Windows in questi sistemi:

-   Fornito l'accuratezza di tempo necessaria per soddisfare i requisiti di autenticazione Kerberos versione 5

-   Fornito ora esatta regime di controllo libero per i client Windows e i server aggiunti a una foresta di Active Directory comune

Una maggiore accuratezza requisiti erano di fuori di specifiche di progettazione del servizio ora di Windows in questi sistemi operativi e non è supportato.

## <a name="windows-10-and-windows-server-2016"></a>Windows 10 e Windows Server 2016

Accuratezza di tempo in Windows 10 e Windows Server 2016 è stata notevolmente migliorata, pur mantenendo all'indietro NTP la compatibilità con le versioni precedenti di Windows. In condizioni operative di destra, i sistemi che eseguono Windows 10 o Windows Server 2016 e versioni successive possono offrire 50 ms secondo, 1 (millisecondi) o 1 ms accuratezza.

>[!IMPORTANT]
>**Origini di ora altamente accurati**<br>
>È altamente dipendente da usando una radice accurata e stabile (strato 1) l'accuratezza di tempo risultante nella topologia di origine dell'ora. Sono presenti Windows in base e non Windows in base estremamente precisi, Windows, ora NTP origine componenti hardware compatibili con venduto da fornitori di terze parti 3rd. Contattare il fornitore l'accuratezza dei loro prodotti.

>[!IMPORTANT]
>**Accuratezza di tempo**<br>
>La distribuzione end-to-end di ora esatta da un'origine ora autorevole estremamente precisi al dispositivo finale comporta l'accuratezza di tempo. Tutto ciò che introduce l'asimmetria di rete influiranno negativamente accuratezza, i dispositivi di rete fisica, ad esempio o un carico elevato della CPU nel sistema di destinazione.

## <a name="high-accuracy-requirements"></a>Requisiti di elevata precisione

Il resto di questo documento sono descritti i requisiti dell'ambiente che devono essere soddisfatti per supportare gli obiettivi di elevata precisione rispettivi.

### <a name="target-accuracy-1-second-1s"></a>Precisione di destinazione: 1 (1) da secondo

Per ottenere 1s accuratezza per una destinazione specifica computer rispetto a un'origine ora esatta elevata:

-   Il sistema di destinazione deve eseguire Windows 10, Windows Server 2016.

-   Il sistema di destinazione deve sincronizzare l'ora di una gerarchia NTP di server ora, che si conclude con un'elevata precisione, Windows compatibile NTP origine dell'ora.

-   Tutti i sistemi operativi Windows nella gerarchia di NTP indicato in precedenza deve essere configurati come documentato nel [configurazione dei sistemi per verificarne l'accuratezza elevata](configuring-systems-for-high-accuracy.md) documentazione.

-   La latenza di rete unidirezionale cumulativo tra origine e destinazione non deve superare 100 ms. Il ritardo di rete cumulativo viene misurato tramite l'aggiunta di singoli unidirezionali ritardi tra coppie di nodi client e server NTP nella gerarchia di destinazione inizia e termina nell'origine. Per altre informazioni, vedere il documento di sincronizzazione ora di elevata precisione.

### <a name="target-accuracy-50-milliseconds"></a>Precisione di destinazione: 50 millisecondi

Tutti i requisiti descritti nella sezione **accuratezza di destinazione: 1 secondo** applica, ad eccezione di dove più severi controlli descritti in questa sezione.

I requisiti aggiuntivi per ottenere precisione di 50 ms per un sistema di destinazione specificati sono:

-   Il computer di destinazione deve avere prestazioni migliori rispetto a 5 ms di latenza di rete tra la relativa origine ora.

-   Il sistema di destinazione deve essere no ulteriormente rispetto a strato 5 da un'origine ora altamente accurati

    >[!Note]
    >Eseguire "w32tm /query /status" dalla riga di comando per visualizzare lo strato.

-   Il sistema di destinazione deve essere compreso minore o uguale a 6 hop di rete dall'origine ora altamente accurati

-   Utilizzo della CPU medio giorno nel stratums tutti non può superare il 90%

-   Per i sistemi virtualizzati, l'utilizzo di un giorno medio della CPU dell'host non deve superare il 90%

### <a name="target-accuracy-1-millisecond"></a>Precisione di destinazione: 1 millisecondo

Tutti i requisiti descritti nelle sezioni **accuratezza di destinazione: 1 secondo** e **accuratezza di destinazione: 50 millisecondi** applica, ad eccezione di dove più severi controlli descritti in questa sezione.

I requisiti aggiuntivi per ottenere 1 ms accuratezza per un sistema di destinazione specificati sono:

-   Il computer di destinazione deve avere prestazioni migliori rispetto a 0,1 ms di latenza di rete tra la relativa origine ora

-   Il sistema di destinazione deve essere no ulteriormente rispetto a strato 5 da un'origine ora altamente accurati

    >[!Note]
    >Eseguire 'w32tm /query /status' dalla riga di comando per visualizzare lo strato

-   Il sistema di destinazione deve essere compreso minore o uguale a 4 hop di rete dall'origine ora altamente accurati

-   L'utilizzo di un giorno medio della CPU tra ogni strato non deve superare 80%

-   Per i sistemi virtualizzati, l'utilizzo di un giorno medio della CPU dell'host non deve superare 80%
