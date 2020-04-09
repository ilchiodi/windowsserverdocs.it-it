---
title: Sottocomando set-TransportServer
description: Windows Commands argomento per sottocommand Set-TransportServer, che imposta le impostazioni di configurazione per un server di trasporto.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7863225c-f4b2-4cd0-b929-78a454bef249
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4c73567c58ff920b67f6e37a6f284d58517f5565
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833814"
---
# <a name="subcommand-set-transportserver"></a>Sottocomando: set-TransportServer

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Imposta le impostazioni di configurazione per un Server di trasporto.

## <a name="syntax"></a>Sintassi
```
wdsutil [Options] /Set-TransportServer [/Server:<Server name>]
     [/ObtainIpv4From:{Dhcp | Range}]
        [/start:<starting IP address>]
        [/End:<Ending IP address>]
     [/ObtainIpv6From:Range]\n\
        [/start:<start IP address>]\n\
        [/End:<End IP address>]      
     [/startPort:<starting port>
        [/EndPort:<starting port>
     [/Profile:{10Mbps | 100Mbps | 1Gbps | Custom}]    
     [/MulticastSessionPolicy]
             [/Policy:{None | AutoDisconnect | Multistream}]
                 [/Threshold:<Speed in KBps>]
                 [/StreamCount:{2 | 3}]
                 [/Fallback:{Yes | No}]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|[/Server:<Server name>]|Specifica il nome del Server di trasporto. Questo può essere il nome NetBIOS o il nome di dominio completo (FQDN). Se viene specificato alcun nome di Server di trasporto, viene utilizzato il server locale.|
|[/ ObtainIpv4From: {Dhcp & #124; Intervallo}]|Imposta l'origine degli indirizzi IPv4 come indicato di seguito:<p>-[/Start: <IP address>] imposta l'inizio dell'intervallo di indirizzi IP. Questa operazione è necessaria e valido solo se questa opzione è impostata su **intervallo**.<br />-[/ Fine: <IP address>] imposta la fine dell'intervallo di indirizzi IP. Questa operazione è necessaria e valido solo se questa opzione è impostata su **intervallo**.<br />-[/startport: <port>] imposta l'inizio dell'intervallo di porte.<br />-[/ EndPort: <port>] imposta la fine dell'intervallo di porte.|
|[/ ObtainIpv6From:Range]|Specifica l'origine di indirizzi IPv6. Questa opzione si applica solo a Windows Server 2008 R2 ed è l'unico valore supportato **intervallo**.<p>-[/Start: <IP address>] imposta l'inizio dell'intervallo di indirizzi IP. Questa operazione è necessaria e valido solo se questa opzione è impostata su **intervallo**.<br />-[/ Fine: <IP address>] imposta la fine dell'intervallo di indirizzi IP. Questa operazione è necessaria e valido solo se questa opzione è impostata su **intervallo**.<br />-[/startport: <port>] imposta l'inizio dell'intervallo di porte.<br />-[/ EndPort: <port>] imposta la fine dell'intervallo di porte.|
|[/Profile: {10 Mbps & #124; 100 Mbps & #124; 1 Gbps & #124; Personalizza}]|Specifica il profilo di rete da utilizzare. Questa opzione è disponibile solo per i server che eseguono Windows Server 2008 o Windows Server 2003.|
|[/MulticastSessionPolicy]|Configura le impostazioni di trasferimento per trasmissioni multicast. Questo comando è disponibile solo per Windows Server 2008 R2.<p>-[/ Criteri: {nessuno & #124; Disconnessione automatica & #124; Ausiliaria}] determina come gestire i client lenti. **Nessuno** significa che mantenere tutti i client in una sessione alla stessa velocità. **Disconnessione automatica** significa che qualsiasi client che scendono di sotto specificato **/Threshold** sono disconnessi. **Ausiliaria** significa che i client saranno separati in più sessioni come specificato da **/StreamCount**.<br />-[/ Soglia:<Speed in KBps>] imposta la velocità di trasferimento minimo in KBps per **/Policy:AutoDisconnect**. I client che scendono di sotto questa frequenza vengono disconnessi trasmissioni multicast.<br />-[/ StreamCount: {124 # & 2, 3}] [/ Fallback: {Sì & #124; No}] determina il numero di sessioni per **/Policy:Multistream**. **2** significa che due sessioni (veloci e lente), e **3** significa (lenta, Media, fast) e tre le sessioni.<br />-[/ Fallback: {Sì & #124; No}] determina se i client che sono disconnessi continuerà il trasferimento mediante un altro metodo (se supportato dal client). Se si utilizza il client di servizi di distribuzione Windows, il computer tornerà all'unicast. Wdsmcast.exe non supporta un meccanismo di fallback. Questa opzione si applica anche ai client che non supportano **ausiliaria**. In tal caso, il computer tornerà a un altro metodo anziché spostarsi a una sessione di trasferimento più lenta.|
## <a name="examples"></a><a name=BKMK_examples></a>Esempi
Per impostare l'intervallo di indirizzi IPv4 per il server, digitare:
```
wdsutil /Set-TransportServer /ObtainIpv4From:Range /start:239.0.0.1 /End:239.0.0.100
```
Per impostare l'intervallo di indirizzi IPv4, l'intervallo di porte e profilo per il server, digitare:
```
wdsutil /Set-TransportServer /Server:MyWDSServer /ObtainIpv4From:Range /start:239.0.0.1 /End:239.0.0.100 /startPort:12000 /EndPort:50000 /Profile:10mbps
```
## <a name="additional-references"></a>Altre informazioni di riferimento
- [Sintassi della riga di comando chiave](command-line-syntax-key.md)
[utilizzando il comando disable-TransportServer](using-the-disable-transportserver-command.md)
[utilizzando il comando enable-TransportServer](using-the-enable-transportserver-command.md)
[utilizzando il comando get-TransportServer](using-the-get-transportserver-command.md)
[sottocomando: start-TransportServer](subcommand-start-transportserver.md)
[sottocomando: stop-TransportServer](subcommand-stop-transportserver.md)
