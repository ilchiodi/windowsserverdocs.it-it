---
title: Servizi Desktop remoto - Eseguire e ottimizzare
description: Informazioni di gestione di Servizi Desktop remoto.
ms.prod: windows-server
ms.technology: remote-desktop-services
ms.author: spatnaik
ms.date: 02/08/2017
ms.topic: article
ms.assetid: 79909767-a4c3-4ecf-8d3f-77d37a663153
author: spatnaik
manager: scottman
ms.openlocfilehash: 8ce006894376ad120d9d23b6c5f0f891615657fb
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "80859064"
---
# <a name="run-and-tune-your-remote-desktop-services-environment"></a>Eseguire e ottimizzare il proprio ambiente Servizi Desktop remoto

L'ottimizzazione della distribuzione richiede tempo, strumentazione e monitoraggio. Usare i processi seguenti per ottimizzare la distribuzione di Desktop remoto, garantirne la disponibilità e abilitare la scalabilità orizzontale (e verticale) in base alle esigenze. 

È buona norma controllare continuamente le metriche e l'equilibrio con i costi operativi.

## <a name="management-and-monitoring"></a>Gestione e monitoraggio

Consultare [Gestire gli utenti nella raccolta di Servizi Desktop remoto](rds-user-management.md) per informazioni su come gestire l'accesso ai desktop e alle risorse remote.

Usare **Microsoft Operations Management Suite (OMS)** per monitorare le distribuzioni di Desktop remoto in modo da individuare eventuali colli di bottiglia e gestirli utilizzando uno dei modi seguenti: 

- **Server Manager**: Usare lo strumento di gestione di Desktop remoto integrato in Windows Server per gestire le distribuzioni con un massimo di 500 utenti finali remoti simultanei. 
- **PowerShell**: Usare il modulo PowerShell di Desktop remoto integrato in Windows Server per gestire le distribuzioni con un massimo di 5.000 utenti finali remoti simultanei.

## <a name="scale-bigger-better-faster"></a>Scalabilità: Dimensioni maggiori, prestazioni migliori, velocità maggiore

Grazie alla visibilità della distribuzione, è possibile controllare la scalabilità con maggiore precisione. È possibile aggiungere e rimuovere facilmente i server host di Desktop remoto in base alle esigenze di scalabilità. 

Le distribuzioni di Desktop remoto integrate in Azure possono utilizzare servizi di Azure, come Azure SQL, per la scalabilità automatica su richiesta.

## <a name="automation-script-for-success"></a>Automazione: Script efficaci

Per mantenere un'applicazione con scalabilità elevata in esecuzione, è necessario ripetere operazioni a intervalli regolari. Usare i cmdlet di PowerShell Servizi Desktop remoto e i provider WMI per sviluppare script che possano essere eseguiti in più distribuzioni quando necessario. Eseguire le regole di Best Practice Analyzer (BPA) per Servizi Desktop remoto per ottimizzare le distribuzioni.

## <a name="load-testing-avoid-surprises"></a>Test di carico: Evitare sorprese

Si consiglia di verificare la distribuzione eseguendo con test di sforzo e una simulazione dell'uso reale. Variare le dimensioni del carico per evitare sorprese. Verificare che la velocità di risposta soddisfi i requisiti degli utenti e che l'intero sistema sia resiliente. Crea test di carico con strumenti di simulazione, come LoginVSI, che verificano la capacità della distribuzione di soddisfare le esigenze degli utenti. 