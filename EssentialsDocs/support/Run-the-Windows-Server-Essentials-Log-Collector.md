---
title: Eseguire Windows Server Essentials Log Collector
description: Viene descritto come usare Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 0d340223-fa24-4c75-ba8e-b654feb120ab
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 6aac2ed382321349d39874c7db7617f6da845919
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852274"
---
# <a name="run-the-windows-server-essentials-log-collector"></a>Eseguire Windows Server Essentials Log Collector
È possibile eseguire l'agente di raccolta log di Windows Server Essentials dal server o da un computer in rete. Se si esegue Log Collector dal server, è possibile raccogliere i registri solo dal server. Se si esegue Log Collector da un computer di rete, è possibile scegliere di raccogliere i registri dal server, oltre a quelli per tale computer.  
  
 Per eseguire Log Collector, è necessario avere i privilegi amministrativi appropriati. Per raccogliere i file di log per un server, è necessario essere un amministratore del server. Per raccogliere i file di log in un computer di rete, è necessario essere un amministratore client per tale computer.  
  
#### <a name="to-run-the-log-collector-on-the-server-by-using-the-wizard"></a>Per eseguire Log Collector sul server usando la procedura guidata  
  
1. Nella pagina **iniziale** del server fare clic su **raccolta log di Windows Server Essentials**.  
  
   > [!NOTE]
   > - Se il programma di raccolta log non viene visualizzato nella pagina **iniziale** , passare a **%System%\Program Files (x86) \Windows Server Essentials Log Collector**, quindi fare doppio clic su **LogCollector**.  
   >   -   Se non si è connessi al server con privilegi amministrativi, Log Collector chiede di immettere le credenziali.  
  
2. Quando viene richiesto di specificare un percorso in cui salvare i file di log raccolti, è possibile scegliere il percorso predefinito, **\\\\< nomeserver\>\Logs**o specificare un altro percorso. Per accettare la posizione predefinita, fare clic su **Next**. Per cambiare la posizione, fare clic su **Browse**, passare alla cartella in cui si vuole salvare i file di log e quindi fare clic su **Save**.  
  
   > [!NOTE]
   >  Non è necessario specificare i nomi file per i file di log. Log Collector assegna un nome alla raccolta di file zip concatenando il nome del computer e il timestamp del file.  
  
3. Durante la raccolta dei registri viene visualizzato un indicatore di stato.  
  
4. Per visualizzare i contenuti del file della raccolta dei registri, selezionare la casella di controllo **Open the file location where the logs were saved** e fare clic su **Close** per chiudere la procedura guidata e aprire il file della raccolta dei registri.  
  
#### <a name="to-run-the-log-collector-on-a-network-computer-by-using-the-wizard"></a>Per eseguire Log Collector su un computer di rete usando la procedura guidata  
  
1.  Passare a **%System%\Program Files (x86) \Windows Server Essentials Log Collector**, quindi fare doppio clic sul file **LogCollector. exe**.  
  
    > [!NOTE]
    >  Se non si è connessi al computer di rete con privilegi amministrativi, quando viene richiesto immettere il nome utente e la password e quindi fare clic su **Next**.  
  
2.  Selezionare i registri da raccogliere, nel modo seguente:  
  
    1.  Selezionare la casella di controllo **Server log files** per raccogliere i file di log sul server.  
  
    2.  La casella di controllo **Client computer log files (this computer)** è selezionata per impostazione predefinita e indica che Log Collector raccoglierà i registri dal computer di rete su cui è in esecuzione. Per raccogliere solo i registri del server, deselezionare la casella di controllo **Client computer log files (this computer)** .  
  
    3.  Fare clic su **Avanti**.  
  
3.  Quando viene richiesto, digitare il nome utente e la password per un amministratore server e quindi fare clic su **Next**.  
  
4.  Digitare o selezionare la posizione in cui si vuole salvare i file di log e quindi fare clic su **Next**.  
  
    > [!NOTE]
    >  Non è necessario specificare i nomi file per i file di log. Log Collector assegna un nome alla raccolta di file zip concatenando il nome del computer e il timestamp del file.  
  
5.  Durante la raccolta dei registri viene visualizzato un indicatore di stato.  
  
6.  Per visualizzare i contenuti del file della raccolta dei registri, selezionare la casella di controllo **Open the file location where the logs were saved** e fare clic su **Close** per chiudere la procedura guidata e aprire il file della raccolta dei registri.  
  
### <a name="running-the-log-collector-manually"></a>Esecuzione manuale di Log Collector  
 Una volta installato Log Collector, viene creata un'attività pianificata per l'esecuzione dello strumento. In seguito sarà possibile eseguire Log Collector da **Scheduled Task Manager** senza usare la procedura guidata, se ci fossero problemi con l'avvio della procedura guidata.  
  
##### <a name="to-manually-run-the-log-collector-on-the-server"></a>Per eseguire manualmente Log Collector sul server  
  
1.  Accedere direttamente o in remoto al server.  
  
2.  Aprire l'**Utilità di pianificazione**.  
  
3.  Nella radice della **Libreria Utilità di pianificazione** selezionare l'attività pianificata denominata **LogCollector**.  
  
4.  Fare clic con il pulsante destro del mouse su **LogCollector**, quindi fare clic su **Esegui**. Log Collector inserisce i registri nella cartella predefinita sul server **\\\\< nomeserver\>\Logs**. Se non si dispone dell'autorizzazione di scrittura per la cartella o la cartella non esiste, i registri vengono inseriti nella sottodirectory **< temp\>** .  
  
##### <a name="to-manually-run-the-log-collector-on-a-network-computer"></a>Per eseguire manualmente Log Collector in un computer di rete  
  
1.  Accedere direttamente o in remoto al computer di rete.  
  
2.  Aprire l'**Utilità di pianificazione**.  
  
3.  Nella radice della **Libreria Utilità di pianificazione** selezionare l'attività pianificata denominata **LogCollector**.  
  
4.  Fare clic con il pulsante destro del mouse su **LogCollector**, quindi fare clic su **Esegui**. Log Collector inserisce i registri nella cartella **< temp\>** sul computer di rete.
