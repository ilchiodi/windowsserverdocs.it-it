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
ms.sourcegitcommit: 07ac08dea2b8f2763c2614a999dc7967018aa0b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/07/2018
ms.locfileid: "6121360"
---
# Migrazione dei ruoli e delle funzionalità in Windows Server

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questa pagina contiene collegamenti a informazioni e strumenti che consentono di agevolare il processo di migrazione di ruoli e funzionalità a Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012. Molti ruoli e funzionalità possono essere eseguiti usando Strumenti di migrazione per Windows Server, un set di cinque cmdlet di Windows PowerShell che è stato introdotto in Windows Server 2008 R2 per semplificare la migrazione di ruoli, funzionalità e dati.

Le guide di migrazione supportano le migrazioni di ruoli e funzionalità specificati da un server all'altro (non aggiornamenti sul posto). Se non diversamente specificato nelle guide, sono supportate le migrazioni tra computer fisici e virtuali e tra le opzioni di installazione completa di Windows Server e i server che eseguono l'opzione di installazione Server Core.  

## Prima di iniziare

Prima di iniziare la migrazione dei ruoli e delle funzionalità, verificare che il server di origine e quello di destinazione eseguano entrambi il service pack più recente disponibile per i rispettivi sistemi operativi.
È ora disponibile un e-book delle guide alla migrazione di Windows Server 2012 R2 e Windows Server 2012. Per ulteriori informazioni e per scaricare l'e-book, vedere [E-Book Gallery for Microsoft Technologies](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#MigrateRoles) (Raccolta e-book per le tecnologie Microsoft). 

