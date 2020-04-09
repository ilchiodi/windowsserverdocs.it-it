---
title: Creazione del file PostIC.cmd per eseguire le attività successive alla configurazione iniziale
description: Viene descritto come usare Windows Server Essentials
ms.date: 10/03/2016
ms.prod: windows-server
ms.topic: article
ms.assetid: 99e258bc-0695-48c9-b694-a7f3cbe2a2d0
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 4655db35803680b7b1ebd0d2da29a3a09f135b63
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80818174"
---
# <a name="create-the-posticcmd-file-for-running-post-initial-configuration-tasks"></a>Creazione del file PostIC.cmd per eseguire le attività successive alla configurazione iniziale

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

È possibile aggiungere personalizzazioni alla configurazione post-iniziale scrivendo un proprio codice e quindi richiamandolo da un file script chiamato PostIC.cmd. Quando si utilizza il file PostIC.cmd, è necessario seguire queste linee guida:  
  
- Il codice di personalizzazione deve essere eseguito in modo automatico (non è possibile visualizzare un'interfaccia utente).  
  
- Non è possibile riavviare il server con il codice di personalizzazione. Come ultima attività, con la configurazione iniziale verrà riavviato il server.  
  
- Il codice di personalizzazione deve essere eseguito in un massimo di tre minuti.  
  
  Impostare il file PostIC.cmd in modo che restituisca il valore 0 se il codice viene eseguito correttamente. Se vengono restituiti altri valori, il sistema operativo ricerca un file denominato [SetupFailure.cmd](Create-the-PostIC.cmd-File-for-Running-Post-Initial-Configuration-Tasks.md#BKMK_SetupFailure), nel quale è contenuto il codice da eseguire qualora l'esecuzione del file PostIC.cmd non avvenga correttamente. Entrambi i file PostIC.cmd e SetupFailure.cmd devono trovarsi in C:\Windows\Setup\Scripts.  
  
#### <a name="to-define-post-initial-configuration-customizations"></a>Per definire le personalizzazioni successive alla configurazione iniziale  
  
1.  Scrivere il codice chiamato dallo script PostIC.cmd.  
  
2.  Utilizzando Blocco note, creare un file denominato PostIC.cmd e aggiungere la chiamata al codice creato al passo 1. Accertarsi che il codice restituisca un valore che indica la corretta esecuzione.  
  
3.  Salvare PostIC.cmd in C:\Windows\Setup\Scripts.  
  
4.  (Facoltativo) Creare un file SetupFailure.cmd che esegua il codice se PostIC.cmd restituisce un valore diverso da 0.  
  
###  <a name="setupfailurecmd"></a><a name="BKMK_SetupFailure"></a>SetupFailure. cmd  
 È possibile fornire la notifica dei problemi che si verificano nella configurazione iniziale utilizzando SetupFailure.cmd. Il file SetupFailure.cmd contiene il codice da eseguire se si verificano dei problemi. Il file SetupFailure.cmd si trova in C:\Windows\Setup\Scripts e viene eseguito qualora si verifichi un problema con un'attività di configurazione o se il file PostIC.cmd restituisce un valore diverso da 0.  
  
##### <a name="to-define-notifications"></a>Per definire le notifiche  
  
1.  Scrivere il codice richiamato dallo script SetupFailure.cmd.  
  
2.  Utilizzando Blocco note, creare un file denominato SetupFailure.cmd e aggiungere la chiamata al codice creato al passo 1. Accertarsi che il codice restituisca un valore che indica la corretta esecuzione.  
  
3.  Salvare SetupFailure.cmd in C:\Windows\Setup\Scripts.  
  
## <a name="see-also"></a>Vedi anche  
 [Introduzione con Windows Server Essentials ADK](Getting-Started-with-the-Windows-Server-Essentials-ADK.md)   
 [Creazione e personalizzazione dell'immagine](Creating-and-Customizing-the-Image.md)   
 [Personalizzazioni aggiuntive](Additional-Customizations.md)   
 [Preparazione dell'immagine per la distribuzione](Preparing-the-Image-for-Deployment.md)   
 [Test di Analisi utilizzo software](Testing-the-Customer-Experience.md)