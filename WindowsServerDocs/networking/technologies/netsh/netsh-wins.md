---
title: File batch di esempio della shell di rete (Netsh)
description: È possibile usare questo argomento per informazioni su come creare un file batch che esegua più attività usando netsh in Windows Server 2016.
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: c94e37a4-3637-4613-9eb5-ed604e831eca
manager: brianlic
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 86fbe66978f7c09a332bba16a27a13fa029cb5a6
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71401913"
---
# <a name="network-shell-netsh-example-batch-file"></a>Shell di rete \(netsh\) file batch di esempio

Si applica a: Windows Server 2016

È possibile usare questo argomento per informazioni su come creare un file batch che esegua più attività usando netsh in Windows Server 2016. In questo file batch di esempio viene usato il contesto **Netsh WINS** .

## <a name="example-batch-file-overview"></a>Panoramica del file batch di esempio

È possibile utilizzare i comandi Netsh per Windows Internet Name Service \(WINS\) in file batch e altri script per automatizzare le attività. Nell'esempio di file batch riportato di seguito viene illustrato come utilizzare i comandi Netsh per WINS per eseguire una serie di attività correlate.

In questo file batch di esempio, WINS\-A è un server WINS con l'indirizzo IP 192.168.125.30 e WINS\-B è un server WINS con indirizzo IP 192.168.0.189.

Il file batch di esempio esegue le attività seguenti.

- Aggiunge un record di nome dinamico con indirizzo IP 192.168.0.205, MY\_RECORD \[04h\], a WINS\-
- Imposta WINS\-B come partner di replica push/pull di WINS\-A
- Si connette a WINS\-B, quindi imposta WINS\-A come partner di replica push/pull di WINS\-B
- Avvia una replica push da WINS\-a WINS\-B
- Si connette a WINS\-B per verificare che il nuovo record, MY\_RECORD, è stato replicato correttamente

## <a name="netsh-example-batch-file"></a>File batch di esempio netsh

Nel file batch di esempio seguente, le righe contenenti commenti sono precedute da "REM" per il contrassegno. Netsh ignora i commenti.

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

## <a name="netsh-wins-commands-used-in-the-example-batch-file"></a>Comandi Netsh WINS usati nel file batch di esempio

Nella sezione seguente sono elencati i comandi **Netsh WINS** utilizzati in questa procedura di esempio.

- **Server**. Sposta il comando WINS corrente\-contesto di riga al server specificato mediante il nome o l'indirizzo IP.
- **aggiungere il nome**. Registra un nome nel server WINS.
- **Aggiungi partner**. Aggiunge un partner di replica nel server WINS.
- **push init**. Avvia e invia un trigger push a un server WINS.
- **Mostra nome**. Visualizza informazioni dettagliate per un particolare record nel database del server WINS.  

Per ulteriori informazioni, vedere la pagina relativa alla [Shell di rete (netsh)](netsh.md).
