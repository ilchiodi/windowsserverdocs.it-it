---
title: arp
description: Argomento i comandi di Windows per **arp** -Visualizza e modifica di voci nella cache del protocollo PNRP (arp) indirizzo usato per archiviare gli indirizzi IP e i relativi indirizzi fisici risolti.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 827e96eb-1945-483f-980f-714703456f7c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5cd84269a5ac1a85d4b6cf359cc97f478a500c4f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59825982"
---
# <a name="arp"></a>arp

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza e modifica di voci nella cache del protocollo ARP (Address Resolution). La cache ARP contiene una o più tabelle che vengono usate per archiviare gli indirizzi IP e gli indirizzi fisici risolti Ethernet o Token Ring. È disponibile una tabella separata per ogni scheda di rete Ethernet o Token Ring installato nel computer. Se utilizzato senza parametri, **arp** Visualizza informazioni della Guida.
## <a name="syntax"></a>Sintassi
```
arp [/a [<Inetaddr>] [/n <ifaceaddr>]] [/g [<Inetaddr>] [-n <ifaceaddr>]] [/d <Inetaddr> [<ifaceaddr>]] [/s <Inetaddr> <Etheraddr> [<ifaceaddr>]]
```
### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|/a [<Inetaddr>] [/n <ifaceaddr>]|Consente di visualizzare tabelle cache arp corrente per tutte le interfacce. Il parametro /n è tra maiuscole e minuscole.<br /><br />Per visualizzare la voce della cache arp di un indirizzo IP specifico, usare **/a arp** con il *IndInet* parametro, in cui *IndInet* è un indirizzo IP. Se *IndInet* viene omesso, viene utilizzata la prima interfaccia applicabile.<br /><br />Per visualizzare la tabella di cache arp di un'interfaccia specifica, usare il **/n * * * IndIfaccia* parametro in combinazione con la **/a** parametro in cui *IndIfaccia* è l'indirizzo IP assegnato all'interfaccia.|
|/g [<Inetaddr>] [/n <ifaceaddr>]|Identico al **/a**.|
|[/d <Inetaddr> [<ifaceaddr>]|Elimina una voce con un indirizzo IP specifico, dove *IndInet* è l'indirizzo IP.<br /><br />Per eliminare una voce in una tabella per un'interfaccia specifica, usare il *IndIfaccia* parametro in cui *IndIfaccia* è l'indirizzo IP assegnato all'interfaccia.<br /><br />Per eliminare tutte le voci, usare l'asterisco (\*) carattere jolly al posto di *IndInet*.|
|/s <Inetaddr> <Etheraddr> [<ifaceaddr>]|Aggiunge una voce statica per la cache arp che risolve l'indirizzo IP *IndInet* all'indirizzo fisico *IndEther*.<br /><br />Per aggiungere una voce della cache arp statico per la tabella per un'interfaccia specifica, usare il *IndIfaccia* parametro in cui *IndIfaccia* è un indirizzo IP assegnato all'interfaccia.|
|/?|Visualizza la guida al prompt dei comandi.|
## <a name="remarks"></a>Note
-   Gli indirizzi IP per *IndInet* e *IndIfaccia* sono espressi in notazione decimale puntata.
-   L'indirizzo fisico per *IndEther* è costituito da sei byte espressi in notazione esadecimale e separati da trattini (ad esempio, 00-AA-00-4F-2A-9C).
-   Le voci aggiunte con il **/s** parametro sono statico e non compreso nel periodo dalla cache arp. Se il protocollo TCP/IP viene arrestato e avviato, vengono rimosse le voci. Per creare le voci della cache arp statico permanente, posizionare l'appropriato **arp** comandi in un batch di file e usare attività pianificate per eseguire il file batch all'avvio.
## <a name="BKMK_Examples"></a>Esempi
Per visualizzare le tabelle di cache arp per tutte le interfacce, digitare:
```
arp /a
```
Per visualizzare la tabella della cache arp per l'interfaccia che viene assegnato l'indirizzo IP 10.0.0.99, digitare:
```
arp /a /n 10.0.0.99
```
Per aggiungere una voce della cache arp statica che consente di risolvere l'indirizzo IP 10.0.0.80 all'indirizzo fisico 00-AA-00-4F-2A-9C, digitare:
```
arp /s 10.0.0.80 00-AA-00-4F-2A-9C 
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)
