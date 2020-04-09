---
ms.assetid: 155abe09-6360-4913-8dd9-7392d71ea4e6
title: Configurazione di un computer per la risoluzione dei problemi
ms.author: joflore
author: MicrosoftGuyJFlo
manager: mtillman
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows-server
ms.technology: identity-adds
ms.openlocfilehash: d9d279615dc1f70ffdcff9e49a4aa619f0106a93
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80822974"
---
# <a name="configuring-a-computer-for-troubleshooting"></a>Configurazione di un computer per la risoluzione dei problemi

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Prima di utilizzare tecniche avanzate di risoluzione dei problemi per identificare e correggere Active Directory problemi, configurare i computer per la risoluzione dei problemi. È inoltre necessario avere una conoscenza di base dei concetti, procedure e strumenti di risoluzione dei problemi.

Per informazioni sugli strumenti di monitoraggio per Windows Server, vedere la guida dettagliata al [monitoraggio delle prestazioni e dell'affidabilità in Windows Server](https://go.microsoft.com/fwlink/?LinkId=123737)

## <a name="configuration-tasks-for-troubleshooting"></a>Attività di configurazione per la risoluzione dei problemi

Per configurare il computer per la risoluzione dei problemi Active Directory Domain Services (AD DS), eseguire le attività seguenti:

### <a name="install-remote-server-administration-tools-for-ad-ds"></a>Installare Strumenti di amministrazione remota del server per servizi di dominio Active Directory

Quando si installa Servizi di dominio Active Directory per creare un controller di dominio, gli strumenti di amministrazione utilizzati per la gestione di servizi di dominio Active Directory vengono installati automaticamente. Se si desidera gestire i controller di dominio in modalità remota da un computer che non è un controller di dominio, è possibile installare il Strumenti di amministrazione remota del server (strumenti di amministrazione remota del server) in un server membro o in una workstation che esegue una versione supportata di Windows. Amministrazione remota Windows sostituisce gli strumenti di supporto di Windows da Windows Server 2003.

Per informazioni sull'installazione di strumenti di amministrazione remota del server, vedere l'articolo [strumenti di amministrazione remota del server](https://docs.microsoft.com/windows-server/remote/remote-server-administration-tools).

### <a name="configure-reliability-and-performance-monitor"></a>Configurare il monitoraggio dell'affidabilità e delle prestazioni

Windows Server include il monitoraggio dell'affidabilità e delle prestazioni di Windows, uno snap-in di Microsoft Management Console (MMC) che combina la funzionalità di precedenti strumenti autonomi, tra cui Avvisi e registri di prestazioni, Server Performance Advisor e monitor di sistema. Questo snap-in fornisce un'interfaccia utente grafica (GUI) per personalizzare gli insiemi agenti di raccolta dati e le sessioni di traccia eventi.

Il monitoraggio dell'affidabilità e delle prestazioni include anche Monitoraggio affidabilità, uno snap-in di MMC che consente di tenere traccia delle modifiche apportate al sistema e di confrontarle con le modifiche apportate alla stabilità del sistema, offrendo una visualizzazione grafica della relazione.

### <a name="set-logging-levels"></a>Impostare i livelli di registrazione

Se le informazioni ricevute nel log del servizio directory in Visualizzatore eventi non sono sufficienti per la risoluzione dei problemi, aumentare i livelli di registrazione usando la voce del registro di sistema appropriata in **HKEY_LOCAL_MACHINE \system\currentcontrolset\services\ntds\diagnostics**.

Per impostazione predefinita, i livelli di registrazione per tutte le voci vengono impostati su **0**, che fornisce la quantità minima di informazioni. Il livello di registrazione più alto è **5**. L'aumento del livello di una voce comporta la registrazione di eventi aggiuntivi nel registro eventi del servizio directory.

Utilizzare la procedura seguente per modificare il livello di registrazione per una voce di diagnostica. L'appartenenza al gruppo **Domain Admins** oppure a un gruppo equivalente è il requisito minimo necessario per completare questa procedura.

> [!WARNING]
> È consigliabile non modificare direttamente il Registro di sistema, a meno che non ci siano altre alternative. Modifiche al Registro di sistema non vengono convalidate dall'editor del Registro di sistema o da Windows prima di applicarle, e di conseguenza, possono essere archiviati i valori non corretti. Ciò può causare errori irreversibili nel sistema. Quando possibile, utilizzare Criteri di gruppo o altri strumenti di Windows, ad esempio gli snap-in MMC, per eseguire le attività, anziché modificare direttamente il registro di sistema. Se è necessario modificare il Registro di sistema, usare la massima cautela.
>

Per modificare il livello di registrazione per una voce di diagnostica

1. Fare clic su **Start** > **Esegui** > digitare **Regedit** > fare clic su **OK**.
2. Passare alla voce per la quale si desidera impostare l'accesso.
   * ESEMPIO: HKEY_LOCAL_MACHINESYSTEMCurrentControlSetServicesNTDSDiagnostics
3. Fare doppio clic sulla voce e in **base**fare clic su **Decimal**.
4. In **valore**Digitare un numero intero compreso tra **0** e **5**, quindi fare clic su **OK**.
