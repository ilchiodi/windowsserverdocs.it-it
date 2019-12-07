---
title: Installare Windows Server Essentials Log Collector
description: Viene descritto come usare Windows Server Essentials
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
ms.openlocfilehash: a1b1a5492a6e5dbc48899b3918314676884e0454
ms.sourcegitcommit: 39244de670f712857a5fdd56630e95d57b7001a5
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/06/2019
ms.locfileid: "74897681"
---
# <a name="install-the-windows-server-essentials-log-collector"></a>Installare Windows Server Essentials Log Collector

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

L'installazione guidata di Windows Server Essentials Log Collector installa Log Collector come componente aggiuntivo launchpad. È possibile installare e usare Log Collector nei computer di rete o nel server o in entrambi. Dopo l'installazione, Log Collector verrà visualizzato nel dashboard.  
  
###  <a name="BKMK_ToInstall"></a>Per installare Log Collector  
  
1.  Scaricare il pacchetto di installazione di Log Collector in un server o in un computer in rete.  
  
    > [!NOTE]
    > [Scaricare il pacchetto di installazione di Windows Server Essentials Log Collector](https://www.microsoft.com/download/details.aspx?id=34821).  
  
2.  Fare doppio clic sull'icona di Log Collector.  
  
3.  Se si esegue l'installazione da un computer di rete, immettere le credenziali di amministratore server, quando viene richiesto.  
  
4.  Scegliere di accettare le condizioni di licenza software Microsoft.  
  
5.  Per installare Log Collector solo nel server, selezionare la casella di controllo **Only on the server** . Per installare Log Collector in tutti i computer di rete, selezionare la casella di controllo **Nel server e in tutti i computer della rete** .  
  
6.  Fare clic su **Install the Add-in**.  
  
###  <a name="BKMK_Reinstall"></a>Reinstallazione di log Collector  
 Se è necessario reinstallare Log Collector, disinstallare e reinstallare Log Collector nel server e nei computer di rete, Disinstallando Log Collector dal server dal dashboard, tutti i computer di rete disinstalleranno automaticamente Log Collector.  
  
##### <a name="to-uninstall-and-reinstall-the-log-collector"></a>Per disinstallare e reinstallare Log Collector  
  
1.  Aprire il dashboard.  
  
2.  Fare clic sulla scheda **Componente aggiuntivo** , selezionare **Log Collector** nell'elenco e quindi fare clic su **Disinstalla**.  
  

3.  Scaricare e installare Log Collector eseguendo i passaggi nella procedura precedente, [To install the Log Collector](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_ToInstall).  

3.  Scaricare e installare Log Collector eseguendo i passaggi nella procedura precedente, [To install the Log Collector](../support/Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_ToInstall).  

  
### <a name="manually-install-the-log-collector"></a>Installare manualmente Log Collector  
 Se l'installazione guidata non riesce a installare Log Collector, è possibile installare Log Collector in un solo computer con la procedura seguente.  
  
##### <a name="to-manually-install-the-log-collector"></a>Per installarenstallare manualmente Log Collector  
  
1.  Rinominare l'estensione del file di installazione scaricato da. wssx a. cab.  
  
2.  Fare doppio clic sul nome del file di installazione.  
  
3.  Fare clic su **OK** se viene richiesto.  
  
4.  Fare doppio clic sul nome file che termina con ". msi" e selezionare una cartella in cui estrarlo.  
  
5.  Passare alla cartella con il file estratto e fare doppio clic sul file di installazione per usare la procedura guidata per completare l'installazione.
