---
title: wbadmin Disabilita backup
description: Argomento di riferimento per Wbadmin Disable backup, che interrompe l'esecuzione dei backup giornalieri pianificati esistenti.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 5176cbd9-0696-4b3f-9c35-272dd84f7898
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6bc211946c908b44b196995a9e5000d6ff3b7f82
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83821421"
---
# <a name="wbadmin-disable-backup"></a>wbadmin Disabilita backup



Interrompe l'esecuzione di backup giornalieri pianificati esistenti.

Per disabilitare un backup giornaliero pianificato, è necessario essere un membro del **amministratori** gruppo, oppure è necessario che siano state delegate le autorizzazioni appropriate. Inoltre, è necessario eseguire **wbadmin** da un prompt dei comandi con privilegi elevati. Per aprire un prompt dei comandi con privilegi elevati, fare clic con il pulsante destro del mouse su **prompt dei comandi** e quindi scegliere **Esegui come amministratore**.

## <a name="syntax"></a>Sintassi

```
wbadmin disable backup
[-quiet]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|-quiet|Esegue il sottocomando senza alcuna richiesta visualizzata all'utente.|

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)