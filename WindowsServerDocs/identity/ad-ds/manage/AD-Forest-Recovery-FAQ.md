---
title: Ripristino della foresta di Active Directory - Domande frequenti
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.assetid: ac9e5a3d-8b1e-41b7-8e02-f64b7acf1359
ms.technology: identity-adds
ms.openlocfilehash: 36c9560b490cc28f006770c869bdcdf2b152dd74
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59878452"
---
# <a name="ad-forest-recovery---faq"></a>Ripristino della foresta di Active Directory - Domande frequenti

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2, Windows Server 2003

Questo documento contiene le domande frequenti (FAQ) per il ripristino di foreste:  

## <a name="general-recovery"></a>Generali di ripristino

**Q: Cosa può fare per rendere più rapida ripristino?**

Velocità di recupero non è l'obiettivo principale di questa Guida, è possibile ottenere tempi di ripristino per:  
  
- Creazione di un piano di ripristino dettagliate dell'insieme di strutture, aggiornarlo a intervalli regolari e mettendo in pratica, in un ambiente simulato di dimensioni ragionevoli almeno una volta all'anno  
- Tramite la clonazione (DC) di controller di dominio virtualizzati  
   - La clonazione di controller di dominio virtualizzato consente di accelerare il processo per ottenere altri controller di dominio in esecuzione dopo un che controller di dominio viene ripristinato da un backup in ogni dominio. Il controller di dominio virtualizzati aggiuntivi possono essere clonati anziché attendere potenzialmente lunga Active Directory le installazioni di completamento e il completamento della replica della parte non critica dopo l'installazione.  
   - Le foreste in cui sono ospitati i controller di dominio virtuale in un numero relativamente ridotto di centri dati ben collegati potenzialmente trarre enorme vantaggio dalla clonazione durante il ripristino. Tuttavia, qualsiasi ambiente in cui più controller di dominio virtualizzati per lo stesso dominio sono installati nello stesso host dell'hypervisor deve trarre vantaggio.  
- Distribuzione di controller di dominio di sola lettura (RODC)  
   - Questi controller possono fornire continuità aziendale durante il processo di ripristino perché non dovranno essere disconnesso dalla rete così come controller di dominio scrivibili. I RODC non eseguono la replica in uscita. Pertanto, non presentano lo stesso rischio che i controller di dominio scrivibile comportare per la replica dei dati dannosi nell'ambiente ripristinato.  
  
Altri fattori che influenzano la durata del processo di ripristino dell'insieme di strutture includono quanto segue:  
  
- Quando si ripristinano i controller di dominio dai backup, il tempo per:  
   - Individuare i supporti di backup fisici, ad esempio i nastri.  
   - Reinstallare il sistema operativo.  
   - Ripristinare i dati dai supporti di backup.  
      - È possibile ridurre il tempo necessario per reinstallare il sistema operativo e ripristinare i dati da backup eseguendo il ripristino completo del server invece di ripristino dello stato del sistema. Poiché ripristino completo del server è basata su file binario, viene completato più velocemente rispetto al ripristino dello stato del sistema.  
      - Tuttavia, se il server contiene i dati che viene esclusa dai dati dello stato del sistema che non si desidera ripristinare, ripristino completo del server potrebbe non essere una valida alternativa al ripristino dello stato del sistema. Prendere in considerazione i vantaggi dell'esecuzione di un ripristino completo del server invece di un ripristino dello stato del sistema per i server in modo specifico e prepararsi di conseguenza eseguendo il tipo di backup che si prevede di ripristinare in un secondo momento appropriato.  
- Quando si ricompila i controller di dominio, serve tempo per la replica dei dati per le promozioni basati sulla rete.  
   - È possibile ridurre il tempo necessario per il ripristino dei controller di dominio eseguendo i passaggi seguenti:  
