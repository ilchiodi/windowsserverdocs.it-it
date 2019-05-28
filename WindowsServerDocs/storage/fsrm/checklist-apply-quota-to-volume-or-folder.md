---
title: Applica una quota a un volume o una cartella
description: Questo articolo descrive come applicare una quota di archiviazione a un volume o una cartella
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: e937464ca3af1292de5fd63303ba4a430f831dcd
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/09/2019
ms.locfileid: "65475856"
---
# <a name="checklist-apply-a-quota-to-a-volume-or-folder"></a>Elenco di controllo: Applicare una Quota per un volume o una cartella

> Si applica a: Windows Server 2019, Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2, Windows Server (canale semestrale)

1. Configurare le impostazioni di posta elettronica se si prevede di inviare notifiche di soglia o rapporti di archiviazione tramite posta elettronica. [Configurare notifiche tramite posta elettronica](configure-email-notifications.md)

2. Valutare i requisiti di archiviazione sul volume o cartella. È possibile utilizzare i rapporti nel nodo **Gestione rapporti di archiviazione** per fornire i dati. (Ad esempio, eseguire un file dal report proprietario su richiesta per identificare gli utenti che utilizzano grandi quantità di spazio su disco.) [Generare rapporti su richiesta](generate-reports-on-demand.md)

3. Esaminare i modelli quota preconfigurati disponibili. (In **gestione delle quote**, fare clic sui **modelli quote** nodo.) [Modifica proprietà modello di Quota](edit-quota-template-properties.md) 
<br />-Oppure- <br /> Creare un nuovo modello quota per applicare un criterio di archiviazione all'interno dell'organizzazione. [Creare un modello Quota](create-quota-template.md)

4. Creare una quota in base al modello in un volume o una cartella.  
 [Creare una quota](create-quota.md) <br /> -Oppure- <br /> Creare una quota automatica per generare automaticamente le quote per le sottocartelle sul volume o cartella. [Creare una quota automatica](create-auto-apply-quota.md)

6. Pianificare un'attività rapporto contenente un rapporto Utilizzo quote per monitorare periodicamente l'utilizzo delle quote. [Pianificare una serie di rapporti](schedule-set-of-reports.md)

> [!Note]
> Se si desidera screening file su un volume o una cartella, vedere [elenco di controllo: Applicare uno screening dei File a un Volume o una cartella](checklist-apply-file-screen-to-volume-or-folder.md).











