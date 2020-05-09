---
title: Data
description: Argomento di riferimento per il comando date, che Visualizza o imposta la data di sistema. Se utilizzata senza parametri,
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: ce6700fb-32f9-4350-a1af-5aee61d4448c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 64d0d94061e1b5c7891b364f4c0fe153b44a564e
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993196"
---
# <a name="date"></a>Data

Visualizza o imposta la data di sistema. Se utilizzata senza parametri, **Data** Visualizza l'impostazione corrente della data di sistema e viene richiesto di immettere una nuova data.

>[!IMPORTANT]
> Per usare questo comando, è necessario essere un amministratore.

## <a name="syntax"></a>Sintassi

```
date [/t | <month-day-year>]
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| `<month-day-year>` | Imposta la data specificata, dove *Month* è il mese (una o due cifre, inclusi i valori da 1 a 12), *Day* è il giorno (una o due cifre, compresi i valori da 1 a 31) e *year* è l'anno (due o quattro cifre, inclusi i valori da 00 a 99 o da 1980 a 2099). È necessario separare i valori per *mese*, *giorno*e *anno* con punti (.), trattini (-) o barre (/).<p>**Nota:** Tenere presente che se si usano 2 cifre per rappresentare l'anno, i valori 80-99 corrispondono a 1980 a 1999. |
| /t | Visualizza la data corrente senza richiedere una nuova data. |
| /? | Visualizza la guida al prompt dei comandi. |

## <a name="examples"></a>Esempi

Se sono abilitate le estensioni dei comandi, per visualizzare la data corrente del sistema, digitare:

```
date /t
```

Per modificare la data corrente del sistema a 3 agosto 2007, è possibile digitare uno dei seguenti:

```
date 08.03.2007
date 08-03-07
date 8/3/07
```

Per visualizzare la data corrente del sistema, seguita da una richiesta di immettere una nuova data, digitare:

```
The current date is: Mon 04/02/2007
Enter the new date: (mm-dd-yyyy)
```

Per mantenere la data corrente e tornare al prompt dei comandi, premere **invio**. Per modificare la data corrente, digitare la nuova data, quindi premere **invio**.

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)