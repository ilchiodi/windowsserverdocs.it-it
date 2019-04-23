---
title: Migrazione di ruoli e funzionalità
description: ''
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.date: 04/03/2017
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 0f78ef4c-dd12-4b1b-8c6e-251dd803c5d1
author: jaimeo
ms.author: jaimeo
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 7469171005164d9ff823dad7de230d877c874dc9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59840882"
---
# <a name="migrating-roles-and-features-in-windows-server"></a>Migrazione dei ruoli e delle funzionalità in Windows Server

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questa pagina contiene collegamenti a informazioni e strumenti che consentono di agevolare il processo di migrazione di ruoli e funzionalità a Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012. Molti ruoli e funzionalità possono essere eseguiti usando Strumenti di migrazione per Windows Server, un set di cinque cmdlet di Windows PowerShell che è stato introdotto in Windows Server 2008 R2 per semplificare la migrazione di ruoli, funzionalità e dati.

Le guide di migrazione supportano le migrazioni di ruoli e funzionalità specificati da un server all'altro (non aggiornamenti sul posto). Se non diversamente specificato nelle guide, sono supportate le migrazioni tra computer fisici e virtuali e tra le opzioni di installazione completa di Windows Server e i server che eseguono l'opzione di installazione Server Core.  

## <a name="before-you-begin"></a>Prima di iniziare

Prima di iniziare la migrazione dei ruoli e delle funzionalità, verificare che il server di origine e quello di destinazione eseguano entrambi il service pack più recente disponibile per i rispettivi sistemi operativi.
È ora disponibile un e-book delle guide alla migrazione di Windows Server 2012 R2 e Windows Server 2012. Per ulteriori informazioni e per scaricare l'e-book, vedere [E-Book Gallery for Microsoft Technologies](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#MigrateRoles) (Raccolta e-book per le tecnologie Microsoft). 

