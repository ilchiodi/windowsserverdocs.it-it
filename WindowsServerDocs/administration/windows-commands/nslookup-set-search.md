---
title: nslookup set search
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 064ac660-8b04-4af9-8b2c-e4e0549771b8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 9972919eae1be21d5dd30820d64dd1576b935666
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80838304"
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

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)