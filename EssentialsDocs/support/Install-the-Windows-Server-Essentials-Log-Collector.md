---
title: Installare Windows Server Essentials Log Collector
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: d271c54f-1ffa-464e-afa5-27b8df61854e
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: ade18cec590392f35e7ad6b30d9a22ccdce44dcd
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="install-the-windows-server-essentials-log-collector"></a>Installare Windows Server Essentials Log Collector

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

L'installazione guidata di Windows Server Essentials Log Collector installa Log Collector come una finestra di avvio Add-in. È possibile installare e usare Log Collector sul computer di rete e/o il server. Dopo l'installazione, Log Collector verrà visualizzato nel Dashboard.  
  
###  <a name="BKMK_ToInstall"></a>Per installare Log Collector  
  
1.  Scaricare il pacchetto di installazione dell'agente di raccolta Log in qualsiasi server o computer in rete.  
  
    > [!NOTE]
    >  È possibile [scaricare il pacchetto di installazione di Log Collector](https://go.microsoft.com/fwlink/p/?LinkId=255470) da Microsoft.  
  
2.  Fare doppio clic sull'icona di Log Collector.  
  
3.  Se si esegue l'installazione da un computer di rete, immettere le credenziali di amministratore server quando richiesto.  
  
4.  Scegliere di accettare le condizioni di licenza Software Microsoft.  
  
5.  Per installare Log Collector solo nel server, selezionare il **solo sul server** casella di controllo. Per installare Log Collector in tutti i computer di rete, selezionare il **nel server e in tutti i computer nella rete** casella di controllo.  
  
6.  Fare clic su **installare il componente aggiuntivo**.  
  
###  <a name="BKMK_Reinstall"></a>Reinstallazione di Log Collector  
 Se è necessario reinstallare Log Collector, è necessario disinstallare e reinstallare Log Collector sul server e i computer di rete nella rete. Disinstallando Log Collector sul server dal Dashboard, tutti i computer di rete disinstalleranno automaticamente Log Collector.  
  
##### <a name="to-uninstall-and-reinstall-the-log-collector"></a>Per disinstallare e reinstallare Log Collector  
  
1.  Aprire il Dashboard.  
  
2.  Fare clic su di **componenti** selezionare **Log Collector** dall'elenco, quindi fare clic su **Disinstalla**.  
  

3.  Scaricare e installare Log Collector eseguendo i passaggi nella procedura precedente, [per installare Log Collector](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_ToInstall).  

3.  Scaricare e installare Log Collector eseguendo i passaggi nella procedura precedente, [per installare Log Collector](../support/Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_ToInstall).  

  
### <a name="manually-install-the-log-collector"></a>Installare manualmente Log Collector  
 Se l'installazione guidata non è riuscito a installare Log Collector, è possibile installare Log Collector in un singolo computer mediante la procedura seguente.  
  
##### <a name="to-manually-install-the-log-collector"></a>Per installare manualmente Log Collector  
  
1.  Rinominare l'estensione del file di installazione scaricato da. wssx CAB.  
  
2.  Fare doppio clic il nome del file di installazione.  
  
3.  Fare clic su **OK** se viene richiesto.  
  
4.  Fare doppio clic sul nome di file che termina con ˜.msi e selezionare una cartella in cui estrarlo.  
  
5.  Passare alla cartella con il file estratto e fare doppio clic sul file di installazione per utilizzare la procedura guidata per completare l'installazione.
