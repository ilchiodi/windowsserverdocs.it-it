---
title: nslookup set search
description: Argomento di riferimento per il comando nslookup set Search, che aggiunge i nomi di dominio di Domain Name System (DNS) nell'elenco di ricerca del dominio DNS alla richiesta fino a quando non viene ricevuta una risposta.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 064ac660-8b04-4af9-8b2c-e4e0549771b8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: c3219434f768a573c9e433c44b6b38bc9dc75f14
ms.sourcegitcommit: 99d548141428c964facf666c10b6709d80fbb215
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/12/2020
ms.locfileid: "84721426"
---
# <a name="nslookup-set-search"></a>nslookup set search

Accoda i nomi di dominio di Domain Name System (DNS) nell'elenco di ricerca del dominio DNS alla richiesta fino a quando non viene ricevuta una risposta. Si applica quando il set e la ricerca richiesta contiene almeno un punto, ma non termina con un punto finale.

## <a name="syntax"></a>Sintassi

```
set [no]search
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| nosearch | Interrompe l'aggiunta dei nomi di dominio di Domain Name System (DNS) nell'elenco di ricerca del dominio DNS per la richiesta. |
| cerca | Accoda i nomi di dominio di Domain Name System (DNS) nell'elenco di ricerca del dominio DNS per la richiesta fino a quando non viene ricevuta una risposta. Si tratta del valore predefinito. |
| /? | Visualizza la guida al prompt dei comandi. |
| /help | Visualizza la guida al prompt dei comandi. |

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
