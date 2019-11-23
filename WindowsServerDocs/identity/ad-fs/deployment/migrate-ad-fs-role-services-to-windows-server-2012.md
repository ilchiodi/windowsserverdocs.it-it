---
title: Eseguire la migrazione dei servizi ruolo di Active Directory Federation Services a Windows Server 2012
description: Vengono fornite istruzioni per la migrazione del servizio AD FS a Windows Server 2012.
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: cdb5523ade5c3c7572656d62d1b4f744683ec96e
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71408271"
---
# <a name="migrate-active-directory-federation-services-role-services-to-windows-server-2012"></a>Eseguire la migrazione dei servizi ruolo di Active Directory Federation Services a Windows Server 2012

Di seguito vengono fornite istruzioni sulla migrazione dei servizi ruolo seguenti a Active Directory Federation Services (AD FS) in Windows Server 2012:  
  
-   AD FS 1,1 agente basato su token Windows e AD FS 1,1 agente in grado di riconoscere attestazioni installato con Windows Server 2008 o Windows Server 2008 R2  
  
-   AD FS 2,0 server federativo e il proxy server federativo di AD FS 2,0 installato in Windows Server 2008 o Windows Server 2008 R2    
  
## <a name="supported-migration-scenarios"></a>Scenari di migrazione supportati  
 Le istruzioni per la migrazione includono le attività seguenti:  
  
- Esportazione dei dati di configurazione di AD FS 2,0 dal server che esegue Windows Server 2008 o Windows Server 2008 R2  
  
- Esecuzione di un aggiornamento sul posto del sistema operativo del server da Windows Server 2008 o Windows Server 2008 R2 a Windows Server 2012.
  
- Ricreare la configurazione originale del AD FS e ripristinare le impostazioni del servizio AD FS rimanenti su questo server, che ora esegue il ruolo del server AD FS installato con Windows Server 2012.  
  
  Questa guida non include istruzioni per la migrazione di un server che esegue più ruoli. Se nel server vengono eseguiti più ruoli, è consigliabile progettare un processo di migrazione personalizzato specifico del proprio ambiente server, basandosi sulle informazioni disponibili in altre guide alla migrazione dei ruoli. Le guide alla migrazione per altri ruoli sono disponibili nel [portale della migrazione per Windows Server](https://go.microsoft.com/fwlink/?LinkId=247608).  
  
## <a name="supported-operating-systems"></a>Sistemi operativi supportati  
 **Sistema operativo del server di destinazione:**  
  

- Windows Server 2012 o Windows Server 2008 R2 (opzioni di base del server e di installazione completa)  
  
  **Processore del server di destinazione:**  
  

- Basato su x64  
  
|Processore del server di origine|Sistema operativo del server di origine|  
|-----|-----|  
|Basato su x86 o su x64|Windows Server 2003 con Service Pack 2|  
|Basato su x86 o su x64|Windows Server 2003 R2|  
|Basato su x86 o su x64|Windows Server 2008, opzioni di installazione completa e Server Core|  
|Basato su x64|Windows Server 2008 R2|  
|Basato su x64|Opzione di installazione dei componenti di base del server di Windows Server 2008 R2|  
|Basato su x64|Server Core e opzioni di installazione completa di Windows Server 2012|  
  
> [!NOTE]
> - Le versioni dei sistemi operativi elencate nella tabella precedente corrispondono alle combinazioni meno recenti di sistemi operativi e Service Pack supportate.  
>   -   Le edizioni Foundation, standard, Enterprise e Datacenter del sistema operativo Windows Server sono supportate come server di origine o di destinazione.  
>   -   Sono supportate le migrazioni tra sistemi operativi fisici e sistemi operativi virtuali.  
  
### <a name="supported-ad-fs-role-services-and-features"></a>Funzionalità e servizi ruolo di AD FS supportati  
 Nella tabella seguente vengono descritti gli scenari di migrazione dei servizi ruolo AD FS e le rispettive impostazioni descritte in questa guida.  
  
|Da|Per AD FS installato con Windows Server 2012|  
|----------|-----|  
|AD FS 1,0 server federativo installato con Windows Server 2003 R2|La migrazione non è supportata|  
|AD FS 1,0 proxy server federativo installato con Windows Server 2003 R2|La migrazione non è supportata|  
|AD FS 1,0 agente basato su token Windows installato con Windows Server 2003 R2|La migrazione non è supportata|  
|AD FS 1,0 agente in grado di riconoscere attestazioni installato con Windows Server 2003 R2)|La migrazione non è supportata|  
|AD FS 1,1 server federativo installato con Windows Server 2008 o Windows Server 2008 R2|La migrazione non è supportata|  
|AD FS 1,1 proxy server federativo installato con Windows Server 2008 o Windows Server 2008 R2|La migrazione non è supportata|  
|AD FS 1,1 agente basato su token Windows installato con Windows Server 2008 o Windows Server 2008 R2|È supportata la migrazione nello stesso server, ma l'agente basato su token Windows AD FS migrato funzionerà solo con un servizio federativo AD FS 1,1 installato con Windows Server 2008 o Windows Server 2008 R2. Per altre informazioni, vedere:<br /><br /> [Eseguire la migrazione di Agenti Web di AD FS 1.1](migrate-the-ad-fs-web-agent.md)<br /><br /> [Interazione con AD FS 1.x](Interoperating-with-AD-FS-1.x.md)|  
|AD FS 1,1 agente in grado di riconoscere attestazioni installato con Windows Server 2008 o Windows Server 2008 R2)|È supportata la migrazione nello stesso server. L'agente Web in grado di riconoscere attestazioni AD FS 1,1 migrato funzionerà con quanto segue:<br /><br /> AD FS 1,1 servizio federativo installato con Windows Server 2008 o Windows Server 2008 R2<br /><br /> AD FS 2,0 servizio federativo installato in Windows Server 2008 o Windows Server 2008 R2<br /><br /> AD FS servizio federativo installato con Windows Server 2012<br /><br /> Per altre informazioni, vedere:<br /><br /> [Eseguire la migrazione di Agenti Web di AD FS 1.1](migrate-the-ad-fs-web-agent.md)<br /><br /> [Interazione con AD FS 1.x](Interoperating-with-AD-FS-1.x.md)|  
|AD FS 2,0 server federativo installato in Windows Server 2008 o Windows Server 2008 R2|È supportata la migrazione nello stesso server. Per altre informazioni, vedere:<br /><br /> [Preparare la migrazione del server federativo di AD FS 2.0](prepare-to-migrate-ad-fs-fed-server.md)<br /><br /> [Eseguire la migrazione del server federativo di AD FS 2.0](migrate-the-ad-fs-fed-server.md)|  
|AD FS 2,0 proxy server federativo installato in Windows Server 2008 o Windows Server 2008 R2|È supportata la migrazione nello stesso server.  Per altre informazioni, vedi:<br /><br /> [Preparare la migrazione del proxy server federativo di AD FS 2.0](prepare-to-migrate-ad-fs-fed-proxy.md)<br /><br /> [Eseguire la migrazione del proxy server federativo di AD FS 2.0](migrate-the-ad-fs-2-fed-server-proxy.md)|  
  
## <a name="see-also"></a>Vedi anche  
 [Preparare la migrazione del server federativo AD FS 2,0](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparare la migrazione del proxy server federativo AD FS 2,0](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Eseguire la migrazione del server federativo AD FS 2,0](migrate-the-ad-fs-fed-server.md)   
 [Eseguire la migrazione del proxy server federativo AD FS 2,0](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Eseguire la migrazione di Agenti Web di AD FS 1.1](migrate-the-ad-fs-web-agent.md)