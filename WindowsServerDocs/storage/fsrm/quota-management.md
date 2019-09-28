---
title: Gestione delle quote
description: In questo articolo viene descritto come creare e gestire quote
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 5a655e28020d08bb1c10fa862c007f914a8cf566
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71403078"
---
# <a name="quota-management"></a>Gestione delle quote

> Si applica a: Windows Server 2019, Windows Server 2016, Windows Server (canale semestrale), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Nel nodo **Gestione quote** dello snap-in Microsoft<sup>®</sup> Management Console (MMC) Gestione risorse file server, è possibile eseguire le attività seguenti:

-   Creare quote per limitare lo spazio consentito per un volume o una cartella e generare notifiche quando vengono raggiunti o superati i limiti di quota.
-   Generare quote automatiche applicabili a tutte le sottocartelle esistenti in un volume o in una cartella e alle eventuali sottocartelle create in futuro.
-   Definire i modelli di quota che possono essere facilmente applicati a nuovi volumi o a nuove cartelle e utilizzati all'interno dell'organizzazione.

Ad esempio, puoi:

-   Inserire un limite di 200 megabyte (MB) per le cartelle del server personale degli utenti, con una notifica di posta elettronica inviata all'utente e all'utente quando è stato superato 180 MB di spazio di archiviazione.
-   Impostare una quota flessibile di 500 MB nella cartella condivisa di un gruppo. Quando viene raggiunto il limite di archiviazione, tutti gli utenti del gruppo ricevono una notifica tramite posta elettronica che indica che la quota di archiviazione è stata temporaneamente estesa a 520 MB in modo che sia possibile eliminare i file non necessari e conformi ai criteri di quota predefiniti di 500 MB.
-   Ricevere una notifica quando una cartella temporanea raggiunge 2 gigabyte (GB) di utilizzo, senza limitare la quota della cartella in quanto è necessaria per un servizio in esecuzione sul server.

Questa sezione include gli argomenti seguenti:

-   [Creare una quota](create-quota.md)
-   [Creare una quota automatica](create-auto-apply-quota.md)
-   [Creare un modello di quota](create-quota-template.md)
-   [Modificare le proprietà dei modelli quota](edit-quota-template-properties.md)
-   [Modificare le proprietà delle quote automatiche](edit-auto-apply-quota-properties.md)

> [!Note]
> Per impostare notifiche tramite posta elettronica e funzionalità di creazione di rapporti, è innanzitutto necessario configurare le opzioni generali di Gestione risorse file server.

## <a name="see-also"></a>Vedere anche

-   [Impostazione delle opzioni di Gestione risorse file server](setting-file-server-resource-manager-options.md)


