---
title: Comando Wbadmin delete catalogo
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d3041407-4577-4716-a39f-2c8ab48818d1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2d9f60cf79fcd856fa972d8f43f195c5823e5d81
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59841412"
---
# <a name="wbadmin-delete-catalog"></a>Comando Wbadmin delete catalogo



Elimina il catalogo di backup archiviati nel computer locale. Usare questo comando quando il catalogo di backup è danneggiato e non è possibile ripristinare utilizzando **catalogo di ripristino wbadmin**.

Per eliminare un catalogo di backup con questo sottocomando, è necessario essere un membro del **al gruppo Backup Operators** gruppo o il **gli amministratori** gruppo, oppure avere ricevuto delegate le autorizzazioni appropriate. Inoltre, è necessario eseguire **wbadmin** da un prompt dei comandi con privilegi elevati. (Per aprire un prompt dei comandi con privilegi elevati di rapida **Command prompt**, quindi fare clic su **Esegui come amministratore**.)

## <a name="syntax"></a>Sintassi

```
wbadmin delete catalog
[-quiet]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|-quiet|Esegue il sottocomando senza alcuna richiesta visualizzata all'utente.|

## <a name="remarks"></a>Note

Se si elimina il catalogo di backup per un computer, non sarà in grado di accedere ai backup creati di quel computer utilizzando lo snap-in Windows Server Backup. In questo caso, se è possibile accedere a un altro percorso di backup, usare **catalogo di ripristino wbadmin** per ripristinare il catalogo di backup da quel percorso. È necessario creare un nuovo backup una volta eliminato il catalogo di backup.

#### <a name="additional-references"></a>Altri riferimenti

-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Remove-WBCatalog](https://technet.microsoft.com/library/jj902445.aspx)