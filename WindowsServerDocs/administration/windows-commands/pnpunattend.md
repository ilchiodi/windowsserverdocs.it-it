---
title: pnpunattend
description: Informazioni su come controllare i driver di dispositivo in un computer, nonché di eseguire installazioni driver invisibile all'utente.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-windows-commands
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4fa88932-cff0-4dfc-936c-98c0e3dfbeb8 britw
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 07/11/2018
ms.openlocfilehash: 7aa09636f8c23678f003bfa5ebf8be164e7fc683
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59879522"
---
# <a name="pnpunattend"></a>pnpunattend

Controlli di un computer per i driver di dispositivo e di eseguire installazioni automatiche di driver, o cercare i driver senza installare e, facoltativamente, riportare i risultati nella riga di comando. Usare questo comando per specificare l'installazione di driver specifici per i dispositivi hardware specifici. Vedi Osservazioni.

## <a name="syntax"></a>Sintassi

```
PnPUnattend.exe auditSystem [/help] [/?] [/h] [/s] [/L]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|auditSystem|Specifica l'installazione di driver in linea.</br>Obbligatorio, eccetto quando **pnpunattend** viene eseguito con il **/Help** o **/?** parametri.|
|/s|Facoltativo. Specifica la ricerca dei driver senza installare.|
|/L|Facoltativo. Specifica per visualizzare le informazioni del log per questo comando al prompt dei comandi.|
|/?|Facoltativo. Visualizza la Guida per questo comando al prompt dei comandi.|

## <a name="remarks"></a>Note

Preparazione preliminare è obbligatorio. Prima di usare questo comando, è necessario completare le attività seguenti:

1.  Creare una directory per i driver da installare. Ad esempio, creare una cartella nel **C:\Drivers\Video** per i driver della scheda video.
2.  Scaricare ed estrarre il pacchetto di driver per il dispositivo. Copiare il contenuto della sottocartella contenente il file INF per la versione del sistema operativo ed eventuali sottocartelle nella cartella video che è stato creato. Ad esempio, copiare i file di driver video C:\Drivers\Video.
3.  Aggiungere una variabile di ambiente path di sistema per la cartella creata nel passaggio 1. ad esempio, **C:\Drivers\Video**.
4.  Creare la seguente chiave del Registro di sistema, quindi per il **DriverPaths** chiave create, impostare il **dati valore** a **1**.
-   Per Windows® 7 passare al percorso del Registro di sistema: **su HKEY_LOCAL_Machine\Software\Microsoft\Windows NT\CurrentVersion.\**, quindi creare le chiavi: **UnattendSettings\PnPUnattend\DriverPaths\**
-   Per Windows Vista, passare al percorso del Registro di sistema: **HK_LM\Software\Microsoft\Windows NT\CurrentVersion.\**, quindi creare le chiavi = **\UnattendSettings\PnPUnattend\DriverPaths** .

## <a name="examples"></a>Esempi

Il comando seguente viene illustrato come utilizzare il **PNPUnattend.exe** per controllare un computer per gli aggiornamenti dei driver possibili e quindi segnalare i risultati al prompt dei comandi.

```
pnpunattend auditsystem /s /l 
```

## <a name="additional-references"></a>Altri riferimenti

[Chiave sintassi della riga di comando](command-line-syntax-key.md)