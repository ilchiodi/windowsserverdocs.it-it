---
title: 'Elenco di controllo: applicare uno screening dei file a un volume o una cartella'
description: Questo articolo descrive come applicare uno screening dei file a un volume o una cartella
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 03325d8a65d88c35f09985223608fc7474a0fde5
ms.sourcegitcommit: ed27ddbe316d543b7865bc10590b238290a2a1ad
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 05/09/2019
ms.locfileid: "65475845"
---
# <a name="checklist---apply-a-file-screen-to-a-volume-or-folder"></a>Elenco di controllo: applicare uno screening dei file a un volume o una cartella

> Si applica a: Windows Server 2019, Windows Server 2016, Windows Server (canale semestrale), Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Per applicare uno screening dei file a un volume o una cartella, utilizzare l'elenco seguente:
1. Configurare le impostazioni di posta elettronica se si intende inviare le notifiche di screening dei file o i rapporti archiviazione tramite posta elettronica seguendo le istruzioni in [Configurare le notifiche tramite posta elettronica](configure-email-notifications.md).

2. Abilitare la registrazione degli eventi di screening dei file nel database di controllo se si prevede di generare rapporti screening dei.
[Configurare il rapporto screening dei file](configure-file-screen-audit.md)

3. Valutare quali sono i tipi di file archiviati candidati per le regole di screening. È possibile utilizzare i rapporti nel nodo **Gestione rapporti di archiviazione** per fornire i dati. (Ad esempio, eseguire un file dal filegroup report o un file di grandi dimensioni su richiesta per identificare i file che occupano grandi quantità di spazio su disco.) [Generare rapporti su richiesta](generate-reports-on-demand.md) 

4. Esaminare i gruppi di file preconfigurati oppure creare un nuovo gruppo di file per applicare un criterio di screening specifico all'interno dell'organizzazione. [Definire gruppi di file per lo screening](define-file-groups-for-screening.md)  

5. Esaminare le proprietà dei modelli di screening dei file disponibili. (In **Gestione screening dei File**, fare clic sui **modelli screening** nodo.) [Modifica proprietà modello di schermata](edit-file-screen-template-properties.md) 
<br />
 -Oppure-
 <br /> Creare un nuovo modello di screening dei file per applicare un criterio di archiviazione all'interno dell'organizzazione.  [Creare un modello di screening dei file](create-file-screen-template.md) 

6. Creare uno screening dei file in base al modello in un volume o una cartella. 
 [Creare uno screening dei File](create-file-screen.md)
 
7. Configurare le eccezioni screening dei file nelle sottocartelle del volume o della cartella. [Creare un'eccezione screening dei file](create-file-screen-exception.md) 

8. Pianificare un'attività rapporto contenente un rapporto screening dei file per monitorare periodicamente l'attività di screening.
  [Pianificare una serie di rapporti](schedule-set-of-reports.md)


> [!NOTE]
> Per limitare l'archiviazione su un volume o una cartella, vedere [elenco di controllo: applicare una quota a un volume o una cartella](checklist-apply-file-screen-to-volume-or-folder.md)