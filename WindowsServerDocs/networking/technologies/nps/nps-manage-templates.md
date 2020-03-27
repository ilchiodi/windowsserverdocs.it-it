---
title: Gestire i modelli del Server dei criteri di rete
description: In questo argomento vengono fornite istruzioni su come creare, applicare, esportare e importare modelli NPS per server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: 989b00c5-4767-4081-ace5-6321f8b2c55e
ms.author: lizross
author: eross-msft
ms.openlocfilehash: 0e6a35640d6a4a7c5665f3638925d9dc99a9df9d
ms.sourcegitcommit: da7b9bce1eba369bcd156639276f6899714e279f
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 03/26/2020
ms.locfileid: "80315935"
---
# <a name="manage-nps-templates"></a>Gestire i modelli del Server dei criteri di rete

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

È possibile utilizzare i modelli server dei criteri di rete \(NPS\) per creare elementi di configurazione, ad esempio Remote Authentication Dial-In User Service \(RADIUS\) client o segreti condivisi, che è possibile riutilizzare nel server dei criteri di rete locale ed esportare per l'utilizzo in altri NPSs. 

Gestione modelli fornisce un nodo nella console NPS in cui è possibile creare, modificare, eliminare, duplicare e visualizzare l'uso dei modelli NPS. I modelli NPS sono progettati per ridurre la quantità di tempo e costi necessari per configurare NPS in uno o più server.

I tipi di modello NPS seguenti sono disponibili per la configurazione nella gestione dei modelli.

- **Segreti condivisi**. Questo tipo di modello consente di specificare un segreto condiviso che può essere riutilizzato (selezionando il modello nella posizione appropriata nella console NPS) quando si configurano i client e i server RADIUS. 

- **Client RADIUS**. Questo tipo di modello consente di configurare le impostazioni client RADIUS che è possibile riutilizzare selezionando il modello nella posizione appropriata nella console NPS.

- **Server RADIUS remoti**. Questo modello consente di configurare le impostazioni del server RADIUS remoto che è possibile riutilizzare selezionando il modello nella posizione appropriata nella console NPS. 

- **Filtri IP**. Questo modello consente di creare i filtri IPv4 (Internet Protocol versione 4) e protocollo Internet versione 6 \(\) IPv6 che è possibile riutilizzare \(selezionando il modello nel percorso appropriato nella console server dei criteri di rete\) quando si configurano i criteri di rete.

## <a name="create-an-nps-template"></a>Creare un modello NPS

La configurazione di un modello è diversa rispetto alla configurazione diretta di NPS. La creazione di un modello non influisce sulla funzionalità del server dei criteri di server. È solo quando si seleziona il modello nella posizione appropriata nella console NPS e si applica il modello che il modello influisca sulla funzionalità NPS. 

Se ad esempio si configura un client RADIUS nella console server dei criteri di rete in **client e server RADIUS**, si modifica la configurazione del server dei criteri di rete e si esegue un passaggio nella configurazione di NPS per comunicare con uno dei server di accesso alla rete. \(il passaggio successivo consiste nel configurare il server di accesso alla rete \(\) NAS per la comunicazione con NPS.\) 

Tuttavia, se si configura un nuovo modello di **client RADIUS** nella console NPS in **Gestione modelli** anziché creare un nuovo client RADIUS in **client e server RADIUS**, è stato creato un modello, ma non è ancora stata modificata la funzionalità NPS. Per modificare la funzionalità NPS, è necessario applicare il modello dalla posizione corretta nella console NPS.

Nella procedura riportata di seguito vengono fornite istruzioni su come creare un nuovo modello.

Per poter completare questa procedura, è richiesta almeno l'appartenenza al gruppo **Administrators** o a un gruppo equivalente.

### <a name="to-create-an-nps-template"></a>Per creare un modello NPS


1. Nel server dei criteri di rete, in Server Manager, fare clic su **strumenti**e quindi su **Server dei criteri di rete**. Si apre la console NPS. 

2. Nella console NPS espandere **Gestione modelli**, fare clic con il pulsante destro del mouse su un tipo di modello, ad esempio **client RADIUS**, quindi fare clic su **nuovo**.

3. Verrà visualizzata una finestra di dialogo nuova proprietà modello che è possibile usare per configurare il modello.

## <a name="apply-an-nps-template"></a>Applicare un modello NPS

È possibile usare un modello creato in **Gestione modelli** passando a un percorso nella console NPS in cui è possibile applicare il modello. Se, ad esempio, si desidera applicare un modello di segreti condivisi a una configurazione client RADIUS, è possibile utilizzare la procedura riportata di seguito.

Per poter completare questa procedura, è richiesta almeno l'appartenenza al gruppo **Administrators** o a un gruppo equivalente.

### <a name="to-apply-an-nps-template"></a>Per applicare un modello NPS

1. Nel server dei criteri di rete, in Server Manager, fare clic su **strumenti**e quindi su **Server dei criteri di rete**. Si apre la console NPS.

2. Nella console NPS espandere **client e server RADIUS**, quindi espandere **client RADIUS**.

3.In **client RADIUS**, nel riquadro dei dettagli, fare clic con il pulsante destro del mouse sul client RADIUS a cui si desidera applicare il modello NPS, quindi scegliere **Proprietà**.

4. Nella finestra di dialogo proprietà per il client RADIUS, in **selezionare un modello di segreti condivisi esistente**selezionare il modello che si desidera applicare dall'elenco di modelli.

## <a name="export-or-import-nps-templates"></a>Esportare o importare modelli NPS

È possibile esportare i modelli per l'uso in altri NPSs oppure è possibile importare i modelli nella **gestione dei modelli** per l'uso nel computer locale. 

Per poter completare questa procedura, è richiesta almeno l'appartenenza al gruppo **Administrators** o a un gruppo equivalente.

### <a name="to-export-or-import-nps-templates"></a>Per esportare o importare modelli NPS

1. Per esportare i modelli NPS, nella console server dei criteri di server fare clic con il pulsante destro del mouse su **Gestione modelli**, quindi scegliere **Esporta modelli in un file**.

2. Per importare i modelli NPS, nella console server dei criteri di server fare clic con il pulsante destro del mouse su **Gestione modelli**, quindi scegliere **Importa modelli da un computer** o **Importa modelli da un file**.


