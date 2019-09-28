---
title: 'secedit: Validate'
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: ece0a0324b77eb4226b679bc29f7bd599f15a120
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71371099"
---
# <a name="seceditvalidate"></a>secedit: Validate



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
-   [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)