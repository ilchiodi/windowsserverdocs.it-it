---
title: pnpunattend
description: Informazioni su come controllare i driver di dispositivo in un computer, nonché eseguire installazioni di driver invisibile all'utente.
ms.custom: na
ms.prod: windows-server
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
ms.openlocfilehash: 77a6ab1ea45322e3c53e8b095c412cf8838be60d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71372273"
---
# <a name="pnpunattend"></a>pnpunattend

Controlla un computer per i driver di dispositivo ed esegue le installazioni automatiche dei driver o cerca i driver senza installare e, facoltativamente, segnalare i risultati alla riga di comando. Usare questo comando per specificare l'installazione di driver specifici per dispositivi hardware specifici. Vedi Osservazioni.

## <a name="syntax"></a>Sintassi

```
PnPUnattend.exe auditSystem [/help] [/?] [/h] [/s] [/L]
```

## <a name="parameters"></a>Parametri

|Parametro|Descrizione|
|---------|-----------|
|Passaggio auditSystem|Specifica l'installazione del driver online.</br>Obbligatorio, tranne quando **pnpunattend** viene eseguito con **/Help** o **/?** parametri.|
|/s|Facoltativo. Specifica la ricerca dei driver senza installare.|
|/L|Facoltativo. Specifica di visualizzare le informazioni di log per questo comando nel prompt dei comandi.|
|/?|Facoltativo. Visualizza la guida per questo comando al prompt dei comandi.|

## <a name="remarks"></a>Note

La preparazione preliminare è obbligatoria. Prima di utilizzare questo comando, è necessario completare le attività seguenti:

1. Creare una directory per i driver da installare. Ad esempio, creare una cartella in **C:\Drivers\Video** per i driver della scheda video.
2. Scaricare ed estrarre il pacchetto driver per il dispositivo. Copiare il contenuto della sottocartella che contiene il file INF per la versione del sistema operativo ed eventuali sottocartelle nella cartella video creata. Ad esempio, copiare i file del driver video in C:\Drivers\Video.
3. Aggiungere una variabile di percorso dell'ambiente di sistema alla cartella creata nel passaggio 1, ad esempio **C:\Drivers\Video**.
4. Creare la chiave del registro di sistema seguente e quindi, per la chiave **DriverPaths** creata, impostare i **dati del valore** su **1**.
5. Per Windows® 7 esplorare il percorso del registro di sistema: **HKEY_LOCAL_Machine\Software\Microsoft\Windows NT\CurrentVersion @ no__t-1**, quindi creare le chiavi: **UnattendSettings\PnPUnattend\DriverPaths @ no__t-1**
6. Per Windows Vista, passare al percorso del registro di sistema: **HK_LM\Software\Microsoft\Windows NT\CurrentVersion @ no__t-1**, quindi creare le chiavi = **\UnattendSettings\PnPUnattend\DriverPaths**.

## <a name="examples"></a>Esempi

Il comando di esempio seguente mostra come usare **PNPUnattend. exe** per controllare un computer per individuare possibili aggiornamenti dei driver, quindi segnalare i risultati al prompt dei comandi.

```
pnpunattend auditsystem /s /l 
```

## <a name="additional-references"></a>Altri riferimenti

[Indicazioni generali sulla sintassi della riga di comando](command-line-syntax-key.md)