---
title: secedit:validate
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9fb06354-f55a-4ca4-9fbc-9a872eb9b9cf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cca64f6b2904ed11f6b45e316c8e4da0093c373e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59877912"
---
# <a name="seceditvalidate"></a>secedit:validate



Convalida le impostazioni di sicurezza archiviate in un modello di sicurezza (file con estensione inf). Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
Secedit /validate <configuration file name>  

```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Nome file di configurazione|Obbligatorio.</br>Specifica il percorso e il nome del modello di protezione che verrà convalidato.|

## <a name="remarks"></a>Note

Convalida dei modelli di sicurezza consentono se uno è danneggiato o impostato in modo non appropriato.

Un modello di sicurezza non valido non verrà applicato.

Il file di log non essere aggiornato.

In Windows Server 2008, `Secedit /refreshpolicy` è stato sostituito con `gpupdate`. Per informazioni su come aggiornare le impostazioni di sicurezza, vedere [Gpupdate](gpupdate.md).

## <a name="BKMK_Examples"></a>Esempi

Dopo un'operazione di rollback viene eseguita su un modello di protezione, si desidera verificare che il file con estensione inf rollback, secRBKcontoso.inf, è valido.
```
Secedit /validate secRBKcontoso.inf
```

#### <a name="additional-references"></a>Altri riferimenti

-   [Secedit:generaterollback](secedit-generaterollback.md)
-   [Secedit](secedit.md)
-   [Chiave sintassi della riga di comando](command-line-syntax-key.md)