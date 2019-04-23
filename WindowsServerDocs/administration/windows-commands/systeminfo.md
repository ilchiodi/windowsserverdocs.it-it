---
title: systeminfo
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 3d08d3f86bdbd176aa4de157f58a58c9ea418470
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841502"
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
|/s \<computer >|Specifica il nome o indirizzo IP di un computer remoto (non utilizzare le barre rovesciate). Il valore predefinito è il computer locale.|
|/u \<Domain>\<UserName>|Esegue il comando con le autorizzazioni dell'account dell'account utente specificato. Se **/u** non è specificato, questo comando vengono utilizzate le autorizzazioni dell'utente attualmente connesso al computer in cui viene eseguito il comando.|
|/p \<Password>|Specifica la password dell'account utente specificato nella **/u** parametro.|
|/Fo \<formato >|Specifica il formato di output con uno dei valori seguenti:</br>TAVOLO: Visualizza l'output in una tabella.</br>ELENCO: Visualizza l'output in un elenco.</br>CSV: Visualizza l'output in formato valori separati da virgole.|
|/NH|Elimina le intestazioni di colonna nell'output. Valido quando il **/fo** parametro è impostato su TABELLA o CSV.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="BKMK_examples"></a>Esempi

Per visualizzare le informazioni di configurazione per un computer denominato Srvprinc, digitare:

**systeminfo /s Srvprinc**

Per visualizzare in modalità remota informazioni di configurazione per un computer denominato Srvmain2 che si trova nella cartella Domprinc, digitare:

**systeminfo /s srvmain2 /u maindom\hiropln**

Per visualizzare in modalità remota informazioni di configurazione (in formato elenco) per un computer denominato Srvmain2 che si trova nella cartella Domprinc, digitare:

**systeminfo /s srvmain2 /u maindom\hiropln /p p@ssW23 /fo list**

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)