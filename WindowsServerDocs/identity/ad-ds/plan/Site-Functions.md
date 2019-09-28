---
ms.assetid: 22c514b2-401e-49e1-a87e-0cbaa2c1dac1
title: Funzioni del sito
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 109f576bfdacf68a0eadc7dd84ddb9a4148e6dd9
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408671"
---
# <a name="site-functions"></a>Funzioni del sito

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

 Windows Server 2008 utilizza le informazioni del sito per molti scopi, tra cui replica di routing, affinità client, replica del volume di sistema (SYSVOL), file system distribuito spazi dei nomi (DFSN) e posizione del servizio.  
  
## <a name="routing-replication"></a>Routing della replica  
Active Directory Domain Services (AD DS) utilizza un metodo di replica multimaster, di archiviazione e di avanzamento. Un controller di dominio comunica le modifiche alla directory di un secondo controller di dominio, che quindi comunica a un terzo e così via, fino a quando tutti i controller di dominio non hanno ricevuto la modifica. Per ottenere il migliore equilibrio tra la riduzione della latenza di replica e la riduzione del traffico, i controlli della topologia del sito Active Directory la replica distinguendo la replica che si verifica all'interno di un sito e la replica eseguita tra siti.  
  
All'interno dei siti, la replica è ottimizzata per la velocità, gli aggiornamenti dei dati attivano la replica e i dati vengono inviati senza l'overhead richiesto dalla compressione dei dati. Viceversa, la replica tra siti viene compressa per ridurre al minimo il costo della trasmissione sui collegamenti Wide Area Network (WAN). Quando si verifica la replica tra siti, un singolo controller di dominio per ogni dominio in ogni sito raccoglie e archivia le modifiche della directory e le comunica a un momento pianificato in un controller di dominio in un altro sito.  
  
## <a name="client-affinity"></a>Affinità del client  
I controller di dominio utilizzano le informazioni del sito per informare Active Directory client sui controller di dominio presenti nel sito più vicino come client. Si consideri, ad esempio, un client nel sito di Seattle che non conosce l'affiliazione del sito e Contatta un controller di dominio dal sito Atlanta. In base all'indirizzo IP del client, il controller di dominio di Atlanta determina il sito da cui il client è effettivamente e invia le informazioni sul sito al client. Il controller di dominio informa inoltre il client se il controller di dominio scelto è quello più vicino. Il client memorizza nella cache le informazioni del sito fornite dal controller di dominio di Atlanta, esegue una query per il record di risorse del servizio specifico del sito (SRV), ovvero un record di risorse Domain Name System (DNS) usato per individuare i controller di dominio per servizi di dominio Active Directory e quindi trova un dominio controller all'interno dello stesso sito.  
  
Trovando un controller di dominio nello stesso sito, il client evita le comunicazioni sui collegamenti WAN. Se nel sito client non è presente alcun controller di dominio, un controller di dominio con le connessioni a costo più basso rispetto ad altri siti connessi annuncia se stesso (registra un record di risorse del servizio specifico del sito (SRV) nel sito che non dispone di un controller di dominio. I controller di dominio pubblicati in DNS sono quelli del sito più vicino come definito dalla topologia del sito. Questo processo garantisce che ogni sito disponga di un controller di dominio preferito per l'autenticazione.  
  
Per ulteriori informazioni sul processo di individuazione di un controller di dominio, vedere Active Directory Collection ([https://go.microsoft.com/fwlink/?LinkID=88626](https://go.microsoft.com/fwlink/?LinkID=88626)).  
  
## <a name="sysvol-replication"></a>Replica SYSVOL  
SYSVOL è una raccolta di cartelle nel file system esistente in ogni controller di dominio in un dominio. Le cartelle SYSVOL forniscono un percorso di Active Directory predefinito per i file che devono essere replicati in un dominio, inclusi oggetti Criteri di gruppo (GPO), script di avvio e arresto e script di accesso e disconnessione.  Windows Server 2008 può utilizzare il servizio Replica file (FRS) o la replica file system distribuito (DFSR) per replicare le modifiche apportate alle cartelle SYSVOL da un controller di dominio ad altri controller di dominio. FRS e DFSR replicano queste modifiche in base alla pianificazione creata durante la progettazione della topologia del sito.  
  
## <a name="dfsn"></a>DFSN  
DFSN utilizza le informazioni del sito per indirizzare un client al server che ospita i dati richiesti all'interno del sito. Se DFSN non trova una copia dei dati all'interno dello stesso sito del client, DFSN usa le informazioni sul sito in servizi di dominio Active Directory per determinare quali file server con dati condivisi DFSN sono più vicini al client.  
  
## <a name="service-location"></a>Posizione servizio  
Grazie alla pubblicazione di servizi quali servizi file e stampa in servizi di dominio Active Directory, è possibile consentire ai client di Active Directory di individuare il servizio richiesto nello stesso sito o in un sito più vicino. I servizi di stampa utilizzano l'attributo location archiviato in servizi di dominio Active Directory per consentire agli utenti di cercare le stampanti in base alla località senza conoscerne la posizione precisa. Per ulteriori informazioni sulla progettazione e la distribuzione di server di stampa, vedere Progettazione e distribuzione di server di stampa ([https://go.microsoft.com/fwlink/?LinkId=107041](https://go.microsoft.com/fwlink/?LinkId=107041)).  
  


