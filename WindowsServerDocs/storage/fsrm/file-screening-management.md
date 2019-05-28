---
title: Gestione screening dei file
description: In questo articolo viene descritto come creare screening dei file, generare notifiche, definire i modelli per lo screening dei file e creare eccezioni di screening dei file
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 05fcaf402347d16bf8c28528d402664ae5f99165
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/09/2019
ms.locfileid: "65476058"
---
# <a name="file-screening-management"></a>Gestione screening dei file

> Si applica a: Windows Server 2019, Windows Server 2016, Windows Server (canale semestrale), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Nel nodo **Gestione screening dei file** dello snap-in di MMC Gestione risorse file server, è possibile eseguire le attività seguenti:

-   Creare screening dei file per controllare i tipi di file che gli utenti possono salvare e generare notifiche quando gli utenti tentano di salvare i file non autorizzati.
-   Definire i modelli per lo screening dei file che possono essere applicati a nuovi volumi o a nuove cartelle e che possono essere utilizzati all'interno di un'organizzazione.
-   Creare eccezioni di screening dei file che estendono la flessibilità delle regole per lo screening di file.

Ad esempio, puoi:

-   Assicurarsi che non vi siano file musicali archiviati nelle cartelle personali su un server, sebbene sia possibile consentire l'archiviazione di determinati tipi di file multimediali che supportano la gestione dei diritti legali o che risultano conformi ai criteri aziendali. Nello stesso scenario potrebbe essere necessario assegnare a un vicepresidente dell'azienda privilegi speciali per l'archiviazione di qualsiasi tipo di file nella propria cartella personale.
-   Implementare un processo di screening per ricevere notifiche tramite posta elettronica quando un file eseguibile viene archiviato in una cartella condivisa, incluse le informazioni relative all'utente che ha archiviato il file e la posizione esatta del file, in modo da consentire l'esecuzione dei passaggi precauzionali appropriati.

Questa sezione include gli argomenti seguenti:

-   [Definire gruppi di file per lo screening](define-file-groups-for-screening.md)
-   [Creare uno screening dei File](create-file-screen.md)
-   [Creare un'eccezione screening dei file](create-file-screen-exception.md)
-   [Creare un modello di screening dei file](create-file-screen-template.md)
-   [Modificare le proprietà dei modelli per lo screening dei file](edit-file-screen-template-properties.md)

> [!Note]
> Per impostare le notifiche tramite posta elettronica e alcune funzionalità di creazione di rapporti, è innanzitutto necessario configurare le opzioni generali di Gestione risorse file server.

## <a name="see-also"></a>Vedere anche

-   [Impostazione delle opzioni di Gestione risorse file server](setting-file-server-resource-manager-options.md)


