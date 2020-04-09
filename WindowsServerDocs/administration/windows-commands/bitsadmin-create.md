---
title: bitsadmin create
description: Argomento dei comandi di Windows per **BITSAdmin create**, che crea un processo di trasferimento con il nome visualizzato specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 9a8c53af-900b-4c24-9265-5b8b08213fac
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 4a922d9f15aff0a9bd064a7e987920adf3a9107d
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850814"
---
# <a name="bitsadmin-create"></a>bitsadmin create

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Crea un processo di trasferimento con il nome visualizzato specificato. Scaricare i processi trasferire i dati da un server a un file locale. I processi di caricamento trasferiscono i dati da un file locale a un server. I processi di caricamento-risposta trasferiscono i dati da un file locale a un server e ricevono un file di risposta dal server.

Usare l'opzione [BITSAdmin Resume](bitsadmin-resume.md) per attivare il processo nella coda di trasferimento.

## <a name="syntax"></a>Sintassi

```
bitsadmin /create [type] displayname
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| ------- | -------- |
| type | -  **/download** trasferisce i dati da un server a un file locale.<p>- **tra/upload** trasferisce i dati da un file locale a un server.<p>-  **/upload-Reply** trasferisce i dati da un file locale a un server e riceve un file di risposta dal server.<p>Il valore predefinito di questo parametro è **/download** quando non è specificato nella riga di comando. Inoltre, i tipi di  **tra/upload** e **/upload-Reply** non sono disponibili in BITS 1,2 e versioni precedenti. |
| displayname | Nome visualizzato assegnato al processo appena creato. |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Crea un processo di download denominato *myDownloadJob*.

```
C:\>bitsadmin /create myDownloadJob
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
