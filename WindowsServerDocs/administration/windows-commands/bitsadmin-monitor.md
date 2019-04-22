---
title: bitsadmin monitor
description: Argomento i comandi di Windows per **monitoraggio bitsadmin** -consente di monitorare i processi nella coda di trasferimento a cui appartiene l'utente corrente.
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: 2c4620d5c8e46cb8bfcb6b9c83261d57781abea5
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59814592"
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
|ALLUSERS|Facoltativo: consente di monitorare i processi per tutti gli utenti.|
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

[Chiave sintassi della riga di comando](command-line-syntax-key.md)