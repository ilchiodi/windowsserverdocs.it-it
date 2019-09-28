---
title: Ripristino della foresta di Active Directory - Domande frequenti
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/09/2018
ms.topic: article
ms.prod: windows-server
ms.assetid: ac9e5a3d-8b1e-41b7-8e02-f64b7acf1359
ms.technology: identity-adds
ms.openlocfilehash: 49cd12621c6ddf89393f0463e4856555ca241491
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71369111"
---
# <a name="ad-forest-recovery---faq"></a>Ripristino della foresta di Active Directory - Domande frequenti

>Si applica a: Windows Server 2016, Windows Server 2012 e 2012 R2, Windows Server 2008 e 2008 R2, Windows Server 2003

Questo documento contiene le domande frequenti relative al ripristino della foresta:  

## <a name="general-recovery"></a>Ripristino generale

**D Cosa è possibile fare per velocizzare il ripristino?**

Sebbene la velocità di recupero non sia l'obiettivo principale di questa guida, è possibile ottenere tempi di ripristino più brevi:  
  
- Creazione di un piano di ripristino dettagliato della foresta, aggiornamento a intervalli regolari e relativa pratica in un ambiente di test simulato di dimensioni ragionevoli almeno una volta all'anno  
- Uso della clonazione del controller di dominio virtualizzato (DC)  
   - La clonazione del controller di dominio virtualizzato velocizza il processo di recupero di controller di dominio aggiuntivi in esecuzione dopo il ripristino di un controller di dominio da un backup in ogni dominio. È possibile clonare i controller di dominio virtualizzati aggiuntivi anziché attendere il completamento di installazioni di servizi di dominio Active Directory potenzialmente lunghe e il completamento della replica non critica dopo l'installazione.  
   - Le foreste in cui i controller di dominio virtuali sono ospitati in un numero relativamente ridotto di centri dati ben connessi potenzialmente traggono vantaggio dalla clonazione durante il ripristino. Tuttavia, qualsiasi ambiente in cui sono presenti più controller di dominio virtualizzati per lo stesso dominio nello stesso host hypervisor dovrebbe trarre vantaggio.  
- Distribuzione di controller di dominio di sola lettura (RODC)  
   - RODC può garantire la continuità aziendale durante il processo di ripristino perché non è necessario disconnettersi dalla rete come controller di dominio scrivibile. RODC non eseguono la replica in uscita. Di conseguenza, non presentano lo stesso rischio che i controller di dominio scrivibili creino per replicare i dati dannosi nell'ambiente recuperato.  
  
Gli altri fattori che influiscono sulla durata del processo di ripristino della foresta includono i seguenti:  
  
- Quando si ripristinano i controller di dominio dai backup, è necessario tempo per:  
   - Individuare i supporti di backup fisici, ad esempio i nastri.  
   - Reinstallare il sistema operativo.  
   - Ripristinare i dati dai supporti di backup.  
      - È possibile ridurre il tempo necessario per reinstallare il sistema operativo e ripristinare i dati dal backup eseguendo il ripristino completo del server anziché il ripristino dello stato del sistema. Poiché il ripristino completo del server è basato su binario, viene completato molto più velocemente rispetto al ripristino dello stato del sistema.  
      - Tuttavia, se il server contiene dati esclusi dai dati dello stato del sistema che non si desidera ripristinare, il ripristino completo del server potrebbe non essere un'alternativa valida per il ripristino dello stato del sistema. Considerare i vantaggi derivanti dall'esecuzione di un ripristino completo del server anziché da un ripristino dello stato del sistema per i server in modo specifico e prepararsi di conseguenza eseguendo il tipo appropriato di backup che si prevede di ripristinare in un secondo momento.  
- Quando si ricompilano i controller di dominio, la replica dei dati per le promozioni basate sulla rete richiede tempo.  
   - È possibile ridurre il tempo necessario per il ripristino dei controller di dominio attenendosi alla procedura seguente:  
