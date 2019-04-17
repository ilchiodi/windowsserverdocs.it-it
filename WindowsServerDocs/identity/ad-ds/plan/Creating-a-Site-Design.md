---
ms.assetid: 83f746e5-81db-4610-9977-1d5c57699f50
title: Creazione di una progettazione di sito
description: 
author: billmath
ms.author: billmath
manager: femila
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 2bcbf30159721e1fc2e12af103ca6f3e79f3ec97
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="creating-a-site-design"></a>Creazione di una progettazione di sito

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Creazione di una progettazione di sito implica la scelta delle posizioni che diventeranno siti, la creazione di oggetti del sito, la creazione di oggetti subnet e associando le subnet con siti.  
  
## <a name="deciding-which-locations-will-become-sites"></a>Decidere quali posizioni che diventeranno siti  
Decidere quali posizioni per creare i siti per come indicato di seguito:  
  
-   Creazione di siti per tutti i percorsi in cui si prevede di collocare i controller di dominio. Fare riferimento alle informazioni documentate nel foglio di lavoro (DSSTOPO_4.doc) "Posizionamento del Controller di dominio" per identificare le posizioni che includono controller di dominio.  
  
-   Creazione di siti per tali percorsi che includono i server che eseguono le applicazioni che richiedono un sito da creare. Alcune applicazioni, ad esempio il spazi dei nomi Distributed File System (DFSN), utilizzano gli oggetti del sito per individuare i server più vicini ai client.  
  
    > [!NOTE]  
    > Se l'organizzazione dispone di più reti in prossimità con connessioni veloce e affidabile, è possibile includere tutte le subnet per quelle stesse reti in un singolo sito di Active Directory. Ad esempio, se la restituzione di andata e ritorno della rete latenza tra due server in diverse subnet è 10 ms o meno, è possibile includere entrambi subnet nello stesso sito Active Directory. Se la latenza di rete tra le due posizioni è maggiore di 10 ms, non includere le subnet in un singolo sito di Active Directory. Anche quando la latenza è 10 ms o meno, è possibile scegliere di distribuire siti separati per segmentare il traffico tra siti per le applicazioni basate su Active Directory.  
  
-   Se un sito non è richiesto per una posizione, aggiungere la subnet del percorso a un sito per cui il percorso ha la massima wide area network (WAN) velocità e la larghezza di banda disponibile.  
  
Documentare le posizioni che diventeranno siti e subnet mask all'interno di ogni posizione e gli indirizzi di rete. Per un foglio di lavoro agevolare la documentazione di siti, vedere processo Aid per Windows Server 2003 Deployment Kit ([https://go.microsoft.com/fwlink/?LinkID=102558](https://go.microsoft.com/fwlink/?LinkID=102558)), scaricare Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip e (DSSTOPO_6.doc) "Associazione di subnet con siti aperti".  
  
## <a name="creating-a-site-object-design"></a>Creazione di una progettazione di oggetti del sito  
Per ogni percorso in cui si è scelto di creare siti, pianificare creare gli oggetti del sito in servizi di dominio Active Directory (AD DS). Percorsi di documento che diventeranno siti nel foglio di lavoro di "associazione di subnet con siti".  
  
Per ulteriori informazioni su come creare gli oggetti del sito, vedere Creazione di un sito ([https://go.microsoft.com/fwlink/?LinkId=107067](https://go.microsoft.com/fwlink/?LinkId=107067)).  
  
## <a name="creating-a-subnet-object-design"></a>Creazione di una progettazione di oggetto subnet  
Per ogni subnet IP e il subnet mask associata ogni percorso, prevede di creare oggetti subnet in Active Directory che rappresenta tutti gli indirizzi IP all'interno del sito.  
  
Quando si crea un oggetto subnet di Active Directory, le informazioni sulla subnet IP di rete e subnet mask viene tradotto automaticamente nel formato notazione lunghezza prefisso rete <IP address>/<prefix length>. Ad esempio, rete IP versione 4 (IPv4) indirizzo 172.16.4.0 con una subnet mask 255.255.252.0 viene visualizzato come 172.16.4.0/22. Oltre agli indirizzi IPv4, Windows Server 2008 supporta inoltre IP versione 6 (IPv6) prefissi di subnet, ad esempio, 3FFE:FFFF:0:C000:::/ 64. Per ulteriori informazioni sulle subnet IP in ogni percorso, vedere "Percorsi e subnet" (DSSTOPO_2.doc) foglio di lavoro [la raccolta di informazioni sulla rete](../../ad-ds/plan/Collecting-Network-Information.md) e [appendice a: posizioni e prefissi di Subnet ](Appendix-A--Locations-and-Subnet-Prefixes.md).  
  
Associare ogni oggetto subnet con un oggetto sito facendo riferimento al foglio di lavoro (DSSTOPO_6.doc) "associazione di subnet con siti" nella sezione "Decidere quali percorsi verrà diventano siti" per determinare la subnet in cui è da associare a quale sito. Documentare l'oggetto subnet di Active Directory associato a ogni posizione nel foglio di lavoro (DSSTOPO_6.doc) "associazione di subnet con siti".  
  
Per ulteriori informazioni su come creare oggetti subnet, vedere Creazione di una Subnet ([https://go.microsoft.com/fwlink/?LinkId=107068](https://go.microsoft.com/fwlink/?LinkId=107068)).  
  


