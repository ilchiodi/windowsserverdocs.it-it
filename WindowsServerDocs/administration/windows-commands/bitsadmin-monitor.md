---
title: bitsadmin monitor
description: "Argomento dei comandi di Windows per **BITSAdmin monitor** : monitora i processi nella coda di trasferimento di proprietà dell'utente corrente."
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 2c424d27-e011-49c2-b579-a2c235467c39
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: fe4963349c7e17fc77500b5adfceafc48a20ac5f
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71381225"
---
# <a name="bitsadmin-monitor"></a>bitsadmin monitor



Consente di monitorare i processi nella coda di trasferimento a cui appartiene l'utente corrente.

## <a name="syntax"></a>Sintassi

```
bitsadmin /Monitor [/allusers] [/refresh <Seconds>]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|ALLUSERS|Facoltativo: monitora i processi per tutti gli utenti.|
|Aggiorna|Facoltativo: aggiornamento dei dati a un intervallo specificato da *secondi*. L'intervallo di aggiornamento predefinito è cinque secondi.|

## <a name="remarks"></a>Note

È necessario disporre dei privilegi di amministratore per utilizzare il **Allusers** parametro.

Utilizzare CTRL + C per interrompere l'aggiornamento.

## <a name="BKMK_examples"></a>Esempi

Nell'esempio seguente consente di monitorare la coda di trasferimento per i processi di proprietà dell'utente corrente e aggiorna le informazioni ogni 60 secondi.
```
C:\>bitsadmin /Monitor /refesh 60
```

#### <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)