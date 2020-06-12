---
title: Informazioni (assenza di) blocco di file distribuiti in DFSR
description: In questo articolo viene illustrata l'assenza di un meccanismo di blocco di file distribuiti con più host all'interno di Windows e, in particolare, nelle cartelle replicate da DFSR.
ms.prod: windows-server
ms.technology: server-general
ms.date: 06/10/2020
author: Deland-Han
ms.author: delhan
ms.openlocfilehash: 739bcd1127877d4c9b1339b64270262d9d48634f
ms.sourcegitcommit: fa9a8badf4eb366aeeca7d2905e2cad711ee8dae
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 06/11/2020
ms.locfileid: "84714975"
---
# <a name="understanding-the-lack-of-distributed-file-locking-in-dfsr"></a>Informazioni (assenza di) blocco di file distribuiti in DFSR

In questo articolo viene illustrata l'assenza di un meccanismo di blocco di file distribuiti con più host all'interno di Windows e, in particolare, nelle cartelle replicate da DFSR.
  
## <a name="some-background"></a>Sfondo

  - Blocco di file distribuiti: si riferisce al concetto di avere più copie di un file in più computer e quando un file viene aperto per la scrittura, tutte le altre copie sono bloccate. In questo modo si impedisce la modifica di un file in più server contemporaneamente da parte di più utenti.
  - File system distribuito replica: [DFSR](/previous-versions/windows/desktop/dfsr/distributed-file-system-replication--dfsr-) opera in una progettazione multimaster basata sullo stato. Nella replica basata sullo stato, ogni server del sistema multimaster applica gli aggiornamenti alla propria replica all'arrivo, senza scambiare i file di log. usa invece i vettori di versione per mantenere le informazioni di "aggiornamento". Nessun server è mai arbitrariamente autorevole dopo la sincronizzazione iniziale, quindi è altamente disponibile e molto flessibile in varie topologie di rete.
  - Server Message Block- [SMB](/openspecs/windows_protocols/ms-smb/f210069c-7086-4dc2-885e-861d837df688) è il protocollo comune usato in Windows per accedere ai file in rete. In termini semplificati, si tratta di un protocollo client-server che fa uso di un redirector per avere file system remoti come file system locali. Non è specifico di Windows ed è piuttosto comune: un esempio non Microsoft noto è Samba, che consente a Linux, Mac e ad altri sistemi operativi di agire come client/server SMB e partecipare alle reti Windows.

  
È importante eseguire una delineatura chiara in cui DFSR e SMB risiedono nell'ambiente dei dati replicati. SMB consente agli utenti di accedere ai file e non ha alcuna consapevolezza di DFSR. Analogamente, DFSR (che usa il protocollo RPC) mantiene sincronizzati i file tra i server e non ha alcuna consapevolezza di SMB. Non confondere il blocco distribuito come definito in questo post e il [blocco opportunistico](/windows/win32/fileio/opportunistic-locks).  
  
Quindi, in questo caso, le cose possono andare a forma di pera, come dicono gli inglesi.  
  
Poiché gli utenti possono modificare i dati su più server e poiché ogni Windows Server è in grado di riconoscere un blocco di file in sé *e* poiché DFSR non è in grado [di riconoscere i blocchi in altri server](/previous-versions/windows/it-pro/windows-server-2003/cc773238(v=ws.10)), è possibile che gli utenti sovrascrivano le modifiche apportate. DFSR usa un algoritmo di conflitto "ultimo writer wins", quindi un utente deve perdere e la persona da salvare per ultime modifiche per mantenere le modifiche. La copia del file persa viene trascinata nella cartella *cartella ConflictAndDeleted* .  
  
