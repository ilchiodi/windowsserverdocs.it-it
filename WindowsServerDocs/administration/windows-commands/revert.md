---
title: ripristinare
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 75ad40e4-502a-401e-b11e-8b31e00424b5
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d7dae609e4615868b03e4b5ea9a681f553aa0667
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80835754"
---
# <a name="revert"></a>ripristinare



Ripristina un volume in una copia shadow specificata. Questa è supportata solo per le copie shadow nel contesto CLIENTACCESSIBLE. Queste copie shadow sono persistenti e possono essere apportate solo dal provider di sistema. Se utilizzata senza parametri, **ripristinare** Visualizza la Guida al prompt dei comandi.

## <a name="syntax"></a>Sintassi

```
revert <ShadowCopyID>
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<ShadowCopyID >|Specifica l'ID della copia shadow per ripristinare il volume.|

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)