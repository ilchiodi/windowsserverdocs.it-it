---
title: Domain Name System (DNS)
description: In questo argomento viene fornita una panoramica del DNS in Windows Server 2016
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking-dns
ms.topic: article
ms.assetid: 1324ba18-4e28-4b9d-bbe7-75707e6d30ab
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 3d4ec63e904dd899a3ddc53a59274ad607136edd
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59870822"
---
# <a name="domain-name-system-dns"></a>Domain Name System (DNS)

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

Sistema DNS (Domain Name) è uno della famiglia di protocolli che comprendono TCP/IP standard del settore e il Client DNS e il Server DNS forniscono insieme computer name-a indirizzo IP mapping dei servizi di risoluzione a utenti e computer.  
  
> [!NOTE]  
> Oltre a questo argomento, il contenuto DNS seguente è disponibile.  
>   
> -   [Che cosa sono le novità di Client DNS](What-s-New-in-DNS-Client.md)  
> -   [Che cosa sono le novità di Server DNS](What-s-New-in-DNS-Server.md)  
> -   [Guida agli scenari dei criteri DNS](deploy/DNS-Policy-Scenario-Guide.md)  
> -   Video: [Windows Server 2016: Gestione del DNS in Gestione indirizzi IP](https://channel9.msdn.com/Blogs/windowsserver/Windows-Server-2016-DNS-management-in-IPAM)  
  
In Windows Server 2016, DNS è un ruolo del server che è possibile installare usando i comandi di Server Manager o Windows PowerShell. Se si sta installando un nuovo insieme di strutture Active Directory e un dominio, DNS viene installato automaticamente con Active Directory come server di catalogo globale per la foresta e del dominio.  
  
Il meccanismo di percorso controller di dominio Active Directory Domain Services (AD DS) basato su DNS. Quando una delle operazioni di Active Directory principale viene eseguita, ad esempio l'autenticazione, l'aggiornamento o la ricerca, i computer usano DNS per individuare i controller di dominio Active Directory. Inoltre, i controller di dominio usano DNS per individuare tra loro.  
  
Il servizio Client DNS è incluso in tutte le versioni client e server del sistema operativo Windows e viene eseguito per impostazione predefinita al momento dell'installazione del sistema operativo. Quando si configura una connessione di rete TCP/IP con l'indirizzo IP di un server DNS, il Client DNS esegue una query server DNS per individuare i controller di dominio e risolvere i nomi dei computer agli indirizzi IP. Ad esempio, quando un utente di rete con un account utente di Active Directory effettua l'accesso a un dominio di Active Directory, il servizio Client DNS esegue una query il server DNS per individuare un controller di dominio per il dominio di Active Directory. Quando il server DNS risponde alla query e fornisce l'indirizzo IP del controller di dominio al client, il client contatta il controller di dominio e può iniziare il processo di autenticazione.  
  
I servizi DNS Client e Server DNS di Windows Server 2016 usano il protocollo DNS che è incluso nella suite di protocolli TCP/IP. DNS fa parte del livello dell'applicazione del modello di riferimento di TCP/IP, come illustrato nella figura seguente.  
  
![DNS in TCP/IP](../media/Domain-Name-System--DNS-/dns_in_tcpip.jpg)  
  

