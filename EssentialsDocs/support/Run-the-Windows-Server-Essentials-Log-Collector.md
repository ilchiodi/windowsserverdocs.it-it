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
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="run-the-windows-server-essentials-log-collector"></a>Eseguire Windows Server Essentials Log Collector
È possibile eseguire Windows Server Essentials Log Collector dal server o da un computer in rete. Se si esegue Log Collector dal server, è possibile raccogliere i registri solo dal server. Se si esegue Log Collector da un computer di rete, è possibile scegliere di raccogliere i registri dal server, oltre ai registri per quel computer.  
  
 È necessario disporre dei privilegi amministrativi appropriati per eseguire Log Collector. Se si stanno raccogliendo i file di log per un server, è necessario essere un amministratore del Server; Se si stanno raccogliendo i file di log in un computer di rete, è necessario essere un amministratore Client per tale computer.  
  
#### <a name="to-run-the-log-collector-on-the-server-by-using-the-wizard"></a>Per eseguire Log Collector sul server usando la procedura guidata  
  
1.  Nel **Start** pagina del server, fare clic su **Windows Server Essentials Log Collector**.  
  
    > [!NOTE]
    >  -   Se il programma Log Collector non viene visualizzato nel **Start** pagina, passare a **%system%\Program Files (x86) \Windows Server Essentials Log Collector**e quindi fare doppio clic su **LogCollector**.  
    > -   Se non si è connessi al server con privilegi amministrativi, Log Collector chiede di immettere le credenziali.  
  
2.  Quando viene chiesta una posizione in cui salvare i file di log raccolti, è possibile scegliere il percorso predefinito, **\ \ \ < ServerName\ > \logs**, o specificarne un'altra posizione. Per accettare il percorso predefinito, fare clic su **Avanti**. Per modificare il percorso, fare clic su **Sfoglia**, passare alla cartella in cui si desidera salvare i file di log e quindi fare clic su **salvare**.  
  
    > [!NOTE]
    >  Non devi specificare i nomi di file per i file di registro. Log Collector assegna un nome dell'insieme di file zip concatenando il nome del computer e il timestamp del file.  
  
3.  Viene visualizzato un indicatore di stato mentre vengono raccolti i registri.  
  
4.  Per visualizzare il contenuto del file di raccolta log, seleziona il **aprire il percorso del file in cui erano stati salvati i registri** casella di controllo e fare clic su **chiudere** per chiudere la procedura guidata e aprire il file di raccolta log.  
  
#### <a name="to-run-the-log-collector-on-a-network-computer-by-using-the-wizard"></a>Per eseguire Log Collector in un computer di rete usando la procedura guidata  
  
1.  Passare a **%system%\Program Files (x86) \Windows Server Essentials Log Collector**e quindi doppio clic sul file **LogCollector.exe**.  
  
    > [!NOTE]
    >  Se non si è connessi al computer di rete con privilegi amministrativi, immettere il nome utente e la password quando richiesto e quindi fare clic su **Avanti**.  
  
2.  Selezionare i registri che si desidera raccogliere, come indicato di seguito:  
  
    1.  Selezionare il **file di log Server** casella di controllo per raccogliere i file di log sul server.  
  
    2.  Il **file di registro dei computer Client (questo computer)** casella di controllo è selezionata per impostazione predefinita, che indica che Log Collector raccoglierà i registri dal computer di rete che esegue il. Se si desidera raccogliere i registri del server, deselezionare il **file di registro dei computer Client (questo computer)** casella di controllo.  
  
    3.  Fare clic su **Avanti**.  
  
3.  Quando richiesto, digitare il nome utente e la password per un amministratore del server e quindi fare clic su **Avanti**.  
  
4.  Digitare o selezionare il percorso in cui si desidera salvare i file di log e quindi fare clic su **Avanti**.  
  
    > [!NOTE]
    >  Non devi specificare i nomi di file per i file di registro. Log Collector assegna un nome dell'insieme di file zip concatenando il nome del computer e il timestamp del file.  
  
5.  Viene visualizzato un indicatore di stato mentre vengono raccolti i registri.  
  
6.  Per visualizzare il contenuto del file di raccolta log, seleziona il **aprire il percorso del file in cui erano stati salvati i registri** casella di controllo e fare clic su **chiudere** per chiudere la procedura guidata e aprire il file di raccolta log.  
  
### <a name="running-the-log-collector-manually"></a>Eseguire manualmente Log Collector  
 Dopo aver installato l'agente di raccolta Log, viene creata un'attività pianificata per eseguire lo strumento. Successivamente è possibile eseguire Log Collector dal **Scheduled Task Manager** senza utilizzare la procedura guidata, se sono presenti problemi di avvio della procedura guidata.  
  
##### <a name="to-manually-run-the-log-collector-on-the-server"></a>Per eseguire manualmente Log Collector sul server  
  
1.  Accedere direttamente o in remoto al server.  
  
2.  Aprire il **utilità di pianificazione**.  
  
3.  Nella radice di **libreria utilità di pianificazione**, selezionare l'attività pianificata denominata **LogCollector**.  
  
4.  Fare doppio clic su **LogCollector**, quindi fare clic su **eseguire**. Log Collector inserisce i registri nella cartella predefinita sul server, **\ \ \ < ServerName\ > \Logs**. Se si dispone dell'autorizzazione di scrittura per la cartella o la cartella non esiste, i registri vengono inseriti nel **< temp\ >** sottodirectory.  
  
##### <a name="to-manually-run-the-log-collector-on-a-network-computer"></a>Per eseguire manualmente Log Collector in un computer di rete  
  
1.  Accedere direttamente o in remoto al computer di rete.  
  
2.  Aprire il **utilità di pianificazione**.  
  
3.  Nella radice di **libreria utilità di pianificazione**, selezionare l'attività pianificata denominata **LogCollector**.  
  
4.  Fare doppio clic su **LogCollector**, quindi fare clic su **eseguire**. Log Collector inserisce i registri nel **< temp\ >** cartella sul computer di rete.