- Ridurre il tempo necessario per il recupero dei supporti di backup:  
   - Utilizzo dello strumento di montaggio del database Active Directory (dsamain. exe) per identificare il backup migliore da utilizzare per le operazioni di ripristino. Per ulteriori informazioni sull'utilizzo dello strumento di montaggio Active Directory database, vedere la [Guida dettagliata allo strumento di montaggio dei database Active Directory](https://go.microsoft.com/fwlink/?LinkId=132577) (https://go.microsoft.com/fwlink/?LinkId=132577).  
   - Etichettare i supporti di backup in modo chiaro e archiviare i supporti in modo organizzato in una posizione pratica, ma sicura, che consente un rapido recupero.  
   - Utilizzando la Servizio Copia Shadow del volume con una rete di archiviazione (SAN) per mantenere i backup da diversi punti nel tempo. Per ulteriori informazioni, vedere [Windows Server 2003 Active Directory ripristino rapido con servizio Copia Shadow del volume e servizio dischi virtuali](https://go.microsoft.com/fwlink/?LinkId=70781) (https://go.microsoft.com/fwlink/?LinkId=70781).  
- Forzare la rimozione di servizi di dominio Active Directory dal controller di dominio anziché reinstallare il sistema operativo. Se la cause dell'errore a livello di foresta è stata identificata come esclusivamente nell'ambito di servizi di dominio Active Directory, non è necessario reinstallare il sistema operativo nei controller di dominio.  
   - Per ulteriori informazioni su come forzare la rimozione di servizi di dominio Active Directory da un controller di dominio che esegue Windows Server 2008 o versione successiva, vedere [forzando la rimozione di un controller di dominio Windows Server 2008](https://go.microsoft.com/fwlink/?LinkId=132627) (https://go.microsoft.com/fwlink/?LinkId=132627). Per ulteriori informazioni su come forzare la rimozione di servizi di dominio Active Directory da un controller di dominio che esegue Windows Server 2003, vedere l' [articolo 332199](https://go.microsoft.com/fwlink/?LinkId=70780) della Microsoft Knowledge Base (https://go.microsoft.com/fwlink/?LinkId=70780).  
- Usare i dispositivi nastro più veloci o i backup su disco per ridurre il tempo necessario per le operazioni di ripristino.  
  
È inoltre possibile velocizzare le installazioni di servizi di dominio Active Directory utilizzando la funzionalità installa da supporto (installazione da supporto) per ricompilare i controller di dominio in ogni dominio. INSTALLAZIONE da supporto riduce la latenza di replica che si verifica quando si ricompilano i controller di dominio in ogni dominio.  
  
Le aziende che hanno un contratto di servizio (SLA) più aggressivo potrebbero considerare la modifica delle procedure di ripristino della foresta per velocizzare il ripristino.  
  
**D È possibile automatizzare il processo di ripristino della foresta?**

A causa della complessità e della natura critica del processo di ripristino della foresta, attualmente non esiste alcuna automazione end-to-end. Il processo di ripristino della foresta è più una sfida logistica e organizzativa del ripristino della continuità aziendale rispetto a un problema tecnico dell'automazione dei processi. Pertanto, l'utente che amministra l'ambiente deve creare un piano di ripristino della foresta specifico di tale ambiente e quindi automatizzare le sezioni che possono essere automatizzate correttamente.  
  
È possibile eseguire la maggior parte dei passaggi di ripristino della foresta tramite gli strumenti da riga di comando. Pertanto, la maggior parte dei passaggi è configurabile tramite script. Ad esempio, Ntdsutil. exe è uno degli strumenti usati più di frequente nel processo di ripristino della foresta.  
  
Sebbene gli script possano velocizzare il ripristino, è necessario testare accuratamente questi script prima di applicarli in un ambiente reale. Inoltre, è necessario aggiornarli in base alle modifiche apportate all'ambiente Active Directory, ad esempio l'aggiunta di un nuovo dominio o controller di dominio o una nuova versione di Active Directory.

## <a name="next-steps"></a>Passaggi successivi

- [Ripristino della foresta di Active Directory - Prerequisiti](AD-Forest-Recovery-Prerequisties.md)  
- [Ripristino della foresta di Active Directory-definizione di un piano di ripristino della foresta personalizzato](AD-Forest-Recovery-Devising-a-Plan.md)  
- [Ripristino della foresta di Active Directory-identificare il problema](AD-Forest-Recovery-Identify-the-Problem.md)
- [Ripristino della foresta di Active Directory-determinare la modalità di ripristino](AD-Forest-Recovery-Determine-how-to-Recover.md)
- [Ripristino della foresta di Active Directory-esecuzione del ripristino iniziale](AD-Forest-Recovery-Perform-initial-recovery.md)  
- [Ripristino della foresta di Active Directory - Procedure](AD-Forest-Recovery-Procedures.md)  
- [Ripristino della foresta di Active Directory-Domande frequenti](AD-Forest-Recovery-FAQ.md)  
- [Ripristino della foresta di Active Directory-ripristino di un singolo dominio in una foresta con più domini](AD-Forest-Recovery-Single-Domain-in-Multidomain-Recovery.md)  
- [Ripristino della foresta di Active Directory-ripristino della foresta con i controller di dominio Windows Server 2003](AD-Forest-Recovery-Windows-Server-2003.md)  
