---
ms.assetid: eb600904-24b8-4488-a278-c1c971dc2f2d
title: Pianificazione della selezione del Controller di dominio regionale
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: bec8595ab6eae8eb6cedaf9307ab97ac9c8316b8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59880442"
---
# <a name="planning-regional-domain-controller-placement"></a>Pianificazione della selezione del Controller di dominio regionale

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Per garantire l'efficienza dei costi, desidera collocare il controller di dominio regionale il minor numero possibile. In primo luogo, esaminare il foglio di lavoro "Geografica posizioni e collegamenti di comunicazione" (DSSTOPO_1.doc) utilizzato in [la raccolta di informazioni di rete](../../ad-ds/plan/Collecting-Network-Information.md) per determinare se un percorso è un hub.  
  
Prevede di inserire i controller di dominio regionale per ogni dominio che viene rappresentato in ogni posizione dell'hub. Dopo aver inserito i controller di dominio regionale in tutti i percorsi di hub, valutare la necessità di posizionamento dei controller di dominio regionale in posizioni satellite. Eliminando i controller di dominio internazionali non necessari dai percorsi satellite riduce i costi di supporto necessari per gestire l'infrastruttura di un server remoto.  
  
Inoltre, garantire la sicurezza fisica dei controller di dominio nei percorsi di hub e satellite in modo che il personale non autorizzato non è possibile accedervi. Non inserire i controller di dominio scrivibili nei percorsi di hub e satellite in cui non è possibile garantire la sicurezza fisica del controller di dominio. Una persona che ha accesso fisico a un controller di dominio scrivibile può attaccare il sistema da:  
  
- L'accesso a dischi fisici avviando un altro sistema operativo in un controller di dominio.  
- La rimozione (e possibilmente sostituendo) dischi fisici in un controller di dominio.  
- Recupero e la modifica di una copia di un backup dello stato sistema del controller di dominio.  
  
Aggiungere i controller di dominio a livello di area accessibile in scrittura solo in posizioni in cui è possibile garantire la sicurezza fisica.  
  
In posizioni inadeguate sicurezza fisica, la distribuzione di un controller di dominio di sola lettura (RODC) è la soluzione consigliata. Fatta eccezione per le password degli account, un RODC contiene tutti gli oggetti Active Directory e attributi che contiene un controller di dominio scrivibile. Tuttavia, è Impossibile apportare modifiche al database che viene archiviato nel RODC. Le modifiche devono essere eseguite in un controller di dominio scrivibile e successivamente replicate nel RODC.  
  
Per autenticare gli accessi client e l'accesso ai file server locali, la maggior parte delle organizzazioni collocare i controller di dominio regionale per tutti i domini regionali sono rappresentati in una posizione specificata. Tuttavia, è necessario considerare numerose variabili quando si valuta se un percorso aziendale richiede di effettuare l'autenticazione locale dai relativi client o il client può contare su autenticazione e query su un collegamento di wide area network (WAN). Nella figura seguente viene illustrato come determinare se installare il controller di dominio in posizioni satellite.  
  
![posizionamento di controller di dominio regionale piano](media/Planning-Regional-Domain-Controller-Placement/49892c8c-2c99-4aab-92ba-808dbc8048e2.gif)  
  
## <a name="onsite-technical-expertise-availability"></a>Disponibilità delle proprie competenze tecniche in loco

I controller di dominio devono essere gestiti in modo continuativo per vari motivi. Inserire un controller di dominio regionale solo in percorsi che includono il personale che possono amministrare il controller di dominio o essere certi che il controller di dominio può essere gestito in remoto.  
  
In succursali con la sicurezza fisica in genere scarsa e personale con scarsa conoscenza di tecnologia di informazioni, la distribuzione di un RODC è spesso la soluzione consigliata. Le autorizzazioni amministrative locali per un controller di dominio possono essere delegate a qualsiasi utente di dominio senza concedergli i diritti utente per il dominio o altri controller di dominio. Ciò consente all'utente di accedere a un RODC ed eseguire interventi di manutenzione nel server, ad esempio l'aggiornamento di un driver branch locale. Tuttavia, l'utente di ramo non può accedere a qualsiasi altro controller di dominio o eseguire altre attività amministrative nel dominio. In questo modo, l'utente di branch può essere delegata la possibilità di gestire in modo efficace il RODC nella succursale senza compromettere la protezione del resto del dominio o foresta.  
  
## <a name="wan-link-availability"></a>Disponibilità del collegamento WAN

Collegamenti WAN che verificano frequenti interruzioni possono causare perdite di produttività significativi per gli utenti se il percorso non include un controller di dominio in grado di autenticare gli utenti. Se la disponibilità del collegamento WAN non è al 100% e i siti remoti non possono tollerare un'interruzione del servizio, inserire un controller di dominio regionale in posizioni in cui gli utenti richiedono la possibilità di effettuare l'accesso o l'accesso al server di exchange durante il collegamento WAN è inattivo.  
  
