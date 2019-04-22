---
title: Modifica delle impostazioni dei flussi multimediali
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59823402"
---
# <a name="change-media-streaming-settings"></a>Modifica delle impostazioni dei flussi multimediali

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Per modificare le impostazioni dei flussi multimediali sono disponibili diverse opzioni Sono disponibili le seguenti opzioni:  
  
-   [Componente aggiuntivo flussi multimediali remoti nascosto](Change-Media-Streaming-Settings.md#BKMK_DisableRemote)  
  
-   [Impostare il nome della Raccolta multimediale](Change-Media-Streaming-Settings.md#BKMK_LibraryName)  
  
-   [Impostare la qualità di streaming video](Change-Media-Streaming-Settings.md#BKMK_StreamingQuality)  
  
-   [Attivare o disattivare i flussi multimediali a livello di codice](Change-Media-Streaming-Settings.md#BKMK_Program)  
  
##  <a name="BKMK_DisableRemote"></a> Componente aggiuntivo flussi multimediali remoti nascosto  
 È possibile nascondere il componente aggiuntivo per i flussi multimediali remoti aggiungendo una voce al Registro di sistema.  
  
#### <a name="to-hide-the-remote-media-streaming-add-in"></a>Per nascondere il componente aggiuntivo per i flussi multimediali remoti  
  
1.  Sul server, spostare il puntatore del mouse verso l'angolo superiore destro dello schermo e fare clic su **Trova**.  
  
2.  Nella casella di **ricerca** digitare **regedit** e quindi fare clic sull'applicazione **Regedit**.  
  
3.  Nel riquadro di sinistra espandere la seguente voce del Registro di sistema:  
  
     **HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\RemoteAccess\DisabledAddIns**  
  
4.  Fare clic con il tasto destro su **DisabledAddIns**, selezionare **Nuovo** e quindi fare clic su **Valore DWORD**.  
  
5.  Come nuovo valore, digitare **497796c6-9cc7-43be-aa26-4e6b5695370d**, che corrisponde all'identificatore del componente aggiuntivo per i flussi multimediali remoti, quindi premere **Enter**.  
  
6.  Fare clic con il tasto destro sul valore, quindi selezionare **Modifica**.  
  
7.  Immettere il valore **1** nel campo Dati valore e quindi fare clic su **OK**.  
  
##  <a name="BKMK_LibraryName"></a> Impostare il nome della Raccolta multimediale  
 Per impostare il nome della raccolta multimediale, è possibile utilizzare una classe delle Soluzioni Windows Server SDK. Per impostare il nome della raccolta multimediale, è possibile utilizzare il metodo **SetMediaLibraryName** della classe **MediaStreamingManager** all'interno dello spazio dei nomi **Microsoft.WindowsServerSolutions.MediaStreaming** . L'esempio seguente mostra come impostare il nome per la raccolta multimediale:  
  
```c#  
  
MediaStreamingManager mediaStreamingManager = new MediaStreamingManager();  
string mediaLibraryName = Guid.NewGuid().ToString("B");   
mediaStreamingManager.SetMediaLibraryName(mediaLibraryName);  
  
```  
  
 Per altre informazioni, vedere [Windows Server Solutions SDK](https://go.microsoft.com/fwlink/?LinkID=248648).  
  
##  <a name="BKMK_StreamingQuality"></a> Impostare la qualità di streaming video  
 È possibile impostare la qualità dello streaming video ricavando il punteggio WinSAT CPU, quindi creando e installando il file .xml contenente le informazioni sul punteggio WinSAT. Se il file .xml contenente le informazioni sul punteggio WinSAT è stato installato prima dell'esecuzione della configurazione iniziale, l'interfaccia utente per l'impostazione della qualità video non viene visualizzata dal cliente. Per ulteriori informazioni, vedere [Impostazione del punteggio WinSAT sul server](Set-the-WinSAT-Score-on-the-Server.md).  
  
##  <a name="BKMK_Program"></a> Attivare o disattivare i flussi multimediali a livello di codice  
 Per attivare e disattivare i flussi multimediali a livello di programmazione, è possibile utilizzare una classe delle Soluzioni Windows Server SDK. Per attivare e disattivare i flussi multimediali, è possibile utilizzare il metodo **SetMediaStreamingEnabled** della classe **MediaStreamingManager** all'interno dello spazio dei nomi **Microsoft.WindowsServerSolutions.MediaStreaming**. Nell'esempio di codice riportato di seguito viene illustrato come attivare i flussi multimediali:  
  
```c#  
  
MediaStreamingManager mediaStreamingManager = new MediaStreamingManager();  
mediaStreamingManager.SetMediaStreamingEnabled(true);  
  
```  
  
 Nell'esempio di codice riportato di seguito viene illustrato come disattivare i flussi multimediali:  
  
```c#  
  
MediaStreamingManager mediaStreamingManager = new MediaStreamingManager();  
mediaStreamingManager.SetMediaStreamingEnabled(false);  
```  
  
## <a name="see-also"></a>Vedere anche  
 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Personalizzazioni aggiuntive](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Testare l'esperienza dei clienti](Testing-the-Customer-Experience.md)