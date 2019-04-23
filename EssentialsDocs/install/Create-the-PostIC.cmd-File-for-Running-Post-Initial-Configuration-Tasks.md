---
title: Creazione del file PostIC.cmd per eseguire le attività successive alla configurazione iniziale
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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59844122"
---
# <a name="create-the-posticcmd-file-for-running-post-initial-configuration-tasks"></a>Creazione del file PostIC.cmd per eseguire le attività successive alla configurazione iniziale

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

È possibile aggiungere personalizzazioni alla configurazione post-iniziale scrivendo un proprio codice e quindi richiamandolo da un file script chiamato PostIC.cmd. Quando si utilizza il file PostIC.cmd, è necessario seguire queste linee guida:  
  
-   Il codice di personalizzazione deve essere eseguito in modo automatico (non è possibile visualizzare un'interfaccia utente).  
  
-   Non è possibile riavviare il server con il codice di personalizzazione. Come ultima attività, con la configurazione iniziale verrà riavviato il server.  
  
-   Il codice di personalizzazione deve essere eseguito in un massimo di tre minuti.  
  
 Impostare il file PostIC.cmd in modo che restituisca il valore 0 se il codice viene eseguito correttamente. Se vengono restituiti altri valori, il sistema operativo ricerca un file denominato [SetupFailure.cmd](Create-the-PostIC.cmd-File-for-Running-Post-Initial-Configuration-Tasks.md#BKMK_SetupFailure), nel quale è contenuto il codice da eseguire qualora il file PostIC.cmd non venga eseguito correttamente. Entrambi i file PostIC.cmd e SetupFailure.cmd devono trovarsi in C:\Windows\Setup\Scripts.  
  
#### <a name="to-define-post-initial-configuration-customizations"></a>Per definire le personalizzazioni successive alla configurazione iniziale  
  
1.  Scrivere il codice chiamato dallo script PostIC.cmd.  
  
2.  Utilizzando Blocco note, creare un file denominato PostIC.cmd e aggiungere la chiamata al codice creato al passo 1. Accertarsi che il codice restituisca un valore che indica la corretta esecuzione.  
  
3.  Salvare PostIC.cmd in C:\Windows\Setup\Scripts.  
  
4.  (Facoltativo) Creare un file SetupFailure.cmd che esegua il codice se PostIC.cmd restituisce un valore diverso da 0.  
  
###  <a name="BKMK_SetupFailure"></a> SetupFailure.cmd  
 È possibile fornire la notifica dei problemi che si verificano nella configurazione iniziale utilizzando SetupFailure.cmd. Il file SetupFailure.cmd contiene il codice da eseguire se si verificano dei problemi. Il file SetupFailure.cmd si trova in C:\Windows\Setup\Scripts e viene eseguito qualora si verifichi un problema con un'attività di configurazione o se il file PostIC.cmd restituisce un valore diverso da 0.  
  
##### <a name="to-define-notifications"></a>Per definire le notifiche  
  
1.  Scrivere il codice richiamato dallo script SetupFailure.cmd.  
  
2.  Utilizzando Blocco note, creare un file denominato SetupFailure.cmd e aggiungere la chiamata al codice creato al passo 1. Accertarsi che il codice restituisca un valore che indica la corretta esecuzione.  
  
3.  Salvare SetupFailure.cmd in C:\Windows\Setup\Scripts.  
  
## <a name="see-also"></a>Vedere anche  
 [Guida introduttiva a Windows Server Essentials ADK](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Personalizzazioni aggiuntive](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Testare l'esperienza dei clienti](Testing-the-Customer-Experience.md)