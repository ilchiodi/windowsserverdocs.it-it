---
title: WinSAT ottimizzato
description: 'Argomento i comandi di Windows per * * *- '
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: cda017bf-6193-43c1-b71f-e379c23e1152
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 7cdae81694a916905f36cdd9e941015e3ce5f15c
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440085"
---
# <a name="winsat-mem"></a>WinSAT ottimizzato



Test del sistema della memoria della larghezza di banda in modo riflettente di copie di buffer di memoria per la memoria di grandi dimensioni, poiché vengono usate nell'elaborazione multimedia.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
winsat mem <parameters>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|-up|Forzare il test con un solo thread della memoria. L'impostazione predefinita consiste nell'eseguire un thread per ogni fisica della CPU o core.|
|-rn|Specificare che i thread di valutazione deve essere eseguito con priorità normale. Per impostazione predefinita viene eseguito con priorità 15.|
|-nc|Specificare che la valutazione deve allocare la memoria e contrassegnarlo come non memorizzata nella cache. Ciò significa che le cache del processore verranno eseguito il bypass per operazioni di copia. Per impostazione predefinita viene eseguito nello spazio memorizzati nella cache.|
|-do \<n>|Specificare la distanza, espressa in byte, tra la fine del buffer di origine e l'inizio del buffer di destinazione. Il valore predefinito è 64 byte. L'offset di destinazione consentita massima è 16MB. Specificando un offset di destinazione non valida. verrà generato un errore.</br>Nota: Zero è un valore valido per  **\<n >** , ma non sono numeri negativi.|
|-mint \<n>|Specificare il valore minimo in secondi per la valutazione della fase di esecuzione. Il valore predefinito è 2.0. Il valore minimo è 1.0. Il valore massimo è 30,0.</br>Nota: Specifica un **-mint** valore maggiore di **- maxt** valore quando i due parametri vengono utilizzati in combinazione comporterà un errore.|
|-maxt \<n>|Specificare il massimo tempo di esecuzione in secondi per la valutazione. Il valore predefinito è la 5.0. Il valore minimo è 1.0. Il valore massimo è 30,0. Se usato in combinazione con il **-mint** parametro, la valutazione inizierà a eseguire controlli statistici periodici i risultati al termine del periodo di tempo specificato in **-mint**. Se i controlli statistici passare, quindi la valutazione verrà terminata prima del periodo di tempo specificato in **- maxt** è trascorso. Se viene eseguita la valutazione per il periodo di tempo specificato nel **- maxt** senza che soddisfano i controlli di statistici e quindi la valutazione fine in quel momento e verrà restituiti i risultati della raccolta.|
|-buffersize \<n >|Specificare le dimensioni del buffer che deve usare il test di copia della memoria. Due volte questa quantità verrà allocata per ogni CPU, che determina la quantità di dati copiati da un buffer a un altro. Il valore predefinito è 16MB. Questo valore viene arrotondato al limite di 4 KB più vicino. Il valore massimo è 32MB. Il valore minimo è 4 KB. Specificando una dimensione del buffer non valido verrà generato un errore.|
|-v|Inviare un output dettagliato a STDOUT, incluse le informazioni di stato e lo stato di avanzamento. Anche gli eventuali errori verranno scritto nella finestra di comando.|
|xml - \<nome file >|Salvare l'output della valutazione come file XML specificato. Se il file specificato esiste, verrà sovrascritto.|
|-idiskinfo|Salvare le informazioni sui volumi fisici e i dischi logici come parte del  **\<SystemConfig >** sezione nell'output XML.|
|-iguid|Creare un identificatore univoco globale (GUID) nel file di output XML.|
|-Si noti "testo nota"|Aggiungere il testo della nota per il  **\<nota >** sezione nel file di output XML.|
|-icn|Includere il nome del computer locale nel file di output XML.|
|-eef|Enumerare le informazioni di sistema aggiuntivi nel file di output XML.|

## <a name="BKMK_examples"></a>Esempi

- Nell'esempio seguente viene eseguita la valutazione per un minimo di 4 secondi e non più di 12 secondi, usando una dimensione del buffer 32MB e salvataggio dei risultati in formato XML nel file **memtest.xml**.  
  ```
  winsat mem -mint 4.0 -maxt 12.0 -buffersize 32MB -xml memtest.xml
  ```

## <a name="remarks"></a>Note

-   L'appartenenza al gruppo Administrators locale o equivalente è il requisito minimo per usare **winsat**. Il comando deve essere eseguito da una finestra del prompt dei comandi con privilegi elevati.
-   Per aprire una finestra del prompt dei comandi con privilegi elevati, fare clic su **avviare**, fare clic su **Accessori**, fare doppio clic su **prompt dei comandi**, fare clic su **Esegui come amministratore**.

#### <a name="additional-references"></a>Altri riferimenti

