---
ms.assetid: eb600904-24b8-4488-a278-c1c971dc2f2d
title: Pianificazione posizionamento del Controller di dominio regionale
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: c5254560e9b1a10030fa37ec0966868986212a4a
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="planning-regional-domain-controller-placement"></a>Pianificazione posizionamento del Controller di dominio regionale

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Per garantire efficienza dei costi, prevede di collocare i controller di dominio regionale il minor numero possibile. Prima di tutto, esaminare il foglio di lavoro (DSSTOPO_1.doc) "Geografica percorsi e collegamenti di comunicazione" utilizzato in [la raccolta di informazioni sulla rete](../../ad-ds/plan/Collecting-Network-Information.md) per determinare se un percorso è un hub.  
  
Se si intende collocare i controller di dominio regionale per ogni dominio che è rappresentato in ogni posizione hub. Dopo aver inserito i controller di dominio regionale in tutti i percorsi hub, valutare la necessità di posizionamento dei controller di dominio regionale posizioni satellite. Eliminando i controller di dominio internazionali non necessari dai percorsi satellite riduce i costi di supporto necessari per gestire un'infrastruttura server remoto.  
  
Verificare inoltre la sicurezza fisica dei controller di dominio hub e satellite percorsi in modo che il personale non autorizzato non possono accedervi. Non inserire i controller di dominio scrivibile in hub e satellite posizioni in cui non è possibile garantire la sicurezza fisica del controller di dominio. Una persona che ha accesso fisico a un controller di dominio scrivibile può attaccare il sistema da:  
  
-   Accesso ai dischi fisici avviando un altro sistema operativo in un controller di dominio.  
  
-   La rimozione (ed eventualmente sostituendo) dischi fisici in un controller di dominio.  
  
-   Recupero e la modifica di una copia di un backup dello stato del sistema controller dominio.  
  
Aggiungere controller di dominio regionale scrivibili solo ai percorsi in cui è possibile garantire la sicurezza fisica.  
  
In percorsi con adeguata sicurezza fisica, la distribuzione di un controller di dominio di sola lettura (RODC) è la soluzione consigliata. Fatta eccezione per le password degli account, un RODC contiene tutti gli oggetti di Active Directory e gli attributi che contiene un controller di dominio scrivibile. Tuttavia, è Impossibile apportare modifiche al database archiviati nel RODC. Modifiche devono essere eseguite in un controller di dominio scrivibile e quindi replicate nel RODC.  
  
Per autenticare gli accessi client e accesso ai file server locali, la maggior parte delle organizzazioni collocare i controller di dominio regionale per tutti i domini regionali sono rappresentati in una determinata posizione. Tuttavia, è necessario considerare numerose variabili quando valuta se la sede aziendale richiede i relativi client per l'autenticazione locale oppure i client possono sfruttare l'autenticazione e la query su un collegamento di wide area network (WAN). Nella figura seguente viene illustrato come determinare se installare il controller di dominio in posizioni satellite.  
  
![Posizionamento di controller di dominio regionale piano](media/Planning-Regional-Domain-Controller-Placement/49892c8c-2c99-4aab-92ba-808dbc8048e2.gif)  
  
## <a name="onsite-technical-expertise-availability"></a>Disponibilità di competenze tecniche in loco  
I controller di dominio devono essere gestiti in modo continuo per diversi motivi. Inserire un controller di dominio regionale solo nei percorsi che includono il personale che possono amministrare il controller di dominio o assicurarsi che il controller di dominio può essere gestito in modalità remota.  
  
In ambienti di filiale con in genere la sicurezza fisica e il personale con conoscenze di tecnologia informazioni limitate, la distribuzione di un RODC è spesso la soluzione consigliata. Le autorizzazioni amministrative locali per un controller di dominio possono essere delegate a qualsiasi utente di dominio senza concedergli i diritti utente per il dominio o altri controller di dominio. Ciò consente all'utente di accedere a un RODC ed eseguire interventi di manutenzione nel server, ad esempio l'aggiornamento di un driver locale della succursale. Tuttavia, l'utente ramo non può accedere a qualsiasi altro controller di dominio o eseguire altre attività amministrative nel dominio. In questo modo, l'utente ramo può essere delegata la possibilità di gestire in modo efficace il RODC nella succursale senza compromettere la protezione del resto del dominio o foresta.  
  
## <a name="wan-link-availability"></a>Disponibilità del collegamento WAN  
Collegamenti WAN che frequente si verificano possono causare la perdita di produttività significativi per gli utenti se il percorso non include un controller di dominio in grado di autenticare gli utenti. Se la disponibilità del collegamento WAN non è pari al 100% e dei siti remoti non sono in grado di tollerare un'interruzione del servizio, inserire un controller di dominio regionale in posizioni in cui gli utenti richiedono la possibilità di accedere o l'accesso a exchange server quando il collegamento WAN.  
  
