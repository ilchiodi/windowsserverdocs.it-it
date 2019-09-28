---
title: Configurare Protezione disco in MultiPoint Services
description: Informazioni su come configurare la protezione del disco per servizi MultiPoint
ms.custom: na
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: bd9bf5b9-e481-499b-9c15-7ee5a4f470c4
author: evaseydl
manager: scottman
ms.author: evas
ms.date: 08/04/2016
ms.openlocfilehash: ae930162de32335ac32e3bda0ac381a26c5ea6dd
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71389825"
---
# <a name="configure-disk-protection"></a>Configurare la protezione del disco
È possibile utilizzare la protezione del disco in MultiPoint Services per proteggere il volume di sistema da aggiornamenti imprevisti, pianificare la conservazione degli aggiornamenti di Windows mentre è attiva la protezione del disco, disabilitare temporaneamente la protezione del disco e disinstallare la protezione del disco.  
  
Abilitando la protezione del disco in MultiPoint Services, è possibile proteggere il volume di sistema (l'unità in cui è installato Windows, in genere C:) da modifiche indesiderate. Quando la protezione del disco è abilitata, le modifiche apportate al volume di sistema vengono archiviate in un percorso temporaneo, in modo che il riavvio del computer lo elimini e ritorni automaticamente il sistema allo stato precedente valido.  
  
L'amministratore può installare facilmente il software o apportare modifiche alla configurazione disabilitando temporaneamente la protezione del disco. Per mantenere aggiornato il sistema con le definizioni degli aggiornamenti di Windows e anti-malware, la protezione del disco pianifica una finestra di manutenzione per scaricare e installare gli aggiornamenti. L'amministratore può anche fornire uno script personalizzato da eseguire durante la finestra di manutenzione per soddisfare le esigenze di manutenzione oltre Windows Update.  
  
## <a name="enable-disk-protection"></a>Abilitare la protezione del disco  
Prima di abilitare la protezione del disco, verificare che tutte le applicazioni e i driver siano installati e aggiornati e spostare i profili utente in un volume che non verrà protetto. Se è necessario eseguire aggiornamenti manuali dopo aver abilitato la protezione del disco, è possibile disabilitare temporaneamente la protezione del disco. Tuttavia, è più semplice ottenere il sistema in uno stato ideale prima che la protezione del disco sia attivata.  
  
 
1.  Accedere al server che esegue MultiPoint Services come amministratore.  
  
2.  Prima di abilitare la protezione del disco:  
  
    -   Verificare che il sistema MultiPoint Services sia esattamente nello stato in cui si desidera che rimanga. Verificare, ad esempio, che il software installato, le impostazioni di sistema e gli aggiornamenti siano corretti.  
  
    -   Spostare i profili utente in un volume non protetto o impostare un percorso di file condiviso dal volume di sistema, come descritto in [abilitare la condivisione di file in MultiPoint Services](Enable-file-sharing-in-MultiPoint-services.md).  
  
3.  Dalla schermata **Start** aprire **Gestione MultiPoint**.  
  
4.  Fare clic sulla scheda **Home** , fare clic su **Abilita protezione disco**, quindi fare clic su **OK**.  
  
Quando la protezione del disco è abilitata per la prima volta, il sistema viene preparato installando un driver e creando un file di cache nel volume di sistema. Il file di cache archivia temporaneamente le modifiche apportate al volume di sistema, mentre la protezione del disco è attiva. Poiché gli aggiornamenti del sistema vengono archiviati nel file di cache, non modificano il contenuto protetto del volume all'esterno del file di cache. Ogni volta che il sistema viene avviato, il file di cache viene reimpostato, che elimina tutte le modifiche archiviate dall'avvio del sistema precedente. Pertanto, il sistema viene sempre avviato nello stesso stato in cui è stata abilitata la protezione del disco.  
  
Windows deve aggiornare alcuni file di sistema, tra cui il file di paging del sistema, il percorso del dump di arresto anomalo del sistema e i registri eventi. Questi file non vengono rimossi quando la protezione del disco è abilitata. A tale scopo, viene creato un nuovo volume denominato DpReserved quando la protezione del disco è abilitata per la prima volta e tali file vengono spostati in tale volume. La partizione DpReserved non è protetta, quindi le Scritture in tali file vengono mantenute durante i riavvii, anche quando la protezione del disco è abilitata.  
  
## <a name="schedule-software-updates"></a>Pianificare gli aggiornamenti software  
Se Windows è configurato per installare automaticamente gli aggiornamenti di Windows, la protezione del disco consente questi aggiornamenti in fase di configurazione e non rimuove gli aggiornamenti. Se, ad esempio, gli aggiornamenti di Windows sono pianificati per le ore 3:00, la protezione del disco controlla la disponibilità di aggiornamenti ogni giorno alle 3:00. Se vengono rilevati aggiornamenti, MultiPoint Services Disabilita temporaneamente la protezione del disco, applica gli aggiornamenti e quindi Abilita nuovamente la protezione del disco.  
   
1.  In Gestione MultiPoint visualizzare la scheda **Home** , quindi fare clic su **Pianifica aggiornamenti software**.  
  
2.  Nella finestra di dialogo Pianifica aggiornamenti software, fare clic su **Aggiorna in**e selezionare un'ora per gli aggiornamenti, ad esempio **3:00 AM**.  
  
3.  Selezionare la casella di controllo **esegui Windows Update** .  
  
4.  Se l'organizzazione esegue uno script di aggiornamento, selezionare la casella di controllo **Esegui il programma seguente** e specificare il percorso dello script di aggiornamento dell'organizzazione.  
  
5.  Selezionare un tempo massimo per consentire l'esecuzione degli aggiornamenti.  
  
6.  In al **termine**scegliere se il sistema deve tornare allo stato di alimentazione precedente o arrestarsi dopo l'applicazione degli aggiornamenti.  
  
7.  Fare clic su **OK**.  
  
## <a name="temporarily-disable-disk-protection"></a>Disabilitare temporaneamente la protezione disco  
Se un amministratore deve installare software, modificare le impostazioni di sistema o eseguire altre attività di manutenzione che coinvolgono gli aggiornamenti del sistema, può disabilitare temporaneamente la protezione del disco. Dopo aver apportato le modifiche, abilitare di nuovo la protezione del disco. Durante i riavvii del sistema, il sistema manterrà il proprio stato quando la protezione del disco è stata abilitata.  
    
1.  In Gestione MultiPoint fare clic sulla scheda **Home** .  
  
2.  Nella scheda Home fare clic su **Disabilita protezione disco**, quindi fare clic su **OK**.  
  
> [!NOTE]  
> Ricordarsi di riabilitare la protezione del disco dopo aver completato la manutenzione. Il sistema non verrà più protetto finché l'amministratore non riabilita in modo esplicito la protezione del disco.  
  
## <a name="uninstall-disk-protection"></a>Disinstalla protezione disco  
La disinstallazione della protezione disco rimuove il driver e il file di cache, quindi è consigliabile eseguire questa operazione solo se si desidera interrompere l'utilizzo della protezione del disco a lungo termine. Se si desidera semplicemente eseguire una manutenzione o arrestare temporaneamente la protezione, utilizzare invece l'attività disabilita protezione disco.  
  
È possibile disinstallare la protezione del disco se è abilitata o disabilitata.  
   
1.  In Gestione MultiPoint fare clic sulla scheda **Home** .  
  
2.  Nella scheda Home, fare clic su **Disinstalla protezione disco**, quindi fare clic su **OK**.  
  
    Dopo aver fatto clic su **OK**, il computer viene riavviato. Il processo di disinstallazione richiede diversi riavvii durante i quali il driver e il file di cache vengono rimossi. La partizione DpReserved rimane e il file di paging, il percorso del dump di arresto anomalo e i file del registro eventi rimangono configurati per l'uso della partizione DpReserved.