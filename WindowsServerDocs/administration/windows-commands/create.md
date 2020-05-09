---
title: create
description: Argomento di riferimento per il comando crea, che consente di creare una partizione o una partizione Shadow su un disco, un volume in uno o più dischi o un disco rigido virtuale (VHD).
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: b45acde1-8f4f-4ec3-b905-d8188f884af8
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f371bee591e29a45b1488b2c36112b55ed54d3f8
ms.sourcegitcommit: fad2ba64bbc13763772e21ed3eabd010f6a5da34
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/09/2020
ms.locfileid: "82993187"
---
# <a name="create"></a>create

Crea una partizione o un'ombreggiatura su un disco, un volume in uno o più dischi o un disco rigido virtuale (VHD). Se si usa questo comando per creare un volume sul disco Shadow, è necessario che nel set di copie shadow sia già presente almeno un volume.

## <a name="syntax"></a>Sintassi

```
create partition
create volume
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| [comando create partition primary](create-partition-primary.md) | Crea una partizione primaria del disco di base con lo stato attivo. |
| [comando create partition EFI](create-partition-efi.md) | Crea una partizione di sistema Extensible Firmware Interface (EFI) in un disco della tabella di partizione GUID (GPT) in computer basati su Itanium. |
| [comando create partition Extended](create-partition-extended.md) | Crea una partizione estesa sul disco con lo stato attivo. |
| [comando create partition logical](create-partition-logical.md) | Crea una partizione logica in una partizione estesa esistente. |
| [comando create partition MSR](create-partition-msr.md) | Crea una partizione Microsoft riservata (MSR) in un disco della tabella di partizione GUID (GPT). |
| [comando Crea volume semplice](create-volume-simple.md) | Crea un volume semplice sul disco dinamico specificato. |
| [comando crea mirror del volume](create-volume-mirror.md) | Crea un mirror di volume utilizzando due dischi dinamici specificati. |
| [comando Crea volume RAID](create-volume-raid.md) | Consente di creare un volume RAID-5 utilizzando tre o più dischi dinamici specificati. |
| [Crea comando Stripe volume](create-volume-stripe.md) | Crea un volume con striping utilizzando due o più dischi dinamici specificati. |

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)
