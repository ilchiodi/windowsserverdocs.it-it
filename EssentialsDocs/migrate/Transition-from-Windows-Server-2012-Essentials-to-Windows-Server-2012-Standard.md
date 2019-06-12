---
title: Eseguire la transizione da Windows Server Essentials a Windows Server 2012 Standard
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 51bcf124-c215-4e9d-9fa8-a90fa2c2fa22
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 445472822de09263b84821e552c931ca19f14b2b
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66432528"
---
# <a name="transition-from-windows-server-essentials-to-windows-server-2012-standard"></a>Eseguire la transizione da Windows Server Essentials a Windows Server 2012 Standard

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

 Windows Server® 2012 Essentials supporta fino a 25 utenti e 50 dispositivi. Quando l'azienda deve superare il limite, è possibile eseguire una transizione di licenza sul posto da Windows Server Essentials a Windows Server 2012 Standard per mantenere la conformità delle licenze.  
  
## <a name="how-the-transition-affects-user-and-device-limits"></a>Impatto della transizione sui limiti relativi a utenti e dispositivi  
 Dopo la transizione a Windows Server 2012 Standard, i limiti di account e i dispositivi utente vengono rimossi, ma le funzionalità esclusive di Windows Server Essentials (ad esempio Dashboard, accesso Web remoto e backup dei computer client), rimangono comunque disponibili. Queste funzionalità prevedono tuttavia alcuni limiti tecnici, in quanto supportano un massimo di 75 account utente e 75 dispositivi. Se si rende necessario aggiungere più di 75 account utente o dispositivi, è necessario disattivare le funzionalità di Windows Server Essentials e usare gli strumenti nativi di Windows Server 2012 Standard per gestire i dispositivi e gli account utente.  
  
> [!IMPORTANT]
>   Windows Server 2012 Standard richiede una licenza CAL (Client Access License) per ogni utente o dispositivo nell'ambiente in uso. Ciò è diverso da Windows Server Essentials, che non usa il modello CAL e che non include alcuna licenza CAL.  Durante la transizione da Windows Server Essentials a Windows Server 2012 Standard, dovrai acquistare il numero appropriato e il tipo di licenze CAL per l'ambiente (la maggior parte dei clienti Acquista licenze CAL utente).  
  
## <a name="before-the-transition"></a>Prima della transizione  
  
-   Prima di una transizione da Windows Server Essentials a Windows Server 2012 Standard, è consigliabile eseguire un backup completo dei dati del server.  
  
    > [!IMPORTANT]
    >  Se non si dispone di un backup completo del server, non sarà possibile ripristinare il server allo stato in cui si trovava prima della transizione.  
  
-   Inoltre, assicurarsi di leggere e accettare il contratto di licenza con l'utente finale (EULA) per Windows Server 2012 Standard. Per visualizzare il Contratto di licenza con l'utente finale:  
  
    1.  Aprire una finestra di comando come amministratore.  
  
    2.  Eseguire il comando seguente:  
  
         **dism /online /set-edition:ServerStandard /geteula: eula path**  
  
         Dove **eula path** rappresenta il percorso in cui si desidera salvare il file del contratto di licenza con l'utente finale. Ad esempio C:\ws8std_eula.rtf  Accertarsi di usare l'estensione file rtf.  
  
    3.  Aprire il percorso in cui è stato salvato il file e quindi fare doppio clic su di esso per aprirlo.  
  
## <a name="transition-to--windows-server-2012-standard"></a>Transizione a Windows Server 2012 Standard  
 Dopo aver deciso di eseguire la transizione da Windows Server Essentials a Windows Server 2012 Standard, completo di questi due passaggi:  
  
1. Acquistare una licenza per Windows Server 2012 Standard e il numero appropriato di licenze di accesso Client utente e/o dispositivo per l'ambiente.  
  
    È possibile acquistare una licenza per Windows Server 2012 Standard presso un negozio al dettaglio, un server di distribuzione o con l'aiuto di un [Partner Microsoft](https://pinpoint.microsoft.com/SelectCulture.aspx).  
  
   > [!NOTE]
   >  Se inizialmente acquistato Windows Server 2012 Standard ed esercitato il diritto di downgrade per installare una delle due istanze virtuali disponibili come Windows Server Essentials, non occorre acquistare altre.  
   >   
   >  Se si acquista Windows Server 2012 Standard tramite il canale contratti multilicenza, è possibile scaricare un'immagine ISO e un codice product key per Windows Server 2012 Standard da Volume Licensing Service Center (VLSC).  
   >   
   >  Se si acquista Windows Server 2012 Standard da tutti gli altri canali può scaricare un'immagine ISO e un codice product key di valutazione per Windows Server Essentials dal [TechNet Evaluation Center](https://technet.microsoft.com/evalcenter/jj659306.aspx). Se si esegue la transizione come descritto nel passaggio successivo, il prodotto di valutazione verrà convertito in un prodotto supportato con licenza completa.  
  
