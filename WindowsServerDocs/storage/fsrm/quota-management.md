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
ms.openlocfilehash: febcd6ab0744a7fddd024e1f0afdb93711e8939a
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59829542"
---
# <a name="quota-management"></a>Gestione delle quote

> Si applica a: Windows Server (canale semestrale), Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Nel nodo **Gestione quote** dello snap-in Microsoft<sup>®</sup> Management Console (MMC) Gestione risorse file server, è possibile eseguire le attività seguenti:

-   Creare quote per limitare lo spazio consentito per un volume o una cartella e generare notifiche quando vengono raggiunti o superati i limiti di quota.
-   Generare quote automatiche applicabili a tutte le sottocartelle esistenti in un volume o in una cartella e alle eventuali sottocartelle create in futuro.
-   Definire i modelli di quota che possono essere facilmente applicati a nuovi volumi o a nuove cartelle e utilizzati all'interno dell'organizzazione.

Ad esempio, puoi:

-   Imporre alcun limite di 200 megabyte (MB) alle cartelle del server personale degli utenti, con una notifica di posta elettronica inviata all'utente e l'utente quando viene superata 180 MB di spazio di archiviazione.
-   Impostare una quota di 500 MB flessibile su una cartella condivisa di un gruppo. Quando viene raggiunto il limite di archiviazione, tutti gli utenti del gruppo ricevono una notifica tramite posta elettronica che la quota di archiviazione è stato esteso temporaneamente a 520 MB, in modo che possano eliminare i file non necessari e rispettare i criteri di quota predefinito di 500 MB.
-   Ricevere una notifica quando una cartella temporanea raggiunge 2 gigabyte (GB) di utilizzo, senza limitare la quota della cartella in quanto è necessaria per un servizio in esecuzione sul server.

Questa sezione include gli argomenti seguenti:

-   [Creare una Quota](create-quota.md)
-   [Creare un'Auto della Quota di applicazione](create-auto-apply-quota.md)
-   [Creare un modello Quota](create-quota-template.md)
-   [Modifica proprietà modello di Quota](edit-quota-template-properties.md)
-   [Modifica automaticamente le proprietà delle quote](edit-auto-apply-quota-properties.md)

> [!Note]
> Per impostare notifiche tramite posta elettronica e funzionalità di creazione di rapporti, è innanzitutto necessario configurare le opzioni generali di Gestione risorse file server.

## <a name="see-also"></a>Vedere anche

-   [Opzioni di gestione risorse di impostazione File Server](setting-file-server-resource-manager-options.md)


