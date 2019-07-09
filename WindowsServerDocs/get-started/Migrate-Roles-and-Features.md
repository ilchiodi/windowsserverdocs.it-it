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
ms.openlocfilehash: 486c11ebd46c6fd23b3bd16cd90463f8d607287e
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/17/2019
ms.locfileid: "66443543"
---
# <a name="migrating-roles-and-features-in-windows-server"></a>Migrazione dei ruoli e delle funzionalità in Windows Server

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questa pagina contiene collegamenti a informazioni e strumenti che semplificano il processo di migrazione di ruoli e funzionalità a Windows Server 2016, Windows Server 2012 R2 e Windows Server 2012. Puoi eseguire la migrazione di molti ruoli e funzionalità tramite Strumenti di migrazione per Windows Server, un set di cinque cmdlet di Windows PowerShell introdotto in Windows Server 2008 R2 per semplificare la migrazione di ruoli, funzionalità e dati.

Le guide alla migrazione supportano le migrazioni di ruoli e funzionalità specificati da un server all'altro (non aggiornamenti sul posto). Se non diversamente specificato nelle guide, sono supportate le migrazioni tra computer fisici e virtuali e tra opzioni di installazione completa di Windows Server e server che eseguono l'opzione di installazione dei componenti di base del server.  

## <a name="before-you-begin"></a>Prima di iniziare

Prima di avviare la migrazione di ruoli e funzionalità, verifica che il server di origine e quello di destinazione eseguano entrambi i Service Pack più recenti disponibili per i rispettivi sistemi operativi.
È ora disponibile un e-book delle guide alla migrazione per Windows Server 2012 R2 e Windows Server 2012. Per altre informazioni e per scaricare l'e-book, vedi [Raccolta di e-book per le tecnologie Microsoft](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#MigrateRoles). 

