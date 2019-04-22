---
title: Risoluzione dei problemi di AD FS - risoluzione DNS
description: Questo documento descrive come risolvere i problemi gli aspetti DNS di AD FS.
author: billmath
ms.author: billmath
manager: mtillman
ms.date: 01/03/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 7e0065feac4241b617b8b13c6867d5dc36634bd0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815902"
---
# <a name="ad-fs-troubleshooting---dns"></a>Risoluzione dei problemi di AD FS - DNS 
Una delle prime cose da controllare, se AD FS non funziona o non risponde, è la risoluzione dei nomi DNS.  Si tratta di semplici test per determinare se il server AD FS o server WAP in cui vengono trovati nella rete.  Per gli utenti interni, questi test devono restituire i server AD FS (STS).    Per gli utenti esterni, questi test dovrebbero risolvere per i server WAP.

Il resto di questo documento verrà illustrato come eseguire alcuni controlli di risoluzione nome rapido usando gli strumenti da riga di comando.

## <a name="ping-test"></a>Test di ping
Verifica la connettività a livello IP a un altro computer TCP/IP inviando messaggi di richiesta Echo di Internet controllo Message Protocol (ICMP). La ricezione di messaggi Echo Reply corrispondenti vengono visualizzati, insieme ai tempi di round trip.  Per altre informazioni, vedere [Ping](https://technet.microsoft.com/library/ff961503.aspx).


>[!NOTE]
>Tenere presente che alcune organizzazioni bloccano la porta nei relativi server ed è possibile ottenere un **timeout della richiesta** risposta.

### <a name="to-use-a-ping-test"></a>Usare un test di PING
1.  Aprire un prompt dei comandi
2. Immettere il comando PING <name of adfs server> una. Esempio:  PING sts.contoso.com
3. Dovrebbe essere una risposta dal server

![Ping](media/ad-fs-tshoot-dns/dns1.png)

## <a name="nslookup"></a>NSLookup
Visualizza le informazioni che è possibile utilizzare per diagnosticare infrastruttura Domain Name System (DNS).  Per altre informazioni, vedere [NSLookup](https://technet.microsoft.com/library/cc725991.aspx).

### <a name="to-use-a-nslookup"></a>Per usare un NSLookup
1.  Aprire un prompt dei comandi
2. Immettere il comando PING <name of adfs server> una. Esempio: sts.contoso.com di nslookup
3. Si dovrebbero vedere le informazioni dns per il server ![NSLookup](media/ad-fs-tshoot-dns/dns2.png)

## <a name="tracert"></a>Tracert
Determina il percorso conduce a una destinazione per l'invio messaggio protocollo ICMP (Internet Control) Echo Request o messaggi ICMPv6 nella destinazione con l'aumentare in modo incrementale ora ai valori di campo Live (TTL).   Per altre informazioni, vedere [Tracert](https://technet.microsoft.com/library/ff961507.aspx).


### <a name="to-use-tracert"></a>Usare Tracert
1.  Aprire un prompt dei comandi
2. Immettere tracert <name of adfs server> una. Esempio: tracert sts.contoso.com
3. Dovrebbe essere il percorso di destinazione utilizzato per raggiungere il server ![Tracert](media/ad-fs-tshoot-dns/dns3.png)

## <a name="next-steps"></a>Passaggi successivi

- [Risoluzione dei problemi di AD FS](ad-fs-tshoot-overview.md)