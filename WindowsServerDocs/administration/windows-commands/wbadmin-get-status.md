---
title: stato Wbadmin get
description: Argomento di riferimento per Wbadmin get status, che indica lo stato dell'operazione di backup o ripristino attualmente in esecuzione.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2911b944-7b95-46aa-8c1e-1d55a2fcc94c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 0e41c54b9f916f0032a4976cdfa6d3ca101fb744
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83821191"
---
# <a name="wbadmin-get-status"></a>stato Wbadmin get



Segnala lo stato dell'operazione di backup o ripristino attualmente in esecuzione.

Per utilizzare questo sottocomando, è necessario essere un membro del gruppo **Backup Operators** o **Administrators** oppure avere ricevuto in delega le autorizzazioni appropriate. Inoltre, è necessario eseguire **wbadmin** da un prompt dei comandi con privilegi elevati. (Per aprire un prompt dei comandi con privilegi elevati di rapida **Command prompt**, quindi fare clic su **Esegui come amministratore**.)

## <a name="syntax"></a>Sintassi

```
wbadmin get status
```

### <a name="parameters"></a>Parametri

Questo sottocomando non ha parametri.

## <a name="remarks"></a>Osservazioni

-   Questo sottocomando non verrà interrotto fino al completamento dell'operazione di backup o ripristino corrente. il sottocomando continuerà a essere eseguito anche se si chiude la finestra di comando.
-   Se si desidera arrestare l'operazione di backup o ripristino corrente, utilizzare il comando **Wbadmin Stop Job** sottocomando.

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   Cmdlet [Get-WBJob](https://technet.microsoft.com/library/jj902426.aspx)