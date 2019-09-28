---
title: WinSAT MEM
description: 'Argomento dei comandi di Windows per * * * *- '
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 23bbaf8551031a2e573b117e672a656213efde12
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361980"
---
# <a name="winsat-mem"></a>WinSAT MEM



Esegue il test della larghezza di banda della memoria di sistema in modo da riflettere le copie del buffer di memoria di grandi dimensioni in memoria, così come vengono usate nell'elaborazione

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
winsat mem <parameters>
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|-su|Forzare il test della memoria con un solo thread. L'impostazione predefinita prevede l'esecuzione di un thread per CPU o core fisico.|
|-RN|Consente di specificare che i thread della valutazione devono essere eseguiti con priorità normale. Per impostazione predefinita, viene eseguito con priorità 15.|
|-NC|Consente di specificare che la valutazione deve allocare memoria e contrassegnarla come non memorizzata nella cache. Ciò significa che le cache del processore verranno ignorate per le operazioni di copia. Per impostazione predefinita, l'esecuzione viene eseguita nello spazio memorizzato nella cache.|
|-do \<n >|Consente di specificare la distanza, in byte, tra la fine del buffer di origine e l'inizio del buffer di destinazione. Il valore predefinito è 64 byte. L'offset massimo consentito per la destinazione è 16 MB. Se si specifica un offset di destinazione non valido, verrà generato un errore.</br>Nota: Zero è un valore valido per  **\<n >** , ma i numeri negativi non lo sono.|
|-Mint \<n >|Consente di specificare il tempo di esecuzione minimo in secondi per la valutazione. Il valore predefinito è 2,0. Il valore minimo è 1,0. Il valore massimo è 30,0.</br>Nota: Se si specifica un valore **-Mint** maggiore del valore **-MaxT** quando i due parametri vengono utilizzati in combinazione, verrà generato un errore.|
|-MaxT \<n >|Consente di specificare il tempo di esecuzione massimo in secondi per la valutazione. Il valore predefinito è 5,0. Il valore minimo è 1,0. Il valore massimo è 30,0. Se usato in combinazione con il parametro **-Mint** , la valutazione inizierà a eseguire verifiche periodiche dei risultati statistici dopo il periodo di tempo specificato in **-Mint**. Se il controllo statistico viene superato, la valutazione viene completata prima che sia trascorso il periodo di tempo specificato in **-MaxT** . Se la valutazione viene eseguita per il periodo di tempo specificato in **-MaxT** senza soddisfare i controlli statistici, la valutazione verrà completata in quel momento e restituirà i risultati raccolti.|
|-buffersize \<n >|Specificare le dimensioni del buffer che devono essere utilizzate dal test di copia della memoria. Il doppio di questo importo verrà allocato per CPU, che determina la quantità di dati copiati da un buffer a un altro. Il valore predefinito è 16 MB. Questo valore viene arrotondato al limite di 4 KB più vicino. Il valore massimo è 32 MB. Il valore minimo è 4 KB. Se si specifica una dimensione del buffer non valida, verrà generato un errore.|
|-v|Inviare l'output dettagliato a STDOUT, incluse le informazioni sullo stato e sullo stato di avanzamento. Eventuali errori verranno anche scritti nella finestra di comando.|
|-nome \<file XML >|Salva l'output della valutazione come file XML specificato. Se il file specificato esiste, verrà sovrascritto.|
|-idiskinfo|Salvare le informazioni sui volumi fisici e i  **\<** dischi logici come parte della sezione SystemConfig > nell'output XML.|
|-iguid|Creare un identificatore univoco globale (GUID) nel file di output XML.|
|-Nota "testo della nota"|Aggiungere il testo della nota alla  **\<sezione Nota >** nel file di output XML.|
|-ICN|Includere il nome del computer locale nel file di output XML.|
|-Eef|Enumerare informazioni aggiuntive sul sistema nel file di output XML.|

## <a name="BKMK_examples"></a>Esempi

- Nell'esempio seguente viene eseguita la valutazione per un minimo di 4 secondi e non più di 12 secondi, utilizzando una dimensione del buffer di 32 MB e salvando i risultati in formato XML nel file **memtest. XML**.  
  ```
  winsat mem -mint 4.0 -maxt 12.0 -buffersize 32MB -xml memtest.xml
  ```

## <a name="remarks"></a>Note

-   L'appartenenza al gruppo Administrators locale o a un gruppo equivalente è il requisito minimo necessario per usare **WinSAT**. Il comando deve essere eseguito da una finestra del prompt dei comandi con privilegi elevati.
-   Per aprire una finestra del prompt dei comandi con privilegi elevati, fare clic su **Start**, **Accessori**, fare clic con il pulsante destro del mouse su **prompt dei comandi**e scegliere **Esegui come amministratore**.

#### <a name="additional-references"></a>Altri riferimenti

