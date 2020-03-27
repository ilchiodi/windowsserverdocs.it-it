---
title: Eseguire la transizione da Windows Server Essentials a Windows Server 2012 Standard
description: Viene descritto come usare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 51bcf124-c215-4e9d-9fa8-a90fa2c2fa22
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 0d7ed80f61dcfa313f867afda5689b2c64b1406a
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318699"
---
# <a name="transition-from-windows-server-essentials-to-windows-server-2012-standard"></a>Eseguire la transizione da Windows Server Essentials a Windows Server 2012 Standard

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

 Windows Server® 2012 Essentials supporta fino a 25 utenti e 50 dispositivi. Quando le esigenze aziendali superano il limite, è possibile eseguire una transizione di licenza sul posto da Windows Server Essentials a Windows Server 2012 standard per mantenere la conformità con le licenze.  
  
## <a name="how-the-transition-affects-user-and-device-limits"></a>Impatto della transizione sui limiti relativi a utenti e dispositivi  
 Dopo aver eseguito la transizione a Windows Server 2012 standard, i limiti relativi agli account utente e ai dispositivi vengono rimossi, ma le funzionalità esclusive di Windows Server Essentials, ad esempio il dashboard, il Accesso Web remoto e il backup dei computer client, rimangono comunque disponibili. Queste funzionalità prevedono tuttavia alcuni limiti tecnici, in quanto supportano un massimo di 75 account utente e 75 dispositivi. Se è necessario aggiungere più di 75 account utente o dispositivi, è necessario disattivare le funzionalità di Windows Server Essentials e usare gli strumenti nativi standard di Windows Server 2012 per gestire gli account utente e i dispositivi.  
  
> [!IMPORTANT]
>   Windows Server 2012 standard richiede una licenza CAL (Client Access License) per ogni utente o dispositivo nell'ambiente in uso. Questa operazione è diversa da Windows Server Essentials, che non utilizza il modello CAL e non dispone di licenze CAL.  Quando si esegue la transizione da Windows Server Essentials a Windows Server 2012 standard, sarà necessario acquistare il numero e il tipo di licenze CAL appropriati per l'ambiente in uso (la maggior parte dei clienti acquista le licenze CAL utente).  
  
## <a name="before-the-transition"></a>Prima della transizione  
  
-   Prima di passare da Windows Server Essentials a Windows Server 2012 standard, è necessario eseguire il backup completo dei dati del server.  
  
    > [!IMPORTANT]
    >  Se non si dispone di un backup completo del server, non sarà possibile ripristinare il server allo stato in cui si trovava prima della transizione.  
  
-   Inoltre, assicurarsi di leggere e comprendere il contratto di licenza con l'utente finale (EULA) per Windows Server 2012 standard. Per visualizzare il Contratto di licenza con l'utente finale:  
  
    1.  Aprire una finestra di comando come amministratore.  
  
    2.  Eseguire il comando seguente:  
  
         **Dism/online/set-Edition: ServerStandard/geteula: EULA percorso**  
  
         Dove **eula path** rappresenta il percorso in cui si desidera salvare il file del contratto di licenza con l'utente finale. Ad esempio C:\ws8std_eula.rtf  Accertarsi di usare l'estensione file rtf.  
  
    3.  Aprire il percorso in cui è stato salvato il file e quindi fare doppio clic su di esso per aprirlo.  
  
## <a name="transition-to--windows-server-2012-standard"></a>Transizione a Windows Server 2012 standard  
 Dopo aver deciso di eseguire la transizione da Windows Server Essentials a Windows Server 2012 standard, completare i due passaggi seguenti:  
  
1. Acquistare una licenza per Windows Server 2012 standard e il numero appropriato di licenze di accesso client per utente e/o dispositivo per l'ambiente in uso.  
  
    È possibile acquistare una licenza per Windows Server 2012 standard da un punto vendita al dettaglio, da un distributore o con l'aiuto di un [partner Microsoft](https://pinpoint.microsoft.com/SelectCulture.aspx).  
  
   > [!NOTE]
   >  Se inizialmente è stato acquistato Windows Server 2012 standard ed è stato eseguito il downgrade dei diritti per l'installazione di una delle due istanze virtuali come Windows Server Essentials, non è necessario acquistare altre attività.  
   >   
   >  Se si acquista Windows Server 2012 standard tramite il canale contratti multilicenza, è possibile scaricare un'immagine ISO e un codice Product Key per Windows Server 2012 standard da Volume Licensing Service Center (VLSC).  
   >   
   >  Se si acquista Windows Server 2012 standard da tutti gli altri canali, è possibile scaricare un'immagine ISO e un codice Product Key di valutazione per Windows Server Essentials da [TechNet Evaluation Center](https://technet.microsoft.com/evalcenter/jj659306.aspx). Se si esegue la transizione come descritto nel passaggio successivo, il prodotto di valutazione verrà convertito in un prodotto supportato con licenza completa.  
  
