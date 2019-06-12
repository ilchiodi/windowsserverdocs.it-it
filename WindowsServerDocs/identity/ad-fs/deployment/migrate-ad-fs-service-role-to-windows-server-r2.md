---
title: Eseguire la migrazione di servizi ruolo di Active Directory Federation Services a Windows Server 2012 R2
description: Vengono fornite istruzioni per la migrazione del servizio AD FS in Windows Server 2012 R2.
author: billmath
ms.author: billmath
manager: femila
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adfs
ms.openlocfilehash: 34eab7d5e0325a4ad8268a900738eea30b944ebb
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66444564"
---
# <a name="migrate-active-directory-federation-services-role-services-to-windows-server-2012-r2"></a>Eseguire la migrazione di servizi ruolo di Active Directory Federation Services a Windows Server 2012 R2
 Questo documento vengono fornite istruzioni per eseguire la migrazione i seguenti servizi ruolo di Active Directory Federation Services (ADFS) installata con Windows Server 2012 R2:  
  
-   Server federativo di AD ADFS 2.0 installato in Windows Server 2008 o Windows Server 2008 R2  
  
-   Server federativo ADFS installato in Windows Server 2012  
  
## <a name="supported-migration-scenarios"></a>Scenari di migrazione supportati  
 Le istruzioni per la migrazione in questa guida includono le attività seguenti:  
  
- Esportazione dei dati di configurazione 2.0 AD FS dal server che esegue Windows Server 2008, Windows Server 2008 R2 o Windows Server 2012  
  
- Eseguire un aggiornamento sul posto del sistema operativo del server da Windows Server 2008, Windows Server 2008 R2 o Windows Server 2012 a Windows Server 2012 R2. 
  
- Ricreare la configurazione originale di ADFS e il ripristino di AD FS rimanenti del servizio delle impostazioni nel server che esegue il ruolo server ADFS installato con Windows Server 2012 R2.  
  
  Questa guida non include istruzioni per la migrazione di un server che esegue più ruoli. Se nel server vengono eseguiti più ruoli, è consigliabile progettare un processo di migrazione personalizzato specifico del proprio ambiente server, basandosi sulle informazioni disponibili in altre guide alla migrazione dei ruoli. Le guide alla migrazione per altri ruoli sono disponibili nel [portale della migrazione per Windows Server](https://go.microsoft.com/fwlink/?LinkId=247608).  
  
### <a name="supported-operating-systems"></a>Sistemi operativi supportati  
 Sistema operativo server di destinazione:  
  
 Windows Server 2012 R2 (Server Core e le opzioni di installazione completa)  
  
 Processore del server di destinazione:  
  
 Basato su x64  
  
|Processore del server di origine|Sistema operativo del server di origine|  
|-----------------------------|------------------------------------|  
|Basato su x86 o su x64| Windows Server 2008, completa e opzioni di installazione Server Core|  
|Basato su x64|Windows Server 2008 R2|  
|Basato su x64|Opzione di installazione Server Core di Windows Server 2008 R2|  
|Basato su x64|Server Core e opzioni di installazione completa di Windows Server 2012|  
  
> [!NOTE]
> - Le versioni dei sistemi operativi elencate nella tabella precedente corrispondono alle combinazioni meno recenti di sistemi operativi e Service Pack supportate.  
>   -   Le edizioni Foundation, Standard, Enterprise e Datacenter del sistema operativo Windows Server sono supportate come origine o il server di destinazione.  
>   -   Sono supportate le migrazioni tra sistemi operativi fisici e sistemi operativi virtuali.  
  
### <a name="supported-ad-fs-role-services-and-features"></a>Funzionalità e servizi ruolo ADFS supportate  
 Nella tabella seguente vengono descritti gli scenari di migrazione dei servizi ruolo ADFS e delle rispettive impostazioni descritte in questa Guida.  
  
|Da|Ad ADFS installato con Windows Server 2012 R2|  
|----------|----------------------------------------------------------------------------------------------|  
|Server federativo di AD ADFS 2.0 installato in Windows Server 2008 o Windows Server 2008 R2|È supportata la migrazione nello stesso server. Per altre informazioni, vedi:<br /><br /> [Preparazione alla migrazione del Server federativo ADFS](prepare-migrate-ad-fs-server-r2.md)<br /><br /> [Migrazione del Server federativo di AD FS](migrate-ad-fs-fed-server-r2.md)|  
|Server federativo ADFS installato in Windows Server 2012|È supportata la migrazione nello stesso server.  Per altre informazioni, vedi:<br /><br /> [Preparazione alla migrazione del Server federativo ADFS](prepare-migrate-ad-fs-server-r2.md)<br /><br /> [Migrazione del Server federativo di AD FS](migrate-ad-fs-fed-server-r2.md)|  
  
## <a name="next-steps"></a>Passaggi successivi
 [Preparazione alla migrazione del Server federativo ADFS](prepare-migrate-ad-fs-server-r2.md)   
 [Migrazione del Server federativo di AD FS](migrate-ad-fs-fed-server-r2.md)   
 [Eseguire la migrazione del Proxy Server federativo di AD FS](migrate-fed-server-proxy-r2.md)   
 [Verifica della migrazione di ADFS a Windows Server 2012 R2](verify-ad-fs-migration.md)