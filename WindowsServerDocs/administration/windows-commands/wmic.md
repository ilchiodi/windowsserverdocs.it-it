---
title: wmic
description: Argomento dei comandi di Windows per WMIC, che visualizza le informazioni WMI all'interno di una shell dei comandi interattiva.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 76397c72-d06f-4cea-88cf-c7603315a983
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 03ba4ecb4b12b03e010318bf6ca260dec00f28f3
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80829054"
---
# <a name="wmic"></a>wmic



Visualizza le informazioni WMI all'interno di una shell comandi interattiva.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
wmic </parameter>
```

## <a name="sub-commands"></a>Comandi secondari

I comandi secondari seguenti sono sempre disponibili:

|Sottocomando|Descrizione|
|-----------|-----------|
|class|Viene eseguito l'escape dalla modalità alias predefinita di WMIC per accedere direttamente alle classi nello schema WMI.|
|path|Viene eseguito l'escape dalla modalità alias predefinita di WMIC per accedere direttamente alle istanze nello schema WMI.|
|context|Consente di visualizzare i valori correnti di tutti i commutatori globali.|
|[uscire \| uscire]|Chiude la shell dei comandi WMIC.|

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per visualizzare i valori correnti di tutte le opzioni globali, digitare:
```
wmic context
```
Viene visualizzato un output simile al seguente:
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
Per modificare l'ID lingua usato dalla riga di comando in inglese (ID impostazioni locali 409), digitare:
```
wmic /locale:ms_409
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
