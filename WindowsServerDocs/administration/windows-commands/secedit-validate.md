---
title: 'secedit: Validate'
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9fb06354-f55a-4ca4-9fbc-9a872eb9b9cf
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b9425f7a1fb821f4ecbaa7c1689c3baabbff6223
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80834874"
---
# <a name="seceditvalidate"></a>secedit: Validate



Convalida le impostazioni di sicurezza archiviate in un modello di sicurezza (file con estensione inf). Per esempi di come è possibile utilizzare questo comando, vedere [esempi](#BKMK_Examples).

## <a name="syntax"></a>Sintassi

```
Secedit /validate <configuration file name>  

```

#### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Nome file di configurazione|Obbligatoria.</br>Specifica il percorso e il nome del modello di protezione che verrà convalidato.|

## <a name="remarks"></a>Note

Convalida dei modelli di sicurezza consentono se uno è danneggiato o impostato in modo non appropriato.

Un modello di sicurezza non valido non verrà applicato.

Il file di log non essere aggiornato.

In Windows Server 2008, `Secedit /refreshpolicy` è stato sostituito con `gpupdate`. Per informazioni su come aggiornare le impostazioni di sicurezza, vedere [Gpupdate](gpupdate.md).

## <a name="examples"></a><a name=BKMK_Examples></a>Esempi

Dopo un'operazione di rollback viene eseguita su un modello di protezione, si desidera verificare che il file con estensione inf rollback, secRBKcontoso.inf, è valido.
```
Secedit /validate secRBKcontoso.inf
```

## <a name="additional-references"></a>Altre informazioni di riferimento

-   [Secedit:generaterollback](secedit-generaterollback.md)
-   [Secedit](secedit.md)
-   - [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)