---
title: 'secedit: Validate'
description: Argomento di riferimento per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9fb06354-f55a-4ca4-9fbc-9a872eb9b9cf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3043a4af6c2ac4a6c58b973cca5abd066109eac5
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82722043"
---
# <a name="seceditvalidate"></a>secedit: Validate



Convalida le impostazioni di sicurezza archiviate in un modello di sicurezza (file con estensione inf).

## <a name="syntax"></a>Sintassi

```
Secedit /validate <configuration file name>  

```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Nome file di configurazione|Obbligatorio.</br>Specifica il percorso e il nome del modello di protezione che verrà convalidato.|

## <a name="remarks"></a>Osservazioni

Convalida dei modelli di sicurezza consentono se uno è danneggiato o impostato in modo non appropriato.

Un modello di sicurezza non valido non verrà applicato.

Il file di log non essere aggiornato.

In Windows Server 2008, `Secedit /refreshpolicy` è stato sostituito con `gpupdate`. Per informazioni su come aggiornare le impostazioni di sicurezza, vedere [Gpupdate](gpupdate.md).

## <a name="examples"></a>Esempi

Dopo un'operazione di rollback viene eseguita su un modello di protezione, si desidera verificare che il file con estensione inf rollback, secRBKcontoso.inf, è valido.
```
Secedit /validate secRBKcontoso.inf
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

-   [Secedit:generaterollback](secedit-generaterollback.md)
-   [Secedit](secedit.md)
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)