---
title: bitsadmin complete
description: Argomento dei comandi di Windows per **BITSAdmin complete**, che completa il processo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: a5e86677-8f7b-43b3-929e-97706c57e7f1
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 847f6e298ff9701064ce4e577c785f7fc78ea22c
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80850824"
---
# <a name="bitsadmin-complete"></a>bitsadmin complete

Completa il processo. I file scaricati non sono disponibili finché non si usa questa opzione. Usare questa opzione dopo che il processo è stato spostato sullo stato trasferito. In caso contrario, saranno disponibili solo i file che sono stati trasferiti correttamente.

## <a name="syntax"></a>Sintassi

```
bitsadmin /complete <job>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| lavoro | Nome visualizzato o GUID del processo. |

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Quando lo stato del processo viene trasferito, BITS ha trasferito correttamente tutti i file nel processo. Tuttavia, i file non sono disponibili finché non si usa l'opzione **/completo** . 

Se più processi usano *myDownloadJob* come nome, è necessario sostituire *MYDOWNLOADJOB* con il GUID del processo per identificare in modo univoco il processo.

```
C:\>bitsadmin /complete myDownloadJob
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)