## <a name="authentication-availability"></a>Disponibilità di autenticazione

Alcune organizzazioni come banche, richiedono che gli utenti autenticati in qualsiasi momento. Inserire un controller di dominio regionale in una posizione in cui la disponibilità di collegamento WAN non è al 100%, ma gli utenti richiedono l'autenticazione in qualsiasi momento.  
  
## <a name="logon-performance-over-wan-links"></a>Prestazioni di accesso sui collegamenti WAN

Se la disponibilità del collegamento WAN è estremamente affidabile, l'inserimento di un controller di dominio in corrispondenza della posizione dipende i requisiti di prestazioni di accesso tramite il collegamento WAN. Fattori che influenzano le prestazioni di accesso tramite la rete WAN includono velocità del collegamento e larghezza di banda disponibile, numero di utenti e i profili di utilizzo e la quantità di traffico di rete di accesso e il traffico di replica.  
  
### <a name="wan-link-speed-and-bandwidth-utilization"></a>Utilizzo della larghezza di banda e velocità di collegamento WAN

Le attività di un singolo utente possono congest un collegamento WAN lento. Inserire un controller di dominio in una posizione se le prestazioni di accesso tramite il collegamento WAN non sono accettabile.  
  
La percentuale media di utilizzo della larghezza di banda indica come sovraccarica un collegamento di rete. Se un collegamento di rete presentano un utilizzo medio della larghezza di banda che è maggiore di un valore accettabile, inserire un controller di dominio in tale posizione.  
  
### <a name="number-of-users-and-usage-profiles"></a>Numero di utenti e i profili di utilizzo

Il numero di utenti e i relativi profili di utilizzo in una posizione specificata può aiutare a determinare se è necessario inserire i controller di dominio regionale in quella posizione. Per evitare perdite di produttività se ha esito negativo di un collegamento WAN, inserire un controller di dominio regionale in una posizione che dispone di 100 o più utenti.  
  
I profili di utilizzo indicano come gli utenti di usano le risorse di rete. Non è necessaria inserire un controller di dominio in un percorso che contiene solo alcuni degli utenti non accedono spesso a risorse di rete.  
  
### <a name="logon-network-traffic-vs-replication-traffic"></a>Traffico di rete di accesso e il traffico di replica

Se un controller di dominio non è disponibile nella stessa località del client Active Directory, il client crea il traffico di accesso di rete. La quantità di traffico di rete di accesso che viene creata nella rete fisica è influenzata da diversi fattori, tra cui appartenenze; numero e dimensioni degli oggetti Criteri di gruppo (GPO); script di accesso; e funzionalità, ad esempio cartelle non in linea, reindirizzamento cartelle e profili mobili.  
  
D'altra parte, un controller di dominio che viene inserito in una posizione specificata genera traffico di replica sulla rete. La frequenza e la quantità di aggiornamenti per le partizioni ospitate nei controller di dominio per determinare la quantità di traffico di replica che viene creata nella rete. I diversi tipi di aggiornamenti che possono essere fatta in partizioni ospitate nei controller di dominio includono l'aggiunta o modifica utenti e gli attributi utente, modifica delle password e aggiunta o la modifica gruppi globali, stampanti o volumi.  
  
Per determinare se è necessario inserire un controller di dominio regionale in corrispondenza della posizione, confronta il costo del traffico di accesso creato da un percorso senza un controller di dominio e il costo del traffico di replica creato mediante l'inserimento di un controller di dominio in corrispondenza della posizione.  
  
Si consideri ad esempio una rete dotata di succursali che sono connessi tramite collegamenti lenti alla sede centrale e in quali controller di dominio possono essere aggiunte facilmente. Se il traffico giornaliero per ricerca di accesso e directory di alcuni utenti del sito remoto comporta maggiore traffico di rete rispetto alla replica di tutti i dati aziendali al ramo, considerare l'aggiunta di un controller di dominio per il ramo.  
  
Se è più importante che il traffico di rete riducendo i costi di manutenzione di controller di dominio, centralizzare i controller di dominio per tale dominio e non inserire qualsiasi controller di dominio regionale in corrispondenza della posizione o è consigliabile inserire i RODC in corrispondenza della posizione.  
  
Per un foglio di lavoro agevolare così a documentare la posizione dei controller di dominio a livello di area e il numero di utenti per ogni dominio che viene rappresentato in ogni posizione, vedere [processo di supporto per Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), scaricare Job_ Aids_Designing_and_Deploying_Directory_and_Security_Services.zip e Apri "dominio Controller"Selezione host (DSSTOPO_4.doc).  
  
È necessario fare riferimento alle informazioni sui percorsi in cui è necessario inserire i controller di dominio a livello di area durante la distribuzione di domini regionali. Per ulteriori informazioni sulla distribuzione di domini regionali, vedere [la distribuzione di Windows Server 2008 domini regionali](https://technet.microsoft.com/library/cc755118.aspx).  
