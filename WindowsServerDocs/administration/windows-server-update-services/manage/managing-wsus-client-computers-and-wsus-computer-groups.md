---
title: Gestione dei computer client WSUS e dei gruppi di computer WSUS
description: Argomento di Windows Server Update Service (WSUS) - come gestire i computer client e i gruppi
ms.custom: na
ms.prod: windows-server-threshold
ms.reviewer: na
ms.suite: na
ms.technology: manage-wsus
ms.tgt_pltfrm: na
ms.topic: article
ms.assetid: 4b1ea915-0f9f-4f0e-8913-a1dd460d07ab
author: coreyp-at-msft
ms.author: coreyp
manager: lizapo
ms.date: 10/16/2017
ms.openlocfilehash: e63aa5d53f01fbff3029c6896556d92c9c99aa50
ms.sourcegitcommit: 0d0b32c8986ba7db9536e0b8648d4ddf9b03e452
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/17/2019
ms.locfileid: "59857682"
---
# <a name="managing-wsus-client-computers-and-wsus-computer-groups"></a>Gestione dei computer client WSUS e dei gruppi di computer WSUS

>Si applica a: Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Il nodo computer è il punto di accesso centrale nella console di amministrazione di WSUS per la gestione dei dispositivi e i computer client WSUS. In questo nodo è possibile trovare i diversi gruppi che è stato configurato (più il gruppo predefinito, i computer non assegnati).

## <a name="managing-client-computers"></a>La gestione dei computer Client
Se si seleziona uno dei gruppi di computer nel **computer** nodo **opzioni** fa sì che i computer nel gruppo da visualizzare nel riquadro dei dettagli. Se un computer viene assegnato a più gruppi, verrà visualizzato negli elenchi di entrambi i gruppi. Se si seleziona un computer nell'elenco, è possibile visualizzare le relative proprietà, che includono informazioni generali sui computer e lo stato degli aggiornamenti, ad esempio l'installazione o il rilevamento stato di un aggiornamento per un determinato computer. È possibile filtrare l'elenco dei computer in un gruppo di computer specificato in base allo stato. L'impostazione predefinita mostra solo i computer per gli aggiornamenti necessari o che hanno avuto gli errori di installazione; Tuttavia, è possibile filtrare la visualizzazione da qualsiasi stato. Fare clic su **Aggiorna** dopo aver modificato il filtro dello stato.

È anche possibile gestire i gruppi di computer nella pagina computer, che include la creazione di gruppi e assegnare i computer ad essi. Per altre informazioni sulla gestione dei gruppi di computer, vedere Gestione dei gruppi di computer nella prossima sezione di questa Guida, sezione e [1.5. Pianificare gruppi di computer WSUS](../plan/plan-your-wsus-deployment.md#BKMK_1.5) nel passaggio 1: Preparare la distribuzione WSUS di Guida alla distribuzione di WSUS.

> [!NOTE]
> È innanzitutto necessario configurare i computer client per contattare il server WSUS prima di poterli gestire da tale server. Fino a quando non si esegue questa attività, il server WSUS non riconosce i computer client e non essere visualizzati nell'elenco nella pagina computer. Per altre informazioni sulla configurazione di computer client, vedere [1.5. Pianificare gruppi di computer WSUS](../plan/plan-your-wsus-deployment.md#BKMK_1.5) del passaggio 1: Preparare la distribuzione di WSUS e passaggio 3: Configurazione di WSUS, nella Guida alla distribuzione di WSUS.

## <a name="controlling-when-wsus-client-computers-install-updates"></a>Controllare quando i computer client WSUS installano gli aggiornamenti
Esistono due metodi per controllare quando i computer client WSUS installano gli aggiornamenti:

-   Le scadenze di approvazione: Le scadenze applicano in modo rigoroso quando viene installato un aggiornamento

-   Criteri di gruppo WSUS: I criteri di gruppo di controllo quando l'agente di aggiornamento di Windows esegue l'analisi e installa gli aggiornamenti

    Per altre informazioni, vedere: [Passaggio 5: Configurare le impostazioni di criteri di gruppo per gli aggiornamenti automatici](../deploy/4-configure-group-policy-settings-for-automatic-updates.md), nella Guida alla distribuzione di WSUS.

## <a name="managing-computer-groups"></a>Gestione dei gruppi di computer
Windows Server Update Services consente di applicare gli aggiornamenti a gruppi di computer client in modo da garantire che determinati computer ottengano sempre gli aggiornamenti corretti negli orari più appropriati. Se, ad esempio, tutti i computer del reparto Contabilità sono configurati in modo specifico, è possibile impostare un gruppo per il reparto, stabilire gli aggiornamenti necessari per i computer e l'orario in cui installarli, quindi utilizzare i rapporti WSUS per valutare gli aggiornamenti effettuati.

I computer sono sempre assegnati al **tutti i computer** raggruppare e rimangono assegnati al **computer non assegnati** raggruppare fino a quando non vengono assegnati a un altro gruppo. I computer possono appartenere a più di un gruppo.

È possibile impostare gruppi di computer in gerarchie, ad esempio i gruppi Retribuzioni e Contabilità fornitori possono essere contenuti nel gruppo Contabilità. Gli aggiornamenti approvati per un gruppo superiore verranno automaticamente distribuiti a gruppi di livello inferiore, oltre che al gruppo successiva di se stesso. Di conseguenza, se si approva Update1 per il gruppo Contabilità, l'aggiornamento verrà distribuito a tutti i computer a gruppo Contabilità, tutti i computer in gruppi retribuzioni e tutti i computer nel gruppo Contabilità.

Dal momento che i computer possono essere assegnati a più gruppi, è possibile che un singolo aggiornamento venga approvato più volte per lo stesso computer. L'aggiornamento sarà tuttavia distribuito una sola volta e gli eventuali conflitti saranno risolti dal server WSUS. Per continuare con l'esempio precedente, se computerA è assegnato a delle retribuzioni e i gruppi di contabilità fornitori e Update1 viene approvato per entrambi i gruppi, in cui verrà distribuito una sola volta.

È possibile assegnare i computer a gruppi di computer utilizzando uno dei due metodi denominati rispettivamente destinazione lato server e destinazione lato client. Con la destinazione lato server, si sposta manualmente uno o più computer client in un gruppo di computer alla volta. Con la destinazione lato client si utilizzano i Criteri di gruppo o si modificano le impostazioni del Registro di sistema nei computer client per abilitare l'aggiunta automatica di tali computer ai gruppi di computer creati in precedenza. Questo processo può essere inserito nello script e distribuito in più computer contemporaneamente. È necessario specificare il metodo di destinazione verrà usato nel server WSUS selezionando una delle due opzioni nel **computer** sezione il **opzioni** pagina.

> [!NOTE]
> Se un server WSUS viene eseguito in modalità di replica, non è possibile creare gruppi di computer. Tutti i gruppi di computer necessari per i client del server di replica devono essere creati nel server WSUS radice della gerarchia di server WSUS. Per altre informazioni sulla modalità di replica, vedere [modalità di Replica WSUS in esecuzione](running-wsus-replica-mode.md) e per ulteriori informazioni sulla destinazione lato client e lato server, vedere sezione [1.5. Pianificare gruppi di computer WSUS](../plan/plan-your-wsus-deployment.md#BKMK_1.5) del passaggio 1: Preparare la distribuzione WSUS nella Guida alla distribuzione di WSUS.


