---
title: Informazioni generali su Spazi dei nomi DFS
ms.prod: windows-server
ms.author: jgerend
manager: daveba
ms.technology: storage
ms.topic: article
author: jasongerend
ms.date: 06/07/2019
description: Questo articolo descrive Spazi dei nomi DFS, ovvero un servizio ruolo di Windows Server che consente di raggruppare le cartelle condivise situate in server diversi in uno o più spazi dei nomi strutturati logicamente.
ms.openlocfilehash: 07f6ac857164257810b297f9e2b83db4e4bd42be
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80858984"
---
# <a name="dfs-namespaces-overview"></a>Informazioni generali su Spazi dei nomi DFS

> Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008, Windows Server (canale semestrale)

Spazi dei nomi DFS è un servizio ruolo di Windows Server che consente di raggruppare le cartelle condivise situate in server diversi in uno o più spazi dei nomi strutturati logicamente. In questo modo gli utenti hanno a disposizione una vista virtuale di cartelle condivise in cui file situati in più server sono accessibili da un unico percorso, come illustrato nella figura riportata di seguito:

![Elementi della tecnologia degli spazi dei nomi DFS](media/dfs-overview.png)

Di seguito viene riportata una descrizione degli elementi che costituiscono uno spazio dei nomi DFS:

- **Server dello spazio dei nomi**: un server dello spazio dei nomi ospita uno spazio dei nomi. Il server dello spazio dei nomi può essere un server membro o un controller di dominio.
- **Radice dello spazio dei nomi**: la radice dello spazio dei nomi è il punto di partenza dello spazio dei nomi. Nella figura precedente, il nome della radice è Public e il percorso dello spazio dei nomi è \\\\contoso\\public. Questo tipo di spazio dei nomi è uno spazio dei nomi basato su dominio poiché inizia con un nome di dominio, ad esempio contoso, e i relativi metadati vengono archiviati in Active Directory Domain Services (AD DS). Sebbene nella figura precedente venga illustrato un unico server dello spazio dei nomi, uno spazio dei nomi basato sul dominio può essere ospitato in più server dello spazio dei nomi per aumentare la disponibilità dello spazio dei nomi.
- **Cartella**: le cartelle senza destinazioni cartella aggiungono struttura e gerarchia allo spazio dei nomi e le cartelle con destinazioni cartella forniscono agli utenti contenuto effettivo. Quando gli utenti accedono a una cartella con destinazioni cartella nello spazio dei nomi, il computer client riceve un riferimento che lo reindirizza in modo trasparente a una delle destinazioni cartella.
- **Destinazioni cartella**: una destinazione cartella è il percorso UNC (Universal Naming Convention) di una cartella condivisa o un altro spazio dei nomi associato a una cartella in uno spazio dei nomi. La destinazione cartella è l'ubicazione dove vengono archiviati dati e contenuto. Nella figura precedente, la cartella denominata Strumenti ha due destinazioni cartella, una a Londra e una a New York, e la cartella denominata Guide di formazione ha un'unica destinazione cartella in New York. Un utente che accede a \\\\contoso\\public\\software\\Tools viene reindirizzato in modo trasparente alla cartella condivisa \\\\LDN-SVR-01\\Tools o \\\\NYC-SVR-01\\Tools, a seconda del sito in cui si trova attualmente l'utente.

In questo argomento viene illustrato come installare il DFS, le novità e dove trovare le informazioni di valutazione e distribuzione.

