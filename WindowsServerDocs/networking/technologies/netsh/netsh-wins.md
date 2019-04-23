---
title: File batch di esempio della shell di rete (Netsh)
description: È possibile utilizzare questo argomento per informazioni su come creare un file batch che esegua più operazioni tramite Netsh in Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: c94e37a4-3637-4613-9eb5-ed604e831eca
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b0528cfaef201ba30e00e30f56a763be39a6b828
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880172"
---
# <a name="network-shell-netsh-example-batch-file"></a>Shell di rete \(Netsh\) File Batch di esempio

Si applica a: Windows Server 2016

È possibile utilizzare questo argomento per informazioni su come creare un file batch che esegue più attività tramite Netsh in Windows Server 2016. In questo file batch di esempio, il **netsh wins** contesto viene utilizzato.

## <a name="example-batch-file-overview"></a>Panoramica di File Batch di esempio

È possibile usare i comandi Netsh per Windows Internet Name Service \(WINS\) in altri script per automatizzare le attività e file batch. L'esempio di file batch seguente viene illustrato come usare i comandi Netsh per WINS per eseguire una serie di attività correlate.

In questo file batch di esempio, prevale\-è un server WINS con l'indirizzo IP 192.168.125.30 e WINS\-B è un server WINS con l'indirizzo IP 192.168.0.189.

Il file batch riportato di seguito viene illustrato come eseguire le attività seguenti.

- Aggiunge un record dei nomi dinamico con l'indirizzo IP 192.168.0.205, MY\_RECORD \[04h\], in WINS\-A
- Imposta WINS\-B come partner di replica push/pull di WINS\-A
- Si connette a WINS\-B, quindi imposta WINS\-come partner di replica push/pull di WINS\-B
- Avvia una replica push da WINS\-A WINS\-B
- Si connette a WINS\-B per verificare che il nuovo record, MY\_RECORD, sono stati replicati correttamente

## <a name="netsh-example-batch-file"></a>File batch di esempio Netsh

Nel file batch di esempio seguente, le righe che contengono i commenti sono precedute da "rem," per osservazione. Commenti vengono ignorati.

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

## <a name="netsh-wins-commands-used-in-the-example-batch-file"></a>Comandi Netsh per WINS utilizzati nel file batch di esempio

La sezione seguente elenca i **netsh wins** comandi usati in questa procedura di esempio.

- **server**. Sposta il comando corrente WINS\-contesto di riga nel server specificato tramite il nome o indirizzo IP.
- **Aggiungi nome**. Registra un nome del server WINS.
- **aggiungere partner**. Aggiunge un partner di replica nel server WINS.
- **push init**. Avvia e invia un trigger di push a un server WINS.
- **Mostra nomi**. Visualizza informazioni dettagliate per un record specifico nel database del server WINS.  

Per altre informazioni, vedere [Network Shell (Netsh)](netsh.md).
