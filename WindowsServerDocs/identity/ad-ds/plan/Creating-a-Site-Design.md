---
ms.assetid: 83f746e5-81db-4610-9977-1d5c57699f50
title: Creazione di un progetto di sito
author: MicrosoftGuyJFlo
ms.author: joflore
manager: mtillman
ms.date: 05/31/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: 0464510b498296570bdef5edb122c6c26e1fe18f
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822774"
---
# <a name="creating-a-site-design"></a>Creazione di un progetto di sito

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

La creazione di una struttura di sito implica la scelta delle posizioni che diventeranno siti, la creazione di oggetti del sito, la creazione di oggetti subnet e l'associazione delle subnet con i siti.  
  
## <a name="deciding-which-locations-will-become-sites"></a>Decidere quali posizioni diventeranno siti

Decidere per quali percorsi creare i siti, come indicato di seguito:  
  
- Creare siti per tutte le posizioni in cui si prevede di collocare i controller di dominio. Per identificare le posizioni che includono i controller di dominio, fare riferimento alle informazioni documentate nel foglio di DSSTOPO_4 relativo alla posizione del controller di dominio (. doc).  
- Creare siti per i percorsi che includono i server che eseguono applicazioni che richiedono la creazione di un sito. Alcune applicazioni, ad esempio file system distribuito spazi dei nomi (DFSN), utilizzano oggetti sito per individuare i server più vicini ai client.  

   > [!NOTE]  
   > Se l'organizzazione dispone di più reti in prossimità di connessioni veloci e affidabili, è possibile includere tutte le subnet per le reti in un singolo sito Active Directory. Se, ad esempio, il round trip restituisce la latenza di rete tra due server in subnet diverse è 10 ms o meno, è possibile includere entrambe le subnet nello stesso sito Active Directory. Se la latenza di rete tra le due posizioni è maggiore di 10 ms, è consigliabile non includere le subnet in un singolo sito Active Directory. Anche se la latenza è di 10 ms o meno, è possibile scegliere di distribuire siti distinti se si vuole segmentare il traffico tra siti per applicazioni basate su Active Directory.  

- Se un sito non è necessario per una località, aggiungere la subnet del percorso a un sito per il quale la posizione ha la velocità massima di Wide Area Network (WAN) e la larghezza di banda disponibile.  
  
Percorsi dei documenti che diventeranno siti e indirizzi di rete e subnet mask all'interno di ogni percorso. Per un foglio di lavoro che fornirà supporto per la documentazione dei siti, vedere la pagina [relativa ai supporti per i processi per Windows Server 2003 Deployment Kit](https://go.microsoft.com/fwlink/?LinkID=102558), scaricare Job_Aids_Designing_and_Deploying_Directory_and_Security_Services. zip e aprire "associazione delle subnet ai siti" (DSSTOPO_6. doc).  
  
## <a name="creating-a-site-object-design"></a>Creazione di un progetto di oggetto sito

Per ogni posizione in cui si è deciso di creare siti, pianificare la creazione di oggetti sito in Active Directory Domain Services (AD DS). Percorsi dei documenti che diventeranno siti nel foglio di testo "associazione delle subnet con i siti".  
  
Per ulteriori informazioni sulla creazione di oggetti sito, vedere l'articolo [creare un sito](https://go.microsoft.com/fwlink/?LinkId=107067).  
  
## <a name="creating-a-subnet-object-design"></a>Creazione di un progetto di oggetto subnet

Per ogni subnet IP e subnet mask associata a ogni percorso, pianificare la creazione di oggetti subnet in servizi di dominio Active Directory che rappresentano tutti gli indirizzi IP all'interno del sito.  
  
Quando si crea un oggetto Active Directory subnet, le informazioni sulla subnet IP e sulla subnet mask della rete vengono convertite automaticamente nel formato della notazione della lunghezza del prefisso di rete <IP address>/<prefix length>. Ad esempio, l'indirizzo IP della rete versione 4 (IPv4) 172.16.4.0 con un subnet mask 255.255.252.0 viene visualizzato come 172.16.4.0/22. Oltre agli indirizzi IPv4, Windows Server 2008 supporta anche i prefissi di subnet IP versione 6 (IPv6), ad esempio 3FFE: FFFF: 0: C000::/64. Per ulteriori informazioni sulle subnet IP in ogni posizione, vedere il foglio di DSSTOPO_2 "posizioni e subnet" (. doc) in [raccolta di informazioni di rete](../../ad-ds/plan/Collecting-Network-Information.md) e [Appendice A: posizioni e prefissi di Subnet](Appendix-A--Locations-and-Subnet-Prefixes.md).  
  
Associare ogni oggetto subnet a un oggetto sito facendo riferimento al foglio di DSSTOPO_6 "associazione delle subnet con i siti" (. doc) nella sezione "scelta dei percorsi che diventeranno siti" per determinare la subnet da associare al sito. Documentare il Active Directory oggetto subnet associato a ogni posizione nel foglio di "associazione delle subnet con i siti" (DSSTOPO_6. doc).  
  
Per ulteriori informazioni su come creare oggetti subnet, vedere l'articolo [creare una subnet](https://go.microsoft.com/fwlink/?LinkId=107068).
