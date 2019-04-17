---
title: Configurare il rapporto screening dei file
description: Questo articolo descrive come configurare lo screening dei file per generare il rapporto screening dei file
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: 89592a9e1f61374d2d909678a91dc4a06e0b1972
ms.sourcegitcommit: 583355400f6b0d880dc0ac6bc06f0efb50d674f7
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 10/17/2017
---
# <a name="configure-file-screen-audit"></a>Configurare il rapporto screening dei file

> Si applica a: Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Utilizzando Gestione risorse file server, è possibile registrare l'attività di screening dei file in un database di controllo. Le informazioni salvate in questo database vengono utilizzate per generare il rapporto screening dei file.

> [!Important]
> Se la casella di controllo **Registra l'attività di screening dei file nel database di controllo** è deselezionata, i rapporti screening dei file non conterranno alcuna informazione.

## <a name="to-configure-file-screen-audit"></a>Per configurare il rapporto screening dei file

1.  Nell'albero della console, fare clic con il pulsante destro del mouse su **Gestione risorse file server**, quindi scegliere **Configura opzioni**. Verrà visualizzata la finestra di dialogo **Gestione risorse file server**.

2.  Nella scheda **Screening dei file**, selezionare la casella di controllo **Registra l'attività di screening dei file nel database di controllo**.

3.  Fare clic su **OK**. Tutte le attività di screening dei file verranno ora archiviate nel database di controllo e possono essere visualizzate mediante la generazione di un rapporto screening dei file.

## <a name="see-also"></a>Vedere anche

-   [Impostazione delle opzioni di Gestione risorse file server](setting-file-server-resource-manager-options.md)
-   [Gestione rapporti di archiviazione](storage-reports-management.md)