>[!NOTE]
>Ogni volta che si esegue la migrazione o l'aggiornamento a una qualsiasi versione di Windows Server, è necessario rivedere e comprendere i [criteri relativi al ciclo di vita del supporto](https://support.microsoft.com/lifecycle) e l'intervallo di tempo per tale versione e pianificare di conseguenza. È possibile [cercare le informazioni sul ciclo di vita](https://support.microsoft.com/lifecycle) per la versione specifica di Windows Server a cui si è interessati.
 
## <a name="windows-server-2016"></a>Windows Server 2016

### <a name="migration-guides"></a>Guide alla migrazione
Le guide aggiornate alla migrazione per Windows Server 2016 sono in fase di sviluppo. Ricontrolla in questa posizione per gli aggiornamenti non appena diventano disponibili. In molti casi, i passaggi nelle guide alla migrazione di Windows Server 2012 R2 sono pertinenti anche per Windows Server 2016.

- [Servizi Desktop remoto](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/migrate-rds-role-services)
- [Web Server (IIS)](https://www.iis.net/downloads/microsoft/web-deploy)
- [Windows Server Update Services](https://technet.microsoft.com/library/hh852339.aspx)
- [MultiPoint Services](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/multipoint-services/multipoint-services-migrate)
 
## <a name="windows-server-2012-r2"></a>Windows Server 2012 R2

### <a name="migration-guides"></a>Guide alla migrazione
Seguire i passaggi descritti in queste guide per eseguire la migrazione di ruoli e funzionalità dai server che eseguono Windows Server 2003, Windows Server 2008, Windows Server 2008 R2, Windows Server 2012 o Windows Server 2012 R2 a Windows Server 2012 R2. Strumenti di migrazione per Windows Server in Windows Server 2012 R2 supporta migrazioni tra subnet.

- [Installazione, utilizzo e rimuovere strumenti di migrazione per Windows Server](https://technet.microsoft.com/library/jj134202.aspx)
- [Guida alla migrazione di servizi di Active Directory certificati per Windows Server 2012 R2](https://technet.microsoft.com/library/dn486797.aspx)
- [La migrazione di servizio di ruolo di Active Directory Federation Services a Windows Server 2012 R2](https://technet.microsoft.com/library/dn486815.aspx)
- [Guida all'aggiornamento e migrazione di servizi di Active Directory Rights Management](https://technet.microsoft.com/library/cc754277.aspx)
- [Eseguire la migrazione di servizi File e archiviazione a Windows Server 2012 R2](https://technet.microsoft.com/library/dn479292.aspx)
- [Eseguire la migrazione di Hyper-V a Windows Server 2012 R2 da Windows Server 2012](https://technet.microsoft.com/library/dn486799.aspx)
- [Eseguire la migrazione di Server dei criteri di rete a Windows Server 2012](https://technet.microsoft.com/library/hh831652)
- [Eseguire la migrazione di Servizi Desktop remoto a Windows Server 2012 R2](https://technet.microsoft.com/library/dn479239.aspx)
- [Eseguire la migrazione di Windows Server Update Services a Windows Server 2012 R2](https://technet.microsoft.com/library/hh852339.aspx)
- [Eseguire la migrazione di ruoli del Cluster a Windows Server 2012 R2](https://technet.microsoft.com/library/dn530779.aspx)
- [Eseguire la migrazione di Server DHCP a Windows Server 2012 R2](https://technet.microsoft.com/library/dn495425.aspx)
 
## <a name="windows-server-2012"></a>Windows Server 2012

### <a name="migration-guides"></a>Guide alla migrazione
Seguire i passaggi descritti in queste guide per eseguire la migrazione di ruoli e funzionalità dai server che eseguono Windows Server 2003, Windows Server 2008, Windows Server 2008 R2 o Windows Server 2012 a Windows Server 2012. Strumenti di migrazione per Windows Server in Windows Server 2012 supporta migrazioni tra subnet.

- [Installazione, utilizzo e rimuovere strumenti di migrazione per Windows Server](https://technet.microsoft.com/library/jj134202)
- [Eseguire la migrazione di servizi ruolo di Active Directory Federation Services a Windows Server 2012](https://technet.microsoft.com/library/jj647765)
- [Eseguire la migrazione di Autorità registrazione integrità a Windows Server 2012](https://technet.microsoft.com/library/hh831513)
- [Eseguire la migrazione di Hyper-V a Windows Server 2012 da Windows Server 2008 R2](https://technet.microsoft.com/library/jj574113)
- [Migrazione della configurazione IP a Windows Server 2012](https://technet.microsoft.com/library/jj574133)
- [Eseguire la migrazione di Server dei criteri di rete a Windows Server 2012](https://technet.microsoft.com/library/hh831652)
- [Eseguire la migrazione di stampa e digitalizzazione Services a Windows Server 2012](https://technet.microsoft.com/library/jj134150)
- [Eseguire la migrazione di accesso remoto a Windows Server 2012](https://technet.microsoft.com/library/hh831423)
- [Eseguire la migrazione di Windows Server Update Services a Windows Server 2012](https://technet.microsoft.com/library/hh852339)
- [Aggiornare controller di dominio Active Directory a Windows Server 2012](https://technet.microsoft.com/library/hh994618.aspx)
- [Migrazione di servizi del cluster e alle applicazioni di Windows Server 2012](https://technet.microsoft.com/library/dn486790.aspx)
 

Per ulteriori risorse di migrazione, visitare [Eseguire la migrazione dei ruoli e delle funzionalità a Windows Server 2012](https://technet.microsoft.com/library/jj134039).

## <a name="windows-server-2008-r2"></a>Windows Server 2008 R2

### <a name="migration-guides"></a>Guide alla migrazione
Seguire i passaggi descritti in queste guide per eseguire la migrazione di ruoli e funzionalità dai server che eseguono Windows Server 2003, Windows Server 2008 o Windows Server 2008 R2 a Windows Server 2008 R2. Strumenti di migrazione per Windows Server in Windows Server 2008 R2 non supporta migrazioni tra subnet.

- [Installazione, accesso e rimozione di strumenti di migrazione di Server Windows](https://technet.microsoft.com/library/dd379545)
- [Guida alla migrazione di servizi certificati Active Directory](https://technet.microsoft.com/library/ee126170)
- [Servizi di dominio Active Directory e Guida alla migrazione di Server di Domain Name System (DNS)](https://technet.microsoft.com/library/dd379558)
- [Guida alla migrazione di BranchCache](https://technet.microsoft.com/library/dd548365)
- [Guida alla migrazione di Dynamic Host Configuration Protocol (DHCP) Server](https://technet.microsoft.com/library/dd379535)
- [Guida alla migrazione di servizi file](https://technet.microsoft.com/library/dd379487)
- [Guida alla migrazione di Autorità registrazione integrità](https://technet.microsoft.com/library/ee791829)
- [Guida alla migrazione di Hyper-V](https://technet.microsoft.com/library/ee849855)
- [Guida IP Configuration Migration Guide](https://technet.microsoft.com/library/dd379537)
- [Guida alla migrazione di gruppo e utente locale](https://technet.microsoft.com/library/dd379531)
- [Guida alla migrazione](https://technet.microsoft.com/library/ee791849)
- [Guida alla migrazione di servizi di stampa](https://technet.microsoft.com/library/dd379488)
- [Guida alla migrazione di Servizi Desktop remoto](https://technet.microsoft.com/library/ff849223)
- [Guida alla migrazione RRAS](https://technet.microsoft.com/library/ee822825)
- [Informazioni e attività comuni di migrazione di Windows Server](https://technet.microsoft.com/library/ff400258)
- [Guida alla migrazione di Windows Server Update Services 3.0 SP2](https://technet.microsoft.com/library/ee822826)  
Per ulteriori risorse di migrazione, visitare [Migrate Roles and Features to Windows Server 2008 R2](https://technet.microsoft.com/library/dd365353) (Eseguire la migrazione dei ruoli e delle funzionalità a Windows Server 2008 R2).
