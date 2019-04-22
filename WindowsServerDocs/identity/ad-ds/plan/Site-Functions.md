---
ms.assetid: 22c514b2-401e-49e1-a87e-0cbaa2c1dac1
title: Funzioni del sito
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 0a330a9dae8ab8d3b9de5d1fff0e52060908f520
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59815812"
---
# <a name="site-functions"></a>Funzioni del sito

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

 Windows Server 2008 utilizza le informazioni del sito per vari scopi, inclusa la replica di routing, affinità del client, replica di sistema volume (SYSVOL), Distributed File System gli spazi dei nomi (DFSN) e posizione del servizio.  
  
## <a name="routing-replication"></a>Replica di routing  
Servizi di dominio Active Directory (AD DS) usa un metodo multimaster, archiviazione e inoltro di replica. Un controller di dominio comunica le modifiche di directory a un secondo controller di dominio, quindi comunica a un terzo e così via, fino a quando tutti i controller di dominio hanno ricevuto la modifica. Per ottenere il miglior rapporto tra la riduzione della latenza di replica e la riduzione del traffico, la topologia del sito controlla la replica di Active Directory, distinguendo tra replica che si verifica all'interno di un sito e che si verifica tra siti.  
  
All'interno dei siti, è con ottimizzazione per la replica per velocità, avviare la replica degli aggiornamenti di dati e i dati vengono inviati senza l'overhead necessario per la compressione dei dati. Invece, la replica tra siti viene compressa per ridurre al minimo i costi della trasmissione in collegamenti wide area network (WAN). Quando si verifica la replica tra siti, raccoglie e archivia le modifiche nella directory un singolo controller di dominio per ogni dominio in ogni sito e li comunica a un'ora pianificata per un controller di dominio in un altro sito.  
  
## <a name="client-affinity"></a>Affinità del client  
I controller di dominio utilizzano le informazioni del sito per segnalare ai client di Active Directory sui controller di dominio presenti all'interno del sito più vicino del client. Si consideri ad esempio un client nel sito di Seattle che non conosce relativa associazione di sito e contatta un controller di dominio dal sito ad Atlanta. Basato sull'indirizzo IP del client, il controller di dominio ad Atlanta determina quale sito il client è effettivamente in ordine e invia le informazioni del sito al client. Il controller di dominio indica inoltre il client, se il controller di dominio scelto è quella più vicina a esso. Il client memorizza nella cache le informazioni sul sito forniti dal controller di dominio ad Atlanta, le query per il record di risorse specifico del sito del servizio (SRV) (un sistema DNS (Domain Name) record di risorse usato per individuare i controller di dominio per AD DS) e in tal modo rileva che un dominio controller nello stesso sito.  
  
Mediante la ricerca di un controller di dominio nello stesso sito, il client consente di evitare le comunicazioni su collegamenti WAN. Se nessun controller di dominio situati nel sito del client, un controller di dominio con le connessioni di costo più basse rispetto a altro siti connessi promuove se stesso (Registra un record di risorse specifico del sito del servizio (SRV) nel DNS) nel sito che non dispone di un controller di dominio. I controller di dominio che vengono pubblicati in DNS sono quelli dal sito più vicino in base alla topologia dei siti. Questo processo assicura che ogni sito dispone di un controller di dominio preferito per l'autenticazione.  
  
Per altre informazioni sul processo di individuazione di un controller di dominio, vedere insieme Active Directory ([https://go.microsoft.com/fwlink/?LinkID=88626](https://go.microsoft.com/fwlink/?LinkID=88626)).  
  
## <a name="sysvol-replication"></a>Replica di SYSVOL  
SYSVOL è una raccolta di cartelle nel file system che esiste in ogni controller di dominio in un dominio. Le cartelle SYSVOL forniscono un percorso di Active Directory predefinito per i file che devono essere replicate in un dominio, inclusi gli oggetti Criteri di gruppo (GPO), script di avvio e arresto del sistema e gli script di accesso e disconnessione.  Windows Server 2008 è possibile usare il servizio Replica File (FRS) o DFSR Distributed File System Replication () per replicare le modifiche apportate alle cartelle SYSVOL da un controller di dominio ad altri controller di dominio. FRS e replica DFS per replicare tali modifiche in base alla pianificazione che crea durante la progettazione di topologia del sito.  
  
## <a name="dfsn"></a>DFSN  
DFSN Usa le informazioni del sito per indirizzare un client al server che ospita i dati all'interno del sito richiesti. Se DFSN non trova una copia dei dati nello stesso sito del client, DFSN utilizza le informazioni del sito in Active Directory Domain Services per determinare quali file server contenente DFSN di dati condivisi sono vicine al client.  
  
## <a name="service-location"></a>Posizione servizio  
Mediante la pubblicazione di servizi, ad esempio file e servizi di stampa in Active Directory Domain Services, è consentire ai client di Active Directory individuare il servizio richiesto all'interno della stessa o più vicino al sito. Servizi di stampa usano l'attributo di percorso archiviato in Active Directory Domain Services per consentire agli utenti di cercare le stampanti dal percorso senza conoscere la posizione esatta. Per altre informazioni sulla progettazione e distribuzione di server di stampa, vedere Progettazione e distribuzione di server di stampa ([https://go.microsoft.com/fwlink/?LinkId=107041](https://go.microsoft.com/fwlink/?LinkId=107041)).  
  


