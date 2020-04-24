---
title: Modificare un disco MBR (Master Boot Record, record di avvio principale) in disco GPT (GUID Partition Table, tabella delle partizioni GUID)
description: Descrive come modificare un disco MBR (Master Boot Record, record di avvio principale) in disco GPT (GUID Partition Table, tabella delle partizioni GUID)
ms.date: 06/07/2019
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 6bd97802fbef342520e92a857a1a53acf3e8d7a3
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "71385945"
---
# <a name="convert-an-mbr-disk-into-a-gpt-disk"></a>Convertire un disco MBR in disco GPT

> **Si applica a:** Windows 10, Windows 8.1, Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

I dischi MBR (Master Boot Record, record di avvio principale) usano la tabella delle partizioni BIOS standard. I dischi GPT (GUID Partition Table, tabella delle partizioni GUID) usano UEFI (Unified Extensible Firmware Interface). Uno dei vantaggi dei dischi GPT è la possibilità di avere più di quattro partizioni in ogni disco. Lo stile di partizione GPT è anche necessario per i dischi con dimensioni maggiori di due terabyte (TB).

Puoi modificare un disco dallo stile di partizione MBR a GPT, purché non contenga partizioni o volumi.

> [!NOTE]
> Prima di convertire un disco, esegui il backup di tutti i dati che contiene e chiudi tutti i programmi che vi accedono.

> [!NOTE]
> Per completare questi passaggi, devi essere un membro del gruppo **Backup Operators** o **Administrators**.

## <a name="converting-using-the-windows-interface"></a>Conversione tramite l'interfaccia di Windows

1.  Esegui il backup o sposta i dati sul disco MBR di base che vuoi convertire in disco GPT.

2.  Se il disco contiene partizioni o volumi, fai clic con il pulsante destro del mouse su ciascuno e quindi scegli **Elimina partizione** o **Elimina volume**.

3.  Fai clic con il pulsante destro del mouse sul disco MBR che vuoi convertire in disco GPT e quindi scegli **Converti in disco GPT**.

## <a name="converting-using-a-command-line"></a>Conversione tramite la riga di comando

Usa la procedura seguente per convertire un disco MBR vuoto in disco GPT. Puoi anche usare lo strumento MBR2GPT.EXE, ma è piuttosto complesso. Per informazioni dettagliate, vedi [Convertire una partizione MBR in GPT](https://docs.microsoft.com/windows/deployment/mbr-to-gpt).

1.  Esegui il backup o sposta i dati sul disco MBR di base che vuoi convertire in disco GPT.

2.  Apri un prompt dei comandi con privilegi elevati facendo clic con il pulsante destro del mouse su **Prompt dei comandi** e quindi scegliendo **Esegui come amministratore**.

3. Digitare `diskpart`. Se il disco non contiene partizioni o volumi, vai al passaggio 6.

4.  Al prompt **DISKPART** digita `list disk`. Prendi nota del numero del disco che vuoi convertire.

5.  Al prompt **DISKPART** digita `select disk <disknumber>`.

6.  Al prompt **DISKPART** digita `clean`.

    > [!NOTE]
    > L'esecuzione del comando **clean** eliminerà tutti i volumi e le partizioni sul disco.

7.  Al prompt **DISKPART** digita `convert gpt`.

| Valore  | Descrizione  |
| ----- | ---- |
| **list disk** | Visualizza un elenco di dischi e le relative informazioni, tra cui dimensioni, quantità di spazio libero, se il disco è un disco di base o dinamico e se il disco usa lo stile di partizione MBR (Master Boot Record, record di avvio principale) o GPT (GUID Partition Table, tabella delle partizioni GUID). Il disco contrassegnato con un asterisco (*) ha lo stato attivo. |
| **select disk** *disknumber* | Seleziona il disco specificato, dove *disknumber* è il numero del disco, assegnando lo stato attivo a questo disco. |
| **clean** | Rimuove tutti i volumi o le partizioni dal disco con lo stato attivo.  |
| **convert gpt**| Converte un disco di base vuoto con lo stile di partizione MBR in disco di base con lo stile di partizione GPT. |

## <a name="see-also"></a>Vedi anche

-   [Notazione della sintassi della riga di comando](https://technet.microsoft.com/library/cc742449(v=ws.11).aspx)