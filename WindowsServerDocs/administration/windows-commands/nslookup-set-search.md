---
title: nslookup set search
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 064ac660-8b04-4af9-8b2c-e4e0549771b8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d9da08a296d61789dbafeccde5d46c8a220d874c
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372783"
---
# <a name="nslookup-set-search"></a>nslookup set search



Accoda i nomi di dominio di Domain Name System (DNS) nell'elenco di ricerca del dominio DNS alla richiesta fino a quando non viene ricevuta una risposta. Si applica quando il set e la ricerca richiesta contiene almeno un punto, ma non termina con un punto finale.

## <a name="syntax"></a>Sintassi

```
set [no]search
```

## <a name="parameters"></a>Parametri

|  Parametro   |                                                                          Descrizione                                                                          |
|--------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **nosearch** |                            Interrompe l'aggiunta dei nomi di dominio di Domain Name System (DNS) nell'elenco di ricerca del dominio DNS alla richiesta.                            |
|  **ricerca**  | Accoda i nomi di dominio di Domain Name System (DNS) nell'elenco di ricerca del dominio DNS alla richiesta fino a quando non viene ricevuta una risposta. La sintassi predefinita è **Search**. |
|    {Guida     |                                                                              ?}                                                                               |

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)