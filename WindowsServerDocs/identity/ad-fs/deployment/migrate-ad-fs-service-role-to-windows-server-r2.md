---
title: Eseguire la migrazione di servizi ruolo di Active Directory Federation Services a Windows Server 2012 R2
description: Vengono fornite istruzioni per la migrazione del servizio AD FS a Windows Server 2012 R2.
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adfs
ms.openlocfilehash: 6c2f9c8079eb2dfaf208c8835940351a925d0a16
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80857504"
---
# <a name="migrate-active-directory-federation-services-role-services-to-windows-server-2012-r2"></a>Eseguire la migrazione di servizi ruolo di Active Directory Federation Services a Windows Server 2012 R2
 In questo documento vengono fornite le istruzioni per eseguire la migrazione dei servizi ruolo seguenti a Active Directory Federation Services (AD FS) installato con Windows Server 2012 R2:  
  
-   AD FS 2,0 server federativo installato in Windows Server 2008 o Windows Server 2008 R2  
  
-   AD FS server federativo installato in Windows Server 2012  
  
## <a name="supported-migration-scenarios"></a>Scenari di migrazione supportati  
 Le istruzioni per la migrazione in questa guida includono le attività seguenti:  
  
- Esportazione dei dati di configurazione di AD FS 2,0 dal server che esegue Windows Server 2008, Windows Server 2008 R2 o Windows Server 2012  
  
- Esecuzione di un aggiornamento sul posto del sistema operativo del server da Windows Server 2008, Windows Server 2008 R2 o Windows Server 2012 a Windows Server 2012 R2. 
  
- Ricreare la configurazione originale del AD FS e ripristinare le impostazioni del servizio AD FS rimanenti su questo server, che ora esegue il ruolo del server AD FS installato con Windows Server 2012 R2.  
  
  Questa guida non include istruzioni per la migrazione di un server che esegue più ruoli. Se nel server vengono eseguiti più ruoli, è consigliabile progettare un processo di migrazione personalizzato specifico del proprio ambiente server, basandosi sulle informazioni disponibili in altre guide alla migrazione dei ruoli. Le guide alla migrazione per altri ruoli sono disponibili nel [portale della migrazione per Windows Server](https://go.microsoft.com/fwlink/?LinkId=247608).  
  
### <a name="supported-operating-systems"></a>Sistemi operativi supportati  
 Sistema operativo del server di destinazione:  
  
 Windows Server 2012 R2 (opzioni di base del server e di installazione completa)  
  
 Processore del server di destinazione:  
  
 Basato su x64  
  
|Processore del server di origine|Sistema operativo del server di origine|  
|-----------------------------|------------------------------------|  
|Basato su x86 o su x64| Windows Server 2008, opzioni di installazione completa e Server Core|  
|Basato su x64|Windows Server 2008 R2|  
|Basato su x64|Opzione di installazione dei componenti di base del server di Windows Server 2008 R2|  
|Basato su x64|Server Core e opzioni di installazione completa di Windows Server 2012|  
  
> [!NOTE]
> - Le versioni dei sistemi operativi elencate nella tabella precedente corrispondono alle combinazioni meno recenti di sistemi operativi e Service Pack supportate.  
>   -   Le edizioni Foundation, standard, Enterprise e Datacenter del sistema operativo Windows Server sono supportate come server di origine o di destinazione.  
>   -   Sono supportate le migrazioni tra sistemi operativi fisici e sistemi operativi virtuali.  
  
### <a name="supported-ad-fs-role-services-and-features"></a>Funzionalità e servizi ruolo di AD FS supportati  
 Nella tabella seguente vengono descritti gli scenari di migrazione dei servizi ruolo AD FS e le rispettive impostazioni descritte in questa guida.  
  
|Da|Per AD FS installato con Windows Server 2012 R2|  
|----------|----------------------------------------------------------------------------------------------|  
|AD FS 2,0 server federativo installato in Windows Server 2008 o Windows Server 2008 R2|È supportata la migrazione nello stesso server. Per altre informazioni, vedi:<p> [Preparazione alla migrazione del server federativo di AD FS](prepare-migrate-ad-fs-server-r2.md)<p> [Migrazione del server federativo AD FS](migrate-ad-fs-fed-server-r2.md)|  
|AD FS server federativo installato in Windows Server 2012|È supportata la migrazione nello stesso server.  Per ulteriori informazioni, vedere:<p> [Preparazione alla migrazione del server federativo di AD FS](prepare-migrate-ad-fs-server-r2.md)<p> [Migrazione del server federativo AD FS](migrate-ad-fs-fed-server-r2.md)|  
  
## <a name="next-steps"></a>Passaggi successivi
 [Preparazione alla migrazione del server federativo di AD FS](prepare-migrate-ad-fs-server-r2.md)   
 [Migrazione del server federativo AD FS](migrate-ad-fs-fed-server-r2.md)   
 [Migrazione del proxy server federativo di AD FS](migrate-fed-server-proxy-r2.md)   
 [Verifica della migrazione del AD FS a Windows Server 2012 R2](verify-ad-fs-migration.md)