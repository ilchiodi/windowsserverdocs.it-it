---
title: Eseguire la transizione da Windows Server Essentials a Windows Server 2012 R2 Standard
description: Viene descritto come usare Windows Server Essentials
ms.custom: na
ms.date: 10/03/2016
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: a14689e3-2310-4229-bd3e-dafc0e739e02
author: nnamuhcs
ms.author: coreyp
manager: dongill
ms.openlocfilehash: 74327b651e76f7a5bbfda2d437b29202f43a90f3
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80318697"
---
# <a name="transition-from-windows-server-essentials-to-windows-server-2012-r2-standard"></a>Eseguire la transizione da Windows Server Essentials a Windows Server 2012 R2 Standard

>Si applica a: Windows Server 2016 Essentials, Windows Server 2012 R2 Essentials, Windows Server 2012 Essentials

Windows Server 2016 è il sistema operativo pronto per il cloud che supporta i carichi di lavoro correnti introducendo nuove tecnologie che semplificano la transizione ai cloud computing. Il contenuto di Windows Server 2016 ti aiuta a prepararti.

 Windows Server Essentials supporta fino a 25 utenti e 50 dispositivi. Quando le esigenze aziendali superano il limite, è possibile eseguire una transizione di licenza sul posto da Windows Server Essentials a Windows Server 2012 R2 Standard per mantenere la conformità della licenza.  
  
 Dopo aver eseguito la transizione a Windows Server 2012 R2 Standard, i limiti relativi agli account utente e ai dispositivi vengono rimossi, ma le funzionalità esclusive di Windows Server Essentials, ad esempio il dashboard, il Accesso Web remoto e il backup dei computer client, rimangono comunque disponibili. Queste funzionalità prevedono tuttavia alcuni limiti tecnici, in quanto supportano un massimo di 100 account utente e 200 dispositivi. La funzionalità di backup del computer client consentirà il backup di un massimo di 75 dispositivi.  
  
> [!IMPORTANT]
>   Windows Server 2012 R2 Standard richiede una licenza CAL (Client Access License) per ogni utente o dispositivo nell'ambiente in uso. Questa operazione è diversa da Windows Server Essentials, che non utilizza il modello CAL e non dispone di licenze CAL. Quando si esegue la transizione da Windows Server Essentials a Windows Server 2012 R2 Standard, sarà necessario acquistare il numero e il tipo di licenze CAL appropriati per l'ambiente in uso (la maggior parte dei clienti acquista le licenze CAL utente).  
  
## <a name="before-the-transition"></a>Prima della transizione  
  
-   Prima di passare da Windows Server Essentials a Windows Server 2012 R2 Standard, è necessario eseguire il backup completo dei dati del server.  
  
    > [!IMPORTANT]
    >  Se non si dispone di un backup completo del server, non sarà possibile ripristinare il server allo stato in cui si trovava prima della transizione.  
  
-   Inoltre, assicurarsi di leggere e comprendere il contratto di licenza con l'utente finale (EULA) per Windows Server 2012 R2 Standard. Per visualizzare il Contratto di licenza con l'utente finale:  
  
    1.  Aprire una finestra di comando come amministratore.  
  
    2.  Eseguire il comando seguente:  
  
         **DISM/online/set-Edition: ServerStandard/geteula:** *EULA percorso* (dove *EULA Path* rappresenta il percorso in cui si vuole salvare il file eula, ad esempio: c:\ ws8std_eula. RTF). Accertarsi di usare l'estensione file rtf.  
  
    3.  Aprire il percorso in cui è stato salvato il file e quindi fare doppio clic su di esso per aprirlo.  
  
## <a name="transition-to--windows-server-2012-r2-standard"></a>Transizione a Windows Server 2012 R2 Standard  
 Dopo aver deciso di eseguire la transizione da Windows Server Essentials a Windows Server 2012 R2 Standard, completare i due passaggi seguenti:  
  
1. Acquistare una licenza per Windows Server 2012 R2 Standard e il numero appropriato di licenze di accesso client per utente e/o dispositivo per l'ambiente in uso.  
  
    È possibile acquistare una licenza per Windows Server 2012 R2 Standard da un punto vendita al dettaglio, da un distributore o con l'aiuto di un [partner Microsoft](https://pinpoint.microsoft.com/SelectCulture.aspx).  
  
   > [!NOTE]
   >  Se inizialmente è stato acquistato Windows Server 2012 R2 Standard ed è stato eseguito il downgrade dei diritti per l'installazione di una delle due istanze virtuali come Windows Server Essentials, non è necessario acquistare altri elementi.  
   >   
   >  Se si acquista Windows Server 2012 R2 Standard tramite il canale contratti multilicenza, è possibile scaricare un'immagine ISO e un codice Product Key per Windows Server 2012 R2 Standard da Volume Licensing Service Center (VLSC).  
   >   
   >  Se si acquista Windows Server 2012 R2 Standard da qualsiasi altro canale, è possibile scaricare un'immagine ISO e un codice Product Key di valutazione per Windows Server Essentials da [TechNet Evaluation Center](https://technet.microsoft.com/evalcenter/jj659306.aspx). Se si esegue la transizione come descritto nel passaggio successivo, il prodotto di valutazione verrà convertito in un prodotto supportato con licenza completa.  
  
2. Aprire Windows PowerShell come amministratore ed eseguire il comando seguente:  
  
    **DISM/online/set-Edition: ServerStandard/AcceptEula/ProductKey:** *Product* Key (dove *Product Key* è il codice Product Key per la copia di Windows Server 2012 R2 Standard).  
  
    Il server viene riavviato per completare il processo di transizione.  
  
   Dopo la transizione, le funzionalità di Windows Server Essentials rimangono nel server e sono supportate per un massimo di 100 utenti e 200 dispositivi.  
  
## <a name="see-also"></a>Vedere anche  
  

-   [Eseguire la migrazione dei dati del server a Windows Server Essentials](Migrate-Server-Data-to-Windows-Server-Essentials.md)

-   [Eseguire la migrazione dei dati del server a Windows Server Essentials](../migrate/Migrate-Server-Data-to-Windows-Server-Essentials.md)

