---
title: Installazione e aggiornamento di Windows Server
description: Come installare, aggiornare o eseguire la migrazione a una versione più recente di Windows Server.
ms.prod: windows-server
ms.date: 05/14/2019
ms.technology: server-general
ms.topic: article
ms.assetid: 98f876bd-63ff-4c3a-95d4-a8dd8d0d119c
author: jasongerend
ms.author: jgerend
manager: dougkim
ms.localizationpriority: medium
ms.openlocfilehash: 140f67a9dab5cf1f10cdb0c5c51a031a0dfb9dd3
ms.sourcegitcommit: eaf071249b6eb6b1a758b38579a2d87710abfb54
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/31/2019
ms.locfileid: "66443548"
---
# <a name="windows-server-installation-and-upgrade"></a>Aggiornamento e installazione di Windows Server

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Alla ricerca di Windows Server 2019? Visualizzare [installare, aggiornare o eseguire la migrazione a Windows Server 2019](../get-started-19/install-upgrade-migrate-19.md).

> [!IMPORTANT]
> Il supporto esteso per Windows Server 2008 R2 e Windows Server 2008 termina nel gennaio 2020. [Scopri le opzioni di aggiornamento](#upgrading-from-windows-server-2008-r2-or-windows-server-2008).

È arrivato il momento di passare a una versione più recente di Windows Server? Le opzioni per accedervi variano a seconda della versione in uso.

## <a name="installation"></a>Installazione

Se desideri passare a una versione più recente di Windows Server utilizzando lo stesso hardware, un metodo che funziona sempre è l'**installazione pulita**, durante la quale il sistema operativo più recente viene installato direttamente sull'hardware precedente, eliminando il sistema operativo precedente. Si tratta del modo più semplice, ma è necessario eseguire il backup dei dati innanzitutto e pianificare di reinstallare le applicazioni. Esistono alcuni aspetti da tenere in considerazione, ad esempio i requisiti di sistema, quindi assicurati di controllare i dettagli di [Windows Server 2016](https://go.microsoft.com/fwlink/?LinkID=825558), [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418) e [Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).

Il passaggio da una versione non definitiva (ad esempio, Windows Server 2016 Technical Preview) alla versione rilasciata (Windows Server 2016) sempre richiede un'installazione pulita.

## <a name="migration-recommended-for-windows-server-2016"></a>Migrazione (scelta consigliata per Windows Server 2016)

Documentazione sulla migrazione di Windows Server consente di eseguire la migrazione di un ruolo o funzionalità alla volta da un computer di origine che esegue Windows Server a un altro computer di destinazione che esegue Windows Server, con la stessa versione o una versione più recente. A tali fini, la migrazione viene definita come lo spostamento di un ruolo o funzionalità e i relativi dati in un altro computer, senza aggiornare la funzionalità nello stesso computer. Questo è il modo consigliato per passare il carico di lavoro e i dati esistenti a una versione più recente di Windows Server. Per iniziare, esaminare i [matrice di aggiornamento e migrazione del ruolo server](https://go.microsoft.com/fwlink/?LinkId=828595) per Windows Server.

## <a name="cluster-os-rolling-upgrade"></a>Aggiornamento in sequenza del sistema operativo del cluster
L'aggiornamento in sequenza del sistema operativo del cluster è una nuova funzionalità di Windows Server 2016 che consente a un amministratore di aggiornare il sistema operativo dei nodi del cluster da Windows Server 2012 R2 a Windows Server 2016 senza interrompere i carichi di lavoro del file server di scalabilità orizzontale o di Hyper-V. Questa funzionalità consente di evitare tempi di inattività che potrebbero influire sui contratti di servizio. La nuova funzionalità è illustrata in modo più dettagliato in [Aggiornamento in sequenza del sistema operativo del cluster](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade).

## <a name="license-conversion"></a>Conversione della licenza
La conversione delle licenze in alcune versioni di sistemi operativi consente di convertire una particolare edizione della versione in un'altra edizione della stessa versione con un'unica operazione, usando un semplice comando e il codice di licenza appropriato. Questo processo è definito **conversione della licenza**. Se ad esempio il server esegue Windows Server 2016 Standard, è possibile convertirlo in Windows Server 2016 Datacenter. In alcune versioni di Windows Server, è anche possibile eseguire la conversione tra versioni OEM, con contratto multilicenza e versioni definite con lo stesso comando e il codice appropriato.

## <a name="upgrade"></a>Aggiornamento
Se desideri mantenere lo stesso hardware e tutti i ruoli del server che hai configurato senza rendere flat il server, l'**aggiornamento** è una possibilità e può essere eseguito in tanti modi. Nell'aggiornamento classico, si passa da un sistema operativo precedente a una versione più recente, mantenendo intatti impostazioni, ruoli del server e dati. Ad esempio, se nel server è in esecuzione Windows Server 2012 R2, è possibile aggiornarlo a Windows Server 2016. Tuttavia, non tutti i sistemi operativi precedenti hanno un percorso alla versione più recente.
 
>[!NOTE]
>L'aggiornamento funziona meglio in macchine virtuali in cui non sono necessari specifici driver hardware originali per l'aggiornamento.
 
È possibile effettuare l'aggiornamento da una versione di valutazione del sistema operativo a una versione definitiva, da una versione definitiva precedente a una più recente oppure, in alcuni casi, da un'edizione del sistema operativo con contratto multilicenza a una normale edizione definitiva.

Prima di iniziare a eseguire un aggiornamento, osserva le tabelle riportate in questa pagina per informazioni su come eseguire il processo.

Per informazioni sulle differenze tra le opzioni di installazione disponibili per Windows Server 2016 Technical Preview, incluse le funzionalità installate con ciascuna opzione e le opzioni di gestione disponibili dopo l'installazione, vedere [Windows Server 2016](https://go.microsoft.com/fwlink/?LinkId=828598).

>[!NOTE]
>Ogni volta che si esegue la migrazione o l'aggiornamento a una qualsiasi versione di Windows Server, è necessario rivedere e comprendere i [criteri relativi al ciclo di vita del supporto](https://support.microsoft.com/lifecycle) e l'intervallo di tempo per tale versione e pianificare di conseguenza. È possibile [cercare le informazioni sul ciclo di vita](https://support.microsoft.com/lifecycle) per la versione specifica di Windows Server a cui si è interessati.
 
 
## <a name="upgrading-to-windows-server-2016"></a>Aggiornamento a Windows Server 2016
Per informazioni dettagliate, inclusi avvisi importanti e limitazioni sull'aggiornamento, la conversione della licenza tra le varie versioni di Windows Server 2016 e la conversione delle edizioni di valutazione in edizioni finali, vedere [Opzioni di aggiornamento e conversione per Windows Server 2016](https://go.microsoft.com/fwlink/?LinkId=828602).
 
>[!NOTE]
>Nota: Gli aggiornamenti che prevedono il passaggio da un'installazione Server Core a un'installazione Server con Esperienza desktop (e viceversa) non sono supportati. Se il sistema operativo precedente a cui si esegue l'aggiornamento o la conversione è un'installazione Server Core, il risultato sarà comunque un'installazione Server Core del sistema operativo più recente.
 
Tabella di riferimento rapido dei percorsi di aggiornamento supportati dalle edizioni finali precedenti di Windows Server alle edizioni finali di Windows Server 2016:


|Edizioni eseguite:|Edizioni e versioni a cui è possibile eseguire l'aggiornamento:|
|--------------------------------|---------------------------------------|
|Windows Server 2012 Standard|Windows Server 2016 Standard o Datacenter|
|Windows Server 2012 Datacenter|Windows Server 2016 Datacenter|
|Windows Server 2012 R2 Standard|Windows Server 2016 Standard o Datacenter|
|Windows Server 2012 R2 Datacenter|Windows Server 2016 Datacenter|
|Hyper-V Server 2012 R2|Hyper-V Server 2016 (con funzionalità di aggiornamento in sequenza del sistema operativo del cluster)|
|Windows Server 2012 R2 Essentials|Windows Server 2016 Essentials|
|Windows Storage Server 2012 Standard|Windows Storage Server 2016 Standard|
|Windows Storage Server 2012 Workgroup|Windows Storage Server 2016 Workgroup|
|Windows Storage Server 2012 R2 Standard|Windows Storage Server 2016 Standard|
|Windows Storage Server 2012 R2 Workgroup|Windows Storage Server 2016 Workgroup|
 
### <a name="license-conversion"></a>Conversione della licenza
È possibile convertire Windows Server 2016 Standard (definitiva) in Windows Server 2016 Datacenter (definitiva).

È possibile convertire Windows Server 2016 Essentials (definitiva) in Windows Server 2016 Standard (definitiva).

È possibile convertire la versione di valutazione di Windows Server 2016 Standard a Windows Server 2016 Standard (definitiva) o Datacenter (definitiva).

È possibile convertire la versione di valutazione di Windows Server 2016 Datacenter alla versione definitiva.
 
## <a name="upgrading-to-windows-server-2012-r2"></a>Aggiornamento a Windows Server 2012 R2
Per informazioni dettagliate, inclusi avvisi importanti e limitazioni sull'aggiornamento, la conversione della licenza tra le varie versioni di Windows Server 2012 R2 e la conversione delle edizioni di valutazione in edizioni finali, vedere [Opzioni di aggiornamento per Windows Server 2012 R2](https://technet.microsoft.com/library/dn303416.aspx).

Tabella di riferimento rapido dei percorsi di aggiornamento supportati dalle edizioni finali precedenti di Windows Server alle edizioni finali di Windows Server 2012 R2:

|Se si utilizza:|Edizioni a cui è possibile eseguire l'aggiornamento|
|-------------------------|---------------------------|
|Windows Server 2008 R2 Datacenter con SP1|Windows Server 2012 R2 Datacenter|
|Windows Server 2008 R2 Enterprise con SP1|Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter|
|Windows Server 2008 R2 Standard con SP1|Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter|
|Windows Web Server 2008 R2 con SP1|Windows Server 2012 R2 Standard|
|Windows Server 2012 Datacenter|Windows Server 2012 R2 Datacenter|
|Windows Server 2012 Standard|Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter|
|Hyper-V Server 2012|Hyper-V Server 2012 R2|

### <a name="license-conversion"></a>Conversione della licenza
È possibile convertire Windows Server 2012 Standard (definitiva) in Windows Server 2012 Datacenter (definitiva).

È possibile convertire Windows Server 2012 Essentials (definitiva) a Windows Server 2012 Standard (definitiva).

È possibile convertire la versione di valutazione di Windows Server 2012 Standard a Windows Server 2012 Standard (definitiva) o Datacenter (definitiva).

## <a name="upgrading-to-windows-server-2012"></a>Aggiornamento a Windows Server 2012
Per informazioni dettagliate, inclusi avvisi importanti e limitazioni sull'aggiornamento e la conversione delle edizioni di valutazione a edizioni finali, vedere [Versioni di prova e opzioni di aggiornamento di Windows Server 2012](https://technet.microsoft.com/library/jj574204.aspx).
 
Tabella di riferimento rapido dei percorsi di aggiornamento supportati dalle edizioni finali precedenti di Windows Server alle edizioni finali di Windows Server 2012:

|Se si utilizza:|Edizioni a cui è possibile eseguire l'aggiornamento|
|--------------------------|--------------------------|
|Windows Server 2008 Standard con SP2 o Windows Server 2008 Enterprise con SP2|Windows Server 2012 Standard, Windows Server 2012 Datacenter|
|Windows Server 2008 Datacenter con SP2|Windows Server 2012 Datacenter|
|Windows Web Server 2008|Windows Server 2012 Standard|
|Windows Server 2008 R2 Standard con SP1 o Windows Server 2008 R2 Enterprise con SP1|Windows Server 2012 Standard, Windows Server 2012 Datacenter|
|Windows Server 2008 R2 Datacenter con SP1|Windows Server 2012 Datacenter|
|Windows Web Server 2008 R2|Windows Server 2012 Standard|

### <a name="license-conversion"></a>Conversione della licenza
È possibile convertire Windows Server 2012 Standard (definitiva) in Windows Server 2012 Datacenter (definitiva).

È possibile convertire Windows Server 2012 Essentials (definitiva) a Windows Server 2012 Standard (definitiva).

È possibile convertire la versione di valutazione di Windows Server 2012 Standard a Windows Server 2012 Standard (definitiva) o Datacenter (definitiva).

## <a name="upgrading-from-windows-server-2008-r2-or-windows-server-2008"></a>L'aggiornamento da Windows Server 2008 R2 o Windows Server 2008

Come descritto in [esegue l'aggiornamento di Windows Server 2008 e Windows Server 2008 R2](modernize-windows-server-2008.md), termina il supporto esteso per Windows Server 2008 R2 e Windows Server 2008 nel gennaio del 2020. Per assicurarsi che nessun gap in modalità di supporto, è necessario eseguire l'aggiornamento a una versione supportata di Windows Server o rehosting in Azure grazie al passaggio a [specializzato macchine virtuali di Windows Server 2008 R2](uploading-specialized-WS08-image-to-azure.md). Consultare il [Guida alla migrazione per Windows Server](https://go.microsoft.com/fwlink/?linkid=872689) per informazioni e considerazioni per la pianificazione della migrazione/aggiornamento.

Per i server in locale, non vi è alcun percorso di aggiornamento diretto da Windows Server 2008 R2 a Windows Server 2016 o versioni successive. Al contrario, aggiornare prima di Windows Server 2012 R2 e quindi [eseguire l'aggiornamento a Windows Server 2016](#upgrading-to-windows-server-2016).

Come si intende l'aggiornamento, tenere presente le linee guida seguenti per il passaggio intermedio di eseguire l'aggiornamento a Windows Server 2012 R2.

  - Non è possibile eseguire un aggiornamento sul posto da un architetture a 32 bit a 64 bit o da un tipo di compilazione per un'altra (da fre a chk, ad esempio).

  - Gli aggiornamenti sul posto sono supportati solo nella stessa lingua. È possibile eseguire l'aggiornamento da una lingua a altra.

  - È possibile eseguire la migrazione da un'installazione server core di Windows Server 2008 a Windows Server 2012 R2 con l'interfaccia utente grafica di Server (denominata "Server con Desktop completo" di Windows Server). L'installazione di core server aggiornato è possibile passare al Server con il Desktop completo, ma solo in Windows Server 2012 R2. Windows Server 2016 e versioni successive *non li* supportano il passaggio dalla base del server al Desktop completo, pertanto è necessario tale commutatore prima di eseguire l'aggiornamento a Windows Server 2016.
  
Per altre informazioni, consultare [versioni di valutazione e opzioni di aggiornamento per Windows Server 2012](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj574204\(v=ws.11\)), che include i dettagli di aggiornamento specifici per il ruolo.

