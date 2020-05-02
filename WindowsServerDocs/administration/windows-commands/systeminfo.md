---
title: systeminfo
description: Argomento di riferimento per SystemInfo, che visualizza informazioni di configurazione dettagliate su un computer e il relativo sistema operativo, inclusi configurazione del sistema operativo, informazioni sulla sicurezza, ID prodotto e proprietà hardware (ad esempio RAM, spazio su disco e schede di rete).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 39954968-3c2e-4d3e-9d89-c9c43347461e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6e95a61e4312e37288bc587264cf35240920abd3
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82721577"
---
# <a name="systeminfo"></a>systeminfo

Visualizza configurazione informazioni dettagliate su un computer e il sistema operativo, tra cui configurazione del sistema operativo, informazioni di sicurezza, ID prodotto e le proprietà dell'hardware (ad esempio RAM, spazio su disco e schede di rete).



## <a name="syntax"></a>Sintassi

```
Systeminfo [/s <Computer> [/u <Domain>\<UserName> [/p <Password>]]] [/fo {TABLE | LIST | CSV}] [/nh]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/s \<> computer|Specifica il nome o l'indirizzo IP di un computer remoto (non utilizzare barre rovesciate). Il valore predefinito è il computer locale.|
|/u \<dominio>\<nome utente>|Esegue il comando con le autorizzazioni dell'account dell'account utente specificato. Se **/u** non è specificato, questo comando vengono utilizzate le autorizzazioni dell'utente attualmente connesso al computer in cui viene eseguito il comando.|
|/p \<password>|Specifica la password dell'account utente specificato nella **/u** parametro.|
|> \<formato/fo|Specifica il formato di output con uno dei valori seguenti:</br>TABELLA: Visualizza l'output in una tabella.</br>ELENCO: Visualizza l'output in un elenco.</br>CSV: Visualizza l'output in formato valori separati da virgola.|
|/NH|Elimina le intestazioni di colonna nell'output. Valido quando il **/fo** parametro è impostato su TABELLA o CSV.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="examples"></a>Esempi

Per visualizzare le informazioni di configurazione per un computer denominato SRVPRINC, digitare:

**systeminfo/s srvprinc**

Per visualizzare in remoto le informazioni di configurazione per un computer denominato Srvprinc2 nella che si trova nel dominio Domprinc, digitare:

**systeminfo/s srvprinc2 nella/u maindom\hiropln**

Per visualizzare in remoto le informazioni di configurazione (in formato elenco) per un computer denominato Srvprinc2 nella che si trova nel dominio Domprinc, digitare:

**systeminfo/s srvprinc2 nella/u maindom\hiropln/p p@ssW23 /fo list**

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)