---
title: Eseguire Windows Server Essentials Log Collector
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0d340223-fa24-4c75-ba8e-b654feb120ab
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 6b49fee7ca4a19d5a501cf96c1ce356f8242c81f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830922"
---
# <a name="run-the-windows-server-essentials-log-collector"></a>Eseguire Windows Server Essentials Log Collector
È possibile eseguire Windows Server Essentials Log Collector dal server o da un computer in rete. Se si esegue Log Collector dal server, è possibile raccogliere i registri solo dal server. Se si esegue Log Collector da un computer di rete, è possibile scegliere di raccogliere i registri dal server, oltre a quelli per tale computer.  
  
 Per eseguire Log Collector, è necessario avere i privilegi amministrativi appropriati. Per raccogliere i file di log per un server, è necessario essere un amministratore del server. Per raccogliere i file di log in un computer di rete, è necessario essere un amministratore client per tale computer.  
  
#### <a name="to-run-the-log-collector-on-the-server-by-using-the-wizard"></a>Per eseguire Log Collector sul server usando la procedura guidata  
  
1.  Nel **avviare** pagina del server, fare clic su **Windows Server Essentials Log Collector**.  
  
    > [!NOTE]
    >  -   Se il programma Log Collector non viene visualizzato nei **avviare** pagina, passare alla **%system%\Program file (x86) \Windows Server Essentials Log Collector**e quindi fare doppio clic su **LogCollector** .  
    > -   Se non si è connessi al server con privilegi amministrativi, Log Collector chiede di immettere le credenziali.  
  
2.  Quando viene chiesto il percorso salvare i file di log raccolti, è possibile scegliere il percorso predefinito,  **\\ \\< nomeserver\>\logs**, oppure specificare un'altra posizione. Per accettare la posizione predefinita, fare clic su **Next**. Per cambiare la posizione, fare clic su **Browse**, passare alla cartella in cui si vuole salvare i file di log e quindi fare clic su **Save**.  
  
    > [!NOTE]
    >  Non è necessario specificare i nomi file per i file di log. Log Collector assegna un nome dell'insieme di file zip concatenando il nome del computer e il timestamp del file.  
  
3.  Durante la raccolta dei registri viene visualizzato un indicatore di stato.  
  
4.  Per visualizzare il contenuto del file della raccolta dei log, selezionare la casella di controllo **Aprire il percorso file in cui sono stati salvati i file di log** e fare clic su **Close** per chiudere la procedura guidata e aprire il file della raccolta dei log.  
  
#### <a name="to-run-the-log-collector-on-a-network-computer-by-using-the-wizard"></a>Per eseguire Log Collector su un computer di rete usando la procedura guidata  
  
1.  Passare a **%system%\Program file (x86) \Windows Server Essentials Log Collector**, quindi fare doppio clic sul file **LogCollector.exe**.  
  
    > [!NOTE]
    >  Se non si è connessi al computer di rete con privilegi amministrativi, quando viene richiesto immettere il nome utente e la password e quindi fare clic su **Next**.  
  
2.  Selezionare i registri da raccogliere, nel modo seguente:  
  
    1.  Selezionare la casella di controllo **Server log files** per raccogliere i file di log sul server.  
  
    2.  La casella di controllo **Client computer log files (this computer)** è selezionata per impostazione predefinita e indica che Log Collector raccoglierà i registri dal computer di rete su cui è in esecuzione. Per raccogliere solo i registri del server, deselezionare la casella di controllo **Client computer log files (this computer)** .  
  
    3.  Fare clic su **Avanti**.  
  
3.  Quando viene richiesto, digitare il nome utente e la password per un amministratore server e quindi fare clic su **Next**.  
  
4.  Digitare o selezionare la posizione in cui si vuole salvare i file di log e quindi fare clic su **Next**.  
  
    > [!NOTE]
    >  Non è necessario specificare i nomi file per i file di log. Log Collector assegna un nome dell'insieme di file zip concatenando il nome del computer e il timestamp del file.  
  
5.  Durante la raccolta dei registri viene visualizzato un indicatore di stato.  
  
6.  Per visualizzare il contenuto del file della raccolta dei log, selezionare la casella di controllo **Aprire il percorso file in cui sono stati salvati i file di log** e fare clic su **Close** per chiudere la procedura guidata e aprire il file della raccolta dei log.  
  
### <a name="running-the-log-collector-manually"></a>Esecuzione manuale di Log Collector  
 Una volta installato Log Collector, viene creata un'attività pianificata per l'esecuzione dello strumento. In seguito sarà possibile eseguire Log Collector da **Scheduled Task Manager** senza usare la procedura guidata, se ci fossero problemi con l'avvio della procedura guidata.  
  
##### <a name="to-manually-run-the-log-collector-on-the-server"></a>Per eseguire manualmente Log Collector sul server  
  
1.  Accedere direttamente o in remoto al server.  
  
2.  Aprire l' **Utilità di pianificazione**.  
  
3.  Nella radice della **Libreria Utilità di pianificazione** selezionare l'attività pianificata denominata **LogCollector**.  
  
4.  Fare clic con il pulsante destro del mouse su **LogCollector**, quindi fare clic su **Esegui**. Log Collector inserisce i registri nella cartella predefinita sul server,  **\\ \\< nomeserver\>\Logs**. Se si dispone dell'autorizzazione di scrittura per la cartella o la cartella non esiste, i log vengono inseriti nel **< temp\>**  sottodirectory.  
  
##### <a name="to-manually-run-the-log-collector-on-a-network-computer"></a>Per eseguire manualmente Log Collector in un computer di rete  
  
1.  Accedere direttamente o in remoto al computer di rete.  
  
2.  Aprire l' **Utilità di pianificazione**.  
  
3.  Nella radice della **Libreria Utilità di pianificazione** selezionare l'attività pianificata denominata **LogCollector**.  
  
4.  Fare clic con il pulsante destro del mouse su **LogCollector**, quindi fare clic su **Esegui**. Log Collector inserisce i registri nella **< temp\>**  cartella sul computer di rete.
