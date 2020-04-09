---
title: Gestione dei computer client WSUS e dei gruppi di computer WSUS
description: Argomento Windows Server Update Service (WSUS)-come gestire i computer e i gruppi client
ms.prod: windows-server
ms.technology: manage-wsus
ms.topic: article
ms.assetid: 4b1ea915-0f9f-4f0e-8913-a1dd460d07ab
author: coreyp-at-msft
ms.author: coreyp
manager: lizapo
ms.date: 10/16/2017
ms.openlocfilehash: 3ac5a0c09d28de53430ded651b6da92ba92a1f68
ms.sourcegitcommit: b00d7c8968c4adc8f699dbee694afe6ed36bc9de
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 04/08/2020
ms.locfileid: "80828626"
---
# <a name="managing-wsus-client-computers-and-wsus-computer-groups"></a>Gestione dei computer client WSUS e dei gruppi di computer WSUS

>Si applica a: Windows Server 2019, Windows Server (canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

Il nodo computer è il punto di accesso centrale nella console di amministrazione di WSUS per la gestione di computer e dispositivi client WSUS. In questo nodo è possibile trovare i diversi gruppi impostati (più il gruppo predefinito, computer non assegnati).

## <a name="managing-client-computers"></a>Gestione dei computer client
Se si seleziona uno dei gruppi di computer nel nodo **computer** in **Opzioni** , i computer del gruppo verranno visualizzati nel riquadro dei dettagli. Se un computer viene assegnato a più gruppi, verrà visualizzato negli elenchi di entrambi i gruppi. Se si seleziona un computer nell'elenco, è possibile visualizzarne le proprietà, che includono i dettagli generali sul computer e lo stato degli aggiornamenti, ad esempio lo stato di installazione o rilevamento di un aggiornamento per un determinato computer. È possibile filtrare l'elenco di computer in un determinato gruppo di computer in base allo stato. Il valore predefinito Mostra solo i computer per cui sono necessari aggiornamenti o per i quali si sono verificati errori di installazione. Tuttavia, è possibile filtrare la visualizzazione in base a qualsiasi stato. Fare clic su **Aggiorna** dopo aver modificato il filtro di stato.

È inoltre possibile gestire i gruppi di computer nella pagina computer, che include la creazione di gruppi e l'assegnazione di computer a tali gruppi. Per ulteriori informazioni sulla gestione dei gruppi di computer, vedere Gestione dei gruppi di computer nella sezione successiva di questa guida e sezione [1,5. Pianificare i gruppi di computer WSUS](../plan/plan-your-wsus-deployment.md#15-plan-wsus-computer-groups) in passaggio 1: preparare la distribuzione WSUS della Guida alla distribuzione di WSUS.

> [!NOTE]
> È innanzitutto necessario configurare i computer client in modo che contattino il server WSUS prima di poterli gestire da tale server. Fino a quando non si esegue questa attività, il server WSUS non riconoscerà i computer client e non verranno visualizzati nell'elenco della pagina computer. Per ulteriori informazioni sulla configurazione dei computer client, vedere [1,5. Pianificare i gruppi di computer WSUS](../plan/plan-your-wsus-deployment.md#15-plan-wsus-computer-groups) del passaggio 1: preparare la distribuzione WSUS e passaggio 3: configurare WSUS nella Guida alla distribuzione di WSUS.

## <a name="controlling-when-wsus-client-computers-install-updates"></a>Controllo quando i computer client WSUS installano gli aggiornamenti
Esistono due metodi per controllare quando i computer client WSUS installano gli aggiornamenti:

-   Approvazione con scadenze: le scadenze applicano rigorosamente quando viene installato un aggiornamento

-   Criteri di gruppo WSUS: controllo dei criteri di gruppo quando l'agente di Windows Update analizza e installa gli aggiornamenti

    Per ulteriori informazioni, vedere: [passaggio 5: configurare le impostazioni di criteri di gruppo per aggiornamenti automatici](../deploy/4-configure-group-policy-settings-for-automatic-updates.md), nella Guida alla distribuzione di WSUS.

## <a name="managing-computer-groups"></a>Gestione dei gruppi di computer
Windows Server Update Services consente di applicare gli aggiornamenti a gruppi di computer client in modo da garantire che determinati computer ottengano sempre gli aggiornamenti corretti negli orari più appropriati. Se, ad esempio, tutti i computer del reparto Contabilità sono configurati in modo specifico, è possibile impostare un gruppo per il reparto, stabilire gli aggiornamenti necessari per i computer e l'orario in cui installarli, quindi utilizzare i rapporti WSUS per valutare gli aggiornamenti effettuati.

I computer vengono sempre assegnati al gruppo **tutti i computer** e rimangono assegnati al gruppo **computer non assegnati** finché non vengono assegnati a un altro gruppo. I computer possono appartenere a più di un gruppo.

È possibile impostare gruppi di computer in gerarchie, ad esempio i gruppi Retribuzioni e Contabilità fornitori possono essere contenuti nel gruppo Contabilità. Gli aggiornamenti approvati per un gruppo superiore verranno distribuiti automaticamente a gruppi inferiori, oltre che al gruppo più elevato. Se pertanto si approva Update1 per il gruppo Contabilità, l'aggiornamento verrà distribuito a tutti i computer del gruppo Contabilità, a tutti i computer del gruppo Payroll e a tutti i computer del gruppo Contabilità fornitori.

Dal momento che i computer possono essere assegnati a più gruppi, è possibile che un singolo aggiornamento venga approvato più volte per lo stesso computer. L'aggiornamento sarà tuttavia distribuito una sola volta e gli eventuali conflitti saranno risolti dal server WSUS. Per continuare con l'esempio precedente, se ComputerA è assegnato ai gruppi retribuzioni e contabilità fornitori e Update1 viene approvato per entrambi i gruppi, sarà distribuito una sola volta.

È possibile assegnare i computer a gruppi di computer utilizzando uno dei due metodi denominati rispettivamente destinazione lato server e destinazione lato client. Con la destinazione lato server, si sposta manualmente uno o più computer client in un gruppo di computer alla volta. Con la destinazione lato client si utilizzano i Criteri di gruppo o si modificano le impostazioni del Registro di sistema nei computer client per abilitare l'aggiunta automatica di tali computer ai gruppi di computer creati in precedenza. Questo processo può essere inserito nello script e distribuito in molti computer contemporaneamente. È necessario specificare il metodo di destinazione che si utilizzerà nel server WSUS selezionando una delle due opzioni disponibili nella sezione **computer** della pagina **Opzioni** .

> [!NOTE]
> Se un server WSUS viene eseguito in modalità di replica, non è possibile creare gruppi di computer. Tutti i gruppi di computer necessari per i client del server di replica devono essere creati nel server WSUS che è la radice della gerarchia del server WSUS. Per ulteriori informazioni sulla modalità di replica, vedere [esecuzione della modalità di replica WSUS](running-wsus-replica-mode.md) e per ulteriori informazioni sulla destinazione lato server e lato client, vedere la sezione [1,5. Pianificare i gruppi di computer WSUS](../plan/plan-your-wsus-deployment.md#15-plan-wsus-computer-groups) del passaggio 1: preparare la distribuzione WSUS nella Guida alla distribuzione di WSUS.


