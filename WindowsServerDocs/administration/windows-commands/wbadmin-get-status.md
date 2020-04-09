---
title: stato Wbadmin get
description: Argomento dei comandi di Windows per Wbadmin get status, che indica lo stato dell'operazione di backup o ripristino attualmente in esecuzione.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 2911b944-7b95-46aa-8c1e-1d55a2fcc94c
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f8ebf1a078632f78dc8d58c232550345f0de78f2
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80829744"
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

## <a name="remarks"></a>Note

-   Questo sottocomando non verrà interrotto fino al completamento dell'operazione di backup o ripristino corrente. il sottocomando continuerà a essere eseguito anche se si chiude la finestra di comando.
-   Se si desidera arrestare l'operazione di backup o ripristino corrente, utilizzare il comando **Wbadmin Stop Job** sottocomando.

## <a name="additional-references"></a>Altre informazioni di riferimento

-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   Cmdlet [Get-WBJob](https://technet.microsoft.com/library/jj902426.aspx)