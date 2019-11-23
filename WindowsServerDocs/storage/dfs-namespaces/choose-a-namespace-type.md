---
title: Scegliere un tipo di spazio dei nomi
description: Questo articolo descrive come scegliere un tipo di spazio dei nomi.
ms.date: 6/5/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: becaab1b4c35492200ad3d75d5f31829d85e10f8
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71402196"
---
# <a name="choose-a-namespace-type"></a>Scegliere un tipo di spazio dei nomi

> Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Quando si crea uno spazio dei nomi, devi scegliere uno dei due tipi di spazio dei nomi: ovvero uno spazio dei nomi autonomo o uno spazio dei nomi basato su dominio. Inoltre, se si sceglie uno spazio dei nomi basato su dominio, è necessario scegliere una modalità dello spazio dei nomi, ovvero la modalità server di Windows 2000 o Windows Server 2008.

## <a name="choosing-a-namespace-type"></a>Scelta di un tipo di spazio dei nomi

Scegli uno spazio dei nomi autonomo se una delle condizioni seguenti si applica al tuo ambiente:

-   L'organizzazione non usa Active Directory Domain Services (AD DS).
-   Desideri aumentare la disponibilità dello spazio dei nomi tramite un cluster di failover.
-   È necessario creare un singolo spazio dei nomi con più di 5.000 cartelle DFS in un dominio che non soddisfi i requisiti per uno spazio dei nomi basato su dominio (modalità Windows Server 2008), come descritto più avanti in questo argomento.

> [!NOTE]
> Per controllare le dimensioni di uno spazio dei nomi, fai clic sullo spazio dei nomi nell'albero della console di Gestione DFS, fai clic su **Proprietà**, quindi visualizza le dimensioni dello spazio dei nomi nella finestra di dialogo **Proprietà spazio dei nomi**. Per ulteriori informazioni sulla scalabilità degli spazi dei nomi DFS, vedi il sito Web Microsoft [Servizi file](https://technet.microsoft.com/library/cc771548.aspx).

Scegli uno spazio dei nomi basato su dominio se una delle condizioni seguenti si applica al tuo ambiente:

-   Desideri garantire la disponibilità dello spazio dei nomi tramite più server dello spazio dei nomi.
-   Desideri nascondere il nome del server dello spazio dei nomi agli utenti. Questo rende più semplice sostituire il server dello spazio dei nomi o eseguire la migrazione dello spazio dei nomi a un altro server.

## <a name="choosing-a-domain-based-namespace-mode"></a>Scelta di una modalità per lo spazio dei nomi basato su dominio

Se si sceglie uno spazio dei nomi basato su dominio, è necessario scegliere se utilizzare la modalità server di Windows 2000 o Windows Server 2008. La modalità Windows Server 2008 include il supporto per l'enumerazione basata sull'accesso e una maggiore scalabilità. Lo spazio dei nomi basato su dominio introdotto in Windows 2000 Server è ora definito "spazio dei nomi basato su dominio (modalità server Windows 2000)".

Per usare la modalità Windows Server 2008, il dominio e lo spazio dei nomi devono soddisfare i requisiti minimi seguenti:

-   La foresta usa il livello di funzionalità Windows Server 2003 o versione successiva.
-   Il dominio usa il livello di funzionalità del dominio Windows Server 2008 o versione successiva.
-   Tutti i server dello spazio dei nomi eseguono Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008.

Se l'ambiente lo supporta, scegliere la modalità Windows Server 2008 quando si creano nuovi spazi dei nomi basati su dominio. Questa modalità fornisce funzionalità e scalabilità aggiuntive e consente inoltre di eliminare la necessità di eseguire la migrazione di uno spazio dei nomi dalla modalità server di Windows 2000.

Per informazioni sulla migrazione di uno spazio dei nomi alla modalità Windows Server 2008, vedere [eseguire la migrazione di uno spazio dei nomi basato su dominio in modalità Windows server 2008](migrate-a-domain-based-namespace-to-windows-server-2008-mode.md).

Se l'ambiente non supporta spazi dei nomi basati su dominio in modalità Windows Server 2008, utilizzare la modalità server di Windows 2000 esistente per lo spazio dei nomi.

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
|Supporto per l'enumerazione basata sull'accesso (se abilitata)|Sì, richiede il server dello spazio dei nomi di Windows Server 2008|No|Sì|
|Metodi supportati per assicurare la disponibilità dello spazio dei nomi|Creare uno spazio dei nomi autonomo in un cluster di failover.|Usare più server dello spazio dei nomi per ospitare lo spazio dei nomi. (I server dello spazio dei nomi devono essere nello stesso dominio.)|Usare più server dello spazio dei nomi per ospitare lo spazio dei nomi. (I server dello spazio dei nomi devono essere nello stesso dominio.)|
|Supporto per usare Replica DFS per replicare le destinazioni cartella|Supportati quando vengono aggiunti a un dominio AD DS|Supportato|Supportato|

## <a name="see-also"></a>Vedi anche

-   [Distribuzione di Spazi dei nomi DFS](deploying-dfs-namespaces.md)
-   [Eseguire la migrazione di uno spazio dei nomi basato su dominio alla modalità Windows Server 2008](migrate-a-domain-based-namespace-to-windows-server-2008-mode.md)


