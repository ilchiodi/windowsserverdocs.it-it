---
title: File batch di esempio della shell di rete (Netsh)
description: Questo argomento fornisce informazioni su come creare un file batch che esegua più attività usando Netsh in Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: c94e37a4-3637-4613-9eb5-ed604e831eca
manager: brianlic
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 73798ff4c8af11cc5cfb6461245ba7873f5d6f36
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80316720"
---
# <a name="network-shell-netsh-example-batch-file"></a>File batch di esempio della shell di rete \(Netsh\)

Si applica a: Windows Server 2016

Questo argomento fornisce informazioni su come creare un file batch che esegua più attività usando Netsh in Windows Server 2016. In questo file batch di esempio, viene usato il contesto **netsh wins**.

## <a name="example-batch-file-overview"></a>Panoramica del file batch di esempio

È possibile usare comandi Netsh per Windows Internet Name Service \(WINS\) in file batch e altri script per automatizzare le attività. Nell'esempio di file batch seguente viene illustrato come usare comandi Netsh per WINS per eseguire una serie di attività correlate.

In questo file batch di esempio, WINS\-A è un server WINS con indirizzo IP 192.168.125.30 e WINS\-B è un server WINS con indirizzo IP 192.168.0.189.

Il file batch di esempio esegue le attività seguenti.

- Aggiunge un record di nome dinamico con indirizzo IP 192.168.0.205, MY\_RECORD \[04h\], a WINS\-A
- Imposta WINS\-B come partner di replica push/pull di WINS\-A
- Si connette a WINS\-B, quindi imposta WINS\-A come partner di replica push/pull di WINS\-B
- Avvia una replica push da WINS\-A a WINS\-B
- Si connette a WINS\-B per verificare che il nuovo record, MY\_RECORD, sia stato replicato correttamente

## <a name="netsh-example-batch-file"></a>File batch di esempio di Netsh

Nel file batch di esempio seguente, le righe contenenti commenti sono precedute da "rem", che sta per "remark" (contrassegno). Netsh ignora i commenti.

    rem: Begin example batch file.
    
    rem two WINS servers:
    
    rem (WINS-A) 192.168.125.30
    
    rem (WINS-B) 192.168.0.189
    
    rem 1. Connect to (WINS-A), and add the dynamic name MY\_RECORD \[04h\] to the (WINS-A) database.
    
    netsh wins server 192.168.125.30 add name Name=MY\_RECORD EndChar=04 IP={192.168.0.205}
    
    rem 2. Connect to (WINS-A), and set (WINS-B) as a push/pull replication partner of (WINS-A).
    
    netsh wins server 192.168.125.30 add partner Server=192.168.0.189 Type=2
    
    rem 3. Connect to (WINS-B), and set (WINS-A) as a push/pull replication partner of (WINS-B).
    
    netsh wins server 192.168.0.189 add partner Server=192.168.125.30 Type=2
    
    rem 4. Connect back to (WINS-A), and initiate a push replication to (WINS-B).
    
    netsh wins server 192.168.125.30 init push Server=192.168.0.189 PropReq=0
    
    rem 5. Connect to (WINS-B), and check that the record MY\_RECORD \[04h\] was replicated successfully.
    
    netsh wins server 192.168.0.189 show name Name=MY\_RECORD EndChar=04
    
    rem 6. End example batch file.

## <a name="netsh-wins-commands-used-in-the-example-batch-file"></a>Comandi WINS di Netsh usati nel file batch di esempio

Nella sezione seguente sono elencati i comandi **netsh wins** usati in questa procedura di esempio.

- **server**. Sposta il contesto della riga di comando WINS corrente sul server specificato tramite il nome o l'indirizzo IP.
- **add name**. Registra un nome sul server WINS.
- **add partner**. Aggiunge un partner di replica nel server WINS.
- **init push**. Avvia e invia un trigger push a un server WINS.
- **show name**. Visualizza informazioni dettagliate su un determinato record nel database del server WINS.  

Per altre informazioni, vedere [Shell di rete (Netsh)](netsh.md).
