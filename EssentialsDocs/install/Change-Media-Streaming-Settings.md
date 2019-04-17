---
title: Modifica le impostazioni dei flussi multimediali
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: dec690d2-f80c-4b09-99d6-3bba41331972
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d34d60e792fcda4d71a4509fe3d1b95fc6e74d0e
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="change-media-streaming-settings"></a>Modifica le impostazioni dei flussi multimediali

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Più opzioni sono disponibili per la modifica delle impostazioni dei flussi multimediali. Sono disponibili le opzioni seguenti:  
  
-   [Nascondere aggiuntivo di flussi multimediali remoti](Change-Media-Streaming-Settings.md#BKMK_DisableRemote)  
  
-   [Impostare il nome della Raccolta multimediale](Change-Media-Streaming-Settings.md#BKMK_LibraryName)  
  
-   [Impostare la qualità di streaming video](Change-Media-Streaming-Settings.md#BKMK_StreamingQuality)  
  
-   [A livello di programmazione di attivare o disattivare i flussi multimediali](Change-Media-Streaming-Settings.md#BKMK_Program)  
  
##  <a name="BKMK_DisableRemote"></a>Nascondere aggiuntivo di flussi multimediali remoti  
 È possibile nascondere i flussi multimediali remoti aggiungere-aggiungendo una voce nel Registro di sistema.  
  
#### <a name="to-hide-the-remote-media-streaming-add-in"></a>Per nascondere i componenti aggiuntivi flussi multimediali remoti  
  
1.  Nel server, sposta il mouse nell'angolo superiore destro dello schermo e fare clic su **ricerca**.  
  
2.  Nel **ricerca** digitare **regedit**, quindi fare clic sul **Regedit** applicazione.  
  
3.  Nel riquadro sinistro, espandere la voce del Registro di sistema seguente:  
  
     **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\RemoteAccess\DisabledAddIns**  
  
4.  Fare doppio clic su **DisabledAddIns**, scegliere **New**, quindi fare clic su **valore DWORD**.  
  
5.  Per il nuovo valore, digitare **497796c6-9cc7-43be-aa26-4e6b5695370d**, che è l'identificatore per i componenti aggiuntivi flussi multimediali remoti e quindi premere **invio**.  
  
6.  Fare clic con il pulsante destro il valore, quindi fare clic su **modifica**.  
  
7.  Tipo **1** per i dati valore e quindi fare clic su **OK**.  
  
##  <a name="BKMK_LibraryName"></a>Impostare il nome della Raccolta multimediale  
 È possibile utilizzare una classe in Windows Server Solutions SDK per impostare il nome del catalogo multimediale. Per impostare il nome del catalogo multimediale, è possibile utilizzare il **SetMediaLibraryName** metodo il **MediaStreamingManager** classe il **MediaStreaming** dello spazio dei nomi. L'esempio seguente viene illustrato come impostare il nome della Raccolta multimediale:  
  
```c#  
  
MediaStreamingManager mediaStreamingManager = new MediaStreamingManager();  
string mediaLibraryName = Guid.NewGuid().ToString("B");   
mediaStreamingManager.SetMediaLibraryName(mediaLibraryName);  
  
```  
  
 Per ulteriori informazioni, vedere [Windows Server Solutions SDK](https://go.microsoft.com/fwlink/?LinkID=248648).  
  
##  <a name="BKMK_StreamingQuality"></a>Impostare la qualità di streaming video  
 Puoi impostare il qualità dello streaming video come ottenere il punteggio WinSAT CPU e quindi creando e installando il file XML che contiene le informazioni sul punteggio WinSAT. Se il file XML che contiene le informazioni sul punteggio WinSAT è stato installato prima che venga eseguita la configurazione iniziale, l'interfaccia utente per l'impostazione della qualità video non verrà visualizzati al cliente. Per ulteriori informazioni, vedere [impostazione del punteggio WinSAT sul Server](Set-the-WinSAT-Score-on-the-Server.md).  
  
##  <a name="BKMK_Program"></a>A livello di programmazione di attivare o disattivare i flussi multimediali  
 È possibile utilizzare una classe in Windows Server Solutions SDK a livello di codice attivare o disattivare i flussi multimediali. Per attivare o disattivare i flussi multimediali, è possibile utilizzare il **SetMediaStreamingEnabled** metodo il **MediaStreamingManager** classe il **MediaStreaming** dello spazio dei nomi. Esempio di codice seguente viene illustrato come attivare i flussi multimediali:  
  
```c#  
  
MediaStreamingManager mediaStreamingManager = new MediaStreamingManager();  
mediaStreamingManager.SetMediaStreamingEnabled(true);  
  
```  
  
 Esempio di codice seguente viene illustrato come disattivare i flussi multimediali:  
  
```c#  
  
MediaStreamingManager mediaStreamingManager = new MediaStreamingManager();  
mediaStreamingManager.SetMediaStreamingEnabled(false);  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Ulteriori personalizzazioni](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Test di analisi utilizzo software](Testing-the-Customer-Experience.md)