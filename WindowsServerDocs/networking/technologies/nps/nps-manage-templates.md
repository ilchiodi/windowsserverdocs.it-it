---
title: Gestire i modelli del Server dei criteri di rete
description: Questo argomento fornisce istruzioni su come creare, applicare, esportare e importare i modelli di criteri di rete per Server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server-threshold
ms.technology: networking
ms.topic: article
ms.assetid: 989b00c5-4767-4081-ace5-6321f8b2c55e
ms.author: pashort
author: shortpatti
ms.openlocfilehash: 170a0e86f7dcca77c6efe841318b522554f8e78e
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59845132"
---
# <a name="manage-nps-templates"></a>Gestire i modelli del Server dei criteri di rete

>Si applica a: Windows Server (canale semestrale), Windows Server 2016

È possibile utilizzare Server dei criteri di rete \(NPS\) modelli per creare elementi di configurazione, ad esempio Remote Authentication Dial-In User Service \(RADIUS\) client o i segreti condivisi, che è possibile riutilizzare in locale Criteri di rete e l'esportazione per l'utilizzo in altri NPSs. 

Gestione modelli fornisce un nodo nella console di NPS in cui è possibile creare, modificare, eliminare, duplicati e visualizzare l'utilizzo dei modelli di criteri di rete. Modelli di criteri di rete sono progettati per ridurre la quantità di tempo e i costi necessari per configurare criteri di rete in uno o più server.

I tipi di modello dei criteri di rete seguenti sono disponibili per la configurazione in Gestione modelli.

- **Segreti condivisi**. Questo tipo di modello consente di specificare un segreto condiviso che è possibile riutilizzare (selezionando il modello nella posizione appropriata nella console di NPS) quando si configurano i client e server RADIUS. 

- **I client RADIUS**. Questo tipo di modello consente di configurare le impostazioni di client RADIUS che è possibile riutilizzare selezionando il modello nella posizione appropriata nella console Criteri di rete.

- **Server RADIUS remoti**. Questo modello rende possibile per la configurazione delle impostazioni del server RADIUS remote che è possibile riutilizzare selezionando il modello nella posizione appropriata nella console Criteri di rete. 

- **I filtri IP**. Questo modello consente di creare protocollo Internet versione 4 (IPv4) e Internet Protocol versione 6 \(IPv6\) filtri che è possibile riutilizzare \(selezionando il modello nella posizione appropriata in NPS console\) quando si configurano i criteri di rete.

## <a name="create-an-nps-template"></a>Creare un modello di criteri di rete

Configurazione di un modello è diversa rispetto alla configurazione direttamente i criteri di rete. Creazione di un modello non incide sulla funzionalità di NPS. È solo quando si seleziona il modello nella posizione appropriata nella console Criteri di rete e si applica il modello che il modello influisce sulla funzionalità dei criteri di rete. 

Ad esempio, se si configura un client RADIUS nella console Criteri di rete in **client RADIUS e server**, si modifica la configurazione dei criteri di rete e richiedere un passaggio della configurazione dei criteri di rete per comunicare con uno dei server di accesso di rete. \(Il passaggio successivo consiste nel configurare il server di accesso di rete \(NAS\) per comunicare con criteri di rete.\) 

Tuttavia, se si configura una nuova **client RADIUS** nella console Criteri di rete nel modello **modelli di gestione** invece di creare un nuovo client RADIUS in **client RADIUS e server**, è stato creato un modello, ma non è stato modificato ancora la funzionalità dei criteri di rete. Per modificare la funzionalità dei criteri di rete, è necessario applicare il modello dal percorso corretto nella console Criteri di rete.

La procedura seguente fornisce istruzioni su come creare un nuovo modello.

L'appartenenza a **amministratori**, o equivalente è il requisito minimo necessario per completare questa procedura.

### <a name="to-create-an-nps-template"></a>Creare un modello di criteri di rete


1. Su NPS, in Server Manager fare clic su **degli strumenti**, quindi fare clic su **Server dei criteri di rete**. Apre la console Criteri di rete. 

2. Nella console Criteri di rete, espandere **modelli di gestione**, fare doppio clic su un tipo di modello, ad esempio **client RADIUS**, quindi fare clic su **New**.

3. Consente di aprire una nuova finestra di dialogo proprietà di modello che è possibile usare per configurare il modello.

## <a name="apply-an-nps-template"></a>Applicare un modello di criteri di rete

È possibile usare un modello creato nel **modelli di gestione** passando a un percorso nella console di NPS in cui è possibile applicare il modello. Ad esempio, se si desidera applicare un modello dei segreti condivisi per una configurazione di client RADIUS, è possibile utilizzare la procedura seguente.

L'appartenenza a **amministratori**, o equivalente è il requisito minimo necessario per completare questa procedura.

### <a name="to-apply-an-nps-template"></a>Per applicare un modello di criteri di rete

1. Su NPS, in Server Manager fare clic su **degli strumenti**, quindi fare clic su **Server dei criteri di rete**. Apre la console Criteri di rete.

2. Nella console Criteri di rete, espandere **client e server RADIUS**, quindi espandere **client RADIUS**.

3. nel **client RADIUS**, nel riquadro dei dettagli, fare doppio clic su client RADIUS in cui si desidera applicare il modello di criteri di rete e quindi fare clic su **proprietà**.

4. Nella finestra di dialogo Proprietà finestra per il client RADIUS, in **selezionare un modello esistente di segreti condivisi**, selezionare il modello che si desidera applicare l'elenco dei modelli.

## <a name="export-or-import-nps-templates"></a>Esportazione o importazione di modelli di criteri di rete

È possibile esportare i modelli per l'utilizzo in altri NPSs, oppure è possibile importare i modelli in **modelli di gestione** per l'uso nel computer locale. 

L'appartenenza a **amministratori**, o equivalente è il requisito minimo necessario per completare questa procedura.

### <a name="to-export-or-import-nps-templates"></a>Per esportare o importare i modelli di criteri di rete

1. Per esportare i modelli di criteri di rete, nella console Criteri di rete, fare doppio clic su **modelli di gestione**, quindi fare clic su **Esporta modelli in un File**.

2. Per importare i modelli di criteri di rete, nella console Criteri di rete, fare doppio clic su **modelli di gestione**, quindi fare clic su **Importa modelli da un Computer** o **Importa modelli da un File**.


