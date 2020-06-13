---
title: nslookup set
description: Argomento di riferimento per il comando nslookup set, che consente di modificare le impostazioni di configurazione che influiscono sul comportamento delle ricerche.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1fe5b36d-e93e-468b-abca-43b0204b32d1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 579334b3b6b0cd5e9373876144f46fa21d57c745
ms.sourcegitcommit: 99d548141428c964facf666c10b6709d80fbb215
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/12/2020
ms.locfileid: "84721184"
---
# <a name="nslookup-set"></a>nslookup set

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Modifica impostazioni di configurazione che influiscono sulla modalità funzione di ricerca.

## <a name="syntax"></a>Sintassi

```
set all [class | d2 | debug | domain | port | querytype | recurse | retry | root | search | srchlist | timeout | type | vc] [options]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| [nslookup set all](nslookup-set-all.md) | Elenca tutte le impostazioni correnti. |
| [nslookup set class](nslookup-set-class.md) | Modifica la classe di query, che specifica il gruppo di protocolli delle informazioni. |
| [nslookup set d2](nslookup-set-d2.md) | Attiva o disattiva la modalità di debug dettagliata. |
| [nslookup set debug](nslookup-set-debug.md) | Disattiva completamente la modalità di debug. |
| [nslookup set domain](nslookup-set-domain.md) | Modifica il nome di dominio predefinito del Domain Name System (DNS) con il nome specificato. |
| [nslookup set port](nslookup-set-port.md) | Consente di modificare la porta predefinita del server dei nomi TCP/UDP Domain Name System (DNS) al valore specificato.
| [nslookup set querytype](nslookup-set-querytype.md) | Modifica il tipo di record di risorse per la query. |
| [nslookup set recurse](nslookup-set-recurse.md) | Indica al server dei nomi DNS (Domain Name System) di eseguire una query su altri server, se non vengono trovate informazioni. |
| [nslookup set retry](nslookup-set-retry.md) | Imposta il numero di tentativi. |
| [nslookup set root](nslookup-set-root.md) | Modifica il nome del server principale per le query. |
| [nslookup set search](nslookup-set-search.md) | Accoda i nomi di dominio di Domain Name System (DNS) nell'elenco di ricerca del dominio DNS alla richiesta fino a quando non viene ricevuta una risposta. |
| [nslookup set srchlist](nslookup-set-srchlist.md) | Modifica il nome di dominio e l'elenco di ricerca predefiniti Domain Name System (DNS). |
| [nslookup set timeout](nslookup-set-timeout.md) | Modifica il numero iniziale di secondi di attesa di una risposta a una richiesta di ricerca. |
| [nslookup set type](nslookup-set-type.md) | Modifica il tipo di record di risorse per la query. |
| [nslookup set vc](nslookup-set-vc.md) | Specifica se utilizzare un circuito virtuale quando si inviano richieste al server. |

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
