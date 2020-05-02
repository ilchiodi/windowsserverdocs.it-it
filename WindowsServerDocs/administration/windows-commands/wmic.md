---
title: wmic
description: Argomento di riferimento per WMIC, che visualizza le informazioni WMI all'interno di una shell dei comandi interattiva.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 76397c72-d06f-4cea-88cf-c7603315a983
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 252ba6b59c29378dd1f5e437de21a2ec4f5ec5c8
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720651"
---
# <a name="wmic"></a>wmic



Visualizza le informazioni WMI all'interno di una shell comandi interattiva.



## <a name="syntax"></a>Sintassi

```
wmic </parameter>
```

## <a name="sub-commands"></a>Comandi secondari

I comandi secondari seguenti sono sempre disponibili:

|Sottocomando|Descrizione|
|-----------|-----------|
|classe|Viene eseguito l'escape dalla modalità alias predefinita di WMIC per accedere direttamente alle classi nello schema WMI.|
|path|Viene eseguito l'escape dalla modalità alias predefinita di WMIC per accedere direttamente alle istanze nello schema WMI.|
|contesto|Consente di visualizzare i valori correnti di tutti i commutatori globali.|
|[Esci \| dall'uscita]|Chiude la shell dei comandi WMIC.|

## <a name="examples"></a>Esempi

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

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
