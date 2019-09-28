---
title: Distribuire i computer primari per il reindirizzamento cartelle e i profili utente mobili
description: Come abilitare il supporto del computer primario e designare i computer primari per gli utenti con reindirizzamento cartelle e profili utente mobili.
ms.prod: windows-server
ms.topic: article
author: JasonGerend
ms.author: jgerend
ms.technology: storage
ms.date: 06/06/2019
ms.localizationpriority: medium
ms.openlocfilehash: be2b41cf32e2020422c32415e2d8f4273eb09859
ms.sourcegitcommit: 6aff3d88ff22ea141a6ea6572a5ad8dd6321f199
ms.translationtype: MT
ms.contentlocale: it-IT
ms.lasthandoff: 09/27/2019
ms.locfileid: "71394442"
---
# <a name="deploy-primary-computers-for-folder-redirection-and-roaming-user-profiles"></a>Distribuire i computer primari per il reindirizzamento cartelle e i profili utente mobili

>Si applica a: Windows 10, Windows 8, Windows 8.1, Windows Server 2019, Windows Server 2016, Windows Server 2012, Windows Server 2012 R2

In questo argomento viene descritto come abilitare il supporto del computer primario e designare i computer primari per gli utenti. In questo modo è possibile controllare quali computer utilizzano il reindirizzamento cartelle e i profili utente mobili.

> [!IMPORTANT]
> Quando si Abilita il supporto del computer primario per i profili utente mobili, abilitare sempre il supporto del computer primario anche per il reindirizzamento cartelle. In questo modo i documenti e altri file utente vengono mantenuti fuori dai profili utente, che consentono ai profili di rimanere piccoli e di accedere rapidamente.

## <a name="prerequisites"></a>Prerequisiti

## <a name="software-requirements"></a>Requisiti software

Il supporto del computer primario prevede i requisiti seguenti:

