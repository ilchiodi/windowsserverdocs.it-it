---
title: wmic
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 76397c72-d06f-4cea-88cf-c7603315a983
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6c68866fbe0c8f5b16dae77e2121331f06cdc726
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885842"
---
# <a name="wmic"></a>wmic



Visualizza le informazioni di WMI all'interno di una shell dei comandi interattiva.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
command </parameter>
```

## <a name="sub-commands"></a>Comandi secondari

I seguenti comandi secondari sono disponibili in qualsiasi momento:

|Sottocomando|Descrizione|
|-----------|-----------|
|classe|Evita la modalità di alias predefiniti di WMIC per accedere direttamente alle classi nello schema WMI.|
|path|Evita la modalità di alias predefiniti di WMIC per accedere direttamente alle istanze nello schema WMI.|
|context|Consente di visualizzare i valori correnti di tutti i parametri globali.|
|[quit \| uscire]|Chiude il WMIC shell dei comandi.|

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|</parameter>|\<Descrizione concisa, inizia con un verbo. >|
|</param2>|\<Un'altra descrizione concisa, inizia con un verbo. >|


## <a name="BKMK_examples"></a>Esempi

Per visualizzare i valori correnti di tutti i parametri globali, digitare:
```
wmic context
```
Output simile a verrà visualizzato il seguente:
```
NAMESPACE    : root\cimv2
ROLE         : root\cli
NODE(S)      : BOBENTERPRISE
IMPLEVEL     : IMPERSONATE
[AUTHORITY   : N/A]
AUTHLEVEL    : PKTPRIVACY
LOCALE       : ms_409
PRIVILEGES   : ENABLE
TRACE        : OFF
RECORD       : N/A
INTERACTIVE  : OFF
FAILFAST     : OFF
OUTPUT       : STDOUT
APPEND       : STDOUT
USER         : N/A
AGGREGATE    : ON
```
Per cambiare la lingua ID utilizzato da riga di comando per inglese (impostazioni locali ID 409), tipo:
```
wmic /locale:ms_409
```

#### <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)