A questo punto, si tratta di un'operazione molto *meno* comune rispetto alle persone che vogliono credere. In genere, i file condivisi true vengono modificati in un ambiente locale. nella succursale o nella stessa riga di cubicoli. In genere vengono utilizzati da persone nello stesso team, quindi le persone sono in genere consapevoli dei colleghi che modificano i dati. Poiché in genere si trovano nello stesso sito, le probabilità sono molto più elevate che tutti gli utenti che lavorano su un documento condiviso utilizzeranno lo stesso server. Windows SMB gestisce la situazione qui. Quando un utente dispone di un file bloccato per la modifica e il suo collaboratore tenta di modificarlo, l'altro utente riceverà un errore simile al seguente:  
  
![File in uso](./media/understanding-the-lack-of-distributed-file-locking-in-dfsr/1.jpg)
  
Se l'applicazione che apre il file è davvero intelligente, ad esempio Word 2007, potrebbe dare:  
  
![File in uso](./media/understanding-the-lack-of-distributed-file-locking-in-dfsr/2.jpg) 
  
DFSR dispone di un meccanismo per i file bloccati, ma solo all'interno del contesto del server. DFSR non eseguirà la replica di un file in o in uscita se la relativa copia locale ha un blocco esclusivo. Questa operazione non impedisce tuttavia a tutti gli utenti di modificare il file in un altro server.  
  
Tornando all'argomento, il problema dei dati condivisi modificati geograficamente esiste *e* per alcuni utenti è piuttosto difficile. A volte viene chiesto perché DFSR non gestisce questo blocco e prende tutto con un'onda della bacchetta magica. Si tratta di uno scenario interessante e complesso da risolvere per un sistema di replica multimaster. Per iniziare a esplorare questo tipo,  
  
## <a name="third-party-solutions"></a>Soluzioni di terze parti
  
Sono disponibili alcune soluzioni di fornitori che interessano questo problema, che in genere affrontano uno o più dei metodi seguenti \* :  

  - Uso di un meccanismo broker

> La presenza di un "traffico COP" centrale consente a un server di riconoscere tutti gli altri server e quali file sono stati bloccati dagli utenti. Sfortunatamente questo significa anche che è spesso presente un singolo punto di guasto nel sistema di blocco distribuito.

![Topologia](./media/understanding-the-lack-of-distributed-file-locking-in-dfsr/3.png) 

  - Requisito per una rete completamente indirizzata

> Poiché un broker centrale deve essere in grado di comunicare con tutti i server che partecipano alla replica di file, in questo modo viene eliminata la possibilità di gestire topologie di rete complesse. Le topologie di anelli e le topologie multihub e spoke non sono in genere possibili. In una rete non completamente instradata, alcuni server potrebbero non essere in grado di contattare direttamente l'altro o un broker e possono comunicare con un partner che può comunicare con un altro server e così via. Si tratta di un ambiente con più master, ma non con un meccanismo di broker.

![Topologia](./media/understanding-the-lack-of-distributed-file-locking-in-dfsr/4.png)

  - Sono limitati a una coppia di server

> Alcune soluzioni limitano la topologia a una coppia di server per semplificare il meccanismo di blocco distribuito. Per ambienti più grandi, questa operazione potrebbe non essere fattibile.

  - Usare gli agenti su client e server
  - Non usare la replica multimaster
  - Non usare il clustering di Microsoft
  - Usare Appliance specializzate

  
***\*** Si noti che in genere \! non si prevedono minacce alla morte perché si dispone di una soluzione che non implementa uno o più di questi metodi\!*  
  
## <a name="deeper-thoughts"></a>Considerazioni più approfondite  
  
Quando si pensa a questo problema, si inizia a ritagliare alcuni problemi fondamentali. Se ad esempio sono presenti quattro server con dati che possono essere modificati dagli utenti in quattro siti e la connessione WAN a uno di essi passa alla modalità offline, cosa facciamo? Gli utenti possono comunque accedere ai singoli server, ma è opportuno lasciarli invariati? Non si vuole che vengano apportate modifiche in conflitto, ma si vuole che continuino a lavorare e a rendere il denaro della società. Se si bloccano arbitrariamente le modifiche in quel momento, nessun utente può funzionare anche se non si verificano effettivamente conflitti\! Non esiste alcun modo per comunicare agli altri server che il file è in uso e che si torna al quadrato.  
  
