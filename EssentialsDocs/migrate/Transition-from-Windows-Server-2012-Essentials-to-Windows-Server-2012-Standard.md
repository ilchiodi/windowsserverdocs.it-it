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
ms.openlocfilehash: d2005b72adede72b718fa5b49b93435f5fbac1bd
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="transition-from-windows-server-essentials-to-windows-server-2012-standard"></a>Eseguire la transizione da Windows Server Essentials a Windows Server 2012 Standard

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

 Windows Server® 2012 Essentials supporta un massimo di 25 utenti e 50 dispositivi. Quando l'azienda deve superare il limite, è possibile eseguire una transizione di licenza sul posto da Windows Server Essentials a Windows Server 2012 Standard per mantenere la conformità delle licenze.  
  
## <a name="how-the-transition-affects-user-and-device-limits"></a>Limita l'impatto della transizione utenti e dispositivi  
 Dopo eseguire la transizione a Windows Server 2012 Standard, i limiti utente, account e i dispositivi vengono rimossi, ma le funzionalità esclusive di Windows Server Essentials (ad esempio il Dashboard, accesso Web remoto e backup dei computer client), rimangono comunque disponibili. Tuttavia, le limitazioni tecniche per queste funzionalità supportano un massimo di 75 account utente e 75 dispositivi. Se è necessario aggiungere più di 75 account utente o dispositivi, si dovrebbe disattivare le funzionalità di Windows Server Essentials e utilizzare gli strumenti nativi di Windows Server 2012 Standard per gestire i dispositivi e gli account utente.  
  
> [!IMPORTANT]
>   Windows Server 2012 Standard richiede una licenza CAL (Client Access License) per ogni utente o dispositivo presente nell'ambiente in uso. Questo è diverso da Windows Server Essentials, che non utilizza il modello CAL e che non include alcuna licenza CAL.  Durante la transizione da Windows Server Essentials a Windows Server 2012 Standard, sarà necessario acquistare il numero appropriato e il tipo di licenze di accesso client per l'ambiente (la maggior parte dei clienti Acquista licenze CAL utente).  
  
## <a name="before-the-transition"></a>Prima della transizione  
  
-   Prima di eseguire la transizione da Windows Server Essentials a Windows Server 2012 Standard, è necessario eseguire un backup completo dei dati del server.  
  
    > [!IMPORTANT]
    >  Senza un backup completo del server, è possibile ripristinare il server allo stato in cui si trovava prima della transizione.  
  
-   Inoltre, assicurarsi di leggere e accettare il contratto di licenza con l'utente finale (EULA) per Windows Server 2012 Standard. Per visualizzare il contratto di licenza:  
  
    1.  Aprire una finestra di comando come amministratore.  
  
    2.  Eseguire il comando seguente:  
  
         **DISM /online -/Set-Edition: ServerStandard /geteula: percorso eula**  
  
         Dove **percorso eula** rappresenta il percorso in cui si desidera salvare il file EULA. Ad esempio: C:\ws8std_eula.RTF..  Assicurati di usare l'estensione di file RTF.  
  
    3.  Aprire il percorso in cui è salvato il file e quindi fare doppio clic sul file per aprirlo.  
  
## <a name="transition-to--windows-server-2012-standard"></a>Eseguire la transizione a Windows Server 2012 Standard  
 Dopo aver deciso di eseguire la transizione da Windows Server Essentials a Windows Server 2012 Standard, completa questi due passaggi:  
  
1.  Acquistare una licenza per Windows Server 2012 Standard e il numero appropriato di licenze di accesso Client utente e/o dispositivo per l'ambiente.  
  
     È possibile acquistare una licenza per Windows Server 2012 Standard da un negozio al dettaglio, un distributore o con l'aiuto di un [Partner Microsoft](https://pinpoint.microsoft.com/SelectCulture.aspx).  
  
    > [!NOTE]
    >  Se Windows Server 2012 Standard acquistati inizialmente e si è esercitato il diritto di downgrade per installare una delle due istanze virtuali disponibili come Windows Server Essentials, non è necessario acquistare ulteriori licenze.  
    >   
    >  Se si acquistano Windows Server 2012 Standard attraverso il canale contratti multilicenza, è possibile scaricare un'immagine ISO e un codice product key per Windows Server 2012 Standard dal Volume Licensing Service Center (VLSC).  
    >   
    >  Se si è acquistato Windows Server 2012 Standard da tutti gli altri canali possibile scaricare un'immagine ISO e un codice product key valutazione per Windows Server Essentials dal [TechNet Evaluation Center](https://technet.microsoft.com/evalcenter/jj659306.aspx). Eseguire la transizione come descritto nel passaggio successivo, il prodotto di valutazione verrà convertito in un prodotto completo concesso in licenza e supportato.  
  
