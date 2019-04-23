---
ms.assetid: 155abe09-6360-4913-8dd9-7392d71ea4e6
title: Configurazione di un computer per la risoluzione dei problemi
description: ''
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server-threshold
ms.technology: identity-adds
ms.openlocfilehash: 1acb5f7d309d58ed4a5a3aca6bb89f01c0cbf933
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59854122"
---
# <a name="configuring-a-computer-for-troubleshooting"></a>Configurazione di un computer per la risoluzione dei problemi

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Prima di utilizzare tecniche di risoluzione dei problemi avanzate per identificare e risolvere i problemi di Active Directory, configurare i computer per la risoluzione dei problemi. È necessario anche una conoscenza di base di risoluzione dei problemi relativi concetti, procedure e strumenti.

Per informazioni sugli strumenti di monitoraggio per Windows Server, vedere la Guida dettagliata per [monitoraggio delle prestazioni e affidabilità in Windows Server](https://go.microsoft.com/fwlink/?LinkId=123737)

## <a name="configuration-tasks-for-troubleshooting"></a>Attività di configurazione per la risoluzione dei problemi

Per configurare il computer per la risoluzione dei problemi di Active Directory Domain Services (AD DS), eseguire le attività seguenti:

### <a name="install-remote-server-administration-tools-for-ad-ds"></a>Installare strumenti di amministrazione Server remota per Active Directory Domain Services

Quando si installa Active Directory Domain Services per creare un controller di dominio, gli strumenti di amministrazione utilizzabili per gestire Active Directory Domain Services vengono installati automaticamente. Se si desidera gestire i controller di dominio in modalità remota da un computer che non è un controller di dominio, è possibile installare strumenti di amministrazione remota Server (RSAT) in un server membro o una workstation in cui è in esecuzione una versione supportata di Windows. Amministrazione remota del server sostituisce gli strumenti di supporto di Windows da Windows Server 2003.

Per informazioni sull'installazione di amministrazione remota del server, vedere l'articolo [strumenti di amministrazione remota del Server](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools).

### <a name="configure-reliability-and-performance-monitor"></a>Configurare Monitoraggio affidabilità e prestazioni

Windows Server include il Windows affidabilità e Performance Monitor, che è uno snap-in Microsoft Management Console (MMC) che combina le funzionalità dei precedenti strumenti autonomi, incluso Server Performance Advisor, avvisi e registri di prestazioni e monitoraggio di sistema. Questo snap-in fornisce un'interfaccia utente grafica (GUI) per la personalizzazione di insiemi agenti di raccolta dati e sessioni di traccia eventi.

Affidabilità e Performance Monitor include anche Monitoraggio affidabilità, uno snap-in MMC che tiene traccia delle modifiche al sistema e li confronta con le modifiche nella stabilità del sistema, che fornisce una visualizzazione grafica del rapporto di collaborazione.

### <a name="set-logging-levels"></a>Impostare i livelli di registrazione

Se le informazioni che viene visualizzato nel log del servizio Directory nel Visualizzatore eventi non sono sufficienti per la risoluzione dei problemi, generare i livelli di registrazione utilizzando la voce del Registro di sistema appropriate nel **HKEY_LOCAL MACHINESYSTEMCurrentControlSetServicesNTDSDiagnostics**.

Per impostazione predefinita, i livelli di registrazione per tutte le voci sono impostati su **0**, che fornisce la quantità minima di informazioni. È il più elevato livello di registrazione **5**. Aumentare il livello di una voce fa in modo che altri eventi da registrare nel registro eventi del servizio Directory.

Utilizzare la procedura seguente per modificare il livello di registrazione per una voce di diagnostica. Per eseguire questa procedura, è necessaria almeno l'appartenenza al gruppo **Domain Admins** o a un gruppo equivalente.

> [!WARNING]
> È consigliabile non modificare direttamente il Registro di sistema, a meno che non ci siano altre alternative. Modifiche al Registro di sistema non vengono convalidate dall'editor del Registro di sistema o da Windows prima di applicarle, e di conseguenza, possono essere archiviati i valori non corretti. Ciò può causare errori irreversibili nel sistema. Quando possibile, usare criteri di gruppo o altri strumenti di Windows, ad esempio lo snap-in MMC, eseguire le attività, piuttosto che modificare direttamente il Registro di sistema. Se è necessario modificare il Registro di sistema, usare la massima cautela.
>

Per modificare il livello di registrazione per una voce di diagnostica

1. Fare clic su **avviare** > **eseguire** > tipo **regedit** > fare clic su **OK**.
2. Passare alla voce per il quale si desidera impostare l'accesso.
   * ESEMPIO: HKEY_LOCAL_MACHINESYSTEMCurrentControlSetServicesNTDSDiagnostics
3. Fare doppio clic sulla voce e nella **Base**, fare clic su **decimale**.
4. Nelle **valore**, digitare un numero intero compreso tra **0** attraverso **5**, quindi fare clic su **OK**.
