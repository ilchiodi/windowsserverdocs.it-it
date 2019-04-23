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
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840082"
---
# <a name="transition-from-windows-server-essentials-to-windows-server-2012-r2-standard"></a>Eseguire la transizione da Windows Server Essentials a Windows Server 2012 R2 Standard

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Windows Server 2016 è il sistema operativo compatibile con il cloud che supporta carichi di lavoro correnti durante l'introduzione di nuove tecnologie che semplificano la transizione al cloud computing. Il contenuto di Windows Server 2016 consente di essere pronti.

 Windows Server Essentials supporta fino a 25 utenti e 50 dispositivi. Quando l'azienda deve superare il limite, è possibile eseguire una transizione di licenza sul posto da Windows Server Essentials a Windows Server 2012 R2 Standard per mantenere la conformità delle licenze.  
  
 Dopo la transizione a Windows Server 2012 R2 Standard, i limiti di account e i dispositivi utente vengono rimossi, ma le funzionalità esclusive di Windows Server Essentials (ad esempio il Dashboard, accesso Web remoto e backup dei computer client) rimangono comunque disponibili. Queste funzionalità prevedono tuttavia alcuni limiti tecnici, in quanto supportano un massimo di 100 account utente e 200 dispositivi. La funzionalità di backup del computer client consentirà il backup di un massimo di 75 dispositivi.  
  
> [!IMPORTANT]
>   Windows Server 2012 R2 Standard richiede una licenza di accesso client (CAL) per ogni utente o dispositivo nell'ambiente in uso. Ciò è diverso da Windows Server Essentials, che non usa il modello CAL e che non include alcuna licenza CAL. Durante la transizione da Windows Server Essentials a Windows Server 2012 R2 Standard, dovrai acquistare il numero appropriato e il tipo di licenze CAL per l'ambiente (la maggior parte dei clienti Acquista licenze CAL utente).  
  
## <a name="before-the-transition"></a>Prima della transizione  
  
-   Prima di una transizione da Windows Server Essentials a Windows Server 2012 R2 Standard, è necessario eseguire un backup completo dei dati del server.  
  
    > [!IMPORTANT]
    >  Se non si dispone di un backup completo del server, non sarà possibile ripristinare il server allo stato in cui si trovava prima della transizione.  
  
-   Inoltre, assicurarsi di leggere e accettare il contratto di licenza con l'utente finale (EULA) per Windows Server 2012 R2 Standard. Per visualizzare il Contratto di licenza con l'utente finale:  
  
    1.  Aprire una finestra di comando come amministratore.  
  
    2.  Eseguire il comando seguente:  
  
         **DISM /online -/Set-Edition: ServerStandard /geteula:** *percorso condizioni di licenza* (dove *percorso eula* rappresenta la posizione in cui si desidera salvare il file EULA, ad esempio: C:\ws8std_eula.rtf). Accertarsi di usare l'estensione file rtf.  
  
    3.  Aprire il percorso in cui è stato salvato il file e quindi fare doppio clic su di esso per aprirlo.  
  
## <a name="transition-to--windows-server-2012-r2-standard"></a>Transizione a Windows Server 2012 R2 Standard  
 Dopo aver deciso di eseguire la transizione da Windows Server Essentials a Windows Server 2012 R2 Standard, completo di questi due passaggi:  
  
1.  Acquistare una licenza per Windows Server 2012 R2 Standard e il numero appropriato di licenze di accesso client dispositivo per l'ambiente e/o utente.  
  
     È possibile acquistare una licenza per Windows Server 2012 R2 Standard presso un negozio al dettaglio, un server di distribuzione o con l'aiuto di un [Partner Microsoft](https://pinpoint.microsoft.com/SelectCulture.aspx).  
  
    > [!NOTE]
    >  Se inizialmente acquistato Windows Server 2012 R2 Standard ed esercitato il diritto di downgrade per installare una delle due istanze virtuali disponibili come Windows Server Essentials, non occorre acquistare altre.  
    >   
    >  Se si acquista Windows Server 2012 R2 Standard tramite il canale contratti multilicenza, è possibile scaricare un'immagine ISO e un codice product key per Windows Server 2012 R2 Standard da Volume Licensing Service Center (VLSC).  
    >   
    >  Se si acquista Windows Server 2012 R2 Standard da un altro canale, è possibile scaricare un'immagine ISO e un codice product key di valutazione per Windows Server Essentials dal [TechNet Evaluation Center](https://technet.microsoft.com/evalcenter/jj659306.aspx). Se si esegue la transizione come descritto nel passaggio successivo, il prodotto di valutazione verrà convertito in un prodotto supportato con licenza completa.  
  
2.  Aprire Windows PowerShell come amministratore ed eseguire il comando seguente:  
  
     **dism /online /set-edition:ServerStandard /accepteula /productkey:** *Codice Product Key* (dove *"Product Key"* è il codice product key per la copia di Windows Server 2012 R2 Standard).  
  
     Il server viene riavviato per completare il processo di transizione.  
  
 Al termine della transizione, le funzionalità di Windows Server Essentials rimangono nel server e sono supportate per un massimo di 100 utenti e 200 dispositivi.  
  
## <a name="see-also"></a>Vedere anche  
  

-   [Eseguire la migrazione di dati del Server a Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [Eseguire la migrazione di dati del Server a Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

