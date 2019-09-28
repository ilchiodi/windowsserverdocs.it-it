---
title: pushprinterconnections
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: c30afb97-b149-478f-a4b9-2cbc25361818 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fe25a29af34f78ebe161dc0d07c5edf64257f5c2
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371964"
---
# <a name="pushprinterconnections"></a>pushprinterconnections



Legge le impostazioni di connessione alla stampante distribuite da Criteri di gruppo e distribuisce/rimuove le connessioni stampante in base alle esigenze.

## <a name="syntax"></a>Sintassi

```
pushprinterconnections <-log> <-?>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|> log <|Scrive un file di log di debug per utente in% temp o scrive un log di debug per computer in%windir%\Temp.|
|< >|Visualizza la Guida al prompt dei comandi.|

## <a name="remarks"></a>Note

Questa utilità viene utilizzata per l'avvio del computer o gli script di accesso utente e non deve essere eseguita dalla riga di comando.

#### <a name="additional-references"></a>Altri riferimenti

-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
-   [Distribuire stampanti usando Criteri di gruppo](https://go.microsoft.com/fwlink/?LinkId=230627)