---
title: Passaggio 3 - approvare e distribuire gli aggiornamenti in Windows Server Update SERVICES
description: 'Argomento di Windows Server Update Services (WSUS): Approvare e distribuire gli aggiornamenti in WSUS è il terzo passaggio di un processo in quattro passaggi per la distribuzione di WSUS'
ms.prod: windows-server
ms.technology: manage-wsus
ms.topic: article
ms.assetid: 8d728ff9-170f-47e6-aefe-52be93315a75
author: coreyp-at-msft
ms.author: coreyp
manager: dongill
ms.date: 10/16/2017
ms.openlocfilehash: d8758c4f957d2d9abb156652a568b23634cb1bf0
ms.sourcegitcommit: 3a3d62f938322849f81ee9ec01186b3e7ab90fe0
ms.translationtype: HT
ms.contentlocale: it-IT
ms.lasthandoff: 04/23/2020
ms.locfileid: "80828854"
---
# <a name="step-3-approve-and-deploy-updates-in-wsus"></a>Passaggio 3: Approvare e distribuire gli aggiornamenti in WSUS

>Si applica a: Windows Server 2019, Windows Server (Canale semestrale), Windows Server 2016, Windows Server 2012 R2, Windows Server 2012

I computer inclusi in un gruppo di computer contattano automaticamente il server WSUS nelle successive 24 ore per ottenere aggiornamenti. È possibile utilizzare la funzionalità di generazione rapporti di WSUS per determinare se gli aggiornamenti sono stati distribuiti nei computer di test. Quando i test vengono completati correttamente è possibile approvare gli aggiornamenti per i gruppi di computer applicabili dell'organizzazione. Nell'elenco di controllo seguente sono descritti i passaggi per l'approvazione e la distribuzione di aggiornamenti mediante la console di gestione di Windows Server Update Services.

