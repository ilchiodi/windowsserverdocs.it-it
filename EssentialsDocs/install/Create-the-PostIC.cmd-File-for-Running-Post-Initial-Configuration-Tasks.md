---
title: "Creare il File PostIC.cmd per eseguire le attività di configurazione iniziale Post"
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 99e258bc-0695-48c9-b694-a7f3cbe2a2d0
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: f5042204cd189e3101f5e0126fd98e786a49032d
ms.sourcegitcommit: 70c1b6cedad55b9c7d2068c9aa4891c6c533ee4c
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 07/03/2017
---
# <a name="create-the-posticcmd-file-for-running-post-initial-configuration-tasks"></a>Creare il File PostIC.cmd per eseguire le attività di configurazione iniziale Post

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

È possibile aggiungere personalizzazioni alla configurazione post-iniziale scrivendo un proprio codice e chiamando quindi il codice da un file script chiamato PostIC.cmd. Quando si utilizza il file PostIC.cmd, è necessario rispettare le linee guida seguenti:  
  
-   Il codice di personalizzazione deve eseguire automaticamente (non può visualizzare un'interfaccia utente).  
  
-   Il codice di personalizzazione non può avviare un riavvio del server. La configurazione iniziale verrà riavviato il server come ultima attività.  
  
-   Il codice di personalizzazione deve eseguire in tre minuti o meno.  
  
 Definire il file PostIC.cmd per restituire il valore 0 se il codice viene eseguito correttamente. Se vengono restituiti altri valori, il sistema operativo ricerca un file denominato [SetupFailure.cmd](Create-the-PostIC.cmd-File-for-Running-Post-Initial-Configuration-Tasks.md#BKMK_SetupFailure), che contiene il codice che deve essere eseguito se il codice nel file PostIC.cmd non venga eseguito correttamente. Sia il file PostIC.cmd e il file SetupFailure.cmd devono essere C:\Windows\Setup\Scripts trova.  
  
#### <a name="to-define-post-initial-configuration-customizations"></a>Per definire le personalizzazioni successive alla configurazione iniziale  
  
1.  Scrivere il codice che viene chiamato dallo script PostIC.cmd.  
  
2.  Utilizzando blocco note, creare un file denominato PostIC.cmd e aggiungere la chiamata al codice creato nel passaggio 1. Assicurarsi che il codice restituisca un valore di esito positivo.  
  
3.  Salvare PostIC.cmd in C:\Windows\Setup\Scripts.  
  
4.  (Facoltativo) Creare un file SetupFailure.cmd che esegua il codice se PostIC.cmd restituisce un valore diverso da 0.  
  
###  <a name="BKMK_SetupFailure"></a>SetupFailure.cmd  
 È possibile fornire una notifica di problemi nella configurazione iniziale utilizzando SetupFailure.cmd. Il file SetupFailure.cmd contiene il codice che si desidera eseguire in caso di problemi. Il file SetupFailure.cmd C:\Windows\Setup\Scripts è attivato e viene eseguito quando si verifica un problema con un'attività di configurazione o quando il file PostIC.cmd restituisce un valore diverso da 0.  
  
##### <a name="to-define-notifications"></a>Per definire le notifiche  
  
1.  Scrivere il codice che viene chiamato dallo script SetupFailure.cmd.  
  
2.  Utilizzando blocco note, creare un file denominato SetupFailure.cmd e aggiungere la chiamata al codice creato nel passaggio 1. Assicurarsi che il codice restituisca un valore di esito positivo.  
  
3.  Salvare SetupFailure.cmd in C:\Windows\Setup\Scripts.  
  
## <a name="see-also"></a>Vedere anche  
 [Guida introduttiva a Windows Server Essentials ADK](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Ulteriori personalizzazioni](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Test di analisi utilizzo software](Testing-the-Customer-Experience.md)