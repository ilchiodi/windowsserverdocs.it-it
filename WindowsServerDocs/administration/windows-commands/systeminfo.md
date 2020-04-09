---
title: systeminfo
description: Windows Commands Topic for SystemInfo, che visualizza informazioni di configurazione dettagliate su un computer e il relativo sistema operativo, inclusi configurazione del sistema operativo, informazioni di sicurezza, ID prodotto e proprietà hardware (ad esempio RAM, spazio su disco e schede di rete).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 39954968-3c2e-4d3e-9d89-c9c43347461e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 41c2a499bc10f5b44f250958471b90f4b88dfede
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80833574"
---
# <a name="systeminfo"></a>systeminfo

Visualizza configurazione informazioni dettagliate su un computer e il sistema operativo, tra cui configurazione del sistema operativo, informazioni di sicurezza, ID prodotto e le proprietà dell'hardware (ad esempio RAM, spazio su disco e schede di rete).

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
Systeminfo [/s <Computer> [/u <Domain>\<UserName> [/p <Password>]]] [/fo {TABLE | LIST | CSV}] [/nh]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/s \<computer >|Specifica il nome o l'indirizzo IP di un computer remoto (non utilizzare barre rovesciate). Il valore predefinito è il computer locale.|
|/u \<dominio >\<nome utente >|Esegue il comando con le autorizzazioni dell'account dell'account utente specificato. Se **/u** non è specificato, questo comando vengono utilizzate le autorizzazioni dell'utente attualmente connesso al computer in cui viene eseguito il comando.|
|/p \<password >|Specifica la password dell'account utente specificato nella **/u** parametro.|
|Formato \</fo >|Specifica il formato di output con uno dei valori seguenti:</br>TABELLA: Visualizza l'output in una tabella.</br>ELENCO: Visualizza l'output in un elenco.</br>CSV: Visualizza l'output in formato valori separati da virgola.|
|/NH|Elimina le intestazioni di colonna nell'output. Valido quando il **/fo** parametro è impostato su TABELLA o CSV.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per visualizzare le informazioni di configurazione per un computer denominato SRVPRINC, digitare:

**systeminfo/s srvprinc**

Per visualizzare in remoto le informazioni di configurazione per un computer denominato Srvprin nella che si trova nel dominio Domprinc, digitare:

**systeminfo/s srvprin nella/u maindom\hiropln**

Per visualizzare in remoto le informazioni di configurazione (in formato elenco) per un computer denominato Srvprin nella che si trova nel dominio Domprinc, digitare:

**systeminfo/s srvprin nella/u maindom\hiropln/p p@ssW23 elenco/fo**

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)