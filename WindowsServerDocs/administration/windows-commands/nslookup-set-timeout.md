---
title: nslookup set timeout
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 07afdaf4-ffec-496f-a188-4e91cf1a28f8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5b7a35a4a8e9e9cc10ea5548875f710fb5a86036
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59837812"
---
# <a name="nslookup-set-timeout"></a>nslookup set timeout

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

modifica il numero di secondi di attesa per la risposta a una richiesta di ricerca iniziale.
## <a name="syntax"></a>Sintassi
```
set timeout=<Number>
```
## <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|<Number>|Specifica il numero di secondi di attesa di una risposta. Il numero predefinito di secondi di attesa è 5.|
|{help &#124; ?}|Viene visualizzato un breve riepilogo di **nslookup** sottocomandi.|
## <a name="remarks"></a>Note
-   Quando una risposta a una richiesta non viene ricevuta entro il periodo di tempo specificato, viene raddoppiato il timeout e la richiesta viene inviata nuovamente. È possibile usare la **set tentativi** comando per controllare il numero di tentativi.
## <a name="BKMK_examples"></a>Esempi
L'esempio seguente imposta il timeout per ottenere una risposta a 2 secondi:
```
set timeout=2
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
[Chiave sintassi della riga di comando](command-line-syntax-key.md)
[nslookup set tentativi](nslookup-set-retry.md)
