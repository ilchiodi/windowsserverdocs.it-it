---
title: "Eseguire la migrazione di uno spazio dei nomi basato su dominio alla modalità Windows Server 2008"
description: "Questo articolo descrive come eseguire la migrazione di uno spazio dei nomi basato su dominio alla modalità Windows Server 2008"
ms.date: 6/5/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 1ef356e4f2abccb375732b7ec60a374c64dc0e7f
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="migrate-a-domain-based-namespace-to-windows-server-2008-mode"></a>Eseguire la migrazione di uno spazio dei nomi basato su dominio alla modalità Windows Server 2008

> Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server 2008

La modalità Windows Server 2008 per spazi dei nomi basati su dominio include il supporto per l'enumerazione basata sull'accesso e una maggiore scalabilità.

## <a name="to-migrate-a-domain-based-namespace-to-windows-server-2008-mode"></a>Per eseguire la migrazione di uno spazio dei nomi basato su dominio alla modalità Windows Server 2008

Per eseguire la migrazione di uno spazio dei nomi basato su dominio dalla modalità Windows 2000 Server alla modalità Windows Server 2008, devi esportare lo spazio dei nomi in un file, eliminare lo spazio dei nomi, ricrearlo in modalità Windows Server 2008 e quindi importare le impostazioni dello spazio dei nomi. A tale scopo, esegui questa procedura:

1.  Apri una finestra del prompt dei comandi e digita il comando seguente per esportare lo spazio dei nomi in un file, in cui \\\\*domain*\\*namespace* è il nome del dominio appropriato e de spazio dei nomi e *path\\filename* è il percorso e il nome di file del file di esportazione:
     ```
     Dfsutil root export \\domain\namespace path\filename.xml 
     ```
2.  Annota il percorso ( \\\\*server* \\*share* ) per ogni server dello spazio dei nomi. È necessario aggiungere manualmente i server dello spazio dei nomi allo spazio dei nomi creato nuovamente perché Dfsutil non può importare i server dello spazio dei nomi.
3.  In Gestione DFS fai clic con il pulsante destro del mouse sullo spazio dei nomi, quindi fai clic su **Elimina** o digita il comando seguente al prompt dei comandi, <br /> dove \\\\*domain*\\*namespace* è il nome del dominio e dello spazio dei nomi appropriati:
     ```
     Dfsutil root remove \\domain\namespace
     ```
4.  In Gestione DFS, ricrea lo spazio dei nomi con lo stesso nome, ma usa la modalità Windows Server 2008 o digita il comando seguente al prompt dei comandi, dove <br /> \\\\*server*\\*namespace* è il nome del server appropriato e della condivisione per la radice dello spazio dei nomi:
     ```
     Dfsutil root adddom \\server\namespace v2
     ```
5.  Per importare lo spazio dei nomi dal file di esportazione, digita il comando seguente al prompt dei comandi, dove <br /> \\\\*domain*\\*namespace* è il nome del dominio e dello spazio dei nomi appropriato e *path\\filename* è il percorso e il nome di file da importare:
     ```
     Dfsutil root import merge path\filename.xml \\domain\namespace
     ```

    > [!NOTE]
    > Per ridurre il tempo necessario per importare uno spazio dei nomi di grandi dimensioni, esegui il comando di importazione radice **Dfsutil** localmente in un server dello spazio dei nomi.
6.  Aggiungi tutti i server dello spazio dei nomi rimanenti allo spazio dei nomi ricreato facendo clic con il pulsante destro del mouse su Gestione DFS e quindi facendo clic su **Aggiungi server dello spazio dei nomi** oppure digitando il comando seguente al prompt dei comandi, dove <br /> \\\\*server*\\*share* è il nome del server appropriato e della condivisione per la radice dello spazio dei nomi:
     ```
     Dfsutil target add \\server\share 
     ```

    > [!NOTE]
    > Puoi aggiungere server dello spazio dei nomi prima di importare lo spazio dei nomi, ma in questo modo si provoca il download incrementale dei metadati per lo spazio dei nomi da parte dei server anziché determinare il download immediato dell'intero spazio dei nomi dopo l'aggiunta come server dello spazio dei nomi.

## <a name="see-also"></a>Vedi anche
-   [Distribuzione di Spazi dei nomi DFS](deploying-dfs-namespaces.md)
-   [Scegliere un tipo di spazio dei nomi](choose-a-namespace-type.md)