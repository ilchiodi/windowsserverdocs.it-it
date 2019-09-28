---
title: Configurare i rapporti di archiviazione
description: Questo articolo descrive come configurare i parametri predefiniti per i rapporti di archiviazione
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: d3500f4ea4fc264f3cb663f17c3a50439b9cb454
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394257"
---
# <a name="configure-storage-reports"></a>Configurare i rapporti di archiviazione

> Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

È possibile configurare i parametri predefiniti per i rapporti di archiviazione. Questi parametri predefiniti vengono utilizzati per i rapporti operazioni non consentite generati quando si verifica un evento di screening dei file o quota. Vengono inoltre utilizzati per i rapporti pianificati e su richiesta ed è possibile sostituire i parametri predefiniti quando si definiscono le proprietà specifiche di questi rapporti.

> [!Important]
> Quando si modificano i parametri predefiniti per un tipo di rapporto, le modifiche interessano tutti i rapporti operazioni non consentite e le attività rapporto pianificate esistenti che utilizzano le impostazioni predefinite.

## <a name="to-configure-the-default-parameters-for-storage-reports"></a>Per configurare i parametri predefiniti per i rapporti di archiviazione.

1. Nell'albero della console, fare clic con il pulsante destro del mouse su **Gestione risorse file server**, quindi scegliere **Configura opzioni**. Verrà visualizzata la finestra di dialogo **Gestione risorse file server**.

2. Nella scheda **Rapporti di archiviazione**, in **Configura parametri predefiniti**, selezionare il tipo di rapporto che si desidera modificare.

3. Fare clic su **Modifica parametri**.

4. A seconda del tipo di rapporto selezionato, saranno disponibili diversi parametri dei rapporti per la modifica. Eseguire tutte le modifiche necessarie, quindi fare clic su **OK** per salvarle come parametri predefiniti per quel tipo di rapporto.

5.  Ripetere i passaggi da 2 a 4 per ciascun tipo di rapporto che si desidera modificare.

6. Per visualizzare un elenco di parametri predefiniti per tutti i rapporti, fare clic su **Visualizza rapporti** e quindi fare clic su **Chiudi**.

7.  Fare clic su **OK**.

## <a name="see-also"></a>Vedere anche

-   [Impostazione delle opzioni di Gestione risorse file server](setting-file-server-resource-manager-options.md)
-   [Gestione rapporti di archiviazione](storage-reports-management.md)