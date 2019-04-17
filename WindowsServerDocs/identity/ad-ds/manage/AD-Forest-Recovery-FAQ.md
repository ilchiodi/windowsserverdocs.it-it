---
title: Ripristino della foresta Active Directory - domande frequenti
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/07/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: ac9e5a3d-8b1e-41b7-8e02-f64b7acf1359
ms.technology: identity-adfs
ms.openlocfilehash: 839e01c88b7ced22501154d8752c6c71cc52b696
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="ad-forest-recovery---faq"></a>Ripristino della foresta Active Directory - domande frequenti

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2, Windows Server 2003

Questo documento contiene frequenti domande frequenti relative al ripristino della foresta:  
 
## <a name="general-recovery"></a>Ripristino generali
  
 
**D: che cosa posso fare per velocizzare il recupero?** 
 
Velocità di recupero non è l'obiettivo principale di questa Guida, è possibile ottenere tempi di ripristino più brevi per:  
  
-   Creazione di un piano di ripristino della foresta dettagliate, eseguirne l'aggiornamento a intervalli regolari ed esercitazione in un ambiente simulato di dimensioni ragionevoli almeno una volta all'anno  
  
-   Con la clonazione di controller (DC) di dominio virtualizzati  
  
     La clonazione del controller di dominio virtualizzato consente di accelerare il processo per ottenere ulteriori controller di dominio in esecuzione dopo un che controller di dominio viene ripristinato da un backup in ogni dominio. Il controller di dominio virtualizzati aggiuntivi possono essere clonati anziché attendere che le installazioni di completare potenzialmente lunga Active Directory e per il completamento della replica non critici dopo l'installazione.  
  
     Gli insiemi di strutture in controller di dominio virtuali ospitati in un numero relativamente ridotto di centri dati ben collegati potenzialmente traggono dalla clonazione durante il ripristino. Tuttavia, qualsiasi ambiente in cui più controller di dominio virtualizzati per lo stesso dominio sono installati nello stesso host hypervisor deve trarre vantaggio.  
  
-   Distribuzione di controller di dominio di sola lettura (RODC)  
  
     Lettura possibile assicurare la continuità aziendale durante il processo di ripristino perché non hanno deve essere disconnessa dalla rete come controller di dominio scrivibile. Lettura non effettuare la replica in uscita. Pertanto, non presentano agli stessi rischi posti dal controller di dominio scrivibile per la replica dei dati dannosi nell'ambiente ripristinato.  
  
 Altri fattori che influenzano la durata del processo di ripristino della foresta, tra cui:  
  
-   Quando si ripristinano i controller di dominio dal backup, il tempo per:  
  
    -   Individuare il supporto di backup fisico, ad esempio i nastri.  
  
    -   Reinstallare il sistema operativo.  
  
    -   Ripristinare i dati dal supporto di backup.  
  
     È possibile ridurre il tempo necessario per reinstallare il sistema operativo e ripristinare i dati dal backup eseguendo il ripristino completo del server anziché il ripristino dello stato di sistema. Poiché ripristino completo del server è basata su file binario, completa molto più veloce di ripristino dello stato del sistema.  
  
     Tuttavia, se il server contiene dati che sono escluso dai dati sullo stato di sistema che non si desidera ripristinare, ripristino completo del server potrebbe non essere una valida alternativa per il ripristino dello stato di sistema. Offre i vantaggi di eseguire un ripristino completo del server anziché un ripristino dello stato di sistema per i server in modo specifico e preparare conseguenza eseguendo il tipo di backup che si intende ripristinare in un secondo momento appropriato.  
  
-   Quando si ricrea i controller di dominio, il tempo di replicare i dati per le promozioni basate sulla rete.  
  
 È possibile ridurre il tempo necessario per il ripristino dei controller di dominio eseguendo i passaggi seguenti:  
  
