---
title: Iniziare con Windows Server Update Services (WSUS)
description: Argomento di Windows Server Update Service (WSUS) - Panoramica del ruolo del Server e le relative applicazioni pratiche
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: get-started article
ms.assetid: 90e3464c-49d8-4861-96db-ee6f8a09ec5b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 5/22/2017
ms.openlocfilehash: 7a6c64e0a4321553162b426e3d6857ff6ac3581c
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59830262"
---
# <a name="windows-server-update-services-wsus"></a>Windows Server Update Services (WSUS)

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Windows Server Update Services (WSUS) consente agli amministratori IT di distribuire gli aggiornamenti dei prodotti Microsoft più recenti. È possibile utilizzare WSUS per gestire completamente la distribuzione degli aggiornamenti rilasciati tramite Microsoft Update ai computer della rete. In questo argomento viene fornita una panoramica di questo ruolo server e altre informazioni sulla distribuzione e la gestione di WSUS.

## <a name="wsus-server-role-description"></a>Descrizione del ruolo Server WSUS
Un server WSUS fornisce funzionalità che è possibile usare per gestire e distribuire gli aggiornamenti tramite una console di gestione. Un server WSUS può anche essere l'origine degli aggiornamenti per altri server WSUS all'interno dell'organizzazione. Quando il server WSUS svolge questa funzione, viene chiamato server upstream. In un'implementazione di WSUS, almeno un server WSUS nella rete deve essere in grado di connettersi a Microsoft Update per ottenere informazioni sugli aggiornamenti disponibili. Come amministratore, è possibile determinare - basata sulla sicurezza di rete e configurazione - quanti altri server WSUS connettersi direttamente a Microsoft Update.

### <a name="practical-applications"></a>Applicazioni pratiche
La gestione degli aggiornamenti è il processo di controllo della distribuzione e manutenzione delle versioni software provvisorie in ambienti di produzione. Consente di gestire l'efficienza operativa, superare le vulnerabilità di sicurezza e conservare la stabilità dell'ambiente di produzione. Se l'organizzazione non è in grado di determinare e mantenere un livello di affidabilità stabilito all'interno dei sistemi operativi e nei software applicativi in uso, può prodursi una serie di vulnerabilità di sicurezza che, se sfruttate, possono provocare perdite di profitti e violazioni della proprietà intellettuale. Per ridurre al minimo questa minaccia, è necessario disporre di sistemi configurati correttamente, utilizzare il software più recente e installare gli aggiornamenti software consigliati.

Gli scenari di base in cui Windows Server Update Services aggiunge valore all'azienda sono i seguenti:

-   gestione centralizzata degli aggiornamenti

-   Automazione della gestione degli aggiornamenti

### <a name="new-and-changed-functionality"></a>Funzionalità nuove e modificate

> [!NOTE]
> Aggiornamento da una qualsiasi versione di Windows Server che supporti WSUS 3.2 a Windows Server 2012 R2 è necessario disinstallare WSUS 3.2.
> 
> In Windows Server 2012, l'aggiornamento da una qualsiasi versione di Windows Server cui è installato WSUS 3.2 è bloccato durante il processo di installazione se viene rilevato WSUS 3.2. In tal caso, verrà richiesto di disinstallare innanzitutto Windows Server Update Services prima di aggiornare il server.
> 
> Tuttavia, a causa di modifiche in questa versione di Windows Server e Windows Server 2012 R2, durante l'aggiornamento da qualsiasi versione di Windows Server e WSUS 3.2, l'installazione non è bloccata. Disinstalla WSUS 3.2 prima di eseguire un aggiornamento di Windows Server 2012 R2 causerà il post di attività di installazione per WSUS in Windows Server 2012 R2 per avere esito negativo. In questo caso, l'unico noti correttiva consiste nel formattare l'unità disco rigido e reinstallare Windows Server.

Windows Server Update Services è un ruolo predefinito del server che include i miglioramenti seguenti:

-   può essere aggiunto o rimosso tramite Server Manager

-   Include cmdlet di Windows PowerShell per gestire le attività amministrative più importanti in WSUS

-   aggiunge la funzionalità di hash SHA256 per ulteriore sicurezza

-   Fornisce la separazione di client e server: le versioni di Windows Update Agent (WUA) possono essere spedite indipendentemente da WSUS

### <a name="using-windows-powershell-to-manage-wsus"></a>Utilizzo di Windows PowerShell per gestire WSUS
Per automatizzare le operazioni, gli amministratori di sistema hanno bisogno dell'automazione della riga di comando. Il principale obiettivo è quello di facilitare l'amministrazione WSUS consentendo agli amministratori di automatizzare le operazioni quotidiane.

**Valore aggiunto da queste modifiche**

Esponendo le operazioni WSUS di base tramite Windows PowerShell, gli amministratori di sistema possono aumentare la produttività, ridurre la curva di apprendimento di nuovi strumenti e diminuire il numero di errori imputabili alle aspettative fallite derivanti da una mancanza di coerenza tra operazioni analoghe.

**Differenze di funzionamento**

Nelle versioni precedenti del sistema operativo Windows Server non sono disponibili cmdlet di Windows PowerShell e l'automazione della gestione delle patch è impegnativa. I cmdlet di Windows PowerShell per operazioni WSUS offrono maggiori flessibilità e agilità all'amministratore di sistema.

## <a name="in-this-collection"></a>In questa raccolta
Sono le seguenti guide per la pianificazione, distribuzione e gestione di Windows Server Update Services in questa raccolta:

-   [Distribuire Windows Server Update Services](../deploy/deploy-windows-server-update-services.md)

-   [Gestire gli aggiornamenti mediante Windows Server Update Services](../manage/update-management-with-windows-server-update-services.md)


