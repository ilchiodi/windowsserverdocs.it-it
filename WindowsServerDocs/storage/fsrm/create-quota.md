---
title: Creare una quota
description: Questo articolo descrive come creare una quota in base a un modello
ms.date: 7/7/2017
ms.prod: windows-server-threshold
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: f3c677f5ebf7dda44f4b99a64d0fbf8d2c72b92e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59883192"
---
# <a name="create-a-quota"></a>Creare una quota

> Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Le quote possono essere create da un modello o con le proprietà personalizzate. La procedura seguente descrive come creare una quota che si basa su un modello (scelta consigliata). Se è necessario creare una quota con proprietà personalizzate, è possibile salvare queste proprietà come un modello da riutilizzare in un secondo momento.

Quando si crea una quota, è possibile scegliere un percorso di quota, ovvero un volume o una cartella a cui si applica il limite di archiviazione. In un determinato percorso di quota, è possibile utilizzare un modello per creare uno dei seguenti tipi di quota:

-   Una singola quota che limita lo spazio per un intero volume o cartella.
-   Una quota automatica, che assegna il modello quota a una cartella o un volume. Le quote basate su questo modello vengono automaticamente generate e applicate a tutte le sottocartelle. Per ulteriori informazioni sulla creazione delle quote automatiche, vedere [Creare una quota automatica](create-auto-apply-quota.md).


> [!Note]
> Se le quote vengono create esclusivamente in base a modelli, è possibile gestirle in modo centralizzato aggiornando i modelli anziché le singole quote. Quindi, è possibile applicare le modifiche a tutte le quote basate sul modello modificato. Questa funzionalità semplifica l'implementazione delle modifiche ai criteri di archiviazione offrendo un'unica posizione centrale in cui eseguire tutti gli aggiornamenti.

## <a name="to-create-a-quota-that-is-based-on-a-template"></a>Per creare una quota basata su un modello

1.  In **Gestione quote** fare clic sul nodo **Modelli quota**.

2.  Nel riquadro dei risultati, selezionare il modello sul quale basare la nuova quota.

3.  Fare clic con il pulsante destro sul modello e fare clic su **Crea quota da un modello** (oppure selezionare **Crea quota da un modello** nel riquadro **Azioni**). Verrà visualizzata la finestra di dialog **crea Quota** con le proprietà di riepilogo del modello quota visualizzato.

4.  In **Percorso quota**, digitare o selezionare il nome della cartella alla quale verrà applicata la quota.

5.  Fare clic sull'opzione **Crea quota nel percorso**. Si noti che le proprietà della quota verranno applicate all'intera cartella.

     > [!Note]
     > Per creare una quota automatica, fare clic sull'opzione **Applica modello e crea quote nelle sottocartelle nuove ed esistenti**. Per ulteriori informazioni sulle quote automatiche, vedere [Creare una quota automatica](create-auto-apply-quota.md).

6.  In **Deriva proprietà dal modello quota seguente**, il modello utilizzato nel passaggio 2 per creare la nuova quota è preselezionato (o è possibile selezionare un altro modello dall'elenco). Si noti che le proprietà del modello vengono visualizzate in **Riepilogo proprietà quota**.

7.  Fare clic su **Crea**.

## <a name="see-also"></a>Vedere anche

-   [Quota Management](quota-management.md)
-   [Creare un'Auto della Quota di applicazione](create-auto-apply-quota.md)
-   [Creare un modello Quota](create-quota-template.md)