- È necessario aggiornare lo schema Active Directory Domain Services (AD DS) in modo da includere le aggiunte allo schema di Windows Server 2012 (l'installazione di un controller di dominio Windows Server 2012 aggiorna automaticamente lo schema). Per informazioni sull'aggiornamento dello schema di servizi di dominio Active Directory, vedere [integrazione di Adprep. exe](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2012-R2-and-2012/hh472161(v=ws.11)#adprepexe-integration>) ed [esecuzione di Adprep. exe](<https://docs.microsoft.com/previous-versions/windows/it-pro/windows-server-2008-R2-and-2008/dd464018(v=ws.10)>).
- I computer client devono eseguire Windows 10, Windows 8.1, Windows 8, Windows Server 2019, Windows Server 2016, Windows Server 2012 R2 o Windows Server 2012.

> [!TIP]
> Sebbene il supporto del computer primario richieda il reindirizzamento delle cartelle e/o i profili utente mobili, se si distribuiscono queste tecnologie per la prima volta, è preferibile configurare il supporto del computer primario prima di abilitare gli oggetti Criteri di gruppo che configurano il reindirizzamento delle cartelle e Profili utente mobili. In tal modo, i dati utente non verranno copiati nei computer non primari prima di aver abilitato il supporto del computer primario. Per informazioni sulla configurazione, vedere [distribuire il reindirizzamento delle cartelle](deploy-folder-redirection.md) e distribuire i [profili utente mobili](deploy-roaming-user-profiles.md).

## <a name="step-1-designate-primary-computers-for-users"></a>Passaggio 1: Designare i computer primari per gli utenti

Il primo passaggio della distribuzione del supporto per i computer primari è la designazione dei computer primari per ogni utente. A tale scopo, utilizzare Active Directory centro di amministrazione per ottenere il nome distinto dei computer rilevanti, quindi impostare l'attributo **msDS-PrimaryComputer** .

> [!TIP]
> Per usare Windows PowerShell per lavorare con i computer primari, vedere il post di Blog relativo alla ricerca di [un po' più approfondito nel computer primario Windows 8](<https://blogs.technet.microsoft.com/askds/2012/10/23/digging-a-little-deeper-into-windows-8-primary-computer/>).

Di seguito viene illustrato come specificare i computer primari per gli utenti:

1. Aprire Server Manager in un computer in cui sono installati gli strumenti di amministrazione di Active Directory.
2. Scegliere **Active Directory centro di amministrazione**dal menu **strumenti** . Verrà visualizzato Centro amministrativo di Active Directory.
3. Passare al contenitore **computer** nel dominio appropriato.
4. Fare clic con il pulsante destro del mouse su un computer che si desidera designare come computer primario, quindi scegliere **Proprietà**.
5. Nel riquadro di spostamento selezionare **estensioni**.
6. Selezionare la scheda **Editor attributi** , scorrere fino a **Distinguishname**, **selezionare Visualizza**, fare clic con il pulsante destro del mouse sul valore elencato, selezionare **copia**, fare clic su **OK**, quindi scegliere **Annulla**.
7. Passare al contenitore **utenti** nel dominio appropriato, fare clic con il pulsante destro del mouse sull'utente a cui si desidera assegnare il computer, quindi scegliere **Proprietà**.
8. Nel riquadro di spostamento selezionare **estensioni**.
9. Selezionare la scheda **Editor attributi** , selezionare **msDS-PrimaryComputer** e quindi fare clic su **modifica**. Verrà visualizzata la finestra di dialogo Editor stringa multivalore.
10. Fare clic con il pulsante destro del mouse sulla casella di testo, selezionare **Incolla**, scegliere **Aggiungi**, selezionare **OK**, quindi selezionare di nuovo **OK** .

## <a name="step-2-optionally-enable-primary-computers-for-folder-redirection-in-group-policy"></a>Passaggio 2: Facoltativamente, abilitare i computer primari per il reindirizzamento cartelle nel Criteri di gruppo

Il passaggio successivo consiste nel configurare facoltativamente Criteri di gruppo per abilitare il supporto del computer primario per il reindirizzamento cartelle. Questa operazione consente di reindirizzare le cartelle di un utente nei computer designati come computer primari dell'utente, ma non in altri computer. È possibile controllare i computer primari per il reindirizzamento delle cartelle in base ai singoli computer o per singolo utente.

Di seguito viene illustrato come abilitare i computer primari per il reindirizzamento delle cartelle:

1. In Gestione Criteri di gruppo fare clic con il pulsante destro del mouse sull'oggetto Criteri di gruppo creato durante la configurazione iniziale del reindirizzamento cartelle e/o dei profili utente mobili (ad esempio, **impostazioni di Reindirizzamento cartelle** o **Impostazioni profili utente mobili**) e quindi Selezionare **modifica**.
2. Per abilitare il supporto per i computer primari utilizzando Criteri di gruppo basati su computer, passare a **Configurazione computer**. Per Criteri di gruppo basate sull'utente, passare a **Configurazione utente**.
    - Criteri di gruppo basata su computer applica l'elaborazione del computer primario a tutti i computer a cui si applica l'oggetto Criteri di gruppo, che influisce su tutti gli utenti dei computer.
    - Criteri di gruppo basata sull'utente per applicare l'elaborazione del computer primario a tutti gli account utente a cui si applica l'oggetto Criteri di gruppo, che influisce su tutti i computer a cui gli utenti eseguono l'accesso.
3. In configurazione **computer** o **Configurazione utente**passare a **criteri**, quindi **modelli amministrativi**, quindi **sistema**, quindi **Reindirizzamento cartelle**.
4. Fare clic con **il pulsante destro del mouse su Reindirizza cartelle solo nei computer primari**e quindi scegliere **modifica**.
5. Selezionare **abilitato**e quindi fare clic su **OK**.

## <a name="step-3-optionally-enable-primary-computers-for-roaming-user-profiles-in-group-policy"></a>Passaggio 3: Facoltativamente, abilitare i computer primari per i profili utente mobili in Criteri di gruppo

Il passaggio successivo consiste nel configurare facoltativamente Criteri di gruppo per abilitare il supporto del computer primario per i profili utente mobili. Questa operazione consente di eseguire il roaming di un profilo utente nei computer designati come computer primari dell'utente, ma non in altri computer.

Di seguito viene illustrato come abilitare i computer primari per i profili utente mobili:

1. Abilitare il supporto del computer primario per il reindirizzamento delle cartelle, se non è già stato fatto.<br>In questo modo i documenti e altri file utente vengono mantenuti fuori dai profili utente, che consentono ai profili di rimanere piccoli e di accedere rapidamente.
2. In Gestione Criteri di gruppo fare clic con il pulsante destro del mouse sull'oggetto Criteri di gruppo creato, ad esempio **impostazioni di Reindirizzamento cartelle e profili utente mobili**, quindi scegliere **modifica**.
3. Passare a **Configurazione computer**, **criteri**, quindi **modelli amministrativi**, **sistema**e quindi **profili utente**.
4. Fare clic con il pulsante destro **del mouse su Scarica profili mobili solo nei computer primari** e quindi scegliere **modifica**.
5. Selezionare **abilitato**e quindi fare clic su **OK**.

## <a name="step-4-enable-the-gpo"></a>Passaggio 4: Abilitare l'oggetto Criteri di gruppo

Dopo aver completato la configurazione di Reindirizzamento cartelle e profili utente mobili, abilitare l'oggetto Criteri di gruppo, se non è già stato fatto. In questo modo, è possibile applicarla a utenti e computer interessati.

Di seguito viene illustrato come abilitare il reindirizzamento cartelle e/o gli oggetti Criteri di gruppo dei profili utente mobili:

1. Apri Gestione Criteri di gruppo
2. Fare clic con il pulsante destro del mouse sugli oggetti Criteri di gruppo creati, quindi scegliere **collegamento abilitato**. Accanto alla voce di menu verrà visualizzata una casella di controllo.

## <a name="step-5-test-primary-computer-function"></a>Passaggio 5: Testare la funzione del computer primario

Per testare il supporto del computer primario, accedere a un computer primario, verificare che le cartelle e i profili siano reindirizzati, quindi accedere a un computer non primario e verificare che le cartelle e i profili non siano reindirizzati.

Di seguito viene illustrato come testare la funzionalità del computer primario:

1. Accedere a un computer primario designato con un account utente per il quale è stato abilitato il reindirizzamento delle cartelle e/o i profili utente mobili.
2. Se l'account utente ha eseguito l'accesso al computer in precedenza, aprire una finestra di Windows PowerShell o una finestra del prompt dei comandi come amministratore, digitare il comando seguente e quindi disconnettersi quando viene richiesto di verificare che le impostazioni di Criteri di gruppo più recenti vengano applicate al computer client:

    ```PowerShell
    Gpupdate /force
    ```

3. Aprire Esplora file.
1. Fare clic con il pulsante destro del mouse su una cartella reindirizzata, ad esempio la cartella documenti nella raccolta documenti, quindi scegliere **Proprietà**.
1. Selezionare la scheda **location** (percorso) e verificare che il percorso visualizzi la condivisione file specificata anziché un percorso locale. Per verificare che il profilo utente sia in roaming, aprire il **Pannello di controllo**, selezionare **sistema e sicurezza**, selezionare **sistema**, **impostazioni di sistema avanzate**, selezionare **Impostazioni** nella sezione profili utente e cercare  **Roaming** nella colonna **tipo** .
1. Accedere con lo stesso account utente a un computer non designato come computer primario dell'utente.
1. Ripetere i passaggi da 2 a 5, cercando i percorsi locali e un tipo di profilo **locale** .

> [!NOTE]
> Se le cartelle sono state reindirizzate in un computer prima di abilitare il supporto del computer primario, le cartelle rimarranno reindirizzate a meno che non sia stata configurata l'impostazione seguente nell'impostazione dei criteri di Reindirizzamento cartelle di ogni cartella: **Reindirizzare la cartella al percorso locale di UserProfile quando i criteri vengono rimossi**. Analogamente, i profili precedentemente in roaming in un particolare computer visualizzeranno il **roaming** nelle colonne di **tipo** ; Tuttavia, nella colonna **stato** viene visualizzato **local**.

## <a name="more-information"></a>Altre informazioni

- [Distribuire il reindirizzamento cartelle con File offline](deploy-folder-redirection.md)
- [Distribuire profili utente mobili](deploy-roaming-user-profiles.md)
- [Panoramica di Reindirizzamento cartelle, File offline e profili utente mobili](folder-redirection-rup-overview.md)
- [Approfondimento sul computer principale Windows 8](https://blogs.technet.com/b/askds/archive/2012/10/23/digging-a-little-deeper-into-windows-8-primary-computer.aspx)