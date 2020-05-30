---
title: pushprinterconnections
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c30afb97-b149-478f-a4b9-2cbc25361818
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1ed83b51da5ad7ad0a6b4dc0afd1d6bc30803a98
ms.sourcegitcommit: 29bc8740e5a8b1ba8f73b10ba4d08afdf07438b0
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/30/2020
ms.locfileid: "84222041"
---
# <a name="pushprinterconnections"></a>pushprinterconnections



Legge le impostazioni di connessione alla stampante distribuite da Criteri di gruppo e distribuisce/rimuove le connessioni stampante in base alle esigenze.

## <a name="syntax"></a>Sintassi

```
pushprinterconnections <-log> <-?>
```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|> log <|Scrive un file di log di debug per utente in% temp o scrive un log di debug per computer in%windir%\Temp.|
|< >|Visualizza la Guida al prompt dei comandi.|

## <a name="remarks"></a>Commenti

Questa utilità viene utilizzata per l'avvio del computer o gli script di accesso utente e non deve essere eseguita dalla riga di comando.

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
-   [Distribuire stampanti usando Criteri di gruppo](https://go.microsoft.com/fwlink/?LinkId=230627)