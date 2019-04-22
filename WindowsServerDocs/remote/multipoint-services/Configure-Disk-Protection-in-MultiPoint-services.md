---
title: Configurare Protezione disco in MultiPoint Services
description: Informazioni su come configurare la protezione del disco per i servizi MultiPoint
ms.custom: na
ms.prod: windows-server-threshold
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
ms.openlocfilehash: dc0a46b57753f08cc7d79fd05de7a9e81469cc86
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59820172"
---
# <a name="configure-disk-protection"></a>Configurare la protezione disco
È possibile usare la protezione disco in Multipoint Services per proteggere il volume di sistema da derivare aggiornamenti, pianificare gli aggiornamenti di Windows deve essere mantenuto mentre è attiva, disabilitare temporaneamente la protezione disco e la disinstallazione di protezione disco di protezione su disco.  
  
Abilita protezione disco in MultiPoint Services, è possibile proteggere il volume di sistema (l'unità in cui è installato Windows, in genere c) da modifiche indesiderate. Quando è abilitata la protezione disco, le modifiche apportate al volume di sistema vengono archiviate in un percorso temporaneo in modo che è sufficiente il riavvio del computer di eliminarli e restituisce automaticamente il sistema allo stato precedente noto.  
  
L'amministratore può facilmente installare software o apportare modifiche di configurazione tramite la disabilitazione temporanea della protezione del disco. Per evitare che il sistema corrente con gli aggiornamenti di Windows e le definizioni anti-malware, protezione disco consente di pianificare una finestra di manutenzione per scaricare e installare gli aggiornamenti. L'amministratore può anche fornire uno script personalizzato da eseguire durante la finestra di manutenzione per soddisfare le esigenze di manutenzione di là di Windows Update.  
  
## <a name="enable-disk-protection"></a>Abilitare la protezione del disco  
Prima di abilitare Protezione disco, assicurarsi che tutte le applicazioni e i driver siano installati e aggiornati e spostare i profili utente in un volume che non verrà protetti. Se è necessario eseguire gli aggiornamenti manuali dopo aver abilitato la protezione disco, è possibile disabilitare temporaneamente protezione su disco. Tuttavia, è più semplice ottenere il sistema in uno stato ideale prima protezione disco sia attivata.  
  
 
1.  Accedere al server che esegue servizi MultiPoint come amministratore.  
  
2.  Prima di abilitare la protezione su disco:  
  
    -   Verificare che i servizi MultiPoint sistema è esattamente lo stato in cui si desidera mantenerlo. Ad esempio, assicurarsi che il software installato, le impostazioni di sistema e gli aggiornamenti siano corretti.  
  
    -   Spostare i profili utente in un volume non protetto, o configurare un percorso di file condiviso dal volume di sistema come descritto [Abilita Condivisione file in MultiPoint Services](Enable-file-sharing-in-MultiPoint-services.md).  
  
3.  Dal **avviare** schermata, aprire **MultiPoint Manager**.  
  
4.  Fare clic sui **Home** scheda, fare clic su **abilitare Protezione disco**, quindi fare clic su **OK**.  
  
Quando Protezione disco è abilitata per la prima volta, il sistema è pronto, installazione di un driver e creare un file di cache nel volume di sistema. Il file di cache archivia temporaneamente le modifiche apportate al volume del sistema mentre è attiva la protezione disco. Poiché gli aggiornamenti del sistema vengono archiviati nel file di cache, non modificano il contenuto protetto del volume all'esterno del file di cache. Ogni volta che viene avviato il sistema, il file di cache viene reimpostato, che rimuove tutte le modifiche archiviate vengono eliminati dopo l'avvio del sistema precedente. Di conseguenza, il sistema inizi sempre con lo stesso se è stata abilitata la protezione del disco.  
  
Windows deve aggiornare alcuni file system, inclusi i file di paging del sistema, percorso dump di arresto anomalo del sistema e i registri eventi. Tali file non vengono rimossi quando Protezione disco è abilitata. A tale scopo, viene creato un nuovo volume denominato DpReserved quando Protezione disco è abilitata per la prima volta, e tali file vengono spostati in tale volume. La partizione DpReserved non è protetta, in modo da salvare in modo permanente scritture a tali file con il riavvio, anche quando è abilitata la protezione disco.  
  
## <a name="schedule-software-updates"></a>Pianificare gli aggiornamenti software  
Se Windows è configurato per installare automaticamente gli aggiornamenti di Windows, protezione disco consente gli aggiornamenti al momento configurata e non elimina gli aggiornamenti. Ad esempio, se gli aggiornamenti di Windows vengono pianificati per 3:00, protezione disco di verifica per gli aggiornamenti ogni giorno alle 3:00 Se vengono trovati aggiornamenti, servizi MultiPoint temporaneamente Disabilita protezione disco, applica gli aggiornamenti e quindi Abilita di nuovo la protezione disco.  
   
1.  Selezionare Gestione MultiPoint, visualizzare il **casa** scheda e quindi fare clic su **pianificare gli aggiornamenti software**.  
  
2.  Nella finestra di dialogo pianificare gli aggiornamenti Software, fare clic su **aggiornate di volta in**e selezionare, ad esempio, un'ora per gli aggiornamenti - **3:00 AM**.  
  
3.  Selezionare il **eseguire Windows Update** casella di controllo.  
  
4.  Se l'organizzazione esegue il proprio script di aggiornamento, selezionare la **eseguire il programma seguente** casella di controllo e specificare il percorso dello script di aggiornamento della propria organizzazione.  
  
5.  Selezionare un tempo massimo per consentire gli aggiornamenti per l'esecuzione.  
  
6.  Sotto **al termine**, scegliere se restituire lo stato dell'alimentazione precedente o arrestato dopo aver applicato gli aggiornamenti del sistema.  
  
7.  Fare clic su **OK**.  
  
## <a name="temporarily-disable-disk-protection"></a>Disabilitare temporaneamente la protezione disco  
Se un amministratore deve installare il software, modificare le impostazioni di sistema o eseguire altre attività di manutenzione che coinvolgono gli aggiornamenti del sistema, è possibile disabilitare temporaneamente protezione su disco. Dopo aver apportato le modifiche, riabilitare la protezione del disco. Durante il riavvio del sistema, il sistema conserverà lo stato quando Protezione disco è stata abilitata.  
    
1.  Selezionare Gestione MultiPoint il **Home** scheda.  
  
2.  Nella scheda Home fare clic su **disabilitare Protezione disco**, quindi fare clic su **OK**.  
  
> [!NOTE]  
> Ricordarsi di riabilitare la protezione disco dopo la manutenzione è stata completata. Il sistema non verrà protetti nuovamente fino a quando l'amministratore in modo esplicito riabilita la protezione disco.  
  
## <a name="uninstall-disk-protection"></a>Disinstallare protezione disco  
Protezione disco di disinstallazione rimuove il driver e il file di cache, pertanto è consigliabile eseguire questa operazione solo se si desidera interrompere l'uso di protezione disco a lungo termine. Se si desidera semplicemente eseguire operazioni di manutenzione o arrestare temporaneamente la protezione dati, utilizzare l'attività di protezione disco Disable.  
  
È possibile disinstallare protezione disco se è abilitato o disabilitato.  
   
1.  Selezionare Gestione MultiPoint il **Home** scheda.  
  
2.  Nella scheda Home e fare clic su **disinstallare protezione disco**, quindi fare clic su **OK**.  
  
    Dopo aver fatto clic **OK**, il riavvio del computer. Il processo di disinstallazione richiede diversi riavvii, durante i quali il driver e file di cache vengono rimossi. Il DpReserved partizionare rimane e il file di paging, percorso di dump di arresto anomalo del sistema, log eventi e i file rimangono configurato per usare la partizione DpReserved.