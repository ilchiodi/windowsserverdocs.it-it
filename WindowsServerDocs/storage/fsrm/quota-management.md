---
title: Gestione delle quote
description: In questo articolo viene descritto come creare e gestire quote
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 60436f12e07b8a3f16312829d53a2885c98f30ed
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="quota-management"></a>Gestione delle quote

> Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Nel nodo **Gestione quote** dello snap-in Microsoft<sup>®</sup> Management Console (MMC) Gestione risorse file server, è possibile eseguire le attività seguenti:

-   Creare quote per limitare lo spazio consentito per un volume o una cartella e generare notifiche quando vengono raggiunti o superati i limiti di quota.
-   Generare quote automatiche applicabili a tutte le sottocartelle esistenti in un volume o in una cartella e alle eventuali sottocartelle create in futuro.
-   Definire i modelli di quota che possono essere facilmente applicati a nuovi volumi o a nuove cartelle e utilizzati all'interno dell'organizzazione.

Ad esempio, è possibile:

-   Impostare un limite di 200 megabyte (MB) per le cartelle server personali degli utenti, con una notifica di posta elettronica inviata all'amministratore e all'utente quando vengono superati 180 MB di spazio di archiviazione.
-   Impostare una quota flessibile di 500 MB su una cartella condivisa di un gruppo. Quando viene raggiunto il limite di archiviazione impostato, tutti gli utenti del gruppo ricevono una notifica tramite posta elettronica che li informa della temporanea estensione del limite a 520 MB, in modo che possano eliminare i file non necessari e uniformarsi al criterio di quota predefinito di 500 MB.
-   Ricevere una notifica quando una cartella temporanea raggiunge 2 gigabyte (GB) di utilizzo, senza limitare la quota della cartella in quanto è necessaria per un servizio in esecuzione sul server.

In questa sezione sono inclusi gli argomenti seguenti:

-   [Creare una quota](create-quota.md)
-   [Creare una quota automatica](create-auto-apply-quota.md)
-   [Creare un modello quota](create-quota-template.md)
-   [Modificare le proprietà dei modelli quota](edit-quota-template-properties.md)
-   [Modificare le proprietà delle quote automatiche](edit-auto-apply-quota-properties.md)

> [!Note]
> Per impostare notifiche tramite posta elettronica e funzionalità di creazione di rapporti, è innanzitutto necessario configurare le opzioni generali di Gestione risorse file server.

## <a name="see-also"></a>Vedere anche

-   [Impostazione delle opzioni di Gestione risorse file server](setting-file-server-resource-manager-options.md)


