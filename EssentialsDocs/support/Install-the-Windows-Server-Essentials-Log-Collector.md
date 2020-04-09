---
title: Installare Windows Server Essentials Log Collector
description: Viene descritto come usare Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: d271c54f-1ffa-464e-afa5-27b8df61854e
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 4ea922142b43e35d6f17207d448c48b2da52ebc8
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80852284"
---
# <a name="install-the-windows-server-essentials-log-collector"></a>Installare Windows Server Essentials Log Collector

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

L'installazione guidata di Windows Server Essentials Log Collector installa Log Collector come componente aggiuntivo launchpad. È possibile installare e usare Log Collector nei computer di rete o nel server o in entrambi. Dopo l'installazione, Log Collector verrà visualizzato nel dashboard.  
  
###  <a name="to-install-the-log-collector"></a><a name="BKMK_ToInstall"></a>Per installare Log Collector  
  
1.  Scaricare il pacchetto di installazione di Log Collector in un server o in un computer in rete.  
  
    > [!NOTE]
    > [Scaricare il pacchetto di installazione di Windows Server Essentials Log Collector](https://www.microsoft.com/download/details.aspx?id=34821).  
  
2.  Fare doppio clic sull'icona di Log Collector.  
  
3.  Se si esegue l'installazione da un computer di rete, immettere le credenziali di amministratore server, quando viene richiesto.  
  
4.  Scegliere di accettare le condizioni di licenza software Microsoft.  
  
5.  Per installare Log Collector solo nel server, selezionare la casella di controllo **Only on the server**. Per installare Log Collector in tutti i computer di rete, selezionare la casella di controllo **On the server and on all of the computers on the network**.  
  
6.  Fare clic su **Install the Add-in**.  
  
###  <a name="reinstalling-the-log-collector"></a><a name="BKMK_Reinstall"></a>Reinstallazione di log Collector  
 Se è necessario reinstallare Log Collector, disinstallare e reinstallare Log Collector nel server e nei computer di rete, Disinstallando Log Collector dal server dal dashboard, tutti i computer di rete disinstalleranno automaticamente Log Collector.  
  
##### <a name="to-uninstall-and-reinstall-the-log-collector"></a>Per disinstallare e reinstallare Log Collector  
  
1.  Aprire il Dashboard.  
  
2.  Fare clic sulla scheda **Componente aggiuntivo**, selezionare **Log Collector** nell'elenco e quindi fare clic su **Disinstalla**.  
  

3.  Scaricare e installare Log Collector eseguendo i passaggi nella procedura precedente, [Per installare Log Collector](Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_ToInstall).  

3.  Scaricare e installare Log Collector eseguendo i passaggi nella procedura precedente, [Per installare Log Collector](../support/Install-the-Windows-Server-Essentials-Log-Collector.md#BKMK_ToInstall).  

  
### <a name="manually-install-the-log-collector"></a>Installare manualmente Log Collector  
 Se l'installazione guidata non riesce a installare Log Collector, è possibile installare Log Collector in un solo computer con la procedura seguente.  
  
##### <a name="to-manually-install-the-log-collector"></a>Per installarenstallare manualmente Log Collector  
  
1.  Rinominare l'estensione del file di installazione scaricato da. wssx a. cab.  
  
2.  Fare doppio clic sul nome del file di installazione.  
  
3.  Fare clic su **OK** se viene richiesto.  
  
4.  Fare doppio clic sul nome file che termina con ". msi" e selezionare una cartella in cui estrarlo.  
  
5.  Passare alla cartella con il file estratto e fare doppio clic sul file di installazione per usare la procedura guidata per completare l'installazione.
