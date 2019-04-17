---
title: Eseguire la migrazione di ruolo di Active Directory Federation Services a Windows Server 2012
description: Fornisce istruzioni per la migrazione del servizio ADFS a Windows Server 2012.
author: billmath
ms.author: billmath
manager: femila
ms.date: 06/28/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 2b44ed504c2b3dc8a8ac0ca9648f1db9e362e075
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="migrate-active-directory-federation-services-role-services-to-windows-server-2012"></a>Eseguire la migrazione di ruolo di Active Directory Federation Services a Windows Server 2012

Di seguito vengono fornite istruzioni sulla migrazione di servizi ruolo seguenti in Active Directory Federation Services (ADFS) in Windows Server 2012:  
  
-   Agente basato su token Windows ADFS 1.1 Active Directory e AD agente in grado di riconoscere attestazioni ADFS 1.1 installato con Windows Server 2008 o Windows Server 2008 R2  
  
-   Server federativo ADFS 2.0 e AD FS proxy server federativo 2.0 installato in Windows Server 2008 o Windows Server 2008 R2    
  
## <a name="supported-migration-scenarios"></a>Scenari di migrazione supportati  
 Le istruzioni di migrazione contiene le attività seguenti:  
  
-   Esportazione dei dati di configurazione 2.0 AD FS dal server che esegue Windows Server 2008 o Windows Server 2008 R2  
  
-   Eseguire un aggiornamento sul posto del sistema operativo del server da Windows Server 2008 o Windows Server 2008 R2 a Windows Server 2012.
  
