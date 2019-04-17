---
title: Gestire i modelli di criteri di rete
description: In questo argomento vengono fornite istruzioni su come creare, applicare, esportare e importare i modelli di criteri di rete per Server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 989b00c5-4767-4081-ace5-6321f8b2c55e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 0a7f4c50d87c155c1adcd445eae8df23aab7730b
ms.sourcegitcommit: 19d9da87d87c9eefbca7a3443d2b1df486b0b010
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/28/2018
---
# <a name="manage-nps-templates"></a>Gestire i modelli di criteri di rete

>Si applica a: Windows Server (canale annuale e virgola), Windows Server 2016

È possibile utilizzare Server dei criteri di rete \(NPS\) modelli per creare elementi di configurazione, ad esempio i client di Remote Authentication Dial-In User Service \(RADIUS\) o i segreti condivisi, è possibile riutilizzare sul server dei criteri di rete e l'esportazione per l'utilizzo in altri server dei criteri di rete locale. 

Gestione modelli fornisce un nodo nella console di criteri di rete in cui è possibile creare, modificare, eliminare, duplicare e visualizzare l'uso di modelli di criteri di rete. Modelli di criteri di rete sono progettati per ridurre la quantità di tempo e i costi necessari per configurare criteri di rete in uno o più server.

I seguenti tipi di modello dei criteri di rete sono disponibili per la configurazione in Gestione modelli.

- **Segreti condivisi**. Questo tipo di modello consente di specificare un segreto condiviso che è possibile riutilizzare (selezionando il modello nella posizione appropriata nella console di criteri di rete) quando si configurano i client e server RADIUS. 

- **Client RADIUS**. Questo tipo di modello consente di configurare le impostazioni di client RADIUS che è possibile riutilizzare selezionando il modello nella posizione appropriata nella console di criteri di rete.

- **Server RADIUS remoti**. Questo modello consente di configurare impostazioni del server RADIUS remote è possibile riutilizzare selezionando il modello nella posizione appropriata nella console di criteri di rete. 

- **Filtri IP**. Questo modello rende possibile per la creazione di protocollo Internet versione 4 (IPv4) e protocollo Internet versione 6 \(IPv6\) filtri che è possibile riutilizzare \ (selezionando il modello nella posizione appropriata nella console di criteri di rete) quando si configurano i criteri di rete.

## <a name="create-an-nps-template"></a>Creare un modello di criteri di rete

Configurazione di un modello è diversa dalla configurazione direttamente il server dei criteri di rete. Creazione di un modello non altera la funzionalità del server dei criteri di rete. È solo quando si seleziona il modello nella posizione appropriata nella console di criteri di rete e applicare il modello che il modello influisce sulla funzionalità server dei criteri di rete. 

Ad esempio, se si configura un client RADIUS nella console di criteri di rete in **client e server RADIUS**, modificare la configurazione del server dei criteri di rete e richiedere un passaggio della configurazione dei criteri di rete per comunicare con uno dei server di accesso di rete. \ (Il passaggio successivo consiste nel configurare il server di accesso di rete per comunicare con criteri di rete \(NAS\). \) 

Tuttavia, se si configura un nuovo **client RADIUS** modello nella console di criteri di rete in **Gestione modelli** invece di creare un nuovo client RADIUS in **client e server RADIUS**, è stato creato un modello, ma non è stato modificato ancora la funzionalità del server dei criteri di rete. Per modificare la funzionalità del server dei criteri di rete, è necessario applicare il modello dal percorso corretto nella console di criteri di rete.

La procedura seguente vengono fornite istruzioni su come creare un nuovo modello.

Appartenenza al gruppo **amministratori**, o equivalente è il requisito minimo necessario per completare questa procedura.

### <a name="to-create-an-nps-template"></a>Per creare un modello di criteri di rete


1. Nel server dei criteri di rete, in Server Manager, fare clic su **strumenti**, quindi fare clic su **Server dei criteri di rete**. Apre la console dei criteri di rete. 

2. Nella console di criteri di rete, espandere **Gestione modelli**, fare doppio clic su un tipo di modello, ad esempio **client RADIUS**, quindi fare clic su **New**.

3. Consente di aprire una finestra di dialogo di proprietà nuovo modello che è possibile utilizzare per configurare il modello.

## <a name="apply-an-nps-template"></a>Applicare un modello di criteri di rete

È possibile utilizzare un modello che è stato creato in **Gestione modelli** passando a una posizione nella console di criteri di rete in cui è possibile applicare il modello. Se si desidera applicare un modello di segreti condivisi per una configurazione di client RADIUS, ad esempio, è possibile utilizzare la procedura seguente.

Appartenenza al gruppo **amministratori**, o equivalente è il requisito minimo necessario per completare questa procedura.

### <a name="to-apply-an-nps-template"></a>Per applicare un modello di criteri di rete

1. Nel server dei criteri di rete, in Server Manager, fare clic su **strumenti**, quindi fare clic su **Server dei criteri di rete**. Apre la console dei criteri di rete.

2. Nella console di criteri di rete, espandere **client e server RADIUS**, quindi espandere **client RADIUS**.

3. In **client RADIUS**, nel riquadro dei dettagli fare doppio clic su client RADIUS a cui si desidera applicare il modello di criteri di rete, quindi fare clic su **proprietà**.

4. Nella finestra di dialogo Proprietà casella per il client RADIUS, in **selezionare un modello esistente segreti condivisi**, selezionare il modello che si desidera applicare dall'elenco dei modelli.

## <a name="export-or-import-nps-templates"></a>Esportazione o importazione di modelli di criteri di rete

È possibile esportare i modelli per l'utilizzo in altri server dei criteri di rete, oppure è possibile importare i modelli in **Gestione modelli** per l'uso nel computer locale. 

Appartenenza al gruppo **amministratori**, o equivalente è il requisito minimo necessario per completare questa procedura.

### <a name="to-export-or-import-nps-templates"></a>Per esportare o importare i modelli di criteri di rete

1. Per esportare i modelli di criteri di rete, nella console di criteri di rete, fare doppio clic su **Gestione modelli**, quindi fare clic su **Esporta modelli in un File**.

2. Per importare i modelli di criteri di rete, nella console di criteri di rete, fare doppio clic su **Gestione modelli**, quindi fare clic su **Importa modelli da un Computer** o **Importa modelli da un File**.


