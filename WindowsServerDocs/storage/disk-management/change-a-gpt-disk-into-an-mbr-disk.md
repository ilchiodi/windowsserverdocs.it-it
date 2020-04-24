---
title: Modificare un disco GPT (GUID Partition Table, tabella delle partizioni GUID) in disco MBR (Master Boot Record, record di avvio principale)
description: Descrive come modificare un disco con stile di partizione GPT (GUID Partition Table, tabella delle partizioni GUID) in disco con stile di partizione MBR (Master Boot Record, record di avvio principale).
ms.date: 06/19/2018
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 5c6efb0697af663b32ce6f0e27634c3962eca492
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "71402104"
---
# <a name="convert-a-gpt-disk-into-an-mbr-disk"></a>Convertire un disco GPT in disco MBR

> **Si applica a:** Windows 10, Windows 8.1, Windows Server (Canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

I dischi MBR (Master Boot Record, record di avvio principale) usano la tabella delle partizioni BIOS standard. I dischi GPT (GUID Partition Table, tabella delle partizioni GUID) usano UEFI (Unified Extensible Firmware Interface). I dischi MBR non supportano più di quattro partizioni in ogni disco. Il metodo di partizione MBR non è consigliato per i dischi con dimensioni maggiori di due terabyte (TB).

Puoi modificare un disco dallo stile di partizione GPT a MBR, purché il disco sia vuoto e non contenga volumi.

> [!NOTE]
> Prima di convertire un disco, esegui il backup di tutti i dati che contiene e chiudi tutti i programmi che vi accedono.

> [!NOTE]
> Per completare questi passaggi, devi essere un membro del gruppo **Backup Operators** o **Administrators**.

## <a name="converting-using-the-windows-interface"></a>Conversione tramite l'interfaccia di Windows

1.  Esegui il backup di tutti i volumi sul disco GPT di base che vuoi convertire in disco MBR.

2.  Se il disco contiene partizioni o volumi, fai clic con il pulsante destro del mouse su ciascuno e quindi scegli **Elimina volume**.

3.  Fai clic con il pulsante destro del mouse sul disco GPT che vuoi convertire in disco MBR e quindi scegli **Converti in disco MBR**.

## <a name="converting-using-a-command-line"></a>Conversione tramite la riga di comando

1.  Esegui il backup di tutti i volumi sul disco GPT di base che vuoi convertire in disco MBR.

2.  Apri un prompt dei comandi con privilegi elevati facendo clic con il pulsante destro del mouse su **Prompt dei comandi** e quindi scegliendo **Esegui come amministratore**.

3. Digitare `diskpart`. Se il disco non contiene partizioni o volumi, vai al passaggio 6.

4.  Al prompt **DISKPART** digita `list disk`. Prendi nota del numero del disco che vuoi eliminare.

5.  Al prompt **DISKPART** digita `select disk <disknumber>`.

6.  Al prompt **DISKPART** digita `clean`.

    > [!IMPORTANT]
    > L'esecuzione del comando **clean** eliminerà tutti i volumi e le partizioni sul disco.

7.  Al prompt **DISKPART** digita `convert mbr`.

|                Valore                  |      Descrizione   |
| ------------------------------------- | -----------------  |
|  <strong>list disk</strong>  | Visualizza un elenco di dischi e le relative informazioni, tra cui dimensioni, quantità di spazio libero, se il disco è un disco di base o dinamico e se il disco usa lo stile di partizione MBR (Master Boot Record, record di avvio principale) o GPT (GUID Partition Table, tabella delle partizioni GUID). Il disco contrassegnato con un asterisco (\*) ha lo stato attivo. |
| <strong>select disk</strong> |                                                                                                          Seleziona il disco specificato, dove <em>disknumber</em> è il numero del disco, assegnando lo stato attivo a questo disco.                                                                                                           |
| <strong>convert mbr</strong> |                                                                               Converte un disco di base vuoto con lo stile di partizione GPT in disco di base con lo stile di partizione MBR.                                                                                |

## <a name="see-also"></a>Vedi anche

-   [Notazione della sintassi della riga di comando](https://technet.microsoft.com/library/cc742449(v=ws.11).aspx)