---
title: Installare Server con Esperienza desktop
description: "Illustra come ottenere e installare un'installazione di Server con Esperienza desktop "
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.date: 01/18/2017
ms.technology: server-general
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 5b38b8a0-4dfc-4130-be00-fc58bba99595
author: jaimeo
ms.author: jaimeo
manager: dongill
ms.localizationpriority: medium
ms.openlocfilehash: eb2e5be2ed19fe7cd64f6c6bd64ca9afafd93bff
ms.sourcegitcommit: 4b9b21ca1f366388a78ead7413cb581f2b23d4c6
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 08/09/2018
ms.locfileid: "2711806"
---
# Installare Server con Esperienza desktop
> Si applica a: Windows Server 2016
  

Quando si installa Windows Server 2016 tramite l'Installazione guidata, è possibile scegliere tra **Windows Server 2016** e **Windows Server (Server con Esperienza desktop)**. L'opzione Server con Esperienza desktop è l'equivalente in Windows Server 2016 dell'opzione Installazione completa disponibile in Windows Server 2012 R2, con la funzionalità Esperienza desktop installata. Se non si sceglie alcuna opzione nell'Installazione guidata, viene installato **Windows Server 2016**, ovvero l'opzione di installazione **Server Core**.

L'opzione Server con Esperienza desktop installa l'interfaccia utente standard e tutti gli strumenti, comprese le funzionalità client che richiedevano un'installazione separata in Windows Server 2012 R2. I ruoli server e le funzionalità vengono installato con Server Manager o per mezzo di altri metodi. Rispetto all'opzione Server Core, questa richiede più spazio su disco e ha requisiti di manutenzione maggiori, è quindi consigliabile scegliere questo tipo di installazione a meno che non occorrano elementi aggiuntivi dell'interfaccia utente e strumenti di gestione grafica inclusi nell'opzione Server con Esperienza desktop. Se si ritiene di poter fare a meno degli elementi aggiuntivi, vedere [Installare Server Core](Getting-Started-with-Server-Core.md). Per un'opzione ancora più leggera, vedi [Installare Nano Server](Getting-Started-with-Nano-Server.md).

>[!NOTE]
>
>Diversamente da alcune versioni precedenti di Windows Server, non puoi eseguire la conversione tra Server Core e la versione Server con Esperienza desktop dopo l'installazione. Se installi la versione Server con Esperienza desktop e successivamente decidi di usare Server Core, devi eseguire un'installazione pulita.

**Interfaccia utente:** interfaccia utente grafica standard ("Shell grafica server"). Shell grafica Server include la nuova shell di Windows 10. Le funzionalità specifiche di Windows installate per impostazione predefinita con questa opzione sono User-Interfaces-Infra, Server-GUI-Shell, Server-GUI-Mgmt-Infra, InkAndHandwritingServices, ServerMediaFoundation ed Esperienza desktop. Anche se queste funzionalità sono presenti in questa versione di Server Manager, la loro disinstallazione non è supportata e non saranno disponibili nelle versioni future.

**Installare, configurare, disinstallare ruoli server in locale:** con Server Manager o con Windows PowerShell.

**Installare, configurare, disinstallare ruoli server in modalità remota:** con Server Manager, server remoto, Strumenti di amministrazione remota del server (RSAT) o Windows PowerShell.

**Microsoft Management Console: installata**

## Scenari di installazione

### Valutazione
È possibile ottenere una copia di valutazione con una licenza di 180 giorni di Windows Server da [Windows Server Valutazioni](https://www.microsoft.com/evalcenter/evaluate-windows-server-2016). Per il download, scegli l'**opzione Windows Server 2016 | ISO a 64 bit** oppure visita **Windows Server 2016 | Laboratorio virtuale**.

> [!IMPORTANT]  
> Per le versioni di Windows Server 2016 precedenti a 14393.0.161119-1705.RS1_REFRESH, è possibile eseguire solo la conversione dalla versione di valutazione a quella definitiva con Windows Server 2016 installato mediante l'opzione Esperienza desktop (non l'opzione Server Core). A partire dalla versione 14393.0.161119-1705. RS1_REFRESH, è possibile convertire le edizioni di valutazione in quella definitiva indipendentemente dall'opzione di installazione utilizzata.


