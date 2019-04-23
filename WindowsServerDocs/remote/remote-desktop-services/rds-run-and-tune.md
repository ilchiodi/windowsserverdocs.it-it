---
title: 'Servizi Desktop remoto: eseguire e ottimizzare'
description: Fornisce informazioni di gestione per Servizi Desktop remoto.
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: remote-desktop-services
ms.author: spatnaik
ms.date: 02/08/2017
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 79909767-a4c3-4ecf-8d3f-77d37a663153
author: spatnaik
manager: scottman
ms.openlocfilehash: 40f8dbd560da359e8764ed715e7776cc2d230a7f
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59862182"
---
# <a name="run-and-tune-your-remote-desktop-services-environment"></a>Eseguire e ottimizzare l'ambiente di Servizi Desktop remoto

Ottimizzazione della distribuzione richiede tempo e richiede la strumentazione e monitoraggio. Usare i processi seguenti per ottimizzare la distribuzione di Desktop remoto, garantire l'esecuzione e abilitare la scalabilità orizzontale () in base alle esigenze. 

È buona norma controllano continuamente le metriche e saldo contro i costi operativi.

## <a name="management-and-monitoring"></a>Gestione e monitoraggio

Consulta [gestire gli utenti nella raccolta di servizi desktop remoto](rds-user-management.md) per informazioni su come gestire l'accesso ai desktop e le risorse remote.

Uso **Microsoft Operations Management Suite (OMS)** per monitorare le distribuzioni di Desktop remoto per potenziali colli di bottiglia e gestirli utilizzando uno dei modi seguenti: 

- **Server Manager**: Usare lo strumento di gestione di desktop remoto che è integrato Windows Server per gestire le distribuzioni con un massimo di 500 utenti finali remoti simultanee. 
- **PowerShell**: Usare il modulo PowerShell di desktop remoto, compilato anche in Windows Server, per gestire le distribuzioni con gli utenti finali di remoti simultanee di fino a 5000.

## <a name="scale-bigger-better-faster"></a>Scalabilità: Dimensioni maggiori, migliore, più velocemente

Con la visibilità della distribuzione, è possibile controllare la scala con maggiore precisione. Puoi aggiungere e rimuovere i server host di Desktop remoto in base alle esigenze di scalabilità. 

Le distribuzioni di Desktop remoto che si basano su Azure possono rendere utilizzare servizi di Azure, come SQL Azure, per la scalabilità automatica su richiesta.

## <a name="automation-script-for-success"></a>Automazione: Script per l'esito positivo

Gestione di un'applicazione in esecuzione, con scalabilità elevata prevede la ripetizione di operazioni a intervalli regolari. Usare i cmdlet di PowerShell di Servizi Desktop remoto e i provider WMI per sviluppare gli script che possono essere eseguiti in più distribuzioni quando necessario. Eseguire le regole di Best Practice Analyzer (BPA) per Servizi Desktop remoto in distribuzioni per ottimizzare le distribuzioni.

## <a name="load-testing-avoid-surprises"></a>Test di carico: Evitare sorprese

La distribuzione con test di stress e simulazione dell'utilizzo realistici di test di carico. Modificare le dimensioni di caricamento per evitare sorprese. Verificare che la velocità di risposta soddisfi i requisiti dell'utente, e che l'intero sistema sia resiliente. Creare i test di carico con strumenti di simulazione, come LoginVSI, che consentono di controllare la capacità della distribuzione per soddisfare le esigenze degli utenti. 