2. Aprire Windows PowerShell come amministratore ed eseguire il comando seguente.  
  
    **DISM/online/set-Edition: ServerStandard/AcceptEula/ProductKey:** *codice Product Key*  
  
    Dove *Product Key* è il codice Product Key per la copia di Windows Server 2012 standard.  
  
    Il server viene riavviato per completare il processo di transizione.  
  
   Dopo la transizione, le funzionalità di Windows Server Essentials rimangono nel server e sono supportate per un massimo di 75 utenti e 75 dispositivi. Se si supera uno di questi limiti, è necessario usare gli strumenti nativi standard di Windows Server 2012 per gestire gli account utente e i dispositivi.  
  
   Inoltre, dopo aver eseguito la transizione a Windows Server 2012 standard, le funzionalità multimediali di Windows Server Essentials non sono più disponibili. le funzionalità multimediali di Accesso Web remoto e le Impostazioni multimediali del dashboard, non sono più disponibili.  
  
## <a name="turn-off--windows-server-essentials-features"></a>Disattivare le funzionalità di Windows Server Essentials  
 Se il dashboard di Windows Server Essentials o altre funzionalità a valore aggiunto non sono più necessari per gestire il server, è possibile disattivare le funzionalità e rimuoverle dal server.  
  
 La **disattivazione guidata funzionalità di Windows Server Essentials** consente di disinstallare le funzionalità di. Pulisce anche il server dei file creati dal software server di Windows Server Essentials.  Alcune operazioni di pulizia vengono eseguite immediatamente, mentre altre verranno eseguite al riavvio del server.  
  
 La **disattivazione guidata funzionalità di Windows Server Essentials** richiede la disinstallazione manuale di tutti i componenti aggiuntivi prima che sia possibile completare la procedura guidata. Per visualizzare l'elenco dei componenti aggiuntivi installati, aprire la pagina applicazione nel dashboard. Se vengono rilevati componenti aggiuntivi installati, la procedura guidata avvisa l'utente e gli chiede di disinstallarli.  
  
 La **disattivazione guidata funzionalità di Windows Server Essentials** consente di scegliere se memorizzare i file di backup per i computer client dopo la disattivazione delle funzionalità di Windows Server Essentials.  
  
 Esistono due modi per eseguire la **disattivazione guidata funzionalità di Windows Server Essentials** dal Dashboard:  
  
#### <a name="from-the-alert"></a>Dall'avviso  
  
1.  Nel Dashboard aprire il Visualizzatore avvisi.  
  
2.  In organizza elenco selezionare l'avviso che segnala le informazioni sulla disattivazione delle funzionalità di Windows Server Essentials dopo la transizione.  
  
3.  Nell'avviso fare clic su **Disattiva le funzionalità di Windows Server Essentials**.  
  
#### <a name="from-the-get-help-and-support-pane"></a>Dal riquadro Guida e supporto tecnico  
  
1. Nella home page fare clic su Guida e supporto tecnico.  
  
2. Fare clic su **Disattiva la procedura guidata funzionalità di Windows Server Essentials**.  
  
   È possibile che alcune attività eseguite dalla **disattivazione guidata funzionalità di Windows Server Essentials** non vengano completate correttamente. Talvolta, questo problema potrebbe impedire l'esecuzione del dashboard. In tal caso, è possibile avviare la procedura guidata manualmente, mediante l'esecuzione del file:  
  
   **%systemdrive%\Program Windows Server\Bin\TurnOffFeaturesWizard.exe**  
  
## <a name="see-also"></a>Vedere anche  
  

-   [Transizione a Windows Server 2012 R2 Standard](Transition-from-Windows-Server-2012-R2-Essentials-to-Windows-Server-2012-R2-Standard.md)  
  
-   [Eseguire la migrazione dei dati del server a Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [Transizione a Windows Server 2012 R2 Standard](../migrate/Transition-from-Windows-Server-2012-R2-Essentials-to-Windows-Server-2012-R2-Standard.md)  
  
-   [Eseguire la migrazione dei dati del server a Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

