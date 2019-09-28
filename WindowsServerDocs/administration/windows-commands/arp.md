---
title: arp
description: 'Argomento comandi di Windows per **ARP** : consente di visualizzare e modificare le voci nella cache ARP (Address Resolution Protocol) utilizzata per archiviare gli indirizzi IP e i relativi indirizzi fisici risolti.'
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 1e6d34ceaa56ed40a1083b710e0db01b106f49e2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71382659"
---
# <a name="arp"></a>arp

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza e modifica le voci nella cache ARP (Address Resolution Protocol). La cache ARP contiene una o più tabelle che vengono usate per archiviare indirizzi IP e i relativi indirizzi fisici Ethernet o anello di token risolti. Nel computer è installata una tabella separata per ogni scheda di rete Ethernet o token ring. Usato senza parametri, **ARP** Visualizza le informazioni della guida.
## <a name="syntax"></a>Sintassi
```
arp [/a [<Inetaddr>] [/n <ifaceaddr>]] [/g [<Inetaddr>] [-n <ifaceaddr>]] [/d <Inetaddr> [<ifaceaddr>]] [/s <Inetaddr> <Etheraddr> [<ifaceaddr>]]
```
### <a name="parameters"></a>Parametri

|                Parametro                |                                                                                                                                                                                                                                                               Descrizione                                                                                                                                                                                                                                                               |
|-----------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|    /a [<Inetaddr>] [/n <ifaceaddr>]     | Visualizza le tabelle della cache ARP correnti per tutte le interfacce. Il parametro/n fa distinzione tra maiuscole e minuscole.<br /><br />Per visualizzare la voce della cache ARP per un indirizzo IP specifico, usare **ARP/a** con il parametro *IndInet* , dove *IndInet* è un indirizzo IP. Se *IndInet* viene omesso, viene utilizzata la prima interfaccia applicabile.<br /><br />Per visualizzare la tabella ARP cache per un'interfaccia specifica, usare il parametro **/n**_IndIfaccia_ insieme al parametro **/a** , dove *IndIfaccia* è l'indirizzo IP assegnato all'interfaccia. |
|    /g [<Inetaddr>] [/n <ifaceaddr>]     |                                                                                                                                                                                                                                                          Identico a **/a**.                                                                                                                                                                                                                                                           |
|      [/d <Inetaddr> [<ifaceaddr>]       |                                                                                           Elimina una voce con un indirizzo IP specifico, dove *IndInet* è l'indirizzo IP.<br /><br />Per eliminare una voce in una tabella per un'interfaccia specifica, usare il parametro *IndIfaccia* in cui *IndIfaccia* è l'indirizzo IP assegnato all'interfaccia.<br /><br />Per eliminare tutte le voci, usare il carattere jolly asterisco (\*) al posto di *IndInet*.                                                                                           |
| /s <Inetaddr> <Etheraddr> [<ifaceaddr>] |                                                                                                                     aggiunge una voce statica alla cache ARP che risolve l'indirizzo IP *IndInet* nell'indirizzo fisico *IndEther*.<br /><br />Per aggiungere una voce della cache ARP statica alla tabella per un'interfaccia specifica, usare il parametro *IndIfaccia* in cui *IndIfaccia* è un indirizzo IP assegnato all'interfaccia.                                                                                                                     |
|                   /?                    |                                                                                                                                                                                                                                                  Visualizza la guida al prompt dei comandi.                                                                                                                                                                                                                                                   |

## <a name="remarks"></a>Note
- Gli indirizzi IP per *IndInet* e *IndIfaccia* sono espressi in notazione decimale punteggiata.
- L'indirizzo fisico per *IndEther* è costituito da sei byte espressi in notazione esadecimale e separati da trattini (ad esempio, 00-AA-00-4F-2a-9C).
- Le voci aggiunte con il **/s** parametro sono statiche e non si timeout della cache ARP. Le voci vengono rimosse se il protocollo TCP/IP viene arrestato e avviato. Per creare voci di cache ARP statiche permanenti, inserire i comandi **ARP** appropriati in un file batch e usare le attività pianificate per eseguire il file batch all'avvio.
  ## <a name="BKMK_Examples"></a>Esempi
  Per visualizzare le tabelle della cache ARP per tutte le interfacce, digitare:
  ```
  arp /a
  ```
  Per visualizzare la tabella ARP cache per l'interfaccia a cui è assegnato l'indirizzo IP 10.0.0.99, digitare:
  ```
  arp /a /n 10.0.0.99
  ```
  Per aggiungere una voce della cache ARP statica che risolve l'indirizzo IP 10.0.0.80 nell'indirizzo fisico 00-AA-00-4F-2A-9C, digitare:
  ```
  arp /s 10.0.0.80 00-AA-00-4F-2A-9C 
  ```
  ## <a name="additional-references"></a>Riferimenti aggiuntivi
- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
