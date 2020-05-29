---
title: Risolvere i problemi del client DHCP
description: In questo artilce viene illustrato come risolvere i problemi del client DHCP e raccogliere i dati.
ms.prod: windows-server
ms.service: na
manager: dcscontentpm
ms.technology: server-general
ms.date: 5/26/2020
ms.topic: article
author: Deland-Han
ms.author: delhan
ms.openlocfilehash: a6064b9e497fcd54671292ade77a08c06ba42920
ms.sourcegitcommit: ef089864980a1d4793a35cbf4cbdd02ce1962054
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/28/2020
ms.locfileid: "84150299"
---
# <a name="troubleshoot-problems-on-the-dhcp-client"></a>Risolvere i problemi del client DHCP

Questo articolo illustra come risolvere i problemi che si verificano nei client DHCP.

## <a name="troubleshooting-checklist"></a>Elenco di controllo per la risoluzione dei problemi

Controllare i dispositivi e le impostazioni seguenti:

  - I cavi sono connessi e funzionanti.

  - Il filtro MAC è abilitato sui commutatori a cui è connesso il client.

  - La scheda di rete è abilitata.

  - Il driver della scheda di rete corretto viene installato e aggiornato.

  - Il servizio client DHCP è avviato e in esecuzione. Per verificarlo, eseguire il comando **net start** e cercare **client DHCP**.

  - Nel computer client non sono presenti porte di blocco del firewall 67 e 68 UDP.

## <a name="event-logs"></a>Log eventi

Esaminare i registri eventi del client Microsoft-Windows-DHCP Events/Operational e Microsoft-Windows-DHCP Events/admin. Tutti gli eventi correlati al servizio client DHCP vengono inviati ai registri eventi.  
Gli eventi del client Microsoft-Windows-DHCP si trovano nella Visualizzatore eventi in **registri applicazioni e servizi**.

Il comando di PowerShell "Get-NetAdapter-IncludeHidden" fornisce le informazioni necessarie per interpretare gli eventi elencati nei log. Ad esempio, l'ID di interfaccia, l'indirizzo MAC e così via.

## <a name="data-collection"></a>Raccolta dati

Quando si verifica il problema, è consigliabile raccogliere i dati contemporaneamente sia sul lato client che sul lato server DHCP. Tuttavia, a seconda del problema effettivo, è anche possibile avviare l'analisi usando un singolo set di dati nel client DHCP o nel server DHCP.

Per raccogliere dati dal server e dal client interessato, usare [Wireshark](https://www.wireshark.org/download.html). Avviare la raccolta allo stesso tempo sul client DHCP e sui computer server DHCP.

Eseguire i comandi seguenti nel client in cui si è verificato il problema:

```console
ipconfig /release  
ipconfig /renew
```

Quindi, arrestare Wireshark nel client e nel server. Controllare le tracce generate. Queste dovrebbero almeno indicare la fase di arresto della comunicazione.