-   Ricreazione della configurazione originale di ADFS e ripristino di AD FS rimanenti impostazioni del servizio nel server, che ora esegue il ruolo del server ADFS installato con Windows Server 2012.  
  
 Questa Guida non include istruzioni per la migrazione di un server che esegue più ruoli. Se il server esegue più ruoli, è consigliabile progettare un processo di migrazione personalizzata specificato del proprio ambiente server, in base alle informazioni disponibili in altre guide alla migrazione di ruoli. Guide alla migrazione per ruoli aggiuntivi sono disponibili sul [portale sulla migrazione di Windows Server](https://go.microsoft.com/fwlink/?LinkId=247608).  
  
## <a name="supported-operating-systems"></a>Sistemi operativi supportati  
 **Sistema operativo server di destinazione:**  
  

-  Windows Server 2012 o Windows Server 2008 R2 (Server Core e opzioni di installazione completa)  
  
 **Processore del server di destinazione:**  
  

-  basato su x64  
  
|Processore del server di origine|Sistema operativo del server di origine|  
|-----|-----|  
|x86-o x64-basato su|Windows Server 2003 con Service Pack 2|  
|x86-o x64-basato su|Windows Server 2003 R2|  
|x86-o x64-basato su|Windows Server 2008, completa e opzioni di installazione Server Core|  
|basato su x64|Windows Server 2008 R2|  
|basato su x64|Opzione di installazione Server Core di Windows Server 2008 R2|  
|basato su x64|Server Core e opzioni di installazione completa di Windows Server 2012|  
  
> [!NOTE]
>  -   Le versioni dei sistemi operativi elencati nella tabella precedente corrispondono alle combinazioni meno recenti di sistemi operativi e service Pack supportati.  
> -   Le edizioni Foundation, Standard, Enterprise e Datacenter del sistema operativo Windows Server sono supportate come origine o come server di destinazione.  
> -   Sono supportate le migrazioni tra sistemi operativi fisici e sistemi operativi virtuali.  
  
### <a name="supported-ad-fs-role-services-and-features"></a>Servizi ruolo ADFS e le funzionalità supportate  
 Nella tabella seguente vengono descritti gli scenari di migrazione dei servizi ruolo ADFS e delle rispettive impostazioni descritte in questa Guida.  
  
|Da|Ad ADFS installato con Windows Server 2012|  
|----------|-----|  
|Server federativo di AD ADFS 1.0 installato in Windows Server 2003 R2|Non è supportata la migrazione|  
|Proxy server federativo ADFS 1.0 AD installato con Windows Server 2003 R2|Non è supportata la migrazione|  
|Agente basato su token Windows ADFS 1.0 di Active Directory installati con Windows Server 2003 R2|Non è supportata la migrazione|  
|AD FS 1.0 grado di riconoscere attestazioni agente installato con Windows Server 2003 R2)|Non è supportata la migrazione|  
|Server federativo di AD FS 1.1 installato con Windows Server 2008 o Windows Server 2008 R2|Non è supportata la migrazione|  
|Proxy server federativo ADFS 1.1 AD installato con Windows Server 2008 o Windows Server 2008 R2|Non è supportata la migrazione|  
|Agente basato su token Windows ADFS 1.1 di Active Directory installati con Windows Server 2008 o Windows Server 2008 R2|È supportata la migrazione nello stesso server, ma l'agente basato su token Windows ADFS migrato funzionerà solo con un servizio federativo 1.1 ADFS installato con Windows Server 2008 o Windows Server 2008 R2. Per ulteriori informazioni, vedere:<br /><br /> [Eseguire la migrazione di Active Directory agenti Web ADFS 1.1](migrate-the-ad-fs-web-agent.md)<br /><br /> [Interoperabilità con AD FS 1. x](Interoperating-with-AD-FS-1.x.md)|  
|Attestazioni agente di ADFS 1.1 installato con Windows Server 2008 o Windows Server 2008 R2)|È supportata la migrazione nello stesso server. L'agente 1.1 riconoscere attestazioni AD FS migrato funzionerà con le operazioni seguenti:<br /><br /> Servizio federativo AD FS 1.1 installato con Windows Server 2008 o Windows Server 2008 R2<br /><br /> Servizio federativo 2.0 ADFS installato in Windows Server 2008 o Windows Server 2008 R2<br /><br /> Servizio federativo AD FS installato con Windows Server 2012<br /><br /> Per ulteriori informazioni, vedere:<br /><br /> [Eseguire la migrazione di Active Directory agenti Web ADFS 1.1](migrate-the-ad-fs-web-agent.md)<br /><br /> [Interoperabilità con AD FS 1. x](Interoperating-with-AD-FS-1.x.md)|  
|Server federativo di AD FS 2.0 installato in Windows Server 2008 o Windows Server 2008 R2|È supportata la migrazione nello stesso server. Per ulteriori informazioni, vedere:<br /><br /> [Preparare la migrazione di Active Directory Server federativo ADFS 2.0](prepare-to-migrate-ad-fs-fed-server.md)<br /><br /> [Eseguire la migrazione di Active Directory Server federativo ADFS 2.0](migrate-the-ad-fs-fed-server.md)|  
|Proxy server federativo ADFS 2.0 AD installato in Windows Server 2008 o Windows Server 2008 R2|È supportata la migrazione nello stesso server.  Per ulteriori informazioni, vedere:<br /><br /> [Preparare la migrazione di AD FS 2.0 Server federativo](prepare-to-migrate-ad-fs-fed-proxy.md)<br /><br /> [Eseguire la migrazione di AD FS 2.0 Server federativo](migrate-the-ad-fs-2-fed-server-proxy.md)|  
  
## <a name="see-also"></a>Vedere anche  
 [Preparare la migrazione di Active Directory Server federativo ADFS 2.0](prepare-to-migrate-ad-fs-fed-server.md)   
 [Preparare la migrazione di AD FS 2.0 Server federativo](prepare-to-migrate-ad-fs-fed-proxy.md)   
 [Eseguire la migrazione di Active Directory Server federativo ADFS 2.0](migrate-the-ad-fs-fed-server.md)   
 [Eseguire la migrazione di AD FS 2.0 Server federativo](migrate-the-ad-fs-2-fed-server-proxy.md)   
 [Eseguire la migrazione di Active Directory agenti Web ADFS 1.1](migrate-the-ad-fs-web-agent.md)