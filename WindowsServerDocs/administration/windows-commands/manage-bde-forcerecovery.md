---
title: Manage-bde forcerecovery
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: eecae37c-c9a3-46c5-b615-a0ace1f1d778
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1213eb583099a575fd118b1ade5d2c9b82c6dea7
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83820681"
---
# <a name="manage-bde-forcerecovery"></a>Manage-bde: forcerecovery



Forza un'unità protetta da BitLocker in modalità di ripristino al riavvio. Questo comando Elimina tutte le protezioni con chiave correlati al TPM di Trusted Platform Module dall'unità. Al riavvio del computer, solo una password o una chiave di ripristino può essere utilizzata per sbloccare l'unità.

## <a name="syntax"></a>Sintassi

```
manage-bde –forcerecovery <Drive> [-computername <Name>] [{-?|/?}] [{-help|-h}]
```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<> unità|Rappresenta una lettera di unità seguita da due punti.|
|-computername|Specifica che verrà utilizzato Gestione bde.exe per modificare la protezione BitLocker su un computer diverso. È inoltre possibile utilizzare **- cn** come una versione abbreviata di questo comando.|
|\<Name>|Rappresenta il nome del computer in cui si desidera modificare la protezione BitLocker. I valori accettati includono nome NetBIOS del computer e l'indirizzo IP del computer.|
|-? o /?|Visualizza una breve guida al prompt dei comandi.|
|-Help o-h|Visualizza la Guida completa al prompt dei comandi.|

## <a name="examples"></a>Esempi

Per illustrare l'uso del comando **-forcerecovery** per attivare la modalità di ripristino di BitLocker nell'unità C.
```
manage-bde –forcerecovery C:
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
-   [Manage-bde](manage-bde.md)