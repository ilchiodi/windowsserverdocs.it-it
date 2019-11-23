---
title: Domain Name System (DNS)
description: Questo argomento fornisce una panoramica del DNS in Windows Server 2016
manager: brianlic
ms.prod: windows-server
ms.technology: networking-dns
ms.topic: article
ms.assetid: 1324ba18-4e28-4b9d-bbe7-75707e6d30ab
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 6ad3b66ff0b271c3b6f6134a96aaf6b5171bc7d4
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71406163"
---
# <a name="domain-name-system-dns"></a>Domain Name System (DNS)

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Domain Name System (DNS) è una delle suite di protocolli standard di settore che includono TCP/IP e insieme il client DNS e il server DNS forniscono servizi di risoluzione dei nomi di mapping di indirizzi IP da nome computer a computer e utenti.  
  
> [!NOTE]  
> Oltre a questo argomento, è disponibile il seguente contenuto DNS.  
>   
> -   [Novità del client DNS](What-s-New-in-DNS-Client.md)  
> -   [Novità del server DNS](What-s-New-in-DNS-Server.md)  
> -   [Guida allo scenario dei criteri DNS](deploy/DNS-Policy-Scenario-Guide.md)  
> -   Video: [Windows Server 2016: gestione DNS in gestione](https://channel9.msdn.com/Blogs/windowsserver/Windows-Server-2016-DNS-management-in-IPAM) indirizzi IP  
  
In Windows Server 2016, DNS è un ruolo del server che è possibile installare usando Server Manager o i comandi di Windows PowerShell. Se si installa una nuova foresta Active Directory e un dominio, il DNS viene installato automaticamente con Active Directory come server di catalogo globale per la foresta e il dominio.  
  
Active Directory Domain Services (AD DS) USA DNS come meccanismo di posizione del controller di dominio. Quando viene eseguita una delle operazioni Active Directory principali, ad esempio l'autenticazione, l'aggiornamento o la ricerca, i computer utilizzano DNS per individuare Active Directory controller di dominio. Inoltre, i controller di dominio utilizzano DNS per individuarsi reciprocamente.  
  
Il servizio client DNS è incluso in tutte le versioni client e server del sistema operativo Windows ed è in esecuzione per impostazione predefinita durante l'installazione del sistema operativo. Quando si configura una connessione di rete TCP/IP con l'indirizzo IP di un server DNS, il client DNS esegue una query sul server DNS per individuare i controller di dominio e per risolvere i nomi dei computer in indirizzi IP. Ad esempio, quando un utente di rete con un account utente Active Directory accede a un dominio Active Directory, il servizio client DNS esegue una query sul server DNS per individuare un controller di dominio per il dominio di Active Directory. Quando il server DNS risponde alla query e fornisce l'indirizzo IP del controller di dominio al client, il client contatta il controller di dominio e il processo di autenticazione può iniziare.  
  
Il server DNS di Windows Server 2016 e i servizi client DNS utilizzano il protocollo DNS incluso nella suite di protocolli TCP/IP. Il DNS fa parte del livello applicazione del modello di riferimento TCP/IP, come illustrato nella figura seguente.  
  
![DNS in TCP/IP](../media/Domain-Name-System--DNS-/dns_in_tcpip.jpg)  
  

