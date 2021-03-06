---
title: Creare una proprietà di classificazione
description: Questo articolo descrive le proprietà di classificazione, utilizzate per assegnare i valori per i file all'interno di una cartella o un volume specificati.
ms.date: 7/7/2017
ms.prod: windows-server
ms.technology: storage
ms.topic: article
author: JasonGerend
manager: brianlic
ms.author: jgerend
ms.openlocfilehash: d330f896c71cced8e97701af2c1008b3531e065d
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394218"
---
# <a name="create-a-classification-property"></a>Creare una proprietà di classificazione

> Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012, Windows Server 2008 R2

Le proprietà di classificazione vengono utilizzate per assegnare i valori per i file all'interno di una cartella o un volume specificati. Esistono molti tipi di proprietà tra cui è possibile scegliere in base alle esigenze. Nella tabella seguente vengono definiti i tipi di proprietà disponibili.

|Proprietà | Descrizione |
| --- | --- |
| Sì/No | Una proprietà booleana che può essere **Sì** o **No**. Se si combinano più valori durante la classificazione o dal contenuto del file, un valore **No** verrà sostituito da un valore **Sì**. |
| Data e ora | Una semplice proprietà data/ora. Se si combinano più valori durante la classificazione o dal contenuto dei file, i valori in conflitto impediranno la nuova classificazione. |
| NUMBER | Una semplice proprietà numerica. Se si combinano più valori durante la classificazione o dal contenuto dei file, i valori in conflitto impediranno la nuova classificazione. |
| Elenco ordinato | Un elenco di valori fissi. È possibile assegnare a una proprietà un solo valore alla volta. Se si combinano più valori durante la classificazione o dal contenuto del file, verrà utilizzato il valore più altro dell'elenco. |
| Stringa | Una semplice proprietà di stringa. Se si combinano più valori durante la classificazione o dal contenuto dei file, i valori in conflitto impediranno la nuova classificazione. |
| Selezione multipla | Un elenco di valori che possono essere assegnati a una proprietà. È possibile assegnare a una proprietà più di un valore alla volta. Se si combinano più valori durante la classificazione o dal contenuto del file, verrà utilizzato ciascun valore in elenco. |
| Multistringa | Un elenco di stringhe che possono essere assegnate a una proprietà. È possibile assegnare a una proprietà più di un valore alla volta. Se si combinano più valori durante la classificazione o dal contenuto del file, verrà utilizzato ciascun valore in elenco. |

<br />

La seguente procedura descrive il processo di creazione di una proprietà di classificazione.

## <a name="to-create-a-classification-property"></a>Per creare una proprietà di classificazione

1.  In **Gestione classificazioni** fare clic sul nodo **Proprietà classificazione**.

2.  Fare clic con il pulsante destro del mouse su **Proprietà classificazione**, quindi fare clic su **Crea proprietà** (oppure fare clic su **Creare proprietà** nel riquadro **Azioni**). Verrà visualizzata la finestra di dialogo **Definizioni proprietà classificazione**.

3.  Digitare un nome per la proprietà nella casella di testo **Nome proprietà**.

4.  Nella casella di testo **Descrizione** aggiungere una descrizione opzionale per la proprietà.

5.  Nel menu a discesa **Tipo proprietà** selezionare un tipo di proprietà.

6.  Fare clic su **OK**.

## <a name="see-also"></a>Vedere anche

-   [Creare una regola di classificazione automatica](create-automatic-classification-rule.md)
-   [Gestione delle classificazioni](classification-management.md)