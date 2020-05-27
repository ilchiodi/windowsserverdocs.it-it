---
title: wbadmin Elimina catalogo
description: Argomento di riferimento per Wbadmin delete catalog, che elimina il catalogo di backup archiviato nel computer locale.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: d3041407-4577-4716-a39f-2c8ab48818d1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4903c7ad2996a9f69d20f4711364669b87366527
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83821441"
---
# <a name="wbadmin-delete-catalog"></a>wbadmin Elimina catalogo



Elimina il catalogo di backup archiviato nel computer locale. Utilizzare questo comando quando il catalogo di backup è danneggiato e non è possibile ripristinarlo utilizzando **Wbadmin restore catalog**.

Per eliminare un catalogo di backup con questo sottocomando, è necessario essere un membro del gruppo **Backup Operators** o **Administrators** oppure avere ricevuto in delega le autorizzazioni appropriate. Inoltre, è necessario eseguire **wbadmin** da un prompt dei comandi con privilegi elevati. (Per aprire un prompt dei comandi con privilegi elevati di rapida **Command prompt**, quindi fare clic su **Esegui come amministratore**.)

## <a name="syntax"></a>Sintassi

```
wbadmin delete catalog
[-quiet]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|-quiet|Esegue il sottocomando senza alcuna richiesta visualizzata all'utente.|

## <a name="remarks"></a>Osservazioni

Se si elimina il catalogo di backup per un computer, non sarà possibile accedere ai backup creati da tale computer utilizzando lo snap-in Windows Server Backup. In tal caso, se è possibile accedere a un altro percorso di backup, usare **Wbadmin restore catalog** per ripristinare il catalogo di backup da tale percorso. È necessario creare un nuovo backup dopo l'eliminazione del catalogo di backup.

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)
-   [Remove-WBCatalog](https://technet.microsoft.com/library/jj902445.aspx)