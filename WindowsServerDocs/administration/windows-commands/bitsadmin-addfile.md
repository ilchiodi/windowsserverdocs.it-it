---
title: bitsadmin addfile
description: Windows Commands Topic for **BITSAdmin AddFile**, che aggiunge un file al processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1b31aa93-0364-465b-af36-754968825989
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 330e79eb2ba5a824cea54094f64ceb6f9cfd66b9
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850964"
---
# <a name="bitsadmin-addfile"></a>bitsadmin addfile

Aggiunge un file al processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /AddFile <Job> <RemoteURL> <LocalName>
```

#### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| Job | Nome visualizzato o GUID del processo. |
| RemoteURL | URL del file nel server. |
| LocalName | Nome del file nel computer locale. *LocalName* deve contenere un percorso assoluto del file. |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Aggiungere un file al processo. Ripetere questa chiamata per ogni file che si desidera aggiungere. Se più processi usano *myDownloadJob* come nome, è necessario sostituire *MYDOWNLOADJOB* con il GUID del processo per identificare in modo univoco il processo.

```
C:\>bitsadmin /addfile myDownloadJob http://downloadsrv/10mb.zip c:\10mb.zip
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Chiave della sintassi della riga di comando](command-line-syntax-key.md)&copy;