2.  Aprire Windows PowerShell come amministratore e quindi eseguire il comando seguente.  
  
     **DISM /online -/Set-Edition: ServerStandard /accepteula /productkey:** *codice Product Key*  
  
     Dove *codice Product Key* è il codice product key per la copia di Windows Server 2012 Standard.  
  
     Il server viene riavviato per completare il processo di transizione.  
  
 Al termine della transizione, le funzionalità di Windows Server Essentials rimangono nel server e sono supportate per un massimo di 75 utenti e 75 dispositivi. Se si supera uno di questi limiti, utilizzare gli strumenti nativi di Windows Server 2012 Standard per gestire dispositivi e gli account utente.  
  
 Inoltre, dopo eseguire la transizione a Windows Server 2012 Standard, le funzionalità multimediali di Windows Server Essentials non sono più disponibili. Ciò include le funzionalità multimediali di accesso Web remoto e le impostazioni multimediali del Dashboard.  
  
## <a name="turn-off--windows-server-essentials-features"></a>Disattivare le funzionalità di Windows Server Essentials  
 Se non è più necessario Dashboard di Windows Server Essentials o altre funzionalità a valore aggiunto per gestire il server, è possibile disattivare le funzionalità e rimuoverli dal server.  
  
 Il **disattivazione guidata funzionalità di Windows Server Essentials** consente di disinstallare le funzionalità. Elimina anche il server dei file che sono stati creati dal software di server Windows Server Essentials.  Alcune operazioni di pulizia vengono eseguite immediatamente, mentre altre verranno eseguite dopo il riavvio del server.  
  
 Il **disattivazione guidata funzionalità di Windows Server Essentials** è necessario disinstallare manualmente tutti i componenti aggiuntivi prima di poter completare la procedura guidata. Per visualizzare un elenco di componenti aggiuntivi installati, aprire la pagina applicazione nel Dashboard. La procedura guidata emetterà un avviso se rileva i componenti aggiuntivi installati e chiede di disinstallarli.  
  
 Il **disattivazione guidata funzionalità di Windows Server Essentials** consente di scegliere se conservare i file di backup per i client computer dopo avere disattivato le funzionalità di Windows Server Essentials.  
  
 Esistono due modi per eseguire il **disattivazione guidata funzionalità di Windows Server Essentials** dal Dashboard:  
  
#### <a name="from-the-alert"></a>Dall'avviso  
  
1.  Dal Dashboard, aprire il Visualizzatore avvisi.  
  
2.  In organizza elenco selezionare l'avviso che segnala l'informazione relativa alla disattivazione delle funzionalità di Windows Server Essentials dopo la transizione.  
  
3.  Nell'avviso, fare clic su **disattivare la funzionalità di Windows Server Essentials**.  
  
#### <a name="from-the-get-help-and-support-pane"></a>Dal riquadro Guida e supporto  
  
1.  Nella Home page, fare clic su Guida e supporto.  
  
2.  Fare clic su **disattivazione guidata funzionalità di Windows Server Essentials**.  
  
 È possibile che alcune attività eseguite dal **disattivazione guidata funzionalità di Windows Server Essentials** non verrà completata correttamente. In alcuni casi, ciò può impedire il Dashboard di esecuzione. In questo caso, è possibile avviare la procedura guidata manualmente eseguendo il file:  
  
 **%SystemDrive%\Programmi c:\Programmi\Windows Server\Bin\TurnOffFeaturesWizard.exe**  
  
## <a name="see-also"></a>Vedere anche  
  

-   [Eseguire la transizione a Windows Server 2012 R2 Standard](Transition-from-Windows-Server-2012-R2-Essentials-to-Windows-Server-2012-R2-Standard.md)  
  
-   [Eseguire la migrazione di dati del Server a Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [Eseguire la transizione a Windows Server 2012 R2 Standard](../migrate/Transition-from-Windows-Server-2012-R2-Essentials-to-Windows-Server-2012-R2-Standard.md)  
  
-   [Eseguire la migrazione di dati del Server a Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

