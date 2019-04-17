---
title: Installazione e aggiornamento di Windows Server
description: ''
ms.custom: na
ms.prod: windows-server
ms.reviewer: na
ms.suite: na
ms.date: 07/12/2018
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 98f876bd-63ff-4c3a-95d4-a8dd8d0d119c
author: jaimeo
ms.author: jaimeo
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: c3b9070fc6cb9227ccfa445e23983d9e91fe5c82
ms.sourcegitcommit: 07ac08dea2b8f2763c2614a999dc7967018aa0b4
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 11/07/2018
ms.locfileid: "6121480"
---
# Installazione e aggiornamento di Windows Server

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

> [!IMPORTANT]
> Il supporto esteso per Windows Server 2008 R2 e Windows Server 2008 termina nel gennaio 2020. [Informazioni sulle opzioni di aggiornamento](#upgrading-from-windows-server-2008-r2-or-windows-server-2008).

È arrivato il momento di passare a una versione più recente di Windows Server? Le opzioni per accedervi variano a seconda della versione in uso.

## Installazione
Se desideri passare a una versione più recente di Windows Server utilizzando lo stesso hardware, un metodo che funziona sempre è l'**installazione pulita**, durante la quale il sistema operativo più recente viene installato direttamente sull'hardware precedente, eliminando il sistema operativo precedente. Si tratta del modo più semplice, ma è necessario eseguire il backup dei dati innanzitutto e pianificare di reinstallare le applicazioni. Esistono alcuni aspetti da tenere in considerazione, ad esempio i requisiti di sistema, quindi assicurati di controllare i dettagli di [Windows Server 2016](https://go.microsoft.com/fwlink/?LinkID=825558), [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418) e [Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).

Il passaggio da una versione non definitiva (ad esempio, Windows Server 2016 Technical Preview) alla versione rilasciata (Windows Server 2016) sempre richiede un'installazione pulita.

## Migrazione (scelta consigliata per Windows Server 2016)

La documentazione di Windows Server [migrazione] consente di eseguire la migrazione di un ruolo o funzionalità per volta da un computer di origine che esegue Windows Server a un altro computer di destinazione che esegue Windows Server, lo stesso o una versione più recente. A tali fini, la migrazione viene definita come lo spostamento di un ruolo o funzionalità e i relativi dati in un altro computer, senza aggiornare la funzionalità nello stesso computer. Questo è il modo consigliato per passare il carico di lavoro e i dati esistenti a una versione più recente di Windows Server. Per iniziare, controlla la [matrice di aggiornamento e migrazione del ruolo del server](https://go.microsoft.com/fwlink/?LinkId=828595) per Windows Server 2016.

## Aggiornamento in sequenza del sistema operativo del cluster
L'aggiornamento in sequenza del sistema operativo del cluster è una nuova funzionalità di Windows Server 2016 che consente a un amministratore di aggiornare il sistema operativo dei nodi del cluster da Windows Server 2012 R2 a Windows Server 2016 senza interrompere i carichi di lavoro del file server di scalabilità orizzontale o di Hyper-V. Questa funzionalità consente di evitare tempi di inattività che potrebbero influire sui contratti di servizio. La nuova funzionalità è illustrata in modo più dettagliato in [Aggiornamento in sequenza del sistema operativo del cluster](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade).

## Conversione della licenza
La conversione delle licenze in alcune versioni di sistemi operativi consente di convertire una particolare edizione della versione in un'altra edizione della stessa versione con un'unica operazione, usando un semplice comando e il codice di licenza appropriato. Questo processo è definito **conversione della licenza**. Se ad esempio il server esegue Windows Server 2016 Standard, è possibile convertirlo in Windows Server 2016 Datacenter. In alcune versioni di Windows Server, è anche possibile eseguire la conversione tra versioni OEM, con contratto multilicenza e versioni definite con lo stesso comando e il codice appropriato.

## Aggiornare
Se desideri mantenere lo stesso hardware e tutti i ruoli del server che hai configurato senza rendere flat il server, l'**aggiornamento** è una possibilità e può essere eseguito in tanti modi. Nell'aggiornamento classico, si passa da un sistema operativo precedente a una versione più recente, mantenendo intatti impostazioni, ruoli del server e dati. Ad esempio, se nel server è in esecuzione Windows Server 2012 R2, è possibile aggiornarlo a Windows Server 2016. Tuttavia, non tutti i sistemi operativi precedenti hanno un percorso alla versione più recente.
 
>[!NOTE]
>L'aggiornamento funziona al meglio nelle macchine virtuali in cui non sono necessari specifici driver hardware OEM per l'aggiornamento.
 
È possibile effettuare l'aggiornamento da una versione di valutazione del sistema operativo a una versione definitiva, da una versione definitiva precedente a una più recente oppure, in alcuni casi, da un'edizione del sistema operativo con contratto multilicenza a una normale edizione definitiva.

Prima di iniziare a eseguire un aggiornamento, osserva le tabelle riportate in questa pagina per informazioni su come eseguire il processo.

Per informazioni sulle differenze tra le opzioni di installazione disponibili per Windows Server 2016 Technical Preview, incluse le funzionalità installate con ciascuna opzione e le opzioni di gestione disponibili dopo l'installazione, vedere [Windows Server 2016](https://go.microsoft.com/fwlink/?LinkId=828598).

>[!NOTE]
>Ogni volta che si esegue la migrazione o l'aggiornamento a una qualsiasi versione di Windows Server, è necessario rivedere e comprendere i [criteri relativi al ciclo di vita del supporto](https://support.microsoft.com/lifecycle) e l'intervallo di tempo per tale versione e pianificare di conseguenza. È possibile [cercare le informazioni sul ciclo di vita](https://support.microsoft.com/lifecycle) per la versione specifica di Windows Server a cui si è interessati.
 
 
## Aggiornamento a Windows Server 2016
Per informazioni dettagliate, inclusi avvisi importanti e limitazioni sull'aggiornamento, la conversione della licenza tra le varie versioni di Windows Server 2016 e la conversione delle edizioni di valutazione in edizioni finali, vedere [Opzioni di aggiornamento e conversione per Windows Server 2016](https://go.microsoft.com/fwlink/?LinkId=828602).
 
>[!NOTE]
>Nota: gli aggiornamenti che prevedono il passaggio da un'installazione Server Core a un'installazione Server con Esperienza desktop (o viceversa) non sono supportati. Se il sistema operativo precedente a cui si esegue l'aggiornamento o la conversione è un'installazione Server Core, il risultato sarà comunque un'installazione Server Core del sistema operativo più recente.
 
Tabella di riferimento rapido dei percorsi di aggiornamento supportati dalle edizioni finali precedenti di Windows Server alle edizioni finali di Windows Server 2016:


|Edizioni eseguite:|Edizioni e versioni a cui è possibile eseguire l'aggiornamento:|
|--------------------------------|---------------------------------------|
|Windows Server 2012 Standard|Windows Server 2016 Standard o Datacenter|
|Windows Server 2012 Datacenter|Windows Server 2016 Datacenter|
|Windows Server 2012 R2 Standard|Windows Server 2016 Standard o Datacenter|
|Windows Server 2012 R2 Datacenter|Windows Server 2016 Datacenter|
|Hyper-V Server 2012 R2|Hyper-V Server 2016 (con funzionalità di aggiornamento in sequenza del sistema operativo del cluster)|
|Windows Server 2012 R2 Essentials|Windows Server 2016 Essentials|
|Windows Storage Server 2012 Standard|Windows Storage Server 2016 Standard|
|Windows Storage Server 2012 Workgroup|Windows Storage Server 2016 Workgroup|
|Windows Storage Server 2012 R2 Standard|Windows Storage Server 2016 Standard|
|Windows Storage Server 2012 R2 Workgroup|Windows Storage Server 2016 Workgroup|
 
### Conversione della licenza
È possibile convertire Windows Server 2016 Standard (definitiva) in Windows Server 2016 Datacenter (definitiva).

È possibile convertire Windows Server 2016 Essentials (definitiva) in Windows Server 2016 Standard (definitiva).

È possibile convertire la versione di valutazione di Windows Server 2016 Standard a Windows Server 2016 Standard (definitiva) o Datacenter (definitiva).

È possibile convertire la versione di valutazione di Windows Server 2016 Datacenter alla versione definitiva.
 
## Aggiornamento a Windows Server 2012 R2
Per informazioni dettagliate, inclusi avvisi importanti e limitazioni sull'aggiornamento, la conversione della licenza tra le varie versioni di Windows Server 2012 R2 e la conversione delle edizioni di valutazione in edizioni finali, vedere [Opzioni di aggiornamento per Windows Server 2012 R2](https://technet.microsoft.com/library/dn303416.aspx).

Tabella di riferimento rapido dei percorsi di aggiornamento supportati dalle edizioni finali precedenti di Windows Server alle edizioni finali di Windows Server 2012 R2:

|Edizione utilizzata:|Edizioni a cui è possibile eseguire l'aggiornamento|
|-------------------------|---------------------------|
|Windows Server2008R2 Datacenter con SP1|WindowsServer 2012 R2 Datacenter|
|Windows Server2008R2 Enterprise con SP1|Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter|
|Windows Server2008R2 Standard con SP1|Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter|
|Windows Web Server2008R2 con SP1|Windows Server 2012 R2 Standard|
|WindowsServer 2012 Datacenter|Windows Server 2012 R2 Datacenter|
|Windows Server 2012 Standard|Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter|
|Hyper-V Server 2012|Hyper-V Server 2012 R2|

### Conversione della licenza
È possibile convertire Windows Server 2012 Standard (definitiva) in Windows Server 2012 Datacenter (definitiva).

È possibile convertire Windows Server 2012 Essentials (definitiva) a Windows Server 2012 Standard (definitiva).

È possibile convertire la versione di valutazione di Windows Server 2012 Standard a Windows Server 2012 Standard (definitiva) o Datacenter (definitiva).

## Aggiornamento a Windows Server 2012
Per informazioni dettagliate, inclusi avvisi importanti e limitazioni sull'aggiornamento e la conversione delle edizioni di valutazione a edizioni finali, vedere [Versioni di prova e opzioni di aggiornamento di Windows Server 2012](https://technet.microsoft.com/library/jj574204.aspx).
 
Tabella di riferimento rapido dei percorsi di aggiornamento supportati dalle edizioni finali precedenti di Windows Server alle edizioni finali di Windows Server 2012:

|Edizione eseguita:|Edizioni a cui è possibile eseguire l'aggiornamento|
|--------------------------|--------------------------|
|Windows Server 2008 Standard con SP2 o Windows Server 2008 Enterprise con SP2|Windows Server 2012 Standard, Windows Server 2012 Datacenter|
|Windows Server 2008 Datacenter con SP2|WindowsServer 2012 Datacenter|
|Windows Web Server 2008|Windows Server 2012 Standard|
|Windows Server2008R2 Standard con SP1 o Windows Server2008R2 Enterprise con SP1|Windows Server 2012 Standard, Windows Server 2012 Datacenter|
|Windows Server2008R2 Datacenter con SP1|WindowsServer 2012 Datacenter|
|Windows Web Server2008R2|Windows Server 2012 Standard|

### Conversione della licenza
È possibile convertire Windows Server 2012 Standard (definitiva) in Windows Server 2012 Datacenter (definitiva).

È possibile convertire Windows Server 2012 Essentials (definitiva) a Windows Server 2012 Standard (definitiva).

È possibile convertire la versione di valutazione di Windows Server 2012 Standard a Windows Server 2012 Standard (definitiva) o Datacenter (definitiva).

## L'aggiornamento da Windows Server 2008 R2 o Windows Server 2008

Come descritto [nell'aggiornamento di Windows Server 2008 e Windows Server 2008 R2](modernize-windows-server-2008.md), il supporto esteso per Windows Server 2008 R2 e Windows Server 2008 termina nel gennaio del 2020. Per garantire che nessuna distanza del supporto, è necessario eseguire l'aggiornamento a una versione supportata di Windows Server o il rehosting in Azure spostando alle [Macchine virtuali specializzata Windows Server 2008 R2](uploading-specialized-WS08-image-to-azure.md). Consulta la [Guida alla migrazione per Windows Server](https://go.microsoft.com/fwlink/?linkid=872689) per informazioni e considerazioni per la pianificazione dell'aggiornamento o migrazione.

Per i server in locale, non esiste alcun percorso di aggiornamento diretto da Windows Server 2008 R2 a Windows Server 2016 o versione successiva. Al contrario, eseguire l'aggiornamento prima di tutto in Windows Server 2012 R2 e quindi [eseguire l'aggiornamento a Windows Server 2016](#Upgrading-to-Windows-Server-2016).

Come si pianifica l'aggiornamento, tenere presente le seguenti linee guida per il passaggio centrale dell'aggiornamento a Windows Server 2012 R2.

  - Non puoi eseguire un aggiornamento sul posto da un'architetture a 32 bit a 64 bit o da un tipo di generazione per un altro ((francese) per chk, ad esempio).

  - Gli aggiornamenti sul posto sono supportati solo nella stessa lingua. Non è possibile eseguire l'aggiornamento da una lingua a un'altra.

  - È possibile eseguire la migrazione da un'installazione di core server Windows Server 2008 a Windows Server 2012 R2 con l'interfaccia utente grafica Server (noto come "Server con Desktop completa" in Windows Server). È possibile passare l'installazione di core aggiornato server a Server con Desktop completa, ma solo in Windows Server 2012 R2. Windows Server 2016 e versioni successive supportano *non* cambiare server core per Desktop completa, assicurati che commutatore prima dell'aggiornamento a Windows Server 2016.
  
Per altre informazioni, consulta [le versioni di valutazione e opzioni di aggiornamento per Windows Server 2012](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj574204\(v=ws.11\)), che include i dettagli dell'aggiornamento specifiche per il ruolo.

