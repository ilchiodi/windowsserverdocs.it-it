---
title: Eseguire la migrazione di ruolo di Active Directory Federation Services a Windows Server 2012 R2
description: Fornisce istruzioni per la migrazione del servizio ADFS a Windows Server 2012 R2.
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 478729a7b6560beba5f04a1a15ad035561ad31f0
ms.sourcegitcommit: db290fa07e9d50686667bfba3969e20377548504
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 12/12/2017
---
# <a name="migrate-active-directory-federation-services-role-services-to-windows-server-2012-r2"></a>Eseguire la migrazione di ruolo di Active Directory Federation Services a Windows Server 2012 R2
 Questo documento fornisce istruzioni per eseguire la migrazione di servizi ruolo seguenti in Active Directory Federation Services (ADFS) installata con Windows Server 2012 R2:  
  
-   Server federativo di AD FS 2.0 installato in Windows Server 2008 o Windows Server 2008 R2  
  
-   Server federativo di ADFS installato in Windows Server 2012  
  
## <a name="supported-migration-scenarios"></a>Scenari di migrazione supportati  
 Le istruzioni di migrazione in questa guida sono costituiti da quanto segue:  
  
-   Esportazione dei dati di configurazione 2.0 AD FS dal server che esegue Windows Server 2008, Windows Server 2008 R2 o Windows Server 2012  
  
-   Eseguire un aggiornamento sul posto del sistema operativo del server da Windows Server 2008, Windows Server 2008 R2 o Windows Server 2012 a Windows Server 2012 R2. 
  
-   Ricreazione della configurazione originale di ADFS e ripristino di AD FS rimanenti impostazioni del servizio nel server, che ora esegue il ruolo del server ADFS installato con Windows Server 2012 R2.  
  
 Questa Guida non include istruzioni per la migrazione di un server che esegue più ruoli. Se il server esegue più ruoli, è consigliabile progettare un processo di migrazione personalizzata specificato del proprio ambiente server, in base alle informazioni disponibili in altre guide alla migrazione di ruoli. Guide alla migrazione per ruoli aggiuntivi sono disponibili sul [portale sulla migrazione di Windows Server](https://go.microsoft.com/fwlink/?LinkId=247608).  
  
### <a name="supported-operating-systems"></a>Sistemi operativi supportati  
 Sistema operativo server di destinazione:  
  
 Windows Server 2012 R2 (Server Core e opzioni di installazione completa)  
  
 Processore del server di destinazione:  
  
 basato su x64  
  
|Processore del server di origine|Sistema operativo del server di origine|  
|-----------------------------|------------------------------------|  
|x86-o x64-basato su| Windows Server 2008, completa e opzioni di installazione Server Core|  
|basato su x64|Windows Server 2008 R2|  
|basato su x64|Opzione di installazione Server Core di Windows Server 2008 R2|  
|basato su x64|Server Core e opzioni di installazione completa di Windows Server 2012|  
  
> [!NOTE]
>  -   Le versioni dei sistemi operativi elencati nella tabella precedente corrispondono alle combinazioni meno recenti di sistemi operativi e service Pack supportati.  
> -   Le edizioni Foundation, Standard, Enterprise e Datacenter del sistema operativo Windows Server sono supportate come origine o come server di destinazione.  
> -   Sono supportate le migrazioni tra sistemi operativi fisici e sistemi operativi virtuali.  
  
### <a name="supported-ad-fs-role-services-and-features"></a>Servizi ruolo ADFS e le funzionalità supportate  
 Nella tabella seguente vengono descritti gli scenari di migrazione dei servizi ruolo ADFS e delle rispettive impostazioni descritte in questa Guida.  
  
|Da|Ad ADFS installato con Windows Server 2012 R2|  
|----------|----------------------------------------------------------------------------------------------|  
|Server federativo di AD FS 2.0 installato in Windows Server 2008 o Windows Server 2008 R2|È supportata la migrazione nello stesso server. Per ulteriori informazioni, vedere:<br /><br /> [Preparazione alla migrazione del Server federativo ADFS](prepare-migrate-ad-fs-server-r2.md)<br /><br /> [La migrazione del Server federativo ADFS](migrate-ad-fs-fed-server-r2.md)|  
|Server federativo di ADFS installato in Windows Server 2012|È supportata la migrazione nello stesso server.  Per ulteriori informazioni, vedere:<br /><br /> [Preparazione alla migrazione del Server federativo ADFS](prepare-migrate-ad-fs-server-r2.md)<br /><br /> [La migrazione del Server federativo ADFS](migrate-ad-fs-fed-server-r2.md)|  
  
## <a name="next-steps"></a>Passaggi successivi
 [Preparazione alla migrazione del Server federativo ADFS](prepare-migrate-ad-fs-server-r2.md)   
 [La migrazione del Server federativo ADFS](migrate-ad-fs-fed-server-r2.md)   
 [La migrazione di Proxy Server federativo AD FS](migrate-fed-server-proxy-r2.md)   
 [Verifica della migrazione di ADFS a Windows Server 2012 R2](verify-ad-fs-migration.md)