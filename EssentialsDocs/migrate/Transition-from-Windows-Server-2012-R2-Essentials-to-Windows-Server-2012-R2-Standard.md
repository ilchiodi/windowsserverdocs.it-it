---
title: Eseguire la transizione da Windows Server Essentials a Windows Server 2012 R2 Standard
description: Viene descritto come utilizzare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server-2016-essentials
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a14689e3-2310-4229-bd3e-dafc0e739e02
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: d371e24b17310c0687666185f56fe07a135ff91f
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="transition-from-windows-server-essentials-to-windows-server-2012-r2-standard"></a>Eseguire la transizione da Windows Server Essentials a Windows Server 2012 R2 Standard

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Windows Server 2016 è il sistema operativo di cloud computing che supporta i carichi di lavoro corrente mentre l'introduzione di nuove tecnologie che semplificano la transizione al cloud computing. Il contenuto di Windows Server 2016 consente di ottenere è pronto.

 Windows Server Essentials supporta un massimo di 25 utenti e 50 dispositivi. Quando l'azienda deve superare il limite, è possibile eseguire una transizione di licenza sul posto da Windows Server Essentials a Windows Server 2012 R2 Standard per mantenere la conformità delle licenze.  
  
 Dopo eseguire la transizione a Windows Server 2012 R2 Standard, i limiti utente, account e i dispositivi vengono rimossi, ma le funzionalità esclusive di Windows Server Essentials (ad esempio il Dashboard, accesso Web remoto e backup dei computer client) rimangono comunque disponibili. Tuttavia, le limitazioni tecniche per queste funzionalità supportano un massimo di 100 account utente e 200 dispositivi. La funzionalità di backup computer client consentirà il backup di un massimo di 75 dispositivi.  
  
> [!IMPORTANT]
>   Windows Server 2012 R2 Standard richiede una licenza di accesso client (CAL) per ogni utente o dispositivo presente nell'ambiente in uso. Questo è diverso da Windows Server Essentials, che non utilizza il modello CAL e che non include alcuna licenza CAL. Durante la transizione da Windows Server Essentials a Windows Server 2012 R2 Standard, sarà necessario acquistare il numero appropriato e il tipo di licenze di accesso client per l'ambiente (la maggior parte dei clienti Acquista licenze CAL utente).  
  
## <a name="before-the-transition"></a>Prima della transizione  
  
-   Prima di eseguire la transizione da Windows Server Essentials a Windows Server 2012 R2 Standard, è necessario eseguire un backup completo dei dati del server.  
  
    > [!IMPORTANT]
    >  Senza un backup completo del server, è possibile ripristinare il server allo stato in cui si trovava prima della transizione.  
  
-   Inoltre, assicurarsi di leggere e accettare il contratto di licenza con l'utente finale (EULA) per Windows Server 2012 R2 Standard. Per visualizzare il contratto di licenza:  
  
    1.  Aprire una finestra di comando come amministratore.  
  
    2.  Eseguire il comando seguente:  
  
         **DISM /online -/Set-Edition: ServerStandard /geteula:** *percorso eula* (in cui *percorso eula* rappresenta il percorso in cui si desidera salvare il file EULA, ad esempio: c:\ws8std_eula.RTF.). Assicurati di usare l'estensione di file RTF.  
  
    3.  Aprire il percorso in cui è salvato il file e quindi fare doppio clic sul file per aprirlo.  
  
## <a name="transition-to--windows-server-2012-r2-standard"></a>Eseguire la transizione a Windows Server 2012 R2 Standard  
 Dopo aver deciso di eseguire la transizione da Windows Server Essentials a Windows Server 2012 R2 Standard, completa questi due passaggi:  
  
1.  Acquistare una licenza per Windows Server 2012 R2 Standard e il numero appropriato di utente e/o di licenze di accesso client di dispositivo per l'ambiente.  
  
     È possibile acquistare una licenza per Windows Server 2012 R2 Standard da un negozio al dettaglio, un distributore o con l'aiuto di un [Partner Microsoft](https://pinpoint.microsoft.com/SelectCulture.aspx).  
  
    > [!NOTE]
    >  Se inizialmente acquistato Windows Server 2012 R2 Standard e si è esercitato il diritto di downgrade per installare una delle due istanze virtuali disponibili come Windows Server Essentials, non è necessario acquistare ulteriori licenze.  
    >   
    >  Se si acquistano Windows Server 2012 R2 Standard attraverso il canale contratti multilicenza, è possibile scaricare un'immagine ISO e un codice product key per Windows Server 2012 R2 Standard dal Volume Licensing Service Center (VLSC).  
    >   
    >  Se si acquistano Windows Server 2012 R2 Standard da un altro canale, è possibile scaricare un'immagine ISO e un codice product key valutazione per Windows Server Essentials dal [TechNet Evaluation Center](https://technet.microsoft.com/evalcenter/jj659306.aspx). Eseguire la transizione come descritto nel passaggio successivo, il prodotto di valutazione verrà convertito in un prodotto completo concesso in licenza e supportato.  
  
2.  Aprire Windows PowerShell come amministratore e quindi eseguire il comando seguente:  
  
     **DISM /online -/Set-Edition: ServerStandard /accepteula /productkey:** *codice Product Key* (in cui *codice Product Key* è il codice product key per la copia di Windows Server 2012 R2 Standard).  
  
     Il server viene riavviato per completare il processo di transizione.  
  
 Al termine della transizione, le funzionalità di Windows Server Essentials rimangono nel server e sono supportate per fino a 100 utenti e 200 dispositivi.  
  
## <a name="see-also"></a>Vedere anche  
  

-   [Eseguire la migrazione di dati del Server a Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [Eseguire la migrazione di dati del Server a Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

