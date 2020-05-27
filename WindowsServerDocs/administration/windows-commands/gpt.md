---
title: GPT
description: Argomento di riferimento per il comando GPT, che assegna gli attributi GPT alla partizione con lo stato attivo.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1d6f9029-807f-4420-a336-36669b5361bc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: b1e33b89c1918fcb83dd9d42c155f845805307d9
ms.sourcegitcommit: 4f407b82435afe3111c215510b0ef797863f9cb4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/24/2020
ms.locfileid: "83818801"
---
# <a name="gpt"></a>GPT

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Nei dischi di base della tabella di partizione GUID (GPT), questo comando assegna gli attributi GPT alla partizione con lo stato attivo. Gli attributi di partizione GPT forniscono informazioni aggiuntive sull'utilizzo della partizione. Alcuni attributi sono specifici per il tipo di partizione GUID.

Per eseguire questa operazione, è necessario scegliere una partizione GPT di base. Usare il [comando select partition](select-partition.md) per selezionare una partizione GPT di base e spostare lo stato attivo su di essa.

> [!CAUTION]
> Se si modificano gli attributi GPT, è possibile che i volumi di dati di base non vengano assegnati a lettere di unità o per impedire il montaggio del file system. Si consiglia vivamente di non modificare gli attributi GPT, a meno che non si sia un produttore OEM (Original Equipment Manufacturer) o un professionista IT esperto di dischi GPT.

## <a name="syntax"></a>Sintassi

```
gpt attributes=<n>
```

### <a name="parameters"></a>Parametri

| Parametro | Descrizione |
| --------- | ----------- |
| attributi =`<n>` | Specifica il valore dell'attributo che si desidera applicare alla partizione con lo stato attivo. Il campo dell'attributo GPT è un campo a 64 bit che contiene due sottocampi. Il campo superiore viene interpretato solo nel contesto dell'ID di partizione, mentre il campo inferiore è comune a tutti gli ID di partizione. I valori consentiti includono:<ul><li>**0x0000000000000001** : specifica che la partizione è necessaria per il corretto funzionamento del computer.</li><li>**0x8000000000000000** : specifica che la partizione non riceverà una lettera di unità per impostazione predefinita quando il disco viene spostato in un altro computer o quando il disco viene visualizzato per la prima volta da un computer.</li><li>**0x4000000000000000** : nasconde il volume di una partizione in modo che non venga rilevato dal gestore di montaggio.</li><li>**0x2000000000000000** : specifica che la partizione è una copia shadow di un'altra partizione.</li><li>**0x1000000000000000** -specifica che la partizione è di sola lettura. Questo attributo impedisce che il volume su cui scrivere.</li></ul><p>Per ulteriori informazioni su questi attributi, vedere la sezione relativa agli attributi in [Create_PARTITION_PARAMETERS struttura](https://docs.microsoft.com/windows/win32/api/vds/ns-vds-create_partition_parameters). |

#### <a name="remarks"></a>Osservazioni

- La partizione di sistema EFI contiene solo i file binari necessari per avviare il sistema operativo. Questo rende facile per i file binari OEM o specifico di file binari in un sistema operativo per essere inserita in altre partizioni.

### <a name="examples"></a>Esempi

Per evitare che il computer assegni automaticamente una lettera di unità alla partizione con lo stato attivo, mentre si sposta un disco GPT, digitare:

```
gpt attributes=0x8000000000000000
```

## <a name="additional-references"></a>Riferimenti aggiuntivi

- [Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)

- [comando select partition](select-partition.md)

- [Struttura create_PARTITION_PARAMETERS](https://docs.microsoft.com/windows/win32/api/vds/ns-vds-create_partition_parameters)
