---
title: gpt
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 1d6f9029-807f-4420-a336-36669b5361bc
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: cba9839f98dfd5a72289838273a057dd0e09a7e5
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66438186"
---
# <a name="gpt"></a>gpt

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Nei dischi di base GUID partizione gpt (tabella), assegna gli attributi gpt alla partizione con lo stato attivo.  Questi attributi forniscono informazioni aggiuntive sull'utilizzo della partizione. Alcuni attributi sono specifici per il tipo di partizione GUID.

> [!CAUTION]
> Modifica degli attributi gpt potrebbe causare dei volumi di dati di base per un errore da assegnare lettere di unità, o per impedire il montaggio del file system. Non è necessario modificare gli attributi gpt a meno che non si è un original equipment manufacturer (OEM) o un professionista IT esperto dischi gpt.
> ## <a name="syntax"></a>Sintassi
> ```
> gpt attributes=<n>
> ```
> ## <a name="parameters"></a>Parametri
> 
> |   Parametro    |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               Descrizione                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
> |----------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
> | attributes=<n> | Specifica il valore dell'attributo che si desidera applicare alla partizione con lo stato attivo. Nel campo dell'attributo gpt è un campo a 64 bit che contiene due campi secondari. Il campo superiore viene interpretato solo nel contesto dell'ID di partizione, mentre il campo inferiore è comune a tutti gli ID di partizione. I valori consentiti includono:<br /><br />-   **0x0000000000000001**. Specifica che la partizione è necessario per il computer per funzionare correttamente.<br />-   **0x8000000000000000**. Specifica che la partizione non riceveranno una lettera di unità per impostazione predefinita quando il disco viene spostato in un altro computer o quando il disco viene visualizzato per la prima volta da un computer.<br />-   **0x4000000000000000**. Nasconde il volume della partizione. Vale a dire, non verrà rilevata la partizione dal gestore di montaggio.<br />-   **0x2000000000000000**. Specifica che la partizione è una copia shadow di un'altra partizione.<br />-   **0x1000000000000000**. Specifica che la partizione è di sola lettura. Questo attributo impedisce che il volume su cui scrivere.<br /><b />per altre informazioni su questi attributi, vedere la sezione attributi in [create_PARTITION_PARAMETERS struttura](https://go.microsoft.com/fwlink/?LinkId=203812) (<https://go.microsoft.com/fwlink/?LinkId=203812>). |
> 
> ## <a name="remarks"></a>Note
> - La partizione di sistema EFI contiene solo i file binari necessari per avviare il sistema operativo. Questo rende facile per i file binari OEM o specifico di file binari in un sistema operativo per essere inserita in altre partizioni.
> - Per eseguire questa operazione, è necessario selezionare una partizione gpt di base. Usare la **Seleziona partizione** comando per selezionare una partizione gpt di base e spostare lo stato attivo a esso.
>   ## <a name="BKMK_examples"></a>Esempi
>   Se si sta spostando un disco gpt per un nuovo computer e si vuole impedire l'assegnazione automatica di una lettera di unità alla partizione con lo stato attivo, tipo di tale computer:
>   ```
>   gpt attributes=0x8000000000000000
>   ```

