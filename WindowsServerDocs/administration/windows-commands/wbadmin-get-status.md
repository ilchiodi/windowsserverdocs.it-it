---
title: Comando Wbadmin ottenere lo stato
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2911b944-7b95-46aa-8c1e-1d55a2fcc94c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 35fd640aa56bca7c5f5d6f3901fe095d0b8a73cc
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59863412"
---
# <a name="wbadmin-get-status"></a>Comando Wbadmin ottenere lo stato



Segnala lo stato dell'operazione di backup o ripristino attualmente in esecuzione.

Per utilizzare questo sottocomando, è necessario essere un membro del **Backup Operators** gruppo o il **gli amministratori** gruppo, oppure è necessario siano state delegate le autorizzazioni appropriate. Inoltre, è necessario eseguire **wbadmin** da un prompt dei comandi con privilegi elevati. (Per aprire un prompt dei comandi con privilegi elevati di rapida **Command prompt**, quindi fare clic su **Esegui come amministratore**.)

## <a name="syntax"></a>Sintassi

```
wbadmin get status
```

## <a name="parameters"></a>Parametri

Il sottocomando non ha parametri.

## <a name="remarks"></a>Note

-   Il sottocomando non verrà arrestata fino al backup corrente o di completamento dell'operazione di ripristino, il sottocomando continueranno a funzionare anche se si chiude la finestra di comando.
-   Se si desidera arrestare l'operazione di ripristino o il backup corrente, usare il **processo di arresto wbadmin** sottocomando.

#### <a name="additional-references"></a>Altri riferimenti

-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Get-WBJob](https://technet.microsoft.com/library/jj902426.aspx) cmdlet