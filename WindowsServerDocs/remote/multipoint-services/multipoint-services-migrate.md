---
title: Eseguire la migrazione a MultiPoint Services in Windows Server 2016
description: Informazioni su come eseguire la migrazione da una versione precedente di MultiPoint Services
ms.custom: na
ms.date: 07/29/2016
ms.prod: windows-server
ms.technology: multipoint-services
ms.reviewer: na
ms.suite: na
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 16c217ad-700a-48a3-8398-4a7f7e9edb52
author: lizap
manager: dongill
ms.author: elizapo
ms.openlocfilehash: 297de9ee2450856e24b9196a8bfb312991657e6d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71389044"
---
# <a name="multipoint-services-migration-in-windows-server-2016"></a>Migrazione di MultiPoint Services in Windows Server 2016
>Si applica a: Windows Server 2016

È possibile eseguire la migrazione da una versione precedente di Windows Server 2016 MultiPoint Services alla versione RTM di MultiPoint Services. Le informazioni seguenti forniscono informazioni sulla preparazione e i passaggi di migrazione e verifica.

La documentazione e gli strumenti per la migrazione semplificano la migrazione delle impostazioni e dei dati del ruolo del server da un server esistente a un server di destinazione che esegue Windows Server 2016. Il processo descritto in questa guida consente di semplificare il processo di migrazione, ridurne i tempi, perfezionarlo ed eliminare possibili conflitti che potrebbero altrimenti verificarsi durante il processo di migrazione. 

## <a name="what-to-know-before-you-begin"></a>Informazioni preliminari
Prima di iniziare il processo di migrazione, tenere presente quanto segue:

- Il processo di migrazione non raccoglie o registra automaticamente le impostazioni per le applicazioni nel ruolo Servizi MultiPoint. È necessario creare un piano di migrazione personalizzato per tutte le applicazioni di cui si desidera eseguire la migrazione. Questo vale anche quando si usa la funzionalità desktop virtuali in MultiPoint Services.
- Questa guida non fornisce indicazioni per lo stato di trasferimento dei dati salvati in cartelle utente o condivise nel server MultiPoint. Questo vale per le stazioni regolari e i desktop virtuali.
- Questa guida non contiene istruzioni su come eseguire la migrazione quando nel server di origine vengono eseguiti più ruoli. Se il server esegue più ruoli, è necessario progettare una procedura di migrazione personalizzata specifica per l'ambiente server, in base alle informazioni fornite nelle guide alla migrazione dei ruoli.
- Questa guida non contiene informazioni per la migrazione di licenze CAL Servizi Desktop remoto. Per informazioni, vedere [eseguire la migrazione di licenze CAL (Client Access License) Servizi Desktop remoto](https://technet.microsoft.com/library/dd851844.aspx).

## <a name="supported-migration-scenarios-for-multipoint-services-in-windows-server-2016"></a>Scenari di migrazione supportati per MultiPoint Services in Windows Server 2016
I servizi ruolo del servizio MultiPoint sono disponibili in Windows Server 2016 standard e Datacenter. In questa guida alla migrazione viene descritto come eseguire la migrazione dei servizi ruolo Servizi MultiPoint da un server di origine che esegue Windows Server 2016 a un server di destinazione che esegue la stessa versione.

## <a name="scenarios-that-are-not-supported"></a>Scenari non supportati

Gli scenari di migrazione seguenti non sono supportati:

- Migrazione o aggiornamento da Windows MultiPoint Server 2012 e 2011.
- Migrazione da un server di origine a un server di destinazione in esecuzione nel sistema operativo con una lingua diversa per l'interfaccia utente del sistema installata.
- Migrazione del servizio ruolo Servizi MultiPoint da server fisici a macchine virtuali.
- Migrazione di applicazioni o impostazioni dell'applicazione dal server MultiPoint.

## <a name="the-impact-of-migration-on-multipoint-services"></a>L'effetto della migrazione su servizi MultiPoint
Tenere presente che il ruolo Servizi MultiPoint non sarà disponibile durante la migrazione. Al fine di ridurre al minimo i tempi di inattività e l'impatto sugli utenti, è possibile pianificare l'esecuzione della migrazione dei dati durante gli orari di minore utilizzo della rete. Avvisare gli utenti che le risorse non saranno disponibili durante tale intervallo di tempo.

## <a name="migration-information-and-steps"></a>Informazioni e procedure per la migrazione
Usare le informazioni seguenti per pianificare ed eseguire la migrazione di MultiPoint Services:

- [Raccogliere le informazioni necessarie per la migrazione.](multipoint-services-migration-preparation.md)
- [Eseguire la migrazione del servizio ruolo Servizi MultiPoint.](multipoint-services-migration-steps.md)
- [Convalidare la migrazione ed eseguire le attività di pulizia successive alla migrazione](multipoint-services-post-migration-steps.md)