2. Aprire Windows PowerShell come amministratore ed eseguire il comando seguente.  
  
    **dism /online /set-edition:ServerStandard /accepteula /productkey:** *Codice Product Key*  
  
    In cui *codice Product Key* è il codice product key per la copia di Windows Server 2012 Standard.  
  
    Il server viene riavviato per completare il processo di transizione.  
  
   Al termine della transizione, le funzionalità di Windows Server Essentials rimangono nel server e sono supportate per un massimo di 75 utenti e 75 dispositivi. Se si supera uno di questi limiti, è necessario usare gli strumenti nativi di Windows Server 2012 Standard per gestire dispositivi e gli account utente.  
  
   Inoltre, dopo la transizione a Windows Server 2012 Standard, le funzionalità multimediali di Windows Server Essentials non sono più disponibili. le funzionalità multimediali di Accesso Web remoto e le Impostazioni multimediali del dashboard, non sono più disponibili.  
  
## <a name="turn-off--windows-server-essentials-features"></a>Disattivare la funzionalità di Windows Server Essentials  
 Se non è più necessario il Dashboard di Windows Server Essentials o altre funzionalità a valore aggiunto per gestire il server, è possibile disattivarle e rimuoverle dal server.  
  
 Il **disattivazione guidata funzionalità di Windows Server Essentials** consente di disinstallare le funzionalità. Elimina anche il server di file che sono stati creati dal software di server Windows Server Essentials.  Alcune operazioni di pulizia vengono eseguite immediatamente, mentre altre verranno eseguite al riavvio del server.  
  
 Il **disattivazione guidata funzionalità di Windows Server Essentials** richiede la disinstallazione manualmente tutti i componenti aggiuntivi per poter completare la procedura guidata. Per visualizzare l'elenco dei componenti aggiuntivi installati, aprire la pagina applicazione nel dashboard. Se vengono rilevati componenti aggiuntivi installati, la procedura guidata avvisa l'utente e gli chiede di disinstallarli.  
  
 Il **disattivazione guidata funzionalità di Windows Server Essentials** consente di scegliere se conservare i file di backup per il client computer dopo avere disattivato le funzionalità di Windows Server Essentials.  
  
 Esistono due modi per eseguire la **disattivazione guidata funzionalità di Windows Server Essentials** dal Dashboard:  
  
#### <a name="from-the-alert"></a>Dall'avviso  
  
1.  Aprire il Visualizzatore avvisi dal dashboard.  
  
2.  In organizza elenco selezionare l'avviso che segnala l'informazione relativa alla disattivazione delle funzionalità di Windows Server Essentials dopo la transizione.  
  
3.  Nell'avviso, fare clic su **disattivare le funzionalità di Windows Server Essentials**.  
  
#### <a name="from-the-get-help-and-support-pane"></a>Dal riquadro Guida e supporto tecnico  
  
1. Nella home page fare clic su Guida e supporto tecnico.  
  
2. Fare clic su **disattivazione guidata funzionalità di Windows Server Essentials**.  
  
   È possibile che alcune attività eseguite per la **disattivazione guidata funzionalità di Windows Server Essentials** non verrà completata correttamente. Talvolta, questo problema potrebbe impedire l'esecuzione del dashboard. In tal caso, è possibile avviare la procedura guidata manualmente, mediante l'esecuzione del file:  
  
   **%systemdrive%\Program Files\Windows Server\Bin\TurnOffFeaturesWizard.exe**  
  
## <a name="see-also"></a>Vedere anche  
  

-   [Transizione a Windows Server 2012 R2 Standard](Transition-from-Windows-Server-2012-R2-Essentials-to-Windows-Server-2012-R2-Standard.md)  
  
-   [Eseguire la migrazione dei dati del server a Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [Transizione a Windows Server 2012 R2 Standard](../migrate/Transition-from-Windows-Server-2012-R2-Essentials-to-Windows-Server-2012-R2-Standard.md)  
  
-   [Eseguire la migrazione dei dati del server a Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