>[!NOTE]
>Ogni volta che esegui la migrazione o l'aggiornamento a qualsiasi versione di Windows Server, devi esaminare e comprendere i [criteri relativi al ciclo di vita del supporto](https://support.microsoft.com/lifecycle) e l'intervallo di tempo per ogni versione, per poi pianificare il processo di conseguenza. Puoi [cercare le informazioni sul ciclo di vita](https://support.microsoft.com/lifecycle) per la versione specifica di Windows Server che ti interessa.
 
## <a name="windows-server-2016"></a>Windows Server 2016

### <a name="migration-guides"></a>Guide alla migrazione
Le guide aggiornate alla migrazione per Windows Server 2016 sono in fase di sviluppo. Torna a questa pagina per verificare la presenza di aggiornamenti non appena diventano disponibili. In molti casi, i passaggi nelle guide alla migrazione per Windows Server 2012 R2 sono pertinenti anche per Windows Server 2016.

- [Servizi Desktop remoto](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/migrate-rds-role-services)
- [Server Web (IIS)](https://www.iis.net/downloads/microsoft/web-deploy)
- [Windows Server Update Services](https://technet.microsoft.com/library/hh852339.aspx)
- [Servizi MultiPoint](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/multipoint-services/multipoint-services-migrate)
 
## <a name="windows-server-2012-r2"></a>Windows Server 2012 R2

### <a name="migration-guides"></a>Guide alla migrazione
Segui i passaggi descritti in queste guide per eseguire la migrazione di ruoli e funzionalità da server che eseguono Windows Server 2003, Windows Server 2008, Windows Server 2008 R2, Windows Server 2012 o Windows Server 2012 R2 a Windows Server 2012 R2. Strumenti di migrazione per Windows Server in Windows Server 2012 R2 supporta migrazioni tra subnet.

- [Installare, usare e rimuovere Strumenti di migrazione per Windows Server](https://technet.microsoft.com/library/jj134202.aspx)
- [Guida alla migrazione di Servizi certificati Active Directory per Windows Server 2012 R2](https://technet.microsoft.com/library/dn486797.aspx)
- [Migrazione del servizio ruolo Active Directory Federation Services a Windows Server 2012 R2](https://technet.microsoft.com/library/dn486815.aspx)
- [Guida all'aggiornamento e alla migrazione di Active Directory Rights Management Services](https://technet.microsoft.com/library/cc754277.aspx)
- [Eseguire la migrazione di servizi file e archiviazione a Windows Server 2012 R2](https://technet.microsoft.com/library/dn479292.aspx)
- [Eseguire la migrazione di Hyper-V a Windows Server 2012 R2 da Windows Server 2012](https://technet.microsoft.com/library/dn486799.aspx)
- [Eseguire la migrazione di Server dei criteri di rete a Windows Server 2012](https://technet.microsoft.com/library/hh831652)
- [Eseguire la migrazione di Servizi Desktop remoto a Windows Server 2012 R2](https://technet.microsoft.com/library/dn479239.aspx)
- [Eseguire la migrazione di Windows Server Update Services a Windows Server 2012 R2](https://technet.microsoft.com/library/hh852339.aspx)
- [Eseguire la migrazione di ruoli cluster a Windows Server 2012 R2](https://technet.microsoft.com/library/dn530779.aspx)
- [Eseguire la migrazione del server DHCP a Windows Server 2012 R2](https://technet.microsoft.com/library/dn495425.aspx)
 
## <a name="windows-server-2012"></a>Windows Server 2012

### <a name="migration-guides"></a>Guide alla migrazione
Segui i passaggi descritti in queste guide per eseguire la migrazione di ruoli e funzionalità da server che eseguono Windows Server 2003, Windows Server 2008, Windows Server 2008 R2 o Windows Server 2012 a Windows Server 2012. Strumenti di migrazione per Windows Server in Windows Server 2012 supporta migrazioni tra subnet.

- [Installare, usare e rimuovere Strumenti di migrazione per Windows Server](https://technet.microsoft.com/library/jj134202)
- [Eseguire la migrazione dei servizi ruolo di Active Directory Federation Services a Windows Server 2012](https://technet.microsoft.com/library/jj647765)
- [Eseguire la migrazione di Autorità registrazione integrità a Windows Server 2012](https://technet.microsoft.com/library/hh831513)
- [Eseguire la migrazione di Hyper-V da Windows Server 2008 R2 a Windows Server 2012](https://technet.microsoft.com/library/jj574113)
- [Eseguire la migrazione della configurazione IP a Windows Server 2012](https://technet.microsoft.com/library/jj574133)
- [Eseguire la migrazione di Server dei criteri di rete a Windows Server 2012](https://technet.microsoft.com/library/hh831652)
- [Eseguire la migrazione di Servizi di stampa e digitalizzazione a Windows Server 2012](https://technet.microsoft.com/library/jj134150)
- [Eseguire la migrazione di Accesso remoto a Windows Server 2012](https://technet.microsoft.com/library/hh831423)
- [Eseguire la migrazione di Windows Server Update Services a Windows Server 2012](https://technet.microsoft.com/library/hh852339)
- [Aggiornare i controller di dominio Active Directory a Windows Server 2012](https://technet.microsoft.com/library/hh994618.aspx)
- [Migrazione di servizi e applicazioni in cluster a Windows Server 2012](https://technet.microsoft.com/library/dn486790.aspx)
 

Per altre risorse sulla migrazione, visita [Eseguire la migrazione di ruoli e funzionalità a Windows Server 2012](https://technet.microsoft.com/library/jj134039).

## <a name="windows-server-2008-r2"></a>Windows Server 2008 R2

### <a name="migration-guides"></a>Guide alla migrazione
Segui i passaggi descritti in queste guide per eseguire la migrazione di ruoli e funzionalità da server che eseguono Windows Server 2003, Windows Server 2008 o Windows Server 2008 R2 a Windows Server 2008 R2. Strumenti di migrazione per Windows Server in Windows Server 2008 R2 non supporta migrazioni tra subnet.

- [Installazione, accesso e rimozione di Strumenti di migrazione per Windows Server](https://technet.microsoft.com/library/dd379545)
- [Guida alla migrazione di Servizi certificati Active Directory](https://technet.microsoft.com/library/ee126170)
- [Guida alla migrazione dei ruoli Active Directory Domain Services e Server DNS](https://technet.microsoft.com/library/dd379558)
- [Guida alla migrazione di BranchCache](https://technet.microsoft.com/library/dd548365)
- [Guida alla migrazione del server DHCP (Dynamic Host Configuration Protocol)](https://technet.microsoft.com/library/dd379535)
- [Guida alla migrazione di Servizi file](https://technet.microsoft.com/library/dd379487)
- [Guida alla migrazione dell'Autorità registrazione integrità](https://technet.microsoft.com/library/ee791829)
- [Guida alla migrazione di Hyper-V](https://technet.microsoft.com/library/ee849855)
- [Guida alla migrazione della configurazione IP](https://technet.microsoft.com/library/dd379537)
- [Guida alla migrazione di utenti e gruppi locali](https://technet.microsoft.com/library/dd379531)
- [Guida alla migrazione del Server dei criteri di rete](https://technet.microsoft.com/library/ee791849)
- [Guida alla migrazione dei servizi di stampa](https://technet.microsoft.com/library/dd379488)
- [Guida alla migrazione di Servizi Desktop remoto](https://technet.microsoft.com/library/ff849223)
- [Guida alla migrazione di RRAS](https://technet.microsoft.com/library/ee822825)
- [Informazioni e attività comuni per la migrazione a Windows Server](https://technet.microsoft.com/library/ff400258)
- [Guida alla migrazione di Windows Server Update Services 3.0 SP2](https://technet.microsoft.com/library/ee822826)
 
Per altre risorse sulla migrazione, visita [Eseguire la migrazione di ruoli e funzionalità a Windows Server 2008 R2](https://technet.microsoft.com/library/dd365353).
