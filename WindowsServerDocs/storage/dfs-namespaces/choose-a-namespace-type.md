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
ms.openlocfilehash: eb471b5bf4e12a05b36973eb1ea5350469f6acd5
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="choose-a-namespace-type"></a>Scegliere un tipo di spazio dei nomi

> Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

Quando si crea uno spazio dei nomi, devi scegliere uno dei due tipi di spazio dei nomi: ovvero uno spazio dei nomi autonomo o uno spazio dei nomi basato su dominio. Inoltre, se scegli uno spazio dei nomi basato su dominio, devi scegliere una modalità dello spazio dei nomi, ovvero la modalità Windows 2000 Server o Windows Server 2008.

## <a name="choosing-a-namespace-type"></a>Scelta di un tipo di spazio dei nomi

Scegli uno spazio dei nomi autonomo se una delle condizioni seguenti si applica al tuo ambiente:

-   L'organizzazione non usa Active Directory Domain Services (AD DS).
-   Desideri aumentare la disponibilità dello spazio dei nomi tramite un cluster di failover.
-   È necessario creare un unico spazio dei nomi con più di 5.000 cartelle DFS in un dominio che non soddisfa i requisiti per uno spazio dei nomi basato su dominio (modalità Windows Server 2008) come descritto più avanti in questo argomento.

> [!NOTE]
> Per controllare le dimensioni di uno spazio dei nomi, fai clic sullo spazio dei nomi nell'albero della console di Gestione DFS, fai clic su **Proprietà**, quindi visualizza le dimensioni dello spazio dei nomi nella finestra di dialogo **Proprietà spazio dei nomi**. Per ulteriori informazioni sulla scalabilità degli spazi dei nomi DFS, vedi il sito Web Microsoft [Servizi file](https://technet.microsoft.com/library/cc771548.aspx).

Scegli uno spazio dei nomi basato su dominio se una delle condizioni seguenti si applica al tuo ambiente:

-   Desideri garantire la disponibilità dello spazio dei nomi tramite più server dello spazio dei nomi.
-   Desideri nascondere il nome del server dello spazio dei nomi agli utenti. Questo rende più semplice sostituire il server dello spazio dei nomi o eseguire la migrazione dello spazio dei nomi a un altro server.

## <a name="choosing-a-domain-based-namespace-mode"></a>Scelta di una modalità per lo spazio dei nomi basato su dominio

Se scegli uno spazio dei nomi basato su dominio, devi scegliere se usare la modalità Windows 2000 Server o Windows Server 2008. La modalità Windows Server 2008 include il supporto per l'enumerazione basata sull'accesso e una maggiore scalabilità. Lo spazio dei nomi basato su dominio introdotta in Windows 2000 Server ora viene definito come "spazio dei nomi basato su dominio (modalità Windows 2000 Server)".

Per usare la modalità Windows Server 2008, il dominio e lo spazio dei nomi devono soddisfare i requisiti minimi seguenti:

-   La foresta usa il livello di funzionalità WindowsServer2003 o versione successiva.
-   Il dominio usa il livello di funzionalità Windows Server2008 o versione successiva.
-   Tutti i server dello spazio dei nomi eseguono Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2 o Windows Server 2008.

Se l'ambiente lo supporta, scegli la modalità Windows Server 2008 quando crei nuovi spazi dei nomi basati su dominio. Questa modalità offre funzionalità e scalabilità aggiuntive ed elimina anche l'eventuale esigenza di eseguire la migrazione di uno spazio dei nomi dalla modalità Windows 2000 Server.

Per informazioni sulla migrazione di uno spazio dei nomi alla modalità Windows Server 2008, vedi [Eseguire la migrazione di uno spazio dei nomi basato su dominio alla modalità Windows Server 2008](migrate-a-domain-based-namespace-to-windows-server-2008-mode.md).

Se l'ambiente non supporta spazi dei nomi basati su dominio in modalità Windows Server 2008, usa la modalità Windows 2000 Server esistente per lo spazio dei nomi.

## <a name="comparing-namespace-types-and-modes"></a>Confronto tra tipi e modalità degli spazi dei nomi

Nella tabella seguente vengono descritte le caratteristiche di ogni tipo e modalità di spazio dei nomi.

|Caratteristica|Spazio dei nomi autonomo|Spazio dei nomi basato su dominio (modalità Windows 2000 Server) |Spazio dei nomi basato su dominio (modalità Windows Server 2008) | 
|---|---|---|---|
|Percorso dello spazio dei nomi|\\\ *ServerName\RootName* |\\\ *NetBIOSDomainName\RootName* <br />\\\ *DNSDomainName\RootName*|\\\ *NetBIOSDomainName\RootName* <br /> \\\ *DNSDomainName\RootName*|
|Percorso di archiviazione delle informazioni dello spazio dei nomi|Nel Registro di sistema e in una cache in memoria nel server dello spazio dei nomi|In AD DS e in una cache in memoria in ogni server dello spazio dei nomi|In AD DS e in una cache in memoria in ogni server dello spazio dei nomi|
|Consigli per le dimensioni dello spazio dei nomi|Lo spazio dei nomi può contenere più di 5.000 cartelle con destinazioni e il limite consigliato è di 50.000 cartelle con destinazioni|Le dimensioni dell'oggetto dello spazio dei nomi in AD DS devono essere inferiori a 5 megabyte (MB) per mantenere la compatibilità con i controller di dominio che non eseguono Windows Server 2008. Questo significa che non devono essere presenti più di circa 5.000 cartelle con destinazioni.|Lo spazio dei nomi può contenere più di 5.000 cartelle con destinazioni e il limite consigliato è di 50.000 cartelle con destinazioni |
|Livello di funzionalità della foresta AD DS minimo|AD DS non è necessario|Windows 2000|Windows Server 2003|
|Livello di funzionalità del dominio AD DS minimo|AD DS non è necessario|Windows2000 misto|Windows Server 2008|
|Server dello spazio dei nomi supportati minimi|Windows 2000 Server|Windows 2000 Server|Windows Server 2008|
|Supporto per l'enumerazione basata sull'accesso (se abilitata)|Sì, richiede il server dello spazio dei nomi di Windows Server 2008|No|Sì|
|Metodi supportati per assicurare la disponibilità dello spazio dei nomi|Creare uno spazio dei nomi autonomo in un cluster di failover.|Usare più server dello spazio dei nomi per ospitare lo spazio dei nomi. (I server dello spazio dei nomi devono essere nello stesso dominio.)|Usare più server dello spazio dei nomi per ospitare lo spazio dei nomi. (I server dello spazio dei nomi devono essere nello stesso dominio.)|
|Supporto per usare Replica DFS per replicare le destinazioni cartella|Supportati quando vengono aggiunti a un dominio AD DS|Supportati|Supportati|

## <a name="see-also"></a>Vedi anche

-   [Distribuzione di Spazi dei nomi DFS](deploying-dfs-namespaces.md)
-   [Eseguire la migrazione di uno spazio dei nomi basato su dominio alla modalità Windows Server 2008](migrate-a-domain-based-namespace-to-windows-server-2008-mode.md)


