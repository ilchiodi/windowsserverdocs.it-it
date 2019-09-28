---
title: systeminfo
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 39954968-3c2e-4d3e-9d89-c9c43347461e
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 32a84a33c5339e9949648a4e40d71daf25c055d8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71370687"
---
# <a name="systeminfo"></a>systeminfo



Visualizza configurazione informazioni dettagliate su un computer e il sistema operativo, tra cui configurazione del sistema operativo, informazioni di sicurezza, ID prodotto e le proprietà dell'hardware (ad esempio RAM, spazio su disco e schede di rete).

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
Systeminfo [/s <Computer> [/u <Domain>\<UserName> [/p <Password>]]] [/fo {TABLE | LIST | CSV}] [/nh]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|/s \<Computer >|Specifica il nome o l'indirizzo IP di un computer remoto (non utilizzare barre rovesciate). Il valore predefinito è il computer locale.|
|/u \<Domain > \<UserName >|Esegue il comando con le autorizzazioni dell'account dell'account utente specificato. Se **/u** non è specificato, questo comando vengono utilizzate le autorizzazioni dell'utente attualmente connesso al computer in cui viene eseguito il comando.|
|/p \<password >|Specifica la password dell'account utente specificato nella **/u** parametro.|
|/fo \<Format >|Specifica il formato di output con uno dei valori seguenti:</br>TAVOLO: Visualizza l'output in una tabella.</br>ELENCO Visualizza l'output in un elenco.</br>CSV Visualizza l'output nel formato con valori delimitati da virgole.|
|/NH|Elimina le intestazioni di colonna nell'output. Valido quando il **/fo** parametro è impostato su TABELLA o CSV.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="BKMK_examples"></a>Esempi

Per visualizzare le informazioni di configurazione per un computer denominato SRVPRINC, digitare:

**systeminfo/s srvprinc**

Per visualizzare in remoto le informazioni di configurazione per un computer denominato Srvprinc2 nella che si trova nel dominio Domprinc, digitare:

**systeminfo/s srvprinc2 nella/u maindom\hiropln**

Per visualizzare in remoto le informazioni di configurazione (in formato elenco) per un computer denominato Srvprinc2 nella che si trova nel dominio Domprinc, digitare:

**systeminfo/s srvprinc2 nella/u maindom\hiropln/p p@ssW23/fo list**

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)