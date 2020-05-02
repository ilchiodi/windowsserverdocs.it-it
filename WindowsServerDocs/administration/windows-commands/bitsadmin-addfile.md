---
title: bitsadmin addfile
description: Argomento di riferimento per il comando Bitsadmin AddFile, che aggiunge un file al processo specificato.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1b31aa93-0364-465b-af36-754968825989
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: eaa7d77c9d6160bbd2bdf6a1431232af22bc3e37
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82718493"
---
# <a name="bitsadmin-addfile"></a>bitsadmin addfile

Aggiunge un file al processo specificato.

## <a name="syntax"></a>Sintassi

```
bitsadmin /addfile <job> <remoteURL> <localname>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| processo | Nome visualizzato o GUID del processo. |
| remoteURL | URL del file nel server. |
| localname | Nome del file nel computer locale. *LocalName* deve contenere un percorso assoluto del file. |

## <a name="examples"></a>Esempi

Per aggiungere un file al processo:

```
bitsadmin /addfile myDownloadJob http://downloadsrv/10mb.zip c:\10mb.zip
```

Ripetere questa chiamata per ogni file da aggiungere. Se più processi usano *myDownloadJob* come nome, è necessario sostituire *MYDOWNLOADJOB* con il GUID del processo per identificare in modo univoco il processo.

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando Bitsadmin](bitsadmin.md)
