---
title: Gestione delle risorse di archiviazione remota
description: In questo articolo viene descritto come gestire risorse di archiviazione in un computer remoto
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 583c36f399848cf67c6f3a850e62015b224768d9
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59836632"
---
# <a name="managing-remote-storage-resources"></a>Gestione delle risorse di archiviazione remota

> Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Per gestire le risorse di archiviazione in un computer remoto, è possibile utilizzare due opzioni:

-   Connettersi al computer dallo snap-in Microsoft<sup>®</sup> Management Console (MMC) Gestione risorse file server (che è possibile usare per gestire le risorse remote).
-   Utilizzare gli strumenti da riga di comando installati con Gestione risorse file server.

Ogni opzione consente di utilizzare le quote, eseguire lo screening dei file, gestire le classificazioni, pianificare le attività di gestione dei file e generare rapporti con tali risorse remote.

> [!Note]
> Gestione risorse file server è in grado di gestire le risorse sia su un computer locale che un computer remoto, ma non su entrambi allo stesso tempo.

Ad esempio, puoi:

-   Connettersi a un altro computer nel dominio utilizzando lo snap-in MMC Gestione risorse file server e verificare l'utilizzo dello spazio di archiviazione in un volume o in una cartella presente nel computer remoto.
-   Creare modelli di quota e modelli per lo screening dei file in un server locale, quindi utilizzare gli strumenti da riga di comando per importare i modelli in un file server di una succursale.

Questa sezione include gli argomenti seguenti:

-   [Connettersi a un Computer remoto](connect-to-remote-computer.md)
-   [Strumenti da riga di comando](command-line-tools.md)
