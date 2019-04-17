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
ms.openlocfilehash: 62910af666fb16db5c2e7a30b49eedfa8c12cacb
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="checklist-apply-a-quota-to-a-volume-or-folder"></a>Elenco di controllo: applicare una quota a un volume o una cartella

> Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

1. Configurare le impostazioni di posta elettronica se si prevede di inviare notifiche di soglia o rapporti di archiviazione tramite posta elettronica. [Configurare le notifiche tramite posta elettronica](configure-email-notifications.md)

2. Valutare i requisiti di archiviazione sul volume o cartella. È possibile utilizzare i rapporti nel nodo **Gestione rapporti di archiviazione** per fornire i dati. Ad esempio, eseguire un rapporto file in base a proprietario su richiesta per identificare gli utenti che utilizzano grandi quantità di spazio su disco. [Generare rapporti su richiesta](generate-reports-on-demand.md)

3. Esaminare i modelli quota preconfigurati disponibili. In **Gestione quote**, fare clic sul nodo **Modelli quota**. [Modificare le proprietà dei modelli quota](edit-quota-template-properties.md) 
<br />-Oppure- <br /> Creare un nuovo modello quota per applicare un criterio di archiviazione all'interno dell'organizzazione. [Creare un modello quota](create-quota-template.md)

4. Creare una quota in base al modello in un volume o una cartella.  
 [Creare una quota](create-quota.md) <br /> -Oppure- <br /> Creare una quota automatica per generare automaticamente le quote per le sottocartelle sul volume o cartella. [Creare una quota automatica](create-auto-apply-quota.md)

6. Pianificare un'attività rapporto contenente un rapporto Utilizzo quote per monitorare periodicamente l'utilizzo delle quote. [Pianificare un set di rapporti](schedule-set-of-reports.md)

> [!Note]
> Se si desidera eseguire lo screening dei file su un volume o una cartella, vedere [Elenco di controllo: applicare uno screening dei file a un volume o una cartella](checklist-apply-file-screen-to-volume-or-folder.md).











