---
title: getsecurityflags Bitsadmin
description: Windows Commands Topic for **BITSAdmin getsecurityflags**, che segnala i flag di sicurezza http per il reindirizzamento URL e i controlli eseguiti sul certificato del server durante il trasferimento.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: c2e73519-34f4-487b-af11-97d5d08ef9bb
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 360f8d5514e5251dd9e4a6a6b60335dc34fe4415
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850474"
---
# <a name="bitsadmin-getsecurityflags"></a>getsecurityflags Bitsadmin

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Flag di protezione report HTTP per il reindirizzamento degli URL e i controlli eseguiti sul certificato del server durante il trasferimento.

## <a name="syntax"></a>Sintassi

```
bitsadmin /getsecurityflags <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| -------------- | -------------- |
| lavoro | Nome visualizzato o GUID del processo. |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Nell'esempio seguente vengono recuperati i flag di sicurezza da un processo denominato *myDownloadJob*.

```
C:\>bitsadmin /getsecurityflags myDownloadJob
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)