![Topologia](./media/understanding-the-lack-of-distributed-file-locking-in-dfsr/5.png)
  
Quindi, si verificano SMB e la gestione degli errori dei blocchi di Reporting. Non è possibile modificare il modo in cui i report SMB condividono le violazioni, in quanto si interrompe una tonnellata di applicazioni e i client non capirebbero comunque i nuovi messaggi di errore estesi. Le applicazioni come Word 2007 eseguono alcune operazioni di tipo "Undercover" per capire chi è il blocco dei file, ma la maggior parte delle applicazioni non sa chi ha un file in uso (o anche che SMB esiste. In realtà. Quindi, quando un utente riceve il messaggio ' questo file è in uso ', non è particolarmente praticabile, perché tutti chiamano il help desk? Il help desk ha accesso a tutti i file server per visualizzare gli utenti che accedono ai file? Complessa.  
  
Poiché si desidera la disponibilità elevata di più master, un sistema Broker è meno auspicabile; potrebbe essere necessario disporre di un elemento in esecuzione in tutti i server che consenta loro di comunicare tutti anche tramite reti non completamente indirizzate. Questa operazione richiede tecniche di sincronizzazione molto complesse. Si aggiungerà un sovraccarico sulla rete (anche se probabilmente non molto) e sarà necessario essere fulmineo per assicurarsi che l'utente non sia tenuto a lavorare. è necessario che la replica di file sia più veloce, in realtà potrebbe essere necessario associare la replica in qualche modo. Sarà inoltre necessario tenere conto di interruzioni del server connesse alla rete e non arresti anomali del server, in qualche modo.  

![Topologia](./media/understanding-the-lack-of-distributed-file-locking-in-dfsr/6.png)
  
Quindi, torniamo al software client speciale per questo scenario in grado di comprendere meglio i blocchi e di fornire all'utente alcune informazioni utili ("go Call Susie in Accounting e informarlo per rilasciarlo", "Ci dispiace, la topologia di blocco dei file è stata interstata e l'amministratore impedisce l'apertura del file fino a quando non viene risolto" e così via). La possibilità di eseguire questa operazione con i milioni di applicazioni in esecuzione in Windows sarà sicuramente interessante. Ci sono molti sistemi operativi che non sono supportati o non sono in grado di ottenere il software: Windows 2000 è fuori dal supporto mainstream e XP sarà presto disponibile. I client Linux e Mac non hanno questo software fino a quando non ritenevano che fosse importante, quindi il cliente avrebbe dovuto sperare che i loro fornitori facessero qualcosa di simile.  
  
## <a name="more-inforamtion"></a>Altre Information
  
Attualmente, il modo più semplice per controllare questa situazione in DFSR consiste nell'usare spazi dei nomi DFS per guidare gli utenti a posizioni prevedibili, con uno spazio dei nomi coerente. Configurando correttamente i collegamenti al server e alla topologia del sito di DFSN, gli utenti possono condividere lo stesso server locale e consentire loro di accedere solo ai computer remoti quando il server "Main" è inattivo. Per la maggior parte degli ambienti, questo funziona correttamente. Alternativa a DFSR, SharePoint è un'opzione a causa del sistema di estrazione/archiviazione. BranchCache (disponibile in Windows Server 2008 R2 e Windows 7) può essere un'opzione che è progettata per facilitare la lettura dei file in uno scenario di Branch, ma alla fine i dati autorevoli continueranno a essere presenti su un solo server. per altre informazioni, vedere [qui](/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj127252(v=ws.11)). Anche in questo caso, i fornitori hanno le proprie soluzioni.

