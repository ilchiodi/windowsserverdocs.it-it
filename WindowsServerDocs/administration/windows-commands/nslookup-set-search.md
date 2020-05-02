---
title: nslookup set search
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 064ac660-8b04-4af9-8b2c-e4e0549771b8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2e3f5bce42d3614b535b2dfb00c4c9ea9cac2346
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82723560"
---
# <a name="nslookup-set-search"></a>nslookup set search



Accoda i nomi di dominio di Domain Name System (DNS) nell'elenco di ricerca del dominio DNS alla richiesta fino a quando non viene ricevuta una risposta. Si applica quando il set e la ricerca richiesta contiene almeno un punto, ma non termina con un punto finale.

## <a name="syntax"></a>Sintassi

```
set [no]search
```

### <a name="parameters"></a>Parametri

|  Parametro   |                                                                          Descrizione                                                                          |
|--------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **nosearch** |                            Interrompe l'aggiunta dei nomi di dominio di Domain Name System (DNS) nell'elenco di ricerca del dominio DNS alla richiesta.                            |
|  **ricerca**  | Accoda i nomi di dominio di Domain Name System (DNS) nell'elenco di ricerca del dominio DNS alla richiesta fino a quando non viene ricevuta una risposta. La sintassi predefinita è **Search**. |
|    {Guida     |                                                                              ?}                                                                               |

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)