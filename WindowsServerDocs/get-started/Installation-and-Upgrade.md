---
title: Installazione e aggiornamento di Windows Server
description: Come eseguire l'installazione, l'aggiornamento o la migrazione a una versione successiva di Windows Server.
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
ms.sourcegitcommit: 3743cf691a984e1d140a04d50924a3a0a19c3e5c
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 06/17/2019
ms.locfileid: "66443548"
---
# <a name="windows-server-installation-and-upgrade"></a>Installazione e aggiornamento di Windows Server

>Si applica a: Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Vuoi passare a Windows Server 2019? Vedi [Eseguire l'installazione, l'aggiornamento e la migrazione a Windows Server 2019](../get-started-19/install-upgrade-migrate-19.md).

> [!IMPORTANT]
> Il supporto esteso per Windows Server 2008 R2 e Windows Server 2008 terminerà a gennaio 2020. [Scopri le opzioni di aggiornamento](#upgrading-from-windows-server-2008-r2-or-windows-server-2008).

È arrivato il momento di passare a una versione più recente di Windows Server? A seconda della versione eseguita, hai a disposizione molte opzioni.

## <a name="installation"></a>Installazione

Se vuoi passare a una versione più recente di Windows Server senza cambiare l'hardware, un metodo che funziona sempre è l'**installazione pulita**, durante la quale il sistema operativo più recente viene installato direttamente su quello precedente nello stesso hardware, eliminando il sistema operativo precedente. Si tratta del metodo più semplice, ma prima di tutto devi eseguire il backup dei dati e pianificare la reinstallazione delle applicazioni. Esistono alcuni aspetti di cui devi tenere conto, ad esempio i requisiti di sistema, quindi assicurati di esaminare le informazioni relative a [Windows Server 2016](https://go.microsoft.com/fwlink/?LinkID=825558), [Windows Server 2012 R2](https://technet.microsoft.com/library/dn303418) e [Windows Server 2012](https://technet.microsoft.com/library/jj134246.aspx).

Il passaggio da una versione non definitiva, ad esempio Windows Server 2016 Technical Preview, alla versione rilasciata (Windows Server 2016) richiede sempre un'installazione pulita.

## <a name="migration-recommended-for-windows-server-2016"></a>Migrazione (scelta consigliata per Windows Server 2016)

La documentazione sulla migrazione di Windows Server permette di eseguire la migrazione di un ruolo o una funzionalità per volta da un computer di origine che esegue Windows Server a un altro computer di destinazione che esegue Windows Server, con la stessa versione o una più recente. A questo scopo, la migrazione viene definita come spostamento di un ruolo o di una funzionalità e dei rispettivi dati in un altro computer, non come aggiornamento della funzionalità nello stesso computer. Questo è il metodo consigliato per spostare il carico di lavoro e i dati esistenti in una versione più recente di Windows Server. Per iniziare, controlla la [matrice di aggiornamento e migrazione del ruolo del server](https://go.microsoft.com/fwlink/?LinkId=828595) per Windows Server.

## <a name="cluster-os-rolling-upgrade"></a>Aggiornamento in sequenza del sistema operativo del cluster
L'aggiornamento in sequenza del sistema operativo del cluster è una nuova funzionalità di Windows Server 2016 che permette a un amministratore di aggiornare il sistema operativo dei nodi del cluster da Windows Server 2012 R2 a Windows Server 2016 senza arrestare i carichi di lavoro di Hyper-V o del file server di scalabilità orizzontale. Questa funzionalità consente di evitare tempi di inattività che potrebbero influire sui contratti di servizio. La nuova funzionalità è illustrata in modo più dettagliato in [Aggiornamento in sequenza del sistema operativo del cluster](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade).

## <a name="license-conversion"></a>Conversione della licenza
In alcune versioni del sistema operativo puoi convertire una determinata edizione della versione in un'altra edizione della stessa versione con un'unica operazione, usando un semplice comando e il codice di licenza appropriato. Questo processo è definito **conversione della licenza**. Ad esempio, se il server esegue Windows Server 2016 Standard, puoi convertirlo in Windows Server 2016 Datacenter. In alcune versioni di Windows Server, puoi anche eseguire la conversione tra versioni OEM con contratto multilicenza e versioni definitive con lo stesso comando e il codice appropriato.

## <a name="upgrade"></a>Aggiornamento
Se vuoi mantenere lo stesso hardware e tutti i ruoli del server che hai configurato senza rendere flat il server, l'**aggiornamento** è una possibilità e può essere eseguito in molti modi. Nell'aggiornamento classico passerai da un sistema operativo precedente a una versione più recente, mantenendo impostazioni, ruoli del server e dati. Ad esempio, se nel server è in esecuzione Windows Server 2012 R2, puoi aggiornarlo a Windows Server 2016. Tuttavia, non per tutti i sistemi operativi precedenti esiste un percorso a ogni versione più recente.
 
>[!NOTE]
>L'aggiornamento funziona meglio in macchine virtuali in cui non sono necessari specifici driver hardware originali per l'aggiornamento.
 
È possibile effettuare l'aggiornamento da una versione di valutazione del sistema operativo a una versione definitiva, da una versione definitiva precedente a una più recente oppure, in alcuni casi, da un'edizione del sistema operativo con contratto multilicenza a una normale edizione definitiva.

Prima di iniziare a eseguire un aggiornamento, osserva le tabelle presenti in questa pagina per informazioni su come eseguire il processo tra le diverse versioni.

Per informazioni sulle differenze tra le opzioni di installazione disponibili per Windows Server 2016 Technical Preview, incluse le funzionalità installate con ogni opzione e le opzioni di gestione disponibili dopo l'installazione, vedi [Windows Server 2016](https://go.microsoft.com/fwlink/?LinkId=828598).

>[!NOTE]
>Ogni volta che esegui la migrazione o l'aggiornamento a qualsiasi versione di Windows Server, devi esaminare e comprendere i [criteri relativi al ciclo di vita del supporto](https://support.microsoft.com/lifecycle) e l'intervallo di tempo per ogni versione, per poi pianificare il processo di conseguenza. Puoi [cercare le informazioni sul ciclo di vita](https://support.microsoft.com/lifecycle) per la versione specifica di Windows Server che ti interessa.
 
 
## <a name="upgrading-to-windows-server-2016"></a>Aggiornamento a Windows Server 2016
Per informazioni dettagliate, inclusi avvisi importanti e limitazioni sull'aggiornamento, la conversione della licenza tra edizioni di Windows Server 2016 e la conversione delle edizioni di valutazione in edizioni definitive, vedi [Opzioni di aggiornamento e conversione per Windows Server 2016](https://go.microsoft.com/fwlink/?LinkId=828602).
 
>[!NOTE]
>Nota: Gli aggiornamenti che prevedono il passaggio da un'installazione Server Core a un'installazione Server con Esperienza desktop (e viceversa) non sono supportati. Se il sistema operativo precedente che stai aggiornando o convertendo è un'installazione dei componenti di base, il risultato resterà un'installazione dei componenti di base del sistema operativo più recente.
 
Tabella di riferimento rapido dei percorsi di aggiornamento supportati dalle edizioni definitive precedenti di Windows Server alle edizioni definitive di Windows Server 2016:


|Se esegui queste versioni ed edizioni:|Puoi eseguire l'aggiornamento a queste versioni ed edizioni:|
|--------------------------------|---------------------------------------|
|Windows Server 2012 Standard|Windows Server 2016 Standard o Datacenter|
|Windows Server 2012 Datacenter|Windows Server 2016 Datacenter|
|Windows Server 2012 R2 Standard|Windows Server 2016 Standard o Datacenter|
|Windows Server 2012 R2 Datacenter|Windows Server 2016 Datacenter|
|Hyper-V Server 2012 R2|Hyper-V Server 2016 (con funzionalità di aggiornamento in sequenza del sistema operativo del cluster)|
|Windows Server 2012 R2 Essentials|Windows Server 2016 Essentials|
|Windows Storage Server 2012 Standard|Windows Storage Server 2016 Standard|
|Windows Storage Server 2012 Workgroup|Windows Storage Server 2016 Workgroup|
|Windows Storage Server 2012 R2 Standard|Windows Storage Server 2016 Standard|
|Windows Storage Server 2012 R2 Workgroup|Windows Storage Server 2016 Workgroup|
 
### <a name="license-conversion"></a>Conversione della licenza
Puoi convertire Windows Server 2016 Standard (definitiva) in Windows Server 2016 Datacenter (definitiva).

Puoi convertire Windows Server 2016 Essentials (definitiva) in Windows Server 2016 Standard (definitiva).

È possibile convertire la versione di valutazione di Windows Server 2016 Standard a Windows Server 2016 Standard (definitiva) o Datacenter (definitiva).

Puoi convertire la versione di valutazione di Windows Server 2016 Datacenter nella versione definitiva.
 
## <a name="upgrading-to-windows-server-2012-r2"></a>Aggiornamento a Windows Server 2012 R2
Per informazioni dettagliate, inclusi avvisi importanti e limitazioni sull'aggiornamento, la conversione della licenza tra edizioni di Windows Server 2012 R2 e la conversione delle edizioni di valutazione in edizioni definitive, vedi [Opzioni di aggiornamento per Windows Server 2012 R2](https://technet.microsoft.com/library/dn303416.aspx).

Tabella di riferimento rapido dei percorsi di aggiornamento supportati dalle edizioni definitive precedenti di Windows Server alle edizioni definitive di Windows Server 2012 R2:

|Se si utilizza:|Edizioni a cui è possibile eseguire l'aggiornamento|
|-------------------------|---------------------------|
|Windows Server 2008 R2 Datacenter con SP1|Windows Server 2012 R2 Datacenter|
|Windows Server 2008 R2 Enterprise con SP1|Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter|
|Windows Server 2008 R2 Standard con SP1|Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter|
|Windows Web Server 2008 R2 con SP1|Windows Server 2012 R2 Standard|
|Windows Server 2012 Datacenter|Windows Server 2012 R2 Datacenter|
|Windows Server 2012 Standard|Windows Server 2012 R2 Standard o Windows Server 2012 R2 Datacenter|
|Hyper-V Server 2012|Hyper-V Server 2012 R2|

### <a name="license-conversion"></a>Conversione della licenza
Puoi convertire Windows Server 2012 Standard (definitiva) in Windows Server 2012 Datacenter (definitiva).

Puoi convertire Windows Server 2012 Essentials (definitiva) in Windows Server 2012 Standard (definitiva).

Puoi convertire la versione di valutazione di Windows Server 2012 Standard a Windows Server 2012 Standard (definitiva) o Datacenter (definitiva).

## <a name="upgrading-to-windows-server-2012"></a>Aggiornamento a Windows Server 2012
Per informazioni dettagliate, inclusi avvisi importanti e limitazioni sull'aggiornamento e la conversione delle edizioni di valutazione alle edizioni definitive, vedi [Versioni di valutazione e opzioni di aggiornamento di Windows Server 2012](https://technet.microsoft.com/library/jj574204.aspx).
 
Tabella di riferimento rapido dei percorsi di aggiornamento supportati dalle edizioni definitive precedenti di Windows Server alle edizioni definitive di Windows Server 2012:

|Se si utilizza:|Edizioni a cui è possibile eseguire l'aggiornamento|
|--------------------------|--------------------------|
|Windows Server 2008 Standard con SP2 o Windows Server 2008 Enterprise con SP2|Windows Server 2012 Standard, Windows Server 2012 Datacenter|
|Windows Server 2008 Datacenter con SP2|Windows Server 2012 Datacenter|
|Windows Web Server 2008|Windows Server 2012 Standard|
|Windows Server 2008 R2 Standard con SP1 o Windows Server 2008 R2 Enterprise con SP1|Windows Server 2012 Standard, Windows Server 2012 Datacenter|
|Windows Server 2008 R2 Datacenter con SP1|Windows Server 2012 Datacenter|
|Windows Web Server 2008 R2|Windows Server 2012 Standard|

### <a name="license-conversion"></a>Conversione della licenza
Puoi convertire Windows Server 2012 Standard (definitiva) in Windows Server 2012 Datacenter (definitiva).

Puoi convertire Windows Server 2012 Essentials (definitiva) in Windows Server 2012 Standard (definitiva).

Puoi convertire la versione di valutazione di Windows Server 2012 Standard a Windows Server 2012 Standard (definitiva) o Datacenter (definitiva).

## <a name="upgrading-from-windows-server-2008-r2-or-windows-server-2008"></a>Aggiornamento da Windows Server 2008 R2 o Windows Server 2008

Come descritto in [Eseguire l'aggiornamento di Windows Server 2008 e Windows Server 2008 R2](modernize-windows-server-2008.md), il supporto esteso per Windows Server 2008 R2/Windows Server 2008 terminerà a gennaio 2020. Per impedire eventuali interruzioni del supporto, devi eseguire l'aggiornamento a una versione supportata di Windows Server o riospitare il sistema in Azure passando a [macchine virtuali Windows Server 2008 R2 specializzate](uploading-specialized-WS08-image-to-azure.md). Per informazioni e considerazioni sulla pianificazione della migrazione o dell'aggiornamento, vedi la [guida alla migrazione per Windows Server](https://go.microsoft.com/fwlink/?linkid=872689).

Per i server locali, non esiste alcun percorso di aggiornamento diretto da Windows Server 2008 R2 a Windows Server 2016 o versioni successive. Devi invece eseguire l'aggiornamento prima a Windows Server 2012 R2 e quindi a [Windows Server 2016](#upgrading-to-windows-server-2016).

Nel pianificare l'aggiornamento, tieni conto delle linee guida seguenti per il passaggio intermedio di aggiornamento a Windows Server 2012 R2.

  - Non puoi eseguire un aggiornamento sul posto da un'architettura a 32 bit a una a 64 bit o da un tipo di compilazione a un altro (ad esempio, da fre a chk).

  - Gli aggiornamenti sul posto sono supportati solo nella stessa lingua. Non poi eseguire l'aggiornamento da una lingua a un'altra.

  - Puoi eseguire la migrazione da un'installazione dei componenti di base del server di Windows Server 2008 a Windows Server 2012 R2 con l'interfaccia utente grafica (denominata "Server con desktop completo" in Windows Server). Puoi passare dall'installazione dei componenti di base del server a Server con desktop completo, ma solo in Windows Server 2012 R2. Poiché Windows Server 2016 e versioni successive *non* supportano il passaggio dall'installazione dei componenti di base del server al desktop completo, esegui questo passaggio prima di eseguire l'aggiornamento a Windows Server 2016.
  
Per altre informazioni, inclusi i dettagli sugli aggiornamenti specifici dei ruoli, vedi [Versioni di valutazione e opzioni di aggiornamento per Windows Server 2012](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/jj574204\(v=ws.11\)).