- Ridurre il tempo per il recupero dei supporti di backup da:  
   - Utilizzando lo strumento Active Directory Database montaggio (Dsamain.exe) per identificare il backup migliore da usare per operazioni di ripristino. Per altre informazioni sull'utilizzo di strumento di montaggio di Database di Active Directory, vedere la [Active Directory Database montaggio strumento Guida dettagliata](https://go.microsoft.com/fwlink/?LinkId=132577) (https://go.microsoft.com/fwlink/?LinkId=132577).  
   - L'assegnazione di etichette in modo chiaro i supporti di backup e archiviare il supporto in modo organizzato in una comoda ancora sicuro, posizione che consente il recupero rapido.  
   - Usando il servizio Copia Shadow del Volume con una rete di archiviazione (SAN) per conservare i backup da diversi punti nel tempo. Per altre informazioni, vedere [Windows Server 2003 Active Directory Fast Recovery con il servizio Copia Shadow del Volume e il servizio dischi virtuali](https://go.microsoft.com/fwlink/?LinkId=70781) (https://go.microsoft.com/fwlink/?LinkId=70781).  
- Forzare la rimozione di dominio Active Directory dal controller di dominio anziché reinstallando il sistema operativo. Se è stata identificata la causa dell'errore a livello di foresta per essere esclusivamente all'interno dell'ambito di dominio Active Directory, non è necessario reinstallare il sistema operativo nei controller di dominio.  
   - Per altre informazioni su come forzare la rimozione di dominio Active Directory da un controller di dominio che esegue Windows Server 2008 o versione successiva, vedere [rimozione forzata di un Controller di dominio di Windows Server 2008](https://go.microsoft.com/fwlink/?LinkId=132627) (https://go.microsoft.com/fwlink/?LinkId=132627). Per altre informazioni su come forzare la rimozione di dominio Active Directory da un controller di dominio che esegue Windows Server 2003, vedere [articolo 332199](https://go.microsoft.com/fwlink/?LinkId=70780) della Microsoft Knowledge Base (https://go.microsoft.com/fwlink/?LinkId=70780).  
- Usare i dispositivi a nastro più veloce o i backup su disco per ridurre il tempo necessario per le operazioni di ripristino.  
  
È possibile inoltre aiutare ad accelerare le installazioni di Active Directory Domain Services tramite l'installazione da funzionalità multimediali (IFM) per ricompilare i controller di dominio in ogni dominio. Installazione da supporto, riduce la latenza di replica che si verifica quando si ricompila i controller di dominio in ogni dominio.  
  
Le aziende che hanno un contratto a livello di servizio più aggressivo (SLA) potrebbero provare a modificare le procedure di ripristino dell'insieme di strutture per velocizzare il recupero.  
  
**Q: È possibile automatizzare il processo di ripristino di foreste?**

A causa della natura complessa e critica del processo di ripristino dell'insieme di strutture, non è attualmente non prevede automazione end-to-end di esso. Il processo di ripristino della foresta è più un problema logistico e aziendale di continuità aziendale rispetto a un problema tecnico di automazione dei processi di ripristino. Pertanto, la persona che amministra l'ambiente deve creare un piano di ripristino dell'insieme di strutture che è specifico di tale ambiente e quindi automatizzare alcune sezioni specifiche che è possibile automatizzare correttamente.  
  
È possibile eseguire la maggior parte dei passaggi di ripristino dell'insieme di strutture con gli strumenti da riga di comando. Pertanto, la maggior parte dei passaggi sono configurabili tramite script. Ad esempio, Ntdsutil.exe è uno degli strumenti usati più di frequente nel processo di ripristino dell'insieme di strutture.  
  
Anche se gli script possono velocizzare il ripristino, è necessario testare accuratamente questi script prima di applicarle in un ambiente reale. Inoltre, è necessario aggiornarli in base alle modifiche nell'ambiente Active Directory, ad esempio l'aggiunta di un nuovo dominio o controller di dominio o una nuova versione di Active Directory.

## <a name="next-steps"></a>Passaggi successivi

- [Ripristino della foresta Active Directory - prerequisiti](AD-Forest-Recovery-Prerequisties.md)  
- [Ripristino della foresta Active Directory - concepire un piano di ripristino personalizzata-foresta](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Ripristino della foresta Active Directory - identificare il problema](AD-Forest-Recovery-Identify-the-Problem.md)
- [Ripristino della foresta Active Directory - determinare la modalità di ripristino](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [Ripristino della foresta Active Directory - eseguire il ripristino iniziale](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [Ripristino della foresta Active Directory - procedure](AD-Forest-Recovery-Procedures.md)  
- [Ripristino della foresta Active Directory - domande frequenti](AD-Forest-Recovery-FAQ.md)  
- [Ripristino della foresta Active Directory - il ripristino di un singolo dominio all'interno di un Multidomain foresta](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [Ripristino della foresta Active Directory - ripristino della foresta con controller di dominio di Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)  