-   Ridurre il tempo per il recupero dei supporti di backup da:  
  
    -   Utilizzando il (Dsamain.exe) lo strumento di montaggio Database di Active Directory per identificare il backup migliore da utilizzare per le operazioni di ripristino. Per ulteriori informazioni sull'utilizzo di utilità di montaggio di Database di Active Directory, vedere il [Active Directory Database montaggio strumento Step-by-Step Guida](https://go.microsoft.com/fwlink/?LinkId=132577) (https://go.microsoft.com/fwlink/?LinkId=132577).  
  
    -   Assegnazione di etichette chiaramente il supporto di backup e archiviare il supporto in modo organizzato in modo pratico ancora proteggere, percorso che consente il recupero rapido.  
  
    -   Utilizzando il servizio Copia Shadow del Volume a una rete di archiviazione (SAN) per gestire i backup da diverse posizioni nel tempo. Per ulteriori informazioni, vedere [Windows Server 2003 Active Directory rapido ripristino con il servizio Copia Shadow del Volume e servizio dischi virtuali](https://go.microsoft.com/fwlink/?LinkId=70781) (https://go.microsoft.com/fwlink/?LinkId=70781).  
  
-   Forzare la rimozione di dominio Active Directory dal controller di dominio anziché reinstallando il sistema operativo. Se è stata identificata la causa dell'errore a livello di foresta per essere esclusivamente all'interno dell'ambito di dominio Active Directory, non è necessario reinstallare il sistema operativo nel controller di dominio.  
  
     Per ulteriori informazioni sulla rimozione forzata di servizi di dominio Active Directory da un controller di dominio che esegue Windows Server 2008 o versione successiva, vedere [forzare la rimozione di un Controller di dominio di Windows Server 2008](https://go.microsoft.com/fwlink/?LinkId=132627) (https://go.microsoft.com/fwlink/?LinkId=132627). Per ulteriori informazioni sulla rimozione forzata di servizi di dominio Active Directory da un controller di dominio che esegue Windows Server 2003, vedere [articolo 332199](https://go.microsoft.com/fwlink/?LinkId=70780) della Microsoft Knowledge Base (https://go.microsoft.com/fwlink/?LinkId=70780).  
  
-   Utilizzare dispositivi a nastro più veloci o i backup del disco per ridurre il tempo richiesto per operazioni di ripristino.  
  
 Puoi anche aiutare accelerare le installazioni di dominio Active Directory utilizzando l'installazione di funzionalità di supporto (IFM) per ricostruire i controller di dominio in ogni dominio. IFM riduce la latenza di replica che si verifica quando si ricrea i controller di dominio in ogni dominio.  
  
 Le aziende che dispongono di un contratto di livello di servizio (SLA) più aggressivo valuta alterare le procedure di ripristino foresta per velocizzare il recupero.  
  

**D: posso automatizzare il processo di ripristino della foresta?**  
 A causa della natura complessa e importanti del processo di ripristino della foresta, non è attualmente non prevede automazione end-to-end di esso. Il processo di ripristino della foresta è più una sfida logistica e organizzativa di continuità aziendale rispetto a un problema tecnico di automazione del processo di ripristino. Di conseguenza, la persona che gestisce l'ambiente deve creare un piano di recupero dell'insieme di strutture è specifico di tale ambiente e quindi automatizzare delle sezioni che è possono automatizzare correttamente.  
  
 È possibile eseguire la maggior parte delle operazioni di ripristino foresta utilizzando gli strumenti da riga di comando. Pertanto, la maggior parte dei passaggi sono configurabili tramite script. Ad esempio, Ntdsutil.exe è uno degli strumenti usati più di frequente nel processo di ripristino della foresta.  
  
 Anche se gli script di velocizzare il ripristino, è necessario testare attentamente questi script prima di applicarle in un ambiente reale. Inoltre, è necessario aggiornarli in base alle modifiche nell'ambiente di Active Directory, ad esempio l'aggiunta di un nuovo dominio o controller di dominio o una nuova versione di Active Directory.

## <a name="next-steps"></a>Passaggi successivi
-   [Ripristino della foresta Active Directory - prerequisiti](AD-Forest-Recovery-Prerequisties.md)  
-   [Ripristino della foresta Active Directory - sviluppo di un piano di ripristino personalizzato foresta](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Ripristino della foresta Active Directory - identificare il problema](AD-Forest-Recovery-Identify-the-Problem.md)
-   [Ripristino della foresta Active Directory - determinare la modalità di ripristino](AD-Forest-Recovery-Determine-how-to-Recover.md)
-   [Ripristino della foresta Active Directory - eseguire il ripristino iniziale](AD-Forest-Recovery-Perform-initial-recovery.md)  
-   [Ripristino della foresta Active Directory - procedure](AD-Forest-Recovery-Procedures.md)  
-   [Ripristino della foresta Active Directory - domande frequenti](AD-Forest-Recovery-FAQ.md)  
-   [Ripristino della foresta Active Directory - ripristino di un singolo dominio all'interno di un insieme di strutture Multidomain](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
-   [Ripristino della foresta Active Directory - ripristino della foresta con i controller di dominio di Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)  
