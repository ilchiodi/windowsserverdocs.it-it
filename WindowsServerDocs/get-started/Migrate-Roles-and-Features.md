---
title: Migrazione di ruoli e funzionalità
description: Informazioni su come eseguire la migrazione di ruoli e funzionalità a una versione più recente di Windows Server.
ms.prod: windows-server
ms.date: 08/28/2019
ms.technology: server-general
ms.topic: article
ms.assetid: 0f78ef4c-dd12-4b1b-8c6e-251dd803c5d1
author: jasongerend
ms.author: jgerend
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 33c1aa654e4c660b4fe2f3305bfaf78b5191220a
ms.sourcegitcommit: e58e1646ffd75d4a89576d967b2dbbbb84764303
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 08/28/2019
ms.locfileid: "70119195"
---
# <a name="migrating-roles-and-features-in-windows-server"></a>Migrazione di ruoli e funzionalità a Windows Server

> Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Questa pagina contiene collegamenti a informazioni e strumenti che semplificano il processo di migrazione di ruoli e funzionalità a una versione più recente di Windows Server. Puoi eseguire la migrazione di file server e risorse di archiviazione usando [Servizio di migrazione archiviazione](../storage/storage-migration-service/overview.md), mentre per molti altri ruoli e funzionalità puoi usare Strumenti di migrazione per Windows Server, un set di cmdlet di PowerShell introdotti in Windows Server 2008 R2 per la migrazione di ruoli e funzionalità.

Le guide alla migrazione supportano le migrazioni di ruoli e funzionalità specificati da un server all'altro (non aggiornamenti sul posto). Se non diversamente specificato nelle guide, sono supportate le migrazioni tra computer fisici e virtuali e tra opzioni di installazione completa di Windows Server e server che eseguono l'opzione di installazione dei componenti di base del server.

## <a name="before-you-begin"></a>Prima di iniziare

Prima di avviare la migrazione di ruoli e funzionalità, verifica che il server di origine e quello di destinazione eseguano entrambi i Service Pack più recenti disponibili per i rispettivi sistemi operativi. 

> [!NOTE]
> Ogni volta che esegui la migrazione o l'aggiornamento a qualsiasi versione di Windows Server, devi esaminare e comprendere i [criteri relativi al ciclo di vita del supporto](https://support.microsoft.com/lifecycle) e l'intervallo di tempo per ogni versione, per poi pianificare il processo di conseguenza. Puoi [cercare le informazioni sul ciclo di vita](https://support.microsoft.com/lifecycle) per la versione specifica di Windows Server che ti interessa.

## <a name="windows-server-2019"></a>Windows Server 2019

Per eseguire la migrazione di file server e risorse di archiviazione a Windows Server 2019 o Windows Server 2016, ti consigliamo di usare [Servizio di migrazione archiviazione](../storage/storage-migration-service/overview.md). Per eseguire la migrazione di altri ruoli, consulta le linee guida per Windows Server 2016 e Windows Server 2012 R2.

## <a name="windows-server-2016"></a>Windows Server 2016

Di seguito sono elencate le guide alla migrazione per Windows Server 2016. Tieni presente che in molti casi puoi anche usare le guide alla migrazione di Windows Server 2012 R2.

- [Servizi Desktop remoto](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/migrate-rds-role-services)
- [Server Web (IIS)](https://www.iis.net/downloads/microsoft/web-deploy)
- [Windows Server Update Services](https://technet.microsoft.com/library/hh852339.aspx)
- [Servizi MultiPoint](https://technet.microsoft.com/windows-server-docs/compute/remote-desktop-services/multipoint-services/multipoint-services-migrate)

Per eseguire la migrazione di file server a Windows Server 2019 o Windows Server 2016, ti consigliamo di usare [Servizio di migrazione archiviazione](../storage/storage-migration-service/overview.md).

## <a name="windows-server-2012-r2"></a>Windows Server 2012 R2

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

È ora disponibile un e-book delle guide alla migrazione per Windows Server 2012 R2 e Windows Server 2012. Per altre informazioni e per scaricare l'e-book, vedi [Raccolta di e-book per le tecnologie Microsoft](https://social.technet.microsoft.com/wiki/contents/articles/11608.e-book-gallery-for-microsoft-technologies.aspx#MigrateRoles).

## <a name="windows-server-2012"></a>Windows Server 2012

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
