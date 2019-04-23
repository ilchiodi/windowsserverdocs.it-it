---
title: pushprinterconnections
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: c571d3adffd0e6a28f63f6d821b2524dc055aa9a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873722"
---
# <a name="pushprinterconnections"></a>pushprinterconnections



Legge le impostazioni di connessione della stampante distribuiti da criteri di gruppo e consente di distribuire/rimuove le connessioni alle stampanti in base alle esigenze.

## <a name="syntax"></a>Sintassi

```
pushprinterconnections <-log> <-?>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|<-log>|Scrive una per ogni file di log di debug utente a % temp o operazioni di scrittura una per ogni log di debug di computer a windir%\temp.|
|<-?>|Visualizza la Guida al prompt dei comandi.|

## <a name="remarks"></a>Note

Questa utilità può essere utilizzata negli script di accesso di avvio o l'utente macchina e non deve essere eseguita dalla riga di comando.

#### <a name="additional-references"></a>Altri riferimenti

-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)
-   [Distribuzione delle stampanti con criteri di gruppo](https://go.microsoft.com/fwlink/?LinkId=230627)