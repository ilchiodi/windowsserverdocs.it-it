---
title: mfmedia WinSAT
description: Informazioni di riferimento per WinSAT mfmedia, che consente di misurare le prestazioni della decodifica video (riproduzione) utilizzando il Framework Media Foundation.
ms.prod: windows-server
ms.technology: manage-windows-commands
ms.topic: article
ms.assetid: 09a3b3dd-f746-4e6e-b684-76a9bde0c78d
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: 3a03304f4df27dc7fdf9bb0af5e2f4d6f2b9c98d
ms.sourcegitcommit: ab64dc83fca28039416c26226815502d0193500c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/01/2020
ms.locfileid: "82720656"
---
# <a name="winsat-mfmedia"></a>mfmedia WinSAT



Misura le prestazioni della decodifica video (riproduzione) utilizzando il Framework Media Foundation.



## <a name="syntax"></a>Sintassi

```
winsat mfmedia <parameters>
```

### <a name="parameters"></a>Parametri

|Parametri|Descrizione|
|----------|-----------|
|-nome \<file di input>|Obbligatorio: specificare il file contenente il clip video da riprodurre o codificare. Il file può essere in qualsiasi formato di cui è possibile eseguire il rendering Media Foundation.|
|-dumpgraph|Consente di specificare che il grafico del filtro deve essere salvato in un file compatibile con GraphEdit prima dell'avvio della valutazione.|
|-NS|Consente di specificare che il grafico del filtro deve essere eseguito con la normale velocità di riproduzione del file di input. Per impostazione predefinita, il grafico del filtro viene eseguito il più rapidamente possibile, ignorando i tempi di presentazione.|
|-Play|Eseguire la valutazione in modalità decodifica e riprodurre qualsiasi contenuto audio fornito nel file specificato in **-input** usando il dispositivo DirectSound predefinito. Per impostazione predefinita, la riproduzione audio è disabilitata.|
|-nopmp|Non usare il processo MFPMP (Protected Media pipeline) Media Foundation durante la valutazione.|
|-PMP|Usare sempre il processo MFPMP durante la valutazione.</br>Nota: se non si specifica **-PMP** o **-nopmp** , MFPMP verrà usato solo quando necessario.|
|-v|Inviare l'output dettagliato a STDOUT, incluse le informazioni sullo stato e sullo stato di avanzamento. Eventuali errori verranno anche scritti nella finestra di comando.|
|-nome \<file XML>|Salva l'output della valutazione come file XML specificato. Se il file specificato esiste, verrà sovrascritto.|
|-idiskinfo|Salvare le informazioni sui volumi fisici e i ** \<** dischi logici come parte della sezione SystemConfig>nell'output XML.|
|-iguid|Creare un identificatore univoco globale (GUID) nel file di output XML.|
|-Nota testo|Aggiungere il testo della nota alla sezione ** \<nota>** nel file di output XML.|
|-ICN|Includere il nome del computer locale nel file di output XML.|
|-Eef|Enumerare informazioni aggiuntive sul sistema nel file di output XML.|

## <a name="examples"></a>Esempi

- Per eseguire la valutazione con il file di input usato durante una valutazione **formale di WinSAT** , senza usare la pipeline di supporti Media Foundation Protected Media (MFPMP), in un computer in cui c:\Windows è il percorso della cartella di Windows.  
  ```
  winsat mfmedia -input c:\windows\performance\winsat\winsat.wmv -nopmp
  ```

## <a name="remarks"></a>Osservazioni

-   L'appartenenza al gruppo Administrators locale o a un gruppo equivalente è il requisito minimo necessario per usare **WinSAT**. Il comando deve essere eseguito da una finestra del prompt dei comandi con privilegi elevati.
-   Per aprire una finestra del prompt dei comandi con privilegi elevati, fare clic su **Start**, **Accessori**, fare clic con il pulsante destro del mouse su **prompt dei comandi**e scegliere **Esegui come amministratore**.

## <a name="additional-references"></a>Riferimenti aggiuntivi

