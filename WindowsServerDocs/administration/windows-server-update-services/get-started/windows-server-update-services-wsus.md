---
title: Introduzione a Windows Server Update Services (WSUS)
description: 'Argomento Windows Server Update Service (WSUS): Panoramica del ruolo del server e delle relative applicazioni pratiche'
ms.prod: windows-server
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
ms.openlocfilehash: 89247f91f616233fc6e4967a0457ff34fac221da
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71361651"
---
# <a name="windows-server-update-services-wsus"></a>Windows Server Update Services (WSUS)

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Windows Server Update Services (WSUS) consente agli amministratori IT di distribuire gli aggiornamenti dei prodotti Microsoft più recenti. È possibile utilizzare WSUS per gestire completamente la distribuzione degli aggiornamenti rilasciati tramite Microsoft Update ai computer della rete. In questo argomento viene fornita una panoramica di questo ruolo server e altre informazioni sulla distribuzione e la gestione di WSUS.

## <a name="wsus-server-role-description"></a>Descrizione del ruolo server WSUS
Un server WSUS fornisce funzionalità che è possibile utilizzare per gestire e distribuire gli aggiornamenti tramite una console di gestione. Un server WSUS può essere anche l'origine degli aggiornamenti per altri server WSUS all'interno dell'organizzazione. Quando il server WSUS svolge questa funzione, viene chiamato server upstream. In un'implementazione di WSUS, almeno un server WSUS nella rete deve essere in grado di connettersi a Microsoft Update per ottenere informazioni sull'aggiornamento disponibili. In qualità di amministratore, è possibile determinare in base alla sicurezza e alla configurazione della rete, ovvero il numero di altri server WSUS che si connettono direttamente a Microsoft Update.

### <a name="practical-applications"></a>Applicazioni pratiche
La gestione degli aggiornamenti è il processo di controllo della distribuzione e manutenzione delle versioni software provvisorie in ambienti di produzione. Consente di gestire l'efficienza operativa, superare le vulnerabilità di sicurezza e conservare la stabilità dell'ambiente di produzione. Se l'organizzazione non è in grado di determinare e mantenere un livello di affidabilità stabilito all'interno dei sistemi operativi e nei software applicativi in uso, può prodursi una serie di vulnerabilità di sicurezza che, se sfruttate, possono provocare perdite di profitti e violazioni della proprietà intellettuale. Per ridurre al minimo questa minaccia, è necessario disporre di sistemi configurati correttamente, utilizzare il software più recente e installare gli aggiornamenti software consigliati.

Gli scenari di base in cui Windows Server Update Services aggiunge valore all'azienda sono i seguenti:

-   gestione centralizzata degli aggiornamenti

-   Automazione della gestione degli aggiornamenti

### <a name="new-and-changed-functionality"></a>Funzionalità nuove e modificate

> [!NOTE]
> Per eseguire l'aggiornamento da qualsiasi versione di Windows Server che supporti WSUS 3,2 a Windows Server 2012 R2, è necessario prima disinstallare WSUS 3,2.
> 
> In Windows Server 2012 l'aggiornamento da una versione qualsiasi di Windows Server con WSUS 3,2 installato viene bloccato durante il processo di installazione se viene rilevato WSUS 3,2. In tal caso, verrà richiesto di disinstallare prima Windows Server Update Services prima di aggiornare il server.
> 
> Tuttavia, a causa delle modifiche apportate a questa versione di Windows Server e Windows Server 2012 R2, quando si esegue l'aggiornamento da una versione qualsiasi di Windows Server e WSUS 3,2, l'installazione non viene bloccata. Se non si disinstalla WSUS 3,2 prima di eseguire un aggiornamento di Windows Server 2012 R2, le attività successive all'installazione per WSUS in Windows Server 2012 R2 avranno esito negativo. In questo caso, l'unica misura correttiva nota consiste nel formattare il disco rigido e reinstallare Windows Server.

Windows Server Update Services è un ruolo predefinito del server che include i miglioramenti seguenti:

-   può essere aggiunto o rimosso tramite Server Manager

-   Include i cmdlet di Windows PowerShell per gestire le attività amministrative più importanti in WSUS

-   aggiunge la funzionalità di hash SHA256 per ulteriore sicurezza

-   Fornisce la separazione tra client e server: le versioni dell'agente Windows Update (WUA) possono essere fornite indipendentemente da WSUS

### <a name="using-windows-powershell-to-manage-wsus"></a>Utilizzo di Windows PowerShell per gestire WSUS
Per automatizzare le operazioni, gli amministratori di sistema hanno bisogno dell'automazione della riga di comando. Il principale obiettivo è quello di facilitare l'amministrazione WSUS consentendo agli amministratori di automatizzare le operazioni quotidiane.

**Valore aggiunto da queste modifiche**

Esponendo le operazioni WSUS di base tramite Windows PowerShell, gli amministratori di sistema possono aumentare la produttività, ridurre la curva di apprendimento di nuovi strumenti e diminuire il numero di errori imputabili alle aspettative fallite derivanti da una mancanza di coerenza tra operazioni analoghe.

**Differenze di funzionamento**

Nelle versioni precedenti del sistema operativo Windows Server non sono disponibili cmdlet di Windows PowerShell e l'automazione della gestione delle patch è impegnativa. I cmdlet di Windows PowerShell per operazioni WSUS offrono maggiori flessibilità e agilità all'amministratore di sistema.

## <a name="in-this-collection"></a>In questa raccolta
In questa raccolta sono disponibili le seguenti guide per la pianificazione, la distribuzione e la gestione di WSUS:

-   [Distribuire Windows Server Update Services](../deploy/deploy-windows-server-update-services.md)

-   [Gestire gli aggiornamenti con Windows Server Update Services](../manage/update-management-with-windows-server-update-services.md)


