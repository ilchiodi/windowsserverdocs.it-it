---
title: bitsadmin cancel
description: Argomento dei comandi di Windows per **BITSAdmin Cancel**, che rimuove il processo dalla coda di trasferimento ed Elimina tutti i file temporanei associati al processo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 7374b544-6a16-4d3e-872c-dcf4c02ad89d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 5c2bdeef824bc269671cc5ae926fb77cd5726c58
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850834"
---
# <a name="bitsadmin-cancel"></a>bitsadmin cancel

Rimuove il processo dalla coda di trasferimento ed Elimina tutti i file temporanei associati al processo.

## <a name="syntax"></a>Sintassi

```
bitsadmin /cancel <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| lavoro | Nome visualizzato o GUID del processo. |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente viene rimosso il processo *myDownloadJob* dalla coda di trasferimento.

```
C:\>bitsadmin /cancel myDownloadJob
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)