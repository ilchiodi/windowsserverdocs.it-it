---
title: Scegliere un tipo di spazio dei nomi
description: Questo articolo descrive come scegliere un tipo di spazio dei nomi.
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: f83be7b2ec7dbe2383deb2d0a79e33d7c73f8849
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59873802"
---
# <a name="choose-a-namespace-type"></a>Scegliere un tipo di spazio dei nomi

> Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Quando si crea uno spazio dei nomi, devi scegliere uno dei due tipi di spazio dei nomi: ovvero uno spazio dei nomi autonomo o uno spazio dei nomi basato su dominio. Inoltre, se si sceglie uno spazio dei nomi basati su dominio, è necessario scegliere una modalità dello spazio dei nomi: Modalità Windows 2000 Server o Windows Server 2008.

## <a name="choosing-a-namespace-type"></a>Scelta di un tipo di spazio dei nomi

Scegli uno spazio dei nomi autonomo se una delle condizioni seguenti si applica al tuo ambiente:

-   L'organizzazione non utilizza servizi di dominio Active Directory (AD DS).
-   Desideri aumentare la disponibilità dello spazio dei nomi tramite un cluster di failover.
-   È necessario creare un unico spazio dei nomi con più di 5.000 cartelle DFS in un dominio che non soddisfa i requisiti di uno spazio dei nomi basati su dominio (modalità Windows Server 2008) come descritto più avanti in questo argomento.

> [!NOTE]
> Per controllare le dimensioni di uno spazio dei nomi, fai clic sullo spazio dei nomi nell'albero della console di Gestione DFS, fai clic su **Proprietà**, quindi visualizza le dimensioni dello spazio dei nomi nella finestra di dialogo **Proprietà spazio dei nomi**. Per ulteriori informazioni sulla scalabilità degli spazi dei nomi DFS, vedi il sito Web Microsoft [Servizi file](https://technet.microsoft.com/library/cc771548.aspx).

Scegli uno spazio dei nomi basato su dominio se una delle condizioni seguenti si applica al tuo ambiente:

-   Desideri garantire la disponibilità dello spazio dei nomi tramite più server dello spazio dei nomi.
-   Desideri nascondere il nome del server dello spazio dei nomi agli utenti. Questo rende più semplice sostituire il server dello spazio dei nomi o eseguire la migrazione dello spazio dei nomi a un altro server.

## <a name="choosing-a-domain-based-namespace-mode"></a>Scelta di una modalità per lo spazio dei nomi basato su dominio

Se si sceglie uno spazio dei nomi basati su dominio, è necessario scegliere se usare la modalità Windows 2000 Server o la modalità di Windows Server 2008. La modalità di Windows Server 2008 include il supporto per l'enumerazione basata sull'accesso e una maggiore scalabilità. Spazio dei nomi basati su dominio introdotta in Windows 2000 Server è ora nota come "basato su dominio dello spazio dei nomi (modalità Windows 2000 Server)."

Per usare la modalità di Windows Server 2008, il dominio e dello spazio dei nomi deve soddisfare i requisiti minimi seguenti:

-   La foresta usa il livello di funzionalità Windows Server 2003 o versione successiva.
-   Il dominio deve utilizzare il Windows Server 2008 o superiore a livello funzionale di dominio.
-   Tutti i server dello spazio dei nomi sono in esecuzione Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008.

Se l'ambiente supporta, scegliere la modalità di Windows Server 2008 quando si creano nuovi spazi dei nomi basati su dominio. Questa modalità offre scalabilità e le funzionalità aggiuntive ed elimina anche l'eventuale necessità di eseguire la migrazione di uno spazio dei nomi dalla modalità Windows 2000 Server.

Per informazioni sulla migrazione di uno spazio dei nomi in modalità Windows Server 2008, vedere [eseguire la migrazione di un Namespace basato su dominio a Windows Server 2008 Mode](migrate-a-domain-based-namespace-to-windows-server-2008-mode.md).

Se l'ambiente non supporta spazi dei nomi basati su dominio in modalità Windows Server 2008, utilizzare la modalità Windows 2000 Server esistente per lo spazio dei nomi.

## <a name="comparing-namespace-types-and-modes"></a>Confronto tra tipi e modalità degli spazi dei nomi

Nella tabella seguente vengono descritte le caratteristiche di ogni tipo e modalità di spazio dei nomi.

|Caratteristica|Spazio dei nomi autonomo|Spazio dei nomi basato su dominio (modalità Windows 2000 Server) |Spazio dei nomi basato su dominio (modalità Windows Server 2008) | 
|---|---|---|---|
|Percorso dello spazio dei nomi|\\\ *ServerName\RootName* |\\\ *NetBIOSDomainName\RootName* <br />\\\ *DNSDomainName\RootName*|\\\ *NetBIOSDomainName\RootName* <br /> \\\ *DNSDomainName\RootName*|
|Percorso di archiviazione delle informazioni dello spazio dei nomi|Nel Registro di sistema e in una cache in memoria nel server dello spazio dei nomi|In AD DS e in una cache in memoria in ogni server dello spazio dei nomi|In AD DS e in una cache in memoria in ogni server dello spazio dei nomi|
|Consigli per le dimensioni dello spazio dei nomi|Lo spazio dei nomi può contenere più di 5.000 cartelle con destinazioni e il limite consigliato è di 50.000 cartelle con destinazioni|Le dimensioni dell'oggetto dello spazio dei nomi in AD DS devono essere inferiori a 5 megabyte (MB) per mantenere la compatibilità con i controller di dominio che non eseguono Windows Server 2008. Questo significa che non devono essere presenti più di circa 5.000 cartelle con destinazioni.|Lo spazio dei nomi può contenere più di 5.000 cartelle con destinazioni e il limite consigliato è di 50.000 cartelle con destinazioni |
|Livello di funzionalità della foresta AD DS minimo|AD DS non è necessario|Windows 2000|Windows Server 2003|
|Livello di funzionalità del dominio AD DS minimo|AD DS non è necessario|Windows 2000 misto|Windows Server 2008|
|Server dello spazio dei nomi supportati minimi|Windows 2000 Server|Windows 2000 Server|Windows Server 2008|
|Supporto per l'enumerazione basata sull'accesso (se abilitata)|Sì, richiede il server dello spazio dei nomi di Windows Server 2008|No|Yes|
|Metodi supportati per assicurare la disponibilità dello spazio dei nomi|Creare uno spazio dei nomi autonomo in un cluster di failover.|Usare più server dello spazio dei nomi per ospitare lo spazio dei nomi. (I server dello spazio dei nomi devono essere nello stesso dominio.)|Usare più server dello spazio dei nomi per ospitare lo spazio dei nomi. (I server dello spazio dei nomi devono essere nello stesso dominio.)|
|Supporto per usare Replica DFS per replicare le destinazioni cartella|Supportati quando vengono aggiunti a un dominio AD DS|Supportato|Supportato|

## <a name="see-also"></a>Vedere anche

-   [Distribuzione di spazi dei nomi DFS](deploying-dfs-namespaces.md)
-   [Eseguire la migrazione di un Namespace basato su dominio in modalità Windows Server 2008](migrate-a-domain-based-namespace-to-windows-server-2008-mode.md)


