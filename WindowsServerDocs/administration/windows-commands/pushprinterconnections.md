---
title: pushprinterconnections
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c30afb97-b149-478f-a4b9-2cbc25361818 vhorne
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 55294b27ec51edb7083debf15e332117b9b4e1df
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83821231"
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

## <a name="remarks"></a>Osservazioni

Questa utilità viene utilizzata per l'avvio del computer o gli script di accesso utente e non deve essere eseguita dalla riga di comando.

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
-   [Distribuire stampanti usando Criteri di gruppo](https://go.microsoft.com/fwlink/?LinkId=230627)