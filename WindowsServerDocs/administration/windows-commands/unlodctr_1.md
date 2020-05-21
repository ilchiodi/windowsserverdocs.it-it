---
title: unlodctr
description: Argomento di riferimento per Unlodctr, che rimuove i nomi dei contatori delle prestazioni e il testo esplicativo per un servizio o un driver di dispositivo dal registro di sistema
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: fc8aa6f0-c1d9-47ea-937a-28152148e774
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d4d81acd98a280ce110c5677f627fee8a9394e1a
ms.sourcegitcommit: bf887504703337f8ad685d778124f65fe8c3dc13
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/16/2020
ms.locfileid: "83436956"
---
# <a name="unlodctr"></a>unlodctr

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Rimuove i nomi dei **contatori delle prestazioni** e il testo **esplicativo** per un servizio o un driver di dispositivo dal registro di sistema.

## <a name="syntax"></a>Sintassi
```
Unlodctr <DriverName>
```
#### <a name="parameters"></a>Parametri
|Parametro|Descrizione|
|-------|--------|
|\<DriverName>|rimuove le impostazioni del nome del contatore delle prestazioni e il testo esplicativo per il driver o <DriverName> il servizio dal registro di sistema di Windows Server 2003.|
|/?|Visualizza la guida al prompt dei comandi.|

## <a name="remarks"></a>Osservazioni
> [!WARNING]
> La modifica non corretta del Registro di sistema potrebbe danneggiare gravemente il sistema. Prima di apportare modifiche al Registro di sistema, si consiglia di effettuare il backup di tutti i dati importanti presenti sul computer.

Se le informazioni fornite contengono spazi, racchiudere il testo tra virgolette (ad esempio, <DriverName> ).

## <a name="examples"></a>Esempi
Per rimuovere le impostazioni del Registro di sistema corrente e il testo esplicativo per il servizio Simple Mail Transfer Protocol (SMTP) del contatore:
```
unlodctr SMTPSVC
```
## <a name="additional-references"></a>Riferimenti aggiuntivi
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

