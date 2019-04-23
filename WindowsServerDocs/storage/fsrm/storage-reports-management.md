---
title: Gestione rapporti di archiviazione
description: In questo articolo viene descritto come generare, pianificare e monitorare rapporti di archiviazione
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: c0d750fd139865daa92319c1d1926dc5d36669b2
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59885772"
---
# <a name="storage-reports-management"></a>Gestione rapporti di archiviazione

> Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Nel nodo **Gestione rapporti di archiviazione** dello snap-in Microsoft<sup>®</sup> Management Console (MMC) Gestione risorse file server, è possibile eseguire le attività seguenti:

-   Pianificare i rapporti di archiviazione periodici che consentono di identificare le tendenze di utilizzo del disco.
-   Monitorare i tentativi di salvataggio di file non autorizzati di tutti gli utenti o di un gruppo selezionato di utenti.
-   Generare immediatamente rapporti di archiviazione.

Ad esempio, puoi:

-   Pianificare un rapporto da eseguire ogni domenica a mezzanotte, che generi un elenco dei file utilizzati più di recente nei due giorni precedenti Con queste informazioni è possibile monitorare l'attività di archiviazione durante il fine settimana e pianificare i tempi di inattività del server in modo da avere un minore impatto sugli utenti che si connettono da casa durante il fine settimana.
-   Eseguire un rapporto in qualsiasi momento per identificare tutti i file duplicati in un volume di un server in modo che lo spazio su disco possa essere recuperato rapidamente senza alcuna perdita di dati.
-   Eseguire un rapporto File in base a gruppo di file per identificare il metodo di segmentazione delle risorse di archiviazione in gruppi di file diversi 
-   Eseguire un rapporto File in base a proprietario per analizzare la modalità di utilizzo delle risorse di archiviazione condivise da parte dei singoli utenti.

Questa sezione include gli argomenti seguenti:

-   [Pianificazione di un Set di report](schedule-set-of-reports.md)
-   [Generare report su richiesta](generate-reports-on-demand.md)

> [!Note]
> Per impostare le notifiche tramite posta elettronica e alcune funzionalità di creazione di rapporti, è innanzitutto necessario configurare le opzioni generali di Gestione risorse file server.

## <a name="see-also"></a>Vedere anche

-   [Opzioni di gestione risorse di impostazione File Server](setting-file-server-resource-manager-options.md)


