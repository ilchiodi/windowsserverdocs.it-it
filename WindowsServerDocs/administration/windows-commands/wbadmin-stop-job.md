---
title: Processo di arresto Wbadmin
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 3b83b398-39c7-4410-bf17-5c1fb1a4f46d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7a9e71fe2e4883c52c2418e21fc8764fd14e6c81
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59889722"
---
# <a name="wbadmin-stop-job"></a>Processo di arresto Wbadmin



Annulla l'operazione di backup o ripristino attualmente in esecuzione. Le operazioni di annullamento non possono essere riavviate, è necessario eseguire nuovamente un'operazione di backup o ripristino annullata dall'inizio.

Per interrompere un'operazione di backup o ripristino con il sottocomando, è necessario essere un membro del **gruppo Backup Operators** gruppo o **amministratori** gruppo, oppure è necessario che siano state delega l'autorità appropriata. Inoltre, è necessario eseguire **wbadmin** da un prompt dei comandi con privilegi elevati. (Per aprire un prompt dei comandi con privilegi elevati di rapida **Command prompt** e quindi fare clic su **Esegui come amministratore**.)

## <a name="syntax"></a>Sintassi

```
wbadmin stop job
[-quiet]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|-quiet|Esegue il sottocomando senza alcuna richiesta visualizzata all'utente.|

#### <a name="additional-references"></a>Altri riferimenti

-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)