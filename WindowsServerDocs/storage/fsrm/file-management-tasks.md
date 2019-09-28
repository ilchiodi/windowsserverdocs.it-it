---
title: Attività di gestione file
description: In questo articolo viene descritto il processo di automazione delle attività di gestione file
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 257ee2955c4f521d14f01ec197fd45e5194eef02
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394098"
---
# <a name="file-management-tasks"></a>Attività di gestione file

> Si applica a: Windows Server 2019, Windows Server 2016, Windows Server (canale semestrale), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Le attività di gestione file automatizzano il processo di individuazione di sottoinsiemi di file in un server e applicazione di comandi semplici. Queste attività possono essere pianificate per l'esecuzione periodica allo scopo di ridurre costi ricorrenti. I file che verranno elaborati da un'attività di gestione file possono essere definiti tramite le proprietà seguenti:

-   Location
-   Proprietà di classificazione
-   Ora di creazione
-   Ora di modifica
-   Ora ultimo accesso

Le attività di gestione file possono anche essere configurate per comunicare ai proprietari di file gli eventuali criteri imminenti che verranno applicati ai loro file.

> [!Note]
> Le singole attività di gestione file vengono eseguite in base a pianificazioni indipendenti.

<br />
Questa sezione include gli argomenti seguenti:

-   [Creare un'attività di scadenza dei file](create-file-expiration-task.md)
-   [Creare un'attività di gestione dei file personalizzata](create-custom-file-management-task.md)

> [!Note]
> Per impostare le notifiche tramite posta elettronica e alcune funzionalità di creazione di rapporti, è innanzitutto necessario configurare le opzioni generali di Gestione risorse file server.

## <a name="see-also"></a>Vedere anche

-   [Impostazione delle opzioni di Gestione risorse file server](setting-file-server-resource-manager-options.md)


