---
title: Introduzione a Windows Server Update Services (WSUS)
description: 'Argomento di Windows Server Update Services (WSUS): Panoramica del ruolo server e delle relative applicazioni pratiche'
ms.prod: windows-server
ms.technology: manage-wsus
ms.topic: get-started article
ms.assetid: 90e3464c-49d8-4861-96db-ee6f8a09ec5b
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 5/22/2017
ms.openlocfilehash: 3bb365c627840d4152b0dc450e24dd83d82103ca
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80828633"
---
# <a name="windows-server-update-services-wsus"></a>Windows Server Update Services (WSUS)

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Windows Server Update Services (WSUS) consente agli amministratori IT di distribuire gli aggiornamenti dei prodotti Microsoft più recenti. WSUS consente di gestire completamente la distribuzione di aggiornamenti resi disponibili tramite Microsoft Update nei computer connessi in rete. In questo argomento viene fornita una panoramica di questo ruolo server e altre informazioni sulla distribuzione e la gestione di WSUS.

## <a name="wsus-server-role-description"></a>Descrizione del ruolo server WSUS
Un server WSUS fornisce le funzionalità necessarie per gestire e distribuire gli aggiornamenti mediante una console di gestione. Un server WSUS può anche fungere da origine degli aggiornamenti per altri server WSUS all'interno dell'organizzazione. Quando il server WSUS svolge questa funzione, viene chiamato server upstream. In un'implementazione di WSUS, almeno un server WSUS nella rete deve essere in grado di connettersi a Microsoft Update per ottenere informazioni sugli aggiornamenti disponibili. L'amministratore può stabilire, in base alla sicurezza e alla configurazione della rete, quanti altri server WSUS possono connettersi direttamente a Microsoft Update.

### <a name="practical-applications"></a>Applicazioni pratiche
La gestione degli aggiornamenti è il processo di controllo della distribuzione e manutenzione delle versioni software provvisorie in ambienti di produzione. Consente di gestire l'efficienza operativa, superare le vulnerabilità di sicurezza e conservare la stabilità dell'ambiente di produzione. Se l'organizzazione non è in grado di determinare e mantenere un livello di affidabilità stabilito all'interno dei sistemi operativi e nei software applicativi in uso, può prodursi una serie di vulnerabilità di sicurezza che, se sfruttate, possono provocare perdite di profitti e violazioni della proprietà intellettuale. Per ridurre al minimo questa minaccia, è necessario disporre di sistemi configurati correttamente, utilizzare il software più recente e installare gli aggiornamenti software consigliati.

Gli scenari di base in cui Windows Server Update Services aggiunge valore all'azienda sono i seguenti:

-   gestione centralizzata degli aggiornamenti

-   Automazione della gestione degli aggiornamenti

### <a name="new-and-changed-functionality"></a>Funzionalità nuove e modificate

> [!NOTE]
> Per aggiornare una versione qualsiasi di Windows Server che supporti WSUS 3.2 a Windows Server 2012 R2 è innanzitutto necessario disinstallare WSUS 3.2.
> 
> In Windows Server 2012, l'aggiornamento da una versione qualsiasi di Windows Server in cui è installato WSUS 3.2 viene bloccato durante il processo di installazione, se WSUS 3.2 viene rilevato. In questo caso, verrà richiesto di disinstallare innanzitutto Windows Server Update Services prima di aggiornare il server.
> 
> Grazie alle modifiche apportate in questa versione di Windows Server e Windows Server 2012 R2, quando si esegue l'aggiornamento da una versione qualsiasi di Windows Server e WSUS 3.2, l'installazione non verrà bloccata. Se non si disinstalla WSUS 3.2 prima di eseguire un aggiornamento a Windows Server 2012 R2, le attività successive all'installazione per WSUS in Windows Server 2012 R2 non verranno eseguite correttamente. In questo caso, l'unico modo per correggere l'errore consiste nel formattare l'unità disco rigido e reinstallare Windows Server.

Windows Server Update Services è un ruolo predefinito del server che include i miglioramenti seguenti:

-   può essere aggiunto o rimosso tramite Server Manager

-   include i cmdlet di Windows PowerShell per gestire le attività amministrative più importanti in WSUS

-   aggiunge la funzionalità di hash SHA256 per ulteriore sicurezza

-   fornisce la separazione di client e server: le versioni di Agente di Windows Update possono essere spedite in modo indipendente da WSUS

### <a name="using-windows-powershell-to-manage-wsus"></a>Utilizzo di Windows PowerShell per gestire WSUS
Per automatizzare le operazioni, gli amministratori di sistema hanno bisogno dell'automazione della riga di comando. Il principale obiettivo è quello di facilitare l'amministrazione WSUS consentendo agli amministratori di automatizzare le operazioni quotidiane.

**Valore aggiunto da queste modifiche**

Esponendo le operazioni WSUS di base tramite Windows PowerShell, gli amministratori di sistema possono aumentare la produttività, ridurre la curva di apprendimento di nuovi strumenti e diminuire il numero di errori imputabili alle aspettative fallite derivanti da una mancanza di coerenza tra operazioni analoghe.

**Differenze di funzionamento**

Nelle versioni precedenti del sistema operativo Windows Server non sono disponibili cmdlet di Windows PowerShell e l'automazione della gestione delle patch è impegnativa. I cmdlet di Windows PowerShell per operazioni WSUS offrono maggiori flessibilità e agilità all'amministratore di sistema.

## <a name="in-this-collection"></a>Contenuto della raccolta
In questa raccolta sono disponibili le guide per la pianificazione, distribuzione e gestione di WSUS seguenti:

-   [Distribuire Windows Server Update Services](../deploy/deploy-windows-server-update-services.md)

-   [Gestire gli aggiornamenti con Windows Server Update Services](../manage/update-management-with-windows-server-update-services.md)


