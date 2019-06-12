---
title: Eseguire la migrazione dei servizi ruolo di Active Directory Federation Services a Windows Server 2012
description: Vengono fornite istruzioni per la migrazione del servizio AD FS in Windows Server 2012.
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 63dc9825b9c9b8a06d0946859e08a536589eb044
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66445706"
---
# <a name="migrate-active-directory-federation-services-role-services-to-windows-server-2012"></a>Eseguire la migrazione dei servizi ruolo di Active Directory Federation Services a Windows Server 2012

Di seguito vengono fornite istruzioni sulla migrazione di servizi ruolo seguenti in Active Directory Federation Services (ADFS) in Windows Server 2012:  
  
-   Agente basato su token di AD FS 1.1 Windows e AD agente in grado di riconoscere attestazioni ADFS 1.1 installato con Windows Server 2008 o Windows Server 2008 R2  
  
-   Server federativo ADFS 2.0 e ADFS proxy server federativo 2.0 installato in Windows Server 2008 o Windows Server 2008 R2    
  
## <a name="supported-migration-scenarios"></a>Scenari di migrazione supportati  
 Le istruzioni di migrazione contiene le attività seguenti:  
  
- Esportazione dei dati di configurazione 2.0 AD FS dal server che esegue Windows Server 2008 o Windows Server 2008 R2  
  
- Eseguire un aggiornamento sul posto del sistema operativo del server da Windows Server 2008 o Windows Server 2008 R2 a Windows Server 2012.
  
- Ricreare la configurazione originale di ADFS e il ripristino di AD FS rimanenti del servizio delle impostazioni nel server che esegue il ruolo server ADFS installato con Windows Server 2012.  
  
  Questa guida non include istruzioni per la migrazione di un server che esegue più ruoli. Se nel server vengono eseguiti più ruoli, è consigliabile progettare un processo di migrazione personalizzato specifico del proprio ambiente server, basandosi sulle informazioni disponibili in altre guide alla migrazione dei ruoli. Le guide alla migrazione per altri ruoli sono disponibili nel [portale della migrazione per Windows Server](https://go.microsoft.com/fwlink/?LinkId=247608).  
  
## <a name="supported-operating-systems"></a>Sistemi operativi supportati  
 **Sistema operativo server di destinazione:**  
  

- Windows Server 2012 o Windows Server 2008 R2 (Server Core e le opzioni di installazione completa)  
  
  **Processore del server di destinazione:**  
  

- Basato su x64  
  
|Processore del server di origine|Sistema operativo del server di origine|  
|-----|-----|  
|Basato su x86 o su x64|Windows Server 2003 con Service Pack 2|  
|Basato su x86 o su x64|Windows Server 2003 R2|  
|Basato su x86 o su x64|Windows Server 2008, completa e opzioni di installazione Server Core|  
|Basato su x64|Windows Server 2008 R2|  
|Basato su x64|Opzione di installazione Server Core di Windows Server 2008 R2|  
|Basato su x64|Server Core e opzioni di installazione completa di Windows Server 2012|  
  
> [!NOTE]
> - Le versioni dei sistemi operativi elencate nella tabella precedente corrispondono alle combinazioni meno recenti di sistemi operativi e Service Pack supportate.  
>   -   Le edizioni Foundation, Standard, Enterprise e Datacenter del sistema operativo Windows Server sono supportate come origine o il server di destinazione.  
>   -   Sono supportate le migrazioni tra sistemi operativi fisici e sistemi operativi virtuali.  
  
### <a name="supported-ad-fs-role-services-and-features"></a>Funzionalità e servizi ruolo ADFS supportate  
 Nella tabella seguente vengono descritti gli scenari di migrazione dei servizi ruolo ADFS e delle rispettive impostazioni descritte in questa Guida.  
  
|Da|Ad ADFS installato con Windows Server 2012|  
|----------|-----|  
|Server federativo di AD FS 1.0 installato in Windows Server 2003 R2|La migrazione non è supportata|  
|Proxy server federativo ADFS 1.0 AD installato con Windows Server 2003 R2|La migrazione non è supportata|  
|AD FS 1.0 Windows basata su token dell'agente installato con Windows Server 2003 R2|La migrazione non è supportata|  
|AD FS 1.0 grado di riconoscere attestazioni agente installato con Windows Server 2003 R2)|La migrazione non è supportata|  
|Server federativo di AD FS 1.1 installato con Windows Server 2008 o Windows Server 2008 R2|La migrazione non è supportata|  
|Proxy server federativo ADFS 1.1 AD installato con Windows Server 2008 o Windows Server 2008 R2|La migrazione non è supportata|  
|AD FS 1.1 Windows basata su token dell'agente installato con Windows Server 2008 o Windows Server 2008 R2|È supportata la migrazione nello stesso server, ma l'agente basato su token AD FS Windows migrato funzionerà solo con un servizio di AD FS federation 1.1 installato con Windows Server 2008 o Windows Server 2008 R2. Per altre informazioni, vedi:<br /><br /> [Eseguire la migrazione di Agenti Web di AD FS 1.1](migrate-the-ad-fs-web-agent.md)<br /><br /> [Interazione con AD FS 1.x](Interoperating-with-AD-FS-1.x.md)|  
|Grado di riconoscere attestazioni agente di ADFS 1.1 installato con Windows Server 2008 o Windows Server 2008 R2)|È supportata la migrazione nello stesso server. L'agente di 1.1 web in grado di riconoscere attestazioni AD FS migrato funzionerà con gli elementi seguenti:<br /><br /> Servizio federativo AD FS 1.1 installato con Windows Server 2008 o Windows Server 2008 R2<br /><br /> Servizio di AD FS federation 2.0 installato in Windows Server 2008 o Windows Server 2008 R2<br /><br /> Servizio federativo AD FS installato con Windows Server 2012<br /><br /> Per altre informazioni, vedi:<br /><br /> [Eseguire la migrazione di Agenti Web di AD FS 1.1](migrate-the-ad-fs-web-agent.md)<br /><br /> [Interazione con AD FS 1.x](Interoperating-with-AD-FS-1.x.md)|  
|Server federativo di AD ADFS 2.0 installato in Windows Server 2008 o Windows Server 2008 R2|È supportata la migrazione nello stesso server. Per altre informazioni, vedi:<br /><br /> [Preparare la migrazione del server federativo di AD FS 2.0](prepare-to-migrate-ad-fs-fed-server.md)<br /><br /> [Eseguire la migrazione del server federativo di AD FS 2.0](migrate-the-ad-fs-fed-server.md)|  
|Proxy server federativo ADFS 2.0 AD installato in Windows Server 2008 o Windows Server 2008 R2|È supportata la migrazione nello stesso server.  Per altre informazioni, vedi:<br /><br /> [Preparare la migrazione del proxy server federativo di AD FS 2.0](prepare-to-migrate-ad-fs-fed-proxy.md)<br /><br /> [Eseguire la migrazione del proxy server federativo di AD FS 2.0](migrate-the-ad-fs-2-fed-server-proxy.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Preparare la migrazione di AD Server federativo ADFS 2.0](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparare la migrazione del Proxy Server 2.0 Federation di AD FS](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Eseguire la migrazione di AD Server federativo ADFS 2.0](migrate-the-ad-fs-fed-server.md)   
 [Eseguire la migrazione del Proxy Server 2.0 Federation di AD FS](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Eseguire la migrazione di Agenti Web di AD FS 1.1](migrate-the-ad-fs-web-agent.md)