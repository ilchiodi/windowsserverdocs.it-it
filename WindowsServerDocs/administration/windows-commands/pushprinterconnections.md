---
title: pushprinterconnections
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c30afb97-b149-478f-a4b9-2cbc25361818 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5941b1eba55ce7524946f3257c093d409ef7d773
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80837074"
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
|< >|Visualizza la Guida dal prompt dei comandi.|

## <a name="remarks"></a>Note

Questa utilità viene utilizzata per l'avvio del computer o gli script di accesso utente e non deve essere eseguita dalla riga di comando.

## <a name="additional-references"></a>Altre informazioni di riferimento

-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
-   [Distribuire stampanti usando Criteri di gruppo](https://go.microsoft.com/fwlink/?LinkId=230627)