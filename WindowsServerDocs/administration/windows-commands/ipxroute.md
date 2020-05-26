---
title: ipxroute
description: Argomento di riferimento per il comando IPXROUTE, che consente di visualizzare e modificare le informazioni sulle tabelle di routing utilizzate dal protocollo IPX.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3a30304f-655e-43d2-a4ac-7568abf8975c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: afcb5064bc8d4eb4a3b4a8920fcfcf71591d7af5
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83818321"
---
# <a name="ipxroute"></a>ipxroute

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Visualizza e modifica le informazioni sulle tabelle di routing utilizzate dal protocollo IPX. Usato senza parametri, **ipxroute** Visualizza le impostazioni predefinite per i pacchetti inviati a indirizzi sconosciuti, broadcast e multicast.

## <a name="syntax"></a>Sintassi

```
ipxroute servers [/type=x]
ipxroute ripout <network>
ipxroute resolve {guid | name} {GUID | <adaptername>}
ipxroute board= N [def] [gbr] [mbr] [remove=xxxxxxxxxxxx]
ipxroute config
```

### <a name="parameters"></a>Parametri
| Parametro | Descrizione |
| ------- | -------- |
| Server`[/type=x]` | Visualizza la tabella Service Access Point (SAP) per il tipo di server specificato. **x** deve essere un numero intero. Ad esempio, `/type=4` Visualizza tutti i file server. Se non si specifica **/Type**, `ipxroute servers` verranno visualizzati tutti i tipi di server, elencati in base al nome del server. |
| Risolvi `{GUID | name}``{GUID | adaptername}` | Risolve il nome del GUID per il relativo nome descrittivo o il nome descrittivo per il relativo GUID. |
| lavagna = *n* | Specifica la scheda di rete per cui si desidera eseguire una query o impostare i parametri. |
| def | Invia pacchetti per la trasmissione di TUTTE le ROUTE. Se un pacchetto viene trasmesso a un unico indirizzo di scheda MAC (Media Access) che non è presente nella tabella di routing di origine, **ipxroute** Invia il pacchetto per la SINGOLA ROUTE broadcast per impostazione predefinita. |
| GBR | Invia pacchetti per la trasmissione di TUTTE le ROUTE. Se un pacchetto viene trasmesso all'indirizzo di broadcast (FFFFFFFFFFFF), **ipxroute** Invia il pacchetto per la SINGOLA ROUTE broadcast per impostazione predefinita. |
| MBR | Invia pacchetti per la trasmissione di TUTTE le ROUTE. Se un pacchetto viene trasmesso a un indirizzo multicast (C000xxxxxxxx), **ipxroute** Invia il pacchetto per la SINGOLA ROUTE broadcast per impostazione predefinita. |
| Remove =*xxxxxxxxxxxx* | Rimuove l'indirizzo del nodo specificato dalla tabella di routing di origine. |
| config | Visualizza informazioni su tutte le associazioni per cui è configurata IPX. |
| /? | Visualizza la guida al prompt dei comandi. |

### <a name="examples"></a>Esempi

Per visualizzare i segmenti di rete a cui è collegata la workstation, l'indirizzo del nodo workstation e il tipo di frame in uso, digitare:

```
ipxroute config
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
