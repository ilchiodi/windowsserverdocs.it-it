---
title: Risoluzione dei problemi di AD FS-risoluzione DNS
description: In questo documento viene descritto come risolvere i problemi relativi a DNS di AD FS
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/03/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 7ffda6916bd91f1195ac0c289959becafff1d2c5
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71407203"
---
# <a name="ad-fs-troubleshooting---dns"></a>Risoluzione dei problemi di AD FS-DNS 
Una delle prime cose da verificare, se AD FS non funziona o non risponde, è la risoluzione dei nomi DNS.  Si tratta di test di base per determinare se i server AD FS o i server WAP sono presenti sulla rete.  Per gli utenti interni, questi test dovrebbero risolversi nei server di AD FS (STS).    Per gli utenti esterni, questi test devono essere risolti nei server WAP.

Nella parte restante di questo documento verrà illustrato come eseguire alcuni controlli di risoluzione dei nomi rapidi utilizzando gli strumenti da riga di comando.

## <a name="ping-test"></a>Ping test
Verifica la connettività a livello IP a un altro computer TCP/IP inviando Internet Control Message Protocol (ICMP) messaggi di richiesta echo. Viene visualizzata la ricezione dei messaggi di risposta echo corrispondenti, insieme ai tempi di round trip.  Per ulteriori informazioni, vedere [ping](https://technet.microsoft.com/library/ff961503.aspx).


>[!NOTE]
>Tenere presente che alcune organizzazioni bloccano questa porta sui server ed è possibile che venga **richiesta** una risposta scaduta.

### <a name="to-use-a-ping-test"></a>Per utilizzare un test di PING
1.  Aprire un prompt dei comandi
2. Immettere PING <name of adfs server> a. Esempio: PING sts.contoso.com
3. Verrà visualizzata una risposta dal server

![Ping](media/ad-fs-tshoot-dns/dns1.png)

## <a name="nslookup"></a>NSLookup
Visualizza le informazioni che è possibile utilizzare per diagnosticare infrastruttura Domain Name System (DNS).  Per ulteriori informazioni, vedere [nslookup](https://technet.microsoft.com/library/cc725991.aspx).

### <a name="to-use-a-nslookup"></a>Per usare un NSLookup
1.  Aprire un prompt dei comandi
2. Immettere PING <name of adfs server> a. Esempio: nslookup sts.contoso.com
3. Verranno visualizzate le informazioni DNS per il server ![NSLookup](media/ad-fs-tshoot-dns/dns2.png)

## <a name="tracert"></a>Tracert
Determina il percorso effettuato a una destinazione inviando i messaggi di richiesta echo Internet Control Message Protocol (ICMP) o ICMPv6 alla destinazione con valori di campo TTL (time to Live) che aumentano in modo incrementale.   Per ulteriori informazioni, vedere [tracert](https://technet.microsoft.com/library/ff961507.aspx).


### <a name="to-use-tracert"></a>Per usare tracert
1.  Aprire un prompt dei comandi
2. Immettere tracert <name of adfs server> a. Esempio: tracert sts.contoso.com
3. Verrà visualizzato il percorso di destinazione usato per raggiungere il server ![tracert](media/ad-fs-tshoot-dns/dns3.png)

## <a name="next-steps"></a>Passaggi successivi

- [Risoluzione dei problemi relativi ad AD FS](ad-fs-tshoot-overview.md)