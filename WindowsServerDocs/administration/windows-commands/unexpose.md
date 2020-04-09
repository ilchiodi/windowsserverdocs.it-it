---
title: volume x:.
description: Windows Commands Topic for unexpoe, che non espone una copia shadow esposta tramite il comando Expose.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 58dc7d0f-52e9-4587-9487-d3b4c3e52640
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: f2f8bbdb3b810ffbf9332608a016fc3b3e188e9f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80832354"
---
# <a name="unexpose"></a>volume x:.

Unexposes una copia shadow esposta tramite il **esporre** comando. La copia shadow esposta può essere specificata dal relativo ID Shadow, lettera di unità, una condivisione o un punto di montaggio.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
unexpose {<ShadowID> | <Drive:> | <Share> | <MountPoint>}
```

### <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|\<IDShadow >|Unexposes la copia shadow specificata per l'ID di Shadow specificato.|
|Unità \<: >|Non espone la copia shadow associata alla lettera di unità specificata (ad esempio, l'unità P).|
|Condivisione \<>|Non espone la copia shadow associata alla condivisione specificata, ad esempio \\\\*MachineName*\).|
|\<MountPoint >|Non espone la copia shadow associata al punto di montaggio specificato, ad esempio C:\shadowcopy\).|

## <a name="remarks"></a>Note

-   È possibile utilizzare un alias esistente o una variabile di ambiente al posto di *IDShadow*. Utilizzare **aggiungere** senza parametri per visualizzare gli alias esistenti.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

Per annullare l'esposizione della copia shadow associata P unità, digitare:
```
unexpose P:
```

## <a name="additional-references"></a>Altre informazioni di riferimento

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)