## <a name="authentication-availability"></a>Disponibilità di autenticazione  
Alcune organizzazioni come banche, è necessario che gli utenti autenticati in qualsiasi momento. Inserire un controller di dominio regionale in una posizione in cui la disponibilità del collegamento WAN non è pari al 100%, ma gli utenti richiedono l'autenticazione in qualsiasi momento.  
  
## <a name="logon-performance-over-wan-links"></a>Prestazioni di accesso sui collegamenti WAN  
Se la disponibilità del collegamento WAN è estremamente affidabile, inserimento di un controller di dominio in corrispondenza della posizione dipende i requisiti di prestazioni di accesso tramite il collegamento WAN. Fattori che influenzano le prestazioni di accesso tramite la rete WAN includono velocità del collegamento e larghezza di banda disponibile, numero di utenti e i profili di utilizzo e la quantità di traffico di rete di accesso e il traffico di replica.  
  
### <a name="wan-link-speed-and-bandwidth-utilization"></a>Utilizzo della larghezza di banda e di velocità del collegamento WAN  
Le attività di un singolo utente possono congest un collegamento WAN lento. Inserire un controller di dominio in una posizione se le prestazioni di accesso tramite il collegamento WAN sono accettabile.  
  
La percentuale media di utilizzo della larghezza di banda indica congestionati come un collegamento di rete. Se un collegamento di rete ha utilizzo medio della larghezza di banda maggiore del valore accettabile, inserire un controller di dominio in tale percorso.  
  
### <a name="number-of-users-and-usage-profiles"></a>Numero di utenti e i profili di utilizzo  
Il numero di utenti e i relativi profili di utilizzo in una determinata posizione consente di determinare se è necessario inserire i controller di dominio regionale in tale percorso. Per evitare perdite di produttività, in caso di errore di un collegamento WAN, inserire un controller di dominio regionale in una posizione che dispone di 100 o più utenti.  
  
I profili di utilizzo indicano come gli utenti possono utilizzare le risorse di rete. Non devi inserire un controller di dominio in un percorso che contenga solo alcuni utenti che non accedono spesso le risorse di rete.  
  
### <a name="logon-network-traffic-vs-replication-traffic"></a>Traffico di rete di accesso e il traffico di replica  
Se un controller di dominio non è disponibile all'interno di nello stesso percorso del client Active Directory, il client crea il traffico di accesso in rete. La quantità di traffico di rete di accesso che viene creato sulla rete fisica è influenzata da diversi fattori, inclusi l'appartenenza al gruppo; numero e dimensioni di oggetti Criteri di gruppo (GPO); script di accesso. e funzionalità, ad esempio cartelle offline, reindirizzamento cartelle e profili mobili.  
  
D'altra parte, un controller di dominio che viene posizionato in una determinata posizione genera il traffico di replica sulla rete. La frequenza e la quantità di aggiornamenti per le partizioni ospitate nei controller di dominio per determinare la quantità di traffico di replica che viene creato nella rete. I diversi tipi di aggiornamenti che possono essere effettuati sulle partizioni ospitate nei controller di dominio includono l'aggiunta o modifica utenti e gli attributi utente, la modifica delle password e l'aggiunta o modifica i gruppi globali, stampanti o volumi.  
  
Per determinare se è necessario inserire un controller di dominio regionale in una posizione, confrontare il costo del traffico di accesso creato da un percorso senza un controller di dominio e il costo del traffico di replica creato inserendo un controller di dominio nel percorso.  
  
Si consideri ad esempio una rete dotata di succursali che sono connessi tramite collegamenti lenti alla sede centrale e in quali controller di dominio è possibile aggiungere facilmente. Se il traffico di rete più rispetto alla replica di tutti i dati aziendali al ramo fa sì che il traffico giornaliero di ricerca di accesso e directory di alcuni utenti del sito remoto, è possibile aggiungere un controller di dominio al ramo.  
  
Se è più importante che il traffico di rete riducendo i costi di manutenzione di controller di dominio, centralizzare i controller di dominio per il dominio e non inserire tutti i controller di dominio regionale nella posizione oppure considerare il posizionamento di lettura in corrispondenza della posizione.  
  
Per un foglio di lavoro agevolare la documentazione del posizionamento del controller di dominio regionale e il numero di utenti per ogni dominio che è rappresentato in ogni percorso, vedere processo Aid per Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)), scaricare < DICT__Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip>Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip</DICT__Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip>, and Aprire (DSSTOPO_4.doc) "Posizionamento del Controller di dominio".  
  
È necessario fare riferimento alle informazioni sui percorsi in cui è necessario inserire i controller di dominio regionale durante la distribuzione di domini regionali. Per ulteriori informazioni sulla distribuzione di domini regionali, vedere [la distribuzione di Windows Server 2008 domini regionali](https://technet.microsoft.com/library/cc755118.aspx).  
  


