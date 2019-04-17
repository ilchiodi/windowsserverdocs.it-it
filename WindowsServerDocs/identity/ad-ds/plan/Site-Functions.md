---
ms.assetid: 22c514b2-401e-49e1-a87e-0cbaa2c1dac1
title: Funzioni del sito
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: f40122d84185c69fa19f5158c2b1c370ec1bfe82
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="site-functions"></a>Funzioni del sito

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

 Windows Server 2008 utilizza le informazioni del sito per diversi scopi, tra cui indirizzare la replica, affinità del client, la replica del volume (SYSVOL) del sistema, Distributed File System spazi dei nomi (DFSN) e il percorso del servizio.  
  
## <a name="routing-replication"></a>Replica del routing  
Servizi di dominio Active Directory (AD DS) utilizza un metodo multimaster, archiviazione e inoltro della replica. Un controller di dominio comunica modifiche alla directory in un secondo controller di dominio, quindi comunica a un terzo e così via, fino a quando tutti i controller di dominio hanno ricevuto la modifica. Per ottenere un equilibrio ottimale tra riducendo la latenza di replica e riducendo il traffico, la topologia del sito controlla la replica di Active Directory distinguendo tra la replica all'interno di un sito e la replica tra siti.  
  
All'interno dei siti, la replica è ottimizzata per la replica di trigger speeddata gli aggiornamenti e i dati vengono inviati senza l'overhead necessario per la compressione dei dati. Al contrario, la replica tra siti viene compresso per ridurre al minimo i costi di trasmissione su collegamenti di wide area network (WAN). Quando viene eseguita la replica tra siti, un singolo controller di dominio per ogni dominio in ogni sito raccoglie e archivia le modifiche nella directory e li comunica a un'ora pianificata per un controller di dominio in un altro sito.  
  
## <a name="client-affinity"></a>Affinità del client  
I controller di dominio utilizzano le informazioni del sito per indirizzare i client di Active Directory sui controller di dominio presenti all'interno del sito più vicino del client. Si consideri ad esempio un client nel sito di Seattle che non dispone di un rapporto relativo sito e contatta un controller di dominio dal sito di Atlanta. In base all'indirizzo IP del client, il controller di dominio di Atlanta determina quale sito il client è effettivamente da e invia le informazioni del sito al client. Il controller di dominio indica anche al client se il controller di dominio scelto è il più vicino a esso. Il client memorizza nella cache le informazioni sul sito forniti dal controller di dominio di Atlanta, le query per il record di risorse (SRV) del servizio specifico del sito (sistema DNS (Domain Name) record di risorsa utilizzato per individuare i controller di dominio per AD DS) e in tal modo consente di trovare un controller di dominio nello stesso sito.  
  
Per trovare un controller di dominio nello stesso sito, il client consente di evitare le comunicazioni su collegamenti WAN. Se nessun controller di dominio si trovano nel sito di client, un controller di dominio che dispone di connessioni il costo più basse rispetto ad altri siti connessi annuncia (Registra un record di risorse specifiche del sito del servizio (SRV) nel DNS) nel sito che non dispone di un controller di dominio. I controller di dominio che vengono pubblicati in DNS sono quelli dal sito più vicino, come definito per la topologia del sito. Questo processo assicura che ogni sito dispone di un controller di dominio preferito per l'autenticazione.  
  
Per ulteriori informazioni sul processo di individuazione di un controller di dominio, vedere l'insieme di Active Directory ([https://go.microsoft.com/fwlink/?LinkID=88626](https://go.microsoft.com/fwlink/?LinkID=88626)).  
  
## <a name="sysvol-replication"></a>Replica di SYSVOL  
SYSVOL è una raccolta di cartelle nel file system che esiste in ogni controller di dominio in un dominio. Le cartelle SYSVOL forniscono una posizione di Active Directory predefinita per i file che devono essere replicate in un dominio, inclusi gli oggetti Criteri di gruppo (GPO), script di avvio e arresto del sistema e gli script di accesso e disconnessione.  Windows Server 2008 è possibile utilizzare il servizio Replica File (FRS) o DFSR Distributed File System Replication () per replicare le modifiche apportate alle cartelle SYSVOL da un controller di dominio di altri controller di dominio. FRS e replica DFS per replicare tali modifiche in base alla pianificazione creati durante la progettazione della topologia del sito.  
  
## <a name="dfsn"></a>DFSN  
DFSN utilizza le informazioni del sito per indicare a un client al server che ospita i dati richiesti all'interno del sito. Se non trova una copia dei dati nello stesso sito del client DFSN, DFSN utilizza le informazioni del sito in Active Directory per determinare quali file server che ha DFSN condividere dati sono più vicina al client.  
  
## <a name="service-location"></a>Percorso del servizio  
La pubblicazione di servizi, ad esempio file e servizi di stampa in Active Directory, consentire ai client di Active Directory individuare il servizio richiesto all'interno della stessa o più vicino al sito. Servizi di stampa usano l'attributo percorso archiviato in Active Directory per consentire agli utenti di cercare le stampanti dal percorso senza conoscere la posizione esatta. Per ulteriori informazioni sulla progettazione e distribuzione di server di stampa, vedere Progettazione e distribuzione di server di stampa ([https://go.microsoft.com/fwlink/?LinkId=107041](https://go.microsoft.com/fwlink/?LinkId=107041)).  
  


