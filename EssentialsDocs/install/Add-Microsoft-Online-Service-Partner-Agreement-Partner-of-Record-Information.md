---
title: Aggiungere le informazioni sul partner registrato per il contratto Microsoft Online Service Partner Agreement
description: Viene descritto come usare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9bd191d6-ecc5-4230-a88e-f3fc281cb956
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: ec8387c59ebf42eb4287807e5959a50cea4215c4
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80310248"
---
# <a name="add-microsoft-online-service-partner-agreement-partner-of-record-information"></a>Aggiungere le informazioni sul partner registrato per il contratto Microsoft Online Service Partner Agreement

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_3rdLevelDomanNames"></a>   
 Se si è un partner Microsoft Online Service Partner Agreement (MOSPA) per Office 365, per garantire la corretta compensazione quando una richiesta di sottoscrizione viene originata da Windows Server Essentials tramite il modulo di integrazione di Office 365, è necessario creare un chiave del registro di sistema che contiene l'identificazione del partner di record (ID POR). Le seguenti informazioni vengono lette e trasmesse al provider di servizi tramite l’URL di iscrizione di Office 365.  
  
-   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO  
  
-   Tipo = Valore stringa  
  
-   Nome chiave = Partner  
  
-   Valore = xxxxx, dove xxxxx è il proprio ID POR  
  
#### <a name="to-add-the-por-id-key-to-the-registry"></a>Per aggiungere la chiave ID POR al Registro di sistema  
  
1.  Nel computer di riferimento fare clic su **Start**, digitare **regedit** e quindi premere INVIO.  
  
2.  Nel riquadro sinistro, espandere **HKEY_LOCAL_MACHINE**, **SOFTWARE**, **Microsoft**, quindi **Windows Server**.  
  
3.  Fare clic con il pulsante destro del mouse su **Windows Server**, scegliere **Nuovo**, quindi fare clic su **Chiave**.  
  
4.  Digitare **MSO** come nome della chiave.  
  
5.  Fare clic con il pulsante destro del mouse sulla chiave appena creata, quindi fare clic su **Valore stringa**.  
  
6.  Digitare **Partner** come nome della stringa e quindi premere INVIO.  
  
7.  Fare clic con il pulsante destro del mouse sulla nuova stringa **Partner** nel riquadro di destra e scegliere **Modifica**.  
  
8.  Digitare il proprio ID POR nella casella di testo **Dati valore**, quindi fare clic su **OK**.  
  
## <a name="see-also"></a>Vedi anche  

 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Personalizzazioni aggiuntive](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Test di Analisi utilizzo software](Testing-the-Customer-Experience.md)

 [Creazione e personalizzazione dell'immagine](../install/Creating-and-Customizing-the-Image.md)   
 [Personalizzazioni aggiuntive](../install/Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](../install/Preparing-the-Image-for-Deployment.md)   
 [Test di Analisi utilizzo software](../install/Testing-the-Customer-Experience.md)

