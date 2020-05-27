---
title: wbadmin Arresta processo
description: Argomento di riferimento per il processo Wbadmin stop, che annulla l'operazione di backup o ripristino attualmente in esecuzione. Le operazioni di annullamento non possono essere riavviate, è necessario eseguire nuovamente un'operazione di backup o ripristino annullata dall'inizio.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 3b83b398-39c7-4410-bf17-5c1fb1a4f46d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 6e6d2be62468102060aad502d47efbb6eae4fca8
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83821361"
---
# <a name="wbadmin-stop-job"></a>wbadmin Arresta processo



Annulla l'operazione di backup o ripristino attualmente in esecuzione. Le operazioni di annullamento non possono essere riavviate, è necessario eseguire nuovamente un'operazione di backup o ripristino annullata dall'inizio.

Per interrompere un'operazione di backup o ripristino con il sottocomando, è necessario essere un membro del **gruppo Backup Operators** gruppo o **amministratori** gruppo, oppure è necessario che siano state delega l'autorità appropriata. Inoltre, è necessario eseguire **wbadmin** da un prompt dei comandi con privilegi elevati. Per aprire un prompt dei comandi con privilegi elevati, fare clic con il pulsante destro del mouse su **prompt dei comandi** e quindi scegliere **Esegui come amministratore**.

## <a name="syntax"></a>Sintassi

```
wbadmin stop job
[-quiet]
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|-quiet|Esegue il sottocomando senza alcuna richiesta visualizzata all'utente.|

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
-   [Wbadmin](wbadmin.md)