|Attività|Descrizione|
|----|--------|
|[3.1. Approvare e distribuire gli aggiornamenti di WSUS](3-approve-and-deploy-updates-in-wsus.md#BKM_3.1.)|Approvare e distribuire gli aggiornamenti di WSUS mediante la console di gestione di Windows Server Update Services.|
|[3.2. Configurare le regole di approvazione automatica](3-approve-and-deploy-updates-in-wsus.md#BKM_3.2.a.)|Configurare WSUS in modo da approvare automaticamente l'installazione degli aggiornamenti per i gruppi selezionati e come approvare le revisioni degli aggiornamenti esistenti.|
|[3.3. Esaminare gli aggiornamenti installati con i rapporti WSUS](3-approve-and-deploy-updates-in-wsus.md#BKM_3.3.)|Esaminare gli aggiornamenti che sono stati installati, i computer che li hanno ricevuti e altre informazioni utilizzando la funzionalità di generazione rapporti di WSUS.|

## <a name="31-approve-and-deploy-wsus-updates"></a><a name=BKM_3.1.></a>3.1. Approvare e distribuire gli aggiornamenti di WSUS
Utilizzare la procedura seguente per approvare e distribuire gli aggiornamenti.

#### <a name="to-approve-and-deploy-wsus-updates"></a>Per approvare e distribuire gli aggiornamenti di WSUS

1.  Nella console di amministrazione di WSUS fare clic su **Aggiornamenti**. Nel riquadro destro è visualizzato un riepilogo dello stato degli aggiornamenti per **Tutti gli aggiornamenti**, **Aggiornamenti critici**, **Aggiornamenti per la sicurezza**e **Aggiornamenti di WSUS**.

2.  Nella sezione **Tutti gli aggiornamenti** fare clic su **Aggiornamenti necessari per i computer**.

3.  Nell'elenco di aggiornamenti selezionare gli aggiornamenti che si desidera approvare per l'installazione nel gruppo di computer di test. Le informazioni relative a un aggiornamento selezionato sono disponibili nel riquadro inferiore del pannello **Aggiornamenti** . Per selezionare più aggiornamenti contigui, fare clic sui nomi degli aggiornamenti tenendo premuto il tasto **MAIUSC**. Per selezionare più aggiornamenti non contigui, fare clic sui nomi degli aggiornamenti tenendo premuto il tasto **CTRL**.

4.  Fare clic con il pulsante destro del mouse sulla selezione e scegliere **Approva**.

5.  Nella finestra di dialogo **Approva aggiornamenti** selezionare il gruppo di test e fare clic sulla freccia giù.

6.  Fare clic su **Approvato per l'installazione**e quindi su **OK**.

7.  Viene visualizzata la finestra **Stato approvazione** , in cui è visualizzato lo stato delle attività relative all'approvazione degli aggiornamenti. Al termine del processo di approvazione fare clic su **Chiudi**.

## <a name="32-configure-auto-approval-rules"></a><a name=BKM_3.2.a.></a>3.2. Configurare le regole di approvazione automatica
Le approvazioni automatiche consentono di specificare come approvare automaticamente l'installazione degli aggiornamenti per i gruppi selezionati e come approvare le revisioni degli aggiornamenti esistenti.

#### <a name="to-configure-automatic-approvals"></a>Per configurare le approvazioni automatiche

1.  Nella console di amministrazione di WSUS, nella sezione **Servizi di aggiornamento**, espandere il server WSUS e quindi fare clic su **Opzioni**. Verrà visualizzata la finestra di dialogo **Opzioni** .

2.  In **Opzioni**fare clic su **Approvazioni automatiche**. Viene visualizzata la finestra di dialogo Approvazioni automatiche.

3.  In **Regole di aggiornamento** fare clic su **Nuova regola**. Viene visualizzata la finestra di dialogo **Aggiungi regola**.

4.  In **Aggiungi regola**, in **Passaggio 1: selezionare le proprietà**, selezionare una singola opzione o qualsiasi combinazione di opzioni tra le seguenti:

    -   **Se l'aggiornamento è in una classificazione specifica**

    -   **Se l'aggiornamento è in un prodotto specifico**

    -   **Impostare una scadenza per l'approvazione**

5.  In **Passaggio 2: modificare le proprietà** fare clic su ciascuna delle opzioni elencate e quindi selezionare le opzioni appropriate per ognuno.

6.  In **Passaggio 3: specificare un nome** digitare un nome per la regola e quindi fare clic su **OK**.

7.  Fare clic su **OK** per chiudere la finestra di dialogo Approvazioni automatiche.

## <a name="33-review-installed-updates-with-wsus-reports"></a><a name=BKM_3.3.></a>3.3. Esaminare gli aggiornamenti installati con i rapporti WSUS
24 ore dopo aver approvato gli aggiornamenti è possibile utilizzare la funzionalità di generazione rapporti di WSUS per determinare se gli aggiornamenti sono stati distribuiti nei computer di test. Per verificare lo stato di un aggiornamento è possibile utilizzare la funzionalità di generazione rapporti di WSUS come illustrato di seguito.

#### <a name="to-review-updates"></a>Per esaminare gli aggiornamenti

1.  Nel riquadro di spostamento della console di amministrazione di WSUS fare clic su **Rapporti**.

2.  Nella pagina **Rapporti** fare clic sul rapporto **Riepilogo stato aggiornamenti** . Verrà visualizzata la finestra **Rapporto aggiornamenti** .

3.  Se si desidera filtrare l'elenco di aggiornamenti, selezionare i criteri da utilizzare, ad esempio **Includi aggiornamenti in queste classificazioni**, quindi fare clic su **Esegui rapporto**.

4.  Verrà visualizzato il riquadro **Rapporto aggiornamenti** . È possibile verificare lo stato dei singoli aggiornamenti selezionando l'aggiornamento nella sezione sinistra del riquadro. Nell'ultima sezione del riquadro del rapporto è visualizzato il riepilogo dello stato dell'aggiornamento.

5.  È possibile salvare o stampare il rapporto facendo clic sull'apposita icona nella barra degli strumenti.

6.  Dopo aver eseguito il test degli aggiornamenti è possibile approvare gli aggiornamenti per l'installazione nei gruppi di computer applicabili dell'organizzazione.
