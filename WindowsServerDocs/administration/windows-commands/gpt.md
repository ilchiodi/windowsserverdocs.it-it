---
title: GPT
description: Argomento dei comandi di Windows per * * * *-
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 1d6f9029-807f-4420-a336-36669b5361bc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 1db2a3fe21b0343c23bbc2432d4d07a500b72e8e
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80842454"
---
# <a name="gpt"></a>GPT

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Nei dischi di base della tabella di partizione GUID (GPT), assegna gli attributi GPT alla partizione con lo stato attivo.  gli attributi di partizione GPT forniscono informazioni aggiuntive sull'utilizzo della partizione. Alcuni attributi sono specifici per il tipo di partizione GUID.

> [!CAUTION]
> Se si modificano gli attributi GPT, è possibile che i volumi di dati di base non vengano assegnati a lettere di unità o per impedire il montaggio del file system. Non modificare gli attributi GPT a meno che non si sia un produttore OEM (Original Equipment Manufacturer) o un professionista IT esperto di dischi GPT.

## <a name="syntax"></a>Sintassi

```
gpt attributes=<n>
```

### <a name="parameters"></a>Parametri

|   Parametro    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               Descrizione                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
|----------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| attributi =<n> | Specifica il valore dell'attributo che si desidera applicare alla partizione con lo stato attivo. Il campo dell'attributo GPT è un campo a 64 bit che contiene due sottocampi. Il campo superiore viene interpretato solo nel contesto dell'ID di partizione, mentre il campo inferiore è comune a tutti gli ID di partizione. I valori consentiti includono:<p>-   **0x0000000000000001**. Specifica che la partizione è necessario per il computer per funzionare correttamente.<br />-   **0x8000000000000000**. Specifica che la partizione non riceveranno una lettera di unità per impostazione predefinita quando il disco viene spostato in un altro computer o quando il disco viene visualizzato per la prima volta da un computer.<br />-   **0x4000000000000000**. Nasconde il volume della partizione. Ovvero, la partizione non verrà rilevata dal gestore di montaggio.<br />-   **0x2000000000000000**. Specifica che la partizione è una copia shadow di un'altra partizione.<br />-   **0x1000000000000000**. Specifica che la partizione è di sola lettura. Questo attributo impedisce che il volume su cui scrivere.<p>Per ulteriori informazioni su questi attributi, vedere la sezione relativa agli attributi in [Create_PARTITION_PARAMETERS struttura](https://go.microsoft.com/fwlink/?LinkId=203812). |

## <a name="remarks"></a>Note

- La partizione di sistema EFI contiene solo i file binari necessari per avviare il sistema operativo. Questo rende facile per i file binari OEM o specifico di file binari in un sistema operativo per essere inserita in altre partizioni.
- Per eseguire questa operazione, è necessario selezionare una partizione GPT di base. Usare il comando **select partition** per selezionare una partizione GPT di base e spostare lo stato attivo su di essa.

## <a name="examples"></a><a name=BKMK_examples></a>Esempi

  Se si sta spostando un disco GPT in un nuovo computer e si desidera impedire a tale computer di assegnare automaticamente una lettera di unità alla partizione con lo stato attivo, digitare:
  ```
  gpt attributes=0x8000000000000000
  ```
