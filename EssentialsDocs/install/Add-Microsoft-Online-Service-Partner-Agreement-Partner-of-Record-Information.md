---
title: Aggiungere informazioni di Microsoft Online Service Partner contratto-Partner of Record
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9bd191d6-ecc5-4230-a88e-f3fc281cb956
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 39ce43228cd7392bcc86de4a410c52676ce15047
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="add-microsoft-online-service-partner-agreement-partner-of-record-information"></a>Aggiungere informazioni di Microsoft Online Service Partner contratto-Partner of Record

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_3rdLevelDomanNames"></a>   
 Se sei un partner di Microsoft Online Service Partner contratto (MOSPA) per Office 365, per assicurarsi che sono ricevere il corretto compenso quando viene originata una richiesta di sottoscrizione da Windows Server Essentials tramite il modulo di integrazione di Office 365, è necessario creare una chiave del Registro di sistema che contiene il record del partner (ID POR). Leggere le informazioni seguenti viene passate al provider di servizi tramite gli URL di abbonamento a Office 365.  
  
-   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO  
  
-   Tipo = valore stringa  
  
-   Nome chiave = Partner  
  
-   Valore = xxxxx, dove xxxxx è il proprio ID POR  
  
#### <a name="to-add-the-por-id-key-to-the-registry"></a>Per aggiungere la chiave ID POR al Registro di sistema  
  
1.  Nel computer di riferimento, fare clic su **Start**, tipo **regedit**, quindi premere INVIO.  
  
2.  Nel riquadro sinistro, espandere **HKEY_LOCAL_MACHINE**, espandere **SOFTWARE**, espandere **Microsoft**, quindi espandere **Windows Server**.  
  
3.  Fare doppio clic su **Windows Server**, scegliere **New**, quindi fare clic su **chiave**.  
  
4.  Tipo **MSO** per il nome della chiave.  
  
5.  Fare clic sulla chiave appena creato, quindi fare clic su **valore stringa**.  
  
6.  Tipo **Partner** per il nome della stringa e quindi premere INVIO.  
  
7.  Fare doppio clic su nuovo **Partner** stringa nel riquadro di destra, quindi fare clic su **modifica**.  
  
8.  Digitare il proprio ID POR nel **dati valore** casella di testo, quindi fare clic su **OK**.  
  
## <a name="see-also"></a>Vedere anche  

 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Ulteriori personalizzazioni](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Test di analisi utilizzo software](Testing-the-Customer-Experience.md)

 [Creazione e personalizzazione dell'immagine](../install/Creating-and-Customizing-the-Image.md)   
 [Ulteriori personalizzazioni](../install/Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](../install/Preparing-the-Image-for-Deployment.md)   
 [Test di analisi utilizzo software](../install/Testing-the-Customer-Experience.md)

