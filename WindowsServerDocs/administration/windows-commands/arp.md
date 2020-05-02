---
title: arp
description: Argomento di riferimento per il comando ARP, che consente di visualizzare e modificare le voci nella cache ARP (Address Resolution Protocol) utilizzata per archiviare gli indirizzi IP e i relativi indirizzi fisici risolti.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 827e96eb-1945-483f-980f-714703456f7c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 70b40b6fe16ce37f6fe7cb64c09463db8db4b47c
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718972"
---
# <a name="arp"></a>arp

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza e modifica le voci nella cache ARP (Address Resolution Protocol). La cache ARP contiene una o più tabelle che vengono usate per archiviare indirizzi IP e i relativi indirizzi fisici Ethernet o anello di token risolti. Nel computer è installata una tabella separata per ogni scheda di rete Ethernet o token ring. Usato senza parametri, **ARP** Visualizza le informazioni della guida.

## <a name="syntax"></a>Sintassi

```
arp [/a [<inetaddr>] [/n <ifaceaddr>]] [/g [<inetaddr>] [-n <ifaceaddr>]] [/d <inetaddr> [<ifaceaddr>]] [/s <inetaddr> <etheraddr> [<ifaceaddr>]]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `[/a [<inetaddr>] [/n <ifaceaddr>]` | Visualizza le tabelle della cache ARP correnti per tutte le interfacce. Il parametro **/n** fa distinzione tra maiuscole e minuscole. Per visualizzare la voce della cache ARP per un indirizzo IP specifico, usare **ARP/a** con il parametro **IndInet** , dove **IndInet** è un indirizzo IP. Se **IndInet** viene omesso, viene utilizzata la prima interfaccia applicabile. Per visualizzare la tabella ARP cache per un'interfaccia specifica, usare il parametro **/n IndIfaccia** insieme al parametro **/a** , dove **IndInet** è l'indirizzo IP assegnato all'interfaccia. |
| `[/g [<inetaddr>] [/n <ifaceaddr>]` | Identico a **/a**. |
| `[/d <inetaddr> [<ifaceaddr>]` | Elimina una voce con un indirizzo IP specifico, dove **IndInet** è l'indirizzo IP. Per eliminare una voce in una tabella per un'interfaccia specifica, usare il parametro **IndIfaccia** in cui **IndIfaccia** è l'indirizzo IP assegnato all'interfaccia. Per eliminare tutte le voci, usare il carattere jolly asterisco (*) al posto di **IndInet**. |
| `[/s <inetaddr> <etheraddr> [<ifaceaddr>]` | Aggiunge una voce statica alla cache ARP che risolve l'indirizzo IP **IndInet** nell'indirizzo fisico **IndEther**. Per aggiungere una voce della cache ARP statica alla tabella per un'interfaccia specifica, usare il parametro **IndIfaccia** in cui **IndIfaccia** è un indirizzo IP assegnato all'interfaccia. |
| /? | Visualizza la guida al prompt dei comandi. |

### <a name="remarks"></a>Osservazioni

- Gli indirizzi IP per **IndInet** e **IndIfaccia** sono espressi in notazione decimale punteggiata.

- L'indirizzo fisico per **IndEther** è costituito da sei byte espressi in notazione esadecimale e separati da trattini (ad esempio, 00-AA-00-4F-2a-9C).

- Le voci aggiunte con il **/s** parametro sono statiche e non si timeout della cache ARP. Le voci vengono rimosse se il protocollo TCP/IP viene arrestato e avviato. Per creare voci di cache ARP statiche permanenti, inserire i comandi **ARP** appropriati in un file batch e usare le attività pianificate per eseguire il file batch all'avvio.

## <a name="examples"></a>Esempi

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
