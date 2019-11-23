---
title: Modelli Server dei criteri di rete
description: Questo argomento fornisce una panoramica dei modelli di server dei criteri di rete in Windows Server 2016.
manager: brianlic
ms.prod: windows-server
ms.technology: networking
ms.topic: article
ms.assetid: fdfc0df1-21c7-492c-9fad-38fe9c7d935a
ms.author: pashort
author: shortpatti
ms.openlocfilehash: bafff7a6a15312ab1bdca2e7b98307bc94731d3a
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71405313"
---
# <a name="nps-templates"></a>Modelli Server dei criteri di rete

>Si applica a: Windows Server (Canale semestrale), Windows Server 2016

Server dei criteri di rete \(NPS\) modelli consentono di creare elementi di configurazione, ad esempio Remote Authentication Dial-In User Service \(RADIUS\) client o segreti condivisi, che è possibile riutilizzare nel server dei criteri di rete locale ed esportare per l'utilizzo in altri NPSs.

I modelli NPS sono progettati per ridurre la quantità di tempo e costi necessari per configurare NPS in uno o più server. I tipi di modello NPS seguenti sono disponibili per la configurazione nella gestione dei modelli:

- Segreti condivisi
- Client RADIUS
- Server RADIUS remoti
- Filtri IP
- Gruppi di server di monitoraggio e aggiornamento

La configurazione di un modello è diversa rispetto alla configurazione diretta di NPS. La creazione di un modello non influisce sulla funzionalità del server dei criteri di server. È solo quando si seleziona il modello nella posizione appropriata nella console NPS che il modello influisca sulla funzionalità NPS. 

Ad esempio, se si configura un client RADIUS nella console server dei criteri di rete in client e server RADIUS, è stata modificata la configurazione del server dei criteri di rete ed è stato effettuato un passaggio nella configurazione del server dei criteri di rete per comunicare con uno dei server di accesso alla rete \(\)del NAS. \(il passaggio successivo consiste nel configurare il server NAS per la comunicazione con NPS.\) tuttavia, se si configura un nuovo modello di client RADIUS nella console NPS in **Gestione modelli** anziché creare un nuovo client RADIUS in **client e server RADIUS**, è stato creato un modello, ma non è ancora stata modificata la funzionalità NPS. Per modificare la funzionalità NPS, è necessario selezionare il modello dalla posizione corretta nella console NPS.

## <a name="creating-templates"></a>Creazione di modelli

Per creare un modello, aprire la console NPS, fare clic con il pulsante destro del mouse su un tipo di modello, ad esempio **filtri IP**, quindi fare clic su **nuovo**. Verrà visualizzata una finestra di dialogo Proprietà nuovo modello che consente di configurare il modello.

## <a name="using-templates-locally"></a>Utilizzo di modelli localmente

È possibile usare un modello creato in **Gestione modelli** passando a una posizione nella console NPS in cui è possibile applicare il modello. Se ad esempio si crea un nuovo modello di segreti condivisi da applicare alla configurazione di un client RADIUS, in client **e server RADIUS** e **client RADIUS**aprire le proprietà del client RADIUS. In **selezionare un modello di segreti condivisi esistente**selezionare il modello creato in precedenza dall'elenco dei modelli disponibili.

Per ulteriori informazioni su NPS, vedere [Server dei criteri di rete (NPS)](nps-top.md).
