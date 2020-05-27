---
title: getmac
description: Argomento di riferimento per il comando getmac, che restituisce l'indirizzo Media Access Control (MAC) e l'elenco dei protocolli di rete associati a ogni, in locale o in una rete.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a749a348-7cd1-4336-9f33-bb42dd0e31e1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b84218b5506770bbefd5af8c89801547cc658f88
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83819091"
---
# <a name="getmac"></a>getmac

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Restituisce la media access control (MAC) indirizzo e l'elenco dei protocolli di rete associati con ogni indirizzo per tutte le schede di rete in ogni computer, localmente o attraverso una rete. Questo comando è particolarmente utile quando si vuole immettere l'indirizzo MAC in un analizzatore di rete o quando è necessario conoscere i protocolli attualmente in uso in ogni scheda di rete in un computer.

## <a name="syntax"></a>Sintassi

```
getmac[.exe][/s <computer> [/u <domain\<user> [/p <password>]]][/fo {table | list | csv}][/nh][/v]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- |------------ |
| /s`<computer>` | Specifica il nome o l'indirizzo IP di un computer remoto (non utilizzare barre rovesciate). Il valore predefinito è il computer locale. |
| /u`<domain>\<user>` | Esegue il comando con le autorizzazioni dell'account dell'utente specificato dall' *utente* o *dominio\utente*. Il valore predefinito è le autorizzazioni dell'oggetto utente nel computer eseguendo il comando connesso. |
| /p`<password>` | Specifica la password dell'account utente specificato nella **/u** parametro. |
| /fo {tabella | list | CSV | Specifica il formato da utilizzare per l'output della query. I valori validi sono **tabella**, **elenco**, e **csv**. Il formato predefinito per l'output è **Table**. |
| /NH | Omette le intestazioni di colonna nell'output. Valido quando il parametro **/fo** è impostato su **Table** o **CSV**. |
| /v | Specifica la visualizzazione di informazioni dettagliate. |
| /? | Visualizza la guida al prompt dei comandi. |

### <a name="examples"></a>Esempi

Gli esempi seguenti illustrano come utilizzare il **getmac** comando:

```
getmac /fo table /nh /v
```

```
getmac /s srvmain
```

```
getmac /s srvmain /u maindom\hiropln
```

```
getmac /s srvmain /u maindom\hiropln /p p@ssW23
```

```
getmac /s srvmain /u maindom\hiropln /p p@ssW23 /fo list /v
```

```
getmac /s srvmain /u maindom\hiropln /p p@ssW23 /fo table /nh
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
