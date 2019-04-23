---
title: Eseguire la migrazione ai servizi MultiPoint in Windows Server 2016
description: Informazioni su come eseguire la migrazione da una versione precedente di MultiPoint Services
ms.custom: na
ms.date: 07/29/2016
ms.prod: windows-server-threshold
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 16c217ad-700a-48a3-8398-4a7f7e9edb52
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 24c35c31bf920c41bafa16901ee30a023565dad8
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59861202"
---
# <a name="multipoint-services-migration-in-windows-server-2016"></a>Migrazione di servizi multiPoint in Windows Server 2016
>Si applica a: Windows Server 2016

È possibile eseguire la migrazione da una versione precedente di Windows Server 2016 MultiPoint Services alla versione RTM di MultiPoint Services. Di seguito vengono fornite preparazione dei passaggi di informazioni e la migrazione e verifica.

Strumenti e documentazione per la migrazione semplificano la migrazione delle impostazioni del ruolo server e i dati da un server esistente a un server di destinazione che esegue Windows Server 2016. Il processo descritto in questa guida consente di semplificare il processo di migrazione, ridurne i tempi, perfezionarlo ed eliminare possibili conflitti che potrebbero altrimenti verificarsi durante il processo di migrazione. 

## <a name="what-to-know-before-you-begin"></a>Cosa è necessario sapere prima di iniziare
Prima di iniziare il processo di migrazione, tenere presente quanto segue:

- Il processo di migrazione non automaticamente raccogliere o registrare le impostazioni per le applicazioni per il ruolo di servizi MultiPoint. È necessario creare un piano di migrazione personalizzato per tutte le applicazioni che si vuole eseguire la migrazione. Questo vale anche quando si usa la funzionalità di desktop virtuali in MultiPoint Services.
- Questa Guida non fornisce materiale sussidiario per lo spostamento dei dati salvati in utenti o cartelle condivise in MultiPoint server. Questo vale per le stazioni regolari e virtual desktop stations.
- Questa Guida non contiene istruzioni su come eseguire la migrazione quando il server di origine esegue più ruoli. Se il server esegue più ruoli, è necessario progettare una procedura di migrazione personalizzata specifica per l'ambiente server, basandosi sulle informazioni fornite nelle guide alla migrazione di ruoli.
- Questa Guida non contiene informazioni per la migrazione di Servizi Desktop remota CAL Servizi Desktop remoto. Queste informazioni, vedere [eseguire la migrazione di Remote Desktop Services licenze di accesso Client (CAL)](https://technet.microsoft.com/library/dd851844.aspx).

## <a name="supported-migration-scenarios-for-multipoint-services-in-windows-server-2016"></a>Scenari di migrazione supportati per i servizi MultiPoint in Windows Server 2016
I servizi ruolo di servizi MultiPoint è disponibile in Windows Server 2016 Standard e Datacenter. Questa Guida alla migrazione viene descritto come eseguire la migrazione di servizi di ruolo di servizi Multipoint da un server di origine che esegue Windows Server 2016 a un server di destinazione che esegue la stessa versione.

## <a name="scenarios-that-are-not-supported"></a>Scenari non supportati

Gli scenari di migrazione seguenti non sono supportati:

- La migrazione o l'aggiornamento da Windows MultiPoint Server 2012 e 2011.
- Migrazione da un server di origine a un server di destinazione che è in esecuzione nel sistema operativo con un altro sistema lingua dell'interfaccia utente installata.
- Migrazione del servizio ruolo di servizi MultiPoint da server fisici in macchine virtuali.
- Migrazione di applicazioni o le impostazioni dell'applicazione dal Server MultiPoint.

## <a name="the-impact-of-migration-on-multipoint-services"></a>L'impatto della migrazione in MultiPoint Services
Tenere presente che il ruolo di servizi MultiPoint non sarà disponibile durante la migrazione. Al fine di ridurre al minimo i tempi di inattività e l'impatto sugli utenti, è possibile pianificare l'esecuzione della migrazione dei dati durante gli orari di minore utilizzo della rete. Avvisare gli utenti che le risorse non saranno disponibili durante tale intervallo di tempo.

## <a name="migration-information-and-steps"></a>I passaggi e le informazioni sulla migrazione
Usare le informazioni seguenti per pianificare ed eseguire la migrazione di servizi MultiPoint:

- [Raccogliere le informazioni necessarie per la migrazione.](multipoint-services-migration-preparation.md)
- [Migrazione del servizio ruolo di servizi MultiPoint.](multipoint-services-migration-steps.md)
- [Convalidare la migrazione ed eseguire qualsiasi attività di pulizia post-migrazione](multipoint-services-post-migration-steps.md)