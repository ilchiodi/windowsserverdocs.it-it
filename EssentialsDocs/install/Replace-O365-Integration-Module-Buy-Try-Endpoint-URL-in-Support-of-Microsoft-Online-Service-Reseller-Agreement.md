---
title: Sostituire O365 integrazione modulo acquisto prova dell'URL dell'Endpoint contratto rivenditore servizi Online Microsoft
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 9860a6b9-baea-4bf0-9a9f-6f1a288f996e
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: b690cedd2f692cc6d11af6e05dd0cd4b4ea5a1d6
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="replace-o365-integration-module-buy-try-endpoint-url-in-support-of-microsoft-online-service-reseller-agreement"></a>Sostituire O365 integrazione modulo acquisto prova dell'URL dell'Endpoint contratto rivenditore servizi Online Microsoft

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

##  <a name="BKMK_O365"></a>   
 Se sei un partner di Microsoft Online Service rivenditore contratto (MOSRA), per garantire che le transazioni di iscrizione dei clienti siano elaborate mediante il portale, sarà necessario sostituire gli URL dell'endpoint utilizzati dal modulo di integrazione di Office 365 di Windows Server Essentials.  
  
 Il modulo di integrazione utilizza i seguenti quattro URL dell'endpoint:  
  
1.  Un endpoint di acquisto abbonamento a Office 365 Enterprise.  
  
    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\  
  
    -   Tipo = REG-SZ  
  
    -   Nome chiave = MOSRASTDBUY  
  
    -   Valore = *xxxxx*, dove xxxxx è l'URL di acquisto abbonamento aziendale. Ad esempio, valore = http://syndicatepartner.office365.com/enterprisebuy.html  
  
2.  Un endpoint di prova abbonamento a Office 365 Enterprise.  
  
    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\  
  
    -   Tipo = REG-SZ  
  
    -   Nome chiave = MOSRASTDTRY  
  
    -   Valore = *xxxxx*, dove xxxxx è l'URL di acquisto abbonamento aziendale. Ad esempio, valore = http://syndicatepartner.office365.com/enterprisetry.html  
  
3.  Un endpoint di acquisto abbonamento a Office 365 Small Business Premium.  
  
    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\  
  
    -   Tipo = REG-SZ  
  
    -   Nome chiave = MOSRALITEBUY  
  
    -   Valore = *xxxxx*, dove xxxxx è l'URL di acquisto abbonamento aziendale. Ad esempio, valore = http://syndicatepartner.office365.com/smallbizbuy.html  
  
4.  Un endpoint prova abbonamento a Office 365 Small Business Premium.  
  
    -   HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows Server\MSO\  
  
    -   Tipo = REG-SZ  
  
    -   Nome chiave = MOSRALITETRY  
  
    -   Valore = *xxxxx*, dove xxxxx è l'URL di acquisto abbonamento aziendale. Ad esempio, valore = http://syndicatepartner.office365.com/smallbiztry.html  
  
#### <a name="to-add-an-endpoint-url-key-to-the-registry"></a>Per aggiungere una chiave di URL dell'endpoint nel Registro di sistema  
  
1.  Nel computer di riferimento, fare clic su **Start**, tipo **regedit**, quindi premere INVIO.  
  
2.  Nel riquadro sinistro, espandere **HKEY_LOCAL_MACHINE**, espandere **SOFTWARE**, espandere **Microsoft**, espandere **Windows Server**, quindi espandere **MSO**.  
  
3.  Se MSO non esiste, fare doppio clic su **Windows Server**, scegliere **New**, fare clic su **chiave**e quindi digitare **MSO** per il nome della chiave.  
  
4.  Fare doppio clic su MSO, quindi fare clic su **valore stringa**. Digitare uno dei seguenti nomi stringa endpoint per il nome della stringa:  
  
    -   MOSRASTDBUY  
  
    -   MOSRASTDTRY  
  
    -   MOSRALITEBUY  
  
    -   MOSRALITETRY  
  
5.  Fare clic sulla nuova stringa nel riquadro di destra, quindi fare clic su **modifica**.  
  
6.  Digitare il nuovo URL dell'endpoint nel **dati valore** casella di testo, quindi fare clic su **OK**.  
  
7.  Ripetere i passaggi 4-6 per ogni nome di stringa elencato nel passaggio 4.  
  
## <a name="see-also"></a>Vedere anche  

 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Ulteriori personalizzazioni](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Test di analisi utilizzo](Testing-the-Customer-Experience.md)[creazione e personalizzazione dell'immagine](../install/Creating-and-Customizing-the-Image.md)   
 [Ulteriori personalizzazioni](../install/Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](../install/Preparing-the-Image-for-Deployment.md)   
 [Test di analisi utilizzo software](../install/Testing-the-Customer-Experience.md)