### Installazione pulita

Per installare l'opzione di installazione Server con Esperienza desktop dal supporto, inserisci il supporto in un'unità, riavvia il computer ed esegui Setup.exe. Nella procedura guidata visualizzata, selezionare **Windows Server (Server con Esperienza desktop)** (Standard o Datacenter) e quindi completare la procedura guidata.

### Aggiornamento di versione/Aggiornare la versione
Il termine **aggiornamento** indica il passaggio dalla versione del sistema operativo esistente a una versione più recente, sempre nello stesso hardware.

Se si dispone già di un'installazione completa del prodotto Windows Server appropriato, è possibile aggiornarla a un'installazione Server con Esperienza desktop dell'edizione appropriata di Windows Server 2016, come indicato di seguito.

> [!IMPORTANT]  
> In questa versione, l'aggiornamento funziona meglio in macchine virtuali in cui non sono necessari specifici driver hardware originali per l'aggiornamento. In caso contrario, la migrazione è l'opzione consigliata.  

- Gli aggiornamenti sul posto da un'architettura a 32 bit a un'architettura a 64 bit non sono supportati. Tutte le edizioni di Windows Server 2016 sono solo a 64 bit.
- Gli aggiornamenti sul posto da una lingua a un'altra non sono supportati.
- Se il server è un controller di dominio, vedere [Aggiornare controller di dominio a Windows Server 2012 R2 e Windows Server 2012](https://technet.microsoft.com/library/hh994618.aspx) per informazioni importanti.
- Gli aggiornamenti da versioni non definitive (anteprime) di Windows Server 2016 non sono supportati. Eseguire un'installazione pulita a Windows Server 2016.
- Gli aggiornamenti che prevedono il passaggio da un'installazione Server Core a un'installazione Server con Esperienza desktop (e viceversa) non sono supportati.

Se la versione in uso non è inclusa nella colonna a sinistra, l'aggiornamento a tale versione di Windows Server 2016 non è supportato.

Se nella colonna a destra sono presenti più edizioni, è supportato l'aggiornamento dalla stessa versione di partenza a **qualsiasi** edizione indicata.

|Edizioni eseguite|Edizioni a cui è possibile eseguire l'aggiornamento|  
|-------------------|----------|  
|Windows Server 2012 Standard|Windows Server 2016 Standard o Datacenter|
|Windows Server 2012 Datacenter|Windows Server 2016 Datacenter|
|Windows Server 2012 R2 Standard|Windows Server 2016 Standard o Datacenter|
|Windows Server 2012 R2 Datacenter|Windows Server 2016 Datacenter|
|Windows Server 2012 R2 Essentials|Windows Server 2016 Essentials|
|Windows Storage Server 2012 Standard|Windows Storage Server 2016 Standard|
|Windows Storage Server 2012 Workgroup|Windows Storage Server 2016 Workgroup|
|Windows Storage Server 2012 R2 Standard|Windows Storage Server 2016 Standard|
|Windows Storage Server 2012 R2 Workgroup|Windows Storage Server 2016 Workgroup|

Per altre opzioni per il passaggio a Windows Server 2016, ad esempio una conversione della licenza tra edizioni multilicenza, edizioni di valutazione e altre edizioni, vedere i dettagli in [Opzioni di aggiornamento](Supported-Upgrade-Paths.md).

### Migrazione
Il termine **migrazione** indica il passaggio dal sistema operativo esistente a Windows Server 2016 eseguendo un'installazione pulita in un set diverso di hardware o macchine virtuali e quindi il trasferimento dei carichi di lavoro del server precedente al nuovo server. Il processo di migrazione, che può variare notevolmente in base ai ruoli server installati, è illustrato in dettaglio in [Windows Server Installation, Upgrade, and Migration](https://technet.microsoft.com/windowsserver/dn458795) (Installazione, aggiornamento e migrazione di Windows Server).

La possibilità di eseguire la migrazione varia a seconda dei ruoli server. La griglia seguente illustra l'aggiornamento del ruolo server e le opzioni di migrazione in modo specifico per lo spostamento a Windows Server 2016. Per indicazioni sulla migrazione dei singoli ruoli, consultare [Migrazione dei ruoli e delle funzionalità in Windows Server](https://technet.microsoft.com/windowsserver/jj554790.aspx). Per altre informazioni sulle opzioni di installazione e aggiornamento del server, vedere [Windows Server Installation, Upgrade, and Migration](https://technet.microsoft.com/windowsserver/dn458795) (Installazione, aggiornamento e migrazione di Windows Server).

|Ruolo server|Aggiornamento da Windows Server 2012 R2|Aggiornamento da Windows Server 2012|Supporto della migrazione|Completamento della migrazione senza tempi di inattività|  
|-------------------|----------|--------------|--------------|----------|  
|Servizi certificati Active Directory| Sì|    Sì|    Sì|    No|
|Active Directory Domain Services|  Sì|    Sì|    Sì|    Sì|
|Active Directory Federation Services|  No| No| Sì|    No (è necessario aggiungere nuovi nodi alla farm)|
|Active Directory Lightweight Directory Services|   Sì|    Sì|    Sì|    Sì|
|Active Directory Rights Management Services|   Sì|    Sì|    Sì|    No|
|Cluster di failover|Sì con il processo di [aggiornamento in sequenza del sistema operativo del cluster](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade) che include le operazioni di sospensione-svuotamento, rimozione e aggiornamento dei nodi a Windows Server 2016 e la ricostituzione del cluster originale. Sì, quando il server viene rimosso dal cluster per l'aggiornamento e quindi aggiunto a un cluster diverso.|Non mentre il server fa parte di un cluster. Sì, quando il server viene rimosso dal cluster per l'aggiornamento e quindi aggiunto a un cluster diverso.  |Sì|No per i cluster di failover di Windows Server 2012. Sì per i cluster di failover di Windows Server 2012 R2 con macchine virtuali Hyper-V o per i cluster di failover di Windows Server 2012 R2 che eseguono il ruolo File server di scalabilità orizzontale. Vedere [Cluster OS Rolling Upgrade](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade) (Aggiornamento in sequenza del sistema operativo del cluster).|
|Servizi file e archiviazione| Sì|    Sì|    Varia in base alla funzionalità secondaria|  No|
|Servizi di stampa e fax|    No| No| Sì (Printbrm.exe)| No|
|Servizi Desktop remoto|   Sì, per tutti i ruoli secondari, ma la farm in modalità mista non è supportata.|   Sì, per tutti i ruoli secondari, ma la farm in modalità mista non è supportata.|   Sì|    No|
|Server Web (IIS)|  Sì|    Sì|    Sì|    No|
|Esperienza Windows Server Essentials|  Sì|    N/D, nuove funzionalità|  Sì|    No|
|Windows Server Update Services|    Sì|    Sì|    Sì|    No|
|Cartelle di lavoro|  Sì|    Sì|    Sì|    Sì da WS 2012 R2 durante l'uso di [Cluster OS Rolling Upgrade](https://technet.microsoft.com/windows-server-docs/failover-clustering/cluster-operating-system-rolling-upgrade) (Aggiornamento in sequenza del sistema operativo del cluster).|

> [!IMPORTANT]  
> Al termine dell'installazione e immediatamente dopo aver installato tutti i ruoli del server e le funzionalità necessarie, cercare e installare gli aggiornamenti disponibili per Windows Server 2016 tramite Windows Update o altri metodi di aggiornamento.

---------------------------------------
Se è necessaria un'altra opzione di installazione oppure se l'installazione è stata completata e si è pronti a distribuire carichi di lavoro specifici, è possibile passare alla [pagina principale di Windows Server 2016](Windows-Server-2016.md).