>[!NOTE]
>Ogni volta che si esegue la migrazione o l'aggiornamento a una qualsiasi versione di Windows Server, è necessario rivedere e comprendere i [criteri relativi al ciclo di vita del supporto](https://support.microsoft.com/lifecycle) e l'intervallo di tempo per tale versione e pianificare di conseguenza. È possibile [cercare le informazioni sul ciclo di vita](https://support.microsoft.com/lifecycle) per la versione specifica di Windows Server a cui si è interessati.
 
## WindowsServer 2016

### Guide alla migrazione
Le guide aggiornate alla migrazione per Windows Server 2016 sono in fase di sviluppo. Ricontrolla in questa posizione per gli aggiornamenti non appena diventano disponibili. In molti casi, i passaggi nelle guide alla migrazione di Windows Server 2012 R2 sono pertinenti anche per Windows Server 2016.

- [Servizi Desktop remoto](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/migrate-rds-role-services)
- [Server Web (IIS)](https://www.iis.net/downloads/microsoft/web-deploy)
- [Windows Server Update Services](https://technet.microsoft.com/library/hh852339.aspx)
- [MultiPoint Services](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/multipoint-services/multipoint-services-migrate)
 
## Windows Server 2012 R2

### Guide alla migrazione
Seguire i passaggi descritti in queste guide per eseguire la migrazione di ruoli e funzionalità dai server che eseguono Windows Server 2003, Windows Server 2008, Windows Server 2008 R2, Windows Server 2012 o Windows Server 2012 R2 a Windows Server 2012 R2. Strumenti di migrazione per Windows Server in Windows Server 2012 R2 supporta migrazioni tra subnet.

- [Installare, utilizzare e rimuovere Strumenti di migrazione per Windows Server](https://technet.microsoft.com/library/jj134202.aspx)
- [Guida alla migrazione di Servizi certificati Active Directory per Windows Server 2012 R2](https://technet.microsoft.com/library/dn486797.aspx)
- [Migrazione del servizio ruolo Active Directory Federation Services a Windows Server 2012 R2](https://technet.microsoft.com/library/dn486815.aspx)
- [Guida di aggiornamento e migrazione di Active Directory Rights Management Services](https://technet.microsoft.com/library/cc754277.aspx)
- [Eseguire la migrazione di Servizi file e archiviazione a Windows Server 2012 R2](https://technet.microsoft.com/library/dn479292.aspx)
- [Eseguire la migrazione di Hyper-V da Windows Server 2012 a Windows Server 2012 R2](https://technet.microsoft.com/library/dn486799.aspx)
- [Eseguire la migrazione di Server dei criteri di rete a Windows Server 2012](https://technet.microsoft.com/library/hh831652)
- [Eseguire la migrazione di Servizi Desktop remoto a Windows Server 2012 R2](https://technet.microsoft.com/library/dn479239.aspx)
- [Eseguire la migrazione di Windows Server Update Services a Windows Server 2012 R2](https://technet.microsoft.com/library/hh852339.aspx)
- [Eseguire la migrazione di ruoli cluster a Windows Server 2012 R2](https://technet.microsoft.com/library/dn530779.aspx)
- [Eseguire la migrazione del server DHCP a Windows Server 2012 R2](https://technet.microsoft.com/library/dn495425.aspx)
 
## Windows Server 2012

### Guide alla migrazione
Seguire i passaggi descritti in queste guide per eseguire la migrazione di ruoli e funzionalità dai server che eseguono Windows Server 2003, Windows Server 2008, Windows Server 2008 R2 o Windows Server 2012 a Windows Server 2012. Strumenti di migrazione per Windows Server in Windows Server 2012 supporta migrazioni tra subnet.

- [Installare, utilizzare e rimuovere Strumenti di migrazione per Windows Server](https://technet.microsoft.com/library/jj134202)
- [Eseguire la migrazione dei servizi ruolo di Active Directory Federation Services a WindowsServer2012](https://technet.microsoft.com/library/jj647765)
- [Eseguire la migrazione di Autorità registrazione integrità a Windows Server 2012](https://technet.microsoft.com/library/hh831513)
- [Eseguire la migrazione di Hyper-V da Windows Server 2008 a Windows Server 2012 R2](https://technet.microsoft.com/library/jj574113)
- [Eseguire la migrazione della configurazione IP a Windows Server 2012](https://technet.microsoft.com/library/jj574133)
- [Eseguire la migrazione di Server dei criteri di rete a Windows Server 2012](https://technet.microsoft.com/library/hh831652)
- [Eseguire la migrazione di Servizi di stampa e digitalizzazione a Windows Server 2012](https://technet.microsoft.com/library/jj134150)
- [Eseguire la migrazione di Accesso remoto a Windows Server 2012](https://technet.microsoft.com/library/hh831423)
- [Eseguire la migrazione di Windows Server Update Services a Windows Server 2012](https://technet.microsoft.com/library/hh852339)
- [Aggiornare i controller di dominio di Active Directory a Windows Server 2012](https://technet.microsoft.com/library/hh994618.aspx)
- [Migrazione di servizi e applicazioni in cluster a Windows Server 2012](https://technet.microsoft.com/library/dn486790.aspx)
 

Per ulteriori risorse di migrazione, visitare [Eseguire la migrazione dei ruoli e delle funzionalità a Windows Server 2012](https://technet.microsoft.com/library/jj134039).

## Windows Server 2008 R2

### Guide alla migrazione
Seguire i passaggi descritti in queste guide per eseguire la migrazione di ruoli e funzionalità dai server che eseguono Windows Server 2003, Windows Server 2008 o Windows Server 2008 R2 a Windows Server 2008 R2. Strumenti di migrazione per Windows Server in Windows Server 2008 R2 non supporta migrazioni tra subnet.

- [Installazione, accesso e rimozione di Strumenti di migrazione per Windows Server](https://technet.microsoft.com/library/dd379545)
- [Guida alla migrazione di Servizi certificati Active Directory](https://technet.microsoft.com/library/ee126170)
- [Guida alla migrazione di Active Directory Domain Services e Server DNS](https://technet.microsoft.com/library/dd379558)
- [Guida alla migrazione di BranchCache](https://technet.microsoft.com/library/dd548365)
- [Guida alla migrazione di un server DHCP (Dynamic Host Configuration Protocol)](https://technet.microsoft.com/library/dd379535)
- [Guida alla migrazione di Servizi file](https://technet.microsoft.com/library/dd379487)
- [Guida alla migrazione dell'Autorità registrazione integrità](https://technet.microsoft.com/library/ee791829)
- [Guida alla migrazione di Hyper-V](https://technet.microsoft.com/library/ee849855)
- [Guida alla migrazione della configurazione IP](https://technet.microsoft.com/library/dd379537)
- [Guida alla migrazione di utenti e gruppi locali](https://technet.microsoft.com/library/dd379531)
- [Guida alla migrazione del Server dei criteri di rete](https://technet.microsoft.com/library/ee791849)
- [Guida alla migrazione dei servizi di stampa](https://technet.microsoft.com/library/dd379488)
- [Guida alla migrazione di Servizi Desktop remoto](https://technet.microsoft.com/library/ff849223)
- [Guida alla migrazione RRAS](https://technet.microsoft.com/library/ee822825)
- [Informazioni e attività comuni della migrazione di Windows Server](https://technet.microsoft.com/library/ff400258)
- [Guida alla migrazione di Windows Server Update Services 3.0 SP2](https://technet.microsoft.com/library/ee822826)
 
Per ulteriori risorse di migrazione, visitare [Migrate Roles and Features to Windows Server 2008 R2](https://technet.microsoft.com/library/dd365353) (Eseguire la migrazione dei ruoli e delle funzionalità a Windows Server 2008 R2).
