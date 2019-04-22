---
ms.assetid: 83f746e5-81db-4610-9977-1d5c57699f50
title: Creazione di un progetto di sito
description: ''
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: f6d896213708f8c3ec5de44a1f85fb4ebd86b8c0
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59819072"
---
# <a name="creating-a-site-design"></a>Creazione di un progetto di sito

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Creazione di una progettazione sito implica decidere posizioni che diventeranno siti, la creazione di oggetti del sito, la creazione di oggetti subnet e associare la subnet con siti.  
  
## <a name="deciding-which-locations-will-become-sites"></a>Decidere posizioni che diventeranno siti

Decidere quali percorsi di creazione dei siti per come indicato di seguito:  
  
- Creare siti per tutte le posizioni in cui si prevede di inserire i controller di dominio. Vedere le informazioni documentate nel foglio di lavoro "dominio Controller" Selezione host (DSSTOPO_4.doc) per identificare i percorsi che includono controller di dominio.  
- Creare siti per tali percorsi che includono server che eseguono le applicazioni che necessitano di un sito da creare. Alcune applicazioni, ad esempio gli spazi dei nomi Distributed File System (DFSN), usano gli oggetti del sito per individuare i server più vicino ai client.  

   > [!NOTE]  
   > Se l'organizzazione dispone di più reti in stretta vicinanza con connessioni veloci e affidabili, è possibile includere tutte le subnet per le reti in un singolo sito di Active Directory. Ad esempio, se il valore restituito round trip latenza di rete tra due server in diverse subnet è 10 ms o poco meno, è possibile includere entrambe le subnet nello stesso sito Active Directory. Se la latenza di rete tra le due posizioni è superiore a 10 ms, non è necessario includere le subnet in un singolo sito di Active Directory. Anche quando la latenza è 10 ms o poco meno, è possibile scegliere di distribuire siti separati, se si desidera segmentare il traffico tra siti per le applicazioni basate su Active Directory.  

- Se un sito non è obbligatorio per una località, aggiungere la subnet dell'indirizzo di un sito per il quale il percorso è la velocità di rete WAN WAN massima e la larghezza di banda disponibile.  
  
Posizioni che diventeranno siti e gli indirizzi di rete e subnet mask all'interno di ogni percorso del documento. Per un foglio di lavoro agevolare la documentazione di siti, vedere [processo di supporto per Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), scaricare Job_Aids_Designing_and_Deploying_Directory_and_Security_Services.zip e aprire "associazione subnet con Siti"(DSSTOPO_6.doc).  
  
## <a name="creating-a-site-object-design"></a>Creazione di una progettazione di oggetti del sito

Per ogni percorso in cui si è deciso di creare siti, prevede di creare gli oggetti del sito in servizi di dominio Active Directory (AD DS). Percorsi di documenti che diventeranno siti nel foglio di lavoro "Associazione di subnet con siti".  
  
Per altre informazioni su come creare gli oggetti del sito, vedere l'articolo [creare un sito](https://go.microsoft.com/fwlink/?LinkId=107067).  
  
## <a name="creating-a-subnet-object-design"></a>Creazione di una progettazione di oggetti subnet

Per ogni subnet IP e subnet mask associata a ogni posizione, prevede di creare oggetti subnet nel dominio di Active Directory che rappresenta tutti gli indirizzi IP all'interno del sito.  
  
Quando si crea un oggetto subnet di Active Directory, le informazioni sulla subnet IP di rete e subnet mask viene automaticamente convertite in formato di notazione lunghezza di prefisso di rete <IP address> / <prefix length>. Ad esempio, la rete IP versione 4 (IPv4) indirizzo 172.16.4.0 con una subnet mask 255.255.252.0 viene visualizzato come 172.16.4.0/22. Oltre agli indirizzi IPv4, Windows Server 2008 supporta inoltre IP versione 6 (IPv6) prefissi di subnet, ad esempio, 3FFE:FFFF:0:C000::/ 64. Per altre informazioni sulle subnet IP in ogni posizione, vedere il foglio di lavoro "Percorsi e subnet" (DSSTOPO_2.doc) [la raccolta di informazioni di rete](../../ad-ds/plan/Collecting-Network-Information.md) e [appendice a: Posizioni e prefissi di Subnet](Appendix-A--Locations-and-Subnet-Prefixes.md).  
  
Associare ogni oggetto di subnet e un oggetto sito facendo riferimento al foglio di lavoro "Associazione di subnet con siti" (DSSTOPO_6.doc) nella sezione "Decidere quali percorsi verranno diventano siti" per determinare la subnet in cui deve essere associato a quale sito. Documento l'oggetto subnet di Active Directory associato a ogni posizione nel foglio di lavoro "Associazione di subnet con siti" (DSSTOPO_6.doc).  
  
Per altre informazioni su come creare oggetti subnet, vedere l'articolo [creare una Subnet](https://go.microsoft.com/fwlink/?LinkId=107068).