È possibile amministrare gli spazi dei nomi mediante Gestione DFS, i [cmdlet DFS Namespace (DFSN) in Windows PowerShell](https://docs.microsoft.com/powershell/module/dfsn/?view=win10-ps), il comando **DfsUtil** o script che chiamano WMI.

## <a name="server-requirements-and-limits"></a>Requisiti e limiti del server

Per l'esecuzione di Gestione DFS o l'utilizzo di Spazi dei nomi DFS, non sono previsti altri requisiti hardware o software.

Un server dello spazio dei nomi è un controller di dominio o server membro che ospita uno spazio dei nomi. Il numero di spazi dei nomi che è possibile ospitare in un server è determinato dal sistema operativo in esecuzione sul server dello spazio dei nomi.

I server che eseguono i seguenti sistemi operativi possono ospitare più spazi dei nomi basati sul dominio oltre a un singolo spazio dei nomi autonomo. 

- Windows Server 2019
- Windows Server 2016
- Windows Server 2012 R2
- Windows Server 2012
- Windows Server 2008 R2 Datacenter ed Enterprise Edition
- Windows Server (Canale semestrale)

I server che eseguono i seguenti sistemi operativi possono ospitare un singolo spazio dei nomi autonomo:

- Windows Server 2008 R2 Standard

La tabella seguente descrive i fattori aggiuntivi da considerare nella scelta dei server per ospitare uno spazio dei nomi.

| Server che ospita spazi dei nomi autonomi | Server che ospita spazi dei nomi basati sul dominio |
| ---                                   |        ---                                |
| Deve contenere un volume NTFS per ospitare lo spazio dei nomi.|Deve contenere un volume NTFS per ospitare lo spazio dei nomi. |
| Può essere un server membro o controller di dominio.|Deve essere un server membro o controller di dominio nel dominio in cui è configurato lo spazio dei nomi. Questo requisito si applica a ogni server dello spazio dei nomi che ospita un determinato spazio dei nomi basato sul dominio. |
| Può essere ospitato da un cluster di failover per aumentare la disponibilità dello spazio dei nomi.|Lo spazio dei nomi non può essere una risorsa in cluster in un cluster di failover. Tuttavia, è possibile individuare lo spazio dei nomi in un server che funge anche da nodo in un cluster di failover se si configura lo spazio dei nomi solo per l'utilizzo di risorse locali su tale server. |

## <a name="installing-dfs-namespaces"></a>Installazione di Spazi dei nomi DFS

Spazi dei nomi DFS e Replica DFS fanno parte del ruolo Servizi file e archiviazione. Gli strumenti di gestione per il file system DFS (Gestione DFS, il modulo Spazi dei nomi DFS per Windows PowerShell e gli strumenti da riga di comando) vengono installati separatamente come parte degli Strumenti di amministrazione remota del server.

Installare spazi dei nomi DFS usando l'interfaccia di [amministrazione di Windows](../../manage/windows-admin-center/understand/windows-admin-center.md), Server Manager o PowerShell, come descritto nelle sezioni successive.

### <a name="to-install-dfs-by-using-server-manager"></a>Per installare il file system DFS tramite Server Manager

1. Aprire Server Manager, fare clic su **Gestione** e quindi su **Aggiungi ruoli e funzionalità**. Verrà visualizzata l'Aggiunta guidata ruoli e funzionalità.

2. Nella pagina **Selezione dei server** selezionare il server o il disco rigido virtuale (VHD) di una macchina virtuale offline in cui si desidera installare il file system DFS.

3. Selezionare i servizi ruolo e le funzionalità che si desidera installare.

    - Per installare il servizio Spazi dei nomi DFS, nella pagina **Ruoli server** selezionare **Spazi dei nomi DFS**.

    - Per installare solo gli Strumenti di gestione DFS, nella pagina **Funzionalità** espandere **Strumenti di amministrazione remota del server**, **Strumenti di amministrazione ruoli**, **Strumenti per Servizi file** e quindi selezionare **Strumenti di gestione DFS**.

         Tramite**Strumenti di gestione DFS** vengono installati lo snap-in Gestione DFS, il modulo Spazi dei nomi DFS per Windows PowerShell e gli strumenti da riga di comando, ma non viene installato alcun servizio DFS nel server.

### <a name="to-install-dfs-by-using-windows-powershell"></a>Per installare il file system DFS tramite Windows PowerShell

Apri una sessione di Windows PowerShell con diritti utente elevati e quindi digita il comando seguente, in cui <name\> è il servizio ruolo o la funzionalità che vuoi installare (vedi la tabella seguente per un elenco di nomi di servizi ruolo o funzionalità rilevanti):

```PowerShell
Install-WindowsFeature <name>
```

| Servizio ruolo o funzionalità | Name |
| ----------------------- | ---- |
| Spazi dei nomi DFS          | `FS-DFS-Namespace` |
| Strumenti di gestione DFS    | `RSAT-DFS-Mgmt-Con` |

Per installare, ad esempio, il componente Strumenti per File system distribuito (DFS) della funzionalità Strumenti di amministrazione remota del server, digitare:

```PowerShell
Install-WindowsFeature "RSAT-DFS-Mgmt-Con"
```

Per installare Spazi dei nomi DFS e i componenti Strumenti per File system distribuito (DFS) della funzionalità Strumenti di amministrazione remota del server, digitare:

```PowerShell
Install-WindowsFeature "FS-DFS-Namespace", "RSAT-DFS-Mgmt-Con"
```

## <a name="interoperability-with-azure-virtual-machines"></a>Interoperabilità con macchine virtuali di Azure

L'uso di Spazi dei nomi DFS su una macchina virtuale in Microsoft Azure è stato testato, ma è necessario rispettare alcuni limiti e requisiti.

- Non è possibile raggruppare spazi dei nomi autonomi in macchine virtuali di Azure.

- È possibile ospitare spazi dei nomi basati su dominio in macchine virtuali di Azure, inclusi gli ambienti con Azure Active Directory.

Per altre informazioni introduttive sulle macchine virtuali di Azure, vedi la [documentazione delle macchine virtuali di Azure](https://docs.microsoft.com/azure/virtual-machines/).

## <a name="see-also"></a>Vedere anche

Per altre informazioni correlate, vedere le risorse seguenti.

| Tipo di contenuto        | Riferimenti |
| ------------------  | ----------------|
| **Valutazione del prodotto** | [Novità di spazi dei nomi DFS e Replica DFS in Windows Server](https://technet.microsoft.com/library/dn281957(v=ws.11).aspx) |
| **Distribuzione**    | [Considerazioni sulla scalabilità dello spazio dei nomi DFS](https://blogs.technet.com/b/filecab/archive/2012/08/26/dfs-namespace-scalability-considerations.aspx) |
| **Operazioni**    | [Spazi dei nomi DFS: domande frequenti](https://technet.microsoft.com/library/ee404780.aspx) |
| **Risorse della community** | [Forum TechNet di servizi file e archiviazione](https://social.technet.microsoft.com/forums/winserverfiles/threads/) |
| **Protocolli**        | [Protocolli di servizi file in Windows Server](https://msdn.microsoft.com/library/cc239318.aspx) (deprecato) |
| **Tecnologie correlate** | [Clustering di failover](../../failover-clustering/failover-clustering-overview.md)|
| **Supporto tecnico** | [Supporto di Windows IT Pro](https://www.microsoft.com/itpro/windows/support)|
