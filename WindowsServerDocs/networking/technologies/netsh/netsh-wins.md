---
title: File Batch di esempio di Network Shell (Netsh)
description: È possibile utilizzare questo argomento per informazioni su come creare un file batch che esegue più attività di utilizzo di Netsh in Windows Server 2016.
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: c94e37a4-3637-4613-9eb5-ed604e831eca
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: b0528cfaef201ba30e00e30f56a763be39a6b828
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="network-shell-netsh-example-batch-file"></a>File Batch di esempio \(Netsh\) Shell di rete

Si applica a: Windows Server 2016

È possibile utilizzare questo argomento per informazioni su come creare un file batch che esegue più attività con Netsh in Windows Server 2016. In questo file batch di esempio, il **netsh wins** contesto viene utilizzato.

## <a name="example-batch-file-overview"></a>Panoramica di File Batch di esempio

È possibile utilizzare i comandi Netsh per Windows Internet Name Service \(WINS\) nei file batch e altri script per automatizzare le attività. L'esempio di file batch seguente illustra come usare i comandi Netsh per WINS per eseguire diverse attività correlate.

In questo file batch di esempio, WINS\-A è un server WINS con l'indirizzo IP 192.168.125.30 e WINS\-B è un server WINS con l'indirizzo IP 192.168.0.189.

Il file batch di esempio esegue le attività seguenti.

- Aggiunge un record di nome dinamico con indirizzo IP 192.168.0.205, MY\_RECORD \[04h\], a WINS\-A
- Imposta WINS\-B come partner di replica push o pull WINS\ a
- Si connette a WINS\-B e quindi imposta WINS\-A come partner di replica push o pull di WINS\ B
- Avvia una replica push da WINS\ A WINS\ b
- Si connette a WINS\-B per verificare che il nuovo record, MY\_RECORD, è stato replicato correttamente

## <a name="netsh-example-batch-file"></a>Netsh file batch di esempio

Nel file batch di esempio seguenti, le righe che contengono i commenti sono precedute da "rem", per osservazione. Commenti vengono ignorati.

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

Il comando seguente sezione elenchi di **netsh wins** comandi utilizzati in questa procedura di esempio.

- **server**. Passa il contesto di riga di comando WINS corrente per il server specificato dal nome o indirizzo IP.
- **Aggiungere nome**. Registra un nome del server WINS.
- **aggiunta di partner**. Aggiunge un partner di replica al server WINS.
- **inizializzazione push**. Avvia e invia un trigger di push a un server WINS.
- **nome visualizzato**. Visualizza informazioni dettagliate per un particolare record nel database del server WINS.  

Per ulteriori informazioni, vedere [Network Shell (Netsh)](netsh.md).
