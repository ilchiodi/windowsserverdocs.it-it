---
title: WinSAT mfmedia
description: Comandi di Windows
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 09a3b3dd-f746-4e6e-b684-76a9bde0c78d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 2c63682e474311a49b01dc8078b023547e1fb170
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66440018"
---
# <a name="winsat-mfmedia"></a>WinSAT mfmedia



Misura le prestazioni di video decodifica (riproduzione) usando il framework di Media Foundation.

Per esempi di utilizzo di questo comando, vedere [Esempi](#BKMK_examples).

## <a name="syntax"></a>Sintassi

```
winsat mfmedia <parameters>
```

## <a name="parameters"></a>Parametri

|Parametri|Descrizione|
|----------|-----------|
|-input \<nome file >|Obbligatorio: Specificare il file contenente la clip video da riprodurre o codificato. Il file può essere in qualsiasi formato che può essere sottoposto a rendering da Media Foundation.|
|-dumpgraph|Specificare che il grafico di filtro deve essere salvato in un file compatibile con GraphEdit prima dell'avvio della valutazione.|
|-ns|Specificare che il grafico di filtro deve essere eseguito con la velocità di riproduzione normale del file di input. Per impostazione predefinita, il grafico di filtro viene eseguito più velocemente possibile, ignorando i tempi di presentazione.|
|-play|Esegui la valutazione in decodificare la modalità e qualsiasi fornito audio contenuto nel file specificato play **-input** usando il dispositivo DirectSound predefinito. Per impostazione predefinita, la riproduzione audio è disabilitata.|
|-nopmp|Si avvale di Media Foundation protetti supporti Pipeline (MFPMP) processo durante la valutazione.|
|-pmp|Assicurarsi sempre di utilizzare il MFPMP processo durante la valutazione.</br>Nota: Se **- pmp** oppure **- nopmp** non viene specificato, verrà utilizzato MFPMP solo quando necessario.|
|-v|Inviare un output dettagliato a STDOUT, incluse le informazioni di stato e lo stato di avanzamento. Anche gli eventuali errori verranno scritto nella finestra di comando.|
|xml - \<nome file >|Salvare l'output della valutazione come file XML specificato. Se il file specificato esiste, verrà sovrascritto.|
|-idiskinfo|Salvare le informazioni sui volumi fisici e i dischi logici come parte del  **\<SystemConfig >** sezione nell'output XML.|
|-iguid|Creare un identificatore univoco globale (GUID) nel file di output XML.|
|-Si noti "testo nota"|Aggiungere il testo della nota per il  **\<nota >** sezione nel file di output XML.|
|-icn|Includere il nome del computer locale nel file di output XML.|
|-eef|Enumerare le informazioni di sistema aggiuntivi nel file di output XML.|

## <a name="BKMK_examples"></a>Esempi

- Nell'esempio seguente viene eseguita la valutazione con il file di input che viene usato durante una **winsat formale** valutazione, senza uso di Media Foundation protetti supporti Pipeline (MFPMP), in un computer in cui c:\windows è il percorso del la cartella Windows.  
  ```
  winsat mfmedia -input c:\windows\performance\winsat\winsat.wmv -nopmp
  ```

## <a name="remarks"></a>Note

-   L'appartenenza al gruppo Administrators locale o equivalente è il requisito minimo per usare **winsat**. Il comando deve essere eseguito da una finestra del prompt dei comandi con privilegi elevati.
-   Per aprire una finestra del prompt dei comandi con privilegi elevati, fare clic su **avviare**, fare clic su **Accessori**, fare doppio clic su **prompt dei comandi**, fare clic su **Esegui come amministratore**.

#### <a name="additional-references"></a>Altri riferimenti

